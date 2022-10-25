#### notes on deploying jena to ubuntu server

~ server

ssh root@206.189.59.185

adduser paulduchesne

usermod -aG sudo paulduchesne

rsync --archive --chown=paulduchesne:paulduchesne ~/.ssh /home/paulduchesne

exit

~ jena

ssh paulduchesne@206.189.59.185

sudo apt update

sudo apt install default-jre

wget https://dlcdn.apache.org/jena/binaries/apache-jena-fuseki-4.6.1.tar.gz

tar -xzvf apache-jena-fuseki-4.6.1.tar.gz

cd apache-jena-fuseki-4.6.1/

screen ./apache-jena-fuseki-4.6.1/fuseki-server --memTDB /my-dataset

~ nginx

sudo apt install nginx certbot python3-certbot-nginx

certbot certonly --nginx --noninteractive --agree-tos --email paulduchesne@tuta.io -d filmbase.wiki

sudo nano /etc/nginx/nginx.conf

```
user  paulduchesne;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {}

http {
    client_max_body_size 100M;
    server {
        listen 80;
        server_name filmbase.wiki;
        return 301 https://$server_name$request_uri;
    }
    server {
        listen 443 ssl;
        server_name filmbase.wiki;
        ssl_certificate     /etc/letsencrypt/live/filmbase.wiki/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/filmbase.wiki/privkey.pem;
        ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers         HIGH:!aNULL:!MD5;
        access_log /var/log/nginx/filmbase.wiki.log;
        location / {
            proxy_pass http://127.0.0.1:3030;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-forwarded-host $host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }  
} 
```

sudo service nginx reload
