CC	 ?= gcc
CFLAGS	 ?= -O2
PREFIX   ?= /usr/local
WITH_GUI ?= 0
WIN32    ?= 0

#LIBPA	=lib/libportaudio-x86.a
#ifeq ($(shell uname -m),x86_64)
#	LIBPA =lib/libportaudio-x86_64.a
#endif
LIBPA =-lportaudio

BIN	= ppm2tx
OBJS	= ppm.o sys.o
LIBS	= -lpthread  $(LIBPA) -lm -lasound

ifeq ($(WIN32),1)
	WINEPREFIX ?= /home/tomas/.winedevcpp
	CC	= WINEPREFIX=$(WINEPREFIX) wine $(WINEPREFIX)/drive_c/Dev-Cpp/bin/gcc.exe
	LIBS	= -lm -lws2_32 lib/libpthread-win32.dll lib/libportaudio-win32.dll
	GTKCFLAGS = -mms-bitfields -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/include/gtk-2.0 -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/lib/gtk-2.0/include -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/include/atk-1.0 -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/include/cairo -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/include/gdk-pixbuf-2.0 -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/include/pango-1.0 -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/include/glib-2.0 -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/lib/glib-2.0/include -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/include -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/include/freetype2 -I$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/include/libpng14  -L$(WINEPREFIX)/drive_c/gtk+-bundle_2.22.1-20101227_win32/lib -lgtk-win32-2.0 -lgdk-win32-2.0 -latk-1.0 -lgio-2.0 -lpangowin32-1.0 -lgdi32 -lpangocairo-1.0 -lgdk_pixbuf-2.0 -lpango-1.0 -lcairo -lgobject-2.0 -lgmodule-2.0 -lgthread-2.0 -lglib-2.0 -lintl
endif

ifeq ($(MACOSX),1)
	LIBS	= -lm -lpthread -framework CoreAudio -framework AudioToolbox -framework AudioUnit -framework Carbon lib/libportaudio-osx-x86.a
endif

ifeq ($(WITH_GUI),1)
	GTKCFLAGS ?= `pkg-config --cflags --libs gtk+-2.0`
	CFLAGS += -DWITH_GUI=1 $(GTKCFLAGS)
	OBJS += gui.o
else
	OBJS += tui.o
endif

all: $(OBJS) $(BIN)

clean:
	rm -rf *.o *.exe $(BIN)

.c.o:
	$(CC) $(CFLAGS) -c -o$@ $< 

$(BIN): $(OBJS)
	$(CC) -o $@ $(OBJS) $(LIBS) $(CFLAGS)

install:
	install $(BIN) $(PREFIX)/bin/
	mkdir -p $(PREFIX)/share/txppm
	install logo.png $(PREFIX)/share/txppm/
	install icon.png /usr/share/pixmaps/
	install txppm.desktop /usr/share/applications/

uninstall:
	rm -f $(PREFIX)/bin/$(BIN)
	rm -rf $(PREFIX)/share/txppm
	rm -f /usr/share/pixmaps/icon.png
	rm -f /usr/share/applications/txppm.desktop
