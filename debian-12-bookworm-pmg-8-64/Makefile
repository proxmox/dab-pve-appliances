BASEDIR:=$(shell dab basedir)

CVD_FILES:=main.cvd bytecode.cvd daily.cvd safebrowsing.cvd

all: info/init_ok ${CVD_FILES}
	dab bootstrap --minimal
	sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/' ${BASEDIR}/etc/ssh/sshd_config
	mkdir -p ${BASEDIR}/etc/systemd/system-preset
	cp systemd-presets ${BASEDIR}/etc/systemd/system-preset/00-pve-template.preset
	touch ${BASEDIR}/proxmox_install_mode
	dab install libdbi-perl perl-openssl-defaults libcgi-pm-perl proxmox-mailgateway-container gpg ifupdown2
	dab install antiword docx2txt odt2txt poppler-utils tesseract-ocr unrtf libsasl2-modules
	rm ${BASEDIR}/proxmox_install_mode
	sed -i '/^deb.*\.proxmox\.com\/.*$$/d;$${/^$$/d;}' ${BASEDIR}/etc/apt/sources.list
	cp ${CVD_FILES} ${BASEDIR}/var/lib/clamav/
	dab finalize --compressor zstd-max

info/init_ok: dab.conf
	dab init
	touch $@

.PHONY: clean
clean:
	dab clean
	rm -f *~

.PHONY: dist-clean
dist-clean: clean
	dab dist-clean
	rm -f ${CVD_FILES}

${CVD_FILES}:
	curl -L --silent --show-error --fail --time-cond $@ --user-agent "CVDUPDATE/0.3.0 (9d591f58-b430-4d0c-99b2-febb1cee0872)" -o $@.tmp https://database.clamav.net/$@
	[ -f $@.tmp ] && mv $@.tmp $@ || true
	if command -v sigtool > /dev/null ; then sigtool -i $@; else echo "skipping verification of $@"; fi
