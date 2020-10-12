# 2 way SSL with spring boot

Example client (nt-gateway) and service (nt-ms) code to show how to get 2 way SSL setup with self signed certificate.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. 

### Prerequisites

* Java 1.8
* Spring boot 2.1.2
* Java keytool utility


### Installing

Download nt-gateway and nt-ms projects in to your STS or Eclipse workspace. 

============
Follow below link

https://medium.com/@niral22/2-way-ssl-with-spring-boot-microservices-2c97c974e83
https://www.baeldung.com/x-509-authentication-in-spring-security


============
I have created same keystore and trustore using OPENSSL and that can be used int both client and server side

 https://www.baeldung.com/x-509-authentication-in-spring-security
 
 openssl req -x509 -sha256 -days 3650 -newkey rsa:4096 -keyout rootCA.key -out rootCA.crt
	
 openssl req -new -newkey rsa:2048 -nodes -keyout server.key -out server.csr
 
#Create a file cert.conf with below content and use it with "-extfile" : ---->>
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
subjectAltName = @alt_names
[alt_names]
DNS.1 = localhost
DNS.2 = 127.0.0.1
#----

openssl x509 -req -CA rootCA.crt -CAkey rootCA.key -in server.csr -out server.crt -days 365 -CAcreateserial -extfile cert.conf

openssl pkcs12 -export -out server.p12 -name "server" -inkey server.key -in server.crt

keytool -importkeystore -srckeystore server.p12 -srcstoretype PKCS12 -destkeystore keystore.jks -deststoretype pkcs12

keytool -keystore truststore.jks -importcert -file server.crt -alias server -storepass password


