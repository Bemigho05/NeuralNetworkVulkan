# CMakeList.txt : CMake project for NeuralNetworkVulkan, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

# Enable Hot Reload for MSVC compilers if supported.
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project ("NeuralNetworkVulkan")

# Add source to this project's executable.
add_executable (NeuralNetworkVulkan "main.cpp")

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET NeuralNetworkVulkan PROPERTY CXX_STANDARD 20)
endif()

# TODO: Add tests and install targets if needed.
find_package(glfw3 CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)

if(DEFINED ENV{VULKAN_SDK})
    message(STATUS "Using Vulkan SDK: $ENV{VULKAN_SDK}")
    include_directories("$ENV{VULKAN_SDK}/Include")
    find_package(Vulkan REQUIRED COMPONENTS glslc shaderc_combined)
    set(Vulkan_LIBRARY "$ENV{VULKAN_SDK}/Lib/vulkan-1.lib")
else()
    message(FATAL_ERROR "VULKAN_SDK environment variable not set!")
endif()

target_link_libraries(NeuralNetworkVulkan PRIVATE
    glfw
    glm::glm
    ${Vulkan_LIBRARY}
)

target_include_directories(NeuralNetworkVulkan PRIVATE
    "$ENV{VULKAN_SDK}/Include"
    ${CMAKE_CURRENT_BINARY_DIR}/vcpkg_installed/x64-windows/include
)
