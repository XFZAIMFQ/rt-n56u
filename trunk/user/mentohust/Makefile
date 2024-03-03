SRC_NAME = mentohust-master
SRC_URL = https://github.com/AutoCONFIG/mentohust-padavan/archive/refs/heads/master.zip
THISDIR = $(shell pwd)
DIR_NAME = mentohust-padavan-master

LDFLAGS += -L$(STAGEDIR)

all: download_test extract_test config_test
	$(MAKE) -C $(DIR_NAME)

download_test: 
	( if [ ! -f $(THISDIR)/$(SRC_NAME).zip ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_NAME).zip $(SRC_URL); \
	fi )
	
extract_test:
	( if [ ! -d $(DIR_NAME) ]; then \
		unzip -qq $(SRC_NAME).zip; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(DIR_NAME) ; \
	./autogen.sh ; \
	am_cv_func_iconv=no \
	./configure \
		--prefix=/usr \
		--with-pcap=dylib \
		--disable-notify \
		--disable-encodepass \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(DIR_NAME)/Makefile ] ; then \
		$(MAKE) -C $(DIR_NAME) distclean ; \
	fi ; \
	rm -f config_done

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/$(DIR_NAME)/src/mentohust /usr/bin/mentohust
	$(ROMFSINST) -p +x $(THISDIR)/scripts/mentohust.sh /usr/bin/mentohust.sh

