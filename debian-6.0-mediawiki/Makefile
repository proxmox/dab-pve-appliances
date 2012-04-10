

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	dab task mysql --password admin
	dab install apache2-mpm-prefork apache2
	dab task php --memlimit 64
	dab install php5-xcache
	sed -e 's/^\s*xcache.var_size\s*=\s*0M/xcache.var_size = 16M/' -i ${BASEDIR}/etc/php5/conf.d/xcache.ini
	dab install mediawiki mediawiki-extensions
	install -m 0644 mediawiki.conf ${BASEDIR}/etc/mediawiki/apache.conf
	install -m 0644 robots.txt ${BASEDIR}/var/www/robots.txt
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
