FROM ruby:2.3-alpine
MAINTAINER Joseph Bauser <coderjoe@coderjoe.net>

# Set up dependencies
ENV BUILD_PACKAGES="build-base git" \
		DEV_PACKAGES="bzip2-dev libgcrypt-dev libxml2-dev libxslt-dev openssl-dev postgresql-dev sqlite-dev zlib-dev" \
		RAILS_DEPS="ca-certificates nodejs tzdata" \
		RAILS_VERSION="5.0.1" \
		WEB_ROOT="/srv/www"

# Install our dependencies and rails
RUN \
	apk add --no-cache --update --upgrade --virtual .railsdeps \
			$BUILD_PACKAGES $DEV_PACKAGES $RAILS_DEPS \
	&& gem install bundler \
	&& gem install nokogiri -- --use-system-libraries \
	&& gem install rails \
	&& rm -rf /var/cache/apk/* \
	&& mkdir -p $WEB_ROOT

WORKDIR $WEB_ROOT

# Onbuild grab gemfile information and add the project to the webroot
ONBUILD ADD Gemfile* $WEB_ROOT/
ONBUILD RUN bundle install --jobs=4 --retry=3
ONBUILD ADD . $WEB_ROOT/

CMD ["bundle", "exec", "rails", "server", "-p", "3000", "-b", "0.0.0.0"]
