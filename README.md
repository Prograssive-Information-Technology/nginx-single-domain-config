# nginx-single-domain-config
This is the nginx config for single domain. 
# YOU MUST SELECT full TO THE SSL OPTION IN CLOUDFLARE


  GNU nano 6.2                                                                 jafrinshomemadefood.com                                                                          
server {
    # Listen for HTTP traffic and redirect it to HTTPS
    listen 80;
    server_name jafrinshomemadefood.com www.jafrinshomemadefood.com;

    # Redirect all HTTP requests to HTTPS
    return 301 https://$host$request_uri;
}

server {
    # Listen for HTTPS traffic
    listen 443 ssl;
    server_name jafrinshomemadefood.com www.jafrinshomemadefood.com;

    # SSL certificates
    ssl_certificate /etc/letsencrypt/live/jafrinshomemadefood.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/jafrinshomemadefood.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    # Reverse proxy to the Next.js app running on port 8084
    location / {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    # Logging
    access_log /var/log/nginx/mtmart_access.log;
    error_log /var/log/nginx/mtmart_error.log;
}

