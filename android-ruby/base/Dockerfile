FROM fadeltd/alpine-android-ndk-ruby-base
LABEL description="Alpine Android NDK Base with Latest Ruby"

# Install required dependencies
RUN apk add --no-cache \
		bash \
		build-base \
		git \
		gmp-dev	\
		make

WORKDIR /home/ruby
COPY Gemfile /Gemfile
RUN bundle install
COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
