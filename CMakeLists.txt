cmake
cmake_minimum_required(VERSION 3.4.1)

project(OpenSSL)

# Set the OpenSSL root directory
set(OPENSSL_ROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR})

# Set the target architecture (e.g., arm64-v8a, armeabi-v7a, x86, x86_64)
set(ANDROID_ABI "arm64-v8a")

# Set the Android NDK path
set(ANDROID_NDK "/path/to/your/ndk")

# Set the toolchain file
set(CMAKE_TOOLCHAIN_FILE ${ANDROID_NDK}/build/cmake/android.toolchain.cmake)

# Set the target platform
set(CMAKE_SYSTEM_NAME Android)
set(CMAKE_SYSTEM_VERSION 21) # Adjust as needed

# Configure OpenSSL build options
set(OPENSSL_NO_ASM ON)        # Disable assembly optimizations
set(OPENSSL_NO_SHARED ON)      # Build static libraries
set(OPENSSL_NO_SSL3 ON)        # Disable SSLv3
set(OPENSSL_NO_SSL3_METHOD ON)  # Disable SSLv3 method
set(OPENSSL_NO_TLS1 ON)        # Disable TLSv1
set(OPENSSL_NO_TLS1_1 ON)      # Disable TLSv1.1
set(OPENSSL_NO_TLS1_METHOD ON)  # Disable TLSv1 method
set(OPENSSL_NO_TLS1_1_METHOD ON) # Disable TLSv1.1 method
# ... (Add other desired options here) ...

# Add OpenSSL source files
add_library(ssl STATIC
    crypto/aes/aes_cbc.c
    crypto/aes/aes_cfb.c
    crypto/aes/aes_core.c
    crypto/aes/aes_ecb.c
    crypto/aes/aes_ige.c
    crypto/aes/aes_misc.c
    crypto/aes/aes_ofb.c
    crypto/aes/aes_wrap.c
    # ... (Add all other necessary source files) ...
)

add_library(crypto STATIC
    crypto/aes/aes_cbc.c
    crypto/aes/aes_cfb.c
    crypto/aes/aes_core.c
    crypto/aes/aes_ecb.c
    crypto/aes/aes_ige.c
    crypto/aes/aes_misc.c
    crypto/aes/aes_ofb.c
    crypto/aes/aes_wrap.c
    # ... (Add all other necessary source files) ...
)

# Set target properties
target_include_directories(ssl PUBLIC include)
target_include_directories(crypto PUBLIC include)

# Install OpenSSL libraries
install(TARGETS ssl crypto
    ARCHIVE DESTINATION lib/${ANDROID_ABI}
    LIBRARY DESTINATION lib/${ANDROID_ABI}
)