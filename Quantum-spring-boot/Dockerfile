FROM openjdk:11-jdk
ENV APP_HOME=/usr/app

WORKDIR $APP_HOME

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y build-essential cmake git vim wget curl gcc libtool make ninja-build libapr1-dev libssl-dev

RUN git clone https://github.com/open-quantum-safe/openssl.git openssl
RUN git clone https://github.com/open-quantum-safe/liboqs.git liboqs

RUN cd liboqs &&\
    mkdir build && cd build &&\
    cmake -GNinja -DCMAKE_INSTALL_PREFIX=$APP_HOME/openssl/oqs .. &&\
    ninja && ninja install

RUN cd openssl &&\
    ./Configure linux-x86_64 -lm &&\
    make -j &&\
    make install

ENV LD_LIBRARY_PATH=$OQS_OPENSSL_DIR

RUN wget https://dlcdn.apache.org/tomcat/tomcat-connectors/native/1.2.36/source/tomcat-native-1.2.36-src.tar.gz
RUN tar -xvzf tomcat-native-1.2.36-src.tar.gz
RUN cd tomcat-native-1.2.36-src/native && ./configure -with-apr=/usr/bin/apr-1-config --with-java-home=$JAVA_HOME --with-ssl=$OQS_OPENSSL_DIR --prefix=$CATALINA_HOME && make && make install
# This allows Tomcat to find the native libs
# ENV -Djava.library.path=/lib:/usr/lib:/usr/lib/ssl:/usr/app/
COPY demo/ demo/
COPY build.gradle demo/build.gradle
COPY application.properties demo/src/main/resources/application.properties
RUN cd demo/ && ./gradlew build -x test
# You can't copy plain jar 
RUN mv demo/build/libs/*.jar application.jar
# For TLS certification test
COPY rsa3072_srv.key openssl/rsa3072_srv.key 
COPY rsa3072_srv.crt openssl/rsa3072_srv.crt
COPY rsa3072_srv.p12 openssl/rsa3072_srv.p12

ENTRYPOINT ["java", "-jar", "application.jar"]