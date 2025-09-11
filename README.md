# server-setup

## Transfer from one server to another


## command in old server terminal
### 1. First zip the folder in the old server
```
tar -czvf folder_name.tar.gz folder_name 

```

## command in New server
### 2. transfer from older server to the new server
```
scp /home/laxmipur/folder_name.tar.gz /home

```
### 3 unzip the folder
```
tar -xzvf folder_name.tar.gz
```
### 4. Deleter the Zip
```
rm -rf folder_name.tar.gz
```


# Gunicorn Setup

## 1.Insatll gunicorn
```
pip install gunicorn
```

## 2.BInd with project
```
gunicorn abdulgafur_backend.wsgi:application --bind 0.0.0.0:8012
```

# 3.Open the gunicorn file

```
sudo nano /etc/systemd/system/gunicorn.service
```

```
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=root
Group=www-data
WorkingDirectory=/home/project_folder/college_backend
ExecStart=/home/college_venv/bin/gunicorn \
  --env DJANGO_SETTINGS_MODULE=abdulgafur_backend.settings \
  --workers 3 --bind 127.0.0.1:8001 abdulgafur_backend.wsgi:application


[Install]
WantedBy=multi-user.target
```


# 4.start, enable and restart the gunicorn
```
sudo systemctl daemon-reexec
sudo systemctl start gunicorn
sudo systemctl enable gunicorn
sudo systemctl restart gunicorn
```


## nginx setup:

# open the nginx file
```
sudo nano /etc/nginx/conf.d/natunhat.conf
```

```
server {
 client_max_body_size 30M;
 listen 80;
 root /home/natunhat_college/natunhatcollege_frontend/dist;
 index index.html index.nginx-debian.html;
 server_name npcj.edu.bd www.npcj.edu.bd;

 location / {
     try_files $uri /index.html;
 }

 location ~ ^/api {
     proxy_pass http://localhost:8000;
     proxy_set_header HOST $host;
     proxy_set_header X-Forwarded-Proto $scheme;
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 }

 location ~ ^/admin {
     proxy_pass http://localhost:8000;
     proxy_set_header HOST $host;
     proxy_set_header X-Forwarded-Proto $scheme;
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 }

 location = /favicon.ico {
     access_log off;
     log_not_found off;
 }

 location /static/ {
        autoindex on;
        alias /home/natunhat_college/college_backend/staticfiles/;
    }

 # Serve Django media files
    location /media {
        autoindex on;
        alias /home/natunhat_college/college_backend/media;  # Path to Django media files
    }

}
```

# log of nginx
```
nginx -t
```

# start,enable,restart nginx
```
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl restart nginx
```

# status of nginx
```
sudo systemctl status nginx
```


# bind with ssl
```
sudo certbot --nginx -d npcj.edu.bd -d www.npcj.edu.bd
```




