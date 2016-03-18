cmake_minimum_required( VERSION 2.8 FATAL_ERROR )
set( CMAKE_VERBOSE_MAKEFILE on )

get_filename_component( CINDER_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../../.." ABSOLUTE )
include( ${CINDER_DIR}/linux/cmake/Cinder.cmake )

project( BlockApp )

set( CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake" )

get_filename_component( OSC_BLOCK_DIR "${CINDER_DIR}/blocks/OSC" ABSOLUTE )
get_filename_component( SKINNING_BLOCK_DIR "${CINDER_DIR}/blocks/Cinder-Skinning" ABSOLUTE )
get_filename_component( SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../src" ABSOLUTE )
get_filename_component( INC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../include" ABSOLUTE )

if( NOT TARGET cinder${CINDER_LIB_SUFFIX} )
    find_package( cinder REQUIRED
		PATHS ${CINDER_DIR}/linux/${CMAKE_BUILD_TYPE}/${CINDER_OUT_DIR_PREFIX}
        $ENV{Cinder_DIR}/linux/${CMAKE_BUILD_TYPE}/${CINDER_OUT_DIR_PREFIX}
    )
endif()

# Get the OSC sources.
file( GLOB OSC_BLOCK_SRCS "${OSC_BLOCK_DIR}/src/*.cpp" ) 

# Get assimp.
find_package( assimp REQUIRED )

# Get the Cinder-Skinning sources
file( GLOB SKINNING_BLOCK_SRCS "${SKINNING_BLOCK_DIR}/src/model/*.cpp" )

# Use PROJECT_NAME since CMAKE_PROJET_NAME returns the top-level project name.
set( EXE_NAME ${PROJECT_NAME} )

set( SRC_FILES
	${SRC_DIR}/BlockApp.cpp
	${OSC_BLOCK_SRCS}
	${SKINNING_BLOCK_SRCS}
)

add_executable( "${EXE_NAME}" ${SRC_FILES} )

target_include_directories(
	"${EXE_NAME}"
	PUBLIC ${INC_DIR} ${OSC_BLOCK_DIR}/src ${assimp_INCLUDE_DIRS} ${SKINNING_BLOCK_DIR}/src/model ${SKINNING_BLOCK_DIR}/src
)

target_link_libraries( "${EXE_NAME}" cinder${CINDER_LIB_SUFFIX} ${assimp_LIBRARIES} )
