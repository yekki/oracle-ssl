# Client Side
## Generate a private key

    openssl genrsa -out diagclientCA.key 2048

## Create a x509 certificate

    openssl req -x509 -new -nodes -key diagclientCA.key \
                -sha256 -days 1024 -out diagclientCA.pem

## Create PKCS12 keystore from private key and public certificate.

    openssl pkcs12 -export -name client-cert \
                   -in diagclientCA.pem -inkey diagclientCA.key \
                   -out clientkeystore.p12

## Convert a PKCS12 keystore into a JKS keystore

	keytool -importkeystore -srckeystore clientkeystore.p12 -destkeystore client.keystore -deststoretype pkcs12 -alias client-cert

## Import a server's certificate to the client's trust store.

    keytool -import -alias server-cert -file diagserverCA.pem \
            -keystore client.truststore

## Import a client's certificate to the client's trust store.

    keytool -import -alias client-cert -file diagclientCA.pem \
            -keystore client.truststore




# Server Side
## Generate a private RSA key

	openssl genrsa -out diagserverCA.key 2048

## Create a x509 certificate

	openssl req -x509 -new -nodes -key diagserverCA.key -sha256 -days 1024 -out diagserverCA.pem

## Create a PKCS12 keystore from private key and public certificate.

	openssl pkcs12 -export -name server-cert \
                   -in diagserverCA.pem -inkey diagserverCA.key \
                   -out serverkeystore.p12

## Convert PKCS12 keystore into a JKS keystore
	
	keytool -importkeystore -srckeystore serverkeystore.p12 -destkeystore server.keystore -deststoretype pkcs12 -alias server-cert

## Import a client's certificate to the server's trust store.

    keytool -import -alias client-cert \
            -file diagclientCA.pem -keystore server.truststore

## Import a server's certificate to the server's trust store.

    keytool -import -alias server-cert \
            -file diagserverCA.pem -keystore server.truststore


# Tools

	keytool -v -list -keystore server.keystore
	keytool -v -list -keystore client.keystore
