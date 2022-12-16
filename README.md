

# Generate CA

Edit openssl-ca.cnf, then

``` sh
openssl genrsa -out mongodb.ca.key 4096
openssl req -new -x509 -days 3650 -key mongodb.ca.key -out mongodb.ca.pem -config openssl-ca.cnf
```

# Generate a Server Certificate

Edit openssl-server.cnf add add DNS names and SANs, then

``` sh
openssl genrsa -out mongodb-server1.key 4096

openssl req -new -key mongodb-server1.key -out mongodb-server1.csr -config openssl-server.cnf

openssl x509 -sha256 -req -days 3650 -in mongodb-server1.csr -CA mongodb.ca.pem \
    -CAkey mongodb.ca.key -CAcreateserial -out mongodb-server1.crt -extfile openssl-server.cnf -extensions v3_req
    
cat mongodb-server1.crt > mongodb.key
cat mongodb-server1.key >> mongodb.key
```


Use mongodb.ca.pem (CA certificate) and mongodb.key (certificate and key).

See: https://www.mongodb.com/docs/manual/appendix/security/appendixA-openssl-ca/
