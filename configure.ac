AC_PREREQ([2.61])
AC_INIT([media-ctl], [0.0.1], [laurent.pinchart@ideasonboard.com])
AC_CONFIG_SRCDIR([src/main.c])
AC_CONFIG_AUX_DIR([config])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

AM_INIT_AUTOMAKE([-Wall -Werror foreign])

AC_MSG_ERROR([
    --------------------------------------------------------------------
    The media-ctl utility and libraries have been moved to the v4l-utils
    package available at
    
        git://linuxtv.org/v4l-utils.git
    
    My v4l-utils development tree can be found at
    
        git://linuxtv.org/pinchartl/v4l-utils.git
    
    This git tree is deprecated and will not be updated.
    --------------------------------------------------------------------
])

# Checks for programs.
AC_PROG_CC
AM_PROG_CC_C_O
# automake 1.12 seems to require this, but automake 1.11 doesn't recognize it
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])
AC_PROG_LIBTOOL

# Checks for libraries.

AC_ARG_WITH([libudev],
    AS_HELP_STRING([--with-libudev],
        [Enable libudev to detect a device name]))

AS_IF([test "x$with_libudev" = "xyes"],
    [PKG_CHECK_MODULES(libudev, libudev, have_libudev=yes, have_libudev=no)],
    [have_libudev=no])

AS_IF([test "x$have_libudev" = "xyes"],
    [
        AC_DEFINE([HAVE_LIBUDEV], [], [Use libudev])
        LIBUDEV_CFLAGS="$libudev_CFLAGS"
        LIBUDEV_LIBS="$libudev_LIBS"
        AC_SUBST(LIBUDEV_CFLAGS)
        AC_SUBST(LIBUDEV_LIBS)
    ],
    [AS_IF([test "x$with_libudev" = "xyes"],
        [AC_MSG_ERROR([libudev requested but not found])
    ])
])


# Kernel headers path.
AC_ARG_WITH(kernel-headers,
    [AC_HELP_STRING([--with-kernel-headers=DIR],
        [specify path of Linux kernel headers [/usr/src/kernel-headers]])],
    [case "${withval}" in
        yes | no) AC_MSG_ERROR([bad value ${withval} for --with-kernel-headers]) ;;
        *)   KERNEL_HEADERS_DIR="${withval}" ;;
     esac],
    [KERNEL_HEADERS_DIR="/usr/src/kernel-headers"])

CPPFLAGS="$CPPFLAGS -I$KERNEL_HEADERS_DIR/include"

# Checks for header files.
AC_CHECK_HEADERS([linux/media.h \
		  linux/types.h \
		  linux/v4l2-mediabus.h \
		  linux/v4l2-subdev.h \
		  linux/videodev2.h],
		  [],
		  [echo "ERROR: Kernel header file not found or not usable!"; exit 1])

AC_CHECK_HEADERS([fcntl.h \
		  stdlib.h \
		  string.h \
		  sys/ioctl.h \
		  sys/time.h \
		  unistd.h],
		  [],
		  [echo "ERROR: Header file not found or not usable!"; exit 1])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_INLINE
AC_TYPE_SIZE_T
AC_CHECK_MEMBERS([struct stat.st_rdev])

# Checks for library functions.
AC_HEADER_MAJOR
AS_IF([test "x$cross_compiling" != "xyes"],
    [
        AC_FUNC_MALLOC
        AC_FUNC_REALLOC
    ])
AC_CHECK_FUNCS([memset strerror strrchr strtoul])

AC_CONFIG_FILES([
 Makefile
 src/Makefile
 libmediactl.pc
 libv4l2subdev.pc
])

AC_OUTPUT

