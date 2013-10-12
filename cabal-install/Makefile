# https://launchpad.net/builders/

VERSION=1.18.0.2
CHANNEL?=raring

SOURCE_DIR=haskell-cabal-install_$(VERSION)
SOURCE_TARBALL=$(SOURCE_DIR).orig.tar.gz

source: $(SOURCE_DIR)
	cd $(SOURCE_DIR) && PATH=/bin:/usr/bin dpkg-buildpackage  -S

binary: $(SOURCE_DIR)
	cd $(SOURCE_DIR) && PATH=/bin:/usr/bin dpkg-buildpackage

release: $(SOURCE_DIR)
	sed -i 's/) UNRELEASED;/~$(CHANNEL)) $(CHANNEL);/' $(SOURCE_DIR)/debian/changelog
	make source
	sed -i 's/~$(CHANNEL)) $(CHANNEL);/) UNRELEASED;/' $(SOURCE_DIR)/debian/changelog

release_all:
	CHANNEL=precise make release
	CHANNEL=quantal make release
	CHANNEL=raring make release

$(SOURCE_DIR): $(SOURCE_TARBALL)
	tar xvf $(SOURCE_TARBALL) && mv cabal-install-$(VERSION) $@
	cp -r debian $@

$(SOURCE_TARBALL):
	~/.cabal/bin/cabal sdist
	mv dist/cabal-install-$(VERSION).tar.gz $@

clean:
	rm -rf haskell-cabal-install_$(VERSION)* dist/
	rm -f cabal-install_*.deb
