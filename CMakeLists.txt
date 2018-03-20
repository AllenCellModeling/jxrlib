cmake_minimum_required(VERSION 3.9)
project(jxrlib)

set(CMAKE_CXX_STANDARD 14)
# set( CMAKE_VERBOSE_MAKEFILE on )

set(JXR_Glue_HEADERS
        jxrgluelib/JXRGlue.h
        jxrgluelib/JXRMeta.h
        jxrgluelib/JXRGluePFC.h
        jxrgluelib/JXRGlueJxr.h
        )

set(JXR_Glue_SOURCES
        jxrgluelib/JXRGlue.c
        jxrgluelib/JXRMeta.c
        jxrgluelib/JXRGluePFC.c
        jxrgluelib/JXRGlueJxr.c
        )

set(JXR_Glue_Test_SOURCES
        jxrtestlib/JXRTest.c
        jxrtestlib/JXRTestBmp.c
        jxrtestlib/JXRTestHdr.c
        jxrtestlib/JXRTestPnm.c
        jxrtestlib/JXRTestTif.c
        jxrtestlib/JXRTestYUV.c
        )

add_definitions(-D__ANSI__)
add_definitions(-Wno-shift-negative-value) # there's some bitshift logic that causes a warning under clang
add_definitions(-Wno-dangling-else) # there's a klugie macro that causes compiler warnings without this

add_library(jxrglue STATIC ${JXR_Glue_SOURCES})
target_include_directories(jxrglue PUBLIC jxrgluelib image/sys common/include)


set(COMMON_SRC
        image/sys/adapthuff.c
        image/sys/image.c
        image/sys/strcodec.c
        image/sys/strPredQuant.c
        image/sys/strTransform.c
        image/sys/perfTimerANSI.c
        )

set(DECODE_SRC
        image/decode/decode.c
        image/decode/postprocess.c
        image/decode/segdec.c
        image/decode/strdec.c
        image/decode/strInvTransform.c
        image/decode/strPredQuantDec.c
        image/decode/JXRTranscode.c
        )

set(ENCODE_SRC
        image/encode/encode.c
        image/encode/segenc.c
        image/encode/strenc.c
        image/encode/strFwdTransform.c
        image/encode/strPredQuantEnc.c
        )

add_definitions(-Wno-unused-value)
add_definitions(-Wno-implicit-function-declaration) # there's some terrible #define stuff that creates macros but isn't exposed in the .h files so it looks hidden to the compiler
add_definitions(-Wno-deprecated-declarations) # this is to override the error message not to use tmpnam
add_definitions(-Wno-constant-conversion)

add_library(jpegxr STATIC ${COMMON_SRC} ${DECODE_SRC} ${ENCODE_SRC})
target_include_directories(jpegxr PUBLIC common/include image/sys)

set(JXRPP_SRC
        cpp/lib/CodecFactory.cpp
        cpp/lib/DecodeContext.cpp
        cpp/lib/Factory.cpp
        cpp/lib/ImageDecoder.cpp
        cpp/lib/ImageEncoder.cpp
        cpp/lib/Stream.cpp)


add_definitions(-Wno-self-assign-field)
add_library(jxr++ STATIC ${JXRPP_SRC})
target_include_directories(jxr++ PUBLIC common/include image/sys jxrgluelib jxrtestlib)


#add_executable(jxrlib
#        common/include/guiddef.h
#        common/include/wmsal.h
#        common/include/wmspecstring.h
#        common/include/wmspecstrings_adt.h
#        common/include/wmspecstrings_strict.h
#        common/include/wmspecstrings_undef.h
#        cpp/lib/CodecFactory.cpp
#        cpp/lib/CodecFactory.hpp
#        cpp/lib/DecodeContext.cpp
#        cpp/lib/DecodeContext.hpp
#        cpp/lib/Factory.cpp
#        cpp/lib/Factory.hpp
#        cpp/lib/FormatConverter.hpp
#        cpp/lib/FormatError.hpp
#        cpp/lib/ImageDecoder.cpp
#        cpp/lib/ImageDecoder.hpp
#        cpp/lib/ImageEncoder.cpp
#        cpp/lib/ImageEncoder.hpp
#        cpp/lib/Resolution.hpp
#        cpp/lib/Stream.cpp
#        cpp/lib/Stream.hpp
#        cpp/jxrdecode.cpp
#        image/decode/decode.c
#        image/decode/decode.h
#        image/decode/JXRTranscode.c
#        image/decode/postprocess.c
#        image/decode/segdec.c
#        image/decode/strdec.c
#        image/decode/strdec_x86.c
#        image/decode/strInvTransform.c
#        image/decode/strPredQuantDec.c
#        image/encode/encode.c
#        image/encode/encode.h
#        image/encode/segenc.c
#        image/encode/strenc.c
#        image/encode/strenc_x86.c
#        image/encode/strFwdTransform.c
#        image/encode/strPredQuantEnc.c
#        image/sys/adapthuff.c
#        image/sys/ansi.h
#        image/sys/common.h
#        image/sys/image.c
#        image/sys/perfTimer.h
#        image/sys/perfTimerANSI.c
#        image/sys/strcodec.c
#        image/sys/strcodec.h
#        image/sys/strPredQuant.c
#        image/sys/strTransform.c
#        image/sys/strTransform.h
#        image/sys/windowsmediaphoto.h
#        image/sys/xplatform_image.h
#        jxrencoderdecoder/JXRDec420.c
#        jxrencoderdecoder/JxrDecApp.c
#        jxrencoderdecoder/JxrEnc420.c
#        jxrencoderdecoder/JxrEncApp.c
#        jxrgluelib/JXRGlue.c
#        jxrgluelib/JXRGlue.h
#        jxrgluelib/JXRGlueJxr.c
#        jxrgluelib/JXRGluePFC.c
#        jxrgluelib/JXRMeta.c
#        jxrgluelib/JXRMeta.h
#        jxrtestlib/JXRTest.c
#        jxrtestlib/JXRTest.h
#        jxrtestlib/JXRTestBmp.c
#        jxrtestlib/JXRTestHdr.c
#        jxrtestlib/JXRTestPnm.c
#        jxrtestlib/JXRTestTif.c
#        jxrtestlib/JXRTestYUV.c)
