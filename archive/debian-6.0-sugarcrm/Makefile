

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	dab task mysql --password admin
	dab install apache2-mpm-prefork
	dab task php --memlimit=64
	sed -e 's/^\s*display_errors\s*=\s*On/display_errors = Off/' -i ${BASEDIR}/etc/php5/apache2/php.ini
	# set php upload filesize limit
	sed -e 's/^\s*upload_max_filesize\s*=\s*2M/upload_max_filesize = 30M/' -i ${BASEDIR}/etc/php5/apache2/php.ini
	dab install php5-mysql php5-imap php5-curl
	unzip -q SugarCE-6.1.4.zip -d ${BASEDIR}/var/www/
	mv ${BASEDIR}/var/www/SugarCE-Full-6.1.4/ ${BASEDIR}/var/www/sugarcrm
	find ${BASEDIR}/var/www/sugarcrm -type f -exec chmod 644 \{\} \;
	find ${BASEDIR}/var/www/sugarcrm -type d -exec chmod 755 \{\} \;
	dab exec chown -R www-data.www-data /var/www/sugarcrm
	install -m 0644 sugarcrm.conf ${BASEDIR}/etc/apache2/sites-available/sugarcrm
	dab exec a2enmod rewrite
	dab exec a2dissite default
	dab exec a2ensite sugarcrm
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
