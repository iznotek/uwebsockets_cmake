cmake_minimum_required(VERSION 3.13)
project(uWebSockets
    DESCRIPTION "uWebSockets - Simple, secure & standards compliant web server for the most demanding of applications"
    LANGUAGES CXX C)
#set(CXX_STANDARD 11)

# LIBDEFLATE
set(LIBDEFLATE_DECOMPRESSION_ONLY OFF CACHE BOOL "Libdeflate decompression only")
set(LIBDEFLATE_DISABLE_ZLIB OFF CACHE BOOL "Libdeflate disable zlib")
set(LIBDEFLATE_DISABLE_GZIP OFF CACHE BOOL "Libdeflate disable gzip")

set(LIBDEFLATE_DIR uwebsockets/libdeflate/)
file( GLOB LIBDEFLATE_SRC libdeflate/lib/**/*.h libdeflate/common/*.h libdeflate/*.h)

set( LIBDEFLATE_SRC ${LIBDEFLATE_SRC} 
  ${LIBDEFLATE_DIR}/lib/deflate_decompress.c 
  ${LIBDEFLATE_DIR}/lib/utils.c
  ${LIBDEFLATE_DIR}/lib/x86/cpu_features.c
  ${LIBDEFLATE_DIR}/lib/arm/cpu_features.c
)

if (NOT LIBDEFLATE_DECOMPRESSION_ONLY)
  set( LIBDEFLATE_SRC ${LIBDEFLATE_SRC} 
    ${LIBDEFLATE_DIR}/lib/deflate_compress.c )
endif()

if (NOT LIBDEFLATE_DISABLE_ZLIB)
  set( LIBDEFLATE_SRC ${LIBDEFLATE_SRC} 
    ${LIBDEFLATE_DIR}/lib/adler32.c 
    ${LIBDEFLATE_DIR}/lib/zlib_decompress.c )
  if (NOT LIBDEFLATE_DECOMPRESSION_ONLY)
    set( LIBDEFLATE_SRC ${LIBDEFLATE_SRC} 
      ${LIBDEFLATE_DIR}/lib/zlib_compress.c )
  endif()
endif()

if (NOT LIBDEFLATE_DISABLE_GZIP)
  set( LIBDEFLATE_SRC ${LIBDEFLATE_SRC} 
    ${LIBDEFLATE_DIR}/lib/crc32.c 
    ${LIBDEFLATE_DIR}/lib/gzip_decompress.c )
  if (NOT LIBDEFLATE_DECOMPRESSION_ONLY)
    set( LIBDEFLATE_SRC ${LIBDEFLATE_SRC} 
      ${LIBDEFLATE_DIR}/lib/gzip_compress.c )
  endif()
endif()

# CFLAGS = -O2 -fomit-frame-pointer -std=c99 -I. -Wall -Wundef	
# LIB_CFLAGS = $(CFLAGS) -fvisibility=hidden -D_ANSI_SOURCE

include_directories(${LIBDEFLATE_DIR})
add_library(deflate ${LIBDEFLATE_SRC})