FROM php:7.4-apache

ENV COMPOSER_ALLOW_SUPERUSER 1

RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
  && apt-get clean && rm -rf /var/lib/apt/lists/* \
  && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
  && composer --version \
  && a2enmod rewrite \
  && service apache2 restart \
  && git clone https://github.com/jikan-me/jikan-rest.git -b v3 . \
  && git checkout v3 \
  && cp .env.dist .env \
  && sed -i "s/GITHUB_REPORTING=.*/GITHUB_REPORTING=false/" .env \
  && sed -i "s/REDIS_HOST=.*/REDIS_HOST=jikan-redis/" .env \
  && sed -i "s/CACHE_DEFAULT_EXPIRE=.*/CACHE_DEFAULT_EXPIRE=3600/" .env \
  && sed -i "s/CACHE_404_EXPIRE=.*/CACHE_404_EXPIRE=3600/" .env \
  && sed -i "s/CACHE_SEARCH_EXPIRE=.*/CACHE_SEARCH_EXPIRE=3600/" .env \
  && composer config --no-plugins allow-plugins true \
  && composer install --prefer-dist --no-progress --classmap-authoritative  --no-interaction \
  && composer update jikan-me/jikan

ADD run-jikan.sh ./

ENTRYPOINT ["./run-jikan.sh"]

EXPOSE 8080
