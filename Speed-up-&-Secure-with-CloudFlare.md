## Overview

To make your server safer, you can use Nginx Reserved Proxy and WebSocket supported CDN to hide your original IP and speed up, we take a VPS and a CloudFlare as example to show how to do.

---

## Situation

 1. Your ISP didn't provide you with a public IP address.
 2. You have a VPS, with Nginx (Port 80), local web expose tool (Port 8080, for example Frp-Server) is installed, and your Raspberry has also installed Frp-Client.
 3. You have your own domain
 4. You have a CloudFlare Account, and your domain is hosted in it.

---

## How to do

 1. Switch on the WebSocket in CloudFlare Dashbroad (Your Domain --> Network --> WebSockets --> Switch On).
![Switch on](https://i.imgur.com/XJbJj3J.jpg)
Then turn to "Crypto", set SSL option to "Flexible" and switch on "Automatic Https Rewrite" to avoid losing stylesheet. If you want to redirect automatically when accessing, switch on " Always use Https".
![Flexible SSL](https://i.imgur.com/dFi0zBg.jpg)
![Automatic Https Rewrites](https://i.imgur.com/PtcsU3o.jpg)
 2. Add your record, forwarding to your VPS and light the "cloud", we set "s.example.org" here.
![Add Record](https://i.imgur.com/j9AWgPm.jpg)
 3. Login to your VPS, edit `/etc/hosts`, add the following line:
```
127.0.0.1 s.example.org
```
 4. Edit Nginx configure file by the following, it'll work in `https://s.example.org/webview/` and WebSocket will work in `wss://s.example.org/ws/`, the root path can put your profile `https://s.example.org/`.
```
server {
    listen 80;
    server_name s.example.org;
        location /webview/ {
        proxy_pass http://s.example.org:8080/;
        proxy_next_upstream error timeout invalid_header http_500 http_503 http_404;
        }
        location /ws/ {
        proxy_redirect off;
        proxy_pass http://s.example.org:8080/ws/;
        proxy_next_upstream error timeout invalid_header http_500 http_503 http_404;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
        }
    index index.php index.html index.htm default.php default.htm default.html;
    root /www/htdocs;
    error_page 404 /404.html;
    error_page 502 /502.html;
    access_log  /www/logs/access.log;
    error_log  /www/logs/error.log;
}
```
![Profile](https://i.imgur.com/XJRwN4X.jpg)
 5. Set the local listen port to 8073 in Frp-Client configure file `frpc.ini`, and run Frp-Client in its folder `./frpc -c ./frpc.ini` (Frp-Server should be running).
```
[common]
server_addr = 1.2.3.4
server_port = 3000
token = 123456
login_fail_exit = false
[web]
type = http
local_ip = 127.0.0.1
local_port = 8073
use_encryption = false
use_compression = false
custom_domains = s.example.org
```
 6. In `htdocs/openwebrx.js`, replace line 1698, 
```
ws_url="ws://"+(window.location.origin.split("://")[1])+"/ws/";
```
with 
```
ws_url="wss://s.example.org/ws/";
```
In `config_webrx.py`, set:
```
server_hostname="s.example.org/webview/"
```
in `openwebrx.py`, replace line 673 with:
```
("%[WS_URL]","wss://s.example.org/ws/"),
```
 7. OK, now type `python openwebrx.py` in terminal, and open `https://s.example.org/webview` in your browser, it runs!
![OpenWebRX under CloudFlare](https://i.imgur.com/diRXKCu.jpg)
