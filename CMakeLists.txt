cmake_minimum_required(VERSION 3.0.2)

file(GLOB INCLUDE_FILES "include/*.h")
file(GLOB SRC_FILES "src/*.c" "common/*.h" "common/*.c")

source_group(include FILES ${INCLUDE_FILES})
source_group(src FILES ${SRC_FILES})

add_library(libusbmuxd SHARED ${INCLUDE_FILES} ${SRC_FILES})
target_include_directories(libusbmuxd PUBLIC "include" "common" "../libplist/include")
target_compile_definitions(libusbmuxd PUBLIC -Dlibusbmuxd_DLL -Dlibusbmuxd_DLL_EXPORTS -Dlibplist_DLL)

if(WIN32)
  set(CMAKE_MFC_FLAG 2)
  target_compile_definitions(libusbmuxd PUBLIC -D_AFXDLL -D_UNICODE -DHAVE_CONFIG_H)
  find_library(libplist_LIBRARY NAMES libplist.lib PATHS "../libplist/build/Release")
  target_link_libraries(libusbmuxd ws2_32 ${libplist_LIBRARY})
endif()
