cmake_minimum_required(VERSION 3.10)
project(mme4ceps)

add_compile_options(-O  -Wall -MD  -std=c++2a  -fPIC -static -Wno-undef )

IF(NOT( DEFINED ENV{CEPSCORE}))
    MESSAGE(FATAL_ERROR "Could not find ceps core (Environment variable CEPSCORE not set).")
ENDIF()

IF(NOT( DEFINED ENV{MACHINES4CEPS}))
    MESSAGE(FATAL_ERROR "Could not find machines4ceps (Environment variable MACHINES4CEPS not set).")
ENDIF()

IF(NOT( DEFINED ENV{LOG4CEPS}))
    MESSAGE(FATAL_ERROR "Could not find log4ceps (Environment variable LOG4CEPS not set).")
ENDIF()

include_directories($ENV{CEPSCORE}/include)
include_directories($ENV{LOG4CEPS}/include)
include_directories($ENV{MACHINES4CEPS})
include_directories($ENV{MACHINES4CEPS}/core/src_gen/logging)
include_directories(include)
include_directories(../include)
include_directories(include/tests)

link_directories($ENV{CEPSCORE}/bin)

add_library(slac4ceps SHARED 
           src/entrypoint.cpp 
           src/mme2ceps.cpp 
           src/ceps2mme.cpp 
           src/mme2ceps_utils.cpp 
           src/mme4ceps.cpp 
           src/ceps2mme.cpp
           src/sctp_server.cpp 
           ../src/mme.cpp 
           src/tests/test_handle_homeplug_mme.cpp)

target_link_libraries(slac4ceps cepscore sctp)

add_executable( slac-sim-ev
                src/tests/ev-slac-sim.cpp)

target_link_libraries(slac-sim-ev sctp pthread)
