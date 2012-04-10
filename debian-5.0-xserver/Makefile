

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	dab install xfonts-base xbase-clients
	dab install vnc4server vnc-java
	# xrdp does not start automatically - really strange
	# after booting, exec /etc/init.d/xrdp restart
	# fix: it should call setsid correctly
	# hack: add -b to start-stop-daemon
	dab install xrdp
	sed -e 's/USERID --exec $$DAEMON/USERID -b --exec $$DAEMON/' -i ${BASEDIR}/etc/init.d/xrdp
	dab install gdm gnome-core gdm-themes 
	dab install fast-user-switch-applet file-roller gcalctool  gnome-backgrounds gnome-about gnome-keyring gnome-menus gnome-system-monitor gnome-themes gnome-utils gtk2-engines desktop-base gksu
	dab install iceweasel iceweasel-gnome-support
	echo "vnc1024 stream tcp nowait nobody /usr/bin/Xvnc Xvnc -inetd -query 127.0.0.1 -once -depth 16 -geometry 1024x768 securitytypes=none" >>${BASEDIR}/etc/inetd.conf 
	echo "vnc1024         5900/tcp        # vnc X11 normal size" >> ${BASEDIR}/etc/services
	install -m 644 gdm.conf ${BASEDIR}/etc/gdm/gdm.conf
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