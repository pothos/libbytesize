# configure.ac for libbytesize

AC_INIT([libbytesize], [0.10], [vpodzime@redhat.com])

# Disable building static libraries.
# This needs to be set before initializing automake
AC_DISABLE_STATIC

AM_INIT_AUTOMAKE([foreign -Wall -Werror -Wno-syntax -Wno-portability])
AC_CONFIG_MACRO_DIR([m4])

# Check for the gettext programs
AC_PATH_PROG([XGETTEXT], [xgettext])
AC_PATH_PROG([MSGFMT], [msgfmt])
AC_PATH_PROG([MSGMERGE], [msgmerge])
AC_PATH_PROG([MSGCAT], [msgcat])
AS_IF([test -z "$XGETTEXT" -o -z "$MSGFMT" -o -z "$MSGMERGE" -o -z "$MSGCAT"],
      [AC_MSG_FAILURE([gettext not found])])

# Define this so gettext.h works without requiring the whole gettext macro
CFLAGS="${CFLAGS} -DENABLE_NLS"

AM_PATH_PYTHON

AM_PROG_AR
AC_PROG_CC

LT_INIT

AC_CONFIG_FILES([Makefile src/Makefile src/bytesize.pc \
                 po/Makefile \
                 src/python/Makefile \
                 dist/Makefile dist/libbytesize.spec \
                 docs/Makefile docs/libbytesize-docs.xml \
                 tests/Makefile])

AC_CONFIG_FILES([tests/libbytesize_unittest.sh],
                [chmod +x tests/libbytesize_unittest.sh])

AC_CONFIG_FILES([tests/canary_tests.sh],
                [chmod +x tests/canary_tests.sh])


LIBBYTESIZE_PKG_CHECK_MODULES([PCRE], [libpcre >= 8.32])

AC_CHECK_LIB(gmp, __gmpz_init)

AC_CHECK_HEADERS([langinfo.h gmp.h mpfr.h stdint.h stdbool.h stdarg.h string.h stdio.h ctype.h],
                 [],
                 [LIBBYTESIZE_SOFT_FAILURE([Header file $ac_header not found.])],
                 [])

AC_ARG_WITH([python3],
    AS_HELP_STRING([--with-python3], [support python3 @<:@default=check@:>@]),
    [],
    [with_python3=check])

AC_SUBST(WITH_PYTHON3, 0)
if test "x$with_python3" != "xno"; then
    AC_PATH_PROG([python3], [python3], [no])
    AS_IF([test "x$python3" == "xno"],
    [if test "x$with_python3" = "xyes"; then
      LIBBYTESIZE_SOFT_FAILURE([Python3 support requested, but python3 is not available])
      fi],
    [AC_SUBST(WITH_PYTHON3, 1)])
fi
AM_CONDITIONAL(WITH_PYTHON3, test "x$with_python3" != "xno" -a "x$python3" != "xno")


AC_ARG_WITH([gtk-doc],
    AS_HELP_STRING([--with-gtk-doc], [generate documentation with gtk-doc @<:@default=check@:>@]),
    [],
    [with_gtk_doc=check])

AC_SUBST(WITH_GTK_DOC, 0)
if test "x$with_gtk_doc" != "xno"; then
    AC_PATH_PROG([gtkdoc_scan], [gtkdoc-scan], [no])
    AS_IF([test "x$gtkdoc_scan" == "xno"],
    [if test "x$with_gtk_doc" = "xyes"; then
      LIBBYTESIZE_SOFT_FAILURE([Building documentation with gtk-doc requested, but not available])
      fi],
      [AC_SUBST(WITH_GTK_DOC, 1)])
fi
AM_CONDITIONAL(WITH_GTK_DOC, test "x$with_gtk_doc" != "xno" -a "x$gtkdoc_scan" != "xno")

AC_OUTPUT
LIBBYTESIZE_FAILURES
