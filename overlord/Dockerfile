FROM ubuntu:14.04


# Set environment variables to fetch Druid // Autobulid

ENV GITHUB_OWNER druid-io
ENV DRUID_VERSION 0.9.2

# Druid env variable
ENV DRUID_XMX           '-'
ENV DRUID_XMS           '-'
ENV DRUID_NEWSIZE       '-'
ENV DRUID_MAXNEWSIZE    '-'
ENV DRUID_HOSTNAME      '-'
ENV DRUID_LOGLEVEL      '-'


# System related updates and installs. 

RUN apt-get update && apt-get install -y software-properties-common \
      && apt-add-repository -y ppa:webupd8team/java \
      && apt-get purge --auto-remove -y software-properties-common \
      && apt-get update \
      && echo oracle-java-8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections \
      && apt-get install -y oracle-java8-installer \
      && apt-get install -y oracle-java8-set-default \
      && apt-get install -y supervisor \
      && apt-get install -y git \
      && rm -rf /var/cache/oracle-jdk8-installer 


# Adding a system User only for Druid

RUN adduser --system --group --no-create-home druid


WORKDIR /tmp/druid

# Getting stable version of Druid

RUN wget -q --no-check-certificate --no-cookies -O - \
    http://static.druid.io/artifacts/releases/druid-$DRUID_VERSION-bin.tar.gz | tar -xzf - -C /var/lib/ 
    
WORKDIR /var/lib/druid-$DRUID_VERSION
  
RUN java -classpath "/var/lib/druid-$DRUID_VERSION/lib/*" io.druid.cli.Main tools pull-deps -c io.druid.extensions:mysql-metadata-storage:$DRUID_VERSION \
    && ln -s /var/lib/druid-$DRUID_VERSION /var/lib/druid


# Custom configuration setup.

COPY conf /var/lib/druid-$DRUID_VERSION/conf 
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf


# Defining how to start Druid.
WORKDIR /tmp/
EXPOSE 8090
ENTRYPOINT export HOSTIP="$(getent hosts $HOSTNAME | awk '{print $1}')" && exec /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf