FROM postgres:14.3-alpine

WORKDIR /var/lib/postgresql/

COPY pg_hba.conf /var/lib/postgresql/

RUN apk add openssl &&\
    mkdir keys certs &&\
    chmod og-rwx keys certs &&\
    openssl req -new -x509 -days 365 -nodes -out certs/ca.crt -keyout keys/ca.key -subj "/CN=root-ca" &&\
    mkdir pgconf &&\
    cp certs/ca.crt pgconf/ca.crt &&\
    openssl req -new -nodes -out server.csr -keyout pgconf/server.key -subj "/CN=postgres-app-db-with-certs" &&\
    openssl x509 -req -in server.csr -days 365 -CA certs/ca.crt -CAkey keys/ca.key -CAcreateserial -out pgconf/server.crt &&\
    chmod og-rwx pgconf/* && chown 70 pgconf/* &&\
    openssl req -new -nodes -out client.csr -keyout keys/client.key -subj "/CN=metabase" &&\
    chmod og-rwx keys/* &&\
    openssl x509 -req -in client.csr -days 365 -CA certs/ca.crt -CAkey keys/ca.key -CAcreateserial -out certs/client.crt && \
    openssl rsa -inform pem -in keys/client.key -outform der -out keys/client.der
