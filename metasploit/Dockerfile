# metasploit docker container

FROM ubuntu:16.04

# install apt-add-repository
RUN apt-get update && apt-get install -y software-properties-common

# install java
RUN add-apt-repository -y ppa:webupd8team/java
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
RUN apt-get update && apt-get -y install oracle-java8-installer

# install dependencies
RUN apt-get update && apt-get install -y build-essential libreadline-dev libssl-dev libpq5 libpq-dev libreadline5 libsqlite3-dev libpcap-dev git-core autoconf postgresql pgadmin3 curl zlib1g-dev libxml2-dev libxslt1-dev vncviewer libyaml-dev curl zlib1g-dev

# install gpg2
RUN apt-get update && apt-get install -y gnupg2

# install a proper version of ruby
RUN curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
RUN curl -L https://get.rvm.io | bash -s stable
RUN echo "source /usr/local/rvm/scripts/rvm" >> ~/.bashrc
RUN /bin/bash -c "source /usr/local/rvm/scripts/rvm && RUBYVERSION=$(curl https://raw.githubusercontent.com/rapid7/metasploit-framework/master/.ruby-version) && rvm install `curl https://raw.githubusercontent.com/rapid7/metasploit-framework/master/.ruby-version` && rvm use `curl https://raw.githubusercontent.com/rapid7/metasploit-framework/master/.ruby-version` --default"

# install nmap
RUN apt-get update && apt-get install -y nmap

# create database user and msf database
RUN /etc/init.d/postgresql start && su postgres -c "createuser msf -P -S -R -D"
RUN /etc/init.d/postgresql start && su postgres -c "createdb -O msf msf"

# install metasploit
WORKDIR /opt
RUN git clone https://github.com/rapid7/metasploit-framework.git
WORKDIR /opt/metasploit-framework/
#RUN bash -c "source /usr/local/rvm/scripts/rvm && rvm --default use ruby-`curl https://raw.githubusercontent.com/rapid7/metasploit-framework/master/.ruby-version`@metasploit-framework"
RUN bash -c "source /usr/local/rvm/scripts/rvm && gem install bundler"
RUN bash -c "source /usr/local/rvm/scripts/rvm && bundle install"
RUN bash -c 'for MSF in $(ls msf*); do ln -s /opt/metasploit-framework/$MSF /usr/local/bin/$MSF;done'

# expose ports for https and http reverse payloads
EXPOSE 443 443
EXPOSE 80 80

# allow terminal highlighting and other nice things
ENV TERM=vt100

# default command is to run the postgres server, exec into the container for a shell
USER postgres
CMD ["/usr/lib/postgresql/9.5/bin/postgres", "-D", "/var/lib/postgresql/9.5/main", "-c", "config_file=/etc/postgresql/9.5/main/postgresql.conf"]
