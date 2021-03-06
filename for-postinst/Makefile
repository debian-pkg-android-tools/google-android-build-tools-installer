PKG_NAME:=build-tools
PKG_VERSION:=23.0.3

PKG_SOURCE:=build-tools_r${PKG_VERSION}-linux.zip
PKG_SOURCE_URL:=${DL_MIRROR}/android/repository/${PKG_SOURCE}

DL_DIR=/var/cache/google-android-build-tools-installer
UNPACK_DIR=$(DL_DIR)/android-6.0
DOC_DIR=/usr/share/doc/google-android-build-tools

all: $(DL_DIR)/$(UNPACK_DIR)/aapt
	sed -i 's,^libdir=.*,libdir=/usr/share/java,' $(UNPACK_DIR)/dx

install: all
	install -d -m0755 /usr/share/java
	install -m0644 $(UNPACK_DIR)/lib/dx.jar /usr/share/java/
	install -d -m0755 /usr/bin
	install -m0755 \
		$(UNPACK_DIR)/aapt \
		$(UNPACK_DIR)/aidl \
		$(UNPACK_DIR)/arm-linux-androideabi-ld \
		$(UNPACK_DIR)/bcc_compat \
		$(UNPACK_DIR)/dexdump \
		$(UNPACK_DIR)/dx \
		$(UNPACK_DIR)/i686-linux-android-ld \
		$(UNPACK_DIR)/llvm-rs-cc \
		$(UNPACK_DIR)/mipsel-linux-android-ld \
		$(UNPACK_DIR)/split-select \
		$(UNPACK_DIR)/zipalign \
		/usr/bin/
	install -d -m0755 /usr/lib/
	for f in lib/libLLVM.so lib/libbcinfo.so lib/libbcc.so lib/libclang.so; do \
		test -e /usr/lib/$$f || install -m0644 $(UNPACK_DIR)/$$f /usr/lib/; done
	install -d -m0755 $(DOC_DIR)
	gzip -9 --stdout $(UNPACK_DIR)/NOTICE.txt > $(DOC_DIR)/copyright.gz
	install -m0644 $(UNPACK_DIR)/source.properties $(DOC_DIR)/
	for f in \
		$(DOC_DIR) \
		$(DOC_DIR)/copyright.gz \
		$(DOC_DIR)/source.properties \
		/usr/bin \
		/usr/bin/aapt \
		/usr/bin/aidl \
		/usr/bin/arm-linux-androideabi-ld \
		/usr/bin/bcc_compat \
		/usr/bin/dexdump \
		/usr/bin/dx \
		/usr/bin/i686-linux-android-ld \
		/usr/bin/llvm-rs-cc \
		/usr/bin/mipsel-linux-android-ld \
		/usr/bin/split-select \
		/usr/bin/zipalign \
		/usr/lib \
		/usr/lib/libbcc.so \
		/usr/lib/libbcinfo.so \
		/usr/lib/libclang.so \
		/usr/lib/libLLVM.so \
		/usr/share/java \
		/usr/share/java/dx.jar; do echo $$f; done \
	 >> /var/lib/dpkg/info/google-android-build-tools-installer.list

$(DL_DIR)/$(UNPACK_DIR)/aapt: $(DL_DIR)/$(PKG_SOURCE)
	cd $(DL_DIR) && unzip -ou $(PKG_SOURCE)

$(DL_DIR)/$(PKG_SOURCE): $(DL_DIR)
	cd $(DL_DIR) && \
		su nobody -s /bin/sh -c "wget --continue $(PKG_SOURCE_URL)"
	sha1sum -c $(PKG_SOURCE).sha1

$(DL_DIR):
	mkdir $(DL_DIR)

clean:
	-rm -rf -- $(UNPACK_DIR)

distclean: clean
	-rm -rf -- $(DL_DIR)

.PHONY: install clean
