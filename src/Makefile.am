AM_CPPFLAGS = @VMOD_INCLUDES@ -Wall -I$(abs_srcdir)/mruby/include

vmoddir = @VMOD_DIR@
vmod_LTLIBRARIES = libvmod_mruby.la

LIBMRUBY = $(srcdir)/mruby/build/host/lib/libmruby.a
libvmod_mruby_la_LDFLAGS = -module -export-dynamic -avoid-version -shared -Imruby -Imruby/include -lm
libvmod_mruby_la_LIBADD = $(LIBMRUBY)

libvmod_mruby_la_SOURCES = \
	vmod_mruby.c  \
	vmod_class.h \
	vmod_class.c

nodist_libvmod_mruby_la_SOURCES = \
	vcc_if.c \
	vcc_if.h

vmod_mruby.lo: vcc_if.c vcc_if.h

vcc_if.c: vcc_if.h

vcc_if.h: @VMODTOOL@ $(top_srcdir)/src/vmod_mruby.vcc
	@VMODTOOL@ $(top_srcdir)/src/vmod_mruby.vcc

VMOD_TESTS = $(top_srcdir)/src/tests/*.vtc
.PHONY: $(VMOD_TESTS)

$(top_srcdir)/src/tests/*.vtc: libvmod_mruby.la
	@VARNISHTEST@ -Dvarnishd=@VARNISHD@ -Dvmod_topbuild=$(abs_top_builddir) $@

check: $(VMOD_TESTS)

$(srcdir)/vmod_mruby.c: $(srcdir)/mruby
$(srcdir)/vmod_class.c: $(srcdir)/mruby

MRUBY_CONFIG_FILE = $(abs_top_srcdir)/build_config.rb
$(LIBMRUBY): $(srcdir)/mruby $(MRUBY_CONFIG_FILE)
	MRUBY_CONFIG=$(MRUBY_CONFIG_FILE) $(abs_srcdir)/mruby/minirake -C $(abs_srcdir)/mruby all

$(srcdir)/mruby:
	@GIT@ clone --depth 1 git://github.com/mruby/mruby.git

EXTRA_DIST = \
	vmod_mruby.vcc \
	$(VMOD_TESTS)

CLEANFILES = \
	$(builddir)/vcc_if.c \
	$(builddir)/vcc_if.h \
	$(builddir)/vmod_mruby.rst \
	$(builddir)/vmod_mruby.man.rst
