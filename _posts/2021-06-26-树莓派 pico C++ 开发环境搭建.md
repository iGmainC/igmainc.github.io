---
title: 树莓派 pico C\C++ 开发环境搭建
tags: 树莓派Pico 硬件
---

# 安装依赖库

```shell
sudo apt update
sudo apt install cmake gcc-arm-none-eabi build-essential libnewlib-arm-none-eabi libstdc++-arm-none-eabi-newlib
```

# 下载 SDK

```shell
git clone https://github.com/raspberrypi/pico-sdk.git
cd pico-sdk
git submodule update --init
```

# 设置环境变量

```shell
export PICO_SDK_PATH=sdk_path
```

# 复制 cmake 文件

从 `pico-sdk/external` 下复制 `pico_sdk_import.cmake` 到项目目录
