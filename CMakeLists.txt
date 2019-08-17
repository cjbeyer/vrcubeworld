cmake_minimum_required(VERSION 3.10)

project(vrcubeworldvk)

#
# native app glue lib
#

add_library(native_app_glue STATIC
  ${ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.h
  ${ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c
)

target_include_directories(native_app_glue PUBLIC
  ${ANDROID_NDK}/sources/android/native_app_glue
)

target_link_libraries(native_app_glue
  android
  log
)

set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -u ANativeActivity_onCreate")

#
# vrapi
#

add_library(vrapi SHARED IMPORTED GLOBAL)

set_target_properties(vrapi PROPERTIES
  IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/vrapi_lib/${CMAKE_BUILD_TYPE}/${ANDROID_ABI}/libvrapi.so"
  INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/vrapi_include"
)


#
# vrcubeworkvk
#

add_library(vrcubeworldvk SHARED
  cpp/Framework_Vulkan.h
  cpp/Framework_Vulkan.c
  cpp/VrCubeWorld_Vulkan.c
)

target_link_libraries(vrcubeworldvk
  android
  log
  native_app_glue
  vrapi
)

