> [🏠](../) [⬅️ ০২। শেল এক্সপ্যানশন](../০২-শেল-এক্সপ্যানশন)  [➡️ ০৪। wget-এর ব্যবহার](../০৪-wget-এর-ব্যবহার)

# ০৩। ডেভঅপস ডেভেলপমেন্ট এনভায়রনমেন্ট

এই guide-টি একটি fresh **Ubuntu 26** workstation-কে **Laravel, React/Inertia, Vite, Docker এবং DevOps development**-এর জন্য প্রস্তুত করার জন্য তৈরি।

## প্রস্তাবিত Architecture

মূল ধারণা:

> Developer tools Host machine-এ থাকবে এবং application runtime ও infrastructure services Docker-এর মধ্যে আলাদা থাকবে।

```text
Ubuntu 26
│
├── Host
│   ├── Git
│   ├── SSH
│   ├── VS Code
│   ├── Zsh
│   ├── NVM
│   ├── Node.js
│   ├── npm / pnpm
│   ├── Docker
│   └── Docker Compose
│
└── Docker
    ├── Laravel / PHP-FPM
    ├── Nginx
    ├── MySQL
    ├── Redis
    ├── phpMyAdmin
    ├── Queue Worker
    └── Scheduler
```

---

## ০১ - Ubuntu Update করুন

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

Development ও DevOps-এর প্রয়োজনীয় utility install করুন:

```bash
sudo apt install -y \
    curl \
    wget \
    git \
    unzip \
    zip \
    build-essential \
    ca-certificates \
    gnupg \
    lsb-release \
    software-properties-common \
    apt-transport-https \
    jq \
    rsync \
    htop \
    btop \
    tree \
    net-tools \
    dnsutils \
    ncdu \
    lsof \
    tmux
```

Verify:

```bash
git --version
curl --version
```

---

## ০২ - Git Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --global core.editor "code --wait"
```

Configuration দেখুন:

```bash
git config --global --list
```

দরকারি alias:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.last 'log -1 HEAD'
```

---

## ০৩ - GitHub, GitLab ও Server-এর জন্য SSH

Ed25519 SSH key তৈরি করুন:

```bash
ssh-keygen -t ed25519 -C "your@email.com"
```

SSH agent চালু করে key add করুন:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

Public key দেখুন:

```bash
cat ~/.ssh/id_ed25519.pub
```

GitHub/GitLab-এর SSH Keys section-এ public key add করে GitHub test করুন:

```bash
ssh -T git@github.com
```

SSH directory তৈরি ও permission ঠিক করুন:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

`~/.ssh/config`-এর উদাহরণ:

```text
Host production
    HostName YOUR_SERVER_IP
    User deploy
    IdentityFile ~/.ssh/id_ed25519

Host staging
    HostName YOUR_SERVER_IP
    User deploy
    IdentityFile ~/.ssh/id_ed25519
```

তারপর:

```bash
ssh production
ssh staging
```

---

## ০৪ - Zsh

Install:

```bash
sudo apt install -y zsh
```

Check:

```bash
zsh --version
```

Default shell করুন:

```bash
chsh -s $(which zsh)
```

এরপর logout করে আবার login করুন।

---

## ০৫ - VS Code

Microsoft-এর official APT repository ব্যবহার করে VS Code install করুন:

```bash
sudo install -d -m 0755 /etc/apt/keyrings

wget -qO- https://packages.microsoft.com/keys/microsoft.asc | \
    gpg --dearmor | \
    sudo tee /etc/apt/keyrings/packages.microsoft.gpg > /dev/null

sudo chmod a+r /etc/apt/keyrings/packages.microsoft.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | \
    sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null

sudo apt update
sudo apt install -y code
```

Current project open করতে:

```bash
code .
```

### Recommended VS Code Extension

- PHP Intelephense
- Laravel Extra Intellisense
- Laravel Blade Snippets
- DotENV
- Docker
- ESLint
- Prettier
- Tailwind CSS IntelliSense
- GitLens
- Git Graph
- REST Client
- EditorConfig

---

## ০৬ - NVM ও Node.js

Ubuntu APT repository থেকে সরাসরি Node.js install না করে **NVM** ব্যবহার করুন। এতে project অনুযায়ী একাধিক Node.js version manage করা যায়।

NVM install:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

Shell reload:

```bash
source ~/.zshrc
```

Check:

```bash
nvm --version
```

Current Node.js LTS install:

```bash
nvm install --lts
```

LTS default করুন:

```bash
nvm alias default 'lts/*'
```

Verify:

```bash
node -v
npm -v
```

### Node.js version পরিবর্তন

```bash
nvm install 20
nvm install 22
nvm use 20
nvm use 22
```

Project-specific version-এর জন্য `.nvmrc`:

```text
22
```

তারপর:

```bash
nvm use
```

---

## ০৭ - pnpm

Corepack enable করুন:

```bash
corepack enable
```

pnpm activate করুন:

```bash
corepack prepare pnpm@latest --activate
```

Verify:

```bash
pnpm --version
```

একটি project যে package manager ব্যবহার করে সেটিই follow করুন। একই project-এ অকারণে `npm`, `yarn` ও `pnpm` mix করবেন না।

---

## ০৮ - Docker

আগে থেকে conflicting package থাকলে remove করুন:

```bash
sudo apt remove -y \
    docker.io \
    docker-doc \
    docker-compose \
    docker-compose-v2 \
    podman-docker \
    containerd \
    runc
```

Docker repository যোগ করুন:

```bash
sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
    -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo \"${UBUNTU_CODENAME:-$VERSION_CODENAME}\") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Docker Engine ও Compose install:

```bash
sudo apt update
sudo apt install -y \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin
```

Verify:

```bash
docker --version
docker compose version
```

---

## ০৯ - `sudo` ছাড়া Docker চালানো

বর্তমান user-কে Docker group-এ add করুন:

```bash
sudo usermod -aG docker $USER
```

Logout করে আবার login করুন। তারপর:

```bash
docker run hello-world
```

`sudo` ছাড়া কাজ করলে setup সম্পন্ন।

---

## ১০ - Docker Storage Management

**512GB SSD**-তে Docker image, build cache ও volume দ্রুত storage দখল করতে পারে।

Docker disk usage:

```bash
docker system df
```

Docker storage দেখুন:

```bash
sudo du -sh /var/lib/docker
```

Unused resource clean করতে:

```bash
docker system prune
```

সতর্কতার সঙ্গে ব্যবহার করুন:

```bash
docker system prune -a
```

এটি এমন unused image-ও remove করতে পারে যা পরে development-এ দরকার হতে পারে।

Overall disk usage:

```bash
df -h
ncdu /
```

---

## ১১ - Laravel Environment Strategy

একাধিক Laravel project থাকলে একটি global PHP version-এর ওপর নির্ভর না করাই ভালো।

### Host-এ রাখুন

```text
Git
SSH
VS Code
Zsh
NVM
Node.js
npm / pnpm
Docker
Docker Compose
```

### Docker-এ রাখুন

```text
PHP / PHP-FPM
Composer
Laravel runtime
Nginx
MySQL
Redis
phpMyAdmin
Queue Worker
Scheduler
```

এতে project অনুযায়ী আলাদা PHP version ব্যবহার করা যায়।

উদাহরণ:

```text
Laravel project A → PHP 8.2
Laravel project B → PHP 8.3
Laravel project C → PHP 8.4
```

---

## ১২ - Recommended Laravel Project Structure

```text
~/Projects/
│
├── crm/
├── ecommerce/
└── other-project/
```

তৈরি করুন:

```bash
mkdir -p ~/Projects
cd ~/Projects
```

Dockerized Laravel project-এর structure:

```text
project/
├── app/
├── bootstrap/
├── config/
├── database/
├── docker/
├── public/
├── resources/
├── routes/
├── storage/
├── Dockerfile
├── compose.yaml
└── .env
```

---

## ১৩ - Docker-এর মাধ্যমে MySQL

Project Docker-based হলে Host-এ global MySQL install করার প্রয়োজন নেই।

Example Compose service:

```yaml
services:
  mysql:
    image: mysql:8.4
    environment:
      MYSQL_DATABASE: laravel
      MYSQL_USER: laravel
      MYSQL_PASSWORD: secret
      MYSQL_ROOT_PASSWORD: root
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

Start:

```bash
docker compose up -d
```

Check:

```bash
docker ps
```

Docker-এর ভেতরে Laravel থেকে MySQL connect করার সময় service name ব্যবহার করুন:

```env
DB_HOST=mysql
DB_PORT=3306
```

এটি নয়:

```env
DB_HOST=127.0.0.1
```

কারণ container-এর `127.0.0.1` সেই container-কেই নির্দেশ করে।

---

## ১৪ - Docker-এর মাধ্যমে Redis

Laravel cache, session ও queue-এর জন্য Redis ব্যবহার করা যায়।

```yaml
services:
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
```

Laravel `.env`:

```env
CACHE_STORE=redis
SESSION_DRIVER=redis
QUEUE_CONNECTION=redis
REDIS_HOST=redis
```

এখানে `redis` হলো Docker Compose service name।

---

## ১৫ - Docker-এর মাধ্যমে phpMyAdmin

Local development-এর জন্য:

```yaml
services:
  phpmyadmin:
    image: phpmyadmin:latest
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
    ports:
      - "8080:80"
    depends_on:
      - mysql
```

Browser:

```text
http://localhost:8080
```

> Production server-এ phpMyAdmin public internet-এ expose করবেন না, যদি না যথেষ্ট secured configuration থাকে।

---

## ১৬ - Laravel Queue Worker

Production-এর কাছাকাছি local architecture-এর জন্য Queue Worker-কে web container থেকে আলাদা রাখা ভালো।

```text
Laravel App
    │
    ▼
  Redis
    │
    ▼
 Queue
    │
    ▼
Queue Worker
```

Compose stack:

```text
app
nginx
mysql
redis
queue
scheduler
```

এতে local environment-এ production queue behaviour reproduce করা সহজ হয়।

---

## ১৭ - Laravel Scheduler

Scheduled job test করার জন্য আলাদা scheduler service ব্যবহার করতে পারেন:

```bash
php artisan schedule:work
```

---

## ১৮ - Nginx + PHP-FPM

Recommended request flow:

```text
Browser
   │
   ▼
Nginx :80
   │
   ▼
PHP-FPM :9000
   │
   ▼
Laravel
   │
   ├── MySQL
   └── Redis
```

Docker-first workflow হলে Host-এ global Nginx install না করে project-এর Docker stack-এর ভেতরে Nginx রাখুন।

---

## ১৯ - React / Inertia / Vite

React, Inertia ও Vite project-এর জন্য:

```bash
npm install
npm run dev
```

অথবা:

```bash
pnpm install
pnpm dev
```

Vite সাধারণত:

```text
http://localhost:5173
```

Docker-এর ভেতরে Vite চালালে:

```js
server: {
    host: '0.0.0.0',
}
```

Developer workstation-এর জন্য Node.js/NVM Host-এ রাখলে Vite hot reload ও project-specific Node version management সহজ হয়।

---

## ২০ - DevOps Utilities

```bash
sudo apt install -y \
    openssh-client \
    openssh-server \
    rsync \
    jq \
    tree \
    htop \
    btop \
    tmux \
    net-tools \
    dnsutils \
    ncdu \
    lsof \
    strace
```

দরকারি command:

```bash
htop
btop
ss -tulpn
df -h
free -h
du -sh *
lsof -i :3306
```

---

## ২১ - UFW Firewall

Status দেখুন:

```bash
sudo ufw status
```

SSH ব্যবহার করলে firewall enable করার আগে SSH allow করুন:

```bash
sudo ufw allow ssh
sudo ufw enable
```

Check:

```bash
sudo ufw status verbose
```

Development laptop-এ শুধুমাত্র প্রয়োজনীয় port open রাখুন।

---

## ২২ - দরকারি Zsh Alias

`~/.zshrc` খুলুন:

```bash
nano ~/.zshrc
```

```bash
alias ll='ls -lah'
alias la='ls -A'

alias dc='docker compose'
alias dps='docker ps'
alias dpa='docker ps -a'
alias dlogs='docker compose logs -f'
alias dup='docker compose up -d'
alias ddown='docker compose down'

alias art='php artisan'

alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git pull'
```

Reload:

```bash
source ~/.zshrc
```

---

## ২৩ - সাধারণ Docker Laravel Workflow

Project start:

```bash
docker compose up -d
```

Service status:

```bash
docker compose ps
```

Live log:

```bash
docker compose logs -f
```

Application container-এ প্রবেশ:

```bash
docker compose exec app bash
```

Laravel command:

```bash
docker compose exec app php artisan migrate
docker compose exec app php artisan optimize:clear
```

Composer dependency:

```bash
docker compose exec app composer install
```

Node container থাকলে:

```bash
docker compose exec node npm install
```

Project বন্ধ:

```bash
docker compose down
```

---

## ২৪ - Host বনাম Docker Final Checklist

| Component | কোথায় রাখা ভালো |
| --- | --- |
| Ubuntu | Host |
| Git | Host |
| SSH | Host |
| VS Code | Host |
| Zsh | Host |
| NVM | Host |
| Node.js | Host |
| npm / pnpm | Host |
| Docker | Host |
| Docker Compose | Host |
| PHP | Docker |
| Composer | Docker |
| Laravel runtime | Docker |
| Nginx | Docker |
| MySQL | Docker |
| Redis | Docker |
| phpMyAdmin | Docker |
| Queue Worker | Docker |
| Scheduler | Docker |

---

## ২৫ - Recommended Development Flow

```text
Ubuntu 26
    │
    ├── Git / SSH
    │
    ├── VS Code
    │
    ├── NVM
    │    └── Node.js / npm / pnpm / Vite
    │
    └── Docker
         │
         ├── Nginx
         ├── PHP-FPM + Laravel
         ├── MySQL
         ├── Redis
         ├── Queue Worker
         ├── Scheduler
         └── phpMyAdmin
```

## মূল নীতি

> **Host = Developer Tooling**
>
> **Docker = Application Runtime + Infrastructure Services**

এই architecture ব্যবহার করলে একাধিক Laravel project-এর environment আলাদা রাখা যায়, PHP/database version conflict কমে এবং local development environment production-এর DevOps architecture-এর কাছাকাছি থাকে।

> [🏠](../) [⬅️ ০২। শেল এক্সপ্যানশন](../০২-শেল-এক্সপ্যানশন)  [➡️ ০৪। wget-এর ব্যবহার](../০৪-wget-এর-ব্যবহার)