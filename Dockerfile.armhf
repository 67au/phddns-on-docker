FROM debian:latest

WORKDIR /root/oray

RUN buildDeps='wget binutils xz-utils' \ 
    && phddnsDeps='procps' \
    && phddnsSource="https://hsk.oray.com/download/download?id=25" \
    && apt-get update \
    && apt-get install -y $buildDeps $phddnsDeps \
    && wget -O phddns.deb $phddnsSource \
    && ar -x phddns.deb \
    && tar -xvJf data.tar.xz \
    && cp -r usr/orayapp /usr \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /root/oray \
    && apt-get purge -y --auto-remove $buildDeps 

CMD /bin/bash /usr/orayapp/phdaemon
