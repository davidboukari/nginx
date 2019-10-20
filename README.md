### Install from epel because it is not available in standard repo###

* yum install epel-release

Configuration 

* Directives
name value
ex: server_name www.domain.com

* Contexts

| -             |  -           | 
| http{ ... }   | http related |
| server{ ... } | Virtual Host like Apache |
| location{ ... } | Matching url location |

http{
  ... 
  server{
  ...  
  }
  location{
  ...
  }
}


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

