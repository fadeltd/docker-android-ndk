FROM fadeltd/alpine-android-ndk-ruby-base

RUN apk --no-cache add --update \
#     bash \
#     build-base \
#     ca-certificates \
    coreutils \
    curl \
#     git \
    g++ \
    libstdc++ \
#     make \
    openssl \
#     ruby \
#     ruby-irb \
#     ruby-dev \
    zip

WORKDIR /home/ruby
COPY Gemfile Gemfile.lock ./
RUN (gem install curb -v '0.9.10' || true); \
    (bundle install || true); \
    (make -v || true)
