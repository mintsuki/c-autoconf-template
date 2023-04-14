AC_INIT([C Autoconf Template], [m4_esyscmd([./version.sh])], [https://github.com/mintsuki/c-autoconf-template/issues], [c-autoconf-template])

AC_PREREQ([2.69])
AC_CONFIG_AUX_DIR([build-aux])

SRCDIR="$(cd "$srcdir" && pwd -P)"
BUILDDIR="$(pwd -P)"

AC_SUBST([SRCDIR])
AC_SUBST([BUILDDIR])

AC_DEFUN([PROG_ABSPATH], [
    case "$$1" in
        .*) $1="$(cd "$(dirname "$$1")" && pwd -P)/$(basename "$$1")" ;;
    esac
])

test "x$CFLAGS" = "x" && CFLAGS='-g -O2 -pipe'

AC_LANG([C])
AC_PROG_CC
PROG_ABSPATH([CC])

AC_CHECK_HEADERS([stdio.h],
    [], [AC_MSG_ERROR([required header not found])])

werror_state="no"
AC_ARG_ENABLE([werror],
    [AS_HELP_STRING([--enable-werror], [treat warnings as errors])],
    [werror_state="$enableval"])
if test "$werror_state" = "yes"; then
    AC_SUBST([WERROR_FLAG], [-Werror])
else
    AC_SUBST([WERROR_FLAG], [-Wno-error])
fi

AC_PROG_MKDIR_P
PROG_ABSPATH([MKDIR_P])
AC_PROG_INSTALL
PROG_ABSPATH([INSTALL])

AC_CHECK_PROG([FIND_FOUND], [find], [yes])
if ! test "x$FIND_FOUND" = "xyes"; then
    AC_MSG_ERROR([find not found, please install find before configuring])
fi

NATIVE_STRIP="$($CC -dumpmachine)"-strip
AC_CHECK_PROG([NATIVE_STRIP_FOUND], [$NATIVE_STRIP], [yes])
if ! test "x$NATIVE_STRIP_FOUND" = "xyes"; then
    NATIVE_STRIP=strip
fi
AC_SUBST([NATIVE_STRIP])

AC_PREFIX_DEFAULT([/usr/local])
AC_CONFIG_FILES([GNUmakefile])
AC_OUTPUT
