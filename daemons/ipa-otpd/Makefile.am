AM_CPPFLAGS := -I$(top_srcdir)/util
AM_CFLAGS := @LDAP_CFLAGS@ @LIBVERTO_CFLAGS@ @KRB5_CFLAGS@ @NSPR_CFLAGS@
AM_LDFLAGS := @LDAP_LIBS@ @LIBVERTO_LIBS@ @KRAD_LIBS@ @KRB5_LIBS@

noinst_HEADERS = internal.h
appdir = $(libexecdir)/ipa/
app_PROGRAMS = ipa-otpd
ipa_otpd_LDADD = $(top_builddir)/util/libutil.la
dist_noinst_DATA = ipa-otpd.socket.in ipa-otpd@.service.in test.py
systemdsystemunit_DATA = ipa-otpd.socket ipa-otpd@.service

ipa_otpd_SOURCES = bind.c forward.c main.c parse.c query.c queue.c stdio.c \
                   oauth2.c

%.socket: %.socket.in
	@sed -e 's|@krb5rundir[@]|$(krb5rundir)|g' \
	     -e 's|@KRB5KDC_SERVICE[@]|$(KRB5KDC_SERVICE)|g' \
	     -e 's|@UNLINK[@]|@UNLINK@|g' \
	     $< > $@

%.service: %.service.in
	@sed -e 's|@libexecdir[@]|$(libexecdir)|g' \
	     -e 's|@sysconfdir[@]|$(sysconfdir)|g' \
	     $< > $@

CLEANFILES = $(systemdsystemunit_DATA)

TESTS =
check_PROGRAMS =

if HAVE_CMOCKA
TESTS += queue_tests
check_PROGRAMS += queue_tests
endif

queue_tests_SOURCES = ipa_otpd_queue_cmocka_tests.c queue.c
queue_tests_CFLAGS = $(CMOCKA_CFLAGS)
queue_tests_LDADD = $(CMOCKA_LIBS)
