# nextcloud

Documentation for the nextcloud installation

## Installation
As we want to have the nextcloud server running on the same server as Kodkollektivet.se we will do a manual installation so we can change the apache settings etc.

* Dependencies
  We will need to some dependencies: 
  ``` 
  apt-get install apache2 mariadb-server libapache2-mod-php7.0
  apt-get install php7.0-gd php7.0-json php7.0-mysql php7.0-curl php7.0-mbstring
  apt-get install php7.0-intl php7.0-mcrypt php-imagick php7.0-xml php7.0-zip
  ``` 
  "This installs the packages for the Nextcloud core system. libapache2-mod-php7.0 provides the following PHP extensions: bcmath bz2 calendar Core ctype date dba dom ereg exif fileinfo filter ftp gettext hash iconv libxml mhash openssl pcre Phar posix Reflection session shmop SimpleXML soap sockets SPL standard sysvmsg sysvsem sysvshm tokenizer wddx xmlreader xmlwriter zlib. If you are planning on running additional apps, keep in mind that they might require additional packages. See Prerequisites for manual installation for details."

* Nextcloud download
  We will download the nextcloud from the website https://download.nextcloud.com/server/releases/nextcloud-x.y.z.tar.bz2 where x.y.z is the version number:
  ```
  wget https://download.nextcloud.com/server/releases/nextcloud-x.y.z.tar.bz2
  ```
  You should check the sha256 sum of the file to ensure integrity. This is done by downloading the sha256 checksum.
  ```
  wget https://download.nextcloud.com/server/releases/nextcloud-x.y.z.tar.bz2.sha256
  sha256sum -c nextcloud-13.0.1.tar.bz2.sha256 < nextcloud-13.0.1.tar.bz2
  nextcloud-13.0.1.tar.bz2: OK
  done!
  ```
* Unpack and move Nextcloud to apache
  To unpack the .tar file use the first command in the comming code block. The rest of the commands will move the Nextcloud folder to the document root of our apache2 installation, and then remove the files in our home directory.
  ```
  tar -xjf nextcloud-13.0.1.tar.bz2
  cp -r nextcloud /var/www/
  rm -r nextcloud nextcloud-13.0.1.tar.bz2 nextcloud-13.0.1.tar.bz2.sha256
  ```
  Then we need to create our apache config file. This is done by creating the file `/etc/apache2/sites-available/nextcloud.conf` and the insert the following code to it: 
  ```
  Alias /nextcloud "/var/www/nextcloud/"

  <Directory /var/www/nextcloud/>
      Options +FollowSymlinks
      AllowOverride All

  <IfModule mod_dav.c>
      Dav off
      
  </IfModule>
      SetEnv HOME /var/www/nextcloud
      SetEnv HTTP_HOME /var/www/nextcloud

  </Directory>
```
