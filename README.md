### Install from epel because it is not available in standard repo

* yum install epel-release
* yum install nginx

### Variables documentation ###

* To see the compiled modules

nginx -V

* http://nginx.org/en/docs/varindex.html

Configuration 

* Directives
name value
ex: server_name www.domain.com

* Contexts

| context       | Sample       |  
| ---           | ---          | 
| http{ ... }   | http related |
| server{ ... } | Virtual Host like Apache |
| location{ ... } | Matching url location |

```bash
http{
  ... 
  server{
  ...  
  }
  location{
  ...
  }
}
```

# Check the config
```bash 
nginx -t
```

```bash
$ head mime.types
types {
    text/html                                        html htm shtml;
    text/css                                         css;
    text/xml                                         xml;
    image/gif                                        gif;
    image/jpeg                                       jpeg jpg;
    application/javascript                           js;
    application/atom+xml                             atom;
    application/rss+xml                              rss;
```

## Access via https -> nginx (Ex: SonarQube)

### Generate the certificat
* https://github.com/davidboukari/ssl/
```
mkdir -p /etc/nginx/<domainname>
cp private.{crt,key} /etc/nginx/<domainname> 

cat nginx-mydomain.com.conf
    server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        ssl_certificate  /etc/nginx/<domainname>/private.crt;
        ssl_certificate_key  /etc/nginx/<domainname>/private.key;
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        # Proxy settings
        proxy_set_header X-Real-IP $remote_addr;
        proxy_ssl_session_reuse on;
        location / {
          proxy_set_header   X-Forwarded-Host $http_host;
         # proxy_set_header   Host $host:$server_port;
          proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header   X-Forwarded-Proto $scheme;
          proxy_http_version 1.1;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header Host $host;

          proxy_pass         http://127.0.0.1:9000;
          proxy_redirect     http://127.0.0.1:9000   https://<THEREMOTEIP>:4490;
        }

systemctl restart nginx

# Go to 
https://<REMOTEIP>:4490
```
