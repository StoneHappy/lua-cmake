cmake_minimum_required(VERSION 3.10 FATAL_ERROR)

project(lua)

option(LUA_BUILD_SHARED "Build lua shared lib" ON)

if(LUA_BUILD_SHARED)
  set(LUA_BUILD_TYPE "SHARED")
else()
  set(LUA_BUILD_TYPE "STATIC")
endif()

file(GLOB_RECURSE HEADER_FILES "${CMAKE_CURRENT_SOURCE_DIR}/*.h")
file(GLOB_RECURSE SOURCE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/onelua.c")

source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES ${HEADER_FILES} ${SOURCE_FILES})

add_library(lua ${LUA_BUILD_TYPE} ${HEADER_FILES} ${SOURCE_FILES})

target_compile_options(lua PUBLIC "$<$<COMPILE_LANG_AND_ID:CXX,MSVC>:/permissive->")
target_compile_options(lua PUBLIC "$<$<COMPILE_LANG_AND_ID:CXX,MSVC>:/WX->")

# Link dependencies    
target_link_libraries(lua PUBLIC
          )

target_include_directories(
  lua 
  PUBLIC 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)
if(LUA_BUILD_SHARED)
  target_compile_definitions(lua PRIVATE
    "LUA_BUILD_AS_DLL" 
    "MAKE_LUA"
  )
endif()