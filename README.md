![Yelactyl](https://raw.githubusercontent.com/Yelactyl/icon/main/New%20Project.png)

<hr>

# Yelactyl v1

All Features:
- Resource Management (Use It To Create Servers, Etc)
- Coins (AFK Page Earning, Linkvertise Earning, Gift Them Away)
- Renewal (Require Coins For Renewal)
- Coupons (Gives Resources & Coins To A User)
- Servers (create, View, Edit Servers)
- Payments (Buy Via Stripe)
- Login Queue (Prevent Overload)
- User System (Auth, Regen Password, Etc)
- Store (Buy Resources With Coins)
- Dashboard (View Resources)
- Join For Rewards (Join Discord Servers For Coins)
- Admin (Set/Add/Remove Coins & Resources, Create/Revoke Coupons)
- API (For Bots & Other Things)

# Warning

We Cannot Force You To Keep The "Powered By Yelactyl" In The Footer, But Please Consider Keeping It. It Helps Getting More Visibility To The Project And So Getting Better. We Won't Do Technical Support For Installations Without The Notice In The Footer. We May DMCA The Website In Certain Conditions.
Please do keep the footer though.

<hr>

# Install Guide

Warning: You Need Pterodactyl Already Set Up On A Domain For Yelactyl To Work
1. Upload The File Above Onto A Pterodactyl NodeJS Server [Download The Egg Here](https://github.com/Yelactyl/discordjsegg)
2. Unarchive The File And Set The Server To Use NodeJS 16
3. Configure settings.json (Specifically Panel Domain/ApiKey And Discord Auth Settings For It To Work)
4. Start The Server (Ignore The 2 Strange Errors That Might Come Up)
5. Login To Your DNS Manager, Point The Domain You Want Your Dashboard To Be Hosted On To Your VPS IP Address. (Example: dashboard.domain.com 192.168.0.1)
6. Run `apt install nginx && apt install certbot` On The VPS
7. Run `ufw allow 80` And `ufw allow 443` On The VPS
8. Run `certbot certonly -d <Your Yelactyl Domain>` Then Do 1 And Put Your Email
9. Run `nano /etc/nginx/sites-enabled/yelactyl.conf`
10. Paste The Configuration At The Bottom Of This And Replace `https://localhost:<port>` With The IP Of The Pterodactyl Server Including The Port And `<domain>` With The Domain You Want Your Dashboard To Be Hosted On.
11. Run `systemctl restart nginx` And Try Open Your Domain.

# Nginx Proxy Config
```Nginx
server {
    listen 80;
    server_name <domain>;
    return 301 https://$server_name$request_uri;
}
server {
    listen 443 ssl http2;
location /afkwspath {
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "upgrade";
  proxy_pass "http://localhost:<port>/afkwspath";
}
    
    server_name <domain>;
ssl_certificate /etc/letsencrypt/live/<domain>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<domain>/privkey.pem;
    ssl_session_cache shared:SSL:10m;
    ssl_protocols SSLv3 TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
location / {
      proxy_pass http://localhost:<port>/;
      proxy_buffering off;
      proxy_set_header X-Real-IP $remote_addr;
  }
}
```