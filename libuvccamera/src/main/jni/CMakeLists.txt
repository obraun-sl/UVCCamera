########################################################################################################################################
## Cmake command line for android : 
## cmake -DCMAKE_TOOLCHAIN_FILE='<path_to_ndk>/build/cmake/android.toolchain.cmake' -DANDROID_ABI=armeabi-v7a ..
## Not used for now. android.mk still used. It is just more easy to develop on a IDE such as qtcreator and make sure it builds
## TODO : switch to cmake to create a single library that contains usb,jpeg, uvc to simplify
#########################################################################################################################################

cmake_minimum_required(VERSION 3.4 FATAL_ERROR)
project(uvccamera)

SET (libUSBSourceCode
    libusb/libusb/core.c
    libusb/libusb/descriptor.c
    libusb/libusb/hotplug.c
    libusb/libusb/io.c
    libusb/libusb/sync.c
    libusb/libusb/strerror.c
    libusb/libusb/os/android_usbfs.c
    libusb/libusb/os/poll_posix.c
    libusb/libusb/os/threads_posix.c
    libusb/libusb/os/android_netlink.c
    )

SET (libUVCSourceCode
    libuvc/src/ctrl.c
    libuvc/src/device.c
    libuvc/src/diag.c
    libuvc/src/frame.c
    libuvc/src/frame-mjpeg.c
    libuvc/src/init.c
    libuvc/src/stream.c
    libuvc/src/misc.c
    )

 
SET (libjpegSourceCode
        libjpeg-turbo-1.5.0/jcapimin.c
        libjpeg-turbo-1.5.0/jcapistd.c
        libjpeg-turbo-1.5.0/jccoefct.c
        libjpeg-turbo-1.5.0/jccolor.c
        libjpeg-turbo-1.5.0/jcdctmgr.c
        libjpeg-turbo-1.5.0/jchuff.c
        libjpeg-turbo-1.5.0/jcinit.c
        libjpeg-turbo-1.5.0/jcmainct.c
        libjpeg-turbo-1.5.0/jcmarker.c
        libjpeg-turbo-1.5.0/jcmaster.c
        libjpeg-turbo-1.5.0/jcomapi.c
        libjpeg-turbo-1.5.0/jcparam.c
        libjpeg-turbo-1.5.0/jcphuff.c
        libjpeg-turbo-1.5.0/jcprepct.c
        libjpeg-turbo-1.5.0/jcsample.c
        libjpeg-turbo-1.5.0/jctrans.c
        libjpeg-turbo-1.5.0/jdapimin.c
        libjpeg-turbo-1.5.0/jdapistd.c
        libjpeg-turbo-1.5.0/jdatadst.c
        libjpeg-turbo-1.5.0/jdatasrc.c
        libjpeg-turbo-1.5.0/jdcoefct.c
        libjpeg-turbo-1.5.0/jdcolor.c
        libjpeg-turbo-1.5.0/jddctmgr.c
        libjpeg-turbo-1.5.0/jdhuff.c
        libjpeg-turbo-1.5.0/jdinput.c
        libjpeg-turbo-1.5.0/jdmainct.c
        libjpeg-turbo-1.5.0/jdmarker.c
        libjpeg-turbo-1.5.0/jdmaster.c
        libjpeg-turbo-1.5.0/jdmerge.c
        libjpeg-turbo-1.5.0/jdphuff.c
        libjpeg-turbo-1.5.0/jdpostct.c
        libjpeg-turbo-1.5.0/jdsample.c
        libjpeg-turbo-1.5.0/jdtrans.c
        libjpeg-turbo-1.5.0/jerror.c
        libjpeg-turbo-1.5.0/jfdctflt.c
        libjpeg-turbo-1.5.0/jfdctfst.c
        libjpeg-turbo-1.5.0/jfdctint.c
        libjpeg-turbo-1.5.0/jidctflt.c
        libjpeg-turbo-1.5.0/jidctfst.c
        libjpeg-turbo-1.5.0/jidctint.c
        libjpeg-turbo-1.5.0/jidctred.c
        libjpeg-turbo-1.5.0/jquant1.c
        libjpeg-turbo-1.5.0/jquant2.c
        libjpeg-turbo-1.5.0/jutils.c
        libjpeg-turbo-1.5.0/jmemmgr.c
        libjpeg-turbo-1.5.0/jmemnobs.c
        libjpeg-turbo-1.5.0/jaricom.c
        libjpeg-turbo-1.5.0/jcarith.c
        libjpeg-turbo-1.5.0/jdarith.c
        libjpeg-turbo-1.5.0/turbojpeg.c
        libjpeg-turbo-1.5.0/transupp.c
        libjpeg-turbo-1.5.0/jdatadst-tj.c
        libjpeg-turbo-1.5.0/jdatasrc-tj.c
        libjpeg-turbo-1.5.0/jsimd_none.c
)

SET(ZEDCppSourceCode 
		UVCCamera/_onload.cpp
		UVCCamera/utilbase.cpp
		UVCCamera/UVCCamera.cpp
		UVCCamera/UVCPreview.cpp
		UVCCamera/UVCButtonCallback.cpp
		UVCCamera/UVCStatusCallback.cpp
		UVCCamera/Parameters.cpp
		UVCCamera/serenegiant_usb_UVCCamera.cpp
	)


if (CMAKE_SIZEOF_VOID_P EQUAL 8)
  set(bits 64)
else ()
  set(bits 32)
endif (CMAKE_SIZEOF_VOID_P EQUAL 8)
	
message (STATUS "machine : " ${bits})


add_definitions(-D__ANDROID__ -DANDROID_STL=c++_static -DACCESS_RAW_DESCRIPTORS -fpermissive -DSIZEOF_SIZE_T=4 -Wno-incompatible-pointer-types )

##find_package(OpenCV REQUIRED)

include_directories (${CMAKE_CURRENT_SOURCE_DIR}/libusb)
include_directories (${CMAKE_CURRENT_SOURCE_DIR}/libusb/libusb)

include_directories (${CMAKE_CURRENT_SOURCE_DIR})
include_directories (${CMAKE_CURRENT_SOURCE_DIR}/libuvc/include)
include_directories (${CMAKE_CURRENT_SOURCE_DIR}/libuvc/include/libuvc)


include_directories (${CMAKE_CURRENT_SOURCE_DIR}/rapidjson/include)
include_directories (${CMAKE_CURRENT_SOURCE_DIR}/libjpeg-turbo-1.5.0/include)
include_directories (${CMAKE_CURRENT_SOURCE_DIR}/libjpeg-turbo-1.5.0)
include_directories (${CMAKE_CURRENT_SOURCE_DIR}/libjpeg-turbo-1.5.0/simd)



include_directories (${opencv_INCLUDE_DIR})
##link_directories(${OPENCV_LIBRARY_DIR})
link_directories(${ANDROID_LIB_DIR})
SET (ANDROID_LIBS m dl log android dl EGL GLESv2)
SET (CMAKE_DEBUG_POSTFIX "d")
	
ADD_LIBRARY(uvccamera SHARED ${ZEDCppSourceCode} ${libUSBSourceCode} ${libUVCSourceCode} ${libjpegSourceCode})
TARGET_LINK_LIBRARIES(uvccamera ${OpenCV_LIBS} ${ANDROID_LIBS})


