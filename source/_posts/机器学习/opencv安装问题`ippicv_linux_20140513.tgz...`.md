---
layout: '[poto]'
title: opencv安装问题`ippicv_linux_20140513.tgz...`
date: 2017-11-13 21:06:58
tags: [机器学习,笔记]
categories: [机器学习]
---
## Ubuntu14.04安装OpenCV3.0 :http://blog.csdn.net/u011762313/article/details/47263845


## opencv安装问题`ippicv_linux_20140513.tgz...`

```
-- ICV: Downloading ippicv_linux_20140513.tgz...
CMake Error at 3rdparty/ippicv/downloader.cmake:71 (file):
  file DOWNLOAD HASH mismatch

    for file: [/home/kuo/opencv/opencv-3.0.0-alpha/3rdparty/ippicv/downloads/linux-d80cb24f3a565113a9d6dc56344142f6/ippicv_linux_20140513.tgz]
      expected hash: [d80cb24f3a565113a9d6dc56344142f6]
        actual hash: [d41d8cd98f00b204e9800998ecf8427e]

Call Stack (most recent call first):
  3rdparty/ippicv/downloader.cmake:108 (_icv_downloader)
  cmake/OpenCVFindIPP.cmake:212 (include)
  cmake/OpenCVFindLibsPerf.cmake:12 (include)
  CMakeLists.txt:449 (include)


CMake Error at 3rdparty/ippicv/downloader.cmake:75 (message):
  ICV: Failed to download ICV package: ippicv_linux_20140513.tgz.
  Status=35;"SSL connect error"
Call Stack (most recent call first):
  3rdparty/ippicv/downloader.cmake:108 (_icv_downloader)
  cmake/OpenCVFindIPP.cmake:212 (include)
  cmake/OpenCVFindLibsPerf.cmake:12 (include)
  CMakeLists.txt:449 (include)


-- Configuring incomplete, errors occurred!
See also "/home/kuo/opencv/opencv-3.0.0-alpha/CMakeFiles/CMakeOutput.log".
See also "/home/kuo/opencv/opencv-3.0.0-alpha/CMakeFiles/CMakeError.log".

```


https://osdn.net/projects/sfnet_opencvlibrary/downloads/3rdparty/ippicv/ippicv_linux_20140513.tgz/
