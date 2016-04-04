cmake_minimum_required(VERSION 3.0.2)

file(GLOB INCLUDE_FILES "include/*.h")
file(GLOB SRC_FILES "src/*.c" "common/*.h" "common/*.c")

source_group(include FILES ${INCLUDE_FILES})
source_group(src FILES ${SRC_FILES})

add_library(libusbmuxd SHARED ${INCLUDE_FILES} ${SRC_FILES})
target_include_directories(libusbmuxd PUBLIC "include" "common" "vendor/include")

if(WIN32)
  set(CMAKE_MFC_FLAG 2)
  target_compile_definitions(libusbmuxd PUBLIC -D_AFXDLL -D_UNICODE -DHAVE_CONFIG_H)
endif()
target_compile_definitions(libusbmuxd PUBLIC -Dlibusbmuxd_DLL -Dlibusbmuxd_DLL_EXPORTS)

find_path(libplist_INCLUDE_DIR NAMES plist.h PATHS "vendor/include")
find_library(libplist_LIBRARY NAMES libplist PATHS "vendor/lib")

target_compile_definitions(libusbmuxd PUBLIC -Dlibplist_DLL)
target_include_directories(libusbmuxd PUBLIC ${libplist_INCLUDE_DIR})
target_link_libraries(libusbmuxd ${libplist_LIBRARY})

if(WIN32)
  target_link_libraries(libusbmuxd ws2_32)
endif()
