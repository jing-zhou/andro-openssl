cmake_minimum_required(VERSION 3.6)
project(OpenSSL)

include(CMakePrintHelpers)
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

# Set the OpenSSL root directory
set(OPENSSL_ROOT_DIR "${CMAKE_SOURCE_DIR}/openssl")

# Set the target architecture (e.g., arm64-v8a, armeabi-v7a, x86, x86_64)
set(ANDROID_ABI "arm64-v8a")

# Set the Android NDK path
set(ANDROID_NDK "${ANDROID_NDK_ROOT}")

# set install prefix
set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/install")

# Set the toolchain file
set(CMAKE_TOOLCHAIN_FILE ${ANDROID_NDK}/build/cmake/android.toolchain.cmake)

# Set the target platform
set(CMAKE_SYSTEM_NAME Android)
set(CMAKE_SYSTEM_VERSION 30) # Adjust as needed

# Configure OpenSSL build options
set(OPENSSL_NO_ASM ON) # Disable assembly optimizations
set(OPENSSL_NO_SHARED ON) # Build static libraries
# ... (Add other desired options here) ...

include_directories(${OPENSSL_ROOT_DIR}/include)
# Add OpenSSL source files
add_library(ssl STATIC
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_cbc.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_cfb.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_core.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_ecb.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_ige.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_misc.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_ofb.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_wrap.c
    # ... (Add all other necessary source files) ...
)

add_library(crypto STATIC
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_cbc.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_cfb.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_core.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_ecb.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_ige.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_misc.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_ofb.c
    ${OPENSSL_ROOT_DIR}/crypto/aes/aes_wrap.c
    # ... (Add all other necessary source files) ...
)

# Set target properties
target_include_directories(ssl PUBLIC ${OPENSSL_ROOT_DIR}/include)
target_include_directories(crypto PUBLIC ${OPENSSL_ROOT_DIR}/include)

# Install OpenSSL libraries
install(TARGETS ssl crypto
    ARCHIVE DESTINATION lib/openssl/${ANDROID_ABI}
    LIBRARY DESTINATION lib/openssl/${ANDROID_ABI}
)

install(FILES 
    ${OPENSSL_ROOT_DIR}/include/internal/deprecated.h 
    ${OPENSSL_ROOT_DIR}/include/openssl/opensslv.h.in 
    DESTINATION include/openssl
)
