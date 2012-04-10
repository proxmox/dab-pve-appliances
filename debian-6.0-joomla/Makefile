

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	dab task mysql --password admin
	dab install apache2-mpm-prefork apache2
	dab task php
	sed -e 's/^\s*display_errors\s*=\s*On/display_errors = Off/' -i ${BASEDIR}/etc/php5/apache2/php.ini
	sed -e 's/^\s*output_buffering\s*=\s*4096/output_buffering = off/' -i ${BASEDIR}/etc/php5/apache2/php.ini
	dab install php5-mysql
	mkdir ${BASEDIR}/var/www/joomla
	tar -xzf Joomla_1.6.3-Stable-Full_Package.tar.gz -C ${BASEDIR}/var/www/joomla
	touch ${BASEDIR}/var/www/joomla/configuration.php
	find ${BASEDIR}/var/www/joomla -type f -exec chmod 644 \{\} \;
	find ${BASEDIR}/var/www/joomla -type d -exec chmod 755 \{\} \;
	dab exec chown -R www-data.www-data /var/www/joomla
	install -m 0644 joomla.conf ${BASEDIR}/etc/apache2/sites-available/joomla
	dab exec a2enmod rewrite
	dab exec a2dissite default
	dab exec a2ensite joomla
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
