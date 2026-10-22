cmake_minimum_required(VERSION 3.25.2)
project(CS171-HW2 LANGUAGES C CXX)

if(NOT COMMAND CPMAddPackage)
  if(NOT DEFINED ENV{CPM_SOURCE_CACHE})
    set(ENV{CPM_SOURCE_CACHE} "${CMAKE_SOURCE_DIR}/CPM")
    file(MAKE_DIRECTORY "$ENV{CPM_SOURCE_CACHE}")

    message(STATUS "CPM_SOURCE_CACHE is not defined, using: $ENV{CPM_SOURCE_CACHE}")
  endif()

  include(FetchContent)
  FetchContent_Declare(
    CPM
    GIT_REPOSITORY https://github.com/cpm-cmake/CPM.cmake.git
    GIT_TAG v0.43.1
  )
  FetchContent_MakeAvailable(CPM)
endif()

CPMAddPackage(
  NAME arietta
  GITHUB_REPOSITORY Nagisaaaaaaaaa/arietta
  GIT_TAG 2a427f2abd5341582f4f64a4b4b66806cab3998f
  OPTIONS "ARIETTA_ENABLE_TESTING OFF"
)

CPMAddPackage(
  NAME glad
  GITHUB_REPOSITORY Dav1dde/glad
  GIT_TAG v2.0.8
  SOURCE_SUBDIR cmake
)
include(cmake/SetupGladPython.cmake)
glad_add_library(glad_gl_core_41 STATIC REPRODUCIBLE API gl:core=4.1 EXTENSIONS NONE)

CPMAddPackage(
  NAME glfw
  GITHUB_REPOSITORY glfw/glfw
  GIT_TAG 3.5.1
  OPTIONS "GLFW_BUILD_DOCS OFF" "GLFW_BUILD_EXAMPLES OFF" "GLFW_BUILD_TESTS OFF" "GLFW_INSTALL OFF"
)

#
#
#
file(GLOB_RECURSE SRC_FILES
  CONFIGURE_DEPENDS
  "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp"

  # "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cu"
)
add_executable(${PROJECT_NAME} ${SRC_FILES})
target_include_directories(${PROJECT_NAME} PRIVATE include)

target_compile_definitions(${PROJECT_NAME} PRIVATE GLFW_INCLUDE_NONE)
target_link_libraries(${PROJECT_NAME} PRIVATE
  Arietta
  glad_gl_core_41
  glfw
)

# The sources are UTF-8 encoded.
target_compile_options(${PROJECT_NAME} PRIVATE $<$<CXX_COMPILER_ID:MSVC>:/utf-8>)
