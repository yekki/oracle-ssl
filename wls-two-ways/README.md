# Server

	java utils.CertGen -selfsigned -certfile server.cer -keyfile server.key -keyfilepass password -cn "localhost" -noskid
	java utils.ImportPrivateKey -keystore identity_store.jks -storepass password -keypass password -alias server-cert -certfile server.cer.pem -keyfile server.key.pem -keyfilepass password
	keytool -import -trustcacerts -alias trustself-cert -keystore trust_store.jks -file server.cer.der -keyalg RSA

# Client

	openssl genrsa -out client.key 2048
	openssl req -x509 -new -nodes -key client.key -sha256 -days 3650 -out client.pem
    openssl pkcs12 -export -name client-cert -in client.pem -inkey client.key -out client.p12
    keytool -import -trustcacerts -alias trustclient-cert -keystore trust_store.jks -file client.pem -keyalg RSA
    keytool -importkeystore -srckeystore client.p12 -destkeystore client_store.jks -deststoretype pkcs12 -alias client-cert


# Utils
	

	keytool -v -list -keystore identity_tore.jks
	keytool -v -list -keystore identity_tore.jks
	keytool -v -list -keystore client_store.jks

	http://weblogic-wonders.com/weblogic/2013/09/24/mutual-authentication-weblogic-server/