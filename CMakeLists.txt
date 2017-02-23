cmake_minimum_required( VERSION 3.1.0 )

project( zlib C CXX )

SET( CMAKE_INSTALL_PREFIX "" CACHE PATH "Don't want an install target" FORCE)

# We do things my submodules.
set( zlib_TRUE_SOURCE_DIR "${zlib_SOURCE_DIR}/zlib" )

set( zlib_INCLUDE_DIR ${zlib_TRUE_SOURCE_DIR}/ )
set( zlib_SOURCES "" )

set( zlib_LINK "" )
set( zlib_DEFINES "" )

set( zlib_INCLUDE_DIR_PRIVATE "" )
set( zlib_LINK_PRIVATE "" )
set( zlib_DEFINES_PRIVATE "" )
set( zlib_COMPILE_FLAGS_PRIVATE "" )

list( APPEND zlib_SOURCES 
	${zlib_TRUE_SOURCE_DIR}/adler32.c
	${zlib_TRUE_SOURCE_DIR}/compress.c 
	${zlib_TRUE_SOURCE_DIR}/crc32.c 
	${zlib_TRUE_SOURCE_DIR}/deflate.c
	${zlib_TRUE_SOURCE_DIR}/gzclose.c
	${zlib_TRUE_SOURCE_DIR}/gzlib.c
	${zlib_TRUE_SOURCE_DIR}/gzwrite.c	
	${zlib_TRUE_SOURCE_DIR}/gzread.c
	${zlib_TRUE_SOURCE_DIR}/infback.c
	${zlib_TRUE_SOURCE_DIR}/inffast.c
	${zlib_TRUE_SOURCE_DIR}/inflate.c
	${zlib_TRUE_SOURCE_DIR}/inftrees.c
	${zlib_TRUE_SOURCE_DIR}/uncompr.c
	${zlib_TRUE_SOURCE_DIR}/zutil.c
	${zlib_TRUE_SOURCE_DIR}/trees.c
	${zlib_TRUE_SOURCE_DIR}/crc32.h 
	${zlib_TRUE_SOURCE_DIR}/deflate.h
	${zlib_TRUE_SOURCE_DIR}/gzguts.h
	${zlib_TRUE_SOURCE_DIR}/inffast.h
	${zlib_TRUE_SOURCE_DIR}/inffixed.h
	${zlib_TRUE_SOURCE_DIR}/inflate.h
	${zlib_TRUE_SOURCE_DIR}/inftrees.h
	${zlib_TRUE_SOURCE_DIR}/trees.h
	${zlib_TRUE_SOURCE_DIR}/zutil.h
)

if( MSVC )
	list( APPEND zlib_DEFINES_PRIVATE 
		"-D_CRT_SECURE_NO_WARNINGS" 
	)

	list( APPEND zlib_COMPILE_FLAGS_PRIVATE 
		"/wd4267"
		"/wd4996"
	)
endif()

add_library( zlib STATIC ${zlib_SOURCES} )

target_include_directories( zlib PRIVATE ${zlib_INCLUDE_DIR} ${zlib_INCLUDE_DIR_PRIVATE} )
target_compile_definitions( zlib PRIVATE ${zlib_DEFINES} ${zlib_DEFINES_PRIVATE} )
target_link_libraries( zlib PRIVATE ${zlib_LINK} ${zlib_LINK_PRIVATE} )
target_compile_options( zlib PRIVATE ${zlib_COMPILE_FLAGS_PRIVATE} )

target_include_directories( zlib INTERFACE $<BUILD_INTERFACE:${zlib_INCLUDE_DIR}> )
target_compile_definitions( zlib INTERFACE $<BUILD_INTERFACE:${zlib_DEFINES}> )
target_link_libraries( zlib INTERFACE $<BUILD_INTERFACE:${zlib_LINK}> )

# we have to do this as so mean cmakes do things the old way
set( ZLIB_INCLUDE_DIR "${zlib_INCLUDE_DIR}" CACHE STRING "" )
set( ZLIB_LIBRARIES zlib CACHE STRING "" )
set( ZLIB_FOUND TRUE CACHE BOOLEAN "" )
set( ZLIB_VERSION_STRING "1.0.0" CACHE STRING "" )

mark_as_advanced( ZLIB_INCLUDE_DIRS ZLIB_INCLUDE_DIR ZLIB_LIBRARIES ZLIB_FOUND ZLIB_VERSION_STRING )