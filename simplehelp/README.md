# How To Manage SimpleHelp in Docker

## Building The Image
```dockerfile
FROM ubuntu:18.04

LABEL maintainer="campbell.justin.tyler@gmail.com"
LABEL version="5.1.9"
LABEL description="SimpleHelp 5.1.8"

EXPOSE 22 80 443

WORKDIR /opt

RUN apt-get update && apt-get install -y wget && apt-get purge && rm -rf /var/lib/apt/lists/* && apt-get clean
# For other releases, see: https://simple-help.com/previous-releases
RUN wget https://justincampbell.dev/SimpleHelp-linux-amd64.tar.gz
RUN tar -zxf SimpleHelp-linux-amd64.tar.gz
RUN rm SimpleHelp-linux-amd64.tar.gz

WORKDIR /opt/SimpleHelp

RUN sed -i 's/&//g' serverstart.sh

CMD ["sh", "serverstart.sh"]
```

```bash
docker build -t simplehelp:5.1.8 .
```

## Running the Container (docker-compose)
```yml
version: '3.3'
services:
    simplehelp:
        container_name: simplehelp
        ports:
            - '8082:80'
            - '8444:443'
        volumes:
            - ./configuration/:/opt/SimpleHelp/configuration/
        image: 'simplehelp:5.1.8'
```

## Running the Container (docker run)
```bash
docker run \
    --name simplehelp \
    -p 8082:80 \
    -p 8444:443 \
    -v $(PWD)/configuration/:/opt/SimpleHelp/configuration/ \
    --detach \
    simplehelp:5.1.8
```

## Networking
justincampbell.dev:80 -> router:80 -> server:8082 -> container:80
justincampbell.dev:443 -> router:443 -> server:443 -> Nginx Reverse Proxy -> localhost:8443 -> container: 443

## Backup and Restore
All configuration is stored in ./configuration.
