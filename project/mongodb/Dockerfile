FROM mongo:7.0
WORKDIR /docker-entrypoint-initdb.d
COPY master-data.js .


## CMD ["mongod"] is run by default from the mongo image. So, it runs when container is created by default