set(server_SRCS
		StdInc.cpp

		CGameHandler.cpp
		CQuery.cpp
		CVCMIServer.cpp
		NetPacksServer.cpp
		NetPacksLobbyServer.cpp
)

set(server_HEADERS
		StdInc.h

		CGameHandler.h
		CQuery.h
		CVCMIServer.h
)

assign_source_group(${server_SRCS} ${server_HEADERS})

if(ANDROID) # android needs client/server to be libraries, not executables, so we can't reuse the build part of this script
	return()
endif()

add_executable(vcmiserver ${server_SRCS} ${server_HEADERS})

set(server_LIBS vcmi)
if(CMAKE_SYSTEM_NAME MATCHES FreeBSD)
	set(server_LIBS execinfo ${server_LIBS})
endif()
target_link_libraries(vcmiserver PRIVATE ${server_LIBS})

target_include_directories(vcmiserver
	PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
)

if(WIN32)
	set_target_properties(vcmiserver
		PROPERTIES
			OUTPUT_NAME "VCMI_server"
			PROJECT_LABEL "VCMI_server"
	)
endif()

vcmi_set_output_dir(vcmiserver "")

set_target_properties(vcmiserver PROPERTIES ${PCH_PROPERTIES})
cotire(vcmiserver)

install(TARGETS vcmiserver DESTINATION ${BIN_DIR})
