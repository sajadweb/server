# Nginx

# install
```sh
sudo apt update
sudo apt install nginx
systemctl status nginx
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl disable nginx
sudo systemctl enable nginx
//firwall
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status

curl -4 icanhazip.com


//update nginx
sudo nginx -t

```

## Config domain
```sh
sudo nano /etc/nginx/sites-available/your_domain
```

```json
server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
}
```
```sh
sudo nano /etc/nginx/nginx.conf

sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```