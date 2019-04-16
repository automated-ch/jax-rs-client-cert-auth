# jax-rs webservice with cert-client authentification on Payara 4.1.2.181

This project demonstrates how to setup a jax-rs webservice to which the clients authenticate with a client certificate.

## create a client certificate with keytool

`keytool -genkey -v -alias linux-utr-client -keyalg RSA -storetype PKCS12 -keystore client_keystore.p12 -storepass changeit -keypass changeit`

after this you should have the folloing file in the directory where the above command was executed.

client_keystore.p12

After that we will export the client cert to a file called client_keystore.cer

`keytool -export -alias linux-utr-client -keystore client_keystore.p12 -storetype PKCS12 -storepass changeit -rfc -file client_keystore.cer`

##Install the client certificate in payara server
Important note: to make this example work properly on payara 4.1.2.181 you need to ad the manager group to the certificate realm in the servers admin console.

backup following files:
* cacerts.jks to cacerts.backup.jks.
* keystore.jks to keystore.backup.jks

import client.cer to the payara truststore:

`keytool -import -v -trustcacerts -alias client-alias -file client_keystore.cer -keystore /data/dev/payara5/glassfish/domains/domain1/config/cacerts.jks -keypass changeit -storepass changeit`

List all certificate in the servers keystore:

`keytool -list -v  -keystore /data/dev/payara-5.1.191/glassfish/domains/domain1/config/cacerts.jks -storepass changeit`




## setup in web.xml
the common name of the certificate should match the principal name

### references
https://docs.oracle.com/javaee/7/tutorial/security-advanced002.htm
