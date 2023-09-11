# NGINX-Vless-gRPC-CDN

# 1 update and upgrade your server with these commands
```
apt update && apt upgrade -y
```

# 2 install NGINX & Certbot
```
sudo apt install nginx certbot python3-certbot-nginx -y
```

# 3 copy default NGINX config to your website
```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/yourwebsite
```

# 4 enable your website 
```
ln -s /etc/nginx/sites-available/yourwebsite /etc/nginx/sites-enabled/
```

```
cd /etc/nginx/sites-enabled && ls -la
```

# 5 edit your website config file and edit as below
```
nano /etc/nginx/sites-available/yourwebsite
```

# 5.1 remove default values
```
server_name yourwebsite;
```

# add another location
```
	location /yourwebsite {
		if ($content_type !~ "application/grpc") {
			return 404;
		}
		client_max_body_size 0;
		client_body_timeout 1071906480m;
		grpc_read_timeout 1071906480m;
		grpc_pass grpc://127.0.0.1:2002;
	}
```
	
# 5.2 restart NGINX service	
```
systemctl restart nginx
```

# 6 get a free SSL 
```
certbot --nginx -d yourwebsite --register-unsafely-without-email
```

# 7 edit your website config file and edit as below again
```
nano /etc/nginx/sites-available/yourwebsite
```

# 7.1 add http2 to your 443 port
```
    listen [::]:443 ssl http2 ipv6only=on; # managed by Certbot
    listen 443 ssl http2; # managed by Certbot
```

# 7.2 restart NGINX service
```
systemctl restart nginx
```

# 8 install xRay panel FranzKafkaYu "https://github.com/FranzKafkaYu/x-ui/blob/main/README_EN.md"
```
bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install_en.sh)
```
