# HTTPS


### Configure
Intall `certbot`
```shell
sudo apt-get install certbot python3-certbot-nginx
```
Edit `/etc/nginx/sites-available/default` to set the correct server name (essential for SSL)
```
server_name <my.domain.com>;
```
```shell
sudo systemctl reload nginx
```
Generate certificate using `certbot`.
```
sudo certbot --nginx -d my.domain.com
```
Enter your real email, as that will be used to communicate you any problem.  
Choose the option to redirect HTTP to HTTPS automatically.  


### Renewal
`SSL` certificates are valid for 90 days, and Certbot is already set up for automated renewal. To simulate and test-drive the renewal process, run:
```
sudo certbot renew --dry-run
```