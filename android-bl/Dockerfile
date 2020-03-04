FROM fadeltd/alpine-android-ndk-ruby-base

RUN apk add --no-cache \
# 		bash \
# 		build-base \
# 		git \
        libcurl
#		make

WORKDIR /home/ruby
COPY Gemfile Gemfile.lock ./
RUN (gem install curb -v '0.9.10' || true); \
    (bundle install || true); \
    (make -v || true)
