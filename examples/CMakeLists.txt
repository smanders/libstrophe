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
  target_link_libraries(${exe} libstrophe expat resolv) # TODO library dependencies
endforeach()
