## A Beginner-Friendly Guide to Compiling Your First Android Kernel..!  

**What You'll Learn:**  

- Downloading the kernel source for your device (Samsung only)
- Understanding the kernel root & choosing the right compilers for compilation
- Customizing the kernel
- Remove Samsung's anti-root protections.  
- Implementing KernelSU (to-do)
- Creating a signed boot image from the compiled kernel

**What You'll Need:** 
- A working 🧠
- Ubuntu/Debian based PC/Server
- Knowledge of basic commands in Linux, and Bash/Shell script knowledge
- Understanding of English.
- Patience

#### Additional Notes:

- You can also use [Gitpod](https://gitpod.io/workspaces) if you don't want to install a Linux distro.  
  - Keep in mind, though, that it might be more challenging for beginners who are not familiar with the command-line interface.  
  - Access the terminal from Gitpod and its GUI using [ravindu644/LinuxRDP](https://github.com/ravindu644/LinuxRDP).
	
### Dependencies for compiling kernels : (Paste this in terminal.)
 ```
sudo apt update && sudo apt install -y git device-tree-compiler lz4 xz-utils zlib1g-dev openjdk-17-jdk gcc g++ python3 python-is-python3 p7zip-full android-sdk-libsparse-utils erofs-utils \
default-jdk git gnupg flex bison gperf build-essential zip curl libc6-dev libncurses-dev libx11-dev libreadline-dev libgl1 libgl1-mesa-dev \
python3 make sudo gcc g++ bc grep tofrodos python3-markdown libxml2-utils xsltproc zlib1g-dev python-is-python3 libc6-dev libtinfo6 \
make repo cpio kmod openssl libelf-dev pahole libssl-dev libarchive-tools zstd --fix-missing && wget http://security.ubuntu.com/ubuntu/pool/universe/n/ncurses/libtinfo5_6.3-2ubuntu0.1_amd64.deb && sudo dpkg -i libtinfo5_6.3-2ubuntu0.1_amd64.deb
```
<br>❗The video Guide for this tutorial can be found here : Open in <a href="https://t.me/SamsungTweaks/137">Telegram</a> </h3>
<br>

### Quick Links :
01. ✅ [Downloading Part (Only for samsung)](https://github.com/ravindu644/Android-Kernel-Tutorials?tab=readme-ov-file#--downloading-part-only-for-samsung)
02. ✅ [Understanding the Kernel root](https://github.com/ravindu644/Android-Kernel-Tutorials?tab=readme-ov-file#-understanding-the-kernel-root)
03. ✅ [Understanding non-GKI & GKI kernels](https://github.com/ravindu644/Android-Kernel-Tutorials?tab=readme-ov-file#-understanding-non-gki--gki-kernels)
04. ✅ [Compiling Part (Universal for any device).](https://github.com/ravindu644/Android-Kernel-Tutorials?tab=readme-ov-file#--compiling-part-universal-for-any-device)
05. ✅ [(❗ Samsung Specific) How to disable kernel securities from the menuconfig..?](https://github.com/ravindu644/Android-Kernel-Tutorials?tab=readme-ov-file#--samsung-specific-how-to-disable-kernel-securities-from-the-menuconfig)
06. ✅ [How to make your kernel supports with APatch..?](https://github.com/ravindu644/Android-Kernel-Tutorials?tab=readme-ov-file#-how-to-make-your-kernel-supports-with-apatch)
07. ✅ [Compilation Process.](https://github.com/ravindu644/Android-Kernel-Tutorials?tab=readme-ov-file#-compilation-process)
08. ✅ [(FINAL) How to put the compiled kernel, inside our boot.img..?](https://github.com/ravindu644/Android-Kernel-Tutorials?tab=readme-ov-file#-final-how-to-put-the-compiled-kernel-inside-our-bootimg)

<hr>
<h2> ✅ Downloading Part. (Only for samsung)</h2>

### 01. Download the kernel source from the [Samsung Opensource]( https://opensource.samsung.com/main).
<img src="./screenshots/1.png">

### 02. Extract the ```Kernel.tar.gz``` from the source zip, unarchive it using this command.
```
tar -xvf Kernel.tar.gz && rm Kernel.tar.gz
```

<img src="./screenshots/2.png">

**Note:** It's a good idea to give the entire kernel directory 755 permissions to remove those 🔒 from the files and folders, preventing any issues when editing files and upstreaming the kernel.

**Do it this way:**

```
chmod +755 -R /path/to/extracted/kernel/
```

**Before:**
<img src="./screenshots/3.png">

**After:**
<img src="./screenshots/4.png">

**The following video demonstrates all the steps mentioned above:** 

[🎥 Extracting Samsung's Kernel.tar.gz & granting required permissions](https://www.youtube.com/watch?v=QLymPkTpC2Y)

<hr>

- **⚠️ For other devices,** You can find them by your OEM's sites or from your OEM's **official** GitHub repos.

## ✅ Understanding ```non-GKI``` & ```GKI kernels```
```
+-------------------------------------+
|       Android Kernel Versions       |
+-------------------------------------+
|       non-GKI       |      GKI      |
+---------------------+---------------+
|         3.10        |      5.4      |
|         3.18        |      5.10     |
|         4.4         |      5.15     |
|         4.9         |      6.1      |
|         4.14        |               |
|         4.19        |               |
+---------------------+---------------+
```

## ✅ Understanding the ```Kernel root```

<img src="./screenshots/6.png">

- As you can see in the above screenshot, it's the Linux kernel source code.
- It must have those folders, **highlighted in blue in the terminal.**
- **In GKI kernels,** the kernel root is located in a folder named "common".

- If you have a **GKI Samsung kernel**, you should use the "common" kernel instead of "msm-kernel" for building your kernel, and the build steps are the same as for a non-GKI kernel. [Refer to this repo for an idea](https://github.com/ravindu644/android_kernel_m145f_common).

<h2> ✅ Compiling Part (Universal for any device).</h2>

### 01. After downloading or cloning the Kernel Source, we must have a build script to compile our kernel.

- Before creating a build script, we must determine the compatible compilers we will use to build our kernel.

- Run ```make kernelversion``` inside the kernel root to check your kernel version.

<img src="./screenshots/5.png">

- In my case, the kernel version is **5.4,** which is [GKI](https://github.com/ravindu644/Android-Kernel-Tutorials?tab=readme-ov-file#-understanding-non-gki--gki-kernels).

- You can find full information about **choosing the correct compiler for your kernel version** [here](./toolchains/) (based on my experience, btw).

  - **❗Mediatek users,** read [this](https://github.com/ravindu644/Android-Kernel-Tutorials/tree/main/toolchains#additional-notes).

- Next, go to [build_scripts](./build_scripts/), choose the appropriate script, download it, and place it inside your kernel's root directory.

<img src="./screenshots/7.png">

- Keep in mind that **you don't need to manually download any of these** since my build scripts handle everything for you :)

- **❗If your device is Samsung Exynos, it doesn't support compiling the kernel in a separated 'out' directory. So, [edit your build script like this](./patches/001.nuke_out.patch)**

<hr>

### Notes :
- Replace `your_defconfig` to your current defconfig which is located in `arch/arm64/configs` In GKI kernels, it's normally `gki_defconfig`

- But just in case, make sure to check `arch/arm64/configs` or `arch/arm64/configs/vendor`

- If your defconfig is located in the `arch/arm64/configs` directory, just replace `your_defconfig` with the name of your defconfig.

- If your defconfig is located in the `arch/arm64/configs/vendor` directory, replace `your_defconfig` like this:
  
  - `vendor/name_of_the_defconfig`
  - Example patch: [here](./patches/005.edit-defconfig.patch)


### 02. Edit the Makefile.

- If you find these variables: ```REAL_CC``` or ```CFP_CC``` in your "Makefile", remove them from the "Makefile", then Search for "wrapper" in your Makefile. If there's a line related to a Python file, remove that entire line/function as well.

    - Example patch of removing the wrapper: [click here](./patches/004.remove_gcc%20wrapper.patch)

- Search ```CONFIG_CC_STACKPROTECTOR_STRONG``` and replace it with ```CONFIG_CC_STACKPROTECTOR_NONE```

    - Example patch of fix -fstack-protector-strong not supported by compiler: [click here](./patches/003.fix_fstack-protector-strong-not-supported-by-compiler.patch)

<hr>

### 03. Now, grant the executable permissions to ```build_xxxx.sh``` using this command.
  ```
  chmod +x build_xxxx.sh
  ```
### 04. Finally, run the build script using this command :
  ```
./build_xxxx.sh
```

<img src="./screenshots/8.png">

- When you run the script for the first time, it will begin to install all the necessary dependencies and start downloading the required toolchains, depending on your kernel version.

- Make sure not to interrupt the first run. If it gets interrupted somehow, delete the `toolchains` folder from "~/" and try again: ```rm -rf ~/toolchains```

<img src="./screenshots/9.png">

## After the initial run is completed, the kernel should start building, and the "menuconfig" should appear.

<img src="./screenshots/10.png">

- Additional notes : Press space bar to enable/disable or enable as a module <M>.
<hr>

## 👇👇👇 outdated guide from here \ 2025.03.17 

## ✅ (❗ Samsung Specific) How to disable kernel securities from the menuconfig..?
<hr>

### 01. ```→ Kernel Features``` => Disable "```Enable RKP (Realtime Kernel Protection) UH feature```", "```Enable LKM authentication by micro hypervisor```", "```Block LKM by micro hypervisor```", "```Enable micro hypervisor feature of Samsung```" respectively.
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/d821da9f-0b45-4701-b681-3996bec509be" width="75%">

### 02. ```→ Kernel Features → Control Flow Protection``` => Disable "```JOP Prevention```", "```ROP Prevention```" and "```JOPP and ROPP```" Respectively.
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/58e0680a-8052-4df8-aba1-cb282d6963ee" width="75%">

### Additional notes : 
- If you can't find them (01 and 02) in the "```→ Kernel Features```", they are located in the "```→ Boot options```".
<hr>

## ❗In Android 14 and some Android 13 sources, they are located in ```→ Hypervisor```. Disable them ALL!
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/eb67e7fd-46ff-4aa7-a424-73e22f1d29da" width="75%">
- As I mentioned at the beginning of this guide, your must use your brain..! 🧠

<hr>

### 03. ```→ Security options``` => Disable "```Integrity subsystem```" and "```Defex Support```".
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/ca396e53-26fc-4ee4-99ea-c8359926ea51" width="75%">
<hr>

## ✅ How to make your kernel supports with APatch..?
<hr>

### 01. Open ```→ General setup → Local version - append to kernel release``` => Choose any string you like.
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/448a24b9-454b-47b9-82a8-0b9c2804e693">
### 02. Open ```→ Kernel hacking``` => Turn on the ```Kernel debugging``` ❗.
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/54eaf94a-3d7e-44ca-81aa-093b76ed9893">
### 03. ```→ General setup → Configure standard kernel features (expert users)``` => Enable everything except "```sgetmask/ssetmask syscalls support``` and ```Sysctl syscall support```"
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/8927d898-d3ef-471a-8f68-bbe418068565" width="75%">
### 04. ```→ Enable loadable module support``` => Enable "```Forced module loading```", "```Module unloading```", "```Forced module unloading```", "```Module versioning support```" and disable others.
- Image : <br><br><img src="https://github.com/ravindu644/Android-Kernel-Tutorials/assets/126038496/865ddeca-88de-484d-8f99-922b439e0e7c" width="75%"><br><br>
**❗Additional Notes :** To force load the vendor/system modules in some devices, use this commit (⚠️ This fixes various hardware related issues after installing a custom kernel) - [Click here](https://github.com/rama982/kernel_common/commit/d5e8b83f0c5eee4e2f8c6d9888cc56d03f2615fb)

### 05. ```→ Boot options``` => enable "```Build a concatenated Image.gz/dtb by default```" and "```Kernel compression method (Build compressed kernel image)```"  ---> "```(X) Build compressed kernel image```"
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/3c7704a7-ea16-4bee-a0bf-6ecd0424f2b7" width="75%">
### 06. ```→ File systems``` => Enable "```<*> Overlay filesystem support```".
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/0cbff894-ba4c-4f51-a1bd-3ffa1963cd51" width="75%">
<hr>

## ✅ Compilation Process.
<hr>

### 07. Exit and Save the config.
- When you see "```configuration written```", stop the compilation process with ```Ctrl+C``` and replace the content of ".config" with your desired defconfig.
### 08. Compile using ```./build.sh``` --> Skip the menuconfig and wait until the compilation finishes..!
- ℹ️ **The compiled kernel** will be located at out/arch/arm64/boot.
<hr>

#### Notes:

- If you encounter errors during the compilation process, it's advisable to search for these errors on GitHub and find a solution.
  
---

## ✅ (FINAL) How to put the compiled kernel, inside our boot.img..?
<hr>

### 01. Extract the boot.img from the stock ROM/ROM ZIP.  If you are a Samsung user, I prefer https://github.com/ravindu644/Scamsung to do this online.
	- Use exact build number to download the firmware.
### 02. Unpack the boot.img using AIK-Linux which can be found in here : https://github.com/ravindu644/AIK-Linux
- Image : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/d5fee81a-6768-4848-a4a6-37fec6cb355f" width="70%"><hr>
## How to check "Which kernel format should I use"..?
- Kernel without GZIP compression : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/cb1d0ff3-32cb-4d98-9892-5a00d1922680" width="70%">
- Kernel <b>with</b> GZIP compression : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/30ee541e-211c-4a84-971b-f67299ee8793" width="70%"><br><br>
# Notes :
- If your split_img has a boot.img-dtb + Uncompressed Kernel => Use "Image".
- If your split_img has a boot.img-dtb + GZIP compressed Kernel => Use "Image.gz".
- If your split_img don't has a boot.img-dtb + uncompressed Kernel => Use "Image-dtb". (if your out/arm64/boot folder don't have such a file, use Image instead)
- If your split_img don't has a boot.img-dtb + GZIP compressed Kernel => Use "Image.gz-dtb".
<hr>

### 03. Choose the required kernel as I mentioned above > Rename it to "```boot.img-kernel```" and copy and replace it with the ```boot.img-kernel```, which is in the split_img folder.
### 04. Repack --> rename "image-new.img" to "boot.img" and make a tar file using this command :
```
tar cvf "DEVICE NAME (APatch Support).tar" boot.img
```
### 05. Flash it using Fastboot/ODIN..!
### 06. DONE..!
- Proof : <br><br><img src="https://github.com/ravindu644/APatch/assets/126038496/f0dd204d-e398-4ce1-9897-96e6a51b5673" width="75%">
<hr>

## Written by [@Ravindu_Deshan](https://t.me/Ravindu_Deshan) for [@SamsungTweaks](https://t.me/SamsungTweaks) and [@APatchChannel](https://t.me/APatchChannel) | Sharing this without proper credit is not allowed..❗
