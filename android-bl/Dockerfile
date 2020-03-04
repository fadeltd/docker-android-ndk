FROM fadeltd/alpine-android-ndk-ruby-base

RUN apk add --no-cache \
# 		bash \
# 		build-base \
# 		git \
		make

WORKDIR /home/ruby
COPY Gemfile Gemfile.lock ./
RUN make -v; \
    (bundle install || true); \
    cat /usr/local/bundle/extensions/x86_64-linux-musl/2.7.0/curb-0.9.10/mkmf.log; \
    gem install curb -v '0.9.10'
