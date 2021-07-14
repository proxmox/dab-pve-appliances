

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	dab task postgres --start
	dab install apache2-mpm-prefork
	dab install rt3.8-db-postgresql
	dab install libio-socket-ssl-perl libtext-quoted-perl request-tracker3.8
	dab install rt3.8-rtfm
	install -m 0755 setup-rt ${BASEDIR}/etc/init.d/
	dab exec insserv setup-rt
	cat rt.aliases >> ${BASEDIR}/etc/aliases
	dab exec newaliases	
	install -m 0644 rt.conf ${BASEDIR}/etc/apache2/sites-available/rt
	dab exec a2enmod rewrite
	dab exec a2enmod ssl
	dab exec a2dissite default
	dab exec a2ensite rt
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
