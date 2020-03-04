# FROM ruby:2.7.0-alpine3.11
FROM fadeltd/alpine-android-ndk-ruby-base

RUN apk --no-cache add --update \
    bash \
    # ca-certificates \
    # coreutils \
    curl \
    git \
    g++ \
    libstdc++ \
    make \
    openssl \
    # wget \
    # unzip \
    # zip
    # && wget -O git-lfs-linux-amd64-2.0.1.tar.gz https://github.com/github/git-lfs/releases/download/v2.0.1/git-lfs-linux-amd64-2.0.1.tar.gz \
    # && tar zxvf git-lfs-linux-amd64-2.0.1.tar.gz \
    # && mv git-lfs-2.0.1/git-lfs /usr/bin/ \
    # && rm -rf git-lfs-2.0.1 \
    # && rm -rf git-lfs-linux-amd64-2.0.1.tar.gz

## Firebase
# RUN curl -Lo ./firebase https://firebase.tools/bin/linux/latest \
#     && chmod +xr firebase \
#     && mv firebase /bin/firebase

## Fastlane
ENV FASTLANE_VERSION=2.142.0
RUN apk --no-cache add --update \
    curl-dev \
    imagemagick \
    && gem install fastlane -N -v $FASTLANE_VERSION \
    && gem install badge;

# ENV LANG en_US.UTF-8
# ENV LANGUAGE en_US:en
# ENV LC_ALL en_US.UTF-8

WORKDIR /home/android
COPY Gemfile Gemfile.lock ./
RUN bundle install
