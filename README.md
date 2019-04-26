`myip` is a small software allowing you to display your external IP as seen on the internet.

Example:
```console
$ curl ip.balthazar-rouberol.com
77.194.52.44
```

## Running it behind a reverse-proxy

If you run `myip` behind a reverse-proxy, such as nginx, the `Host` header will always contain your local IP, and not the requester's. To make it work, configure your reverse-proxy to include the requester's ip in the request header. I run `myip` with the following command line and nginx configuration:

```
# command line
$ /usr/local/bin/myip -listen-addr localhost:8000 -proxy-header X-Forwarded-For

# nginx configuration
server {
    listen 80;
    server_name ip.balthazar-rouberol.com;

    location / {
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_pass http://localhost:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

## Usage

```
Usage of myip:
  -listen-addr string
        server listen address (default ":5000")
  -proxy-header string
        pass if running behind a reverse-proxy
```
