# Function description

View Chinese description  |  [查看中文说明](README.cn.md)

[unifreq/rk-ubuntu-build](https://github.com/unifreq/rk-ubuntu-build) is an Ubuntu build script repository developed by `Flippy`. It supports Rockchip series devices such as `rock-5b`, `h28k`, etc.

This Actions uses his building scripts without any modification, only developed into a smart Action application, making the use of github Actions for building simpler and more personalized.

## Default Information for OpenWrt Firmware

| Default Username | Default Password  | SSH Port  | IP Address  |
| ---------------- | ----------------- | --------- | ----------- |
| root | root | 22 | From router DHCP |

## Usage

This Actions can be used by referencing it in the `.github/workflows/*.yml` cloud compilation script, for example [build-ubuntu.yml](.github/workflows/build-ubuntu.yml). The code is as follows:

```yaml
- name: Build Ubuntu
  uses: ophub/flippy-build-actions@main
  env:
    MAKE_TARGET: img
    UBUNTU_SOC: e20c_h28k
    ENV_LINUX_FLAVOR: noble-xfce
    ENV_CUSTOM_BOOT: boot256-ext4root
    KERNEL_VERSION_NAME: 6.1.y_6.12.y
```

## Optional Parameters

Based on the latest kernel building scripts released by `Flippy`, optional parameter configurations have been made for `Select Kernel Version`, `Select SoC`, and so on.

| Parameter              | Default                | Description                                                    |
|------------------------|------------------------|---------------------------------------------------------------|
| SCRIPT_REPO_URL        | unifreq/rk-ubuntu-build | Set `<owner>/<repo>` of the building script source repository |
| SCRIPT_REPO_BRANCH     | main                 | Set the branch of the building script source repository      |
| KERNEL_REPO_URL        | breakingbadboy/OpenWrt | Set `<owner>/<repo>` of the kernel download repository, it downloads from the [kernel Releases](https://github.com/breakingbadboy/OpenWrt/releases/tag/kernel_stable) maintained by breakingbadboy by default. |
| KERNEL_VERSION_NAME    | 6.12.y                 | Set the [Mainline Kernel version](https://github.com/breakingbadboy/OpenWrt/releases/tag/kernel_stable), you can check and select a specific one. You can specify a single kernel such as `6.1.y`, or select multiple kernels connected with `_` like `6.1.y_6.12.y` |
| KERNEL_AUTO_LATEST     | true                   | Set whether to automatically adopt the latest version kernel of the same series. When set to `true`, it will automatically look for whether there is an updated version of the kernel specified in `KERNEL_VERSION_NAME`, such as `6.1.y`, in the kernel library, and if there is an updated version, it will automatically replace it with the latest version. When set to `false`, it will compile the specified version kernel. |
| UBUNTU_SOC             | all                    | Set the target SoC. Defaults to all (builds all devices). Supports single device (e.g., e20c) or multiple devices separated by _ (e.g., e20c_e54c). For available values, see: [env/machine](https://github.com/unifreq/rk-ubuntu-build/tree/main/env/machine) |
| MAKE_TARGET            | img                    | Set whether to build an img image or rootfs file. Optional values are `img` / `rootfs`. Default is `img`. |
| ENV_LINUX_FLAVOR       | noble-rk-media         | Set the Linux distribution flavor for the image or rootfs. Default is `noble-rk-media`. For available values, see: [env/linux](https://github.com/unifreq/rk-ubuntu-build/tree/main/env/linux) |
| ENV_CUSTOM_BOOT        | boot256-ext4root       | Set the custom boot mode. Default is `boot256-ext4root`. For available values, see: [env/custom](https://github.com/unifreq/rk-ubuntu-build/tree/main/env/custom) |
| GZIP_IMGS              | auto                   | Set the format of the file compression after building, optional values are `.gz` (default) / `.xz` / `.zip` / `.zst` / `.7z` |
| SELECT_PACKITPATH      | rk-ubuntu-build        | Set the name of the building directory under `/opt`          |
| SELECT_OUTPUTPATH      | output                 | Set the name of the firmware output directory in the `${SELECT_PACKITPATH}` directory |
| SCRIPT_MKIMG           | mkimg.sh               | Set the image building script filename |
| SCRIPT_MKROOTFS        | mkrootfs.sh            | Set the rootfs building script filename |


## Output Parameters Explanation

According to the standard of github.com, 3 environment variables have been output for use in subsequent compilation steps. Since github.com recently changed the settings of fork repositories, read-write permissions for Workflow are turned off by default. Therefore, Therefore, to upload to Releases, it is necessary to `set Workflow read and write permissions`. For details, see [User Manual](https://github.com/ophub/amlogic-s9xxx-openwrt/tree/main/documents#3-fork-the-repository-and-set-workflow-permissions).

| Parameter                   | Default Value               | Description                         |
|-----------------------------|-----------------------------|-------------------------------------|
| ${{ env.BUILD_OUTPUTPATH }} | /opt/rk-ubuntu-build/output | Build output path                   |
| ${{ env.BUILD_OUTPUTDATE }} | 07.15.1058                  | Building date                       |
| ${{ env.BUILD_STATUS }}     | success / failure           | Building status. Success / Failure  |

## Links

- [unifreq/rk-ubuntu-build](https://github.com/unifreq/rk-ubuntu-build)
- [breakingbadboy/OpenWrt](https://github.com/breakingbadboy/OpenWrt)
- [ophub/kernel](https://github.com/ophub/kernel)

## License

The flippy-ubuntu-actions © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/flippy-ubuntu-actions/blob/main/LICENSE)

