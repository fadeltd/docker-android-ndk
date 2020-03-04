# FROM fadeltd/alpine-android-ndk-ruby-base

# RUN apk --no-cache add --update \
#     bash \
#     build-base \
#     ca-certificates \
#     coreutils \
#     curl \
#     git \
#     g++ \
#     libstdc++ \
#     make \
#     openssl \
#     ruby \
#     ruby-irb \
#     ruby-dev \
#     zip

# WORKDIR /home/ruby
# COPY Gemfile Gemfile.lock ./
# RUN gem install bundler --version='1.17.3'; \
#     bundler config default '1.17.3'; \
#     (gem install curb -v '0.9.10' || true); \
#     (bundle install || true); \
#     (make -v || true)

FROM ruby:2.7.0-alpine3.11

LABEL maintainer="BL"

RUN apk --no-cache add --update \
    bash \
    bc \
    ca-certificates \
    coreutils \
    curl \
    git \
    g++ \
    libstdc++ \
    make \
    openssl \
    ruby \
    ruby-irb \
    ruby-dev \
    wget \
    unzip \
    zip \
    && curl -sLO https://github.com/github/git-lfs/releases/download/v2.0.1/git-lfs-linux-amd64-2.0.1.tar.gz \
    && tar zxvf git-lfs-linux-amd64-2.0.1.tar.gz \
    && mv git-lfs-2.0.1/git-lfs /usr/bin/ \
    && rm -rf git-lfs-2.0.1 \
    && rm -rf git-lfs-linux-amd64-2.0.1.tar.gz

## Firebase
RUN curl -Lo ./firebase https://firebase.tools/bin/linux/latest \
    && chmod +xr firebase \
    && mv firebase /bin/firebase

## Fastlane
ENV FASTLANE_VERSION=2.140.0
RUN apk --no-cache add --update curl-dev imagemagick \
    && gem install fastlane -N -v $FASTLANE_VERSION \
    && gem install badge; exit 0;

## Resolve Jenkins issue https://github.com/gradle/gradle/issues/3117
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

WORKDIR /home/android
COPY Gemfile Gemfile.lock ./
RUN (gem install curb -v '0.9.10' || true); \
    (bundle install || true); \
    (make -v || true)