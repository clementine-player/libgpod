cmake_minimum_required(VERSION 2.6)

include(FindPkgConfig)

set(SOURCES
  src/itdb_thumb.c
  src/itdb_track.c
  src/itdb_zlib.c
  src/pixmaps.c
  src/itdb_itunesdb.c
  src/db-parse-context.c
  src/db-artwork-parser.c
  src/db-artwork-writer.c
  src/itdb_artwork.c
  src/itdb_iphone.c
  src/itdb_playlist.c
  src/rijndael.c
  src/ithumb-writer.c
  src/itdb_tzinfo.c
  src/itdb_photoalbum.c
  src/itdb_hash58.c
  src/db-image-parser.c
  src/itdb_hash72.c
  src/itdb_sqlite.c
  src/itdb_device.c
  src/itdb_sysinfo_extended_parser.c
  src/itdb_chapterdata.c
  src/db-artwork-debug.c
  src/itdb_hashAB.c
  src/itdb_plist.c
)

pkg_check_modules(GLIB REQUIRED glib-2.0)

set(WITH_INTERNAL_GCHECKSUM 0)
set(LIBGPOD_BLOB_DIR "${CMAKE_INSTALL_PREFIX}/libgpod")

configure_file(
  "config.h.in"
  "config.h")

include_directories(${GLIB_INCLUDE_DIRS})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)
include_directories(${CMAKE_CURRENT_BINARY_DIR})
add_definitions(-DGETTEXT_PACKAGE="\\"libgpod\\"")

add_library(gpod ${SOURCES})
