FROM ubuntu:16.04

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    cmake \
    gcc \
    libc6-dev \
    make \
    pkg-config \
    git \
    automake \
    libtool \
    m4 \
    autoconf \
    make \
    file \
    binutils \
    # Clang is needed in order for the Rust bindgen to work.
    # cf. https://github.com/rust-embedded/cross/issues/174
    llvm-3.9-dev libclang-3.9-dev clang-3.9 \
    libc6-dev-i386 && \
    apt-get clean

#COPY xargo.sh /
#RUN bash /xargo.sh

COPY android-ndk.sh /
RUN bash /android-ndk.sh arm 21
ENV PATH=$PATH:/android-ndk/bin

#COPY android-system.sh /
#RUN bash /android-system.sh arm

#COPY qemu.sh /
#RUN bash /qemu.sh arm android

#COPY openssl.sh /
#RUN bash /openssl.sh android-armv7 arm-linux-androideabi-

#RUN cp /android-ndk/sysroot/usr/lib/libz.so /system/lib/

# Libz is distributed in the android ndk, but for some unknown reason it is not
# found in the build process of some crates, so we explicit set the DEP_Z_ROOT
ENV CARGO_TARGET_ARMV7_LINUX_ANDROIDEABI_LINKER=arm-linux-androideabi-gcc \
    CARGO_TARGET_ARMV7_LINUX_ANDROIDEABI_RUNNER=qemu-arm \
    CC_armv7_linux_androideabi=arm-linux-androideabi-gcc \
    CXX_armv7_linux_androideabi=arm-linux-androideabi-g++ \
    DEP_Z_INCLUDE=/android-ndk/sysroot/usr/include/ \
    OPENSSL_STATIC=1 \
    OPENSSL_DIR=/openssl \
    OPENSSL_INCLUDE_DIR=/openssl/include \
    OPENSSL_LIB_DIR=/openssl/lib \
    RUST_TEST_THREADS=1 \
    HOME=/tmp/ \
    TMPDIR=/tmp/ \
    ANDROID_DATA=/ \
    ANDROID_DNS_MODE=local \
    ANDROID_ROOT=/system
