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
scp -r root@OLD_VPS_IP:/home/folder_name.tar.gz /home

```
### 3 unzip the folder
```
tar -xzvf folder_name.tar.gz
```
### 4. Deleter the Zip
```
rm -rf folder_name.tar.gz
```


## Postgresql setup

### 1. backup the databse

```
PGPASSWORD="db_password" pg_dump -h localhost -U db_user db_table > backup.sql
```

### 2. zip the database
```
gzip utshab_backup.sql

```

### 3. transfer from loder server to new server (command in new server)
```
scp -r root@86.48.3.219:/home/utshab_backup.sql.gz /home/utshab
```

### 4. unzip the sql file:
```
gunzip utshab_backup.sql.gz
```

### 5. switch to postgresql 
```
sudo -u postgres psql
```
### 6. create new postgresql
```
CREATE DATABASE db_table;
CREATE USER db_user WITH PASSWORD 'db_password';
GRANT ALL PRIVILEGES ON DATABASE db_table TO db_user;
ALTER DATABASE db_table OWNER TO db_user;
```
### 7. Restore the backup
do it before migrations
```
PGPASSWORD="db_password" psql -h localhost -U db_user -d db_table -f /home/project_dir/backup.sql
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




