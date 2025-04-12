FROM alpine

# for stunnel support uncomment all commands on this file and run.sh and add stunnel and openssl into the apk command
RUN apk update
RUN apk add nodejs gcc g++ cmake make tmux dropbear bash linux-headers

#WORKDIR /etc/stunnel
#RUN openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 3650 -nodes -subj "/C=AR/ST=Tierra del Fuego/L=Usuahia/O=Common LLC/OU=Common LLC/CN=localhost"
#RUN cat key.pem cert.pem > stunnel.pem

WORKDIR /workdir

COPY badvpn-src/ ./badvpn-src
COPY proxy3.js ./
#COPY stunnel.conf /etc/stunnel
COPY run.sh ./

WORKDIR /workdir/badvpn-src
RUN mkdir -p build
WORKDIR /workdir/badvpn-src/build
RUN cmake .. -DBUILD_NOTHING_BY_DEFAULT=1 -DBUILD_TUN2SOCKS=1 -DBUILD_UDPGW=1 -DCMAKE_BUILD_TYPE=Release
RUN make -j2 install

WORKDIR /workdir
RUN rm -rf badvpn-src
RUN echo -e "/bin/false\n/usr/sbin/nologin\n" >> /etc/shells
RUN adduser -DH test -s /bin/false
RUN echo -e "test:qweasdzxc" | chpasswd
RUN chmod +x /workdir/run.sh

EXPOSE 8080

CMD ./run.sh
