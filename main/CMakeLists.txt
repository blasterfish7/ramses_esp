# IDF main component bridging to existing src/ sources for PlatformIO compatibility

set(MAIN_SRC_DIR ${CMAKE_SOURCE_DIR}/src)

set(MAIN_SOURCES
    ${MAIN_SRC_DIR}/main.c
    ${MAIN_SRC_DIR}/radio.c
    ${MAIN_SRC_DIR}/host.c
    ${MAIN_SRC_DIR}/platform.c
)

idf_component_register(
    SRCS ${MAIN_SOURCES}
    INCLUDE_DIRS ${MAIN_SRC_DIR}
    REQUIRES driver esp_app_format esp_event console esp_timer
    PRIV_REQUIRES command ramses-debug ramses-buttons ramses-led ramses-nvs ramses-network ramses-mqtt cc1101 frame message gateway
)
