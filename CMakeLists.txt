cmake_minimum_required(VERSION 3.6)
project(OpenSSL)

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

# Set the toolchain file
set(CMAKE_TOOLCHAIN_FILE ${ANDROID_NDK}/build/cmake/android.toolchain.cmake)

# Set the target platform
set(CMAKE_SYSTEM_NAME Android)
set(CMAKE_SYSTEM_VERSION 30) # Adjust as needed

# Configure OpenSSL build options
set(OPENSSL_NO_ASM ON) # Disable assembly optimizations
set(OPENSSL_NO_SHARED ON) # Build static libraries
set(OPENSSL_NO_SSL3 ON) # Disable SSLv3
set(OPENSSL_NO_SSL3_METHOD ON) # Disable SSLv3 method
set(OPENSSL_NO_TLS1 ON) # Disable TLSv1
set(OPENSSL_NO_TLS1_1 ON) # Disable TLSv1.1
set(OPENSSL_NO_TLS1_METHOD ON) # Disable TLSv1 method
set(OPENSSL_NO_TLS1_1_METHOD ON) # Disable TLSv1.1 method
set(OPENSSL_NO_COMP ON) # Disable compression
set(OPENSSL_NO_HEARTBEATS ON) # Disable heartbeats
set(OPENSSL_NO_EC2M ON) # Disable EC2M
set(OPENSSL_NO_ENGINE ON) # Disable engine
set(OPENSSL_NO_DYNAMIC_ENGINE ON) # Disable dynamic engine
set(OPENSSL_NO_NEXTPROTONEG ON) # Disable Next Protocol Negotiation
set(OPENSSL_NO_SRTP ON) # Disable SRTP
set(OPENSSL_NO_DGRAM ON) # Disable datagram support
set(OPENSSL_NO_SCTP ON) # Disable SCTP support
set(OPENSSL_NO_SOCK ON) # Disable socket support
set(OPENSSL_NO_FILTERS ON) # Disable filters
set(OPENSSL_NO_DEPRECATED ON) # Disable deprecated features
set(OPENSSL_NO_WEAK_SSL_CIPHERS ON) # Disable weak SSL ciphers
set(OPENSSL_NO_SSL2 ON) # Disable SSLv2
set(OPENSSL_NO_SSL2_METHOD ON) # Disable SSLv2 method
set(OPENSSL_NO_TLS1_2 ON) # Disable TLSv1.2
set(OPENSSL_NO_TLS1_2_METHOD ON) # Disable TLSv1.2 method
set(OPENSSL_NO_TLS1_3 ON) # Disable TLSv1.3
set(OPENSSL_NO_TLS1_3_METHOD ON) # Disable TLSv1.3 method
set(OPENSSL_NO_DTLS ON) # Disable DTLS
set(OPENSSL_NO_DTLS1 ON) # Disable DTLSv1
set(OPENSSL_NO_DTLS1_METHOD ON) # Disable DTLSv1 method
set(OPENSSL_NO_DTLS1_2 ON) # Disable DTLSv1.2
set(OPENSSL_NO_DTLS1_2_METHOD ON) # Disable DTLSv1.2 method
set(OPENSSL_NO_RFC3779 ON) # Disable RFC3779
set(OPENSSL_NO_CMS ON) # Disable CMS
set(OPENSSL_NO_OCSP ON) # Disable OCSP
set(OPENSSL_NO_TS ON) # Disable TS
set(OPENSSL_NO_STORE ON) # Disable store
set(OPENSSL_NO_UI_CONSOLE ON) # Disable UI consoleset(OPENSSL_NO_UI ON) # Disable UI
set(OPENSSL_NO_CRYPTO_MDEBUG ON) # Disable crypto mdebug
set(OPENSSL_NO_CRYPTO_MDEBUG_BACKTRACE ON) # Disable crypto mdebug backtrace
set(OPENSSL_NO_UNIT_TEST ON) # Disable unit test
set(OPENSSL_NO_IDEA ON) # Disable IDEA
set(OPENSSL_NO_MD2 ON) # Disable MD2
set(OPENSSL_NO_MD4 ON) # Disable MD4
set(OPENSSL_NO_MDC2 ON) # Disable MDC2
set(OPENSSL_NO_RC2 ON) # Disable RC2
set(OPENSSL_NO_RC4 ON) # Disable RC4
set(OPENSSL_NO_RC5 ON) # Disable RC5
set(OPENSSL_NO_BF ON) # Disable Blowfish
set(OPENSSL_NO_CAST ON) # Disable CAST
set(OPENSSL_NO_DES ON) # Disable DES
set(OPENSSL_NO_WHIRLPOOL ON) # Disable Whirlpool
set(OPENSSL_NO_RMD160 ON) # Disable RIPEMD-160
set(OPENSSL_NO_SHA ON) # Disable SHA-1
set(OPENSSL_NO_SHA0 ON) # Disable SHA-0
set(OPENSSL_NO_CAMELLIA ON) # Disable Camellia
set(OPENSSL_NO_SEED ON) # Disable SEED
set(OPENSSL_NO_SM2 ON) # Disable SM2
set(OPENSSL_NO_SM3 ON) # Disable SM3
set(OPENSSL_NO_SM4 ON) # Disable SM4
set(OPENSSL_NO_BLAKE2 ON) # Disable BLAKE2
set(OPENSSL_NO_ZLIB ON) # Disable ZLIB
set(OPENSSL_NO_ZLIB_DYNAMIC ON) # Disable ZLIB dynamic
set(OPENSSL_NO_PADLOCKENG ON) # Disable Padlock engine
set(OPENSSL_NO_AFALGENG ON) # Disable AFALG engine
set(OPENSSL_NO_DEVCRYPTOENG ON) # Disable devcrypto engine
set(OPENSSL_NO_STATIC_ENGINE ON) # Disable static engine
set(OPENSSL_NO_GOSTENG ON) # Disable GOST engine
set(OPENSSL_NO_RDSEED ON) # Disable RDSEED
set(OPENSSL_NO_DEPRECATED_3_0 ON) # Disable deprecated features in OpenSSL 3.0
set(OPENSSL_NO_DEPRECATED_1_1_0 ON) # Disable deprecated features in OpenSSL 1.1.0
set(OPENSSL_NO_DEPRECATED_1_0_2 ON) # Disable deprecated features in OpenSSL 1.0.2
set(OPENSSL_NO_DEPRECATED_1_0_1 ON) # Disable deprecated features in OpenSSL 1.0.1
set(OPENSSL_NO_DEPRECATED_0_9_8 ON) # Disable deprecated features in OpenSSL 0.9.8
set(OPENSSL_NO_DEPRECATED_0_9_7 ON) # Disable deprecated features in OpenSSL 0.9.7
set(OPENSSL_NO_DEPRECATED_0_9_6 ON) # Disable deprecated features in OpenSSL 0.9.6
set(OPENSSL_NO_DEPRECATED_0_9_5 ON) # Disable deprecated features in OpenSSL 0.9.5
set(OPENSSL_NO_DEPRECATED_0_9_4 ON) # Disable deprecated features in OpenSSL 0.9.4
set(OPENSSL_NO_DEPRECATED_0_9_3 ON) # Disable deprecated features in OpenSSL 0.9.3
set(OPENSSL_NO_DEPRECATED_0_9_2 ON) # Disable deprecated features in OpenSSL 0.9.2
set(OPENSSL_NO_DEPRECATED_0_9_1 ON) # Disable deprecated features in OpenSSL 0.9.1
set(OPENSSL_NO_DEPRECATED_0_9_0 ON) # Disable deprecated features in OpenSSL 0.9.0
set(OPENSSL_NO_DEPRECATED_0_8_0 ON) # Disable deprecated features in OpenSSL 0.8.0
set(OPENSSL_NO_DEPRECATED_0_7_0 ON) # Disable deprecated features in OpenSSL 0.7.0
set(OPENSSL_NO_DEPRECATED_0_6_0 ON) # Disable deprecated features in OpenSSL 0.6.0
set(OPENSSL_NO_DEPRECATED_0_5_0 ON) # Disable deprecated features in OpenSSL 0.5.0
# ... (Add other desired options here) ...

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
target_include_directories(ssl PUBLIC include)
target_include_directories(crypto PUBLIC include)

# Install OpenSSL libraries
install(TARGETS ssl crypto
    ARCHIVE DESTINATION lib/${ANDROID_ABI}
    LIBRARY DESTINATION lib/${ANDROID_ABI}
)