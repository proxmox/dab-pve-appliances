BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap
	###
	# NOTE: zstd appliances are only supported on PVE >= 7.0, take care on upload!
	###
	dab finalize --compressor zstd-max

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
