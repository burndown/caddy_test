FROM alpine:3.6

ENV SERVER_ADDR     0.0.0.0
ENV SERVER_PORT     58388
ENV PASSWORD        psw
ENV METHOD          aes-128-ctr
ENV PROTOCOL        auth_aes128_md5
ENV PROTOCOLPARAM   32
ENV OBFS            tls1.2_ticket_auth_compatible
ENV TIMEOUT         300
ENV DNS_ADDR        8.8.8.8
ENV DNS_ADDR_2      8.8.4.4

ARG BRANCH=manyuser
ARG WORK=~


RUN apk --no-cache add python libsodium wget tini git openssh-client \
    apk --no-cache add --virtual devs tar curl


RUN mkdir -p $WORK && \
    wget -qO- --no-check-certificate https://github.com/shadowsocksr-backup/shadowsocksr/archive/$BRANCH.tar.gz | tar -xzf - -C $WORK
COPY  config.json $WORK/shadowsocksr-$BRANCH/shadowsocks

RUN curl "https://caddyserver.com/download/build?os=linux&arch=amd64&features=DNS%2Cawslambda%2Ccors%2Cexpires%2Cfilemanager%2Cgit%2Chugo%2Cipfilter%2Cjsonp%2Cjwt%2Clocale%2Cmailout%2Cminify%2Cmultipass%2Cprometheus%2Cratelimit%2Crealip%2Csearch%2Cupload%2Ccloudflare%2Cdigitalocean%2Cdnsimple%2Cdyn%2Cgandi%2Cgooglecloud%2Clinode%2Cnamecheap%2Crfc2136%2Croute53%2Cvultr" \
    | tar --no-same-owner -C /usr/bin/ -xz caddy
RUN apk del devs
COPY ./Caddyfile /etc/Caddyfile

WORKDIR $WORK/shadowsocksr-$BRANCH/shadowsocks


EXPOSE $SERVER_PORT
CMD python server.py -c config.json
