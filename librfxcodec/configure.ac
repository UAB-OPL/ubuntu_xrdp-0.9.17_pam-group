# Process this file with autoconf to produce a configure script

AC_PREREQ(2.59)
AC_INIT([rfxcodec], [0.1.5], [xrdp-devel@googlegroups.com])
AC_CONFIG_HEADERS(config_ac.h:config_ac-h.in)
AM_INIT_AUTOMAKE([1.6 foreign])
AC_CONFIG_MACRO_DIR([m4])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])
AC_PROG_CC
AC_SYS_LARGEFILE
AC_FUNC_FSEEKO
AC_C_CONST
AC_PROG_LIBTOOL
PKG_INSTALLDIR

AX_CFLAGS_WARN_ALL
AX_APPEND_COMPILE_FLAGS([-Wwrite-strings])

# SIMD is optional
AC_ARG_WITH([simd],
    AC_HELP_STRING([--without-simd],[Omit SIMD extensions.]))
if test "x${with_simd}" != "xno"; then
  # Check if we're on a supported CPU
  AC_MSG_CHECKING([if we have SIMD optimisations for cpu type])
  case "$host_cpu" in
    x86_64 | amd64)
      AC_MSG_RESULT([yes (x86_64)])
      AC_PROG_NASM
      simd_arch=x86_64
      AC_DEFINE([RFX_USE_ACCEL_AMD64], [1], [Use x86_64 SIMD instructions])
    ;;
    i*86 | x86 | ia32)
      AC_MSG_RESULT([yes (i386)])
      AC_PROG_NASM
      simd_arch=i386
      AC_DEFINE([RFX_USE_ACCEL_X86], [1], [Use x86 SIMD instructions])
    ;;
    *)
      AC_MSG_RESULT([no ("$host_cpu")])
      AC_MSG_WARN([SIMD support not available for this CPU.  Performance will suffer.])
    ;;
  esac
fi

AM_CONDITIONAL(WITH_SIMD_AMD64, [test x$simd_arch = xx86_64])
AM_CONDITIONAL(WITH_SIMD_X86, [test x$simd_arch = xi386])

AC_CONFIG_FILES([Makefile
                 include/Makefile
                 src/Makefile
                 src/amd64/Makefile
                 src/x86/Makefile
                 tests/Makefile
                 rfxcodec.pc
                 rfxcodec-uninstalled.pc
])

CPPFLAGS="-include config_ac.h $CPPFLAGS"
AC_OUTPUT

