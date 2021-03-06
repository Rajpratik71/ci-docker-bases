FROM ubuntu:18.04@sha256:0925d086715714114c1988f7c947db94064fd385e171a63c07730f1fa014e6f9

LABEL maintainer="Michael Cooper <mcooper@mozilla.com>"

RUN apt-get update
RUN apt-get --no-install-recommends install -y \
        apt-transport-https \
        apt-utils \
        ca-certificates \
        curl \
        git \
        gnupg \
        make \
        openssh-client \
        software-properties-common

# Install Firefox dependencies (but not Firefox)
RUN apt-get --no-install-recommends install -y $(apt-cache depends firefox | awk '/Depends/ {print $2}')

# Install node
RUN apt-get --no-install-recommends install -y nodejs npm
RUN apt-get --no-install-recommends install apt-transport-https

# Install Yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update \
    && apt-get --no-install-recommends install yarn
ENV PATH="$PATH:$HOME/.yarn/bin"

# Install Firefox
RUN yarn global add get-firefox
RUN cd /opt && get-firefox --check --extract --verbose --branch release && ln -s /opt/firefox/firefox /usr/bin/firefox

# Cleanup
RUN apt-get clean
