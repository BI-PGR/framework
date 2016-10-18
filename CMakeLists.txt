cmake_minimum_required (VERSION 3.0)

project (pgr)

set(USE_PACKAGE_MANAGER OFF)

if(APPLE)
  set(USE_PACKAGE_MANAGER ON)
  add_definitions(-Wno-deprecated)
endif()

if(USE_PACKAGE_MANAGER)
  set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/modules/")

  find_package(DevIL REQUIRED)
  find_package(AntTweakBar REQUIRED)
  find_package(assimp REQUIRED)
  find_package(GLUT REQUIRED)
else(USE_PACKAGE_MANAGER)
  # AntTweakBar
  add_library(AntTweakBar STATIC IMPORTED GLOBAL)
  set_target_properties(AntTweakBar PROPERTIES
    IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/AntTweakBar/lib/AntTweakBar.lib"
    INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/AntTweakBar/inc"
  )
  set(TW_LIBRARIES AntTweakBar)

  # Assimp
  add_library(assimp STATIC IMPORTED GLOBAL)
  set_target_properties(assimp PROPERTIES
    IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/assimp/lib/libassimp.so.3.2.0"
    INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/assimp/inc"
  )
  set(ASSIMP_LIBRARIES assimp)

  # DevIL
  add_library(devil STATIC IMPORTED GLOBAL)
  set_target_properties(devil PROPERTIES
    IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/DevIL/lib/libIL.so.1.1.0"
    INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/DevIL/inc"
  )
  set(IL_LIBRARIES devil)

  # FreeGLUT
  add_library(freeglut STATIC IMPORTED GLOBAL)
  set_target_properties(freeglut PROPERTIES
    IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/freeglut/lib/libglut.so.3.9.0"
    INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/freeglut/inc"
  )
  set(GLUT_LIBRARIES freeglut)
endif()

find_package(OpenGL REQUIRED)

# pgr-framework
file (GLOB_RECURSE SOURCES src/*)

add_library (${PROJECT_NAME} SHARED ${SOURCES})

target_include_directories (
  ${PROJECT_NAME} PUBLIC
  inc/
  ${OPENGL_INCLUDE_DIR}
  ${TW_INCLUDE_DIR}
  ${ASSIMP_INCLUDE_DIR}
  ${IL_INCLUDE_DIR}
  ${GLUT_INCLUDE_DIR}
)

target_link_libraries (
  ${PROJECT_NAME}
  PUBLIC
  ${OPENGL_LIBRARIES}
  ${TW_LIBRARIES}
  ${ASSIMP_LIBRARIES}
  ${IL_LIBRARIES}
  ${GLUT_LIBRARIES}
)
