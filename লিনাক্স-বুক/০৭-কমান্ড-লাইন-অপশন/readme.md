> [🏠](../) [⬅️ ০৬। লিনাক্স প্যাকেজ ইনস্টলেশন আর্কিটেকচার](../০৬-লিনাক্স-প্যাকেজ-ইনস্টলেশন-আর্কিটেকচার) [➡️ ০৮। উবুন্টুর সেল](../০৮-উবুন্টুর-সেল)


# ০৭। কমান্ড লাইন অপশন: `-`, `--`, `-O`, `--dearmor`

## ভূমিকা

Linux কমান্ড ব্যবহার করার সময় প্রায়ই `-a`, `-l`, `-r`, `--help`, `--version`, `--dearmor` ইত্যাদি অপশন দেখা যায়। এগুলোকে command-line option বা flag বলা হয়।

## Single Dash (`-`)

একটি `-` সাধারণত short option বোঝায়।

```bash
ls -l
```

এখানে `-l` অর্থ long listing format।

আরও উদাহরণ:

```bash
cp -r source destination
```

এখানে `-r` অর্থ recursive।

একাধিক short option একসাথে লেখা যায়:

```bash
ls -lah
```

এটি সমান:

```bash
ls -l -a -h
```

## Double Dash (`--`)

দুটি ড্যাশ সাধারণত long option বোঝায়।

```bash
gpg --dearmor
```

এখানে `--dearmor` একটি long option।

অনেক ক্ষেত্রে short এবং long option একই কাজ করে।

```bash
apt install -y nginx
```

এবং

```bash
apt install --yes nginx
```

## `-O` বনাম `--dearmor`

```bash
gpg --dearmor -O output.gpg input.key
```

- `--dearmor` → ASCII-armored key কে binary keyring format-এ রূপান্তর করে।
- `-O output.gpg` → output কোন ফাইলে লেখা হবে তা নির্ধারণ করে।

## শুধুমাত্র `--` এর বিশেষ ব্যবহার

শুধু `--` লিখলে option parsing বন্ধ হয়ে যায়।

ধরুন একটি ফাইলের নাম:

```bash
-test.txt
```

ভুল:

```bash
rm -test.txt
```

সঠিক:

```bash
rm -- -test.txt
```

এখানে `--` বলছে, এর পরের অংশকে option নয়, filename হিসেবে বিবেচনা করতে।

## সংক্ষিপ্তসার

| Syntax | অর্থ |
|---------|------|
| `-a` | Short option |
| `-r` | Short option |
| `-O file` | Argument সহ short option |
| `--help` | Long option |
| `--dearmor` | Long option |
| `--` | End of options marker |

> [🏠](../) [⬅️ ০৬। লিনাক্স প্যাকেজ ইনস্টলেশন আর্কিটেকচার](../০৬-লিনাক্স-প্যাকেজ-ইনস্টলেশন-আর্কিটেকচার) [➡️ ০৮। উবুন্টুর সেল](../০৮-উবুন্টুর-সেল)