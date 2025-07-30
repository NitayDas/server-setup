# server-setup


## Gunicorn Setup

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
