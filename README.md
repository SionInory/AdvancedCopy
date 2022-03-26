# Advanced Copy
Advanced Copy 静态编译二进制文件  
Advanced Copy static link bin file
## Source
[jarun/advcpmv](https://github.com/jarun/advcpmv)  
[luciusmagn/coreutils-static](https://github.com/luciusmagn/coreutils-static)
## Compile
```bash
docker run --rm -it -v "$(pwd)/out":/root/out alpine

apk add alpine-sdk xz perl upx

ADVCPMV_VERSION=0.9
CORE_UTILS_VERSION=9.0

wget http://ftp.gnu.org/gnu/coreutils/coreutils-$CORE_UTILS_VERSION.tar.xz
tar xvJf coreutils-$CORE_UTILS_VERSION.tar.xz
rm coreutils-$CORE_UTILS_VERSION.tar.xz
cd coreutils-$CORE_UTILS_VERSION/
wget https://raw.githubusercontent.com/jarun/advcpmv/master/advcpmv-$ADVCPMV_VERSION-$CORE_UTILS_VERSION.patch
patch -p1 -i advcpmv-$ADVCPMV_VERSION-$CORE_UTILS_VERSION.patch

export CFLAGS="-static"
env FORCE_UNSAFE_CONFIGURE=1 CFLAGS="$CFLAGS -Os -ffunction-sections -fdata-sections" LDFLAGS='-Wl,--gc-sections' ./configure
make

cp src/cp ~/out/
cp src/mv ~/out/

strip -s -R .comment -R .gnu.version --strip-unneeded *
upx -9 *
```