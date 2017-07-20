FROM alpine:3.6
MAINTAINER  Yourtion Guo <yourtion@gmail.com>

ADD ./user-config.json /user-config.json
WORKDIR /shadowsocksr-manyuser/shadowsocks
EXPOSE 8080-8089

RUN apk --no-cache add python libsodium wget
RUN wget -qO- --no-check-certificate https://github.com/shadowsocksr/shadowsocksr/archive/manyuser.tar.gz | tar -xzf - -C /

CMD python server.py -c /user-config.json
