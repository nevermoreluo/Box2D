cmake_minimum_required(VERSION 3.10)

project(Box2D)

set(CMAKE_CXX_STANDARD 11)
MESSAGE(STATUS ${CMAKE_CURRENT_SOURCE_DIR})

if(UNIX)
    set(BOX2D_INSTALL_BY_DEFAULT ON)
else(UNIX)
    set(BOX2D_INSTALL_BY_DEFAULT OFF)
endif(UNIX)

set(Box2D_SOURCE_DIR
        ${CMAKE_CURRENT_SOURCE_DIR})


option(BOX2D_INSTALL "Install Box2D libs, includes, and CMake scripts" ${BOX2D_INSTALL_BY_DEFAULT})
option(BOX2D_INSTALL_DOC "Install Box2D documentation" OFF)
option(BOX2D_BUILD_SHARED "Build Box2D shared libraries" OFF)
option(BOX2D_BUILD_STATIC "Build Box2D static libraries" ON)
option(BOX2D_BUILD_EXAMPLES "Build Box2D examples" ON)

set(BOX2D_VERSION 2.1.0)

# The Box2D library.
add_subdirectory(Box2D)


if(BOX2D_BUILD_EXAMPLES)
    # HelloWorld console example.
    add_subdirectory(HelloWorld)

    # Testbed and dependencies.
    find_package(OpenGL REQUIRED)
    add_subdirectory(Testbed)
endif(BOX2D_BUILD_EXAMPLES)

if(BOX2D_INSTALL_DOC)
    install(DIRECTORY Documentation DESTINATION share/doc/Box2D PATTERN ".svn" EXCLUDE)
endif(BOX2D_INSTALL_DOC)