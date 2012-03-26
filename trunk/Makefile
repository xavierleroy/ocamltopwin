CONFIG=`ocamlc -where`/Makefile.config
-include Makefile.local

include $(CONFIG)

DESTDIR=$(PREFIX)
CC=$(BYTECC)
CFLAGS=$(BYTECCCOMPOPTS)

OBJS=startocaml.$(O) ocamlres.$(O) ocaml.$(O) menu.$(O) \
  history.$(O) editbuffer.$(O)

LIBS=$(call SYSLIB,kernel32) $(call SYSLIB,advapi32) $(call SYSLIB,gdi32) \
     $(call SYSLIB,user32) $(call SYSLIB,comdlg32) $(call SYSLIB,comctl32)

all: ocamlwin.exe

ocamlwin.exe: $(OBJS)
	$(MKEXE) -o ocamlwin.exe $(OBJS) $(LIBS) $(EXTRALIBS) -subsystem windows

ocamlres.$(O): ocaml.rc ocaml.ico
ifeq ($(TOOLCHAIN),msvc)
	rc ocaml.rc
ifeq ($(ARCH),amd64)
	cvtres /nologo /machine:amd64 /out:$@ ocaml.res
else
	cvtres /nologo /machine:ix86 /out:$@ ocaml.res
endif
	rm -f ocaml.res
endif
ifeq ($(TOOLCHAIN),mingw)
	$(TOOLPREF)windres -i ocaml.rc -o $@
endif

$(OBJS): inria.h inriares.h history.h editbuffer.h

clean:
	rm -f ocamlwin.exe *.$(O) *.pdb ocamlwin.ilk

install:
	cp ocamlwin.exe $(DESTDIR)/OCamlWin.exe

.SUFFIXES: .c .$(O)

.c.$(O):
	$(CC) $(CFLAGS) -c $*.c
