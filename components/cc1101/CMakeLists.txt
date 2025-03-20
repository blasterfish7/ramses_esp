set(component_srcs "cc1101.c" "cc1101_param.c")

idf_component_register(
    SRCS "${component_srcs}"
    INCLUDE_DIRS "include"
    PRIV_REQUIRES driver
)

# if(EXISTS "${COMPONENT_DIR}/Kconfig.projbuild")
#     set(COMPONENT_KCONFIGS ${COMPONENT_DIR}/Kconfig)
# endif()
