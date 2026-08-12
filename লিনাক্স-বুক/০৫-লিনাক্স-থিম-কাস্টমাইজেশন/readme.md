# ০৫। লিনাক্স থিম কাস্টমাইজেশন

> Ubuntu 26 (GNOME)-কে Windows 11-এর মতো আধুনিক, সুন্দর এবং প্রোডাক্টিভ ডেস্কটপে রূপান্তর করার পূর্ণাঙ্গ বাংলা গাইড।

## শেখার উদ্দেশ্য

এই অধ্যায় শেষে আপনি জানতে পারবেন:

- GNOME Desktop কী
- Linux Customization কী
- GNOME Tweaks ব্যবহার
- GNOME Extensions ইনস্টল
- Dash to Panel কনফিগার
- ArcMenu দিয়ে Start Menu তৈরি
- Fluent GTK Theme ব্যবহার
- Fluent Icons ও Bibata Cursor ব্যবহার
- Dark Mode ও Blur Effect
- Backup ও Restore
- Ubuntu-কে Windows 11-এর মতো সাজানো

---

# GNOME Desktop কী?

Ubuntu-এর ডিফল্ট Desktop Environment হলো GNOME।

GNOME মিনিমাল, দ্রুত এবং আধুনিক একটি ডেস্কটপ পরিবেশ। Linux-এর সবচেয়ে বড় সুবিধাগুলোর একটি হলো আপনি পুরো Desktop-এর চেহারা ও আচরণ নিজের মতো পরিবর্তন করতে পারেন।

---

# সিস্টেম আপডেট

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
  git curl wget
```

ইনস্টল হওয়ার পর Application Menu-তে GNOME Tweaks এবং Extension Manager দেখতে পাবেন।

---

# GNOME Tweaks

GNOME Tweaks হলো Advanced Configuration Tool।

এখান থেকে:

- Theme
- Icons
- Cursor
- Fonts
- Window Buttons

পরিবর্তন করা যায়।

---

# GNOME Extensions

GNOME Extensions Desktop-এ নতুন Feature যোগ করে।

জনপ্রিয় Extensions:

| Extension | কাজ |
|------------|------|
| Dash to Panel | Windows-style Taskbar |
| ArcMenu | Start Menu |
| User Themes | Custom Themes |
| Blur My Shell | Blur Effects |

---

# Dash to Panel

Windows-এর মতো Taskbar তৈরি করার জন্য সবচেয়ে জনপ্রিয় Extension।

Settings:

- Position → Bottom
- Panel Size → 44px
- Center Applications → Enabled
- Click Action → Minimize

---

# ArcMenu

ArcMenu Windows-এর Start Menu-এর মতো Application Launcher প্রদান করে।

Recommended Layout:

- Windows Modern
- Windows 11

---

# Fluent GTK Theme

Windows 11-এর Fluent Design দ্বারা অনুপ্রাণিত Theme।

```bash
git clone https://github.com/vinceliuice/Fluent-gtk-theme.git
cd Fluent-gtk-theme
./install.sh
```

---

# Fluent Icon Theme

```bash
git clone https://github.com/vinceliuice/Fluent-icon-theme.git
cd Fluent-icon-theme
./install.sh
```

---

# Bibata Cursor

```bash
sudo apt install bibata-cursor-theme
```

---

# Theme Apply করা

GNOME Tweaks → Appearance

- Applications → Fluent
- Icons → Fluent
- Cursor → Bibata
- Shell → Fluent

---

# Window Buttons যোগ করা

GNOME Tweaks → Window Titlebars

Enable:

- Minimize
- Maximize

---

# Blur My Shell

Windows 11-এর মতো Blur Effect পাওয়ার জন্য:

- Blur My Shell Install করুন
- Panel Blur Enable করুন
- Overview Blur Enable করুন

---

# Dark Mode

Settings → Appearance → Dark

---

# Microsoft Fonts

```bash
sudo apt install ttf-mscorefonts-installer
```

---

# Useful Tweaks

Center New Windows:

```bash
gsettings set org.gnome.mutter center-new-windows true
```

---

# Backup ও Restore

Backup:

```bash
dconf dump / > gnome-backup.ini
```

Restore:

```bash
dconf load / < gnome-backup.ini
```

---

# Troubleshooting

## Theme Apply হচ্ছে না

User Themes Extension Enable আছে কিনা পরীক্ষা করুন।

## Dash to Panel দেখা যাচ্ছে না

Extension Manager থেকে Enable করুন।

## ArcMenu কাজ করছে না

```bash
sudo apt install libgnome-menu-3-0 gir1.2-gmenu-3.0
```

---

# অধ্যায়ের সারাংশ

এই অধ্যায়ে আমরা শিখেছি:

- GNOME Customization
- GNOME Tweaks
- Dash to Panel
- ArcMenu
- Fluent Theme
- Fluent Icons
- Bibata Cursor
- Dark Mode
- Backup & Restore

---

# অনুশীলনী

১. GNOME Tweaks কী?

২. Dash to Panel কী কাজ করে?

৩. ArcMenu কেন ব্যবহার করা হয়?

৪. Fluent Theme কী?

৫. GNOME Settings Backup করার Command লিখুন।

---

🏠 [সূচিপত্র](../readme.md)

⬅️ Previous: ০৪। wget-এর ব্যবহার

➡️ Next: Coming Soon