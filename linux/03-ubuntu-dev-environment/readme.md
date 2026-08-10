# Ubuntu 26 Development Environment

## Laravel + React + Docker + DevOps

This guide is for setting up a fresh **Ubuntu 26** workstation for Laravel, React/Inertia, Vite, Docker and DevOps development.

### Recommended Architecture

The main principle is:

> Keep developer tools on the host and keep application services isolated inside Docker.

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

## 01 - Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

Install common development and DevOps utilities:

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

## 02 - Git Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --global core.editor "code --wait"
```

Check:

```bash
git config --global --list
```

Useful aliases:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.last 'log -1 HEAD'
```

---

## 03 - SSH for GitHub, GitLab and Servers

Generate an Ed25519 key:

```bash
ssh-keygen -t ed25519 -C "your@email.com"
```

Start the SSH agent and add the key:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

Show the public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Add the public key to GitHub/GitLab and test GitHub access:

```bash
ssh -T git@github.com
```

Create the SSH directory with safe permissions:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Example `~/.ssh/config`:

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

Then connect using:

```bash
ssh production
ssh staging
```

---

## 04 - Zsh

Install Zsh:

```bash
sudo apt install -y zsh
```

Check:

```bash
zsh --version
```

Make Zsh the default shell:

```bash
chsh -s $(which zsh)
```

Log out and log back in after changing the shell.

---

## 05 - VS Code

Install VS Code using Microsoft's official APT repository.

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

Open the current project:

```bash
code .
```

### Recommended VS Code Extensions

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

## 06 - NVM and Node.js

Use **NVM** instead of installing Node.js directly from the Ubuntu APT repository. It allows different projects to use different Node.js versions.

Install NVM using the current installer published by the NVM project:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
```

Reload the shell:

```bash
source ~/.zshrc
```

Check NVM:

```bash
nvm --version
```

Install the current Node.js LTS:

```bash
nvm install --lts
```

Make the LTS version the default:

```bash
nvm alias default 'lts/*'
```

Verify:

```bash
node -v
npm -v
```

### Switch Node.js versions

```bash
nvm install 20
nvm install 22
nvm use 20
nvm use 22
```

For a project-specific version, add a `.nvmrc` file:

```text
22
```

Then:

```bash
nvm use
```

---

## 07 - pnpm

Enable Corepack:

```bash
corepack enable
```

Activate pnpm:

```bash
corepack prepare pnpm@latest --activate
```

Verify:

```bash
pnpm --version
```

Use the package manager already defined by a project. Do not randomly mix `npm`, `yarn` and `pnpm` in the same project.

---

## 08 - Docker

Remove conflicting packages if they exist:

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

Add the Docker repository:

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

Install Docker Engine and Compose:

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

## 09 - Run Docker Without sudo

Add the current user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

Log out and log back in, then test:

```bash
docker run hello-world
```

If the command works without `sudo`, the user setup is complete.

---

## 10 - Docker Storage Management

A 512GB SSD can fill quickly when Docker images, build cache and volumes accumulate.

Check Docker disk usage:

```bash
docker system df
```

Inspect Docker storage:

```bash
sudo du -sh /var/lib/docker
```

Clean unused containers, networks and dangling images when needed:

```bash
docker system prune
```

Be careful with:

```bash
docker system prune -a
```

It can remove unused images that you may still want for local development.

Check overall disk usage:

```bash
df -h
ncdu /
```

---

## 11 - Laravel Environment Strategy

For a multi-project Laravel workstation, avoid depending on one global PHP version.

### Keep on the Host

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

### Prefer Docker for Application Services

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

This allows different projects to use different PHP versions without conflicts.

Example:

```text
Laravel project A → PHP 8.2
Laravel project B → PHP 8.3
Laravel project C → PHP 8.4
```

---

## 12 - Recommended Laravel Project Structure

```text
~/Projects/
│
├── crm/
├── ecommerce/
└── other-project/
```

Create the directory:

```bash
mkdir -p ~/Projects
cd ~/Projects
```

A Dockerized Laravel project can contain:

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

## 13 - MySQL with Docker

Do not install MySQL globally if the project is already Docker-based.

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

Start the service:

```bash
docker compose up -d
```

Check:

```bash
docker ps
```

Inside Docker, Laravel should normally connect using the service name:

```env
DB_HOST=mysql
DB_PORT=3306
```

Not:

```env
DB_HOST=127.0.0.1
```

---

## 14 - Redis with Docker

Redis is useful for Laravel cache, sessions and queues.

Example Compose service:

```yaml
services:
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
```

Typical Laravel environment:

```env
CACHE_STORE=redis
SESSION_DRIVER=redis
QUEUE_CONNECTION=redis
REDIS_HOST=redis
```

Again, `redis` is the Docker Compose service name.

---

## 15 - phpMyAdmin with Docker

For local development:

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

Open:

```text
http://localhost:8080
```

Do not expose phpMyAdmin publicly on a production server unless there is a specific, secured reason to do so.

---

## 16 - Laravel Queue Worker

For a production-like local architecture, keep the queue worker separate from the web container.

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

Conceptually the Compose stack becomes:

```text
app
nginx
mysql
redis
queue
scheduler
```

This makes it easier to reproduce production queue behaviour locally.

---

## 17 - Laravel Scheduler

Run the scheduler in its own service when testing scheduled jobs:

```bash
php artisan schedule:work
```

This avoids manually triggering scheduled commands during development.

---

## 18 - Nginx + PHP-FPM

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

For a Docker-first workflow, keep Nginx inside the project stack instead of installing another global Nginx instance on the host.

---

## 19 - React / Inertia / Vite

For projects using React, Inertia and Vite:

```bash
npm install
npm run dev
```

Or with pnpm:

```bash
pnpm install
pnpm dev
```

Vite commonly runs on:

```text
http://localhost:5173
```

If Vite runs inside Docker, bind it to all interfaces:

```js
server: {
    host: '0.0.0.0',
}
```

For a developer workstation, keeping Node.js/NVM on the host is convenient for Vite hot reload and project-specific Node version management.

---

## 20 - DevOps Utilities

Install common tools:

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

Useful commands:

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

## 21 - UFW Firewall

Check status:

```bash
sudo ufw status
```

If you use SSH, allow SSH before enabling the firewall:

```bash
sudo ufw allow ssh
sudo ufw enable
```

Check again:

```bash
sudo ufw status verbose
```

For a development laptop, only open ports that you actually need.

---

## 22 - Useful Zsh Aliases

Add useful shortcuts to `~/.zshrc`:

```bash
nano ~/.zshrc
```

Example:

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

## 23 - Typical Docker Laravel Workflow

Start the project:

```bash
docker compose up -d
```

Check services:

```bash
docker compose ps
```

Follow logs:

```bash
docker compose logs -f
```

Open the application container:

```bash
docker compose exec app bash
```

Run Laravel commands:

```bash
docker compose exec app php artisan migrate
docker compose exec app php artisan optimize:clear
```

Install Composer dependencies:

```bash
docker compose exec app composer install
```

Install frontend dependencies if Node is part of the container:

```bash
docker compose exec node npm install
```

Stop the project:

```bash
docker compose down
```

---

## 24 - Final Host vs Docker Checklist

| Component | Recommended Location |
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

## 25 - Recommended Development Flow

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

### Core Principle

> **Host = developer tooling. Docker = application runtime and infrastructure services.**

This keeps Laravel projects isolated, makes PHP/database versions easier to manage, and gives the local environment a structure that is closer to a real DevOps deployment.
