

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	dab task mysql --password random
	dab install apache2-mpm-prefork
	dab task php
	sed -e 's/^\s*display_errors\s*=\s*On/display_errors = Off/' -i ${BASEDIR}/etc/php5/apache2/php.ini
	dab install php5-mysql
	tar xzf latest.tar.gz -C ${BASEDIR}/var/www/
	# change owner, in order to enable web based updates
	dab exec chown -R www-data.www-data /var/www/wordpress
	install -m 0644 wordpress.conf ${BASEDIR}/etc/apache2/sites-available/wordpress
	install -m 0755 wordpress_setup ${BASEDIR}/etc/init.d/
	dab exec insserv wordpress_setup
	dab exec a2enmod rewrite
	dab exec a2dissite default
	dab exec a2ensite wordpress
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