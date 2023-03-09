# Not yet finishing content

# AntMediaServer-Nginx
AMS with Nginx 


## Quick Start
### Step 1: To run ant-media-server(CE)
```shell
docker run --name ams -d -p 5080:5080 nibrev/ant-media-server:latest
```
### Step 2: To create a file (name: default.conf)
```shell
server {
  listen 1111 ssl;
  ssl_certificate     /etc/letsencrypt/live/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/privkey.pem;


  server_name std11.****.com;

  location / {
     proxy_pass http://192.168.21.75:8080;  ## replace your application IP address
     proxy_http_version 1.1;
     proxy_connect_timeout 7d;
     proxy_send_timeout 7d;
     proxy_read_timeout 7d;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_set_header Host $host;
     proxy_set_header Upgrade $http_upgrade;
     proxy_set_header Connection "Upgrade";
  }
}
```
### Step 3: To run NGINX
```shell
docker run -it --rm  -p 1111:1111 --name nginx \
-v "${PWD}"/nginx/conf/nginx.conf:/etc/nginx/conf.d/default.conf \
-v /home/willie/nginx-certbot/live/:/etc/letsencrypt/live/ \
-it nginx
```
PS: "/home/willie/nginx-certbot/live/" this location is included "fullchain.pem" and "privkey.pem" [from 'Nginx-Certbot-Docker'](https://github.com/williehao/nginx-certbot/edit/main/README.md)

### Done
Now, you can input your Domain name on the browser to check it is get the CA key
![image](https://user-images.githubusercontent.com/15116422/223975732-2abe368f-76e4-4d59-a3fe-746c31c3546e.png)
![image](https://user-images.githubusercontent.com/15116422/223976350-2151cf43-fbe7-44f5-980a-0335ff7ae5b9.png)




