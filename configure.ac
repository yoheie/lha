dnl Process this file with autoconf to produce a configure script.
AC_INIT(lha, 1.14i)
AC_CANONICAL_TARGET
AM_INIT_AUTOMAKE
AM_CONFIG_HEADER(config.h)

AC_DEFINE_UNQUOTED(PLATFORM, "$ac_cv_host",
		   [the type of system on which the package will run.])

dnl Checks for programs.
AC_PROG_CC
AC_PROG_GCC_TRADITIONAL
AC_PROG_INSTALL
AC_PROG_MAKE_SET

dnl on MINIX, define _MINIX and _POSIX_SOURCE
AC_MINIX

dnl Checks for libraries.

dnl Checks for header files.
AC_HEADER_DIRENT
AC_HEADER_STDC
AC_CHECK_HEADERS(fcntl.h limits.h sys/file.h sys/time.h)

dnl Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_TIME
AC_STRUCT_TM
AC_STRUCT_TIMEZONE

AC_CACHE_CHECK([for tm_gmtoff in struct tm], ac_cv_struct_tm_gmtoff,
[AC_TRY_COMPILE([
#if TM_IN_SYS_TIME
#include <sys/time.h>
#else
#include <time.h>
#endif
  ], [struct tm t; t.tm_gmtoff;],
  ac_cv_struct_tm_gmtoff=yes, ac_cv_struct_tm_gmtoff=no)
])

if test "x$ac_cv_struct_tm_gmtoff" = xyes; then
  AC_DEFINE(HAVE_TM_GMTOFF, 1, [Define if your struct tm has tm_gmtoff])
fi

AC_CACHE_CHECK([for st_ino in struct stat], ac_cv_struct_st_ino,
[AC_TRY_COMPILE([
#include <sys/types.h>
#include <sys/stat.h>
  ], [struct stat s; s.st_ino;],
  ac_cv_struct_st_ino=yes, ac_cv_struct_st_ino=no)
])

if test "x$ac_cv_struct_st_ino" = xyes; then
  AC_DEFINE(HAVE_ST_INO, 1, [Define if your struct stat has st_ino])
fi

dnl Checks for library functions.
AC_TYPE_SIGNAL
AC_FUNC_UTIME_NULL
AC_CHECK_FUNCS(strchr memcpy)
AC_CHECK_FUNCS(mktime timelocal tzset ftime gettimeofday)
AC_CHECK_FUNCS(mkdir rmdir strdup memset ftruncate lchown mkstemp)
AC_CHECK_FUNCS(strcasecmp link)

if test $ac_header_dirent = no; then
  AC_LIBOBJ(lhdir)
fi

dnl support kanji code conversion
AC_SUBST(DEF_KCODE)
AC_ARG_ENABLE(text_conv,
  [  --enable-text-conv      support text code convert from/to EUC [default=yes]],
  , enable_text_conv=yes)

if test $enable_text_conv = yes; then
  DEF_KCODE=-DEUC
fi

# force support -lh7-
AC_SUBST(SUPPORT_LZHUFF_METHOD)
SUPPORT_LZHUFF_METHOD=-DSUPPORT_LH7

dnl whether use the -lh567- method
AC_MSG_CHECKING(default archive method is )
AC_ARG_WITH(default_method,
  [  --with-default-method=[567]  create the -lh[567]- archive default [default=5]],
  [case $withval in
   [567]) ;;
   *) AC_MSG_ERROR(you should have specified 5, 6 or 7);;
   esac],
  with_default_method=5)

AC_MSG_RESULT(-lh$with_default_method-)
AC_DEFINE_UNQUOTED(DEFAULT_LZHUFF_METHOD, LZHUFF${with_default_method}_METHOD_NUM, [Define it to 5, 6 or 7 which you want to use -lhX- method, default])

dnl decide path names
AC_MSG_CHECKING(template of the temporary file is )
AC_ARG_WITH(tmp_file,
  [  --with-tmp-file=TEMPLATE temporary file template [default=/tmp/lhXXXXXX]],
  [case $withval in
   yes|no) AC_MSG_ERROR(you should have specified path name);;
   esac],
  with_tmp_file=/tmp/lhXXXXXX)

AC_MSG_RESULT($with_tmp_file)
AC_DEFINE_UNQUOTED(TMP_FILENAME_TEMPLATE, "$with_tmp_file",
  [temporary file template])

dnl incremental indicator
AC_ARG_ENABLE(indicator,
  [  --enable-indicator      need incremental indicator [default=yes]],
  ,
  # default
  enable_indicator=yes)

if test "x$enable_indicator" = xyes; then
  AC_DEFINE(NEED_INCREMENTAL_INDICATOR, 1, [Define you want to use the incremental indicator])
fi

dnl support multibyte filename
AC_ARG_ENABLE(multibyte-char,
  [  --enable-multibyte-char support multibyte(sjis) filename [default=yes]],
  ,
  # default
  enable_multibyte_char=yes)

if test "x$enable_multibyte_char" = xyes; then
  AC_DEFINE(MULTIBYTE_CHAR, 1, [Define you want to use multibyte filename])
fi

AC_CONFIG_FILES([Makefile src/Makefile man/Makefile])
AC_OUTPUT