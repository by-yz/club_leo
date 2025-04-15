# Part 1 : 

## I : Install 

```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Pour demarer et activer docker : 

```
sudo systemctl start docker
sudo systemctl enable docker
```

Pour ajouter l'utilisateur au groupe docker : 

```
sudo usermod -aG docker $(whoami)
exit
```

## II : Nginx

```
docker run -d -p 9999:80 nginx
```

Après avoir autorisé la règle de parefeu on a : 


```
azureuser@test-tp1:~$ curl http://20.61.61.61:9999
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```


Pour un site un peu custom
```
docker run --name meow -d -v /var/www/tp_docker/index.html:/var/www/html/index.html -v /etc/nginx/conf.d/nginx.conf:/etc/nginx/conf.d/nginx.conf -p 9999:7777 --memory=512m nginx
```

dans le nginx.conf : 

```
server {
  listen 7777;
  root /var/www/html/;
}
```

# Part II

Le docker : 

```
FROM ubuntu

RUN apt update -y && apt upgrade -y


RUN apt install apache2 -y

COPY index.html /var/www/html/


CMD ["apache2ctl", "-D", "FOREGROUND"]
```

# Part III

le docker compose : 

```
cat docker-compose.yml
services:

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: wiki
      POSTGRES_PASSWORD: wikijsrocks
      POSTGRES_USER: wikijs
    logging:
      driver: none
    restart: unless-stopped
    volumes:
      - db-data:/var/lib/postgresql/data

  wiki:
    image: ghcr.io/requarks/wiki:2
    depends_on:
      - db
    environment:
      DB_TYPE: postgres
      DB_HOST: db
      DB_PORT: 5432
      DB_USER: wikijs
      DB_PASS: wikijsrocks
      DB_NAME: wiki
    restart: unless-stopped
    ports:
      - "3000:3000"

volumes:
  db-data:

```


on fait le curl : 

```
 curl http://20.61.61.61:3000
<!DOCTYPE html><html><head><meta http-equiv="X-UA-Compatible" content="IE=edge"><meta charset="UTF-8"><meta name="viewport" content="user-scalable=yes, width=device-width, initial-scale=1, maximum-scale=5"><meta name="theme-color" content="#1976d2"><meta name="msapplication-TileColor" content="#1976d2"><meta name="msapplication-TileImage" content="/_assets/favicons/mstile-150x150.png"><title>Wiki.js Setup</title><link rel="apple-touch-icon" sizes="180x180" href="/_assets/favicons/apple-touch-icon.png"><link rel="icon" type="image/png" sizes="192x192" href="/_assets/favicons/android-chrome-192x192.png"><link rel="icon" type="image/png" sizes="32x32" href="/_assets/favicons/favicon-32x32.png"><link rel="icon" type="image/png" sizes="16x16" href="/_assets/favicons/favicon-16x16.png"><link rel="mask-icon" href="/_assets/favicons/safari-pinned-tab.svg" color="#1976d2"><link rel="manifest" href="/_assets/manifest.json"><script>var siteConfig = {"title":"Wiki.js"}
</script><link type="text/css" rel="stylesheet" href="/_assets/css/setup.22871ffac1b643eed4d9.css"><script type="text/javascript" src="/_assets/js/runtime.js?1742780132"></script><script type="text/javascript" src="/_assets/js/setup.js?1742780132"></script></head><body><div id="root"><setup wiki-version="2.5.307"></setup></div></body></html>

```

Avec Redis :

```
 curl http://20.61.61.61:8888
<h1>Add key</h1>
<form action="/add" method = "POST">

Key:
<input type="text" name="key" >

Value:
<input type="text" name="value" >

<input type="submit" value="Submit">
</form>

<h1>Check key</h1>
<form action="/get" method = "POST">

Key:
<input type="text" name="key" >
<input type="submit" value="Submit">
</form>

Host : 047e7a810170
```

# Part IV

docker run -it -v /etc/shadow:/etc/shadow:ro alpine cat /etc/shadow


 trivy image nginx

 trivy image mon_apache

  trivy image postgres:15-alpine

 trivy image ghcr.io/requarks/wiki:2