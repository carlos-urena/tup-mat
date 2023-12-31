## --------------------------------------------------------------------------------
## Ejemplo AATests
##
## Archivo de configuración para compilar usando CMake en Linux
## creado: 26-Abril de 2023
## actualizaciones:
##    2023 Jun 19 : separo carpetas 'cmake' y 'bin', añado targets debug y release
##                  añado soporte para librería 'glm'
##
## --------------------------------------------------------------------------------

cmake_minimum_required (VERSION 3.5.1)
project (aatest_linux)

## diversos aspectos configurables (path relativos a la carpeta que tiene este archivo)

set( carpeta_fuentes      "../../src")
set( includes_adicionales "../../submodules/glm")
set( carpeta_ejecutables  "../bin" )
set( flags_compilador     "-std=c++20 -Wall -Wfatal-errors -Wno-deprecated-declarations" ) 
set( nombre_exe_debug     "debug_exe" )
set( nombre_exe_release   "release_exe" )

## definir directorios donde están los fuentes

file ( GLOB unidades  ${carpeta_fuentes}/*.cpp )
file ( GLOB cabeceras ${carpeta_fuentes}/*.h   )

## fijar las opciones que se van a usar para compilar y enlazar (ambos ejecutables)

set ( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${flags_compilador}")
include_directories( ${carpeta_fuentes} ${includes_adicionales} )

## definir las librerías que se van a enlazar (para ambos ejecutables)

find_package( OpenGL REQUIRED )
find_package( glfw3 3.3 REQUIRED )
link_libraries( glfw OpenGL::GL )

## definir ejecutables: 'debug_exe' (opción '-g') y 'release_exe' (opción '-O3')

add_executable       ( ${nombre_exe_debug} ${unidades} ${cabeceras} )
set_target_properties( ${nombre_exe_debug} PROPERTIES COMPILE_FLAGS "-g" )

add_executable       ( ${nombre_exe_release} EXCLUDE_FROM_ALL ${unidades} ${cabeceras} ) ## (no se compila por defecto)
set_target_properties( ${nombre_exe_release} PROPERTIES COMPILE_FLAGS "-O3" )

set_target_properties( ${nombre_exe_debug} ${nombre_exe_release} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${carpeta_ejecutables} )

