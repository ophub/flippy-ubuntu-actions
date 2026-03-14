# Description

View Chinese description  |  [查看中文说明](README.cn.md)

[unifreq/rk-ubuntu-build](https://github.com/unifreq/rk-ubuntu-build) is an Ubuntu image build script repository developed by `Flippy`, supporting Rockchip series devices such as `rock-5b`, `h28k`, etc.

This Action uses his build scripts without any modification. It is packaged as a GitHub Action application to make online building via GitHub Actions simpler and more flexible.

## Default Information for Ubuntu Firmware

| Default Username | Default Password  | SSH Port  | IP Address  |
| ---------------- | ----------------- | --------- | ----------- |
| root | root | 22 | Obtained via router DHCP |

## Usage

This Action can be used by referencing it in the `.github/workflows/*.yml` workflow configuration file. See [build-ubuntu.yml](.github/workflows/build-ubuntu.yml) for an example.

It is recommended to use an ARM64 architecture runner for building: `runs-on: ubuntu-24.04-arm`

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

## Optional Parameters

Based on the latest kernel build scripts released by `Flippy`, this Action provides optional parameter configurations for kernel version, target SoC, and more.

| Parameter              | Default                | Description                                                    |
|------------------------|------------------------|---------------------------------------------------------------|
| SCRIPT_REPO_URL        | unifreq/rk-ubuntu-build | Set the `<owner>/<repo>` of the build script source repository |
| SCRIPT_REPO_BRANCH     | main                 | Set the branch of the build script source repository         |
| KERNEL_REPO_URL        | breakingbadboy/OpenWrt | Set the `<owner>/<repo>` of the kernel download repository. By default, kernels are downloaded from the [kernel Releases](https://github.com/breakingbadboy/OpenWrt/releases/tag/kernel_stable) maintained by breakingbadboy. |
| KERNEL_VERSION_NAME    | 6.12.y                 | Set the [mainline kernel version](https://github.com/breakingbadboy/OpenWrt/releases/tag/kernel_stable). Supports specifying a single kernel (e.g., `6.1.y`) or multiple kernels joined with `_` (e.g., `6.1.y_6.12.y`). |
| KERNEL_AUTO_LATEST     | true                   | Set whether to automatically use the latest kernel version within the same series. When set to `true`, the Action will check the kernel repository for a newer version in the same series as specified in `KERNEL_VERSION_NAME` (e.g., `6.1.y`) and automatically use the latest one if available. When set to `false`, the specified version will be used as-is. |
| BUILD_TARGET           | image                  | Set the build target type: full image or rootfs file. Available values: `image` / `rootfs`. Default: `image`. |
| ENV_MACHINE            | all                    | Set the target device SoC. Defaults to `all` (builds for all devices). Supports a single device (e.g., `e20c`) or multiple devices joined with `_` (e.g., `e20c_e54c`). For available values, see: [env/machine](https://github.com/unifreq/rk-ubuntu-build/tree/main/env/machine) |
| ENV_LINUX_FLAVOR       | noble-rk-media         | Set the Linux distribution flavor for the image or rootfs. Default: `noble-rk-media`. For available values, see: [env/linux](https://github.com/unifreq/rk-ubuntu-build/tree/main/env/linux) |
| ENV_CUSTOM_BOOT        | boot256-ext4root       | Set the custom boot configuration. Default: `boot256-ext4root`. For available values, see: [env/custom](https://github.com/unifreq/rk-ubuntu-build/tree/main/env/custom) |
| GZIP_IMGS              | auto                   | Set the compression format for build output files. Available values: `.gz` (default) / `.xz` / `.zip` / `.zst` / `.7z` |
| SELECT_PACKITPATH      | rk-ubuntu-build        | Set the build working directory name under `/opt`            |
| SELECT_OUTPUTPATH      | output                 | Set the firmware output directory name within `${SELECT_PACKITPATH}` |
| SCRIPT_MKIMG           | mkimg.sh               | Set the image build script filename |
| SCRIPT_MKROOTFS        | mkrootfs.sh            | Set the rootfs build script filename |


## Output Parameters

Three environment variables are output following GitHub Actions conventions for use in subsequent workflow steps. Since GitHub now disables Workflow read-write permissions by default for forked repositories, you must enable `Workflow read and write permissions` before uploading to Releases. For details, see the [User Guide](https://github.com/ophub/amlogic-s9xxx-openwrt/tree/main/documents#3-fork-the-repository-and-set-workflow-permissions).

| Parameter                   | Default Value               | Description                         |
|-----------------------------|-----------------------------|-------------------------------------|
| ${{ env.BUILD_OUTPUTPATH }} | /opt/rk-ubuntu-build/output | Build output path                   |
| ${{ env.BUILD_OUTPUTDATE }} | 07.15.1058                  | Build date (MM.DD.HHmm)            |
| ${{ env.BUILD_STATUS }}     | success / failure           | Build status: success or failure    |

## Links

- [unifreq/rk-ubuntu-build](https://github.com/unifreq/rk-ubuntu-build)
- [breakingbadboy/OpenWrt](https://github.com/breakingbadboy/OpenWrt)
- [ophub/kernel](https://github.com/ophub/kernel)

## License

The flippy-ubuntu-actions © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/flippy-ubuntu-actions/blob/main/LICENSE)

