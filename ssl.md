To create an SSL certificate for a domain on a VPS, you can use **Let's Encrypt**, a free and widely trusted certificate authority. The easiest way to obtain and install a Let's Encrypt SSL certificate is by using a tool called **Certbot**. Here’s how to do it:

1. **Install Certbot and the Nginx plugin**:
   - On Ubuntu/Debian:
     ```bash
     sudo apt update
     sudo apt install certbot python3-certbot-nginx -y
     ```
   - On CentOS/RHEL:
     ```bash
     sudo yum install epel-release
     sudo yum install certbot python3-certbot-nginx
     ```

2. **Ensure your Nginx configuration is correct**:
   - Make sure your Nginx configuration file for the domain is set up correctly and that your server is accessible via HTTP (port 80).
   - If you’ve set up the Nginx configuration as described earlier, you should be ready to proceed.

3. **Obtain and install the SSL certificate**:
   - Run the following Certbot command to obtain an SSL certificate and automatically configure Nginx:
     ```bash
     sudo certbot --nginx -d your_domain -d www.your_domain

     
     sudo certbot --nginx -d api.studioimmigrato.com

      IMPORTANT NOTES:
       - Congratulations! Your certificate and chain have been saved at:
         /etc/letsencrypt/live/api.studioimmigrato.com/fullchain.pem
         Your key file has been saved at:
         /etc/letsencrypt/live/api.studioimmigrato.com/privkey.pem
         Your cert will expire on 2025-01-24. To obtain a new or tweaked
         version of this certificate in the future, simply run certbot again
         with the "certonly" option. To non-interactively renew *all* of
         your certificates, run "certbot renew"
       - If you like Certbot, please consider supporting our work by:
      
         Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
         Donating to EFF:                    https://eff.org/donate-le
     ```
   - Replace `your_domain` with your actual domain name.
   - Certbot will prompt you to provide an email address and agree to the terms of service.
   - It will automatically configure Nginx for HTTPS.

4. **Verify the SSL certificate installation**:
   - You can check your site in a web browser by visiting `https://your_domain` to confirm that it's using HTTPS.

5. **Set up automatic renewal**:
   - Let's Encrypt certificates are valid for 90 days, but you can easily set up automatic renewal by adding a Cron job:
     ```bash
     sudo crontab -e
     ```
   - Add the following line to the file:
     ```
     0 3 * * * /usr/bin/certbot renew --quiet
     ```
   - This Cron job will run the renewal command daily at 3 AM and renew the certificate automatically if it’s close to expiration.

6. **Restart Nginx to apply any changes (optional)**:
   ```bash
   sudo systemctl restart nginx
   ```

Now, your domain should have an SSL certificate, and Nginx will serve your site over HTTPS. Certbot takes care of configuring Nginx for SSL and setting up automatic renewal.
