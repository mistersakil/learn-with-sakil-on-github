> [🏠](../) [⬅️ ০৪। wget-এর ব্যবহার](../০৪-wget-এর-ব্যবহার) [➡️ ০৬। থার্ড পার্টি প্যাকেজ ইনস্টলেশন আর্কিটেকচার](../০৬-থার্ড-পার্টি-প্যাকেজ-ইনস্টলেশন-আর্কিটেকচার)

# ০৫। লিনাক্স থিম কাস্টমাইজেশন

> Ubuntu 26 (GNOME)-কে Windows 11-এর মতো আধুনিক, সুন্দর এবং প্রোডাক্টিভ ডেস্কটপে রূপান্তর করার পূর্ণাঙ্গ বাংলা গাইড।

---

## শেখার উদ্দেশ্য

এই অধ্যায় শেষে আপনি জানতে পারবেন:

- GNOME Desktop কী এবং কেন জনপ্রিয়
- Linux Customization-এর মৌলিক ধারণা
- Theme, Icon, Cursor এবং Font কীভাবে কাজ করে
- GNOME Tweaks ব্যবহার
- GNOME Extensions ইনস্টল ও কনফিগার
- Dash to Panel দিয়ে Windows-Style Taskbar তৈরি
- ArcMenu দিয়ে Start Menu তৈরি
- Fluent GTK Theme ব্যবহার
- Fluent Icons ও Bibata Cursor ব্যবহার
- Dark Mode ও Blur Effect ব্যবহার
- Backup ও Restore
- Ubuntu-কে Windows 11-এর মতো সাজানো

---

# ভূমিকা

Windows ব্যবহারকারীরা যখন Ubuntu-তে আসেন, তখন প্রথম যে বিষয়টি চোখে পড়ে তা হলো Desktop Layout। GNOME-এর Default Interface Windows-এর তুলনায় ভিন্ন।

সুখবর হলো Linux-এ Desktop Customization-এর প্রায় কোনো সীমা নেই। আপনি চাইলে Ubuntu-কে Windows 11, macOS অথবা সম্পূর্ণ নিজস্ব ডিজাইনে রূপান্তর করতে পারেন।

এই অধ্যায়ে আমরা Ubuntu 26 GNOME Desktop-কে Windows 11-এর মতো আধুনিক রূপ দেওয়ার পুরো প্রক্রিয়া শিখব।

---

# GNOME Desktop কী?

GNOME হলো Ubuntu-এর ডিফল্ট Desktop Environment।

GNOME-এর বৈশিষ্ট্য:

- সহজ ও পরিচ্ছন্ন Interface
- কম বিভ্রান্তিকর Workflow
- Keyboard Friendly Navigation
- Extension Support
- Modern Design

GNOME-এর শক্তি হলো Extensions এবং Themes।

---

# Linux Customization কী?

Customization বলতে Desktop-এর চেহারা এবং আচরণ পরিবর্তন করাকে বোঝায়।

উদাহরণ:

- Theme পরিবর্তন
- Icon পরিবর্তন
- Cursor পরিবর্তন
- Taskbar পরিবর্তন
- Start Menu যোগ করা
- Blur Effect
- Dark Mode
- Font পরিবর্তন

---

# সিস্টেম আপডেট

Customization শুরু করার আগে সিস্টেম আপডেট করা ভালো অভ্যাস।

```bash
sudo apt update
sudo apt upgrade -y
```

---

# প্রয়োজনীয় টুল ইনস্টল

```bash
sudo apt install -y \
gnome-tweaks \
gnome-shell-extension-manager \
git \
curl \
wget
```

ইনস্টল হওয়ার পরে Application Menu-তে পাবেন:

- GNOME Tweaks
- Extension Manager

---

# GNOME Tweaks

GNOME Tweaks হলো Advanced Desktop Configuration Tool।

এখান থেকে আপনি:

- Theme
- Icons
- Cursor
- Fonts
- Window Buttons
- Startup Applications

নিয়ন্ত্রণ করতে পারবেন।

---

# GNOME Extensions কী?

GNOME Extensions Desktop Environment-এর ক্ষমতা বৃদ্ধি করে।

Browser Extension যেমন Browser-এ Feature যোগ করে, GNOME Extension তেমনি Desktop-এ Feature যোগ করে।

### জনপ্রিয় Extensions

| Extension | কাজ |
|-----------|------|
| Dash to Panel | Windows-style Taskbar |
| ArcMenu | Start Menu |
| User Themes | Custom Shell Theme |
| Blur My Shell | Blur Effect |
| Clipboard Indicator | Clipboard History |

---

# Extension Manager ব্যবহার

Application Menu → Extension Manager

তারপর:

Browse → Search

এখান থেকে Extension Install করা যায়।

---

# Dash to Panel

Dash to Panel Ubuntu Dock এবং Top Bar-কে একত্র করে Windows-এর মতো Taskbar তৈরি করে।

### Install

Extension Manager-এ Search করুন:

```text
Dash to Panel
```

Install করুন।

### Recommended Settings

Position:

```text
Bottom
```

Panel Size:

```text
44px
```

Enable:

```text
Center Running Applications
```

Click Action:

```text
Minimize
```

---

# ArcMenu

ArcMenu GNOME-এর জন্য একটি শক্তিশালী Application Launcher।

### Install

Search করুন:

```text
ArcMenu
```

### Recommended Layout

```text
Windows Modern
```

অথবা

```text
Windows 11
```

---

# Dash to Panel + ArcMenu

Windows 11 Experience তৈরির জন্য সবচেয়ে জনপ্রিয় Combination:

```text
Dash to Panel + ArcMenu
```

এই দুই Extension ব্যবহার করলে Desktop Layout অনেকটাই Windows-এর মতো হয়ে যায়।

---

# User Themes Extension

GNOME Shell Theme ব্যবহার করতে হলে User Themes Extension Enable করতে হবে।

Install:

```text
User Themes
```

---

# Fluent GTK Theme

Windows 11-এর Fluent Design দ্বারা অনুপ্রাণিত GTK Theme।

### Download ও Install

```bash
mkdir -p ~/.themes

cd /tmp

git clone https://github.com/vinceliuice/Fluent-gtk-theme.git

cd Fluent-gtk-theme

./install.sh
```

---

# Fluent Icon Theme

Theme-এর সাথে Matching Icon Theme ব্যবহার করলে Desktop আরও সুন্দর দেখায়।

```bash
mkdir -p ~/.icons

cd /tmp

git clone https://github.com/vinceliuice/Fluent-icon-theme.git

cd Fluent-icon-theme

./install.sh
```

---

# Bibata Cursor Theme

Bibata বর্তমানে Linux Community-তে অন্যতম জনপ্রিয় Cursor Theme।

```bash
sudo apt install bibata-cursor-theme
```

---

# Theme Apply করা

GNOME Tweaks → Appearance

Applications:

```text
Fluent
```

Icons:

```text
Fluent
```

Cursor:

```text
Bibata
```

Shell:

```text
Fluent
```

---

# Window Buttons যোগ করা

GNOME Default-এ Minimize Button থাকে না।

GNOME Tweaks → Window Titlebars

Enable:

- Minimize
- Maximize

এতে Windows-এর মতো Window Controls পাওয়া যাবে।

---

# Blur My Shell

Windows 11-এর Blur Effect পেতে Install করুন:

```text
Blur My Shell
```

Enable করুন:

- Overview Blur
- Panel Blur
- Menu Blur

---

# Wallpaper নির্বাচন

একটি সুন্দর Wallpaper পুরো Desktop-এর চেহারা বদলে দিতে পারে।

Wallpaper বাছাইয়ের ক্ষেত্রে:

- High Resolution ব্যবহার করুন
- Dark Theme হলে Dark Wallpaper ব্যবহার করুন
- Minimal Wallpaper বেশি Professional দেখায়

---

# Dark Mode

Settings → Appearance → Dark

Dark Mode দীর্ঘ সময় কাজ করার সময় চোখের চাপ কমাতে সাহায্য করে।

---

# Microsoft Fonts

Windows-এর মতো Typography চাইলে:

```bash
sudo apt install ttf-mscorefonts-installer
```

---

# Productivity Tweaks

### Center New Windows

```bash
gsettings set org.gnome.mutter center-new-windows true
```

### Enable Night Light

Settings → Display → Night Light

### Workspace ব্যবহার

একাধিক Project নিয়ে কাজ করলে GNOME Workspaces ব্যবহার করুন।

---

# Backup ও Restore

Customization করার পরে Backup রাখা অত্যন্ত গুরুত্বপূর্ণ।

### Backup

```bash
dconf dump / > gnome-backup.ini
```

### Restore

```bash
dconf load / < gnome-backup.ini
```

---

# Troubleshooting

## Theme Apply হচ্ছে না

সমাধান:

- User Themes Enable করুন
- GNOME Restart করুন

## Dash to Panel দেখা যাচ্ছে না

সমাধান:

- Extension Manager খুলুন
- Extension Enable করুন

## ArcMenu কাজ করছে না

```bash
sudo apt install libgnome-menu-3-0 gir1.2-gmenu-3.0
```

## Theme ভেঙে গেছে

Backup Restore করুন অথবা Default Theme-এ ফিরে যান।

---

# Best Practices

- একসাথে অনেক Extension ব্যবহার করবেন না
- Theme পরিবর্তনের আগে Backup নিন
- Unknown Source থেকে Theme Install করবেন না
- System Upgrade-এর আগে Extensions Compatibility পরীক্ষা করুন

---

# অধ্যায়ের সারাংশ

এই অধ্যায়ে আমরা শিখেছি:

- GNOME Desktop
- Linux Customization
- GNOME Tweaks
- GNOME Extensions
- Dash to Panel
- ArcMenu
- Fluent GTK Theme
- Fluent Icons
- Bibata Cursor
- Dark Mode
- Blur My Shell
- Backup ও Restore
- Troubleshooting

এখন আপনি Ubuntu Desktop-কে নিজের পছন্দমতো সাজাতে পারবেন এবং Windows 11-এর কাছাকাছি অভিজ্ঞতা তৈরি করতে পারবেন।

---

# অনুশীলনী

১. GNOME Desktop Environment কী?

২. GNOME Tweaks-এর কাজ কী?

৩. Dash to Panel এবং ArcMenu-এর মধ্যে পার্থক্য কী?

৪. Theme, Icon এবং Cursor-এর ভূমিকা ব্যাখ্যা করুন।

৫. GNOME Settings Backup করার Command লিখুন।

৬. User Themes Extension কেন প্রয়োজন?

৭. Ubuntu-কে Windows 11-এর মতো করতে কোন কোন Component ব্যবহার করা হয়েছে?

> [🏠](../) [⬅️ ০৪। wget-এর ব্যবহার](../০৪-wget-এর-ব্যবহার) [➡️ ০৬। থার্ড পার্টি প্যাকেজ ইনস্টলেশন আর্কিটেকচার](../০৬-থার্ড-পার্টি-প্যাকেজ-ইনস্টলেশন-আর্কিটেকচার)
