ifndef TUXDIR
$(error TUXDIR is not set)
endif

export PATH:=$(TUXDIR)/bin:$(PATH)
export LD_LIBRARY_PATH:=$(TUXDIR)/lib:$(LD_LIBRARY_PATH)
export TUXCONFIG:=$(CURDIR)/tuxconfig

default: client server tuxconfig
	tmboot -y
	./client "Hello, Fuxedo!"
	tmshutdown -k KILL -y
	tmipcrm -y

ubbconfig: ubbconfig.in
	cat $< \
          | sed s:@TUXDIR@:$(TUXDIR):g \
          | sed s:@UNAME@:`uname -n`:g \
          | sed s:@CURDIR@:$(CURDIR):g > $@

tuxconfig: ubbconfig
	tmloadcf -y $<

server: server.c
	buildserver -o $@ -f $< -s TOUPPER -v

client: client.c
	buildclient -o $@ -f $< -v

clean:
	-tmipcrm -y
	-rm -f *.o ubbconfig tuxconfig client server ULOG.*
