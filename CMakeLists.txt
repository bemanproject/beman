# SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception

cmake_minimum_required(VERSION 3.23)
project(beman LANGUAGES CXX)

include(CTest)
include(FetchContent)

option(
    BEMAN_USE_MAIN_BRANCHES
    "Instead of using pinned versions of beman libraries, use each from its main branch."
    OFF
)

# jsonfile = ./libraries.json
set(Beman_jsonfile "${CMAKE_CURRENT_LIST_DIR}/libraries.json")

# force CMake to reconfigure if the JSON file changes
set_property(
    DIRECTORY "${CMAKE_CURRENT_LIST_DIR}"
    APPEND
    PROPERTY CMAKE_CONFIGURE_DEPENDS "${Beman_jsonfile}"
)

# Read the JSON file
file(READ "${Beman_jsonfile}" Beman_rootobj)

# Get the libraries array and store it in Beman_librariesobj
string(
    JSON
    Beman_librariesobj
    ERROR_VARIABLE Beman_error
    GET "${Beman_rootobj}"
    "libraries"
)
if(Beman_error)
    message(FATAL_ERROR "${Beman_error}")
endif()

# Get the length of the libraries array
string(
    JSON
    Beman_numlibraries
    ERROR_VARIABLE Beman_error
    LENGTH "${Beman_librariesobj}"
)
if(Beman_error)
    message(FATAL_ERROR "${Beman_error}")
endif()

# Loop over each library object
math(EXPR Beman_maxindex "${Beman_numlibraries} - 1")
foreach(Beman_index RANGE "${Beman_maxindex}")
    # libraryobj = libraryobjs[index]
    string(
        JSON
        Beman_libraryobj
        ERROR_VARIABLE Beman_error
        GET "${Beman_librariesobj}"
        "${Beman_index}"
    )
    if(Beman_error)
        message(FATAL_ERROR "${Beman_error}")
    endif()

    # name = libraryobj["name"]
    string(
        JSON
        Beman_name
        ERROR_VARIABLE Beman_error
        GET "${Beman_libraryobj}"
        "name"
    )
    if(Beman_error)
        message(FATAL_ERROR "${Beman_error}")
    endif()

    # repo = libraryobj["git_repository"]
    string(
        JSON
        Beman_repo
        ERROR_VARIABLE Beman_error
        GET "${Beman_libraryobj}"
        "git_repository"
    )
    if(Beman_error)
        message(FATAL_ERROR "${Beman_error}")
    endif()

    # Select the field to use to fetch the library based on the value of
    # the BEMAN_USE_MAIN_BRANCHES option
    if(BEMAN_USE_MAIN_BRANCHES)
        set(Beman_tagfieldkey "default_branch")
    else()
        set(Beman_tagfieldkey "git_tag")
    endif()

    # tag = libraryobj[Beman_tagfieldkey]
    string(
        JSON
        Beman_tag
        ERROR_VARIABLE Beman_error
        GET "${Beman_libraryobj}"
        "${Beman_tagfieldkey}"
    )
    if(Beman_error)
        message(FATAL_ERROR "${Beman_error}")
    endif()

    message(DEBUG "Fetching ${Beman_name} from ${Beman_repo} at ${Beman_tag}")
    FetchContent_Declare(
        "${Beman_name}"
        GIT_REPOSITORY "${Beman_repo}"
        GIT_TAG "${Beman_tag}"
    )
    list(APPEND Beman_names "${Beman_name}")
endforeach()

FetchContent_MakeAvailable(${Beman_names})

add_subdirectory(example)
