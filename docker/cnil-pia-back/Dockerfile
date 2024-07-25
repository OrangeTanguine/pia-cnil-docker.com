FROM ruby:2.7.4
ARG PIA_VERSION=3.0.0

ENV DEBIAN_FRONTEND noninteractive
ENV RAILS_ENV production
ENV RAILS_SERVE_STATIC_FILES=true

RUN apt-get update \
 && apt-get install --no-install-recommends -y \
    git \
    postgresql-client \
    shared-mime-info \
    curl \
    iputils-ping \
 && apt-get clean

RUN wget https://github.com/LINCnil/pia-back/archive/refs/tags/v$PIA_VERSION.zip \
 && unzip v$PIA_VERSION.zip \
 && rm v$PIA_VERSION.zip \
 && mv pia-back-$PIA_VERSION /var/www

#RUN wget https://github.com/LINCnil/pia-back/archive/refs/heads/master.zip \
# && unzip master.zip \
# && rm master.zip \
# && mv pia-back-master /var/www


COPY database.yml /var/www/config/database.yml
COPY application.yml /var/www/config/application.yml

WORKDIR /var/www/

RUN gem install bundler -v 2.4.22
RUN bundle config set force_ruby_platform true
#install shared-mime info and update mimemagic
RUN bundle update mimemagic
#RUN gem install mimemagic -v '0.3.10'
RUN bundle install

COPY entrypoint /entrypoint
RUN chmod +x /entrypoint
ENTRYPOINT ["/entrypoint"]

CMD ["bin/rails", "server"]
