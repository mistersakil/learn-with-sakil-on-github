# 📦 ০৬। থার্ড-পার্টি প্যাকেজ ইনস্টলেশন আর্কিটেকচার

> Ubuntu ও Debian-ভিত্তিক Linux সিস্টেমে Official Repository-এর বাইরে থেকে সফটওয়্যার কীভাবে নিরাপদে ইনস্টল করা হয় তার পূর্ণাঙ্গ আর্কিটেকচার।

## 📑 সূচিপত্র

- Third-Party Package কী?
- Ubuntu Package Management Architecture
- APT কীভাবে কাজ করে?
- GPG Signature কেন প্রয়োজন?
- Repository Architecture
- Installation Lifecycle
- Generic 4-Step Installation Process
- Practical Example (Microsoft Edge)
- Alternative Installation Methods
- Security Best Practices
- Troubleshooting Guide
- Real World Examples
- Summary

---

## 🎯 থার্ড-পার্টি প্যাকেজ কী?

Ubuntu-এর নিজস্ব Repository-এর বাইরে থাকা যেকোনো Software-কে সাধারণত Third-Party Package বলা হয়।

উদাহরণ: Google Chrome, Microsoft Edge, VS Code, Docker, Node.js, GitHub CLI।

---

## 🏗️ Ubuntu Package Management Architecture

```text
Developer Company
       │
       ▼
Official Repository
       │
       ▼
 GPG Key
       │
       ▼
APT Source List
       │
       ▼
apt update
       │
       ▼
Package Metadata
       │
       ▼
apt install
       │
       ▼
Installed Software
```

---

## 🛠️ Generic 4-Step Installation Process

### Step 1 — System Preparation

```bash
sudo apt update
sudo apt install wget curl gpg -y
```

### Step 2 — Import GPG Key

```bash
wget -qO- KEY_URL | sudo gpg --dearmor -o PACKAGE.gpg
```

### Step 3 — Add Repository

```bash
echo "deb [signed-by=PACKAGE.gpg] REPO_URL stable main" | sudo tee FILE.list
```

### Step 4 — Install Package

```bash
sudo apt update
sudo apt install package-name
```

---

## 🌐 Microsoft Edge Example

```bash
curl https://packages.microsoft.com/keys/microsoft.asc \
| gpg --dearmor \
| sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null
```

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft-edge.gpg] https://packages.microsoft.com/repos/edge stable main" \
| sudo tee /etc/apt/sources.list.d/microsoft-edge.list
```

```bash
sudo apt update
sudo apt install microsoft-edge-stable -y
```

---

## 🛡️ Security Best Practices

- Official documentation অনুসরণ করুন
- HTTPS repository ব্যবহার করুন
- GPG key verify করুন
- অপ্রয়োজনীয় repository যোগ করবেন না

---

## 🔗 Navigation

← ০৫। লিনাক্স থিম কাস্টমাইজেশন

↑ সূচিপত্র

→ ০৭। Shell Scripting Fundamentals (Coming Soon)
