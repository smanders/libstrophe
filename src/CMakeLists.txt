set(lib_name ${PROJECT_NAME})
########################################
# source
set(${lib_name}_libsrcs
  auth.c
  common.h
  conn.c
  crypto.c
  ctx.c
  event.c
  handler.c
  hash.c
  hash.h
  jid.c
  md5.c
  md5.h
  ostypes.h
  parser_expat.c
  parser.h
  #parser_libxml2.c # parser_expat instead
  rand.c
  rand.h
  resolver.c
  resolver.h
  sasl.c
  sasl.h
  scram.c
  scram.h
  sha1.c
  sha1.h
  snprintf.c
  snprintf.h
  sock.c
  sock.h
  stanza.c
  #tls_dummy.c # tls_openssl instead
  #tls_gnutls.c # tls_openssl instead
  tls.h
  tls_openssl.c
  #tls_schannel.c # tls_openssl instead
  util.c
  util.h
  uuid.c
  )
source_group("" FILES ${${lib_name}_libsrcs})
########################################
# library
add_library(${lib_name} STATIC ${${lib_name}_libsrcs})
target_link_libraries(${lib_name} ${EXPAT_LIBRARIES} ${OPENSSL_LIBRARIES} ${SYS_LIBS})
target_include_directories(${lib_name} PUBLIC $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  PRIVATE
  $<BUILD_INTERFACE:${EXPAT_INCLUDE_DIRS}>
  $<BUILD_INTERFACE:${OPENSSL_INCLUDE_DIR}>
  )
set_target_properties(${lib_name} PROPERTIES
  OUTPUT_NAME ${lib_name}${ver}
  PREFIX "" # strip off the "lib" prefix, since it's already libstrophe
  )
########################################
# install
set(targetsFile ${PROJECT_NAME}${ver}-targets)
install(TARGETS ${lib_name} EXPORT ${targetsFile}
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  )
install(EXPORT ${targetsFile} DESTINATION lib/cmake)
