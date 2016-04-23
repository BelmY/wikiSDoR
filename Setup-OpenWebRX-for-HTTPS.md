Setup Nginx for HTTPS on the same host, then use it as a reverse proxy.

Sample configuration:
```
location /openwebrx/ {
    proxy_pass http://127.0.0.1:8073/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header        Host            $host;
    proxy_set_header        X-Real-IP       $remote_addr;
    proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header        X-Forwarded-Proto $scheme;
}
```

In `htdocs/openwebrx.js` and `openwebrx.py`, replace any `ws://` with `wss://`.

In `config_webrx.py`, set:

    server_hostname="nginx-host.example.com/openwebrx/"

Disable access to port 8073 with `iptables` so that clients can only connect over HTTPS.

> This information was shared by Benjamin, HB3YIW. Thanks for that!