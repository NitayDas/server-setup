# server-setup


# Gunicorn Setup

## 1.Insatll gunicorn
```
pip install gunicorn
```

## 2.BInd with project
```
gunicorn mysite.wsgi:application --bind 0.0.0.0:8000
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
WorkingDirectory=/home/project-folder
ExecStart=/home/project-folder/venv/bin/gunicorn \
  --env DJANGO_SETTINGS_MODULE=project-name.settings \
  --workers 3 --bind 127.0.0.1:8000 project-name.wsgi:application


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
