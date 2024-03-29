AC_PREREQ([2.66])

AC_INIT([dleyna-connector-dbus],
	[0.1.0],
	[https://github.com/01org/dleyna-connector-dbus/issues/new],
	,
	[https://01.org/dleyna/])

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR([src/connector-dbus.c])

AC_PREFIX_DEFAULT(/usr/local)

AM_INIT_AUTOMAKE([subdir-objects])

AM_MAINTAINER_MODE
AM_SILENT_RULES([yes])

DLEYNA_CONNECTOR_DBUS_COMPILER_FLAGS

# Checks for languages.
AC_LANG_C

# Checks for programs.
AC_PROG_CC
AM_PROG_CC_C_O
AC_PROG_MKDIR_P

# Initialize libtool
# Disable generation of static libraries
LT_PREREQ([2.2.6])
LT_INIT([dlopen disable-static])
LT_LANG([C])

# Checks for libraries.
PKG_PROG_PKG_CONFIG(0.16)
PKG_CHECK_MODULES([GLIB], [glib-2.0 >= 2.28])
PKG_CHECK_MODULES([GIO], [gio-2.0 >= 2.28])
PKG_CHECK_MODULES([DBUS], [dbus-1])

# Checks for header files.
AC_CHECK_HEADERS([stdlib.h string.h syslog.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_UINT8_T
AC_HEADER_STDBOOL
AC_TYPE_SIZE_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_CHECK_FUNCS([memset strchr strrchr strstr])

# Define Log Level values
LOG_LEVEL_0=0x00
LOG_LEVEL_1=0x01
LOG_LEVEL_2=0x02
LOG_LEVEL_3=0x04
LOG_LEVEL_4=0x08
LOG_LEVEL_5=0x10
LOG_LEVEL_6=0x20
LOG_LEVEL_7=0x13
LOG_LEVEL_8=0x3F

AC_DEFINE_UNQUOTED([DLEYNA_LOG_LEVEL_DISABLED], [${LOG_LEVEL_0}], [Log level flag for disabled messages])
AC_DEFINE_UNQUOTED([DLEYNA_LOG_LEVEL_ERROR],    [${LOG_LEVEL_1}], [Log level flag for errors])
AC_DEFINE_UNQUOTED([DLEYNA_LOG_LEVEL_CRITICAL], [${LOG_LEVEL_2}], [Log level flag for critical messages])
AC_DEFINE_UNQUOTED([DLEYNA_LOG_LEVEL_WARNING],  [${LOG_LEVEL_3}], [Log level flag for warnings])
AC_DEFINE_UNQUOTED([DLEYNA_LOG_LEVEL_MESSAGE],  [${LOG_LEVEL_4}], [Log level flag for messages])
AC_DEFINE_UNQUOTED([DLEYNA_LOG_LEVEL_INFO],     [${LOG_LEVEL_5}], [Log level flag for informational messages])
AC_DEFINE_UNQUOTED([DLEYNA_LOG_LEVEL_DEBUG],    [${LOG_LEVEL_6}], [Log level flag for debug messages])
AC_DEFINE_UNQUOTED([DLEYNA_LOG_LEVEL_DEFAULT],  [${LOG_LEVEL_7}], [Log level flag to display default level messages])
AC_DEFINE_UNQUOTED([DLEYNA_LOG_LEVEL_ALL],      [${LOG_LEVEL_8}], [Log level flag for all messages])

AC_ARG_ENABLE(master-build,,
		[],
		[master_build=no])

AS_IF([test "x$master_build" = "xno"],
      [PKG_CHECK_MODULES([DLEYNA_CORE], [dleyna-core-1.0 >= 0.0.1])],
      [DLEYNA_CORE_CFLAGS="-I../dleyna-core";
       DLEYNA_CORE_LIBS="-L../dleyna-core/.libs -ldleyna-core-1.0"
      ])

AC_ARG_ENABLE(debug,
		AS_HELP_STRING(
			[--enable-debug],
			[enable compiling with debugging information]),
		[],
		[enable_debug=no])

AS_CASE("${enable_debug}",
	[yes], [CFLAGS="$CFLAGS -g";
		AC_DEFINE_UNQUOTED([DLEYNA_DEBUG_ENABLED],[1], [Compiling with debugging information enabled])
	       ],
	[no], [],
	[AC_MSG_ERROR([bad value ${enable_debug} for --enable-debug])])


AC_ARG_ENABLE(werror,
		AS_HELP_STRING(
			[--enable-werror],
			[warnings are treated as errors]),
		[],
		[enable_werror=no])

AS_CASE("${enable_werror}",
	[yes], [CFLAGS="$CFLAGS -Werror"],
	[no], [],
	[AC_MSG_ERROR([bad value ${enable_werror} for --enable-werror])])


AC_ARG_ENABLE(optimization,
		AS_HELP_STRING(
			[--disable-optimization],
			[disable code optimization through compiler]),
		[],
		[enable_optimization=yes])

AS_CASE("${enable_optimization}",
	[yes], [disable_optimization=no],
	[no], [CFLAGS="$CFLAGS -O0"; disable_optimization=yes],
	[AC_MSG_ERROR([bad value ${enable_optimization} for --enable-werror])])

AC_ARG_WITH(log-level,
		AS_HELP_STRING(
			[--with-log-level],
			[enable logging information (0,1..6,7,8)\
			 0=disabled \
			 7=default (=1,2,5) \
			 8=all (=1,2,3,4,5,6) \
			 1,..,6=a comma separated list of log level\
			]),
		[],
		[with_log_level=7])

DLEYNA_LOG_LEVEL_CHECK([${with_log_level}])

AC_CONFIG_FILES([Makefile	\
		dleyna-connector-dbus-1.0.pc
		])

AC_OUTPUT

AS_ECHO(["-------------------------------------------------

${PACKAGE_NAME} Version ${PACKAGE_VERSION}

Prefix    : '${prefix}'
Compiler  : '${CC}'
CFLAGS    : '${CFLAGS}'

-Package features:
	- enable-werror       : ${enable_werror}
	- enable-debug        : ${enable_debug}
	- disable-optimization: ${disable_optimization}
	- with-log-level      : ${with_log_level}

--------------------------------------------------"])
