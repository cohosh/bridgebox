FROM python:3.8

ARG USER_ID
ARG GROUP_ID

RUN apt-get update && apt-get install -y build-essential openssl python3 python3-dev \
  python3-setuptools sqlite3 gnupg2 libgeoip-dev geoip-database sudo python3-pip flog

RUN pip install Babel==2.8.0 beautifulsoup4==4.9.1 html5lib==1.0.1 Mako==1.1.2 pycryptodome==3.9.7 Twisted==20.3.0 ipaddr==2.2.0 Pillow==7.1.2 pyOpenSSL==19.1.0 pygeoip==0.3.2 qrcode==6.1 service_identity==18.1.0 stem==1.8.0 zope.interface==5.1.0 numpy==1.18.5

RUN addgroup --gid $GROUP_ID bridgedb && adduser --uid $USER_ID --gid $GROUP_ID bridgedb && adduser bridgedb sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

RUN openssl genrsa -des3 -passout pass:bridgedb -out privkey 4096 && \
    openssl req -batch -passin pass:bridgedb -new -key privkey -out server.csr && \
    cp privkey privkey.nopasswd && \
    openssl rsa -passin pass:bridgedb -in privkey.nopasswd -out /home/bridgedb/privkey.pem && \
    openssl x509 -req -days 365 -in server.csr -signkey /home/bridgedb/privkey.pem -out /home/bridgedb/cert && \
    test -f "privkey.nopasswd" && rm -f privkey.nopasswd && \
    test -f "privkey" && rm -f privkey && \
    test -f "server.csr" && rm -f server.csr && \
    chown bridgedb.bridgedb /home/bridgedb/*

USER bridgedb
WORKDIR /home/bridgedb
RUN mkdir from-authority
COPY bridgedb.conf .
