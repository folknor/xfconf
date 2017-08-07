
AM_CPPFLAGS = \
	-I$(top_srcdir) \
	-DLIBXFCONF_COMPILATION \
	-DG_LOG_DOMAIN=\"xfconf\" \
	-Wno-expansion-to-defined \
	$(PLATFORM_CPPFLAGS)

lib_LTLIBRARIES = libxfconf-0.la

libxfconfincludedir = $(includedir)/xfce4/xfconf-$(LIBXFCONF_VERSION_API)/xfconf

libxfconfinclude_HEADERS = \
	xfconf-binding.h \
	xfconf-channel.h \
	xfconf-errors.h \
	xfconf-types.h \
	xfconf.h

libxfconf_0_la_SOURCES = \
	$(libxfconfinclude_HEADERS) \
	xfconf-binding.c \
	xfconf-cache.h \
	xfconf-cache.c \
	xfconf-channel.c \
	xfconf-private.h \
	xfconf.c \
	$(top_srcdir)/common/xfconf-types.c

libxfconf_0_la_CFLAGS = \
	$(GLIB_CFLAGS) \
	$(GIO_CFLAGS) \
	$(GIO_UNIX_CFLAGS) \
	$(GTHREAD_CFLAGS) \
	$(PLATFORM_CFLAGS)

libxfconf_0_la_LDFLAGS = \
	-export-dynamic \
	-version-info $(LIBXFCONF_VERINFO) \
	-export-symbols-regex "^[^_].*" \
	-no-undefined \
	$(PLATFORM_LDFLAGS)

libxfconf_0_la_LIBADD = \
	$(top_builddir)/common/libxfconf-common.la \
	$(top_builddir)/common/libxfconf-gvaluefuncs.la \
	$(top_builddir)/common/libxfconf-dbus.la \
	$(GIO_LIBS) \
	$(GIO_UNIX_LIBS) \
	$(GLIB_LIBS) \
	$(GTHREAD_LIBS)

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = libxfconf-0.pc

# GObject Introspection
include $(INTROSPECTION_MAKEFILE)
INTROSPECTION_GIRS =
INTROSPECTION_SCANNER_ENV = CC="$(CC)"
INTROSPECTION_SCANNER_ARGS = \
	--add-include-path=$(srcdir) \
	--add-include-path=$(prefix)/share/gir-1.0 \
	--warn-all \
	--identifier-prefix=Xfconf \
	--identifier-prefix=xfconf \
	--identifier-prefix=Xfce \
	--identifier-prefix=xfce
INTROSPECTION_COMPILER_ARGS = \
	--includedir=$(srcdir) \
	--includedir=$(prefix)/share/gir-1.0 \
	--includedir=.

if HAVE_INTROSPECTION
libxfconf-0.gir: libxfconf-0.la Makefile
libxfconf_0_gir_INCLUDES = GObject-2.0 GLib-2.0
libxfconf_0_gir_CFLAGS = $(AM_CPPFLAGS)
libxfconf_0_gir_LIBS = libxfconf-0.la
libxfconf_0_gir_FILES = xfconf-binding.h xfconf-channel.h xfconf-errors.h xfconf-types.h xfconf.h xfconf-binding.c xfconf-channel.c xfconf.c
libxfconf_0_gir_EXPORT_PACKAGES = libxfconf-0
INTROSPECTION_GIRS += libxfconf-0.gir

girdir = $(datadir)/gir-1.0
gir_DATA = $(INTROSPECTION_GIRS)

typelibdir = $(libdir)/girepository-1.0
typelib_DATA = $(INTROSPECTION_GIRS:.gir=.typelib)

DISTCLEANFILES = $(gir_DATA) $(typelib_DATA)
endif
# endif HAVE_INTROSPECTION

EXTRA_DIST = \
	abicheck.sh \
	xfconf.symbols

if ENABLE_VAPIGEN
-include $(VAPIGEN_MAKEFILE)

libxfconf-0.vapi: libxfconf-0.gir

VAPIGEN_VAPIS = libxfconf-0.vapi
VAPIGEN_GIRDIRS = $(prefix)/share/gir-1.0

libxfconf_0_vapi_METADATADIRS = .
libxfconf_0_vapi_FILES = libxfconf-0.gir

vapidir = $(datadir)/vala/vapi
vapi_DATA = $(VAPIGEN_VAPIS)

EXTRA_DIST += libxfconf-0.metadata
endif

# required for gtk-doc
dist-hook: all

if HAVE_GNUC_VISIBILITY
TESTS = abicheck.sh
endif
