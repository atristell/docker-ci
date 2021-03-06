##============================================
# From Node
#============================================
ARG NODE_VERSION="10.14.2"
FROM node:${NODE_VERSION}
LABEL maintainer "atristell"

USER root

#============================================
# Cypress dependencies
# Source: https://github.com/cypress-io/cypress-docker-images/tree/master/base/10
#============================================
RUN apt-get update && \
  apt-get install -y \
    libgtk2.0-0 \
    libnotify-dev \
    libgconf-2-4 \
    libnss3 \
    libxss1 \
    libasound2 \
    xvfb

#============================================
# Google Chrome
# Source: https://github.com/SeleniumHQ/docker-selenium/blob/master/NodeChrome/Dockerfile
#============================================
# can specify versions by CHROME_VERSION;
#  e.g. google-chrome-stable=53.0.2785.101-1
#       google-chrome-beta=53.0.2785.92-1
#       google-chrome-unstable=54.0.2840.14-1
#       latest (equivalent to google-chrome-stable)
#       google-chrome-beta  (pull latest beta)
#============================================
ARG CHROME_VERSION="google-chrome-stable=53.0"
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
  && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update -qqy \
  && apt-get -qqy install \
    ${CHROME_VERSION:-google-chrome-stable} \
  && rm /etc/apt/sources.list.d/google-chrome.list \
  && rm -rf /var/lib/apt/lists/* /var/cache/apt/*

#=========
# Firefox
# Source: https://github.com/SeleniumHQ/docker-selenium/blob/master/NodeFirefox/Dockerfile.txt
#=========
ARG FIREFOX_VERSION=latest
RUN FIREFOX_DOWNLOAD_URL=$(if [ $FIREFOX_VERSION = "latest" ] || [ $FIREFOX_VERSION = "nightly-latest" ] || [ $FIREFOX_VERSION = "devedition-latest" ]; then echo "https://download.mozilla.org/?product=firefox-$FIREFOX_VERSION-ssl&os=linux64&lang=en-US"; else echo "https://download-installer.cdn.mozilla.net/pub/firefox/releases/$FIREFOX_VERSION/linux-x86_64/en-US/firefox-$FIREFOX_VERSION.tar.bz2"; fi) \
  && apt-get update -qqy \
  && rm -rf /var/lib/apt/lists/* /var/cache/apt/* \
  apt-get install firefox \
  && wget --no-verbose -O /tmp/firefox.tar.bz2 $FIREFOX_DOWNLOAD_URL \
  #&& apt-get -y purge firefox \
  && rm -rf /opt/firefox \
  && tar -C /opt -xjf /tmp/firefox.tar.bz2 \
  && rm /tmp/firefox.tar.bz2 \
  && mv /opt/firefox /opt/firefox-$FIREFOX_VERSION \
  && ln -fs /opt/firefox-$FIREFOX_VERSION/firefox /usr/bin/firefox
#ADD scripts/xvfb-firefox /usr/bin/xvfb-firefox
#RUN ln -sf /usr/bin/xvfb-firefox /usr/bin/firefox

#============================================
# Watchman
# Source: https://github.com/sergiolepore/docker-ember/blob/3.1.3-node_10.0.0/Dockerfile
#============================================
ARG WATCHMAN_VERSION=v4.9.0
## watchman dependencies 
RUN apt-get -qqy update \
    && apt-get -qqy install \
      python-dev \
      --no-install-recommends

# Watchman (increase s.inotify.max_user_watches)
#RUN cd /tmp/ \
#    && git clone https://github.com/facebook/watchman.git watchman \
#    && cd watchman \
#    && git checkout ${WATCHMAN_VERSION} \
#    && ./autogen.sh \
#    && ./configure \
#    && make \
#    && make install

#============================================
# Ember
#============================================
#RUN \
#	npm install -g ember-cli@3.6.0

#============================================
# JQ, JSON processor
#============================================
RUN apt-get -y install jq


#============================================
# Cleanup
#============================================
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/* 

# NPM version should already be pretty new (> 6.4.0)
RUN npm -v && yarn -v \
  && node -v \
  && google-chrome --version \
  && firefox --version \
  #&& watchman -v \
  && jq --version



CMD ["bash"]