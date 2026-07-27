# 🚀 CodeSandbox + VPS + Pterodactyl Panel Setup

This repository contains all the commands and resources used in my YouTube tutorial to set up a VPS-style environment inside CodeSandbox and install Pterodactyl Panel with Wings.

> **Disclaimer:** This guide is for educational purposes. CodeSandbox is a cloud development environment and does not officially provide a permanent VPS. Availability depends on CodeSandbox's policies.

---

# 📚 Table of Contents

- Keep CodeSandbox Active
- Create the VPS Environment
- Install Pterodactyl Panel
- Create SSL Certificate
- Install Wings
- Troubleshooting

---

# 🟢 Step 1: Keep CodeSandbox Active

## Cron Job

Open crontab:

```bash
crontab -e
```

Add:

```cron
*/5 * * * * curl -fsSL https://YOUR-CODESANDBOX-URL >/dev/null 2>&1
```

Save and exit.

---

## Google Apps Script

Create a new Google Apps Script project.

Paste:

```javascript
function keepAlive() {
  UrlFetchApp.fetch("https://YOUR-CODESANDBOX-URL");
}
```

Create a Time Trigger to run every 5 minutes.

---

# ⚡ Step 2: Create the VPS Environment

Run:

```bash
bash <(curl -sSL https://raw.githubusercontent.com/dxdgame/DAYTONA-VPS1/main/install.sh)
```

This script will prepare the VPS environment and install the required packages.

---

# 🛠️ Step 3: Install Pterodactyl Panel

Run the installer:

```bash
bash <(curl -s https://pterodactyl-installer.se)
```

Select:

```
0) Install Panel
```

or

```
2) Install Panel + Wings
```

Complete the installer by entering:

- Domain
- Database Details
- Admin Username
- Admin Email
- Password
- Timezone

---

# 🔒 Step 4: Generate a Self-Signed SSL Certificate

Create the certificate directory:

```bash
mkdir -p /etc/certs
cd /etc/certs
```

Generate the certificate:

```bash
openssl req -new -newkey rsa:4096 -days 3650 -nodes -x509 \
-subj "/C=NA/ST=NA/L=NA/O=NA/CN=Generic SSL Certificate" \
-keyout privkey.pem \
-out fullchain.pem
```

Certificate files:

```
/etc/certs/fullchain.pem
/etc/certs/privkey.pem
```

---

# 🪽 Step 5: Configure Wings SSL

Edit the Wings configuration:

```bash
nano /etc/pterodactyl/config.yml
```

Use:

```yaml
ssl:
  enabled: true
  cert: /etc/certs/fullchain.pem
  key: /etc/certs/privkey.pem
```

Save the file.

Restart Wings:

```bash
systemctl restart wings
```

Enable Wings at boot:

```bash
systemctl enable wings
```

---

# 🌐 Step 6: Open Your Panel

Visit:

```
https://YOUR-DOMAIN
```

or

```
http://YOUR-SERVER-IP
```

Login using the administrator account created during installation.

---

# 🛠 Troubleshooting

Restart Panel

```bash
systemctl restart nginx
systemctl restart mariadb
systemctl restart redis-server
systemctl restart php*-fpm
```

Restart Wings

```bash
systemctl restart wings
```

Check Wings logs

```bash
journalctl -u wings -f
```

Check Panel logs

```bash
tail -f /var/www/pterodactyl/storage/logs/laravel.log
```

---

# ⭐ Support

If this repository helped you:

- ⭐ Star the repository
- 👍 Like the YouTube video
- 🔔 Subscribe to the channel

Happy Hosting! 🚀
