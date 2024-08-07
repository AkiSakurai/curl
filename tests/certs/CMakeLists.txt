transform_makefile_inc("Makefile.inc" "${CMAKE_CURRENT_BINARY_DIR}/Makefile.inc.cmake")
include("${CMAKE_CURRENT_BINARY_DIR}/Makefile.inc.cmake")


add_custom_target(clean-certs
  COMMAND rm -f ${GENERATEDCERTS}
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

add_custom_target(build-certs
  DEPENDS ${CERTCONFIGS} ${SRPFILES}
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/scripts/genroot.sh EdelCurlRoot
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/scripts/genserv.sh Server-localhost EdelCurlRoot
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/scripts/genserv.sh Server-localhost.nn EdelCurlRoot
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/scripts/genserv.sh Server-localhost0h EdelCurlRoot
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/scripts/genserv.sh Server-localhost-firstSAN EdelCurlRoot
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/scripts/genserv.sh Server-localhost-lastSAN EdelCurlRoot
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/scripts/genserv.sh stunnel EdelCurlRoot
  COMMAND cp ${CMAKE_CURRENT_SOURCE_DIR}/stunnel-sv.pem ${CMAKE_CURRENT_BINARY_DIR}/stunnel.pem
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/stunnel.pem
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/stunnel-sv.pem
  COMMAND cp $< $@
)
