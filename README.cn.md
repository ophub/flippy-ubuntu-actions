# 功能说明

[English Instructions](README.md) | [中文说明](README.cn.md)

[unifreq/rk-ubuntu-build](https://github.com/unifreq/rk-ubuntu-build) 是 `Flippy` 开发的 Ubuntu 镜像构建脚本仓库，支持 Rockchip 系列设备，如 `rock-5b`、`h28k` 等。

本 Action 直接使用其构建脚本，未做任何修改，仅在此基础上进行了智能化的 GitHub Action 应用封装，使通过 GitHub Actions 在线构建更加便捷和灵活。

## Ubuntu 固件默认信息

| 默认账号 | 默认密码  | SSH 端口 | IP 地址 |
| ------- | ------- | ------- | ------- |
| root | root | 22 | 从路由器获取 IP |

## 使用方法

在 `.github/workflows/*.yml` 工作流配置文件中引入本 Action 即可使用，示例参考 [build-ubuntu.yml](.github/workflows/build-ubuntu.yml)。

推荐使用 ARM64 架构的 Runner 进行构建：`runs-on: ubuntu-24.04-arm`

```yaml
- name: Build Ubuntu
  uses: ophub/flippy-build-actions@main
  env:
    BUILD_TARGET: image
    ENV_MACHINE: e20c_h28k
    ENV_LINUX_FLAVOR: noble-xfce
    ENV_CUSTOM_BOOT: boot256-ext4root
    KERNEL_VERSION_NAME: 6.1.y_6.12.y
```

## 可选参数说明

基于 `Flippy` 最新发布的内核构建脚本，本 Action 提供了 `内核版本`、`目标设备 SoC` 等可选参数配置。

| 参数                   | 默认值                  | 说明                                            |
|------------------------|------------------------|------------------------------------------------|
| SCRIPT_REPO_URL        | unifreq/rk-ubuntu-build | 设置构建脚本源码仓库，格式为 `<owner>/<repo>` |
| SCRIPT_REPO_BRANCH     | main                   | 设置构建脚本源码仓库的分支                        |
| KERNEL_REPO_URL        | breakingbadboy/OpenWrt | 设置内核下载仓库，格式为 `<owner>/<repo>`。默认从 breakingbadboy 维护的[内核 Releases](https://github.com/breakingbadboy/OpenWrt/releases/tag/kernel_stable) 下载。 |
| KERNEL_VERSION_NAME    | 6.12.y                 | 设置[主线内核版本](https://github.com/breakingbadboy/OpenWrt/releases/tag/kernel_stable)，可查看并选择指定版本。支持指定单个内核（如 `6.1.y`），也可使用 `_` 连接多个内核（如 `6.1.y_6.12.y`） |
| KERNEL_AUTO_LATEST     | true                   | 设置是否自动采用同系列最新版本内核。设为 `true` 时，将自动在内核仓库中查找 `KERNEL_VERSION_NAME` 所指定内核（如 `6.1.y`）的同系列最新版本，若存在更新版本则自动替换。设为 `false` 时将使用指定版本内核进行构建。 |
| BUILD_TARGET           | image                  | 设置构建目标类型：完整镜像或 rootfs 文件。可选值为 `image` / `rootfs`，默认为 `image` |
| ENV_MACHINE            | all                    | 设置目标设备的 SoC，默认为 `all`（构建全部设备）。支持指定单个设备（如 `e20c`），也可使用 `_` 连接多个设备（如 `e20c_e54c`）。可选值参考：[env/machine](https://github.com/unifreq/rk-ubuntu-build/tree/main/env/machine) |
| ENV_LINUX_FLAVOR       | noble-rk-media         | 设置构建镜像或 rootfs 所使用的 Linux 发行版风格，默认为 `noble-rk-media`。可选值参考：[env/linux](https://github.com/unifreq/rk-ubuntu-build/tree/main/env/linux) |
| ENV_CUSTOM_BOOT        | boot256-ext4root       | 设置自定义引导配置，默认为 `boot256-ext4root`。可选值参考：[env/custom](https://github.com/unifreq/rk-ubuntu-build/tree/main/env/custom) |
| GZIP_IMGS              | auto                   | 设置构建完成后的文件压缩格式，可选值为 `.gz`（默认） / `.xz` / `.zip` / `.zst` / `.7z` |
| SELECT_PACKITPATH      | rk-ubuntu-build        | 设置 `/opt` 下的构建工作目录名称                  |
| SELECT_OUTPUTPATH      | output                 | 设置 `${SELECT_PACKITPATH}` 目录下的固件输出目录名称 |
| SCRIPT_MKIMG           | mkimg.sh               | 设置构建镜像的脚本文件名                         |
| SCRIPT_MKROOTFS        | mkrootfs.sh            | 设置构建 rootfs 的脚本文件名                     |


## 输出参数说明

按照 GitHub Actions 的规范输出了 3 个环境变量，供后续工作流步骤使用。由于 GitHub 已将 Fork 仓库的 Workflow 读写权限默认关闭，因此上传至 `Releases` 前需要手动开启 `Workflow 读写权限`，详见[使用说明](https://github.com/ophub/amlogic-s9xxx-openwrt/blob/main/documents/README.cn.md#3-fork-仓库并设置工作流权限)。

| 参数                         | 默认值                       | 说明               |
|-----------------------------|-----------------------------|--------------------|
| ${{ env.BUILD_OUTPUTPATH }} | /opt/rk-ubuntu-build/output | 构建产物输出路径     |
| ${{ env.BUILD_OUTPUTDATE }} | 07.15.1058                  | 构建日期（月.日.时分）|
| ${{ env.BUILD_STATUS }}     | success / failure           | 构建状态：成功 / 失败 |

## 相关链接
- [unifreq/rk-ubuntu-build](https://github.com/unifreq/rk-ubuntu-build)
- [breakingbadboy/OpenWrt](https://github.com/breakingbadboy/OpenWrt)
- [ophub/kernel](https://github.com/ophub/kernel)

## License

The flippy-ubuntu-actions © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/flippy-ubuntu-actions/blob/main/LICENSE)
