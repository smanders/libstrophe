set(examples
  active
  basic
  bot
  component
  roster
  uuid
  )
foreach(exe ${examples})
  add_executable(${exe} ${exe}.c)
  target_link_libraries(${exe} PRIVATE libstrophe)
  set_property(TARGET ${exe} PROPERTY FOLDER examples)
endforeach()
