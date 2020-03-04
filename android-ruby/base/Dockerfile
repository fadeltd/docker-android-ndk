# FROM fadeltd/alpine-android-ndk-ruby-base
FROM ruby:2.7.0-alpine3.11

# throw errors if Gemfile has been modified since Gemfile.lock
RUN bundle config --global frozen 1

# Install required dependencies
RUN apk add --no-cache \
		bash \
		build-base \
		git \
		gmp-dev	\
		make

WORKDIR /home/ruby

COPY Gemfile ./
RUN bundle install
