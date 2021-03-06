# To make Boost ublas much more efficient.
ADD_DEFINITIONS(-DNDEBUG)

LINK_DIRECTORIES (
	${PROJECT_BINARY_DIR}/opencog/atomspace
	${PROJECT_BINARY_DIR}/opencog/server
	${PROJECT_BINARY_DIR}/opencog/dynamics/attention
)

# The atom_types.h file is written to the build directory
INCLUDE_DIRECTORIES (
	${CMAKE_BINARY_DIR}
	${ODBC_INCLUDE_DIRS}
)

IF (XMLRPC_FOUND AND CURL_FOUND)
    SET (HOPFIELD_UBI_SRC HopfieldUbigrapher.cc)
    SET (PROJECT_UBI_LIB ubigraph
        ${XMLRPC_LIBRARIES}
        )
ENDIF (XMLRPC_FOUND AND CURL_FOUND)

ADD_EXECUTABLE (hopfield
	HopfieldDemo.cc
	HopfieldServer.cc
	Pattern.cc
	HopfieldOptions.cc
	StorkeyAgent.cc
    ImprintAgent.cc
    ${HOPFIELD_UBI_SRC}
)

ADD_DEPENDENCIES(hopfield attention_atom_types)

TARGET_LINK_LIBRARIES (hopfield
	server
	attention
    ${PROJECT_UBI_LIB}
	${Boost_SIGNALS_LIBRARY}
	${EXPAT_LIBRARIES}
)
