# Minimal cmake project file for building winebuild
#
# This file was created for testing winebuild standalone (without autoconf) and is NOT part of the
# official winehq project.
#
# For more information please see https://github.com/LMMS/lmms/pull/7987

cmake_minimum_required(VERSION 3.13)

project(winebuild LANGUAGES C)

# Parse version info from VERSION file
file(READ ../../VERSION version_file_content)
string(REPLACE "\n" "" version_file_content "${version_file_content}")
string(REPLACE " " ";" version_list "${version_file_content}")
list(GET version_list 2 WINE_VERSION)

# Create a stub config.h
set(HEADER_CONTENT
"/* Minimalist config.h for the extracted winebuild sources */
#ifndef __WINE_CONFIG_H
#define __WINE_CONFIG_H
#define EXEEXT \"\"
#define PACKAGE_VERSION \"${WINE_VERSION}\"
#endif /* __WINE_CONFIG_H */"
)

file(GENERATE OUTPUT config.h CONTENT "${HEADER_CONTENT}")

add_executable(winebuild
    import.c
    main.c
    parser.c
    relay.c
    res16.c
    res32.c
    spec16.c
    spec32.c
    utils.c
)

target_include_directories(winebuild PRIVATE ../../include)
