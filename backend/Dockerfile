FROM node:8.12-alpine as node
FROM ruby:2.5.3-alpine as builder

ENV LANG C.UTF-8

RUN ln -s /opt/yarn/bin/yarn /usr/local/bin/yarn \
    && ln -s /opt/yarn/bin/yarnpkg /usr/local/bin/yarnpkg

ENV BUILD_PACKAGES="curl-dev ruby-dev build-base" \
    DEV_PACKAGES="zlib-dev libxml2-dev libxslt-dev tzdata yaml-dev sqlite-dev" \
    RUBY_PACKAGES="ruby-json yaml"

RUN apk --update add --virtual build-dependencies \
    build-base \
    curl-dev \
    mysql-dev \
    linux-headers

RUN gem install bundler

# Update and install base packages and nokogiri gem that requires a
# native compilation
RUN apk update && \
    apk upgrade && \
    apk add --no-cache --update\
    $BUILD_PACKAGES \
    $DEV_PACKAGES \
    $RUBY_PACKAGES && \
    mkdir -p /usr/src/myapp

WORKDIR /var/www

RUN apk add --no-cache --update mariadb-dev && \
    rm -rf /usr/lib/mysqld* && \
    rm -rf /usr/bin/mysql*

COPY Gemfile /var/www/Gemfile
COPY Gemfile.lock /var/www/Gemfile.lock

RUN bundle install --jobs=4 --retry=10 --clean

RUN apk del build-dependencies

FROM ruby:2.5.3-alpine

# GET node and yarn
COPY --from=node /opt/yarn-* /opt/yarn
COPY --from=node /usr/local/bin/node /usr/local/bin/

RUN apk --update add \
    bash \
    mariadb-dev \
    tzdata && \
    rm -rf /usr/lib/mysqld* && \
    rm -rf /usr/bin/mysql*

RUN gem install bundler

WORKDIR /tmp

COPY ./Gemfile ./Gemfile
COPY ./Gemfile.lock ./Gemfile.lock
COPY --from=builder /usr/local/bundle /usr/local/bundle

ENV APP_DIR /var/www

RUN mkdir -p $APP_DIR
WORKDIR $APP_DIR
COPY . $APP_DIR

EXPOSE 3000

CMD ["rails", "s", "-b", "0.0.0.0", "-p" , "3000"]
