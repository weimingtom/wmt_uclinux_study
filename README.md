# wmt_uclinux_study
My uclinux study

## (TODO) elinux's linux and cpio rootfs  
* STM32F469I-DISCO移植Linux4.13.12  
* https://www.likecs.com/show-205025138.html  
* https://github.com/mcoquelin-stm32/afboot-stm32  
* 下载引导程序afboot-stm32-master.zip  
* https://www.kernel.org/  
* 下载最新的Linux内核linux-4.13.12.tar.xz  
* https://elinux.org/File:Stm32_mini_rootfs.cpio.bz2   

## (TODO) Aboriginal Linux  
http://landley.net/aboriginal/screenshots/  

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

## (Not built, Tested) DZ51's S3C44B0X uclinux  
* search baidupan, ARM 44BOX.iso  
* search baidupan, DZ51_ARM使用手册.pdf  

## uc-PC 序言  
* https://hhuysqt.github.io/ucpc0/  

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
