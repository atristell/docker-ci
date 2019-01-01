# From Centos 7
ARG LINUX_VERSION=centos:7
FROM ${LINUX_VERSION}
LABEL maintainer "atristell"

USER root
WORKDIR /tmp

#RUN yum install epel-release -y \ 


#============================================
# Node LTS, latest
#============================================
ARG NODE_VERSION=10.x
RUN curl -sL https://rpm.nodesource.com/setup_${NODE_VERSION} | bash - \
  && yum -y install nodejs wget

# Google Chrome
ARG CHROME_VERSION=google-chrome-stable
ADD ./google-x86_64.repo /etc/yum.repos.d/external.repo
RUN yum install -y ${CHROME_VERSION:-google-chrome-stable} 

# Firefox 
ARG FIREFOX_VERSION=latest
RUN yum install -y wget \
  bzip2 -y \
  firefox \
  && rm -rf /opt/firefox \
  && wget -O firefox.tar.bz2 "https://download.mozilla.org/?product=firefox-${FIREFOX_VERSION}-ssl&os=linux64&lang=en-US" \
  && tar -C /opt -xjvf firefox.tar.bz2 \
  && rm firefox.tar.bz2 \
  && mv /opt/firefox /opt/firefox-${FIREFOX_VERSION} \
  && ln -fs /opt/firefox-${FIREFOX_VERSION}/firefox /usr/bin/firefox

# JQ Json Processor
ARG JQ_VERSION=1.5
RUN wget https://github.com/stedolan/jq/releases/download/jq-${JQ_VERSION}/jq-linux64 -O jq \
  && chmod +x jq \
  && mv jq /usr/local/bin

# Cypress 
ARG CYPRESS_VERSION=3.1.4
# Install Cypress dependencies
RUN yum install -y xorg-x11-server-Xvfb \
  gtk2-2.24* \
  libXtst* \
  libXScrnSaver* \
  # provides libgconf-2
  GConf2* \
  # provides libasound
  alsa-lib*
RUN npm install cypress@${CYPRESS_VERSION}

# Sonarqube Client
ARG SONAR_VERSION=2.1.2
RUN npm install sonarqube-scanner@${SONAR_VERSION}


#============================================
# Cleanup
#============================================
#RUN yum clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/* 
RUN yum clean all && rm -rf /var/cache/yum && rm -rf /var/tmp/yum-*

# Versions
RUN node -v \
  && npm -v \
  && google-chrome -version \
  && firefox --version \
  && jq --version \
  && node_modules/.bin/cypress -v 
  #&& node_modules/.bin/sonar-scanner --version


