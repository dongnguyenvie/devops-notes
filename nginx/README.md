## OS: Ubuntu 18.10 x64

## Install nodejs

```
apt install nodejs
```

## Install npm

```
apt install npm
```

## Install yarn

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
```

```
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```

```
apt-get update && apt-get install yarn
```

## Intall pm2

```
npm install pm2 -g
```

## Install nginx

```
apt-get install nginx
```

## Copy default nginx config to new file

Use your own hostname (audiovyvy.com)

```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/audiovyvy.com
```

## Edit config file

Use your own hostname (audiovyvy.com)

```
nano /etc/nginx/sites-available/audiovyvy.com
```

Use your own server name (www.audiovyvy.com, audiovyvy.com) and port (7000)

```
server {
    listen 80;
    server_name www.audiovyvy.com audiovyvy.com;

    location / {
        proxy_pass http://localhost:7000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Save config file and enable config

Use your own hostname (audiovyvy.com)

```
ln -s /etc/nginx/sites-available/audiovyvy.com /etc/nginx/sites-enabled/audiovyvy.com
```

## Test config

```
service nginx configtest
```

## Restart nginx

```
service nginx restart
```

## Startup default run nginx

```
update-rc.d nginx defaults
```

## Create SSH key for Gitlab CI/CD

Use your own email (user@mail.com)

```
ssh-keygen -t rsa -b 4096 -C "user@mail.com"
```

Enter three times when it asks

```
copy “id_rsa.pub” file into root/.ssh/ and rename it authorized_keys
cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
```

Use your own keyName (SSH_PRIVATE_KEY)

```
go into Gitlab => Settings => CI/CD => Environment variables => SSH_PRIVATE_KEY
create new variable with “id_rsa” file’s content
to get content of file
cat ~/.ssh/id_rsa
```

Use your own hostname (SERVER_HOST)

```
go into Gitlab => Settings => CI/CD => Environment variables =>
create new variable with username@hostIP ex: root@xx.xxx.x.x.x
```

```
copy “id_rsa.pub” file content into [Github SSH Keys](https://gitlab.com/profile/keys)
to get content of file
cat ~/.ssh/id_rsa.pub
```

## Setup SSL for domain

Enable firewall

```
sudo ufw enable
```

Enable SSH

```
sudo ufw allow ssh
```

Install cerbot

```
add-apt-repository ppa:certbot/certbot
```

```
apt-get update
```

```
apt-get install python-certbot-nginx
```

Test nginx config

```
nginx -t
```

Reload nginx

```
systemctl reload nginx
```

Check firewall status

```
ufw status
```

Allow SSL in nginx

```
ufw allow 'Nginx Full'
ufw delete allow 'Nginx HTTP'
```

Apply cerbot to domain (audiovyvy.com)

```
certbot --nginx -d audiovyvy.com -d www.audiovyvy.com
```

Choose 1 (no redirect) or 2 (redirect) base on your need
Auto renew SSL certificate

```
certbot renew --dry-run
```
