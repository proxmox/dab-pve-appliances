

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	echo 'Acquire::Check-Valid-Until "0";' > ${BASEDIR}/etc/apt/apt.conf.d/10no-check-valid-until
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
