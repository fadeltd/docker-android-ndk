FROM ruby:2.7.0-alpine3.11
# FROM fadeltd/alpine-android-ndk-ruby-base

# Install required dependencies
# RUN apk add --no-cache \
# 		bash \
# 		build-base \
# 		git \
# 		make

WORKDIR /home/ruby
COPY Gemfile Gemfile.lock ./
# COPY Gemfile /Gemfile
RUN bundle -v \
    bundle env \
    ruby -v \
    pwd \
    ls \
    bundle install
