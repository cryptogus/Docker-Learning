# Open Quantum Safe + Tomcat Server
FROM openjdk:11-jdk

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y build-essential cmake git vim wget curl gcc libtool make ninja-build libapr1-dev libssl-dev

WORKDIR /root

# Install apache tomcat 10.0.23
RUN wget https://archive.apache.org/dist/tomcat/tomcat-10/v10.0.23/bin/apache-tomcat-10.0.23.tar.gz
RUN tar -xvzf apache-tomcat-10.0.23.tar.gz

ENV CATALINA_HOME /root/apache-tomcat-10.0.23
ENV CATALINA_BASE /root/apache-tomcat-10.0.23
# ENV OPENSSL_DIR=/root/openssl
# tomcat-native
RUN tar xvzf /root/apache-tomcat-10.0.23/bin/tomcat-native.tar.gz -C /root/apache-tomcat-10.0.23/bin
RUN cd /root/apache-tomcat-10.0.23/bin/tomcat-native-1.2.35-src/native &&\
   ./configure --with-apr=/usr/bin/apr-1-config \
                 --with-java-home=$JAVA_HOME \
                 --with-ssl=yes \
                 --prefix=$CATALINA_HOME &&\
    make && make install

# https://github.com/open-quantum-safe/openssl
RUN git clone https://github.com/open-quantum-safe/openssl.git openssl
RUN git clone https://github.com/open-quantum-safe/liboqs.git liboqs

# build oqs-openssl
RUN cd liboqs &&\
    mkdir build && cd build &&\
    cmake -GNinja -DCMAKE_INSTALL_PREFIX=/root/openssl/oqs .. &&\
    ninja && ninja install

RUN cd openssl &&\
    ./Configure linux-x86_64 -lm &&\
    make -j &&\
    make install

ENV LD_LIBRARY_PATH=/root/openssl

# container 내부로 연결 가능하도록 포트 공개, http 웹 서버 애플리케이션은 80번 포트를 주시
# EXPOSE 80

ENTRYPOINT ["./apache-tomcat-10.0.23/bin/catalina.sh", "run"]
