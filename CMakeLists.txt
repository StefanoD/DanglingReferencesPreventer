cmake_minimum_required(VERSION 3.16)
project(MyProject)

execute_process(COMMAND git rev-parse --git-path hooks
                WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
                OUTPUT_VARIABLE GIT_HOOK_DIR
                OUTPUT_STRIP_TRAILING_WHITESPACE)

if(NOT IS_ABSOLUTE ${GIT_HOOK_DIR})
    set(GIT_HOOK_DIR "${CMAKE_CURRENT_LIST_DIR}/${GIT_HOOK_DIR}")
endif()

# Your CMake Config in your root folder

# Copy python script to the folder .git/hooks
configure_files(${PROJECT_SOURCE_DIR}/Workspace_Setup/Git_Hooks/ ${GIT_HOOK_DIR})
