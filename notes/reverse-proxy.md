# Reverse Proxy



### Configuration for node
Access `default site-available` or create your own one:
```shell
sudo nano /etc/nginx/sites-available/default
```
Example for `node` server listening on 3000
```
server {
        listen 80 default_server;
        listen [::]:80 default_server;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        server_name hellonode;

        location ^~ /assets/ {
                gzip_static on;
                expires 12h;
                add_header Cache-Control public;
        }

        location / {
                proxy_http_version 1.1;
                proxy_cache_bypass $http_upgrade;

                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;

                proxy_pass http://localhost:3000;
        }
}
```
Adding a reverse proxy route for a service running on `3001`
```
location /myservice {
        rewrite ^/myservice/(.*)$ /$1 break;

        proxy_http_version 1.1;
        proxy_cache_bypass $http_upgrade;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_pass http://localhost:3001;
}
```
Check validity of configuration
```
sudo nginx -t
```
Restart to activate reverse proxy
```
sudo systemctl restart nginx
```

