
Documentation is available at http://nginx.org

# System
Ubuntu 14.04

# Install
```bash
sudo apt-get install libpcre3 libpcre3-dev
```

# Build
```bash
./configure --prefix=/opt/nginx --sbin-path=/opt/nginx/sbin/nginx --conf-path=/opt/nginx/conf/nginx.conf --with-http_stub_status_module --with-http_gzip_static_module --with-stream
make
sudo make install
cd /opt/nginx
```

# Sample

```
user  nobody nogroup;
worker_processes  1;

events {
    worker_connections  1024;
}

stream {
    upstream ss {
        #least_time first_byte;
        #hash $remote_addr consistent;
        server xx.xx.xx.xx:443 max_fails=0 fail_timeout=1s;
        server xx.xx.xx.xx:443 max_fails=0 fail_timeout=1s;
        server xx.xx.xx.xx:443 max_fails=0 fail_timeout=1s;
        server xx.xx.xx.xx:443 max_fails=0 fail_timeout=1s;
    }

    server {
        listen 443;
        listen 443 udp;
        proxy_pass ss;
    }
}

http {
    server {
        listen 8080;
        server_name  xx.xx.xx;
        root   /var/www/file;
        location / {
            autoindex on;
        }
    }       
}
```

# Run
检测语法
```bash
/opt/nginx/sbin/nginx -t
```

开启NGINX
```bash
/opt/nginx/sbin/nginx
```

重启NGINX
```bash
/opt/nginx/sbin/nginx -s reload
```

# Reference
[Nginx的Stream模块的编译使用](https://www.jianshu.com/p/5dcd1e027e17)

[nginx-latest.sh](https://gist.github.com/Globegitter/685e3739c0f181bda3ec)

[Nginx TCP和UDP负载均衡](https://my.oschina.net/leeck/blog/729443)

[Nginx中转Shadowsocks与负载均衡](https://lxiange.com/posts/nginx-loadbalance.html)

[TCP and UDP Load Balancing](https://docs.nginx.com/nginx/admin-guide/load-balancer/tcp-udp-load-balancer/)

[Choosing an NGINX Plus Load‑Balancing Technique](https://www.nginx.com/blog/choosing-nginx-plus-load-balancing-techniques/)
