#
# Install/Renew SSL Certificate In Tomcat on Linux

### To renew SSL Certificate in Tomcat, you can follow the steps below.

#### check tomcat status
~~~
systemctl status tomcat.service
~~~
#### check that Java is installed
~~~
java --version
~~~
#### Step 1. Generating a Keystore and CSR

#### Navigate to the default OpenSSL SSL directory
~~~
cd /etc/ssl
~~~
#### Take Backup the old keystore file and SSL Cerfificates files with server.xml file


#### Create the Keystore:
~~~
keytool -genkey -keysize 2048 -keyalg RSA -alias server -keystore nsderp.navy.mil.bd.jks
~~~
#### Enter the Keystore Password: (Make sure it is something you remember as you will need the password to access your keystore later)

#### You will then get a prompt asking you to input the following details regarding your CSR:

What is your First and Last Name: nsderp.navy.mil.bd [Enter your domain name]

What is the name of your organizational unit: Bangladesh Navy [Enter the organizational unit]

What is the name of your organization: NSD 

What is the name of your city or locality: CHITTAGONG

What is the name of your State or Province: CHITTAGONG 

What is the two-letter country code for this unit: BD 

#### Confirm that all the above stated information is correct by typing: yes.

#### After you hit Enter, your Keystore should be generated in the selected directory.

#### Generate the CSR: 
~~~
keytool -certreq -keyalg RSA -alias server -file nsderp.navy.mil.bd.csr -keystore nsderp.navy.mil.bd.jks
~~~
#### Check the CSR text:
~~~
openssl req -text -in nsderp.navy.mil.bd.csr 
~~~
#### view CSR file:
~~~
cat nsderp.navy.mil.bd.csr
~~~
#### Step 2. Order the SSL Certificate:
Now we need to order an SSL Certificate from global ssl providers.

we need to the CSR file when purchase ssl certificate.

#### Select Server Type as your web server enginee:
Apache

Nginx

Java Based Server

etc

#### Here we need to select the Authentication Method to validate your domain name.

email

#### Download certificate file from ssl provider portal:
Now collect/download our certificate in .cer format as individual files. 

#### Upload these certificate files to webserver via FTP or SCP.
~~~
scp -rv [file-name.type] root@ourwebserverip:/etc/ssl	[linux-paste-directory]
~~~
#### Unzip certificates zip file:
~~~
unzip archive.zip
~~~
#### Step 3. Import the SSL Certificates :

#### Note: Import the 3 certificates by order:
1st import (TrustedRoot.crt)

2nd import (DigiCertCA.crt)

3rd import (ourdomain.crt)

#### import your main certificate:
~~~
keytool -import -trustcacerts -alias root -keystore nsderp.navy.mil.bd.jks -file /etc/ssl/cert2023/TrustedRoot.crt
~~~
#### import any intermediate certificates:
~~~
keytool -import -trustcacerts -alias intermed -keystore nsderp.navy.mil.bd.jks -file /etc/ssl/cert2023/DigiCertCA.crt
~~~
#### import the root certificate:
~~~
keytool -import -trustcacerts -alias server -keystore nsderp.navy.mil.bd.jks -file /etc/ssl/cert2023/nsderp_navy_mil_bd.crt
~~~

#### Step 4. Configure the server.xml file:

#### Navigate to this directory:
By default Tomcat Installation location are:
~~~
cd /opt/tomcat/conf
~~~
#### edit the server.xml file
~~~
nano server.xml
~~~
#### After that, scroll down to the connector config part and add the following lines to enable HTTPS on your Tomcat Server.
~~~
  <Connector port="8443" protocol="HTTP/1.1"
  connectionTimeout="20000"
  redirectPort="8443"
  SSLEnabled="true"
  scheme="https"
  secure="true"
  sslProtocol="TLSv1.2"
  keystoreFile="/etc/ssl/yourdomain.com.jks"
  keystorePass="Your-Password" />
~~~

#### Restart Tomcat
~~~
systemctl restart tomcat8	[replace our tomcat version tomcat8,9]
~~~
~~~
systemctl status tomcat8
~~~
#### Restart Tomcat without systemctl command
~~~
cd /U01/apache-tomcat-7.0.63/bin/
~~~
~~~
./startup.sh
~~~
#### Step 5. Check the SSL Installation:
Go to> www.ssllabs.com/ssltest/

Paste: ourwebserver URL		[https://www.yourdomain.com]

#### Complete Installation/Renew SSL Certificate

#
# Error and Troubleshoot on Tomcat SSL


#### Error Message
keytool error: java.lang.Exception: Failed to establish chain from reply

#### Cause
Old certificate exist and Root/Intermediate certificates have not been imported properly or in the correct order.

#### Solution
If Root and/or Intermediate certificates have already been imported, remove them.

1. View the pervious keystore/ssl
~~~
keytool -list -keystore nsderp.navy.mil.bd.jks [select your keystore .jks file]
~~~
2. Run the following command:
#### note: In server.xml file has locate our perviours alias name. 
~~~
keytool -delete -alias mydomain -keystore nsderp.navy.mil.bd.jks [select your keystore .jks file]
~~~
#
