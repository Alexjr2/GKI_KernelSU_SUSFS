### This is a repository for automated GKI kernel builds

> Not supported: OnePlus ColorOS14/15. After flashing you may need to wipe data to boot.
>
> Please read the following carefully on first use — don't waste others' time by being lazy!
>
> Attempted to build a GKI kernel integrated with [hymo mount meta-module](https://github.com/Anatdx/hymo). Because that project currently only supports 6.6, it has not been fully merged into this repository's main branch. Link: [hymo+gki](https://github.com/zzh20188/GKI_KernelSU_SUSFS/releases/tag/v2.0.0-r19)
>
> Attempted to build a GKI kernel with [hymo](https://github.com/Anatdx/hymo) integration. However, since the project currently only supports kernel 6.6, it has not been fully merged into the main branch. Link: [hymo+gki](https://github.com/zzh20188/GKI_KernelSU_SUSFS/releases/tag/v2.0.0-r19)

### Endless reboots?
1. OnePlus/realme/OPPO: enter system recovery and wipe Data, then reboot.
2. Xiaomi: on some devices the boot fails due to AVB verification on the boot partition; disable [AVB verification](https://magiskcn.com/disable-avb).
3. zram: some devices or systems running kernels with zram patches may also trigger this; in that case flash a kernel from the [Release page](https://github.com/Alexjr2/GKI_KernelSU_SUSFS/releases) that does not include zram, or in compilation disable zram patches (if applicable).
4. Other: KSU driver causing bootloop — if the error code is not fixed in the source used to build the kernel, the problem will remain in the compiled kernel.

### Bug reports?
This repository only provides the GKI kernel build flow — i.e., it merges KSU variant drivers into the kernel binary. Relationship: when the corresponding KSU repository or SUSFS updates code, this repository builds kernels that include the latest KSU. If by chance a bug is introduced in the latest KSU commit and you flash a kernel containing that code, you should report the bug to the source that introduced it or wait for the next upstream fix.
Do not assume "this repository updated a buggy kernel — fix it immediately."

### Tips
1. About security patches
    - The security patch date shown in the phone settings is unrelated to the GKI kernel's security patch date — ignore it.
2. About Android versions
    - The Android release of the phone's system is unrelated to the GKI kernel's Android target. Match the kernel to the phone's kernel Android tag.
    - Example: if your phone's kernel version shows `5.10.66-android12-9-00001-g41ff3fa8fop9-ab8161528`, then you need to flash the `android12-5.10.66-2022-01-AnyKernel3.zip` file downloaded from [here](https://github.com/zzh20188/GKI_KernelSU_SUSFS/releases).

### Downloads
You can download releases [here](https://github.com/Alexjr2/GKI_KernelSU_SUSFS/releases)
1. AnyKernel3.zip: Download and it's ready to use!
   - Then use a flasher app such as [KernelFlasher](https://github.com/fatalcoder524/KernelFlasher/releases) to flash the kernel.
2. boot.img: download the boot image that matches your kernel format (no compression, gz, lz4). See [this guide](https://kernelsu.org/zh_CN/guide/installation.html#install-by-kernelsu-boot_image) to find the correct boot.img.
    - Flash with FASTBOOT ([guide](https://magiskcn.com/)) or use a flasher to write to the boot partition on the slot that has root (e.g., Aiwanj, Kernelflasher).

### Supported
| Feature | Notes |
| --- | --- |
| [KernelSU](https://kernelsu.org/zh_CN/) | Includes original, MKSU, SUKISU, NEXT variants |
| [SUSFS4](https://gitlab.com/simonpunk/susfs4ksu) | Kernel-level patches that assist KernelSU hiding features |
| [LZ4KD](https://github.com/ShirkNeko/SukiSU_patch/tree/main/other) | A ZRAM algorithm reportedly from HUAWEI source; patch ported by [云彩之枫](http://www.coolapk.com/u/24963680) |
| [LZ4 1.10.0](https://github.com/lz4/lz4/releasesr) | Upgrade of the default LZ4 algorithm for GKI kernels |

<details>

<summary>Other algorithms supported (can be switched in scene ZRAM)</summary>

### LZ4K, LZ4HC, deflate, 842, ~~zstdn~~, lz4k_oplus

</details>

### KSU Manager & SUSFS module
Because of some reasons, you need the latest manager and module (see below).
> ##### If you keep using an old manager while only updating the kernel (e.g., flashing with ak3), the app may display things incorrectly and look different from others; for example SUKISU may show "LKM", and some NEXT parameters may appear missing...
> ##### The SUKISU built-in SUSFS features are more limited than the module: it lacks try mount/umount count display and some custom UI options.
#### After a successful build you will see zip files like `SukiSU-Manager(13235)` and `susfs-release-1.5.2+_537cdba` — these are the latest manager and susfs module uploaded together with the kernel. In short, they are the latest manager and susfs module that accompany the kernel build.

![example](./assets/action.png)

#### Similarly, these are also included at the bottom of the [Release page](https://github.com/zzh20188/GKI_KernelSU_SUSFS/releases)

![release](./assets/release.png)


### Kernel build time
You can specify the kernel build time when building. Enter a string in the Action input in the specified format.
Example: **Thu Jul 17 14:26:50 UTC 2025**
> This time means 2025-07-17 14:26:50 (UTC).
If you do not specify a time, the kernel build will use the build machine's current time.

### Emergency rescue guide

> [!IMPORTANT]
> **Trigger conditions**  
> Perform rescue when the device cannot boot due to:
> - Flashing an incorrect/incompatible kernel
> - Kernel version mismatches (e.g., flashing a kernel for a different subversion)
1. Enter FASTBOOT mode

- Physical key combo: Power + Volume Down, or use ADB: `adb reboot bootloader`

2. Execute flash command
```bash
$ fastboot flash boot <full-boot.img-file-name>
```
### Ways to obtain original images
1. Extract from existing firmware

- From update zip: unpack and use [payload-dumper tool](https://magiskcn.com/payload-dumper-go-boot.html)

- From firmware package: unpack and directly get boot.img

2. Get externally

- Community searches: look for stock boot for your model on XDA/Coolapk/etc.

- [Remote extraction tools for mobile](https://magiskcn.com/payload-dumper-compose.html)

> [!TIP]
> ### Kernel version compatibility notes
> 
> 1. Cross-subversion flashing rules  
> If the phone's GKI main version is 5.10.x (e.g., 5.10.168), you can flash kernels of the same main version but with a higher subversion (e.g., 5.10.198).  
> Example with `X-lts`: for `android12-5.10.X-lts-AnyKernel3.zip`:
> - `X-lts` indicates Long Term Support (largest subversion, current example 5.10.238)
> - LTS will increment as GKI source updates; other numbers like 198 are fixed
> - ⚠️ Note: LTS being newest ≠ most stable (for example, 6.6.x has an auto-reboot bug)
> 
> 2. Kernel version spoofing method  
> In MT Manager terminal run:
> ```bash
> uname -r | sed 's/^[^-]*//'
> ```
> Copy the output and paste that version into the Action build panel to spoof the kernel version.
> 
> 3. Build optimization suggestions  
> Edit the [workflow files](.github/workflows/kernel-a12-5.10.yml) (e.g., kernel-a12-5.10.yml):
> - ▶️ Remove or comment out GKI versions you don't need (this will speed up builds)

### More
You can leave feedback... I will try!
