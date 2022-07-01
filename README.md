To run this
===============================

1) go to the db-dockerfile directory
2) build and tag the image with "docker build . -t name-of-your-image". I used paoliniluis/app-db-with-certs:postgres-14.3-alpine
3) then go to the docker-compose file and include the name of the image you just built and tagged in line 16
4) do a docker-compose up (things won't run, this is only to extract the certificates from the db container)
5) do the following (to copy the certificates):
   1) docker cp postgres-app-db-with-certs:/var/lib/postgresql/certs/ca.crt metabase-dockerfile/app-db/ca.crt
   2) docker cp postgres-app-db-with-certs:/var/lib/postgresql/certs/client.crt metabase-dockerfile/app-db/client.crt
   3) docker cp postgres-app-db-with-certs:/var/lib/postgresql/keys/client.der metabase-dockerfile/app-db/client.der
6) now kill the containers and remove the image that was created for the metabase container (docker rmi postgres-with-certs_metabase-with-certs)
7) now do docker-compose up again (this will rebuild the Metabase image with the client certificates inside and will authenticate with those)

Pending
===

- put some more automation to this