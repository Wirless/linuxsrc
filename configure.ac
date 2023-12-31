AC_PREREQ([2.50])
AC_INIT([TheForgottenServer], [0.4])
AM_INIT_AUTOMAKE([1.10 foreign])
AC_CONFIG_SRCDIR([account.h])
AM_CONFIG_HEADER([config.h])

AC_PROG_CXX
AC_LANG(C++)
# add own flags in here
OPTIONAL_FLAGS=""

# check if we want server diag enabled
serverDiag=no
AC_ARG_ENABLE(server-diag, [  --enable-server-diag		enable server diagnostic],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__ENABLE_SERVER_DIAGNOSTIC__"]
	serverDiag=yes
)

# check if we want login server mode
AM_CONDITIONAL(LOGIN_SERVER, false)
loginServer=no
AC_ARG_ENABLE(login-server, [  --enable-login-server		enable login server mode],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__LOGIN_SERVER__"]
	AM_CONDITIONAL(LOGIN_SERVER, true)
	loginServer=yes
)

# check if we want OTAdmin protocol
AM_CONDITIONAL(OT_ADMIN, false)
otAdmin=no
AC_ARG_ENABLE(ot-admin, [  --enable-ot-admin		enable OTAdmin protocol],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__OTADMIN__"]
	AM_CONDITIONAL(OT_ADMIN, true)
	otAdmin=yes
)

# check if we want otserv allocator enabled
otservAllocator=no
AC_ARG_ENABLE(otserv-allocator, [  --enable-otserv-allocator	enable otserv custom allocator],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__OTSERV_ALLOCATOR__"]
	otservAllocator=yes
)

# check if we want homedir conf enabled
homedirConf=no
AC_ARG_ENABLE(homedir-conf, [  --enable-homedir-conf		enable home directory configuration],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__HOMEDIR_CONF__"]
	homedirConf=yes
)

# check do we want to allow running on root user
rootPermission=no
AC_ARG_ENABLE(root-permission, [  --enable-root-permission	enable running on root user],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__ROOT_PERMISSION__"]
	rootPermission=yes
)

# check do we want to be a login server for multiworld
loginServer=no
AC_ARG_ENABLE(login-server, [  --enable-login-server  enable running in a login server mode],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__LOGIN_SERVER__"]
	loginServer=yes
)

# check if we want to use luajit instead of lua
luaJIT=no
AC_ARG_ENABLE(luajit, [  --enable-luajit		enable luajit instead of lua],
	[OPTIONAL_FLAGS="$OPTIONAL_FLAGS -D__LUAJIT__"]
	luaJIT=yes
)
AC_SUBST(OPTIONAL_FLAGS)

# check if we want a debug build
debugBuild=no
AC_ARG_ENABLE(debug, [  --enable-debug		enable debuging],
	[DEBUG_FLAGS="-D__DEBUG__ -D__DEBUG_MOVESYS__ -D__DEBUG_CHAT__ -D__DEBUG_EXCEPTION_REPORT__ -D__DEBUG_HOUSES__ -D__DEBUG_LUASCRIPTS__ -D__DEBUG_MAILBOX__ -D__DEBUG_NET__ -D__DEBUG_NET_DETAIL__ -D__DEBUG_RAID__ -D__DEBUG_SCHEDULER__ -D__DEBUG_SPAWN__ -D__SQL_QUERY_DEBUG__ -O0 -g3"]
	debugBuild=yes
)
if test "$debugBuild" = "no" ; then
	DEBUG_FLAGS="-O2 -fomit-frame-pointer" # disable all kinds of debugging
fi

AC_SUBST(DEBUG_FLAGS)

AM_CONDITIONAL(USE_MYSQL, false)
useMySQL=no
AM_CONDITIONAL(USE_SQLITE, false)
useSQLite=no

# check if we want mysql enabled
AC_ARG_ENABLE(mysql, [  --enable-mysql		enable MySQL support], [
	AM_CONDITIONAL(USE_MYSQL, true)
	MYSQL_FLAGS=-D__USE_MYSQL__
	AC_SUBST(MYSQL_FLAGS)
	useMySQL=yes
])

# check if we want sqlite enabled
AC_ARG_ENABLE(sqlite, [  --enable-sqlite		enable SQLite support], [
	AM_CONDITIONAL(USE_SQLITE, true)
	SQLITE_FLAGS=-D__USE_SQLITE__
	AC_SUBST(SQLITE_FLAGS)
	useSQLite=yes
])

# check if we want the profiler
AC_ARG_ENABLE(profiler, [  --enable-profiler		enable profiler support], [PROFILER_FLAGS=-pg])
AC_SUBST(PROFILER_FLAGS)

# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([arpa/inet.h netdb.h netinet/in.h stddef.h stdint.h stdlib.h sys/socket.h sys/timeb.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_CONST
AC_C_INLINE
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT64_T
AC_TYPE_SIZE_T
AC_STRUCT_TM
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T
AC_CHECK_TYPES([ptrdiff_t])

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_MEMCMP
AC_FUNC_REALLOC
AC_CHECK_FUNCS([ceil floor ftime gethostbyname gethostname memset pow sqrt strcasecmp strncasecmp strstr strtol])

# check for xml2
AM_PATH_XML2(2.6.5, , AC_MSG_ERROR([You need libxml2 >= 2.6.5 to compile theforgottenserver!]))
LIBS="$LIBS $XML_LIBS"

# check for gmp
AC_CHECK_HEADERS([gmp.h], ,[AC_MSG_ERROR("gmp header not found.")])
AC_CHECK_LIB(gmp, __gmpz_init2, ,[AC_MSG_ERROR("Linking against gmp library failed.")])

# check for zlib
AC_CHECK_HEADERS([zlib.h], ,[AC_MSG_ERROR("zlib header not found.")])
AC_CHECK_LIB(z, main, ,[AC_MSG_ERROR("Linking against zlib library failed.")])

# check for boost
AC_CHECK_LIB(boost_thread-gcc-mt, main, , [
	AC_CHECK_LIB(boost_thread-mt, main, , [
		AC_CHECK_LIB(boost_thread, main, , [
			AC_MSG_ERROR("Linking against boost::thread library failed.")
		])
	])
])

AC_CHECK_LIB(boost_regex-gcc-mt, main, , [
	AC_CHECK_LIB(boost_regex-mt, main, , [
		AC_CHECK_LIB(boost_regex, main, , [
			AC_MSG_ERROR("Linking against boost::regex library failed.")
		])
	])
])

AC_CHECK_LIB(boost_system-gcc-mt, main, , [
	AC_CHECK_LIB(boost_system-mt, main, , [
		AC_CHECK_LIB(boost_system, main, , [
			AC_MSG_ERROR("Linking against boost::system library failed.")
		])
	])
])

AC_CHECK_LIB(boost_date_time-gcc-mt, main, , [
	AC_CHECK_LIB(boost_date_time-mt, main, , [
		AC_CHECK_LIB(boost_date_time, main, , [
			AC_MSG_ERROR("Linking against boost::date-time library failed.")
		])
	])
])

AC_CHECK_LIB(boost_filesystem-gcc-mt, main, , [
	AC_CHECK_LIB(boost_filesystem-mt, main, , [
		AC_CHECK_LIB(boost_filesystem, main, , [
			AC_MSG_ERROR("Linking against boost::filesystem library failed.")
		])
	])
])

# check for boost::asio
AC_CHECK_HEADERS([boost/asio.hpp], , [AC_MSG_ERROR("boost::asio header not found.")])

# check for boost::unordered_set
AC_CHECK_HEADERS([boost/unordered_set.hpp], , [AC_MSG_ERROR("boost::unordered_set header not found.")])

# check for OpenSSL
AC_CHECK_HEADERS([openssl/rsa.h openssl/bn.h openssl/err.h openssl/sha.h openssl/md5.h], , [AC_MSG_ERROR("Required OpenSSL headers not found.")])
AC_CHECK_LIB(crypto, main, , [AC_MSG_ERROR("Linking against OpenSSL library failed")])

# check for Lua
PKG_CHECK_MODULES(LUA, lua5.1 >= 5.1, , [
	AC_CHECK_HEADERS([lua.hpp], , [AC_MSG_ERROR("Lua header not found.")])
	AC_CHECK_LIB(lua, main, , [AC_MSG_ERROR("Linking against Lua library failed.")])
])
AC_SUBST(LUA_CFLAGS)
AC_SUBST(LUA_LIBS)

# check for mysql if it is enabled
if test -n "$MYSQL_FLAGS"; then
	AC_CHECK_HEADERS([mysql/mysql.h],[MYSQL_LIBS=-lmysqlclient],[AC_MSG_ERROR("MySQL headers missing.")])
	AC_CHECK_LIB(mysqlclient, main,[],[AC_MSG_ERROR("Linking against mysql-client failed.")])
	AC_SUBST(MYSQL_LIBS)
fi

# check for sqlite if it is enabled
if test -n "$SQLITE_FLAGS"; then
	AC_CHECK_HEADERS([sqlite3.h],[SQLITE_LIBS=-lsqlite3],[AC_MSG_ERROR("SQLite3 headers missing.")])
	AC_CHECK_LIB(sqlite3, main,[],[AC_MSG_ERROR("Linking against sqlite3 failed.")])
	AC_SUBST(SQLITE_LIBS)
fi

if test "$useMySQL" = "no" ; then
	if test "$useSQLite" = "no" ; then
		echo Error: no database driver was enabled! To enable a database driver, configure with --enable-\<dbdriver\> \(example: --enable-mysql\).
		echo
		exit 1
		return 1
	fi
fi
AC_CONFIG_FILES([Makefile])
AC_OUTPUT

echo
#echo The Forgotten Server 0.4
echo $PACKAGE $VERSION
echo
echo Server diagnostics.......... : $serverDiag
echo Login server mode........... : $loginServer
echo OTAdmin protocol............ : $otAdmin
echo Root run permission......... : $rootPermission
echo Login server mode........... : $loginServer
echo Home-directory configuration : $homedirConf
echo OTServ custom allocator..... : $otservAllocator
echo Debug build................. : $debugBuild
echo Using LuaJIT................ : $luaJIT
echo
echo Build with MySQL............ : $useMySQL
echo Build with SQLite........... : $useSQLite
echo

echo Configure complete, now you may type \'./build.sh\'.
