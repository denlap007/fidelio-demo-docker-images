FROM debian:jessie

MAINTAINER Lappas Dionysis "dio@freelabs.net"

ENV NGINX_VERSION 1.9.10-1~jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
	&& echo "deb http://nginx.org/packages/mainline/debian/ jessie nginx" >> /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install -y ca-certificates nginx=${NGINX_VERSION} gettext-base \
	&& rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

# install oracle server jre
ENV JAVA_VERSION "8u72"
ENV BUILD_VERSION "b15"
ENV JDK_VERSION "1.8.0_72"

RUN apt-get update && apt-get install -y \
    wget \
    curl \
    && mkdir -p /usr/lib/jvm \
    && cd /usr/lib/jvm \
    && wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}-${BUILD_VERSION}/server-jre-${JAVA_VERSION}-linux-x64.tar.gz" \
    && tar -zxvf server-jre-${JAVA_VERSION}-linux-x64.tar.gz \
    && rm /usr/lib/jvm/server-jre-${JAVA_VERSION}-linux-x64.tar.gz \
    && ln -s /usr/lib/jvm/jdk${JDK_VERSION}/bin/java /bin/ \
    && cd /usr/lib/jvm/jdk${JDK_VERSION} \
    && find . -type f -name '*.html' -delete \
    && find . -type f -name '*.bat' -delete \
    && find . -type f -name 'COPYRIGHT' -delete \
    && find . -type f -name 'LICENSE' -delete \
    && find . -type f -name 'NOTICE' -delete \
    && find . -type f -name '3RDPARTY' -delete \
    && find . -type f -name 'THIRDPARTYLICENSE.txt' -delete \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# set java environment variable and update PATH
ENV JAVA_HOME /usr/lib/jvm/jdk${JDK_VERSION}/bin
ENV PATH ${PATH}:${JAVA_HOME}

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]