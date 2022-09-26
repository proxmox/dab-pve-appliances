

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	dab task mysql --password admin --start
	dab install apache2-mpm-prefork
	dab task php --memlimit 64
	dab install php5-mysql php5-imagick imagemagick
	tar xzf acquia-drupal-1.2.21.5181.tar.gz -C ${BASEDIR}/var/www
	mv ${BASEDIR}/var/www/acquia-drupal-1.2.21/ ${BASEDIR}/var/www/drupal
	find ${BASEDIR}/var/www/drupal -type f -exec chmod 644 \{\} \;
	find ${BASEDIR}/var/www/drupal -type d -exec chmod 755 \{\} \;
	dab exec chown -R www-data.www-data /var/www/drupal
	# set php ini settings recommended by Drupal
	sed -e 's/^\s*display_errors\s*=\s*On/display_errors = Off/' -i ${BASEDIR}/etc/php5/apache2/php.ini
	sed -e 's/^\s*magic_quotes_gpc\s*=\s*On/magic_quotes_gpc = Off/' -i ${BASEDIR}/etc/php5/apache2/php.ini
	cp ${BASEDIR}/var/www/drupal/sites/default/default.settings.php ${BASEDIR}/var/www/drupal/sites/default/settings.php
	cat drupal.mysql|dab exec mysql -padmin
	# install crontab
	echo "*/5 * * * * root /usr/bin/wget -O /dev/null -q http://localhost/cron.php" > ${BASEDIR}/etc/cron.d/drupal
	install -m 0644 drupal.conf ${BASEDIR}/etc/apache2/sites-available/drupal
	dab exec a2enmod rewrite
	dab exec a2dissite default
	dab exec a2ensite drupal
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
