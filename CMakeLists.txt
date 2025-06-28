cmake_minimum_required (VERSION 2.6)
project (HPL)

set(HPL_VERSION_MAJOR 1)
set(HPL_VERSION_MINOR 0)

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/include
	${CMAKE_CURRENT_SOURCE_DIR}/../OALWrapper/include
	${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/include
)
file(GLOB_RECURSE all_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/sources/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/sources/*.c
)
# Removed Sources
file(GLOB_RECURSE to_remove_all
    win32.cpp
    Video*Theora.cpp
    )
list(REMOVE_ITEM all_sources
    ${to_remove_all})
list(REMOVE_DUPLICATES all_sources)

add_library(HPL STATIC ${all_sources})

add_library(Newton STATIC IMPORTED)
set_target_properties(Newton PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/lib/linux/libNewton.a)

add_library(glew STATIC ${CMAKE_CURRENT_SOURCE_DIR}/include/dependencies/glew/src/glew.c)
target_include_directories(glew PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include/dependencies/glew/include)
target_link_libraries(HPL PRIVATE glew)

find_package(OpenGL REQUIRED)
target_link_libraries(HPL PRIVATE Open::GL)

# add_library(angelscript STATIC IMPORTED)
# set_target_properties(angelscript PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/lib/linux/libangelscript.a)

target_include_directories(HPL PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include/dependencies/angelscript/include)
target_link_libraries(HPL PRIVATE angelscript)



add_library(SDLmain STATIC IMPORTED)
set_target_properties(SDLmain PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/lib/linux/libSDLmain.a)
