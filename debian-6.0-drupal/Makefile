

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	dab task mysql --password random
	dab install apache2-mpm-prefork
	dab task php
	# install all packages needed for drupal6
	dab install php5-mysql wwwconfig-common dbconfig-common curl libjs-jquery
	# unpack drupal6 (configuration is done at first start)
	dab unpack drupal6
	# set php ini settings recommended by Drupal
	sed -e 's/^\s*display_errors\s*=\s*On/display_errors = Off/' -i ${BASEDIR}/etc/php5/apache2/php.ini
	sed -e 's/^\s*magic_quotes_gpc\s*=\s*On/magic_quotes_gpc = Off/' -i ${BASEDIR}/etc/php5/apache2/php.ini
	install -m 0644 drupal.conf ${BASEDIR}/etc/drupal/6/apache2.conf
	install -m 755 drupal_setup ${BASEDIR}/etc/init.d/
	dab exec ln -s /etc/drupal/6/apache2.conf /etc/apache2/conf.d/drupal6.conf
	dab exec insserv drupal_setup
	dab exec a2enmod rewrite
	dab exec a2dissite default
	dab finalize

info/init_ok: dab.conf
	dab init
	touch $@

.PHONY: clean
clean:
	dab clean
	rm -f *~

.PHONY: dist-clean
dist-clean:
	dab dist-clean
	rm -f *~


