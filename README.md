# linux-cmd-website

## Step-by-Step Guide to Set Up a Basic Website on Linux (Ubuntu/Debian)

1. Update Your System
   
Always start by updating packages to avoid issues.
```
sudo apt update && sudo apt upgrade -y
```

2. Install a Web Server (Nginx)
   
Nginx is great for beginners and efficient.
```
sudo apt install nginx -y
```
- Start and enable Nginx:
```
sudo systemctl start nginx
sudo systemctl enable nginx
```
- Check status:
```
sudo systemctl status nginx
```
You should see "active (running)".

3. Configure Firewall (UFW) for Website Access
   
As we discussed before, open ports 80 (HTTP) and 443 (HTTPS). If you haven't set up UFW yet:
```
sudo ufw allow ssh  # Don't lock yourself out!
sudo ufw allow http
sudo ufw allow https
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw enable
sudo ufw status
```

4. Set Up Your Website Files
- Create a directory for your site (e.g., example.com):
```
sudo mkdir -p /var/www/html/example.com
sudo chown -R $USER:$USER /var/www/html/example.com  # Give yourself ownership
```

5. Configure Nginx for Your Site
- Create a config file:
```
sudo nano /etc/nginx/sites-available/example.com
```
Paste this basic config (replace example.com with your domain):
```
server {
    listen 80;
    listen [::]:80;

    server_name example.com www.example.com;

    root /var/www/example.com/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
- Enable the site:
```
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```
- Test config and restart Nginx:
```
sudo nginx -t
sudo systemctl restart nginx
```

6. Set Up a Domain (Optional but Recommended)
- Point your domain's A record to your server's IP (do this in your domain registrar like GoDaddy or Namecheap).
- Get a free SSL cert with Let's Encrypt for HTTPS:
```
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d example.com -d www.example.com
```

7. Test Your Website
- Locally on the server:
```
curl http://localhost
```
- From your browser: Visit http://your-server-ip or http://example.com.
- If issues: Check logs with sudo journalctl -u nginx or sudo tail -f /var/log/nginx/error.log.











