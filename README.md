# Server-connect

1 serverni tayorlash
sudo apt update
sudo apt install nodejs npm -y

Tekshirish
node -v
npm -v

 MongoDB key va repo qo‘shish
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-6.0.gpg

echo "deb [signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
Paketlar ro‘yxatini yangilash va MongoDB o‘rnatish
sudo apt update
sudo apt install -y mongodb-org

✅ 3. MongoDB xizmatini ishga tushirish
sudo systemctl start mongod
sudo systemctl enable mongod
sudo systemctl status mongod

SSH ulash
ssh-keygen
cat .ssh/id_ed25519.pub 
Gitga ulash
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHZI/Bag1HIcUD8VtOQaXxmsl2RKc573ghPSSg9mR+mW root@Humo-server
SSH orqali yuklash 
git@github.com:Saidumarov/humo-group.git
PM2 bilan serverni ishga tushurish
sudo npm install pm2 -g
pm2 start index.js --name Humo-file-server
pm2 save
pm2 startup
pm2 restart <app_name_or_id>
✅ 5. Nginx o‘rnatish va domeni activation qilish
sudo apt install nginx -y
sudo systemctl status nginx
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
server {
    listen 80;
    server_name api.picnic-tour.uz;

    location / {
        proxy_pass http://localhost:PORT;  # API serveringiz ishlayotgan portni yozing
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

sudo ln -s /etc/nginx/sites-available/api.picnic-tour.uz /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

sudo certbot --nginx -d api.picnic-tour.uz

