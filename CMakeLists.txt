cmake_minimum_required(VERSION 2.8)
project(jpegturbo)
find_package(qibuild)

include_directories( "." "inc" )
add_definitions( -DWITH_SIMD )

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")

add_subdirectory(simd)
set_source_files_properties(${SIMD_OBJS} PROPERTIES GENERATED 1)

qi_create_lib( jpegturbo
  SRC jcapimin.c jcapistd.c jccoefct.c jccolor.c jcdctmgr.c jchuff.c
    jcinit.c jcmainct.c jcmarker.c jcmaster.c jcomapi.c jcparam.c jcphuff.c
    jcprepct.c jcsample.c jctrans.c jdapimin.c jdapistd.c jdatadst.c jdatasrc.c
    jdcoefct.c jdcolor.c jddctmgr.c jdhuff.c jdinput.c jdmainct.c jdmarker.c
    jdmaster.c jdmerge.c jdphuff.c jdpostct.c jdsample.c jdtrans.c jerror.c
    jfdctflt.c jfdctfst.c jfdctint.c jidctflt.c jidctfst.c jidctint.c jidctred.c
    jquant1.c jquant2.c jutils.c jmemmgr.c jmemnobs.c jaricom.c jcarith.c jdarith.c
    simd/jsimd_i386.c ${SIMD_OBJS}
  SHARED
)

add_dependencies(jpegturbo simd)

qi_install_header( jpeglib.h
                   jerror.h
                   jconfig.h
                   jmorecfg.h )

qi_stage_lib(jpegturbo)
