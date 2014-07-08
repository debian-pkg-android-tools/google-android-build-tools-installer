
PKG_NAME:=build-tools
PKG_VERSION:=20

PKG_SOURCE:=build-tools_r${PKG_VERSION}-linux.zip
PKG_SOURCE_URL:=https://dl-ssl.google.com/android/repository/${PKG_SOURCE}

DL_DIR=/var/cache/google-android-build-tools-installer
UNPACK_DIR=$(DL_DIR)/android-4.4W

all: $(DL_DIR)/$(UNPACK_DIR)/aapt
	sed -i 's,^libdir=.*,libdir=/usr/share/java,' $(UNPACK_DIR)/dx

install: all
	install -d -m0755 /usr/share/java
	install -m0644 $(UNPACK_DIR)/lib/dx.jar /usr/share/java/
	install -d -m0755 /usr/bin
	install -m0755 $(UNPACK_DIR)/bcc_compat \
		$(UNPACK_DIR)/arm-linux-androideabi-ld \
		$(UNPACK_DIR)/aapt \
		$(UNPACK_DIR)/aidl \
		$(UNPACK_DIR)/dexdump \
		$(UNPACK_DIR)/dx \
		$(UNPACK_DIR)/i686-linux-android-ld \
		$(UNPACK_DIR)/llvm-rs-cc \
		$(UNPACK_DIR)/mipsel-linux-android-ld \
		$(UNPACK_DIR)/zipalign \
		/usr/bin/
	install -d -m0755 /usr/lib/
	for f in libLLVM.so libbcinfo.so libbcc.so libclang.so; do \
		test -e /usr/bin/$$f || install -m0644 $(UNPACK_DIR)/$$f /usr/lib/; done
	install -d -m0755 /usr/share/doc/google-android-build-tools
	gzip -9 $(UNPACK_DIR)/NOTICE.txt > /usr/share/doc/google-android-build-tools/copyright.gz
	install -m0644 $(UNPACK_DIR)/source.properties /usr/share/doc/google-android-build-tools/

uninstall:
	rm -f /usr/share/java/dx.jar
	rm -f /usr/bin/bcc_compat \
		/usr/bin/arm-linux-androideabi-ld \
		/usr/bin/aapt \
		/usr/bin/aidl \
		/usr/bin/dexdump \
		/usr/bin/dx \
		/usr/bin/i686-linux-android-ld \
		/usr/bin/llvm-rs-cc \
		/usr/bin/mipsel-linux-android-ld \
		/usr/bin/zipalign \
	rm -f /usr/lib/libLLVM.so \
		/usr/lib/libbcinfo.so \
		/usr/lib/libbcc.so \
		/usr/lib/libclang.so
	rm -f /usr/share/doc/google-android-build-tools/copyright.gz
	rm -f /usr/share/doc/google-android-build-tools/source.properties
	for d in `find \
			/usr/bin \
			/usr/share/java \
			/usr/share/doc/google-android-build-tools \
			/usr/share/doc -type d -empty`; do \
		test -d $$d && rmdir $$d || true ; done

$(DL_DIR)/$(UNPACK_DIR)/aapt: $(DL_DIR)/$(PKG_SOURCE)
	cd $(DL_DIR) && unzip $(PKG_SOURCE)

$(DL_DIR)/$(PKG_SOURCE): $(DL_DIR)
	cd $(DL_DIR) && \
		wget --continue $(PKG_SOURCE_URL)
	md5sum -c $(PKG_SOURCE).md5
	sha1sum -c $(PKG_SOURCE).sha1
	sha256sum -c $(PKG_SOURCE).sha256

$(DL_DIR):
	mkdir $(DL_DIR)

clean:
	-rm -rf -- $(UNPACK_DIR)

distclean: clean
	-rm -rf -- $(DL_DIR)

.PHONY: install clean