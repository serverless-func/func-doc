FROM ruby:2.6-slim

ARG local=false
RUN if [ "$local" = "true" ] ; then sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list; fi

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        nodejs \
    && gem install bundler

WORKDIR /srv/slate
VOLUME /srv/slate/build

COPY config.rb Gemfile Gemfile.lock /srv/slate/
RUN if [ "$local" = "true" ] ; then bundle config mirror.https://rubygems.org https://gems.ruby-china.com && bundle install ; else bundle install ; fi

COPY lib /srv/slate/lib

CMD ["bundle", "exec", "middleman", "build", "--clean"]