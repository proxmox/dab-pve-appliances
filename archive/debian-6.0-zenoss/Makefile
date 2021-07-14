

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	# install zenoss key
	cat zenoss-gpg.key | chroot ${BASEDIR}/ apt-key add -
	dab install zenoss-stack
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