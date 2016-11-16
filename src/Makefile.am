# suffixes.c must be created before psl.c is compiled
BUILT_SOURCES = suffixes_dafsa.c
VENDOR ?=community
BUILD_INFO=\#\#1\#\# $(shell date +'%d %b %Y %H:%M:%S')     $(shell uname -n)
BUILDLEVEL_INFO=@\#$(VENDOR):$(LIBPSL_VERSION)\#@$(BUILD_INFO)::::$(LIBPSL_VERSION_PATCH)::

# suffixes.c is a built source that must be cleaned
CLEANFILES = suffixes_dafsa.c

lib_LTLIBRARIES = libpsl.la

libpsl_la_SOURCES = psl.c lookup_string_in_fixed_set.c
libpsl_la_CPPFLAGS = -I$(top_srcdir)/include
# include ABI version information
libpsl_la_LDFLAGS = -version-info $(LIBPSL_SO_VERSION) \
	-buildlevel '$(BUILDLEVEL_INFO)'
if WITH_LIBICU
  libpsl_la_LDFLAGS += $(LIBICU_LIBS) 
endif
if WITH_LIBIDN2
  libpsl_la_LDFLAGS += -lidn2 -lunistring
endif
if WITH_LIBIDN
  libpsl_la_LDFLAGS += -lidn -lunistring
endif

noinst_PROGRAMS = psl2c
psl2c_SOURCES = psl2c.c lookup_string_in_fixed_set.c
psl2c_CPPFLAGS = -I$(top_srcdir)/include -DMAKE_DAFSA=\"$(top_srcdir)/src/psl-make-dafsa\"
if BUILTIN_GENERATOR_LIBICU
  psl2c_LDADD = $(LIBICU_LIBS) $(LTLIBINTL)
endif
if BUILTIN_GENERATOR_LIBIDN2
  psl2c_LDADD = @LTLIBICONV@ -lidn2 -lunistring
endif
if BUILTIN_GENERATOR_LIBIDN
  psl2c_LDADD = @LTLIBICONV@ -lidn -lunistring
endif

# Build rule for suffix_dafsa.c
# PSL_FILE can be set by ./configure --with-psl-file=[PATH]
suffixes_dafsa.c: $(PSL_FILE) psl2c$(EXEEXT)
	./psl2c$(EXEEXT) "$(PSL_FILE)" suffixes_dafsa.c

EXTRA_DIST = psl-make-dafsa LICENSE.chromium

dist_man_MANS = psl-make-dafsa.1
