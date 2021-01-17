if (NOT CMAKE_INSTALL_PREFIX OR
        CMAKE_INSTALL_PREFIX STREQUAL "/usr/local")
    set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_SOURCE_DIR}/../../../Install")
endif()


file(GLOB_RECURSE NODEJS_SRCS ${CMAKE_CURRENT_SOURCE_DIR} *.c *.cc)

add_custom_target(nodejs ALL
  DEPENDS ${CMAKE_INSTALL_PREFIX}/nodejs/lib/libnode.so
  )


add_custom_command(OUTPUT ${CMAKE_INSTALL_PREFIX}/nodejs/lib/libnode.so
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/out/Debug/libnode.so.88
  COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/out/Debug/libnode.so.88 ${CMAKE_INSTALL_PREFIX}/nodejs/lib/
  COMMAND ${CMAKE_COMMAND} -E create_symlink ${CMAKE_INSTALL_PREFIX}/nodejs/lib/libnode.so.88 ${CMAKE_INSTALL_PREFIX}/nodejs/lib/libnode.so
  BYPRODUCTS ${CMAKE_INSTALL_PREFIX}/nodejs/lib/libnode.so.88
  BYPRODUCTS ${CMAKE_INSTALL_PREFIX}/nodejs/lib/libnode.so
  )

add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/out/Debug/libnode.so.88
  DEPENDS ${NODEJS_SRCS}
  COMMAND ./configure --shared --prefix=${CMAKE_INSTALL_PREFIX}/nodejs --debug
  COMMAND make -j4
  COMMAND make install
  BYPRODUCTS ${CMAKE_CURRENT_BINARY_DIR}
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

