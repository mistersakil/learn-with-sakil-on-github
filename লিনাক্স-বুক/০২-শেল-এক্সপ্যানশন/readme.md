> [🏠](../) [⬅️ ০১.১। ইউজার ম্যানেজমেন্ট](../০১.১-ইউজার-ম্যানেজমেন্ট) [➡️ ০৩। ডেভঅপস ডেভেলপমেন্ট এনভায়রনমেন্ট](../০৩-ডেভঅপস-ডেভেলপমেন্ট-এনভায়রনমেন্ট)

# ০২। শেল এক্সপ্যানশন

Linux shell expansion (যাকে shell expansion বা parameter expansion-ও বলা হয়) হলো Bash-এর মতো shell-এর একটি গুরুত্বপূর্ণ feature। Command execute হওয়ার আগে shell আপনার লেখা input-কে বিভিন্ন variable, pattern ও expression অনুযায়ী expand এবং interpret করে।

সহজভাবে ভাবুন: **shell command execute করার আগে একটি preprocessing step হিসেবে command-টিকে পরিবর্তন করে পূর্ণ value তৈরি করে।**

উদাহরণ:

```bash
echo i love linux
```

Output:

```text
i love linux
```

Single quote-এর ভেতরের whitespace সাধারণত preserved থাকে:

```bash
echo 'i -----  love -----   linux'
```

Double quote-এর ভেতরে `-----` নিজে whitespace নয়; তাই output-এ সেটি থাকবে।

> Shell expansion বোঝার সময় quote, variable, command substitution এবং word splitting-এর আচরণ আলাদা করে মনে রাখা গুরুত্বপূর্ণ।

## Shell Expansion-এর ক্রম

Shell expansion নির্দিষ্ট একটি sequence অনুসরণ করে:

1. Brace expansion
2. Tilde expansion
3. Parameter ও variable expansion
4. Command substitution
5. Arithmetic expansion
6. Word splitting
7. Filename বা glob expansion

### ১. Brace (`{}`) Expansion

Pattern থেকে একাধিক string তৈরি করতে ব্যবহৃত হয়।

#### নির্বাচিত value

```bash
echo file{1,3,5}.txt
```

Output:

```text
file1.txt file3.txt file5.txt
```

#### Range expansion

```bash
echo file{1..3}.txt
```

Output:

```text
file1.txt file2.txt file3.txt
```

আরেকটি উদাহরণ:

```bash
echo {a..e}
```

Output:

```text
a b c d e
```

### ২. Tilde (`~`) Expansion

`~` সাধারণত বর্তমান user-এর home directory বোঝায়।

```bash
cd ~
```

উদাহরণস্বরূপ `/home/sakil`-এ expand হতে পারে।

```bash
cd ~root
```

Root user-এর home directory-তে নিয়ে যায়।

---

### ৩. Parameter / Variable (`$`) Expansion

ধরা যাক:

```bash
name=Sakil
```

তাহলে:

```bash
echo $name
echo "$name"
echo "${name}"
```

তিনটির output হবে:

```text
Sakil
Sakil
Sakil
```

কিন্তু:

```bash
echo '$name'
```

Output হবে:

```text
$name
```

কারণ single quote-এর ভেতরের `$name` expand হয় না।

Variable-এর value-এর length দেখতে:

```bash
echo ${#name}
```

### ৪. Command Substitution — `$( )`

একটি command execute করে তার output-কে অন্য command-এর অংশ হিসেবে ব্যবহার করা যায়।

```bash
echo "Today is $(date)"
```

এখানে `$(date)` বর্তমান date/time-এর output দিয়ে replace হবে।

### ৫. Arithmetic Expansion — `$(( ))`

Shell-এর ভেতরে arithmetic calculation করা যায়:

```bash
echo $((5 + 3))
```

Output:

```text
8
```

Variable দিয়েও করা যায়:

```bash
a=10
b=5

echo $((a + b))
```

Output:

```text
15
```

### ৬. Filename / Glob Expansion

File pattern ব্যবহার করে matching file খুঁজে command-এ পাঠানো যায়।

```bash
ls *.txt
```

বর্তমান directory-এর সব `.txt` file দেখাবে।

```bash
ls *.docx
```

সব `.docx` file দেখাবে।

### গুরুত্বপূর্ণ Pattern

| Pattern | অর্থ |
| --- | --- |
| `*` | যেকোনো সংখ্যক character match করে |
| `?` | একটি character match করে |
| `[abc]` | `a`, `b` অথবা `c`-এর যেকোনো একটি match করে |

### Combined Example

```bash
echo file_{1..3}_$(date +%Y).txt
```

ধাপে ধাপে:

```text
{1..3}       → 1 2 3
$(date +%Y)  → বর্তমান বছর
```

উদাহরণ output:

```text
file_1_2026.txt file_2_2026.txt file_3_2026.txt
```
---

## Linux-এ Command-এর ধরন

Linux shell, বিশেষ করে Bash, আপনি যে command লিখছেন সেটিকে বিভিন্ন category অনুযায়ী শনাক্ত ও resolve করে। এটি গুরুত্বপূর্ণ, কারণ shell নির্দিষ্ট priority অনুযায়ী command খুঁজে execute করে।

### ১. Built-in Command

এগুলো shell-এর ভেতরেই implement করা থাকে; আলাদা external binary প্রয়োজন হয় না।

#### বৈশিষ্ট্য

- সাধারণত দ্রুত, কারণ আলাদা process চালানোর প্রয়োজন নেই।
- Shell-এর মধ্যে সবসময় available থাকে।
- Shell-এর state পরিবর্তন করতে পারে; যেমন `cd`, `export`।

`type` দিয়ে command-এর ধরন দেখা যায়:

```bash
type cd
type type
type echo
```

উদাহরণ:

```text
cd is a shell builtin
type is a shell builtin
echo is a shell builtin
```

সব builtin সম্পর্কে দেখতে:

```bash
help
```

### ২. External Command / Binary Executable

এগুলো disk-এ থাকা actual executable program। সাধারণত নিচের directory-গুলোতে পাওয়া যায়:

```text
/bin
/usr/bin
/usr/local/bin
```

উদাহরণ:

```bash
type ls
whereis ls
```

`ls` distribution অনুযায়ী external command হতে পারে এবং alias-ও হতে পারে।

### ৩. Shell Function

User-defined reusable command block।

```bash
myFunc() {
  git pull
  npm install
}
```

তারপর:

```bash
myFunc
```

### বৈশিষ্ট্য

- Shell memory-তে থাকে।
- Automation-এর জন্য useful।
- একই নামে external command থাকলে function সেটিকে override করতে পারে।

### ৪. Alias

দীর্ঘ command-এর shortcut হিসেবে alias ব্যবহার করা হয়।

```bash
alias myList="ls -la"
```

এখন:

```bash
myList
```

`alias` command দিয়ে সব alias দেখা যায়:

```bash
alias
```

#### বৈশিষ্ট্য

- মূলত text substitution।
- Simple shortcut-এর জন্য ভালো।
- Complex logic-এর জন্য function ব্যবহার করা ভালো।

### ৫. Keywords / Reserved Words

Shell parser-এর বিশেষ syntax element।

উদাহরণ:

```text
if
then
fi
for
while
case
```

এগুলো সাধারণ command নয়; standalone executable হিসেবে চালানো যায় না।

---

## Command Resolution-এর ধারণা

আপনি command লিখলে shell সাধারণভাবে alias, function, builtin এবং `PATH`-এর external command-এর মধ্যে খুঁজে সঠিক command resolve করে।

Command-এর ধরন জানতে সবচেয়ে সহজ:

```bash
type ls
type cd
type ll
```

উদাহরণ:

```text
ls is /usr/bin/ls
cd is a shell builtin
ll is aliased to 'ls -la'
```

---

## Shell Variable-এর ধরন

Shell variable হলো shell-এর পরিচালিত named storage location, যেখানে string বা অন্যান্য value রাখা যায়। এটি scripting, command execution এবং environment configuration-এ অত্যন্ত গুরুত্বপূর্ণ।

### ১. Local Variable

বর্তমান shell session-এর মধ্যে define করা variable।

```bash
name="Sakil"
echo $name
```

Output:

```text
Sakil
```

#### বৈশিষ্ট্য

- Scope → বর্তমান shell
- Child process সাধারণত এটি inherit করে না।
- Script-এর logic-এ ব্যবহার করা হয়।

### ২. Environment Variable

`export` করা variable child process-এ inherit হয়।

```bash
export APP_ENV=production
```

তারপর:

```bash
echo $APP_ENV
```

Output:

```text
production
```

Environment variable application ও system tool-এর configuration-এ ব্যাপকভাবে ব্যবহৃত হয়।

### ৩. Built-in / Special Shell Variable

Shell নিজে কিছু variable automatically maintain করে।

| Variable | অর্থ |
| --- | --- |
| `$HOME` | বর্তমান user-এর home directory |
| `$USER` | বর্তমান username |
| `$PWD` | বর্তমান working directory |
| `$SHELL` | বর্তমান shell-এর path |
| `$?` | সর্বশেষ command-এর exit status; `0` সাধারণত success |
| `$$` | বর্তমান shell process-এর PID |


### ৪. Positional Parameter

Shell script-এ argument গ্রহণ করার জন্য ব্যবহৃত হয়।

```bash
$0  # script-এর নাম
$1  # প্রথম argument
$2  # দ্বিতীয় argument
$@  # সব argument
$#  # argument-এর সংখ্যা
```

উদাহরণ:

```bash
echo "First arg: $1"
```

### ৫. Readonly Variable

একবার set করার পর পরিবর্তন করা যায় না।

```bash
readonly PI=3.14
```

### দরকারি command

```bash
set
```

সব shell variable ও function-এর তথ্য দেখায়।

```bash
env
```

Environment variable দেখায়।

```bash
unset varName
```

Variable remove করে।

### সংক্ষেপে

```text
Local variable        → বর্তমান shell-এর জন্য
Environment variable  → child process-এর সঙ্গে share/inherit হয়
Special variable      → shell/system-এর গুরুত্বপূর্ণ context
```

---

## Variable / Identifier-এর Naming Rules

Linux shell variable-এর নাম লেখার সময়:

- Letter, number এবং underscore (`_`) ব্যবহার করা যায়।
- নামের শুরুতে number ব্যবহার করা যায় না।
- `=`-এর দুই পাশে space দেওয়া যাবে না।
- `-`, `@`, `#`-এর মতো special character ব্যবহার করা যাবে না।
- Variable case-sensitive; `VAR` এবং `var` আলাদা।

### Valid Example

```bash
user_name="sakil"
APP_ENV=production
_count=10
export userName="sakil"
```

### Invalid Example

```bash
user-name="sakil"
1name="sakil"
name = "sakil"
```

Variable ব্যবহার:

```bash
echo $user_name
echo $APP_ENV
echo $_count
```

---

## Built-in `$PATH` Variable

### ১. `$PATH` দেখা

```bash
echo $PATH
```

উদাহরণ output:

```text
/root/.local/bin:/root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
```

এখানে:

- `echo` → terminal-এ output দেখায়।
- `$PATH` → PATH variable-এর value expand করে।

### ২. PATH কী?

`PATH` হলো colon-separated directory-এর একটি তালিকা, যেখানে shell কোনো command চালানোর সময় executable খুঁজে দেখে।

যেমন আপনি লিখলেন:

```bash
ls
```

Shell `ls` কোথায় আছে তা আগে থেকেই magical ভাবে জানে না। বরং `PATH`-এর directory-গুলো একে একে search করে executable খুঁজে বের করে।

### ৩. PATH-এর প্রতিটি অংশ

উদাহরণ:

```text
/root/.local/bin:/root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
```

| Directory | উদ্দেশ্য |
| --- | --- |
| `/root/.local/bin` | root user-এর locally installed executable |
| `/root/bin` | root user-এর custom script/binary |
| `/usr/local/sbin` | locally installed system administration binary |
| `/usr/local/bin` | manually installed user executable |
| `/usr/sbin` | system administration command |
| `/usr/bin` | সাধারণ system command; যেমন `ls`, `cp`, `cat` |

### ৪. Shell কীভাবে PATH ব্যবহার করে?

আপনি লিখলে:

```bash
php
```

Shell PATH-এর directory-গুলোতে `php` খুঁজবে, যেমন:

```text
/root/.local/bin/php
/root/bin/php
/usr/local/sbin/php
/usr/local/bin/php
/usr/sbin/php
/usr/bin/php
```

যে matching executable প্রথমে পাওয়া যায়, সাধারণত সেটিই ব্যবহার করা হয়।

### ৫. দ্রুত Debug Tip

কোন executable ব্যবহার হচ্ছে তা দেখতে:

```bash
which php
type php
```

---

## System Admin Binary ও System Administration Command-এর পার্থক্য

দুটো বিষয় দেখতে একই রকম হলেও মূল পার্থক্য হলো এগুলো কোথা থেকে install ও manage করা হয়েছে।

### ১. System Administration Command → `/usr/sbin`

এগুলো Linux distribution-এর package manager দ্বারা install করা OS-provided administration tool।

#### বৈশিষ্ট্য

- OS বা `apt`, `yum`, `dnf`-এর মতো package manager দিয়ে install হয়।
- System package manager দ্বারা managed হয়।
- সাধারণত distribution-এর official ও maintained tool।
- অনেক command চালাতে root privilege প্রয়োজন হতে পারে।

উদাহরণ:

```text
iptables
fdisk
useradd
reboot
```

### ২. Locally Installed System Admin Binary → `/usr/local/sbin`

System administrator নিজে manually install করা administration tool সাধারণত এখানে রাখা হয়।

#### বৈশিষ্ট্য

- OS package manager-এর মাধ্যমে managed নাও হতে পারে।
- Source থেকে `make install` বা custom installer দিয়ে install করা হতে পারে।
- Custom setup বা distribution-এর package-এর চেয়ে নতুন version ব্যবহারের জন্য রাখা যায়।
- সাধারণত administrative privilege প্রয়োজন হতে পারে।

উদাহরণ:

```text
Custom compiled Nginx
Manually installed PHP
Internal administration script
```

### ৩. মূল পার্থক্য

| বিষয় | `/usr/sbin` | `/usr/local/sbin` |
| --- | --- | --- |
| Source | OS / package manager | Manual installation / administrator |
| Ownership | Distribution-controlled | User/admin-controlled |
| Update | Package manager-এর মাধ্যমে | সাধারণত manual |
| Stability | Distribution-এর managed version | Configuration-এর ওপর নির্ভরশীল |
| Use case | Default system administration tool | Custom বা overridden tool |

### সহজভাবে মনে রাখুন

```text
/usr/sbin
→ Distribution-এর official system administration command

/usr/local/sbin
→ Administrator-এর manually installed/custom system administration binary
```

> [🏠](../) [⬅️ ০১.১। ইউজার ম্যানেজমেন্ট](../০১.১-ইউজার-ম্যানেজমেন্ট) [➡️ ০৩। ডেভঅপস ডেভেলপমেন্ট এনভায়রনমেন্ট](../০৩-ডেভঅপস-ডেভেলপমেন্ট-এনভায়রনমেন্ট)