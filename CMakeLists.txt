cmake_minimum_required(VERSION 3.10)
project(AndroidOpenSSL)


include(CMakePrintHelpers)
include(ExternalProject)

include(FetchContent)
function(CloneRepository repositoryURL branchName projectName sourceDir)
    #Commands are left empty so that we only checkout the source and no not perform any kind of build
    message("Starting to clone ${projectName} into ${sourceDir}")
    FetchContent_Declare(
            "${projectName}"
            GIT_REPOSITORY "${repositoryURL}"
            SOURCE_DIR "${sourceDir}"
            GIT_TAG "${branchName}"
            CONFIGURE_COMMAND ""
            BUILD_COMMAND ""
            INSTALL_COMMAND ""
    )
    FetchContent_MakeAvailable(${projectName})
endfunction(CloneRepository)
CloneRepository("https://github.com/jing-zhou/openssl.git" "openssl-3.4.0" "OpenSSL" "${CMAKE_SOURCE_DIR}/openssl")


set(OPENSSL_SOURCE_DIR ${CMAKE_SOURCE_DIR}/openssl)
set(OPENSSL_BUILD_DIR ${CMAKE_BINARY_DIR}/openssl-build)

# set install prefix
set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/install")

# Set the OpenSSL root directory
set(OPENSSL_ROOT_DIR "${CMAKE_INSTALL_PREFIX}") 

cmake_print_variables(CMAKE_SOURCE_DIR)
cmake_print_variables(CMAKE_BINARY_DIR)
cmake_print_variables(OPENSSL_SOURCE_DIR)
cmake_print_variables(OPENSSL_BUILD_DIR)
cmake_print_variables(CMAKE_INSTALL_PREFIX)
cmake_print_variables(OPENSSL_ROOT_DIR)

# Set the Android NDK path
set(ANDROID_NDK "${ANDROID_NDK_ROOT}")

# Set the toolchain file
set(CMAKE_TOOLCHAIN_FILE ${ANDROID_NDK}/build/cmake/android.toolchain.cmake)

# Set the target platform
set(CMAKE_SYSTEM_NAME Android)
set(CMAKE_SYSTEM_VERSION 30) # Adjust as needed

# Set the target architecture (e.g., arm64-v8a, armeabi-v7a, x86, x86_64)
set(ANDROID_ABI "arm64-v8a")

add_custom_target(openssl_configure
    COMMAND ${CMAKE_COMMAND} -E env 
    PATH=${ANDROID_NDK}/toolchains/llvm/prebuilt/linux-x86_64/bin:$ENV{PATH} 
    ${CMAKE_COMMAND} -E chdir ${OPENSSL_SOURCE_DIR} 
    ./Configure android-${ANDROID_ABI} no-shared no-asm --prefix=${OPENSSL_BUILD_DIR}
    WORKING_DIRECTORY ${OPENSSL_SOURCE_DIR}
    DEPENDS openssl
    COMMENT "Running OpenSSL configure for Android"
)

add_custom_target(openssl_build
    COMMAND ${CMAKE_COMMAND} -E chdir ${OPENSSL_BUILD_DIR} make
    WORKING_DIRECTORY ${OPENSSL_BUILD_DIR}
    DEPENDS openssl_configure
    COMMENT "Building OpenSSL for Android"
)

add_custom_target(openssl_install
    COMMAND ${CMAKE_COMMAND} -E chdir ${OPENSSL_BUILD_DIR} make install
    WORKING_DIRECTORY ${OPENSSL_BUILD_DIR}
    DEPENDS openssl_build
    COMMENT "Installing OpenSSL for Android"
)