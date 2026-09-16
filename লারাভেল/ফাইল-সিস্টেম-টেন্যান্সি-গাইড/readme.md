# ফাইলসিস্টেম টেন্যান্সি গাইড

```markdown
> **সংস্করণ:** Laravel Tenancy v3
> **লক্ষ্য:** Local disk ব্যবহার করে প্রতিটি tenant-এর জন্য আলাদা storage ব্যবস্থাপনা
```

## 📌 সূচিপত্র

1. [ভূমিকা](#ভূমিকা)
2. [কেন Local Disk?](#কেন-local-disk)
3. [প্রয়োজনীয়তা](#প্রয়োজনীয়তা)
4. [Bootstrapper কীভাবে কাজ করে](#bootstrapper-কীভাবে-কাজ-করে)
5. [ধাপে ধাপে সেটআপ](#ধাপে-ধাপে-সেটআপ)
6. [Folder Structure](#folder-structure)
7. [Storage Facade ব্যবহার](#storage-facade-ব্যবহার)
8. [Asset Helper ব্যবহার](#asset-helper-ব্যবহার)
9. [সাধারণ সমস্যা ও সমাধান](#সাধারণ-সমস্যা-ও-সমাধান)
10. [সেরা অনুশীলন](#সেরা-অনুশীলন)

---

## ভূমিকা

Laravel Tenancy প্যাকেজে **Filesystem Tenancy Bootstrapper** ব্যবহার করে প্রতিটি tenant-এর জন্য আলাদা storage path তৈরি করা যায়। এই গাইডে আমরা **local disk** ব্যবহার করে কীভাবে সম্পূর্ণ সেটআপ করব তা দেখব।

মূল ধারণা: প্রতিটি tenant-এর ফাইল আলাদা folder-এ থাকবে, যাতে এক tenant অন্য tenant-এর ফাইল দেখতে বা পরিবর্তন করতে না পারে।

---

## কেন Local Disk?

Local disk ব্যবহারের সুবিধা:

- ✅ **সহজ সেটআপ** — কোনো cloud service লাগে না
- ✅ **দ্রুত** — local file system সরাসরি access
- ✅ **ডেভেলপমেন্টে সুবিধাজনক** — testing সহজ
- ✅ **খরচ নেই** — S3 বা cloud storage bill নেই
- ✅ **Backup সহজ** — পুরো `storage` folder কপি করলেই হয়

**কখন Local Disk ব্যবহার করবেন না:**
- ❌ একাধিক server চালালে (load balancing)
- ❌ Horizontal scaling দরকার হলে
- ❌ CDN integration দরকার হলে

---

## প্রয়োজনীয়তা

- Laravel 9+ (v3 এর জন্য)
- `stancl/tenancy` প্যাকেজ v3
- PHP 8.0+
- Composer

**প্যাকেজ ইনস্টল (যদি না করা থাকে):**

```bash
composer require stancl/tenancy
php artisan tenancy:install
```

---

## Bootstrapper কীভাবে কাজ করে

Filesystem Bootstrapper তিনটি জিনিস tenant-aware করে:

| Component | কী পরিবর্তন হয় |
|---|---|
| `storage_path()` | Path-এ tenant suffix যোগ হয় |
| `Storage` facade | Disk root-এ tenant suffix বসে |
| `asset()` helper | Tenant-এর asset URL return করে |

**Suffix গঠন:**
```
suffix = suffix_base + tenant_key
```

- `suffix_base` — default `tenant` (config-এ পরিবর্তনযোগ্য)
- `tenant_key` — tenant এর unique ID

**উদাহরণ:** tenant ID = `42`, `suffix_base` = `tenant` হলে suffix = `tenant42`

---

## ধাপে ধাপে সেটআপ

### ধাপ ১: Config প্রকাশ করুন

```bash
php artisan vendor:publish --tag=tenancy
```

এটি `config/tenancy.php` ফাইল তৈরি করবে।

### ধাপ ২: Bootstrapper সক্রিয় করুন

`config/tenancy.php` ফাইলে `bootstrappers` array-তে `FilesystemTenancyBootstrapper` আছে কিনা নিশ্চিত করুন:

```php
'bootstrappers' => [
    Stancl\Tenancy\Bootstrappers\DatabaseTenancyBootstrapper::class,
    Stancl\Tenancy\Bootstrappers\CacheTenancyBootstrapper::class,
    Stancl\Tenancy\Bootstrappers\FilesystemTenancyBootstrapper::class, // ← এটি থাকতে হবে
    Stancl\Tenancy\Bootstrappers\QueueTenancyBootstrapper::class,
],
```

### ধাপ ৩: Filesystem Config সেট করুন

`config/tenancy.php` এর `filesystem` section-এ local disk কনফিগার করুন:

```php
'filesystem' => [
    // Tenant suffix এর base নাম
    'suffix_base' => 'tenant',
    
    // যেসব disk tenant-aware হবে
    'disks' => [
        'local',
        'public',
    ],
    
    // Disk root override — Laravel এর নিজস্ব suffixing conflict এড়াতে
    'root_override' => [
        'local'  => '%storage_path%/app/',
        'public' => '%storage_path%/app/public/',
    ],
    
    // asset() helper tenant-aware করবে কিনা
    'asset_helper_tenancy' => true,
],
```

### ধাপ ৪: Laravel Filesystem Config যাচাই করুন

`config/filesystems.php` ফাইলে `local` এবং `public` disk সঠিকভাবে আছে কিনা দেখুন:

```php
'disks' => [
    'local' => [
        'driver' => 'local',
        'root' => storage_path('app'),
        'throw' => false,
    ],

    'public' => [
        'driver' => 'local',
        'root' => storage_path('app/public'),
        'url' => env('APP_URL') . '/storage',
        'visibility' => 'public',
        'throw' => false,
    ],
],
```

> ⚠️ **গুরুত্বপূর্ণ:** `root` এখানে সাধারণ `storage_path('app')` থাকবে — tenancy bootstrapper runtime-এ এটাকে suffix করবে।

### ধাপ ৫: Storage Directory Structure তৈরি করুন

Tenant তৈরি হলে তার জন্য প্রাথমিক folder structure তৈরি করা দরকার। এজন্য একটি **Tenancy Event Listener** লিখুন।

`app/Providers/TenancyServiceProvider.php` ফাইলে:

```php
use Stancl\Tenancy\Events\TenantCreated;
use Stancl\Tenancy\Events\TenancyInitialized;
use Illuminate\Support\Facades\Event;
use Illuminate\Support\Facades\File;

Event::listen(TenancyInitialized::class, function () {
    $tenant = tenant();
    
    // Tenant এর জন্য প্রয়োজনীয় folder তৈরি
    $paths = [
        storage_path('app/public'),
        storage_path('framework/cache'),
        storage_path('framework/sessions'),
        storage_path('framework/views'),
    ];
    
    foreach ($paths as $path) {
        if (!File::exists($path)) {
            File::makeDirectory($path, 0755, true);
        }
    }
});
```

### ধাপ ৬: Tenant Asset Middleware সেট করুন

`app/Providers/TenancyServiceProvider.php` এর `boot()` method-এ:

```php
use Stancl\Tenancy\Controllers\TenantAssetsController;
use App\Http\Middleware\InitializeTenancyByDomain; // আপনার middleware

public function boot()
{
    TenantAssetsController::$tenancyMiddleware = InitializeTenancyByDomain::class;
    
    // ... বাকি কোড
}
```

### ধাপ ৭: Storage Symlink তৈরি করুন

Public disk এর জন্য symlink দরকার:

```bash
php artisan storage:link
```

> 💡 **টেন্যান্ট-স্পেসিফিক symlink:** প্রতিটি tenant এর জন্য আলাদা symlink লাগলে listener-এ যোগ করুন। তবে বেশিরভাগ ক্ষেত্রে `TenantAssetsController` ব্যবহার করাই ভালো।

---

## Folder Structure

সেটআপ সম্পন্ন হলে folder structure এমন হবে:

```
storage/
├── logs/                           # সব tenant এর common log
│   └── laravel.log
│
├── app/                            # (Laravel এর default, tenancy তে ব্যবহার হয় না)
│
├── tenant1/
│   ├── app/
│   │   ├── public/                 # tenant1 এর public files
│   │   │   ├── avatar.jpg
│   │   │   └── documents/
│   │   └── private/                # tenant1 এর private files
│   │
│   └── framework/
│       ├── cache/
│       ├── sessions/
│       └── views/
│
├── tenant2/
│   ├── app/
│   │   └── public/
│   └── framework/
│       ├── cache/
│       ├── sessions/
│       └── views/
│
└── tenant3/
    └── ... (একই structure)
```

**গুরুত্বপূর্ণতা:**

- `storage/logs/` — সব tenant common (debugging সহজ)
- `storage/tenantX/` — প্রতিটি tenant আলাদা
- Cache, session, views সব tenant আলাদা

---

## Storage Facade ব্যবহার

### ফাইল আপলোড (Local Disk)

```php
use Illuminate\Support\Facades\Storage;

// Tenant context-এ চললে স্বয়ংক্রিয়ভাবে tenant এর folder-এ save হবে
$path = $request->file('avatar')->store('avatars', 'public');

// Path হবে: storage/tenant1/app/public/avatars/xxx.jpg
```

### ফাইল পড়া

```php
// Tenant1 এর context-এ
$content = Storage::disk('local')->get('documents/file.pdf');

// প্রকৃত path: storage/tenant1/app/documents/file.pdf
```

### ফাইল Delete

```php
Storage::disk('public')->delete('avatars/old.jpg');
// Delete হবে: storage/tenant1/app/public/avatars/old.jpg
```

### Absolute Path পাওয়া

```php
$fullPath = Storage::disk('local')->path('reports/monthly.pdf');
// রিটার্ন: /var/www/app/storage/tenant1/app/reports/monthly.pdf
```

### Directory তালিকা

```php
$files = Storage::disk('public')->files('avatars');
// Tenant1 এর avatars folder-এর files
```

---

## Asset Helper ব্যবহার

### Tenant Asset (default)

```blade
{{-- Tenant1 এর context-এ --}}
<img src="{{ asset('images/logo.png') }}">
{{-- Output: https://example.com/tenancy/assets/images/logo.png --}}
```

এই URL টি `TenantAssetsController` handle করে এবং tenant1 এর `storage/tenant1/app/public/images/logo.png` থেকে ফাইল serve করে।

### Global Asset (সব tenant common)

```blade
{{-- Global branding disk থেকে --}}
<img src="{{ Storage::disk('branding')->url('header-logo.png') }}">
```

### Global JS/CSS

```blade
<link rel="stylesheet" href="{{ global_asset('css/app.css') }}">
<script src="{{ mix('js/app.js') }}"></script>
```

### Disable করলে

```php
// config/tenancy.php
'asset_helper_tenancy' => false,
```

এরপর explicit `tenant_asset()` ব্যবহার করুন:

```blade
<img src="{{ tenant_asset('images/logo.png') }}">
```

---

## সাধারণ সমস্যা ও সমাধান

### ❌ সমস্যা ১: Cache/Session folder নেই

**Error:** `Failed to open stream: No such file or directory`

**সমাধান:** TenancyInitialized listener-এ folder creation যোগ করুন (ধাপ ৫ দেখুন)।

### ❌ সমস্যা ২: Asset লোড হচ্ছে না

**Error:** 404 Not Found

**কারণ:** `TenantAssetsController::$tenancyMiddleware` সেট করা হয়নি।

**সমাধান:**
```php
// TenancyServiceProvider::boot() এ
TenantAssetsController::$tenancyMiddleware = InitializeTenancyByDomain::class;
```

### ❌ সমস্যা ৩: Storage path suffix দ্বিগুণ হচ্ছে

**Error:** `/storage/tenant1/tenant1/app/...`

**সমাধান:** `config/filesystems.php`-এ root সাদা রাখুন (`storage_path('app')`), এবং `config/tenancy.php` এর `root_override` ব্যবহার করুন।

### ❌ সমস্যা ৪: Logs tenant অনুযায়ী আলাদা হচ্ছে

**সমস্যা:** ডিবাগিং কঠিন হয়ে যাচ্ছে।

**সমাধান:** এটা স্বাভাবিক — logs সবসময় `storage/logs/`-এ থাকে, suffix হয় না। আলাদা করতে চাইলে custom bootstrapper লিখুন।

### ❌ সমস্যা ৫: Queue job-এ tenant context হারিয়ে যাচ্ছে

**সমাধান:** `QueueTenancyBootstrapper` সক্রিয় করুন (default-এ থাকে)।

### ❌ সমস্যা ৬: Permission denied

**সমাধান:**
```bash
chmod -R 775 storage/
chown -R www-data:www-data storage/
```

---

## সেরা অনুশীলন

### ✅ করুন

1. **Listener-এ folder তৈরি করুন** — প্রতিটি tenant তৈরি হলে প্রাথমিক structure বানান
2. **Global assets আলাদা disk-এ রাখুন** — JS, CSS, logo ইত্যাদি
3. **`root_override` config ব্যবহার করুন** — এটা সবচেয়ে reliable
4. **Regular backup নিন** — প্রতিটি tenant folder আলাদাভাবে
5. **Disk space monitor করুন** — multi-tenant এ অনেক space লাগে
6. **`.gitignore`-এ tenant folder যোগ করুন** — যাতে git-এ commit না হয়

### ❌ করবেন না

1. **`storage_path()` hardcode করবেন না** — সবসময় helper ব্যবহার করুন
2. **Direct `storage/` path লিখবেন না** — tenant suffix এড়িয়ে যাবে
3. **Tenant folder manually delete করবেন না** — event দিয়ে handle করুন
4. **`config/filesystems.php`-এ tenant logic লিখবেন না** — tenancy config-এ রাখুন

---

## Production Deployment Checklist

- [ ] Storage folder-এ সঠিক permission (`775` বা `755`)
- [ ] `storage:link` চালানো হয়েছে
- [ ] Backup strategy তৈরি হয়েছে
- [ ] Disk space monitoring সক্রিয়
- [ ] Tenant deletion cleanup logic আছে
- [ ] Log rotation সেট করা
- [ ] `.gitignore`-এ tenant folder যোগ করা

---

## সম্পূর্ণ Config উদাহরণ

### `config/tenancy.php`

```php
return [
    'tenant_model' => \App\Models\Tenant::class,
    'id_generator' => Stancl\Tenancy\UUIDGenerator::class,

    'domain_model' => \Stancl\Tenancy\Database\Models\Domain::class,

    'central_domains' => [
        '127.0.0.1',
        'localhost',
    ],

    'bootstrappers' => [
        Stancl\Tenancy\Bootstrappers\DatabaseTenancyBootstrapper::class,
        Stancl\Tenancy\Bootstrappers\CacheTenancyBootstrapper::class,
        Stancl\Tenancy\Bootstrappers\FilesystemTenancyBootstrapper::class,
        Stancl\Tenancy\Bootstrappers\QueueTenancyBootstrapper::class,
    ],

    'filesystem' => [
        'suffix_base' => 'tenant',
        'disks' => [
            'local',
            'public',
        ],
        'root_override' => [
            'local' => '%storage_path%/app/',
            'public' => '%storage_path%/app/public/',
        ],
        'asset_helper_tenancy' => true,
    ],
];
```

---

## সহায়ক লিংক

- [Tenancy for Laravel - Official Docs](https://tenancyforlaravel.com/docs/v3/)
- [Filesystem Bootstrapper](https://tenancyforlaravel.com/docs/v3/tenancy-bootstrappers/#filesystem-tenancy-boostrapper)
- [Laravel Filesystem Docs](https://laravel.com/docs/filesystem)

---

Tenancy for Laravel-এর ডিফল্ট ডিজাইনে, টেন্যান্টের পাবলিক ফাইলগুলি সরাসরি `public/storage` সিমলিংকের মাধ্যমে অ্যাক্সেস করা হয় **না**। আপনার ছবি সত্যিই বিদ্যমান, কিন্তু সঠিক URL পাথটি হলো `/tenancy/assets/...`, `/storage/...` নয়।

### কেন `public/storage`-এ ফাইল দেখা যায় না?

Tenancy-এর Filesystem Bootstrapper চালু হলে, `storage_path()` ফাংশন একটি টেন্যান্ট সাফিক্স যোগ করে (`storage/tenant_ihelpkl/`-এর মতো)। কিন্তু Laravel-এর ডিফল্ট `public/storage` সিমলিংক শুধুমাত্র `storage/app/public`-এর দিকে নির্দেশ করে, যা সেন্ট্রাল স্টোরেজ পাথ, টেন্যান্টের সাবডিরেক্টরি নয়।

তাই `php artisan storage:link` চালানোর পরেও, `public/storage` সবসময় `storage/app/public`-এর দিকে নির্দেশ করে, আপনার টেন্যান্টের ফাইলগুলি `storage/tenant_ihelpkl/app/public/`-এ থাকে, সিমলিংক স্বাভাবিকভাবেই সেগুলি অ্যাক্সেস করতে পারে না।

### সমাধান

টেন্যান্ট অ্যাসেট অ্যাক্সেস করার জন্য, সঠিক পদ্ধতি হলো **TenantAssetsController** ব্যবহার করা, যা `/tenancy/assets/...` পাথে রিকোয়েস্ট গ্রহণ করে, তারপর টেন্যান্টের `storage_path('app/public/')` থেকে ফাইল পড়ে প্রতিক্রিয়া জানায়।

#### ১. `tenant_asset()` ব্যবহার করে সঠিক URL তৈরি করুন

ব্লেড টেমপ্লেটে, `asset()` পরিবর্তে `tenant_asset()` ব্যবহার করুন:

```blade
{{-- ভুল --}}
<img src="{{ asset('storage/tenant/branding/logo.png') }}">

{{-- সঠিক --}}
<img src="{{ tenant_asset('tenant/branding/logo.png') }}">
```

`tenant_asset('tenant/branding/logo.png')` আউটপুট দেবে `https://your-domain.com/tenancy/assets/tenant/branding/logo.png` এর মতো URL।

#### ২. নিশ্চিত করুন যে Middleware কনফিগার করা হয়েছে

`TenancyServiceProvider`-এর `boot()` মেথডে, `TenantAssetsController`-এর জন্য টেন্যান্ট আইডেন্টিফিকেশন Middleware সেট করুন:

```php
use Stancl\Tenancy\Controllers\TenantAssetsController;

public function boot()
{
    TenantAssetsController::$tenancyMiddleware = InitializeTenancyByDomain::class;
    // অথবা আপনার ব্যবহৃত টেন্যান্ট আইডেন্টিফিকেশন Middleware
}
```

এই ধাপটি বাদ দিলে, অ্যাসেট রিকোয়েস্ট সঠিকভাবে টেন্যান্ট আইডেন্টিফাই করতে পারবে না এবং ব্যর্থ হবে।

#### ৩. নিশ্চিত করুন যে ফাইলগুলি সঠিক অবস্থানে আছে

`tenant_asset()` প্রত্যাশা করে যে ফাইলগুলি টেন্যান্টের `app/public/` ডিরেক্টরিতে থাকবে। আপনার ক্ষেত্রে, ফাইল পাথ হলো:

```
storage/tenant_ihelpkl/app/public/tenant/ihelpkl/branding/
```

অতএব `tenant_asset('tenant/ihelpkl/branding/your-image.jpg')` সঠিকভাবে অ্যাক্সেস করতে পারবে।

### বিকল্প: আপনি যদি `public/storage` পদ্ধতি ব্যবহার করতে চান

আপনি যদি অবশ্যই `public/storage`-এর মাধ্যমে টেন্যান্ট ফাইল অ্যাক্সেস করতে চান, তাহলে একটি কাস্টম টেন্যান্ট ডিস্ক তৈরি করতে পারেন, যার `url` সরাসরি টেন্যান্ট অ্যাসেট পাথে নির্দেশ করে:

`config/filesystems.php`-এ যোগ করুন:

```php
'tenant' => [
    'driver' => 'local',
    'root' => storage_path('app/public'),
    'url' => '/tenancy/assets',
    'visibility' => 'public',
    'throw' => false,
],
```

তারপর Filament বা অন্যান্য জায়গায় `->disk('tenant')` ব্যবহার করুন। তবে মনে রাখবেন, এই পদ্ধতিতে URL এখনও `/tenancy/assets/...`-এর মতোই হবে, `public/storage` সিমলিংকের মাধ্যমে যাবে না।

### সারসংক্ষেপ

| বিষয় | সেন্ট্রাল (সাধারণ Laravel) | টেন্যান্ট |
|------|--------------------------|-----------|
| ফাইল স্টোরেজ পাথ | `storage/app/public/` | `storage/tenant_xxx/app/public/` |
| `public/storage` সিমলিংক | ✅ কাজ করে | ❌ কাজ করে না |
| সঠিক URL পদ্ধতি | `asset('storage/...')` | `tenant_asset('...')` |
| URL পাথ | `/storage/...` | `/tenancy/assets/...` |

সুতরাং আপনার ছবি "অদৃশ্য" নয়, বরং সেগুলি টেন্যান্ট আইসোলেশনের জন্য সঠিক জায়গায় সংরক্ষিত আছে, শুধুমাত্র একটি ভিন্ন URL পাথের মাধ্যমে অ্যাক্সেস করা প্রয়োজন।

## 📅 পরিবর্তন ইতিহাস

| তারিখ | সংস্করণ | পরিবর্তন |
|------|---------|---------|
| 2026-09-16 | 1.0 | প্রাথমিক সংস্করণ |

---
