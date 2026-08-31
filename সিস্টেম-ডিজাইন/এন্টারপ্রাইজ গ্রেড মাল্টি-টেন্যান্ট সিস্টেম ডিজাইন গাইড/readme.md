# এন্টারপ্রাইজ গ্রেড মাল্টি-টেন্যান্ট সিস্টেম ডিজাইন গাইড

আপনার সিস্টেমের চাহিদা অনুযায়ী (১৫০+ টেবিল, হাজার হাজার টেন্যান্ট, ১৫+ টেবিল জয়েন, ১৬ কোর/১৬ জিবি র্যাম) Citus ব্যবহার করে একটি স্কেলেবল আর্কিটেকচার তৈরি করব। প্রতিটি অংশের গভীর ব্যাখ্যা সহ সম্পূর্ণ গাইড নিচে দেওয়া হলো।

---

## 🎯 Citus কী এবং কেন আপনার দরকার?

### সমস্যা বিশ্লেষণ:
আপনার বর্তমান PostgreSQL সিঙ্গেল সার্ভারে:
- **১৫০+ টেবিল** → প্রতিটি টেবিলের মেটাডেটা, স্ট্যাটিস্টিক্স, ইনডেক্স র্যামে থাকে
- **হাজার হাজার টেন্যান্ট** → সব টেন্যান্টের ডাটা এক জায়গায়
- **১৫+ টেবিল জয়েন** → একটি কুয়েরিতে ১৫টা টেবিল স্ক্যান + জয়েন + সর্ট
- **১৬ কোর, ১৬ জিবি র্যাম** → ডাটা বাড়লে র্যামে ক্যাশ আঁটবে না, ডিস্ক I/O বাড়বে

**Citus সমাধান:**
- ডাটাকে **শার্ডিং** করে একাধিক সার্ভারে ভাগ করে
- প্রতিটি টেন্যান্টের ডাটা **একটি নির্দিষ্ট শার্ডে** রাখে
- কুয়েরি **লোকালাইজড** হয় → নেটওয়ার্ক ট্রাফিক কম → পারফরম্যান্স বাড়ে

### Citus Cluster আর্কিটেকচার:

```
                    ┌─────────────────────┐
                    │  Application Layer  │
                    │  (Web/API Servers)  │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │   PgBouncer (6432)  │  ← Connection Pooler
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │  Coordinator Node   │  ← Query Router
                    │   (10.10.10.11)     │
                    └──────┬───────┬──────┘
                           │       │
              ┌────────────▼─┐   ┌─▼────────────┐
              │  Worker 1    │   │  Worker 2    │
              │ (10.10.10.12)│   │ (10.10.10.13)│
              │ Tenants:     │   │ Tenants:     │
              │ 101-105      │   │ 106-110      │
              └──────────────┘   └──────────────┘
```

**Coordinator Node:**
- কুয়েরি গ্রহণ করে
- টেন্যান্ট আইডি অনুযায়ী রাউটিং সিদ্ধান্ত নেয়
- Worker-দের থেকে ফলাফল একত্রিত করে

**Worker Nodes:**
- আসল ডাটা সংরক্ষণ করে
- ডিস্ট্রিবিউটেড কুয়েরি এক্সিকিউট করে
- লোকাল জয়েন/অ্যাগ্রিগেশন করে

---

## 🔧 Part 1: ইনস্টলেশন ও কনফিগারেশন

### 1.1 Repository যোগ ও ইনস্টলেশন

```bash
# Citus অফিসিয়াল রিপোজিটরি যোগ
curl https://citusdata.com | sudo bash
```

**ব্যাখ্যা:**
- Ubuntu-র ডিফল্ট রিপোজিটরিতে Citus থাকে না
- এই কমান্ড Citus-এর অফিসিয়াল APT রিপোজিটরি যোগ করে
- GPG কী যুক্ত করে প্যাকেজ ভেরিফিকেশন নিশ্চিত করে

```bash
# PostgreSQL 16 + Citus 12.1 ইনস্টল
sudo apt-get install -y postgresql-16-citus-12.1
```

**কেন এই নির্দিষ্ট ভার্সন?**
- PostgreSQL 16: সর্বশেষ স্টেবল ভার্সন (২০২৪ অনুযায়ী)
- Citus 12.1: PostgreSQL 16-এর সাথে কম্প্যাটিবল
- **গুরুত্বপূর্ণ:** সব নোডে একই ভার্সন ব্যবহার করুন

### 1.2 ফাইল লিমিট কনফিগারেশন

```bash
# /etc/security/limits.conf
postgres    soft    nofile    65536
postgres    hard    nofile    65536
```

**কেন ৬৫৫৩৬?**

আপনার ১৫০ টেবিল × ৪ শার্ড = ৬০০ ফিজিক্যাল টেবিল ফাইল। প্রতিটি টেবিলের:
- ডাটা ফাইল (.dat)
- ইনডেক্স ফাইল (.idx) 
- TOAST টেবিল (বড় ডাটার জন্য)
- Visibility map
- Free space map

মোট ফাইল ≈ ৬০০ × ৫ = ৩০০০+ ফাইল। সাথে কানেকশন সকেট, লগ ফাইল, কনফিগ ফাইল। ডিফল্ট ১০২৪ লিমিট স্পষ্টতই অপর্যাপ্ত।

---

## ⚙️ Part 2: PostgreSQL Tuning — বিস্তারিত

### 2.1 মেমোরি বরাদ্দ ক্যালকুলেশন

১৬ জিবি র্যামের সঠিক ভাগ:

```
মোট র্যাম: ১৬ জিবি
├── shared_buffers: ৪ জিবি (২৫%)
│   └── PostgreSQL-এর নিজস্ব ক্যাশ
│       └── ঘন ঘন ব্যবহৃত পেজ এখানে থাকে
├── effective_cache_size: ১২ জিবি (৭৫%)
│   └── OS Page Cache + shared_buffers (আনুমানিক)
│       └── Planner-কে জানায় কত ক্যাশিং সম্ভব
├── maintenance_work_mem: ১ জিবি
│   └── VACUUM, CREATE INDEX, ALTER TABLE-এর জন্য
├── work_mem: ৬৪ এমবি × N
│   └── প্রতি Sort/Hash অপারেশনে
│       └── ১৫+ টেবিল জয়েনে একাধিক sort/hash হয়
└── OS + PostgreSQL ওভারহেড: ~৩ জিবি
```

### 2.2 প্রতিটি প্যারামিটারের গভীর ব্যাখ্যা:

#### `shared_buffers = 4GB`
- **কী করে:** PostgreSQL-এর প্রাইমারি ক্যাশ। সবচেয়ে ঘন ঘন ব্যবহৃত ডাটা ব্লক এখানে থাকে।
- **কেন ৪ জিবি?** র্যামের ২৫%। বেশি দিলে OS ক্যাশ কমে যায়, কম দিলে ডিস্ক I/O বাড়ে।
- **মনিটরিং:** `pg_stat_bgwriter` দেখে hit ratio চেক করুন। ৯৯%+ হলে ভালো।

#### `work_mem = 64MB`
- **কী করে:** প্রতি Sort, Hash Join, Merge Join অপারেশনে বরাদ্দ হয়।
- **১৫+ টেবিল জয়েনের ইমপ্যাক্ট:**
  ```sql
  SELECT ... FROM orders o
  JOIN order_items oi ON ...
  JOIN customers c ON ...
  JOIN invoices i ON ...
  JOIN payments p ON ...
  JOIN shipping s ON ...
  JOIN products pr ON ...
  JOIN categories cat ON ...
  JOIN warehouses w ON ...
  JOIN employees e ON ...
  JOIN addresses a ON ...
  JOIN tax_rates tr ON ...
  JOIN discounts d ON ...
  JOIN audit_log al ON ...
  JOIN attachments att ON ...
  JOIN user_roles ur ON ...
  WHERE o.tenant_id = 105;
  ```
  - এই কুয়েরিতে ১০+ Hash Join অপারেশন হতে পারে
  - প্রতিটি Hash Join-এর জন্য ৬৪ এমবি করে র্যাম দরকার
  - মোট সম্ভাব্য ব্যবহার: ১০ × ৬৪ এমবি = ৬৪০ এমবি প্রতি কানেকশন
  
  ⚠️ **সতর্কতা:** ২০ কানেকশন × ৬৪০ এমবি = ১২.৮ জিবি → র্যাম ক্র্যাশ! তাই PgBouncer দিয়ে কানেকশন সীমিত করা জরুরি।

#### `effective_cache_size = 12GB`
- **কী করে:** Planner-কে জানায় "তুমি ধরে নাও OS লেভেলে এত ক্যাশিং হবে"।
- **র্যাম রিজার্ভ করে না**, শুধু Planner-কে স্মার্ট ডিসিশন নিতে সাহায্য করে।
- **প্রভাব:** বেশি হলে Planner Index Scan প্রেফার করবে (কারণ মনে করবে ডাটা ক্যাশে থাকবে), কম হলে Sequential Scan করবে।

#### `max_worker_processes = 16`
- **কী করে:** মোট ব্যাকগ্রাউন্ড ওয়ার্কারের সিলিং (একাধিক কুয়েরির প্যারালাল ওয়ার্কার + ব্যাকগ্রাউন্ড প্রসেস)।
- **কেন ১৬?** আপনার কোর সংখ্যা অনুযায়ী।

#### `max_parallel_workers_per_gather = 4`
- **কী করে:** একটি কুয়েরির জন্য সর্বোচ্চ কতগুলো প্যারালাল ওয়ার্কার ব্যবহার হবে।
- **কেন ১৬ নয়?** ২০-৩০ কানেকশন একসাথে ১৬ কোর দখল করলে সব কুয়েরি হ্যাং হবে। ৪ কোর দিলে সবার মধ্যে ব্যালান্স থাকে।

#### `max_parallel_workers = 12`
- **কী করে:** সব কুয়েরি মিলে সর্বোচ্চ ১২টা প্যারালাল ওয়ার্কার।
- **কেন?** বাকি ৪ কোর সিস্টেম কাজের জন্য ফাঁকা রাখা হয়।

### 2.3 Citus-স্পেসিফিক সেটিংস:

#### `citus.shard_count = 4` — সবচেয়ে গুরুত্বপূর্ণ!

```sql
-- Coordinator-এ সেট করুন
ALTER DATABASE yourdb SET citus.shard_count = 4;
```

**গাণিতিক বিশ্লেষণ:**
- ডিফল্ট: ৩২ শার্ড
- আপনার ১৫০ টেবিল × ৩২ = ৪,৮০০ ফিজিক্যাল টেবিল
- প্রতিটি শার্ডের মেটাডেটা (স্ট্যাটিস্টিক্স, ইনডেক্স, ভ্যাকুয়াম স্ট্যাটাস) র্যামে থাকে
- ৪,৮০০ টেবিলের মেটাডেটা ≈ ২-৩ জিবি র্যাম
- ৪ শার্ড × ১৫০ টেবিল = ৬০০ টেবিল → মেটাডেটা ≈ ৩০০-৪০০ এমবি

**কেন ৪?**
- ২ Worker নোড × ২ শার্ড = ৪ (লোড ব্যালান্সিং)
- ভবিষ্যতে ৪ Worker হলে প্রতিটিতে ১ শার্ড

#### `citus.replication_factor = 1`
- **কী:** প্রতি শার্ডের কতগুলো কপি থাকবে
- **কেন ১?** র্যাম/ডিস্ক সাশ্রয়। HA দরকার হলে Worker-লেভেলে Streaming Replication করুন।

#### `citus.enable_repartition_joins = on`
- **কী:** Cross-shard জয়েনের সময় ডাটা রিপার্টিশন করার অনুমতি
- **সাবধানতা:** এটি enable রাখলে সিস্টেম ধীরে ধীরে কাজ করবে, disable করলে দ্রুত এরর দেবে। আপনার কেসে disable রাখাই ভালো (কারণ সব জয়েন tenant_id দিয়ে হবে)।

### 2.4 pg_hba.conf কনফিগারেশন:

```conf
# /etc/postgresql/16/main/pg_hba.conf
# TYPE  DATABASE  USER  ADDRESS      METHOD
local   all       all               peer
host    all       all   10.10.10.0/24  scram-sha-256
host    all       all   127.0.0.1/32   scram-sha-256
```

**প্রতিটি লাইনের অর্থ:**
1. `local all all peer` → লোকাল সকেট কানেকশনে OS ইউজারনেম দিয়ে অথেনটিকেশন
2. `host all all 10.10.10.0/24 scram-sha-256` → শুধু প্রাইভেট নেটওয়ার্ক থেকে কানেকশন, SCRAM-SHA-256 পাসওয়ার্ড এনক্রিপশন
3. `host all all 127.0.0.1/32 scram-sha-256` → লোকালহোস্ট থেকে TCP কানেকশন

**নিরাপত্তা:**
- ইন্টারনেট থেকে কেউ কানেক্ট করতে পারবে না
- পাসওয়ার্ড এনক্রিপ্টেড
- প্রতিটি Worker-এ একই কনফিগ

---

## 🌐 Part 3: Citus Cluster সেটআপ — বিস্তারিত

### 3.1 Extension সেটআপ

```sql
-- প্রতিটি নোডে (Coordinator + সব Worker) চালান
CREATE EXTENSION citus;
```

**কী ঘটে?**
- Citus-এর ফাংশন, টাইপ, ব্যাকগ্রাউন্ড প্রসেস লোড হয়
- `pg_catalog`-এ Citus-সম্পর্কিত টেবিল তৈরি হয়
- শেয়ার্ড মেমোরি সেগমেন্ট বরাদ্দ হয়

### 3.2 Coordinator কনফিগারেশন

```sql
-- Coordinator-এ চালান
SELECT citus_set_coordinator_host('10.10.10.11', 5432);
```

**কেন লোকালহোস্ট নয়?**
- Worker-রা Coordinator-এর কাছে কানেক্ট করে
- Worker-দের কাছে "10.10.10.11" পাবলিক/প্রাইভেট IP হিসেবে পরিচিত
- লোকালহোস্ট দিলে Worker-রা নিজেদের দিকেই কানেক্ট করার চেষ্টা করবে

```sql
-- Worker নোড যোগ করুন
SELECT * FROM citus_add_node('10.10.10.12', 5432);  -- Worker 1
SELECT * FROM citus_add_node('10.10.10.13', 5432);  -- Worker 2
```

**কী ঘটে?**
- Coordinator Worker-দের সাথে কানেক্ট করে
- Worker-দের ক্লাস্টার মেম্বারশিপ দেয়
- Metadata সিঙ্ক্রোনাইজ করে
- প্রতিটি Worker-এ `pg_dist_node` টেবিল আপডেট হয়

```sql
-- ভেরিফিকেশন
SELECT * FROM citus_get_active_worker_nodes();
```

**আউটপুট:**
```
 node_name  | node_port 
------------+-----------
 10.10.10.12|      5432
 10.10.10.13|      5432
(2 rows)
```

### 3.3 Distributed Deadlock Detection

```sql
-- Coordinator-এ
SET citus.distributed_deadlock_detection_factor = 2;
```

**কী করে?** ডিস্ট্রিবিউটেড ট্রানজেকশনে ডেডলক ডিটেক্ট করে। আপনি যেহেতু মাল্টি-টেন্যান্টে সিঙ্গেল-নোড রাউটিং করবেন, এটা কম গুরুত্বপূর্ণ, তবে রেখে দেওয়া ভালো।

---

## 📐 Part 4: Schema Design — বিস্তারিত ব্যাখ্যা

### 4.1 Core Tables Design

#### ৪.১.১ `tenants` — সেন্ট্রাল টেবিল

```sql
CREATE TABLE tenants (
    tenant_id SERIAL PRIMARY KEY,
    company_name VARCHAR(255) NOT NULL,
    subdomain VARCHAR(100) UNIQUE NOT NULL,
    plan_type VARCHAR(50) NOT NULL DEFAULT 'basic',
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    billing_email VARCHAR(255),
    tech_email VARCHAR(255),
    phone VARCHAR(50),
    address TEXT
);

-- Reference table হিসেবে ডিস্ট্রিবিউট
SELECT create_reference_table('tenants');
```

**কেন Reference Table?**
- ছোট টেবিল (কয়েক হাজার রো)
- ঘন ঘন জয়েন হয়
- প্রতিটি Worker-এ ফুল কপি রাখলে জয়েন লোকাল হয়

#### ৪.১.২ `orders` — Distributed Table

```sql
CREATE TABLE orders (
    tenant_id INT NOT NULL,
    order_id BIGSERIAL NOT NULL,
    customer_id BIGINT NOT NULL,
    order_date DATE NOT NULL,
    order_status VARCHAR(20) NOT NULL DEFAULT 'pending',
    total_amount NUMERIC(15,2) NOT NULL,
    discount_amount NUMERIC(15,2) DEFAULT 0,
    tax_amount NUMERIC(15,2) DEFAULT 0,
    shipping_address_id BIGINT,
    billing_address_id BIGINT,
    created_by INT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    deleted_at TIMESTAMPTZ,  -- Soft delete
    PRIMARY KEY (tenant_id, order_id)
);

-- ডিস্ট্রিবিউট করুন
SELECT create_distributed_table('orders', 'tenant_id');
```

**কেন Composite Primary Key?**
- Citus-এর হার্ড রুল: ডিস্ট্রিবিউশন কলাম PK-র অংশ হতে হবে
- `tenant_id` + `order_id` ইউনিক কোয়ালিফায়ার
- `BIGSERIAL` অটো-ইনক্রিমেন্ট কাজ করে (শার্ডে ভাগ হলেও)

#### ৪.১.৩ `order_items` — Co-located Table

```sql
CREATE TABLE order_items (
    tenant_id INT NOT NULL,
    order_item_id BIGSERIAL NOT NULL,
    order_id BIGINT NOT NULL,
    product_id BIGINT NOT NULL,
    quantity INT NOT NULL,
    unit_price NUMERIC(15,2) NOT NULL,
    discount_amount NUMERIC(15,2) DEFAULT 0,
    tax_amount NUMERIC(15,2) DEFAULT 0,
    total_amount NUMERIC(15,2) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    PRIMARY KEY (tenant_id, order_item_id),
    FOREIGN KEY (tenant_id, order_id) REFERENCES orders(tenant_id, order_id)
);

-- একই tenant_id দিয়ে ডিস্ট্রিবিউট → Co-location নিশ্চিত
SELECT create_distributed_table('order_items', 'tenant_id');
```

**Co-location এর শক্তি:**
- টেন্যান্ট ১০৫-এর `orders` এবং `order_items` একই Worker-এ
- জয়েন লোকাল → নেটওয়ার্ক ট্রাফিক নেই
- FK ভ্যালিডেশন লোকাল হয়

### 4.2 সম্পূর্ণ Schema (১৫০ টেবিলের কয়েকটি উদাহরণ)

#### ৪.২.১ গ্রাহক সম্পর্কিত টেবিল:

```sql
-- Customers
CREATE TABLE customers (
    tenant_id INT NOT NULL,
    customer_id BIGINT NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(255),
    phone VARCHAR(50),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    PRIMARY KEY (tenant_id, customer_id)
);
SELECT create_distributed_table('customers', 'tenant_id');

-- Customer Addresses
CREATE TABLE customer_addresses (
    tenant_id INT NOT NULL,
    address_id BIGINT NOT NULL,
    customer_id BIGINT NOT NULL,
    address_type VARCHAR(20) NOT NULL, -- billing/shipping
    address_line1 TEXT,
    address_line2 TEXT,
    city VARCHAR(100),
    state VARCHAR(100),
    postal_code VARCHAR(20),
    country_code CHAR(2),
    PRIMARY KEY (tenant_id, address_id),
    FOREIGN KEY (tenant_id, customer_id) REFERENCES customers(tenant_id, customer_id)
);
SELECT create_distributed_table('customer_addresses', 'tenant_id');
```

#### ৪.২.২ ইনভয়েস ও পেমেন্ট:

```sql
-- Invoices
CREATE TABLE invoices (
    tenant_id INT NOT NULL,
    invoice_id BIGINT NOT NULL,
    order_id BIGINT NOT NULL,
    invoice_number VARCHAR(50) NOT NULL,
    invoice_date DATE NOT NULL,
    due_date DATE NOT NULL,
    total_amount NUMERIC(15,2) NOT NULL,
    paid_amount NUMERIC(15,2) DEFAULT 0,
    status VARCHAR(20) NOT NULL DEFAULT 'unpaid',
    PRIMARY KEY (tenant_id, invoice_id),
    FOREIGN KEY (tenant_id, order_id) REFERENCES orders(tenant_id, order_id),
    UNIQUE (tenant_id, invoice_number)
);
SELECT create_distributed_table('invoices', 'tenant_id');

-- Payments
CREATE TABLE payments (
    tenant_id INT NOT NULL,
    payment_id BIGINT NOT NULL,
    invoice_id BIGINT NOT NULL,
    payment_date TIMESTAMPTZ NOT NULL,
    amount NUMERIC(15,2) NOT NULL,
    payment_method VARCHAR(50),
    transaction_ref VARCHAR(100),
    PRIMARY KEY (tenant_id, payment_id),
    FOREIGN KEY (tenant_id, invoice_id) REFERENCES invoices(tenant_id, invoice_id)
);
SELECT create_distributed_table('payments', 'tenant_id');
```

#### ৪.২.৩ প্রোডাক্ট ও ইনভেন্টরি:

```sql
-- Products (Reference Table হতে পারে, যদি সব টেন্যান্টের জন্য একই হয়)
CREATE TABLE products (
    product_id BIGSERIAL PRIMARY KEY,
    sku VARCHAR(100) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    price NUMERIC(15,2) NOT NULL,
    category_id INT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
SELECT create_reference_table('products');

-- Product Categories (Reference Table)
CREATE TABLE product_categories (
    category_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    parent_category_id INT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
SELECT create_reference_table('product_categories');

-- Warehouse Inventory (Tenant-specific)
CREATE TABLE inventory (
    tenant_id INT NOT NULL,
    product_id BIGINT NOT NULL,
    warehouse_id INT NOT NULL,
    quantity_on_hand INT NOT NULL DEFAULT 0,
    reorder_level INT NOT NULL DEFAULT 0,
    last_restock_date DATE,
    PRIMARY KEY (tenant_id, product_id, warehouse_id)
);
SELECT create_distributed_table('inventory', 'tenant_id');
```

### 4.3 Index Strategy — ১৫+ টেবিল জয়েনের জন্য

#### ৪.৩.১ Primary Index (Automatic)
```sql
-- প্রতিটি টেবিলের PK (tenant_id, id) স্বয়ংক্রিয়ভাবে ইনডেক্স তৈরি হয়
-- এটি সবচেয়ে গুরুত্বপূর্ণ ইনডেক্স
```

#### ৪.৩.২ Foreign Key Indexes
```sql
-- প্রতিটি FK-তে ইনডেক্স তৈরি করুন
CREATE INDEX idx_order_items_order_id ON order_items (tenant_id, order_id);
CREATE INDEX idx_invoices_order_id ON invoices (tenant_id, order_id);
CREATE INDEX idx_payments_invoice_id ON payments (tenant_id, invoice_id);
CREATE INDEX idx_customer_addresses_customer_id ON customer_addresses (tenant_id, customer_id);
```

**কেন?** জয়েনের সময় FK কলাম দিয়ে লুকআপ হয়। ইনডেক্স ছাড়া ফুল টেবিল স্ক্যান হবে।

#### ৪.৩.৩ Composite Indexes for Common Queries
```sql
-- ঘন ঘন ব্যবহৃত কুয়েরি প্যাটার্নের জন্য
CREATE INDEX idx_orders_date_status ON orders (tenant_id, order_date DESC, order_status);
CREATE INDEX idx_orders_customer_date ON orders (tenant_id, customer_id, order_date DESC);
CREATE INDEX idx_invoices_status_due ON invoices (tenant_id, status, due_date);
CREATE INDEX idx_payments_date ON payments (tenant_id, payment_date DESC);
```

#### ৪.৩.৪ Partial Indexes — র্যাম সাশ্রয়
```sql
-- শুধু অ্যাক্টিভ অর্ডারের ইনডেক্স
CREATE INDEX idx_active_orders ON orders (tenant_id, order_id) 
WHERE order_status NOT IN ('cancelled', 'completed');

-- শুধু আনপেইড ইনভয়েসের ইনডেক্স
CREATE INDEX idx_unpaid_invoices ON invoices (tenant_id, invoice_id)
WHERE status = 'unpaid';

-- গত ৩০ দিনের পেমেন্ট
CREATE INDEX idx_recent_payments ON payments (tenant_id, payment_id)
WHERE payment_date > CURRENT_DATE - INTERVAL '30 days';
```

**Partial Index-এর সুবিধা:**
- ইনডেক্স সাইজ ছোট → র্যামে ফিট
- ইনসার্ট দ্রুত (কম ইনডেক্স আপডেট)
- সাধারণত ৬০-৭০% স্পেস সাশ্রয়

### 4.4 Data Lifecycle Management

```sql
-- পুরনো ডাটা আর্কাইভ করার জন্য
CREATE TABLE orders_archive (LIKE orders INCLUDING ALL);
SELECT create_distributed_table('orders_archive', 'tenant_id');

-- ২ বছরের পুরনো ডাটা আর্কাইভ করুন
INSERT INTO orders_archive 
SELECT * FROM orders 
WHERE order_date < CURRENT_DATE - INTERVAL '2 years'
AND tenant_id = :tenant_id;

DELETE FROM orders 
WHERE order_date < CURRENT_DATE - INTERVAL '2 years'
AND tenant_id = :tenant_id;
```

---

## 🔍 Part 5: Query Optimization — ১৫+ টেবিল জয়েন

### 5.1 সঠিক কুয়েরি প্যাটার্ন

#### ✅ ভালো: Single-Tenant Query
```sql
-- ১৫+ টেবিল জয়েন, কিন্তু tenant_id ফিল্টার সহ
SELECT 
    o.order_id,
    o.order_date,
    c.customer_name,
    oi.product_id,
    p.product_name,
    i.invoice_number,
    pay.payment_ref,
    s.shipping_tracking,
    -- ... আরও ৮-১০ টেবিল
FROM orders o
JOIN customers c ON c.tenant_id = o.tenant_id AND c.customer_id = o.customer_id
JOIN order_items oi ON oi.tenant_id = o.tenant_id AND oi.order_id = o.order_id
JOIN products p ON p.product_id = oi.product_id  -- Reference table
JOIN invoices i ON i.tenant_id = o.tenant_id AND i.order_id = o.order_id
JOIN payments pay ON pay.tenant_id = o.tenant_id AND pay.invoice_id = i.invoice_id
JOIN shipping s ON s.tenant_id = o.tenant_id AND s.order_id = o.order_id
-- ... আরও টেবিল
WHERE o.tenant_id = 105  -- ← এটাই সবচেয়ে গুরুত্বপূর্ণ
  AND o.order_date >= '2024-01-01'
ORDER BY o.order_date DESC;
```

**এক্সিকিউশন প্ল্যান:**
```
Custom Scan (Citus Adaptive)  (cost=0.00..0.00 rows=0 width=0)
  Task Count: 1
  Tasks Shown: All
  ->  Task
        Node: host=10.10.10.12 port=5432 dbname=yourdb
        ->  Sort  (cost=1234.56..1245.67 rows=1000 width=500)
              Sort Key: o.order_date DESC
              ->  Hash Join  (cost=...)
                    ->  Hash Join  (cost=...)
                          ->  Index Scan using idx_orders_date_status on orders o
                          ->  Index Scan using idx_customers_pkey on customers c
                          -- ... আরও জয়েন
```

**কী লক্ষ্য করবেন?** `Task Count: 1` — মানে পুরো কুয়েরি একটি Worker-এ চলেছে, নেটওয়ার্ক ক্রস করেনি।

#### ❌ খারাপ: Cross-Shard Query
```sql
-- tenant_id ফিল্টার ছাড়া
SELECT COUNT(*) FROM orders WHERE order_status = 'pending';
```

**ফলাফল:**
- Coordinator সব Worker থেকে সব টেন্যান্টের ডাটা আনে
- নেটওয়ার্ক ট্রাফিক বিশাল
- Coordinator-এর র্যামে ডাটা লোড হয় → OOM রিস্ক
- পারফরম্যান্স: ১০-১০০ গুণ ধীর

### 5.2 Query Performance Testing

```sql
-- EXPLAIN ANALYZE ব্যবহার করুন
EXPLAIN (ANALYZE, BUFFERS, TIMING) 
SELECT ... FROM orders o
JOIN ... (১৫+ টেবিল)
WHERE o.tenant_id = 105;
```

**কী দেখবেন?**
- `Buffers: shared hit=XXX read=YYY` → hit ratio কত?
- `Execution Time` → টার্গেট: ৫০-১০০ms
- `Task Count` → সবসময় ১ হওয়া উচিত

### 5.3 Common Performance Issues & Solutions

#### সমস্যা ১: Sequential Scan
```sql
-- সমাধান: ইনডেক্স তৈরি করুন
CREATE INDEX idx_orders_tenant_status ON orders (tenant_id, order_status) 
WHERE order_status = 'pending';
```

#### সমস্যা ২: Hash Join বড় টেবিলে
```sql
-- সমাধান: work_mem বাড়ান (কিন্তু সাবধানে)
SET work_mem = '128MB';  -- শুধু এই সেশনে

-- অথবা JOIN অপ্টিমাইজ করুন
SELECT /*+ HASHJOIN(o, oi) */ ...  -- Force hash join
```

#### সমস্যা ৩: LIMIT ছাড়া বড় ফলাফল
```sql
-- ❌ খারাপ
SELECT * FROM orders WHERE tenant_id = 105;

-- ✅ ভালো
SELECT * FROM orders 
WHERE tenant_id = 105 
ORDER BY order_id DESC 
LIMIT 100;
```

---

## 🚀 Part 6: PgBouncer — Connection Pooling

### 6.1 সমস্যা: Connection Overhead

প্রতিটি PostgreSQL কানেকশন:
- একটি OS প্রসেস তৈরি করে
- ~১০ এমবি র্যাম নেয়
- Fork + Initialization ≈ ১০-২০ms

**আপনার ক্ষেত্রে:**
- ১০০০ কানেকশন × ১০ এমবি = ১০ জিবি র্যাম → সিস্টেম ক্র্যাশ
- কানেকশন সেটআপ টাইম যোগ করলে মোট ওভারহেড বিশাল

### 6.2 PgBouncer সেটআপ

```ini
# /etc/pgbouncer/pgbouncer.ini

[databases]
yourdb = host=10.10.10.11 port=5432 dbname=yourdb

[pgbouncer]
listen_addr = *
listen_port = 6432
auth_type = scram-sha-256
auth_file = /etc/pgbouncer/userlist.txt
pool_mode = transaction
max_client_conn = 1000
default_pool_size = 20
min_pool_size = 5
reserve_pool_size = 5
reserve_pool_timeout = 3
max_db_connections = 50
max_user_connections = 50
server_reset_query = DISCARD ALL
server_check_delay = 30
server_check_query = SELECT 1
log_connections = 1
log_disconnections = 1
log_pooler_errors = 1
stats_period = 60
```

### 6.3 Pool Mode ব্যাখ্যা

#### Transaction Mode (আপনার জন্য সেরা):
```sql
-- Client A: BEGIN; SELECT...; COMMIT;
-- এই সময়ে সার্ভার কানেকশন A-কে দেওয়া হয়
-- COMMIT-এর পর অন্য ক্লায়েন্টকে দেওয়া যায়
```

**সুবিধা:**
- সর্বোচ্চ কানেকশন রিইউজ
- ২০ কানেকশনে ১০০০ ক্লায়েন্ট সাপোর্ট

**অসুবিধা:**
- Session-লেভেল ফিচার কাজ করে না (SET, LISTEN/NOTIFY, PREPARE)
- আপনার অ্যাপে `SET search_path` ব্যবহার করা যাবে না

#### Session Mode:
- প্রতিটি ক্লায়েন্ট এক্সক্লুসিভ কানেকশন পায়
- Session ফিচার কাজ করে
- কিন্তু কানেকশন রিইউজ কম

#### Statement Mode:
- শুধু একটি স্টেটমেন্টের জন্য কানেকশন
- অটো-কমিট মোডে ভালো
- ট্রানজেকশনে সমস্যা

### 6.4 অ্যাপ্লিকেশন কানেকশন স্ট্রিং

```
# .env ফাইল
DATABASE_URL=postgresql://user:password@10.10.10.10:6432/yourdb?sslmode=disable
```

**মনে রাখবেন:**
- পোর্ট ৬৪৩২ (PgBouncer), ৫৪৩২ নয়
- Transaction মোডে Prepared Statement সমস্যা হতে পারে

---

## 📊 Part 7: Monitoring & Maintenance

### 7.1 মূল মেট্রিক্স ট্র্যাক করুন

```sql
-- ১. টেবিল ডিস্ট্রিবিউশন চেক
SELECT * FROM citus_tables;

-- ২. শার্ড প্লেসমেন্ট
SELECT shardid, nodename, nodeport, shardminvalue, shardmaxvalue
FROM citus_shard_placement
ORDER BY nodename, shardid;

-- ৩. শার্ড সাইজ
SELECT 
    nodename,
    COUNT(*) AS shard_count,
    pg_size_pretty(SUM(shard_size)) AS total_size
FROM (
    SELECT 
        nodename,
        shardid,
        pg_total_relation_size(shardid::regclass) AS shard_size
    FROM citus_shard_placement
) sub
GROUP BY nodename;
```

### 7.2 Performance Monitoring

```sql
-- ধীর কুয়েরি খুঁজুন
SELECT query, calls, total_time, mean_time, rows
FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 10;

-- লক চেক
SELECT pid, state, wait_event_type, wait_event, query
FROM pg_stat_activity
WHERE wait_event IS NOT NULL;
```

### 7.3 Regular Maintenance

```sql
-- প্রতি রাতে (সব নোডে)
VACUUM ANALYZE orders;
VACUUM ANALYZE order_items;
-- ... সব ১৫০ টেবিল

-- অথবা autovacuum কনফিগার
ALTER TABLE orders SET (autovacuum_vacuum_scale_factor = 0.05);
ALTER TABLE orders SET (autovacuum_analyze_scale_factor = 0.02);
```

---

## ⚠️ Part 8: Critical Production Considerations

### 8.1 Backup Strategy

#### ভুল পদ্ধতি:
```bash
pg_dump -h coordinator yourdb > backup.sql
```
**কেন ভুল?** সব ডাটা Coordinator দিয়ে যায়, Worker-রা ডিরেক্টলি ব্যাকআপ হয় না।

#### সঠিক পদ্ধতি: pgBackRest

```ini
# /etc/pgbackrest/pgbackrest.conf
[global]
repo1-path=/backup/pg