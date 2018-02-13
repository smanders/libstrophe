add_library(testlib STATIC test.c test.h)
target_link_libraries(testlib libstrophe expat resolv) # TODO library dependencies
set_property(TARGET testlib PROPERTY FOLDER tests)
set(tests
  check_parser
  res_query_dump
  test_base64
  test_ctx
  test_hash
  test_jid
  test_md5
  test_rand
  test_resolver
  test_sasl
  test_scram
  test_sha1
  test_snprintf
  test_sock
  test_string
  )
foreach(exe ${tests})
  if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/${exe}.c) # there are some tests only in repo, not in download
    add_executable(${exe} ${exe}.c)
    target_link_libraries(${exe} testlib)
    add_test(NAME ${exe} COMMAND ${exe})
    set_property(TARGET ${exe} PROPERTY FOLDER tests)
  endif()
endforeach()
