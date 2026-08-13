>  [🏠](../) [➡️  ০১। লিনাক্স পরিচিতি](./০১-লিনাক্স-পরিচিতি)

# ০০। সংক্ষিপ্ত প্রশ্নোত্তর (FAQ)

### Linux কী?
Linux মূলত একটি Kernel। Kernel-এর সাথে Shell, Package Manager, Libraries এবং অন্যান্য Software যুক্ত করে Ubuntu, Debian, Fedora, Rocky Linux ইত্যাদি Distribution তৈরি করা হয়।

### Kernel কী?
Kernel হলো Operating System-এর Core Component। এটি Hardware এবং Software-এর মধ্যে যোগাযোগ পরিচালনা করে।

### Shell এবং Kernel কি একই জিনিস?
না। Shell User-এর Command গ্রহণ করে এবং Kernel-এর কাছে পাঠায়। Kernel Hardware ও System Resource পরিচালনা করে।

### GNU কি? 
GNU-এর পূর্ণ অর্থ হলো GNU's Not Unix! (জিএনইউ ইজ নট ইউনিক্স!)। এটি একটি বিশেষ ধরনের শব্দ বা রিভার্স এক্রোনিম (Recursive acronym), যেখানে নামের ভেতরেই নিজের নাম রাখা হয়েছে।'

### CLI কী?
CLI (Command Line Interface) হলো Text-Based Interface যেখানে Command লিখে System পরিচালনা করা হয়।

### Bash কী?
Bash (Bourne Again Shell) হলো Linux-এর সবচেয়ে জনপ্রিয় Shell।

### `/` এবং `/root` কি একই?
না। `/` হলো System Root Directory। `/root` হলো Root User-এর Home Directory।

### `~` চিহ্নের অর্থ কী?
`~` বর্তমান Login করা User-এর Home Directory বোঝায়।

### Hidden File কী?
যেসব File বা Directory-এর নাম `.` দিয়ে শুরু হয়, সেগুলো Hidden File হিসেবে বিবেচিত হয়।

### `.` এবং `..` এর অর্থ কী?
- `.` = বর্তমান Directory
- `..` = Parent Directory

### `man` Command কী কাজে লাগে?
`man` Command-এর মাধ্যমে Linux Manual Page পড়া যায়।

### `wget` কী?
`wget` হলো Internet থেকে File Download করার Command-Line Tool।

### Package Manager কী?
Package Install, Update এবং Remove করার Software-কে Package Manager বলা হয়।

উদাহরণ:
- Debian/Ubuntu → apt
- Fedora/RHEL → dnf
- Arch Linux → pacman

### GPG Key কী?
GPG Key ব্যবহার করে Software Repository বা Package-এর Authenticity Verify করা হয়।

### `gpg --dearmor` কী করে?
ASCII Format-এর Public Key-কে Binary Keyring Format-এ রূপান্তর করে।

### `/usr/share/keyrings/` Directory কেন ব্যবহার করা হয়?
Trusted Repository Key সংরক্ষণ করার জন্য ব্যবহৃত হয়।

### `apt update` কী করে?
Repository থেকে Package Index Download ও Refresh করে।

### `apt install` কী করে?
নির্বাচিত Package Download এবং Install করে।

### File Permission কী?
কে কোন File পড়তে, লিখতে বা Execute করতে পারবে তা নির্ধারণ করে File Permission।

### `chmod 755` এর অর্থ কী?
- Owner = rwx (7)
- Group = r-x (5)
- Others = r-x (5)

### Linux Theme পরিবর্তন করলে কি System নষ্ট হয়?
সাধারণত না। Theme পরিবর্তন User Interface-এর Appearance পরিবর্তন করে, Operating System-এর Core Functionality পরিবর্তন করে না।