cmake_minimum_required(VERSION 3.0)
project(libstrophe)
include(flags OPTIONAL)
enable_testing()
set_property(GLOBAL PROPERTY USE_FOLDERS ON) # enables MSVC Solution Folders
########################################
if(MSVC)
  add_definitions(
    -D_CRT_NONSTDC_NO_DEPRECATE
    -D_CRT_SECURE_NO_WARNINGS
    -D_WINSOCK_DEPRECATED_NO_WARNINGS
    )
endif()
########################################
option(CMAKE_USE_OPENSSL_MODULE_PATH "Find OpenSSL in CMAKE_MODULE_PATH." OFF)
mark_as_advanced(CMAKE_USE_OPENSSL_MODULE_PATH)
if(CMAKE_USE_OPENSSL_MODULE_PATH)
  find_package(usexp-OpenSSL REQUIRED PATHS ${CMAKE_MODULE_PATH} NO_DEFAULT_PATH)
else()
  find_package(OpenSSL)
endif()
if(OPENSSL_FOUND)
  include_directories(${OPENSSL_INCLUDE_DIR})
else()
  message(FATAL_ERROR "can't find OpenSSL - libstrophe cmake currently hard-coded to use OpenSSL")
endif()
####################
option(CMAKE_USE_EXPAT_MODULE_PATH "Find expat in CMAKE_MODULE_PATH." OFF)
mark_as_advanced(CMAKE_USE_EXPAT_MODULE_PATH)
if(CMAKE_USE_EXPAT_MODULE_PATH)
  find_package(usexp-EXPAT REQUIRED PATHS ${CMAKE_MODULE_PATH} NO_DEFAULT_PATH)
else()
  find_package(EXPAT)
endif()
if(EXPAT_FOUND)
  include_directories(${EXPAT_INCLUDE_DIRS})
else()
  message(FATAL_ERROR "can't find expat - libstrophe cmake currently hard-coded to use expat")
endif()
########################################
# version
set(verString "^AC_INIT.*\\[\([.0-9]*\)\\].*")
set(verFile configure.ac)
file(STRINGS ${verFile} VER REGEX "${verString}")
string(REGEX REPLACE "${verString}" "\\1" VER ${VER})
if(NOT DEFINED LIBSTROPHE_VER)
  set(LIBSTROPHE_VER ${VER})
elseif(NOT ${LIBSTROPHE_VER} STREQUAL ${VER})
  message(AUTHOR_WARNING "version passed in (${LIBSTROPHE_VER}) doesn't match version from ${verFile} (${VER})")
endif()
set(ver _${LIBSTROPHE_VER})
set(verDir /${PROJECT_NAME}${ver})
########################################
add_subdirectory(src)
add_subdirectory(examples)
add_subdirectory(tests)
install(FILES strophe.h DESTINATION include${verDir}/${PROJECT_NAME})
