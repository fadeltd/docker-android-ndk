FROM ruby:2.7.0-alpine3.11
# FROM fadeltd/alpine-android-ndk-ruby-base

# Install required dependencies
# RUN apk add --no-cache \
# 		bash \
# 		build-base \
# 		git \
# 		gmp-dev	\
# 		make

WORKDIR /home/ruby
# COPY Gemfile ./
COPY Gemfile /Gemfile
RUN bundle install
