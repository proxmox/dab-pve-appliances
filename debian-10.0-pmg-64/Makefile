BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap --minimal
	sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/' ${BASEDIR}/etc/ssh/sshd_config
	dab exec /bin/systemctl enable systemd-timesyncd.service
	touch ${BASEDIR}/proxmox_install_mode
	dab install libdbi-perl perl-openssl-defaults libcgi-pm-perl proxmox-mailgateway-container gpg
	rm ${BASEDIR}/proxmox_install_mode
	sed -i '/^deb.*\.proxmox\.com\/.*$$/d;$${/^$$/d;}' ${BASEDIR}/etc/apt/sources.list
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
