# 编译问题处理说明

1. Found no assembler Minimum version is nasm-2.13 If you really want to compile without asm, configure with --disable-asm. （缺少nasm 通过brew安装 ）
brew install nasm

2. No working C compiler found.（编译x264时报的错， 如果fdk-aac编译时也报这个也这样处理）
CFLAGS="$CFLAGS -mios-simulator-version-min=7.0" 和 5.0 都改为8.0 就好了，我的是这样

3. no member named “encoderDelay” （ffmpeg编译集成fdk-aac报的错误）
ffmpeg针对fdk-aac,存在如下patch解决此问题。大家可以参考下：https://github.com/libav/libav/commit/141c960e21d2860e354f9b90df136184dd00a9a8.patch 这是由于使用的fdk-aac版本太新，数据结构有所改变。修改文件目录ffmpeg-4.0.2/libavcodec/libfdk-aacenc.c 。另外一种变通的修改方式，降低fdk-aac的版本，也可以解决问题

4. GNU assembler not found, install/update gas-preprocessor
是因为 gas-preprocessor.pl 版本太老了，去这个地址 https://github.com/libav/gas-preprocessor 下载最新的，下载完放在/usr/local/lib/下，增加权限即可。

5. 如果需要更新 x264 版本，可以从 https://download.videolan.org/pub/x264/snapshots/ 查看需要的版本，然后修改编译脚本（build-x264.sh）中的 X264_SOURCE_VERSION 变量，再执行脚本即可。


# x264 iOS build script

**My blog: [http://depthlove.github.io/](http://depthlove.github.io/)**

This is a shell script to build x264 for iOS apps.

Tested with:

* x264-snapshot-20140914-2245
* Xcode 6.4

## Requirements

* https://github.com/libav/gas-preprocessor

## Usage

* To build everything:

        ./build-x264.sh

* To build for arm64:

        ./build-x264.sh arm64

* To build fat library for armv7 and x86_64 (64-bit simulator):

        ./build-x264.sh armv7 x86_64

* To build fat library from separately built thin libraries:

        ./build-x264.sh lipo

#### About x264 encode video streaming to h264, you can watch my paper [http://depthlove.github.io/2015/09/17/use-x264-encode-iOS-camera-video-to-h264/](http://depthlove.github.io/2015/09/17/use-x264-encode-iOS-camera-video-to-h264/)

##### 关于x264编码视频流为h264的内容，可以参看我的文章：[http://depthlove.github.io/2015/09/17/use-x264-encode-iOS-camera-video-to-h264/](http://depthlove.github.io/2015/09/17/use-x264-encode-iOS-camera-video-to-h264/)

---

##### If you want to know more things, you can view my github blog: [http://depthlove.github.io/](http://depthlove.github.io/).

##### 更多内容可参看我的github博客：[http://depthlove.github.io/](http://depthlove.github.io/)
