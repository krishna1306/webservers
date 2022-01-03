# Gunicorn <---> Nginx:HTTPS

This tutorial assumes that **Gunicorn** is already serving the website at `localhost:5000`

- Install nginx using epel-release repo
  - Config file of nginx located at `/etc/nginx/nginx.conf`
  - `systemctl enable --now nginx`
- Install `snapd`
  - `systemctl enable --now snapd.socket`
  - `ln -s /var/lib/snapd/snap /snap`
  - Restart `snapd` and `snapd.socket` to check if snap is working
  - Check `snap version` to see if its loading quickly
  - Disable **SE Linux** if snap is not responsive
- Install **Cert bot**
  - `snap install --classic certbot`
  - `ln -s /snap/bin/certbot /usr/bin/certbot`

> Change the **A** record in your DNS provider to point to the server public IP - before proceeding to the next step

https://www.redhat.com/sysadmin/setting-reverse-proxies-nginx

#### `/etc/nginx/nginx.conf`

```bash
worker_processes  1;

events {
    worker_connections  1024; 
}
    
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name  stclub.site;

        location / {
            proxy_pass http://localhost:5000/;
            index  index.html index.htm;
            } # end location
        } # end server
    } # end http
```

- `certbot --nginx` to automatically change **nginx** config to enable HTTPS