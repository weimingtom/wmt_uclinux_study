# wmt_uclinux_study
My uclinux study

## (TODO) s3c44b0x, some document  
* https://github.com/darkspr1te/s3c44b0x  
* https://gitee.com/weimingtom2000/s3c44b0x  

## h8 uclinux, h8max, H8/3069F    
* https://strawberry-linux.com/h8max/  

## sh-2, superh, J2 open processor, j-core, run linux  
* https://j-core.org/#get_hardware  

## how to unzip uclinux cross-compiler .sh file    
* 我知道怎么解压uclinux的交叉工具链sh文件了——其实只要用notepad++删掉开头那部分文本即可，然后重命名为.tar.gz，然后就可以用压缩软件打开  
* 网上的解决方法是  
将第39行的代码：  
tail +${SKIP} ${SCRIPT} | gunzip | tar xvf -  
改成如下：  
tail -n +${SKIP} ${SCRIPT} | gunzip | tar xvf -  
* see https://github.com/darkspr1te/s3c44b0x  

## jaty68k, Java emulator  
* m68k emulator repurposed to run Katy68 PCB linux for debugging and testing    
* https://github.com/alexwinston/Jaty68k  
* https://github.com/tonyheadford/m68k  
* http://www.bigmessowires.com/68-katy/  

## WL-530g  
https://github.com/xurubin/wl530g-mini  
https://github.com/Broadcom/stblinux-2.6.37  
https://github.com/MVoz/mipsonqemu  

## (TODO) proteus port    
* 基于ARM7的ucLinux内核移植的proteus仿真  
* https://blog.csdn.net/qq_33782655/article/details/79396891  

## (TODO) LPC2210, LPC2200    
see skyeye-1.2.6_rc1/misc/uClinux/uClinux-dist-20040408-lpc.diff  
see skyeye-1.3.5_rc1/...  

## (TODO) change cross compiler for uclinux  

## (TODO) uclinux下stm32开发环境搭建  
* http://www.eepw.com.cn/article/201808/388127.htm  

## (TODO) https://github.com/i25ffz/uclinux-stm32f4  

## (TODO) main line linux  

## (TODO) Aboriginal Linux  
http://landley.net/aboriginal/screenshots/  

## (Built, Tested) skyeye 1.2.5, uclinux 0404, for samsung s3c4510b  
* search uc_20040408_4510b_patch_v3.tar.gz  
* build about 2 minutes  
* 我整理出编译三星4510b的uclinux 0404 skyeye固件的方法，全程大概需要2分钟左右，  
对比起buildroot快得多（buildroot大概最快也要十分钟左右，甚至可能要一两个小时），  
下一步看能不能合并到44b0x的代码中去（因为我手头上的开发板SDK似乎也是基于uclinux 0404版改的）。  
另外补充一点，我本来打算用一个简单的make编译，后来发现不行，只能用shell脚本编译，  
依次执行多个make命令，否则编译出来的固件会跑不动。另外我这种方法参考网上的做法，  
但代价是需要编译两次镜像，因为romfs需要在linux之后编译（不知道为什么），  
但linux需要在rootfs.o生成之后编译，所以要编译两次内核，第一次要用一个临时的rootfs.o代替  
* gcc: ubuntu 140432  
$ sudo tar xzf arm-elf-compiler.tar.gz -C /usr/local  
$ rm -rf /usr/local/include  
(test ar) $ arm-elf-ar  
* run: skyeye -e linux-2.4.x/linux  
* patch ref:  
如何编译一个可以运行的 uClinux Kernel  
https://blog.csdn.net/mybirdsky/article/details/2058769  
ubuntu下编译uclinux skyeye上运行  
https://blog.csdn.net/paoxungan5156/article/details/72667924  
* skyeye.conf, from skyeye-testsuite-1.2.5.tar.bz2, see sourceforge    
```
#skyeye config file sample
cpu: arm7tdmi

mach: s3c4510b 


mem_bank: map=M, type=RW, addr=0x00000000, size=0x00800000
mem_bank: map=M, type=RW, addr=0x04000000, size=0x00800000
mem_bank: map=M, type=R,  addr=0x01000000, size=0x00200000
mem_bank: map=I, type=RW, addr=0x03ff0000, size=0x00010000
net: type=s3c4510b, hostip=10.0.0.2, ethmod=tuntap
#dbct:state=on
```

## (Built, Tested) main line buildroot  
* 我用ubuntu 14编译较新版本（2022.02）的buildroot的STM32F429固件，  
* 可以编译和正常运行（需要改一下elf2flt的patch，把bool改成int，  
* 把true改成1，把false改成0）。具体我就不截图了，  
* 最终编译出来的linux版本是5.15.0。说明编译较新的buildroot也可以  
* ubuntu 140432  
* buildroot-2022.02.7.tar.gz  
* search buildroot-2022.02.7_after_burn.bin  
* usart: st-link v2 on board  

## (Built, Tested) elinux's linux build, only linux-4.14.0 (other version failed)      
* STM32F429I-DISCO ucLinux 开发环境搭建  
* https://blog.csdn.net/u010444107/article/details/78580947  
* Stm32f429移植linux4.13.12  
* https://blog.csdn.net/u011371090/article/details/81587170  
* images002_v5_after_burn.bin  
* images002_v5_success_use_4.14.0.rar  
* init没有执行权限失败, 以及没有用sudo解压cpio  
* (**NOTE**) gcc: gcc-arm-none-eabi-4_9-2014q4  
https://launchpad.net/gcc-arm-embedded/4.9/4.9-2014-q4-major  
* ubuntu: ubuntu 140432  
* (**NOTE**) must use sudo to extract cpio     
mkdir Stm32_mini_rootfs  
cd Stm32_mini_rootfs  
sudo cpio -idmv < ../Stm32_mini_rootfs.cpio  
* (**NOTE**) MUST use linux-4.14.tar.xz  
or if use linux-4.14.299.tar.xz, will see kernel panic: init Not tainted  
* (**NOTE**) create file under Stm32_mini_rootfs: /init, and sudo chmod 755 init    
```
#!/bin/sh
# devtmpfs does not get automounted for initramfs
/bin/mount -t devtmpfs devtmpfs /dev
exec 0</dev/console
exec 1>/dev/console
exec 2>/dev/console
exec /sbin/init $*
```
* .config  
```
CONFIG_INITRAMFS_SOURCE="/home/wmt/work_uc5/Stm32_mini_rootfs"
CONFIG_INITRAMFS_ROOT_UID=0
CONFIG_INITRAMFS_ROOT_GID=0
CONFIG_INITRAMFS_COMPRESSION=".gz"
```
* build (if change rootfs only, just execute make all)    
```
export PATH=~/uclinux/stm32/gcc-arm-none-eabi-4_9-2014q4/bin:$PATH
make ARCH=arm CROSS_COMPILE=arm-none-eabi- stm32_defconfig
make ARCH=arm CROSS_COMPILE=arm-none-eabi-
```

## (OLD) elinux's linux and cpio rootfs  
* STM32F469I-DISCO移植Linux4.13.12  
* https://www.likecs.com/show-205025138.html  
* https://github.com/mcoquelin-stm32/afboot-stm32  
* 下载引导程序afboot-stm32-master.zip  
* https://www.kernel.org/  
* 下载最新的Linux内核linux-4.13.12.tar.xz  
* https://elinux.org/File:Stm32_mini_rootfs.cpio.bz2   

## (Built, Tested) uclinux skyeye build  
* uclinux_04_skyeye_1.2.5_build_output.tar.gz  
* uclinux的预编译工具链有什么问题呢？它所提供的是一种sh安装包，  
但ubuntu不支持（可能只支持redhat），这个预编译工具链arm-elf-gcc，  
不可以像现在的交叉工具链那样绿色安装，只能安装在指定位置，/usr/local，  
其实就是类似于编译安装（因为编译安装也是固定输出路径），所以这产生两个问题，  
一个是怎么自己编译，二是怎么替换成现在的arm交叉工具链（现在的可以随意复制目录）  
* 我试过好像可以成功在ubuntu 14下编译uclinux 04旧版本，使用预编译的arm工具链，    
并且在skyeye模拟器下运行。有时间我把输出的命令行内容展示出来。不过这里还有很多复杂的问题，  
例如怎么编译工具链，怎么替换工具链，怎么组合不同的linux版本和libc版本。  
uclinux的旧版本和预编译工具链我是花了很长时间收集的（来源于存档和一些旧书的光盘），  
编译方法和运行方法也需要查阅很多资料，uclinux似乎不提供相关的文档  
* gcc: arm-elf-compiler.tar.gz, from 嵌入式系统开发实验与实践_实验与实践.iso  
* uclinux: uClinux-dist-20041215.tar.gz  
* build:  
ubuntu 140432  
$ sudo tar xzf arm-elf-compiler.tar.gz -C /usr/local/  
$ rm -rf /usr/local/include  
(test ar) $ arm-elf-ar  
tar xzf uClinux-dist-20041215.tar.gz  
cd uClinux-dist  
make xconfig # choose gdb/skyeye and linux-2.4  
make dep  
make  
sudo apt install skyeye  
skyeye -h  
SkyEye 1.2.5  
copy file skyeye.conf  
skyeye -e linux-2.4.x/linux  
```
# skyeye -e linux-2.4.x/linux

cpu:arm7tdmi
mach:at91
mem_bank:map=M, type=RW, addr=0x00000000, size=0x00004000
mem_bank:map=M, type=RW, addr=0x01000000, size=0x00400000
mem_bank:map=M, type=R, addr=0x01400000 , size=0x00400000, file=images/romfs.img
mem_bank:map=M, type=RW, addr=0x02000000, size=0x00400000
mem_bank:map=M, type=RW, addr=0x02400000, size=0x00008000
mem_bank:map=M, type=RW, addr=0x04000000, size=0x00400000
mem_bank:map=I, type=RW, addr=0xf0000000, size=0x10000000
```
* ref: uClinux-dist-20040408 skyeye 仿真  
https://zhuanlan.zhihu.com/p/27045296  

## (Built, Tested) STM32F429I-DISC1 uclinux    
* http://github.com/jserv/stm32f429-linux-builder  
* https://www.freesion.com/article/7452512540/  
* search baidupan, uclinux_test_ok_0x08000000_size_0x200000.bin  
* search baidupan, stm32f429-linux-builder-master_ubuntu_140432_20221109_uclinux_stm32.tar.gz  
* uart: 115200-8-1-none-none  
stm32<->uart converter  
GND<->GND(black)  
PC11<->TXD(orange)  
PC10<->RXD(yellow)  
* gcc: arm-2010q1-189-arm-uclinuxeabi-i686-pc-linux-gnu.tar.bz2  
* busybox: busybox-1.22.1.tar.bz2  

## STM3210E-EVAL uclinux  
* https://www.st.com/content/st_com/en/products/embedded-software/mcu-mpu-embedded-software/stm32-embedded-software/stm32-standard-peripheral-library-expansion/stsw-stm32024.html  
* search baidupan, uClinux-dist-20080808-20090112.patch.gz  
* https://web.archive.org/web/20120501030746/http://www.uclinux.org/pub/uClinux/dist/patches/  
* https://community.st.com/s/question/0D53W000005q7duSAA/uclinux-patch-for-stm32f10x-devices  

## (Not built, Tested ok only on board, but failed with skyeye) DZ51's S3C44B0X uclinux  
* search baidupan, ARM 44BOX.iso  
* search baidupan, DZ51_ARM使用手册.pdf  

## uc-PC 序言  
* https://hhuysqt.github.io/ucpc0/  
* https://hhuysqt.github.io/tags/  

## (Built, Tested) STM32F429I-disco_Buildroot, need mod linux and busybox source see issues    
* https://github.com/fdu/STM32F429I-disco_Buildroot  
* https://elinux.org/STM32#STM32F429i-Discovery  
* https://github.com/fdu/STM32F429I-disco_Buildroot/issues/1  
* search baidupan, stm32f429i_disc1_buildroot_step2_after_burn.bin  
* search baidupan, STM32F429I-disco_Buildroot_v3_success_step2.tar.gz  
* uart: 控制台串口在COM34，板载st-link的虚拟串口, 115200-8-1-none-none (aka. STM32F429 USART1, PA9-TX, PA10-RX)    
* see https://os.mbed.com/platforms/ST-Discovery-F429ZI/  
* busybox: 1.27.2  
* linux: 4.15.7  
* buildroot: 2018.02  

## STM32F429I-DISC1 factory rom  
* factory_0x08000000.bin  

## (NOT GOOD) 飞凌, ok1052c, u-boot and kernel are closed source     

## emcraft MIMXRT1050-EVK uclinux    
* RT1050(地上最强Cortex-M7) uclinux初体验  
* https://blog.csdn.net/u010444107/article/details/78548358  
* https://emcraft.com/products/819#demo  

## (TODO) emcraft, stm32f429ig上的ucLinux修改外部晶振频率  
* https://blog.csdn.net/tianyake_1/article/details/84633562  
* 将stm32f429i-discovery开发板的 emcraft uclinux 例程移植到秉火stm32f429的开发板上，由于stm32f429i-discovery上使用的是8MHZ外部晶振，而秉火stm32f429的开发板使用的是25MHZ的晶振。  
* search baidupan, stm32f429i-discovery uclinux BSP  

## k210-linux-nommu  
* https://github.com/vowstar/k210-linux-nommu  
* https://www.cnx-software.com/2020/02/17/how-to-build-run-linux-on-kendryte-k210-risc-v-nommu-processor/  

## uCLinux启动日志, NXP IMXRT1050 board  
* https://www.jianshu.com/p/0258d037b026  

## https://github.com/jdkoftinoff/mb-linux-msli  

## mount romfs  
```
ls /mnt  
sudo mount <romfs.img> /mnt -o loop  
sudo umount /mnt  
```
* https://blog.csdn.net/enthan809882/article/details/113172192  
* https://blog.csdn.net/chengwenyang/article/details/71841862  

## search arm-2010q1-189-arm-uclinuxeabi-i686-pc-linux-gnu.tar.bz2  
* https://emcraft.com/products/343  
* https://emcraft.com/stm32f429discovery/installing-linux-images-to-internal-flash  

## lpc1788 uclinux, EA LPC1788 board    
* https://github.com/hujiafu/lpc1788_uclinux  
* EA LPC1788 Evaluation Board  
* https://www.nxp.com/products/processors-and-microcontrollers/arm-microcontrollers/general-purpose-mcus/lpc1700-arm-cortex-m3/ea-lpc1788-evaluation-board:OM13001  
* https://www.nxp.com.cn/products/processors-and-microcontrollers/arm-microcontrollers/general-purpose-mcus/lpc1700-arm-cortex-m3/ea-lpc1788-evaluation-board:OM13001  
* 128 MB NAND FLASH + 512 kB internal  
* 32 MB SDRAM + 96 kB internal  

## linux-emcraft for stm32f746, framebuffer LCD driver      
* https://github.com/angmouzakitis/linux-stm32f7/blob/master/arch/arm/configs/stm32f746_disco_defconfig  
* https://elinux.org/STM32  

## Dragonball, MC68EZ328  
* https://kanpapa.com/today/2021/03/uclinux-build-ubuntu20-04-wsl.html  
* https://github.com/kanpapa/MC68EZ328  
* https://github.com/kanpapa/uClinux-dist-20020220  
* https://elinux.org/images/a/a1/M68ez328-user-man.pdf  
* https://www.nxp.com/files-static/32bit/doc/prod_brief/MC68EZ328.pdf  
* http://www.mediumware.net/DragonOne/DragonOne.htm  

## LPC2200 (actually LPC2210) uclinux port  
* search baidupan, SmartARM2200光盘 (.rar)      
* SmartARM2200v1.4.iso  

## 源码开放的嵌入式系统软件分析与实践:基于SkyEye和ARM开发平台  
* search 源码开放的嵌入式系统软件分析与实践  
* P.130, P.239  

## NUC710, NUC745, W90P710    
* search NUC7X_CD.iso, uClinux-Arm7.tar.gz  
* http://www.metavert.com/public/    
* http://www.computersolutions.cn/blog/zh/2010/04/ip-cam-hacking-pt5/  

## 中科大  
* http://staff.ustc.edu.cn/~xlanchen/resources.html  

## snapgear  
* http://www.snapgear.org/index.html  

## ubuntu8.04上搭建uClinux编译开发环境并用skyeye仿真  
* hello    
* https://blog.csdn.net/tony821224/article/details/4988275  
```
//arm-elf-gcc -Wl,-elf2flt -o Hello Hello.c
#include<stdio.h>
int main()
{
    printf("Hello!This is Embedded Linux!");
    return 0;
}
```

