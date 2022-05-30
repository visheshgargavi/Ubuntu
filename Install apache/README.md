# Installing Apache Webserver without root user and sudo with all the dependencies 
https://httpd.apache.org/docs/2.4/install.html

# PRE-REQUISITES

What is make?

The Linux make command is used to build and maintain groups of programs and files from the source code. In Linux, it is one of the most frequently used commands by the developers. It assists developers to install and compile many utilities from the terminal. Further, it handles the compilation process of the sizeable projects. It saves the compilation time.

The make command takes targets as arguments. These arguments are specified in 'Makefile.' The makefile contains the targets as well as associated actions related to these targets.

```hcl
sudo apt-get update
sudo apt update
sudo apt-get install make
sudo apt install gcc
```

Create a user:
```hcl
useradd username
passwd username
```

Switch to otheruser:
```hcl
su - username
```

Working Dir:

Always work in /home/username directory because it is the only directory in other user that has significant amount of power. If you used any other directory than you have to use sudo that won't be a efficient solution when we are working for other client they neither gives us the root power nor sudo.
```hcl
cd /home/username
pwd (to check the present working directory)
```

Downloading httpd:
```hcl
wget https://dlcdn.apache.org/httpd/httpd-2.4.53.tar.gz
gzip -d httpd-2.4.53.tar.gz
tar xvf httpd-2.4.53.tar
```

# Installing Dependencies:

### APR
https://apr.apache.org/

https://apr.apache.org/download.cgi
```hcl
wget https://dlcdn.apache.org//apr/apr-1.7.0.tar.gz
gzip -d apr-1.7.0.tar.gz
tar xvf apr-1.7.0.tar
cd apr-1.7.0
```

To Search in configure file using vim editor
```hcl
vim configure
Press ":" + "/" + "$cfgfile" (go to the line and edit the text($RM "$cfgfile") to)
$RM -f "$cfgfile"
```
```hcl
./configure --prefix=/home/username/opt/apr
make
make install
```

### Expat library
```hcl
wget https://github.com/libexpat/libexpat/releases/download/R_2_4_8/expat-2.4.8.tar.gz
gzip -d expat-2.4.8.tar.gz
tar xvf expat-2.4.8.tar
cd expat-2.4.8
./configure --prefix=/home/username/opt/expat
make
make install
```

### APR-UTIL
```hcl
wget https://dlcdn.apache.org//apr/apr-util-1.6.1.tar.gz
gzip -d apr-util-1.6.1.tar.gz
tar xvf apr-util-1.6.1.tar
cd apr-util-1.6.1
./configure --prefix=/home/username/opt/apr-util --with-expat=/home/username/opt/expat --with-apr=/home/username/opt/apr
make
make install
```

### PCRE
http://www.pcre.org/
```hcl
wget https://github.com/PCRE2Project/pcre2/releases/download/pcre2-10.40/pcre2-10.40.tar.gz
gzip -d pcre2-10.40.tar.gz
tar xvf pcre2-10.40.tar
./configure --prefix=/home/username/opt/pcre
make
make install
```

### Installing httpd
```hcl
cd httpd-2.4.53
./configure --prefix=/home/username/pkg/apache --with-apr=/home/ubuntu/opt/apr --with-pcre=/home/ubuntu/opt/pcre/bin/pcre2-config --with-apr-util=/home/ubuntu/opt/apr-util
make
make install
```

### Starting apache webserver
```hcl
cd /home/username/pkg/apache/conf
vim httpd.conf
"Set Listner to higher port eg. LISTEN 50000"
cd /home/username/pkg/apache/bin
./apachectl -k start
curl http://1.2.3.4:50000
```