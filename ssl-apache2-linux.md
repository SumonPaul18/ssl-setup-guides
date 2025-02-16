## Install SSL on Apache2 in Linux


#### To install/renew SSL Certificate in Apache2, you can follow the steps below.

#### First we browse the website and check ssl cert have active or not expried.
#### Go to the server, and check apache2 service status.
#### Find apache2 configuration files where have ssl.conf file and check in the file sslengine on/off, we can see website source file location, certificate locations. 

#### check apache status
~~~
systemctl status httpd
~~~
#### By default apache2 ssl.conf file in exist
#### apache2 called httpd. 
~~~
cat /etc/httpd/conf.d/ssl.conf
~~~

#### Generating a Private key and CSR


#### Two ways can generate private key and csr.
1. Online
   
2. Command Line

#### 1: Generate CSR from Online:
#### From a browser search [ Generate csr from online ] we can see some online csr generated website. Then choose any trusted website and generate csr with private key. 

#### Bellow listing some trusted csr generated link.
1. https://www.ssl.com/online-csr-and-key-generator/

2. https://www.ssltrust.com.au/ssl-tools/generate-csr

3. https://decoder.link/csr_generator

Access any link and filup there form as your domain information. Then click generate button and download or copy paste csr & key file.

#### 2: Generate CSR from Command Line:

#### This way we need to access that server terminal and run a single line command for generate private key and csr.

#### Create a Directory, where store Private key and CSR file.
~~~
mkdir -p /etc/httpd/ssl/
chmod 700 /etc/httpd/ssl/
~~~
#### Navigate the directory and run bellow command.
~~~
cd /etc/httpd/ssl/
~~~
~~~
openssl req -new -newkey rsa:2048 -nodes -keyout exampledomain.com.key -out exampledomain.com.csr
~~~
#### Enter the following CSR details when prompted:
#### Common Name:www.paulco.xyz - (website address) This is Too Important. 
```
Common Name(CN): www.paulco.xyz
Organization: Computing Technology
Organization Unit (OU): IT
City or Locality: Dhaka
State or Province: Dhaka
Country: BD
```
#### We can see two file in this diretory
~~~
ls -ln
~~~
> exampledomain.com.key
> exampledomain.com.csr

#### Now we need to the .csr file for baught paid SSL.

#### Create a backup the .key file as it will be required later when installing your SSL certificate in apache2.

++++++++++++++++++++++++++++++++++++++++++++++++++++++
+ Download certificate file from ssl provider portal +
++++++++++++++++++++++++++++++++++++++++++++++++++++++
#upzip the .zip file in we got 3 files.
#Note: The 3 certificates are.

1. TrustedRoot.crt
2. DigiCertCA.crt
3. Yourdomain.crt

#Put this certificates file on /etc/httpd/ssl/ location.

copy TrustedRoot.crt DigiCertCA.crt Yourdomain.crt /etc/httpd/ssl/


#Some Times need to that.
#Concatenate the primary and intermediate certificates

#cat Yourdomain.crt DigiCertCA.crt TrustedRoot.crt  >> sslbundle.crt

+++++++++++++++++++++++++++++++++++++++
+ Edit the Apache2 virtual hosts file +
+++++++++++++++++++++++++++++++++++++++
#Find to .conf file for locate ssl configuration file.

find ssl.conf

find / -name "ssl.conf"

#or 

locate ssl.conf

locate .conf

#Default VirtualHost file is.

<VirtualHost *:443>
DocumentRoot /var/www/html
ServerName www.paulco.xyz
SSLEngine on
SSLCertificateFile /etc/httpd/ssl/certificate.crt
SSLCertificateKeyFile /etc/httpd/ssl/private.key
SSLCertificateChainFile /etc/httpd/ssl/nssd_navy_mil_bd.crt
SSLCACertificateFile /etc/httpd/ssl/My_CA_Bundle.ca-bundle
</VirtualHost>

+++++++++++++++++++++
+ Redirect to HTTPS +
+++++++++++++++++++++

#To redirect traffic to become SSL encrypted, go ahead and open a file ending in .conf in the /etc/httpd/conf.d directory:

nano /etc/httpd/conf/httpd.conf

<VirtualHost *:80>
        ServerName www.example.com
        Redirect "/" "https://www.paulco.xyz/"
</VirtualHost>

#Once completed, save and close the file.

+++++++++++++++++++++++++++++
+ Systax Check and Restart  +
+++++++++++++++++++++++++++++

apache2ctl -t

apachectl configtest

service apache2 restart

systemctl restart httpd

#service httpd restart

#Apache service start/stop
apachectl stop
apachectl start

service httpd status

+++++++++++++++++++++++++
+ Some Basic Operation  +
+++++++++++++++++++++++++

#/etc/apache2/sites-enabled/your domain.conf

netstat -tupan | grep -i http

netstat -tulpn | grep --colorb:80

++++++++++++++++++++++++++++++ End ++++++++++++++++++++++++++++++
