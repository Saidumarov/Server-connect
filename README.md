# 🖥️ Server Setup Guide

Ushbu hujjat `Humo-server` backendini sozlash, MongoDB o‘rnatish, SSH konfiguratsiyasi, PM2 orqali serverni boshqarish va nginx yordamida domen bilan ulash bosqichlarini o‘z ichiga oladi.

## 1. Node.js va npm o‘rnatish

```bash
sudo apt update
sudo apt install nodejs npm -y
node -v
npm -v
```

## 2. MongoDB o‘rnatish

```bash
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-6.0.gpg

echo "deb [signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

sudo apt update
sudo apt install -y mongodb-org
```

## 3. MongoDB xizmatini ishga tushurish

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
sudo systemctl status mongod
```

## 4. SSH kalit yaratish va ulash

```bash
ssh-keygen
cat ~/.ssh/id_ed25519.pub
```

GitHub-ga ulash:

```bash
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHZI/Bag1HaIcUD8VtOQaXxmsl2xxxxxxRKc57z3ghPSSg9mR+mW root@Humo-server
```

SSH orqali klonlash:

```bash
git@github.com:Saidumarov/humo-group.git
```

## 5. PM2 orqali serverni ishga tushurish

```bash
sudo npm install pm2 -g
pm2 start index.js --name Humo-server
pm2 save
pm2 startup
pm2 restart Humo-server
```

## 6. Nginx o‘rnatish va domen sozlash

```bash
sudo apt install nginx -y
sudo systemctl status nginx
```

### Certbot va SSL uchun

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
```

### Nginx konfiguratsiyasi

```bash
sudo nano /etc/nginx/sites-available/api.humo.uz
```

```nginx
server {
    listen 80;
    server_name api.humo.uz;

    location / {
        proxy_pass http://localhost:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/api.humo.uz /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

SSL sertifikat olish:

```bash
sudo certbot --nginx -d api.humo.uz
```


