# ফাইল সিস্টেম ভূমিকা : (Introduction to File Systems)

Laravel একটি শক্তিশালী ফাইলসিস্টেম অ্যাবস্ট্রাকশন প্রদান করে, যা **Flysystem** PHP প্যাকেজের (Frank de Jonge তৈরি) উপর ভিত্তি করে তৈরি। Laravel-এর Flysystem ইন্টিগ্রেশন নিম্নলিখিত ড্রাইভারগুলোর জন্য সহজ সমর্থন দেয়:

- **Local** (স্থানীয় ফাইলসিস্টেম)
- **SFTP** (SSH কী-ভিত্তিক FTP)
- **Amazon S3** (ক্লাউড স্টোরেজ)

সবচেয়ে ভালো বিষয় হলো — আপনি সহজেই লোকাল ডেভেলপমেন্ট মেশিন এবং প্রোডাকশন সার্ভারের মধ্যে স্টোরেজ অপশন পরিবর্তন করতে পারবেন, কারণ প্রতিটি সিস্টেমের API একই থাকে।

---

## কনফিগারেশন (Configuration)

Laravel-এর ফাইলসিস্টেম কনফিগারেশন ফাইলটি অবস্থিত:
```
config/filesystems.php
```

এই ফাইলে আপনি আপনার ফাইলসিস্টেমের **ডিস্ক**গুলো কনফিগার করতে পারবেন। প্রতিটি ডিস্ক একটি নির্দিষ্ট স্টোরেজ ড্রাইভার এবং লোকেশন নির্দেশ করে।

- **local** ড্রাইভার: Laravel অ্যাপ্লিকেশন চলমান সার্ভারে স্থানীয়ভাবে সংরক্ষিত ফাইলের সাথে কাজ করে।
- **sftp** ড্রাইভার: SSH কী-ভিত্তিক FTP-এর জন্য ব্যবহৃত হয়।
- **s3** ড্রাইভার: Amazon-এর S3 ক্লাউড স্টোরেজে লেখার জন্য ব্যবহৃত হয়।

আপনি যত খুশি ডিস্ক কনফিগার করতে পারেন, এমনকি একই ড্রাইভার ব্যবহার করে একাধিক ডিস্কও তৈরি করতে পারেন।

### Local ড্রাইভার

local ড্রাইভার ব্যবহার করার সময়, সমস্ত ফাইল অপারেশন কনফিগারেশনে সংজ্ঞায়িত **root** ডিরেক্টরির সাপেক্ষে হয়। ডিফল্টভাবে এটি `storage/app/private` ডিরেক্টরিতে সেট করা থাকে। তাই:

```php
use Illuminate\Support\Facades\Storage;

Storage::disk('local')->put('example.txt', 'Contents');
```

এটি `storage/app/private/example.txt` ফাইলে লিখবে।

### Public ডিস্ক

`public` ডিস্ক সেই ফাইলগুলোর জন্য তৈরি যা **পাবলিকলি অ্যাক্সেসযোগ্য** হবে। ডিফল্টভাবে এটি local ড্রাইভার ব্যবহার করে এবং `storage/app/public`-এ ফাইল সংরক্ষণ করে।

এই ফাইলগুলো ওয়েব থেকে অ্যাক্সেসযোগ্য করতে হলে একটি **সিম্বলিক লিংক** তৈরি করতে হবে:

```bash
php artisan storage:link
```

এটি `storage/app/public` থেকে `public/storage`-এ একটি লিংক তৈরি করবে।

লিংক তৈরি হওয়ার পর, `asset` হেল্পার ব্যবহার করে ফাইলের URL তৈরি করা যাবে:

```php
echo asset('storage/file.txt');
```

আপনি `filesystems` কনফিগারেশন ফাইলে অতিরিক্ত সিম্বলিক লিংকও কনফিগার করতে পারেন:

```php
'links' => [
    public_path('storage') => storage_path('app/public'),
    public_path('images') => storage_path('app/images'),
],
```

`storage:unlink` কমান্ড দিয়ে কনফিগার করা সিম্বলিক লিংকগুলো মুছে ফেলা যায়:

```bash
php artisan storage:unlink
```

### ড্রাইভার প্রয়োজনীয়তা (Driver Prerequisites)

#### S3 ড্রাইভার কনফিগারেশন

S3 ড্রাইভার ব্যবহার করার আগে Composer-এর মাধ্যমে Flysystem S3 প্যাকেজ ইনস্টল করতে হবে:

```bash
composer require league/flysystem-aws-s3-v3 "^3.0" --with-all-dependencies
```

`config/filesystems.php`-তে S3 ডিস্ক কনফিগারেশন থাকে। সাধারণত নিচের এনভায়রনমেন্ট ভেরিয়েবলগুলো ব্যবহার করে S3 তথ্য ও ক্রেডেনশিয়াল কনফিগার করা হয়:

```
AWS_ACCESS_KEY_ID=<your-key-id>
AWS_SECRET_ACCESS_KEY=<your-secret-access-key>
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=<your-bucket-name>
AWS_USE_PATH_STYLE_ENDPOINT=false
```

এই ভেরিয়েবলগুলোর নাম AWS CLI-এর সাথে মিলে যায়।

#### FTP ড্রাইভার কনফিগারেশন

FTP ড্রাইভার ব্যবহার করার আগে:

```bash
composer require league/flysystem-ftp "^3.0"
```

ডিফল্ট কনফিগারেশন ফাইলে FTP-এর নমুনা নেই, তবে এভাবে কনফিগার করা যায়:

```php
'ftp' => [
    'driver' => 'ftp',
    'host' => env('FTP_HOST'),
    'username' => env('FTP_USERNAME'),
    'password' => env('FTP_PASSWORD'),
    // ঐচ্ছিক FTP সেটিংস...
    // 'port' => env('FTP_PORT', 21),
    // 'root' => env('FTP_ROOT'),
    // 'passive' => true,
    // 'ssl' => true,
    // 'timeout' => 30,
],
```

#### SFTP ড্রাইভার কনফিগারেশন

SFTP ড্রাইভার ব্যবহার করার আগে:

```bash
composer require league/flysystem-sftp-v3 "^3.0"
```

কনফিগারেশনের নমুনা:

```php
'sftp' => [
    'driver' => 'sftp',
    'host' => env('SFTP_HOST'),
    // বেসিক অথেন্টিকেশনের সেটিংস...
    'username' => env('SFTP_USERNAME'),
    'password' => env('SFTP_PASSWORD'),
    // SSH কী-ভিত্তিক অথেন্টিকেশনের সেটিংস...
    'privateKey' => env('SFTP_PRIVATE_KEY'),
    'passphrase' => env('SFTP_PASSPHRASE'),
    // ফাইল/ডিরেক্টরি পারমিশন সেটিংস...
    'visibility' => 'private', // `private` = 0600, `public` = 0644
    'directory_visibility' => 'private', // `private` = 0700, `public` = 0755
    // ঐচ্ছিক SFTP সেটিংস...
],
```

### Scoped, Read-Only, এবং Read-Through ফাইলসিস্টেম

**Scoped ডিস্ক**: একটি নির্দিষ্ট পাথ প্রিফিক্স দিয়ে সব পাথ স্বয়ংক্রিয়ভাবে প্রিফিক্স করা যায়। আগে ইনস্টল করতে হবে:

```bash
composer require league/flysystem-path-prefixing "^3.0"
```

উদাহরণ:

```php
's3-videos' => [
    'driver' => 'scoped',
    'disk' => 's3',
    'prefix' => 'path/to/videos',
],
```

**Read-Only ডিস্ক**: রাইট অপারেশন নিষিদ্ধ করে। ইনস্টল:

```bash
composer require league/flysystem-read-only "^3.0"
```

তারপর কনফিগারেশনে যোগ করুন:

```php
's3-videos' => [
    'driver' => 's3',
    // ...
    'read-only' => true,
],
```

**Read-Through ডিস্ক**: ডাউনটাইম ছাড়াই ডিস্কের মধ্যে ফাইল মাইগ্রেট করতে দেয়। পড়ার সময় আগে প্রাইমারি ডিস্ক চেক করে, না থাকলে ফলব্যাক ডিস্ক থেকে পড়ে এবং প্রাইমারিতে কপি করে।

```php
'assets' => [
    'driver' => 'read-through',
    'primary' => 's3',
    'fallback' => 'legacy-s3',
],
```

### Amazon S3-Compatible ফাইলসিস্টেম

`s3` ডিস্ক শুধু Amazon S3-তেই নয়, যেকোনো S3-সামঞ্জস্যপূর্ণ স্টোরেজ সার্ভিসে (যেমন RustFS, DigitalOcean Spaces, Vultr Object Storage, Cloudflare R2, Hetzner Cloud Storage) ব্যবহার করা যায়।

সাধারণত শুধু `endpoint` কনফিগারেশন অপশনের মান আপডেট করতে হয়:

```php
'endpoint' => env('AWS_ENDPOINT', 'https://rustfs:9000'),
```

---

## ডিস্ক ইনস্ট্যান্স পাওয়া (Obtaining Disk Instances)

`Storage` ফ্যাসাদ ব্যবহার করে যেকোনো কনফিগার করা ডিস্কের সাথে কাজ করা যায়:

```php
use Illuminate\Support\Facades\Storage;

Storage::put('avatars/1', $content);
```

একাধিক ডিস্ক থাকলে `disk` মেথড ব্যবহার করুন:

```php
Storage::disk('s3')->put('avatars/1', $content);
```

### On-Demand ডিস্ক

কনফিগারেশন ফাইলে না রেখে রানটাইমে ডিস্ক তৈরি করতে `build` মেথড ব্যবহার করুন:

```php
use Illuminate\Support\Facades\Storage;

$disk = Storage::build([
    'driver' => 'local',
    'root' => '/path/to/root',
]);

$disk->put('image.jpg', $content);
```

---

## ফাইল পুনরুদ্ধার (Retrieving Files)

ফাইলের কন্টেন্ট পড়তে `get` মেথড:

```php
$contents = Storage::get('file.jpg');
```

JSON ফাইলের জন্য `json` মেথড:

```php
$orders = Storage::json('orders.json');
```

ফাইল আছে কিনা চেক করতে `exists`:

```php
if (Storage::disk('s3')->exists('file.jpg')) {
    // ...
}
```

না থাকলে চেক করতে `missing`:

```php
if (Storage::disk('s3')->missing('file.jpg')) {
    // ...
}
```

### ফাইল ডাউনলোড

ব্রাউজারকে ফাইল ডাউনলোড করতে বাধ্য করতে `download` মেথড:

```php
return Storage::download('file.jpg');
return Storage::download('file.jpg', $name, $headers);
```

### ফাইল URL

ফাইলের URL পেতে `url` মেথড:

```php
$url = Storage::url('file.jpg');
```

local ড্রাইভারে পাবলিকলি অ্যাক্সেসযোগ্য ফাইল `storage/app/public`-এ রাখতে হবে এবং `public/storage`-এ সিম্বলিক লিংক তৈরি করতে হবে।

#### URL হোস্ট কাস্টমাইজেশন

ডিস্ক কনফিগারেশনে `url` অপশন যোগ বা পরিবর্তন করে হোস্ট পরিবর্তন করা যায়:

```php
'public' => [
    'driver' => 'local',
    'root' => storage_path('app/public'),
    'url' => env('APP_URL').'/storage',
    'visibility' => 'public',
    'throw' => false,
],
```

### অস্থায়ী URL (Temporary URLs)

`temporaryUrl` মেথড দিয়ে local এবং s3 ড্রাইভারে সংরক্ষিত ফাইলের অস্থায়ী URL তৈরি করা যায়:

```php
$url = Storage::temporaryUrl(
    'file.jpg', now()->plus(minutes: 5)
);
```

#### Local Temporary URL চালু করা

যদি আপনার অ্যাপ্লিকেশন temporary URL সাপোর্টের আগে তৈরি হয়ে থাকে, তাহলে local ডিস্ক কনফিগারেশনে `serve` অপশন যোগ করুন:

```php
'local' => [
    'driver' => 'local',
    'root' => storage_path('app/private'),
    'serve' => true,
    'throw' => false,
],
```

#### S3 রিকোয়েস্ট প্যারামিটার

S3-তে অতিরিক্ত প্যারামিটার দিতে `temporaryUrl`-এর তৃতীয় আর্গুমেন্টে অ্যারে পাঠান:

```php
$url = Storage::temporaryUrl(
    'file.jpg',
    now()->plus(minutes: 5),
    [
        'ResponseContentType' => 'application/octet-stream',
        'ResponseContentDisposition' => 'attachment; filename=file2.jpg',
    ]
);
```

#### Temporary URL কাস্টমাইজেশন

নির্দিষ্ট ডিস্কের জন্য temporary URL তৈরি কাস্টমাইজ করতে `buildTemporaryUrlsUsing` মেথড:

```php
Storage::disk('local')->buildTemporaryUrlsUsing(
    function (string $path, DateTime $expiration, array $options) {
        return URL::temporarySignedRoute(
            'files.download',
            $expiration,
            array_merge($options, ['path' => $path])
        );
    }
);
```

#### Temporary Upload URL

ক্লায়েন্ট-সাইড থেকে সরাসরি ফাইল আপলোড করার জন্য temporary URL তৈরি করতে `temporaryUploadUrl` মেথড (শুধু s3 এবং local ড্রাইভারে):

```php
['url' => $url, 'headers' => $headers] = Storage::temporaryUploadUrl(
    'file.jpg', now()->plus(minutes: 5)
);
```

### ফাইল মেটাডেটা

- `size`: বাইটে ফাইলের আকার
- `lastModified`: শেষ পরিবর্তনের UNIX টাইমস্ট্যাম্প
- `mimeType`: ফাইলের MIME টাইপ
- `path`: ফাইলের পাথ (local ড্রাইভারে অ্যাবসোলিউট, s3-এ রিলেটিভ)

```php
$size = Storage::size('file.jpg');
$time = Storage::lastModified('file.jpg');
$mime = Storage::mimeType('file.jpg');
$path = Storage::path('file.jpg');
```

---

## ফাইল সংরক্ষণ (Storing Files)

`put` মেথড দিয়ে ফাইল কন্টেন্ট সংরক্ষণ:

```php
Storage::put('file.jpg', $contents);
Storage::put('file.jpg', $resource);
```

#### ব্যর্থ লেখা

`put` ব্যর্থ হলে `false` রিটার্ন করে। `throw` অপশন `true` হলে `League\Flysystem\UnableToWriteFile` এক্সেপশন থ্রো করে:

```php
'public' => [
    'driver' => 'local',
    'throw' => true,
],
```

### ফাইলের শুরুতে/শেষে যোগ করা

```php
Storage::prepend('file.log', 'Prepended Text');
Storage::append('file.log', 'Appended Text');
```

### কপি এবং মুভ

```php
Storage::copy('old/file.jpg', 'new/file.jpg');
Storage::move('old/file.jpg', 'new/file.jpg');
```

অন্য ডিস্কে কপি/মুভ করতে:

```php
Storage::disk('local')->copyToDisk('s3', 'reports/report.csv');
Storage::disk('local')->moveToDisk('s3', 'reports/report.csv', 'archive/report.csv');
```

### স্বয়ংক্রিয় স্ট্রিমিং

মেমরি কম ব্যবহার করতে `putFile` বা `putFileAs` মেথড:

```php
$path = Storage::putFile('photos', new File('/path/to/photo'));
$path = Storage::putFileAs('photos', new File('/path/to/photo'), 'photo.jpg');
```

ভিজিবিলিটি নির্দিষ্ট করতে:

```php
Storage::putFile('photos', new File('/path/to/photo'), 'public');
```

### ফাইল আপলোড

আপলোড করা ফাইল সংরক্ষণ করতে `store` মেথড:

```php
$path = $request->file('avatar')->store('avatars');
```

`Storage` ফ্যাসাদেও `putFile` ব্যবহার করা যায়:

```php
$path = Storage::putFile('avatars', $request->file('avatar'));
```

#### ফাইলের নাম নির্দিষ্ট করা

স্বয়ংক্রিয় নাম চাইলে না হলে `storeAs`:

```php
$path = $request->file('avatar')->storeAs('avatars', $request->user()->id);
$path = Storage::putFileAs('avatars', $request->file('avatar'), $request->user()->id);
```

#### ডিস্ক নির্দিষ্ট করা

```php
$path = $request->file('avatar')->store('avatars/'.$request->user()->id, 's3');
$path = $request->file('avatar')->storeAs('avatars', $request->user()->id, 's3');
```

#### অন্যান্য আপলোড ফাইল তথ্য

```php
$name = $file->getClientOriginalName();
$extension = $file->getClientOriginalExtension();
```

নিরাপত্তার জন্য `hashName` এবং `extension` ব্যবহার করা ভালো:

```php
$name = $file->hashName();
$extension = $file->extension();
```

### ফাইল ভিজিবিলিটি (Visibility)

ফাইল `public` বা `private` ঘোষণা করা যায়:

```php
Storage::put('file.jpg', $contents, 'public');
$visibility = Storage::getVisibility('file.jpg');
Storage::setVisibility('file.jpg', 'public');
```

পাবলিক আপলোডের জন্য:

```php
$path = $request->file('avatar')->storePublicly('avatars', 's3');
$path = $request->file('avatar')->storePubliclyAs('avatars', $request->user()->id, 's3');
```

### ইমেজ ম্যানিপুলেশন

আপলোড করা ইমেজ রিসাইজ/ক্রপ/কনভার্ট করতে:

```php
$path = $request->image('avatar')
    ->cover(400, 400)
    ->toWebp()
    ->storePublicly('avatars', 'public');
```

সংরক্ষিত ইমেজ থেকে ইমেজ ইনস্ট্যান্স:

```php
$image = Storage::disk('public')->image('avatars/photo.jpg');
```

#### Local ফাইল এবং ভিজিবিলিটি

local ড্রাইভারে public ভিজিবিলিটি মানে ডিরেক্টরির জন্য 0755 এবং ফাইলের জন্য 0644 পারমিশন। কনফিগারেশনে পরিবর্তন করা যায়:

```php
'local' => [
    'driver' => 'local',
    'root' => storage_path('app'),
    'permissions' => [
        'file' => [
            'public' => 0644,
            'private' => 0600,
        ],
        'dir' => [
            'public' => 0755,
            'private' => 0700,
        ],
    ],
    'throw' => false,
],
```

---

## ফাইল মুছে ফেলা (Deleting Files)

```php
Storage::delete('file.jpg');
Storage::delete(['file.jpg', 'file2.jpg']);
Storage::disk('s3')->delete('path/file.jpg');
```

---

## ডিরেক্টরি (Directories)

```php
// একটি ডিরেক্টরির সব ফাইল
$files = Storage::files($directory);
$files = Storage::allFiles($directory);

// একটি ডিরেক্টরির সব ডিরেক্টরি
$directories = Storage::directories($directory);
$directories = Storage::allDirectories($directory);

// ডিরেক্টরি তৈরি
Storage::makeDirectory($directory);

// ডিরেক্টরি মুছে ফেলা
Storage::deleteDirectory($directory);
```

---

## টেস্টিং (Testing)

`Storage` ফ্যাসাদের `fake` মেথড দিয়ে সহজে ফাইল আপলোড টেস্ট করা যায়:

```php
use Illuminate\Http\UploadedFile;
use Illuminate\Support\Facades\Storage;

test('albums can be uploaded', function () {
    Storage::fake('photos');

    $response = $this->json('POST', '/photos', [
        UploadedFile::fake()->image('photo1.jpg'),
        UploadedFile::fake()->image('photo2.jpg')
    ]);

    Storage::disk('photos')->assertExists('photo1.jpg');
    Storage::disk('photos')->assertExists(['photo1.jpg', 'photo2.jpg']);
    Storage::disk('photos')->assertMissing('missing.jpg');
    Storage::disk('photos')->assertMissing(['missing.jpg', 'non-existing.jpg']);
    Storage::disk('photos')->assertCount('/wallpapers', 2);
    Storage::disk('photos')->assertDirectoryEmpty('/wallpapers');
    Storage::disk('photos')->assertEmpty();
});
```

ডিফল্টভাবে `fake` মেথড টেম্পোরারি ডিরেক্টরির সব ফাইল মুছে ফেলে। ফাইল রাখতে চাইলে `persistentFake` ব্যবহার করুন।

`image` মেথডের জন্য GD এক্সটেনশন প্রয়োজন।

---

## কাস্টম ফাইলসিস্টেম (Custom Filesystems)

Flysystem-এ আরও অনেক স্টোরেজ সিস্টেমের অ্যাডাপ্টার আছে। কাস্টম ড্রাইভার তৈরি করতে একটি Flysystem অ্যাডাপ্টার দরকার। উদাহরণস্বরূপ Dropbox অ্যাডাপ্টার:

```bash
composer require spatie/flysystem-dropbox
```

তারপর সার্ভিস প্রোভাইডারের `boot` মেথডে `Storage::extend` দিয়ে রেজিস্টার করুন:

```php
use Illuminate\Contracts\Foundation\Application;
use Illuminate\Filesystem\FilesystemAdapter;
use Illuminate\Support\Facades\Storage;
use League\Flysystem\Filesystem;
use Spatie\Dropbox\Client as DropboxClient;
use Spatie\FlysystemDropbox\DropboxAdapter;

Storage::extend('dropbox', function (Application $app, array $config) {
    $adapter = new DropboxAdapter(new DropboxClient(
        $config['authorization_token']
    ));

    return new FilesystemAdapter(
        new Filesystem($adapter, $config),
        $adapter,
        $config
    );
});
```

`extend` মেথডের প্রথম আর্গুমেন্ট ড্রাইভারের নাম, দ্বিতীয়টি একটি ক্লোজার যা `$app` এবং `$config` পায়। ক্লোজারকে `Illuminate\Filesystem\FilesystemAdapter`-এর ইনস্ট্যান্স রিটার্ন করতে হবে। এরপর `config/filesystems.php`-তে `dropbox` ড্রাইভার ব্যবহার করা যাবে।

---

**সংক্ষেপে:** Laravel-এর File Storage সিস্টেম একটি শক্তিশালী এবং নমনীয় অ্যাবস্ট্রাকশন, যা local, SFTP, S3 সহ বিভিন্ন স্টোরেজ সিস্টেমের সাথে একই API ব্যবহার করে কাজ করার সুবিধা দেয়। ফাইল সংরক্ষণ, পড়া, ডাউনলোড, URL তৈরি, ভিজিবিলিটি নিয়ন্ত্রণ, ডিরেক্টরি ম্যানেজমেন্ট, টেস্টিং এবং কাস্টম ড্রাইভার — সবকিছুর জন্য সুস্পষ্ট মেথড প্রদান করে।