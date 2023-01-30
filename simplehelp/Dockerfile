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
