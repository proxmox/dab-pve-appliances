BASEDIR:=$(shell dab basedir)

CVD_FILES:=main.cvd bytecode.cvd daily.cvd safebrowsing.cvd

all: info/init_ok ${CVD_FILES}
	dab bootstrap --minimal
	sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/' ${BASEDIR}/etc/ssh/sshd_config
	dab exec /bin/systemctl enable systemd-timesyncd.service
	touch ${BASEDIR}/proxmox_install_mode
	dab install libdbi-perl perl-openssl-defaults libcgi-pm-perl proxmox-mailgateway-container gpg
	rm ${BASEDIR}/proxmox_install_mode
	sed -i '/^deb.*\.proxmox\.com\/.*$$/d;$${/^$$/d;}' ${BASEDIR}/etc/apt/sources.list
	cp ${CVD_FILES} ${BASEDIR}/var/lib/clamav/
	dab finalize

info/init_ok: dab.conf
	dab init
	touch $@

.PHONY: clean
clean:
	dab clean
	rm -f ${CVD_FILES}
	rm -f *~

.PHONY: dist-clean
dist-clean:
	dab dist-clean
	rm -f ${CVD_FILES}
	rm -f *~

.PHONY: ${CVD_FILES}
${CVD_FILES}:
	curl -L --silent --show-error --fail --time-cond $@ -o $@.tmp http://database.clamav.net/$@
	[ -f $@.tmp ] && mv $@.tmp $@ || true
	if command -v sigtool > /dev/null ; then sigtool -i $@; else echo "skipping verification of $@"; fi
