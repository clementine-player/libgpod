cmake_minimum_required(VERSION 2.6)
cmake_policy(SET CMP0012 NEW)

include(CheckIncludeFiles)
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
pkg_check_modules(LIBIMOBILEDEVICE libimobiledevice-1.0)
pkg_check_modules(GDK_PIXBUF gdk-pixbuf-2.0)
check_include_files(unistd.h HAVE_UNISTD_H)

option(ENABLE_GDKPIXBUF
  "Whether to include gdk-pixbuf support" ${GDK_PIXBUF_FOUND})
option(ENABLE_LIBIMOBILEDEVICE
  "Whether to include libimobiledevice support" ${LIBIMOBILEDEVICE_FOUND})

set(HAVE_GDKPIXBUF ${ENABLE_GDKPIXBUF})
set(HAVE_LIBIMOBILEDEVICE ${ENABLE_LIBIMOBILEDEVICE})

set(WITH_INTERNAL_GCHECKSUM 0)
set(LIBGPOD_BLOB_DIR "${CMAKE_INSTALL_PREFIX}/libgpod")
set(VERSION 0.8.0)

if (${HAVE_GDKPIXBUF})
  set(GDKPIXBUF_REQ "gdk-pixbuf-2.0")
endif ()

if (${HAVE_LIBIMOBILEDEVICE})
  set(LIBIMOBILEDEVICE_REQ "libimobiledevice-1.0")
endif ()

configure_file(config.h.in config.h)
configure_file(libgpod-1.0.pc.in libgpod-1.0.pc)

include_directories(${GLIB_INCLUDE_DIRS})

if (${GDK_PIXBUF_FOUND})
  include_directories(${GDK_PIXBUF_INCLUDE_DIRS})
endif ()

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)
include_directories(${CMAKE_CURRENT_BINARY_DIR})
add_definitions(-DGETTEXT_PACKAGE="\\"libgpod\\"")

add_library(gpod SHARED ${SOURCES})

target_link_libraries(gpod
  ${GLIB_LIBRARIES}
  ${GDK_PIXBUF_LIBRARIES}
  ${LIBIMOBILEDEVICE_LIBRARIES}
)

if (${HAVE_LIBIMOBILEDEVICE})
  target_link_libraries(gpod ${LIBIMOBILEDEVICE_LIBRARIES})
endif ()

if (${HAVE_GDKPIXBUF})
  target_link_libraries(gpod ${GDK_PIXBUF_LIBRARIES})
endif ()

install(TARGETS gpod
  LIBRARY DESTINATION lib
)

install(
  DIRECTORY src/
  DESTINATION include/gpod-1.0/gpod
  FILES_MATCHING PATTERN itdb.h
)

install(
  FILES ${CMAKE_CURRENT_BINARY_DIR}/libgpod-1.0.pc
  DESTINATION lib/pkgconfig
)
