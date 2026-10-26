# E.164 কী?

**E.164** হলো international telephone numbering-এর standard format। এর মূল নিয়ম:

```text
+[Country Code][National Number]
```

এখানে:

* `+` → international format বোঝায়
* Country Code → দেশের code
* National Number → দেশের national phone number
* কোনো space, `-`, `(`, `)` থাকে না

## বাংলাদেশি উদাহরণ

ধরো একটি নম্বর:

```text
01712345678
```

বাংলাদেশের country code:

```text
+880
```

তাহলে E.164 হবে:

```text
+8801712345678
```

আর:

```text
+880 1712-345678
```

বা

```text
+880 (1712) 345678
```

এগুলোও একই নম্বরের বিভিন্ন representation, কিন্তু database-এ normalized value হিসেবে রাখা উচিত:

```text
+8801712345678
```

---

## ২. Normalize বলতে কী বোঝায়?

User বিভিন্নভাবে phone number দিতে পারে:

```text
01712345678
+8801712345678
+880 1712345678
01712-345678
8801712345678
```

তোমার application এগুলোকে একটি **canonical format**-এ convert করবে।

```text
01712345678
        ↓
+8801712345678
```

এটাই normalization।

অর্থাৎ database-এ ideally:

```text
phone = "+8801712345678"
```

রাখবে।

---

## ৩. কেন E.164 database-এ রাখা ভালো?

ধরো তোমার CRM-এ একই customer-এর number দুইভাবে আছে:

```text
01712345678
+8801712345678
```

Application যদি simple string comparison করে, তাহলে এগুলো আলাদা মনে হবে।

ফলে:

* duplicate contact তৈরি হতে পারে
* WhatsApp integration সমস্যা করতে পারে
* SMS gateway-তে সমস্যা হতে পারে
* call system-এর matching ভুল হতে পারে
* customer search inconsistent হতে পারে
* lead/contact merge করা কঠিন হবে

কিন্তু দুটোকে normalize করলে:

```text
+8801712345678
+8801712345678
```

তখন সহজেই বুঝবে যে দুটো একই number।

---

## ৪. Database-এ কী রাখবে?

আমি সাধারণত এই architecture recommend করব:

```text
phone
---------
+8801712345678
```

অর্থাৎ **canonical E.164 value**।

যদি phone number-এর original user input-ও business requirement অনুযায়ী রাখতে হয়, তাহলে আলাদা field রাখা যায়:

```text
phone             = +8801712345678
phone_raw         = 01712-345678
```

তবে `phone_raw` শুধু প্রয়োজন হলে। সাধারণ CRM-এর জন্য শুধু normalized `phone`-ই যথেষ্ট হতে পারে।

---

## ৫. Localize বলতে কী?

এখানে একটা গুরুত্বপূর্ণ distinction আছে।

***Database-এ E.164 রাখবে, কিন্তু UI-তে সবসময় E.164 দেখানো বাধ্যতামূলক নয়।***

ধরো database:

```text
+8801712345678
```

User Bangladesh থেকে CRM ব্যবহার করছে।

UI-তে তুমি দেখাতে পারো:

```text
01712-345678
```

অথবা:

```text
01712345678
```

অর্থাৎ:

```text
Database
    ↓
E.164
+8801712345678
    ↓
Display formatter
    ↓
01712-345678
```

এটাই **localize for display**।

---

## ৬. Normalize এবং Localize-এর পার্থক্য

সহজভাবে:

| বিষয়            | কাজ                                      |
| --------------- | ---------------------------------------- |
| Normalize       | Input → standard/canonical format        |
| Store           | Database-এ canonical format রাখা         |
| Localize        | Canonical format → user-friendly display |
| API/Integration | সাধারণত E.164 ব্যবহার করা                |

উদাহরণ:

```text
User Input
01712-345678
      ↓
Normalize
+8801712345678
      ↓
Database
+8801712345678
      ↓
Localize
01712-345678
```

---

## ৭. CRM-এর জন্য সবচেয়ে ভালো flow

তোমার SaaS CRM-এর ক্ষেত্রে আমি flow-টা এভাবে রাখতাম:

```text
                ┌─────────────────┐
                │ User enters     │
                │ phone number    │
                └────────┬────────┘
                         ↓
                ┌─────────────────┐
                │ Parse country   │
                │ + validate      │
                └────────┬────────┘
                         ↓
                ┌─────────────────┐
                │ Normalize       │
                │ to E.164        │
                └────────┬────────┘
                         ↓
                ┌─────────────────┐
                │ Database        │
                │ +8801712345678  │
                └────────┬────────┘
                         ↓
              ┌──────────┴──────────┐
              ↓                     ↓
        External APIs            CRM UI
        WhatsApp/SMS             Display
              ↓                     ↓
        E.164 format          Local format
        +8801712345678        01712-345678
```

---

## ৮. WhatsApp / SMS / GSM-এর ক্ষেত্রে

এখানে E.164-এর গুরুত্ব আরও বেশি।

ধরো তোমার CRM থেকে WhatsApp API-তে পাঠাতে হবে:

```text
+8801712345678
```

API-কে সাধারণত এই canonical international number দেওয়াই নিরাপদ।

একইভাবে SMS provider বা telephony provider যদি international numbering চায়, তাহলে:

```text
+8801712345678
```

ব্যবহার করবে।

***UI-এর localized format API-তে পাঠাবে না।***

অর্থাৎ:

```text
UI:
01712-345678

Database:
+8801712345678

WhatsApp API:
+8801712345678

SMS API:
+8801712345678
```

এটা খুব clean architecture।

---

## ৯. Country code কীভাবে handle করবে?

এখানে একটা গুরুত্বপূর্ণ বিষয় হলো:

```text
01712345678
```

থেকে system কীভাবে বুঝবে যে এটা Bangladesh-এর number?

যদি application-এর context নির্দিষ্ট Bangladesh হয়, default country হিসেবে `BD` ব্যবহার করা যায়।

কিন্তু SaaS CRM যদি international হয়, তাহলে user-এর country context দরকার হতে পারে।

যেমন:

```text
01712345678
```

with:

```text
country = BD
```

→

```text
+8801712345678
```

অন্যদিকে:

```text
4155552671
```

with:

```text
country = US
```

→

```text
+14155552671
```

তাই international SaaS-এর জন্য **default country assumption খুব সাবধানে** করতে হবে।

---

## ১০. শুধু string manipulation করে normalize করা উচিত?

Production CRM-এ আমি recommend করব **phone-number parsing library** ব্যবহার করতে।

কারণ phone numbering rules শুধু:

```text
0 remove
+880 add
```

এত simple না।

প্রতিটি দেশের:

* country code
* national prefix
* number length
* mobile prefix
* landline rules
* valid ranges
* formatting rules

আলাদা হতে পারে।

PHP/Laravel ecosystem-এ Google-এর **libphonenumber**-based package ব্যবহার করা খুব common approach।

---

## ১১. Laravel-এ conceptual implementation

তোমার Laravel CRM-এ ধরো request:

```json
{
    "phone": "01712-345678"
}
```

Backend:

```text
Request
   ↓
Phone Normalizer
   ↓
+8801712345678
   ↓
Validation
   ↓
Save
```

Database:

```php
$contact->phone = $normalizedPhone;
$contact->save();
```

তারপর React/Inertia frontend-এ database থেকে:

```text
+8801712345678
```

আসবে।

Display formatter সেটাকে:

```text
01712-345678
```

দেখাবে।

---

## ১২. Search-এর জন্যও সুবিধা

এটা CRM-এর জন্য খুব গুরুত্বপূর্ণ।

User search করল:

```text
01712345678
```

কিন্তু database:

```text
+8801712345678
```

তাহলে search layer-এ input-টাকেও normalize করতে পারবে:

```text
Search input
01712345678
       ↓
Normalize
+8801712345678
       ↓
DB query
WHERE phone = '+8801712345678'
```

ফলে consistent matching হবে।

---

## ১৩. Duplicate detection

এটাও CRM-এ খুব useful।

ধরো Lead A:

```text
01712345678
```

Lead B:

```text
+8801712345678
```

Normalization-এর পরে:

```text
Lead A → +8801712345678
Lead B → +8801712345678
```

তারপর unique constraint বা duplicate detection করা সহজ:

```text
phone = +8801712345678
```

তোমার CRM-এ Contact/Lead deduplication-এর জন্য এটা বিশেষভাবে গুরুত্বপূর্ণ।

---

## ১৪. একটা গুরুত্বপূর্ণ rule

আমি architecture হিসেবে এই rule রাখতাম:

> **Never modify the stored E.164 value just for display.**

অর্থাৎ database:

```text
+8801712345678
```

সবসময় একই থাকবে।

Display layer decide করবে:

```text
Bangladesh user:
01712-345678

International context:
+880 1712 345678

Raw/API context:
+8801712345678
```

এতে **storage layer এবং presentation layer আলাদা** থাকে।

---

## তোমার CRM-এর জন্য recommended architecture

শেষ পর্যন্ত তিনটা layer ভাবতে পারো:

```text
1. INPUT
   01712-345678

        ↓

2. NORMALIZATION / STORAGE
   +8801712345678

        ↓

3. PRESENTATION
   01712-345678
```

আর external integrations:

```text
WhatsApp → +8801712345678
SMS      → +8801712345678
GSM      → provider-required international format
Reports  → localized display
Search   → normalize before query
Database → E.164
```

### এক লাইনে

***Store phone numbers in E.164 as the canonical source of truth; localize/format them only at the presentation layer.***