# $Id: Makefile.am,v 1.51 2011/06/08 09:25:53 freddy77 Exp $
SUBDIRS		= unittests
AM_CPPFLAGS	=	-I$(top_srcdir)/include $(ODBC_INC)

lib_LTLIBRARIES	=	libtdsodbc.la
##EXTRA_LTLIBRARIES	=	libtdsodbc.la
if MINGW32
MINGW_SOURCES	= ../../win32/winlogin.c ../../win32/winsetup.c ../../win32/initnet.c
else
MINGW_SOURCES	=
endif
libtdsodbc_la_SOURCES=	odbc.c connectparams.c convert_tds2sql.c \
	descriptor.c prepare_query.c odbc_util.c \
	native.c sql2tds.c error.c odbc_checks.c sqlwchar.c sqlwparams.h $(MINGW_SOURCES)
if MINGW32
libtdsodbc_la_LIBADD=	../../win32/setup.res ../tds/libtds.la ../replacements/libreplacements.la $(ODBCINSTLIB) \
	$(NETWORK_LIBS) $(LTLIBICONV) $(FREETDS_LIBGCC)
libtdsodbc_la_LDFLAGS = -Wl,--kill-at -Wl,--enable-stdcall-fixup -Wl,-s -Wl,$(top_srcdir)/win32/FreeTDS.def -Wl,../../win32/setup.res $(FREETDS_SYMBOLIC)

.rc.res:
	$(RC) -i $< -I $(top_builddir)/win32 --input-format=rc -o $@ -O coff
else
libtdsodbc_la_LIBADD=	../tds/libtds.la ../replacements/libreplacements.la $(ODBCINSTLIB) $(NETWORK_LIBS) $(LTLIBICONV) $(FREETDS_LIBGCC)
if MACOSX
SYMBOLS		=
else
SYMBOLS		=	-export-symbols-regex '^(SQL|ODBCINST).*'
endif
# -module is needed by Darwin (Mac OS X)
libtdsodbc_la_LDFLAGS = $(SYMBOLS) $(FREETDS_SYMBOLIC) $(FREETDS_ODBC_MODULE)
endif

## Need blank statement to avoid compiling odbc.c
odbc: $(EXTRA_LTLIBRARIES)
	@echo ''

