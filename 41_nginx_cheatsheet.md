# **Palowan Guru Cheatsheet - Nginx**

## **Table of Contents**

0. [TL;DR](#tldr) 
> If you are looking only for the syntax or the functionality, go directly to the TL;DR section. (Thanks Mr. Kierr Sunega for the suggestion)
1. [Basics](#basic)
2. [Control Nginx](#control-nginx)
3. [Configuration](#configuration)
    - [Serve static file](#serve-static-file)
    - [Reverse proxy server](#reverse-proxy-server)
    - [Load Balancing](#load-balancing)
    - [Cache](#cache)
6. [References](#references) 

---

**NOTES**
> 
> 1. For the sake of consistency, here is the structure of commands in this cheat sheet: 
>     - `command [OPTIONS] PARAMETER`
>       - Commands are indicated in lowercase.
>       - Parameters are indicated in uppercase.
>       - Parameters inside square brackets "[ ]" are optional.
>
> 2. You may know more about the nginx command by typing `nginx -h` or `man nginx`


---

## **TL;DR**

- Start nginx service: `nginx`
- Test the configuration file: `nginx -t`
- Stop nginx service: 
    - `nginx -s stop`
    - `kill -s TERM PROCESS_ID`
- Stop nginx service after finishing serving current requests: 
    - `nginx -s quit`
    - `kill -s QUIT PROCESS_ID`
- Reload the configuration file: `nginx -s reload`
- Reopen log files: 
    - `nginx -s reopen`
    - `kill -s USR1 PROCESS_ID`

---
 **Basics**
- One master process and several worker processes
  - Master process: read and evaluate configuration, maintain worker processes
  - Worker processes: handle processing of requests
      - Number of workers is defined in configuration file (fixed or adjusted to the number of available CPU cores)
- event-based model
- OS-dependent mechanism
---

## **Control Nginx**

> **Installation**
> 
> Install Nginx with homebrew: 
> 1. `brew update` to make sure using the latest version of homebrew
> 2. `brew install nginx`
> 
> Install Nginx with apt: 
> 1. `sudo apt update` to make sure using the latest version of apt
> 2. `sudo apt install nginx`

`ps -ax | grep nginx` = List all running nginx processes

`nginx` = Start nginx service

`nginx [OPTIONS]`
- `-t` = Test the configuration file
- `-s stop` = Stop nginx service 
- `-s quit` = Stop nginx service after finishing serving current requests
- `-s reload` = Reload the configuration file
- `-s reopen` = Reopen log files

---

## **Configuration**
> With regard to configuring Nginx service, knowing the purpose of the following files/directories before hand is crucial.
> 
> `/etc/nginx/nginx.conf`
> - This is the main configuration file that the Nginx daemon reads. All of your configurations should be directly or indirectly included here, in other words, you cam either add to this file the configurations or the `include` directives to reference configuration files stored elsewhere. (`/etc/nginx/conf.d/*.conf`and `/etc/nginx/sites-enabled/*` are referenced in this way by default.)
> 
> `/etc/nginx/nginx/sites-available`
> - In standard practice, this directory won't be included into the main configuration file `/etc/nginx/nginx.conf`, so you are free to store all of your configuration files here.
> 
> `/etc/nginx/nginx/sites-enabled`
> - All files in this directory are included into the main configuration file `/etc/nginx/nginx.conf`, so just store the symbol links referencing to the files in sites-available which you would like to activate.
> 
> `/etc/nginx/nginx/conf.d`
> - Since every config file in this directory must end with the extension `.conf` for being included in the main configuration file `/etc/nginx/nginx.conf`, storing configuration files here allows you to manage the files more intuitively. Unlike the former practice in which the available and enabled files are separated into two directories, all files are stored in one place. You can simply add an additional suffix like `.off` to a file in order to exclude the service.
> 
> **_Use `include` directive to enhance the scalability and maintainability of the configuration_**
> 
> Online Nginx config generator: [NGINXConfig](https://www.digitalocean.com/community/tools/nginx)


### **Serve static file**
```nginx
http {

    # Define server instance, can have more than one
    server {
        # Specify the port you want it to listen, default is 80
        # [::] is the IPv6 address, equivalent to 0000:0000:0000:0000:0000:0000:0000:0000
        listen 9990;
        listen [::]9990;

        # Process request if the Host request header matches this server name
        server_name example.com www.example.com;
        
        # Prefix will be compared with the URI, proceed if matches
        location / {

            # The result path = "root" + "prefix".
            # For example, "root /data/websites/www"; "location /;" = "/data/websites/www/"
            root /data/websites/www;

            # or "alias /data/websites/www/;" to specify the directory where the files are stored. 
            # So unlike root directive, location directive functions ONLY as an identifier,
            # itself would not affect the path where the files are served.

            # Specify the file names served by default
            # Can also be set in server context
            index index.html index.htm;

            # "try_files $uri $uri/ =404;" = first attempts to serve the request as file,
            # then as folder if no matching file,
            # finally display 404 page if no matching location.
            try_files $uri $uri/ =404;
        }

        location /asset {
            root /data/websites;   # or "alias /data/websites/asset/;"
            index index.html index.htm;
            try_files $uri $uri/ =404;
        }

        location /stat {

            # 'stub_status on' to display the network and connection status
	    	stub_status on;

            # 'access_log on' to display the access message 
	        access_log on;
		    
            # 'error_log' to display the error message
            error_log on;
            
            # Allow access from address specified
            # "allow all;" to allow access from every address
            # Can also be set in server context
            allow 127.0.0.1;
            
            # Deny access from address specified
            # "deny all;" to deny access from every address
            # Can also be set in server context
            deny all;
            
            # Activate HTTP authentication, "auth_basic off;" to deactivate.
            auth_basic "nginx status";
            
            # Specify file that stores the username and encrypted password
            # you may generate a password with openssl: `openssl passwd PASSWORD`
            auth_basic_user_file /etc/nginx/auth/passwd;
	    }
    }

    server {
        listen 81;
        listen [::]81;
        server_name example2.com www.example2.com;
        

        location / {
            root /data/websites2/www;   # or "alias /data/websites2/www/;"
            index index.html index.htm;
            try_files $uri $uri/ =404;
            auth_basic off;
        }

        location /stat {
	    	stub_status on;
	        access_log on;
		    error_log on;
            allow 127.0.0.1;
            deny all;
	    }
    }
}
```

### **Reverse proxy server**
```nginx
# for example, there are three apps running in different 
http {

    upstream apps {
        server 127.0.0.2:8001;
        server 127.0.0.3:8082;
        server 127.0.0.4:8103;
    }

    server {
        listen 80;
        listen [::]80;
        server_name example.com www.example.com;

        location / {

            # Specify to which server the requests will be passed, 
            proxy_pass http://apps;

            # You may import proxy configuration from /etc/nginx/proxy_params
        }

        location /asset/ {
            root /data;
            index index.html index.htm;
            try_files $uri $uri/ =404;
        }

        location /status {
	    	stub_status on;
	        access_log on;
		    error_log on;
            allow 127.0.0.1;
            deny all;
            auth_basic "NginxStatus";
            auth_basic_user_file /etc/nginx/auth/passwd;
	    }
    }
    ...
}


# /etc/nginx/proxy_params

# Specify the http protocol version (default is 1.0)
proxy_http_version 1.1;

# Set the request header passed to the proxied server
proxy_set_header HOST $host:$proxy_port; # The value of $host is the primary server name if the host field is not present in the request

# The value of $remote_addr is the client IP address
proxy_set_header X-Real-IP $remote_addr;

# The value of $remote_port is the client PORT
proxy_set_header X-Real-PORT $remote_port;

# This is especially useful if there are multiple proxy servers. $proxy_add_x_forwarded_for stores all the ip addresses the request chain of the client and of the previous proxy servers.
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

# Identify the protocol (HTTP or HTTPS)
proxy_set_header X-Forwarded-Proto $scheme;

```

### **Load Balancing**
> Setting up multiple servers helps serving a great amount of loads and prevents servers from being slowed down and crashing. Whenever there are more than one web server, we have to define how the incoming network traffic is distributed to those servers, and that's what we call load balancing.

 Nginx provides the following load balancing methods:
- Round Robin
- Weighted Round Robin
- Hash (Using hash and ip hash ensures that the client be directed to the same server every time (unless the server is down).)
- IP Hash
- Least Connections
- Least Time
- Random

```nginx
http {

...

    # Round robin (Default method): 
        # Requests are distributed to each server in turn.
    upstream apps {
        server 127.0.0.1:8001;
        server 127.0.0.2:8002;
        server 127.0.0.3:8003;
    }

# or

    # Weighted round robin: 
        # Distribute requests proportionally.
    upstream apps {
        server 127.0.0.1:8001 weight= 1;
        server 127.0.0.2:8002 weight= 2;
        server 127.0.0.3:8003 weight= 3;
    }

# or

    # Hash: 
        # Distribute requests based on the defined key
    upstream apps {
        hash $remote_addr consistent;   # Hash the full ip address 
        server 127.0.0.1:8001 weight= 1; 
        server 127.0.0.2:8002 weight= 2;
        server 127.0.0.3:8003 down;   # marked with down for temporary suspension
    }

# or

    # IP hash: 
        # Distribute requests based on client ip addresses (hash only the first three octets of the client ipv4 address and entire ipv6 address)
    upstream apps {
        ip_hash;
        server 127.0.0.1:8001 weight= 1; 
        server 127.0.0.2:8002 weight= 2;
        server 127.0.0.3:8003 down;     # marked with down for temporary suspension
    }

# or

    # Least connection: 
        # Pass the requests to the server with the least number of connections
    upstream apps {
        least_conn;
        server 127.0.0.1:8001;
        server 127.0.0.2:8002;
        server 127.0.0.3:8003;
    }

# or

    # Least time: 
        # Pass the requests to the server with the least response time and least number of connections
    upstream apps {
        least_time header;   # count the time of receive response header
        
        # or

        least_time last_byte;   # count the time of receive full response

        server 127.0.0.1:8001;
        server 127.0.0.2:8002;
        server 127.0.0.3:8003;
    }

# or

    # Random: 
        # Randomly distribute the request to server
    upstream apps {
        random;

        # or
        
        random two [METHOD]; # Randomly pick two servers and distribute the request to one by using the least connection method as the default method (or using least time by specifying least_time=header or least_time=last_byte)

        server 127.0.0.1:8001 weight= 1;
        server 127.0.0.2:8002 weight= 2; 
        server 127.0.0.3:8003 weight= 3;
    }
}
...
```
### **Cache**

```nginx
http {

    # Set the cache path for the proxy server
    # Can also be set in server or location context
    proxy_cache_path /var/cache/nginx levels=1:2 use_temp_path=on keys_zone=my_custom_cache:10m inactive=60m max_size=50g min_free=500m;
    # "levels": defines the hierarchy level of cache directory
    # "use_temp_path": put the temporary files in the directory set by proxy_temp_path directive. If the sue_temp_path is set to "off", the temporary files will be put in the cache directory.
    # "keys_zone": defines name and size of the shared memory zone
    # "inactive": cached data will be removed if not accessed during the time limit defined (is set to 10 minutes by default).
    # "max_size": defines the maximum cache size
    # "min_free": defines the minimum free space

    server {
    
        ...

        # Use the cache zone "my_custom_cache" defined by keys_zone parameter in proxy_cache_path directive
        # or set it to off to disable the cache function
        proxy_cache my_custom_cache;

        # Specify the directory for storing the temporary files received
        # the last two integer set the hierarchy 
        proxy_temp_path  /spool/nginx/proxy_temp 1 2;

        # Cache are stored as a key-value pair in which the request is the key and the response the value. This directive allows you to define the key for the cache
        proxy_cache_key $scheme$proxy_host$request;

        # Specify under what circumstances a stale cached response is used
        proxy_cache_use_stale error timeout invalid_header http_403 http_503;
        
        # By default, only responses with status code 200, 301, 302 will be cached. 
        # You may specify what responses to be cached and the caching time with this directive
        proxy_cache_valid 404 5m;
        proxy_cache_valid 200 302 30m;
        proxy_cache_valid 301 1h;

        # Specify what methods to be cached
        proxy_cache_methods GET HEAD POST;

        # Set how many times the same request should be made for caching the response
        proxy_cache_min_uses 5;

        location {

            ...

    }
}
```
---

## References:

1. [Nginx Official - Beginners guide](http://nginx.org/en/docs/beginners_guide.html) 
2. [Nginx Official - Control Nginx](http://nginx.org/en/docs/control.html)
3. [Nginx Official - Full Example Config](https://www.nginx.com/resources/wiki/start/topics/examples/full/?msclkid=e5a31490d0e911ec9ce439af1f484f63)
4. [Nginx Official - Upstream module](https://nginx.org/en/docs/http/ngx_http_upstream_module.html)
5. [Nginx Official - Load Balancing](https://www.nginx.com/resources/glossary/load-balancing/)
6. [Nginx - Basic configuration (Chinese)](https://blog.hellojcc.tw/nginx-beginner-tutorial/)
7. [Nginx basic concepts (Chinese)](https://medium.com/starbugs/web-server-nginx-2-bc41c6268646)
8. [Setting up an Nginx reverse proxy](https://linuxize.com/post/nginx-reverse-proxy/#:~:text=To%20configure%20Nginx%20as%20a%20reverse%20proxy%20to,an%20optional%20port%20and%20URI%20as%20an%20address.)
9. [Nginx Config generator](https://www.digitalocean.com/community/tools/nginx)


#### Brought to you by Suen Lam ;)
