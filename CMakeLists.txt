# Copyright (C) 2007-2009 LuaDist.
# Created by Peter Kapec
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT(zlib C)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
INCLUDE(dist.cmake)

# Platform checks
INCLUDE (CheckIncludeFiles)
CHECK_INCLUDE_FILES("unistd.h" HAVE_UNISTD_H)
IF(HAVE_UNISTD_H)
	ADD_DEFINITIONS(-DHAVE_UNISTD_H)
ENDIF(HAVE_UNISTD_H)

IF(WIN32)
	ADD_DEFINITIONS(-DZLIB_DLL)
ENDIF(WIN32)
IF(MSVC)
	ADD_DEFINITIONS(-D_CRT_SECURE_NO_WARNINGS)
ENDIF(MSVC)

# ZLib Library
SET(ZLIB_SRCS
	adler32.c
	compress.c
	crc32.c
	deflate.c
	gzio.c
	infback.c
	inffast.c
	inflate.c
	inftrees.c
	trees.c
	uncompr.c
	zutil.c
	)

ADD_LIBRARY(z SHARED ${ZLIB_SRCS})

INCLUDE(CTest)
IF(BUILD_TESTING)
	ADD_EXECUTABLE(example example.c)
	TARGET_LINK_LIBRARIES(example z)
	ADD_EXECUTABLE(minigzip minigzip.c)
	TARGET_LINK_LIBRARIES(minigzip z)

	ADD_TEST(example example)
	#~2DO: check "echo hello world | ./minigzip | ./minigzip -d" succeeds
ENDIF(BUILD_TESTING)

INSTALL(TARGETS z RUNTIME DESTINATION ${INSTALL_BIN} LIBRARY DESTINATION ${INSTALL_LIB} ARCHIVE DESTINATION ${INSTALL_LIB})
INSTALL(FILES zlib.h zconf.h DESTINATION ${INSTALL_INC})
INSTALL(FILES FAQ ChangeLog INDEX README zlib.3 algorithm.txt old/zlib.html win32/DLL_FAQ.txt DESTINATION ${INSTALL_DOC})

