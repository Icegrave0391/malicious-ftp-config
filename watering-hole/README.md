## Scenario

The victim user tries to download a series of packages for his environment via `wget`. However, one of the source URLs has been compromised by the attacker, in which the original package has been replaced with a malware `mal-package`. 

After execution, `mal-package` copies sensitive files from the victim's original directory to a shared folder, which provides web sources for the shared nginx server on victim's PC.

Finally, the attack steals those files by connecting and requesting to victim's server.

### Similar



## Implementation

### sources

Ubuntu 16.04: http://mirror.nus.edu.sg/ubuntu-ISO/16.04/    
nginx 1.4.0: http://nginx.org/download/nginx-1.20.2.tar.gz    
wget 1.20.3: https://ftp.gnu.org/gnu/wget/wget-1.20.3.tar.gz

### deploy

**Nginx server (Victim IP1)**

* build and install
```
tar xzvf nginx-1.20.2.tar.gz && cd nginx-1.20.2
./configure --prefix=/home/chuqi/local/nginx --error-log-path=PATH=/home/chuqi/log/nginx/error.log
make && make install
```

* configure file

Download the `nginx.conf` file in this github dir.
```
mv nginx.conf /home/chuqi/local/nginx/conf/nginx.conf
```

* web sources

Download the folder `web-source` under this github directory.
```
cp web-source /home/chuqi
```

* deploy
```
help: /home/chuqi/local/nginx/sbin/nginx -h
start: /home/chuqi/local/nginx/sbin/nginx
stop: /home/chuqi/local/nginx/sbin/nginx -s stop
```

**Exploit (Attacker IP2)**    

Access victim's web-server on attacker's browser:
```
IP1:8080/data/xxx
IP1:8080/images/xxx
```
