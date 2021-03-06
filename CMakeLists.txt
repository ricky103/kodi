project(pvr.octonet)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})
enable_language(CXX)

find_package(Kodi REQUIRED)
find_package(kodiplatform REQUIRED)
find_package(p8-platform REQUIRED)
find_package(JsonCpp REQUIRED)

include_directories(
	${kodiplatform_INCLUDE_DIRS}
	${p8-platform_INCLUDE_DIRS}
	${KODI_INCLUDE_DIR}
	${JSONCPP_INCLUDE_DIRS})

set(DEPLIBS
	${p8-platform_LIBRARIES}
	${JSONCPP_LIBRARIES})

set(OCTONET_SOURCES
	src/OctonetData.cpp
	src/client.cpp
	src/Socket.cpp
	src/rtsp_client.cpp)

set(OCTONET_HEADERS
	src/client.h
	src/OctonetData.h
	src/Socket.h)

build_addon(pvr.octonet OCTONET DEPLIBS)

if(WIN32)
	if(NOT CMAKE_SYSTEM_NAME STREQUAL WindowsStore)
		target_link_libraries(pvr.octonet wsock32 ws2_32)
	else()
		target_link_libraries(pvr.octonet ws2_32)
	endif()
endif()

include(CPack)
