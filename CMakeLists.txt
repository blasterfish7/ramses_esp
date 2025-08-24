cmake_minimum_required(VERSION 3.16.0)

# Allow selecting a board-specific defaults file via env or fallback to sdkconfig.defaults
if(NOT DEFINED ENV{SDKCONFIG_DEFAULTS})
	# Users can set SDKCONFIG_DEFAULTS env var (comma separated) in CI for different boards
	set(ENV{SDKCONFIG_DEFAULTS} "sdkconfig.defaults")
endif()

include($ENV{IDF_PATH}/tools/cmake/project.cmake)

project(ramses_esp_c6)

# (Legacy / future) extra Kconfig includes can be appended here if needed
list(APPEND kconfigs "${CMAKE_SOURCE_DIR}/lib/cc1101/Kconfig.projbuild")

