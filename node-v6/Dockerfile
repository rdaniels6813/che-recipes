FROM ubuntu

ENV JAVA_HOME /usr/lib/jvm/java-9-openjdk-amd64/
ENV NODE_VERSION 6.5.0
ENV NODE_PATH /usr/local/lib/node_modules

RUN apt-get update && \
 apt-get upgrade -y && \
 apt-get install -y curl build-essential libssl-dev libkrb5-dev gcc make ruby-full rubygems-integration python openjdk-9-jdk-headless unzip bash sudo git subversion openssh-server && \
 mkdir /var/run/sshd && \
 sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN gem install sass compass && \
    apt-get clean && \
    apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/* && \
    set -ex \
    && for key in \
  0xb63b535a4c206ca9 \
      9554F04D7259F04124DE6B476D5A82AC7E37093B \
      94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
      0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93 \
      FD3A5288F042B6850C66B31F09FE44734EB7990E \
      71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
      DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    ; do \
      gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
    done && \
  mkdir -p /home/user && cd /home/user && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz" \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --verify SHASUMS256.txt.asc \
  && grep "node-v$NODE_VERSION-linux-x64.tar.gz\$" SHASUMS256.txt.asc | sha256sum -c - \
  && tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 \
  && rm "node-v$NODE_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc

RUN cd $(npm root -g)/npm \
  && npm install fs-extra \
  && sed -i -e s/graceful-fs/fs-extra/ -e s/fs\.rename/fs.move/ ./lib/utils/rename.js

RUN npm install -g npm@latest
RUN npm install -g typings gulp bower grunt grunt-cli angular-cli aurelia-cli

EXPOSE 3000 5000 9000

WORKDIR /projects

CMD /usr/sbin/sshd -D && tail -f /dev/null
