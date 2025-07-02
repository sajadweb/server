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
Yes, here's a basic Nginx configuration to proxy the domain `api-cso.zagrosagency.xyz` to a service running on `127.0.0.1:4000`.

### 🔧 Step-by-step Nginx Configuration

1. **Edit or create your Nginx site config:**
   On most Linux systems with Nginx, create a file like:

   ```bash
   sudo nano /etc/nginx/sites-available/api-cso.zagrosagency.xyz
   ```

2. **Paste the following config:**

   ```nginx
   server {
       listen 80;
       server_name api-cso.zagrosagency.xyz;

       location / {
           proxy_pass http://127.0.0.1:4000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

3. **Enable the site:**

   ```bash
   sudo ln -s /etc/nginx/sites-available/api-cso.zagrosagency.xyz /etc/nginx/sites-enabled/
   ```

4. **Check Nginx config for syntax errors:**

   ```bash
   sudo nginx -t
   ```

5. **Reload Nginx to apply the changes:**

   ```bash
   sudo systemctl reload nginx
   ```

---

### 🔒 Optional (HTTPS with Let's Encrypt)

If you want HTTPS, you can use Certbot:

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d api-cso.zagrosagency.xyz
```

Let me know if you're behind a firewall, using Docker, or want HTTPS or a custom path — I can adjust the config accordingly.
