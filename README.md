
# README #
* ИНСТРУКЦИЯ ПО СБОРКЕ ПРОШИВКИ *

1) Для сборки прошивки требуется Linux окружение 32 или 64 бита. Сборка прошивки
   протестирована и рекомендуется на Linux дистрибутивах Ubuntu 16.04.1 LTS и
   Debian 8.5.
2) Первым делом необходимо собрать кросс-toolchain (набор для кросс-компиляции)
   под MIPS32_R2 CPU, состоящий из пакетов binutils-2.24, gcc-447, uclibc-0.9.33.2.
   Перейти в директорию toolchain-mipsel и выполнить скрипт сборки. Сборка
   кросс-toolchain занимает от 10 до минут до нескольких часов, в зависимости 
   от типа CPU хоста. Если кросс-toolchain уже собран, этот пункт пропускается.
3) Отредактировать вручную файл ".config" в корне дерева. Для комментирования 
   строк используется символ #. Если требуется отключить параметр, не следует 
   менять y на n, необходимо закомментировать строку целиком. Изменить параметр 
   "CONFIG_TOOLCHAIN_DIR=" на актуальный путь до директории с кросс-toolchain.
4) Собрать прошивку, запустив скрипт "build_firmware". Сборка прошивки может 
   занимать от 20 минут до нескольких часов. После сборки файл образа прошивки 
   (*.trx) будет находиться в директории images.

Установка необходимых пакетов:
```shell
# Debian/Ubuntu
sudo apt update
sudo apt install unzip libtool-bin curl cmake gperf gawk flex bison nano xxd \
	fakeroot kmod cpio git python3-docutils gettext automake autopoint \
	texinfo build-essential help2man pkg-config zlib1g-dev libgmp3-dev \
	libmpc-dev libmpfr-dev libncurses5-dev libltdl-dev wget libc-dev-bin

# Archlinux/Manjaro
sudo pacman -Syu --needed git base-devel cmake gperf ncurses libmpc \
        gmp python-docutils vim rpcsvc-proto fakeroot cpio help2man

# Alpine
sudo apk add make gcc g++ cpio curl wget nano xxd kmod \
	pkgconfig rpcgen fakeroot ncurses bash patch \
	bsd-compat-headers python2 python3 zlib-dev \
	automake gettext gettext-dev autoconf bison \
	flex coreutils cmake git libtool gawk sudo

# CentOS 7
sudo yum update
sudo yum groupinstall "Development Tools"
sudo yum install ncurses-* flex byacc bison zlib-* texinfo gmp-* mpfr-* gettext \
	libtool* libmpc-* gettext-* python-docutils nano help2man fakeroot

# CentOS 8
sudo yum update
sudo yum groupinstall "Development Tools"
sudo yum install ncurses-* flex byacc bison zlib-* gmp-* mpfr-* gettext \
	libtool* libmpc-* gettext-* nano fakeroot
```
**Сборка прошивки**
Сборка займёт от получаса до нескольких часов в зависимости от мощности ПК. Собранные тулчейн и прошивка займут до 4ГБ дискового пространства.

**Компиляция тулчейна:**
```bash
cd /rt-n56u/toolchain-mipsel
sudo ./clean_toolchain
sudo ./build_toolchain
```

**Конфигурирование прошивки**
Файл конфигурации прошивки лежит в /rt-n56u/trunk/.config. Отредактируйте его по своему усмотрению или возьмите готовый шаблон из /rt-n56u/trunk/configs/templates/. В примере ниже для сборки берётся шаблон для сборки n11p_nano:

```bash
cd /rt-n56u/trunk
sudo cp configs/templates/n11p_nano.config .config
```

**Компиляция прошивки**
```bash
cd /rt-n56u/trunk
sudo ./clear_tree
sudo ./build_firmware
```
Если сборка пройдёт успешно, то образ собранной прошивки можно будет найти в папке /rt-n56u/trunk/images.

При обновлении исходных кодов прошивки в дереве сборки необходимо выполнить:

```bash
sudo git stash
sudo git pull
```
После чего сконфигурировать и скопмпилировать прошивку по новой.

https://github.com/gorden5566/padavan
https://www.jianshu.com/p/d76a63a12eae

***

- 使用[gorden5566](https://github.com/gorden5566/padavan)的汉化字典
- aria2前端更换为[AriaNg](https://github.com/mayswind/AriaNg)
- [curl](https://github.com/curl/curl)可选编译可执行程序 ```CONFIG_FIRMWARE_INCLUDE_CURL```
- 使用了[PROMETHEUS](http://pm.freize.net/index.html)提供的部分补丁
- 使用了[Linaro1985/padavan-ng](https://gitlab.com/padavan-ng/padavan-ng)的部分软件包
- 可选以下插件：
>- [scutclient](https://github.com/hanwckf/scutclient) ```CONFIG_FIRMWARE_INCLUDE_SCUTCLIENT```
>- [gdut-drcom](https://github.com/chenhaowen01/gdut-drcom) ```CONFIG_FIRMWARE_INCLUDE_GDUT_DRCOM```
>- [dogcom](https://github.com/hanwckf/dogcom) ```CONFIG_FIRMWARE_INCLUDE_DOGCOM```
>- [minieap](https://github.com/hanwckf/minieap) ```CONFIG_FIRMWARE_INCLUDE_MINIEAP```
>- [njit-client](https://github.com/hanwckf/njit8021xclient) ```CONFIG_FIRMWARE_INCLUDE_NJIT_CLIENT```
>- [napt66](https://github.com/mzweilin/napt66) ```CONFIG_FIRMWARE_INCLUDE_NAPT66```
>- [softether-vpnserver](https://github.com/SoftEtherVPN/SoftEtherVPN_Stable) ```CONFIG_FIRMWARE_INCLUDE_SOFTETHERVPN_SERVER```
>- [softether-vpnclient](https://github.com/SoftEtherVPN/SoftEtherVPN_Stable) ```CONFIG_FIRMWARE_INCLUDE_SOFTETHERVPN_CLIENT```
>- [softether-vpncmd](https://github.com/SoftEtherVPN/SoftEtherVPN_Stable) ```CONFIG_FIRMWARE_INCLUDE_SOFTETHERVPN_CMD```
>- [vlmcsd](https://github.com/hanwckf/vlmcsd) ```CONFIG_FIRMWARE_INCLUDE_VLMCSD```
>- [ttyd](https://github.com/tsl0922/ttyd) ```CONFIG_FIRMWARE_INCLUDE_TTYD```
>- [lrzsz](https://ohse.de/uwe/software/lrzsz.html) ```CONFIG_FIRMWARE_INCLUDE_LRZSZ```
>- [htop](https://hisham.hm/htop/releases/) ```CONFIG_FIRMWARE_INCLUDE_HTOP```
>- [nano](https://www.nano-editor.org/dist/) ```CONFIG_FIRMWARE_INCLUDE_NANO```
>- [iperf3](https://github.com/esnet/iperf) ```CONFIG_FIRMWARE_INCLUDE_IPERF3```
>- [dump1090](https://github.com/hanwckf/dump1090) ```CONFIG_FIRMWARE_INCLUDE_DUMP1090```
>- [rtl-sdr](https://github.com/osmocom/rtl-sdr) ```CONFIG_FIRMWARE_INCLUDE_RTL_SDR```
>- [samba3.6](https://gitlab.com/padavan-ng/padavan-ng/tree/master/trunk/user/samba36) ```CONFIG_FIRMWARE_INCLUDE_SMBD36```
>- [mtr](https://github.com/traviscross/mtr) ```CONFIG_FIRMWARE_INCLUDE_MTR```
>- [socat](http://www.dest-unreach.org/socat) ```CONFIG_FIRMWARE_INCLUDE_SOCAT```
>- [srelay](https://socks-relay.sourceforge.io) ```CONFIG_FIRMWARE_INCLUDE_SRELAY```
>- [3proxy](https://github.com/z3APA3A/3proxy) ```CONFIG_FIRMWARE_INCLUDE_3PROXY```
>- [mentohust](https://github.com/hanwckf/mentohust-1) ```CONFIG_FIRMWARE_INCLUDE_MENTOHUST```
>- [frpc](https://github.com/fatedier/frp) ```CONFIG_FIRMWARE_INCLUDE_FRPC```
>- [frps](https://github.com/fatedier/frp) ```CONFIG_FIRMWARE_INCLUDE_FRPS```
>- [tunsafe](https://github.com/TunSafe/TunSafe) ```CONFIG_FIRMWARE_INCLUDE_TUNSAFE```
>- [wireguard-go](https://git.zx2c4.com/wireguard-go/) ```CONFIG_FIRMWARE_INCLUDE_WIREGUARD```
>- [smartdns](https://github.com/pymumu/smartdns) ```CONFIG_FIRMWARE_INCLUDE_SMARTDNS```


>- PSG1208
>- PSG1218
>- 5K-W20 (USB)
>- OYE-001 (USB)
>- NEWIFI-MINI (USB)
>- MI-MINI (USB)
>- MI-3 (USB)
>- MI-3C
>- MI-4
>- MI-R3G (USB)
>- MI-R4A
>- MI-R3P (USB)
>- HC5661A
>- HC5761A (USB)
>- HC5861B
>- 360P2 (USB)
>- MI-NANO
>- MZ-R13
>- MZ-R13P
>- RT-AC1200GU (USB)
>- XY-C1 (USB)
>- WR1200JS (USB)
>- NEWIFI3 (USB)
>- B70 (USB)
>- A3004NS (USB)
>- K2P
>- K2P-USB (USB)
>- JCG-836PRO (USB)
>- JCG-AC860M (USB)
>- DIR-882 (USB)
>- DIR-878
>- MR2600 (USB)
>- WDR7300
>- RM2100
>- CR660x (CR6606, CR6608, CR6609)
>- R2100
>- JCG-Y2 (USB)
>- E8820V2 (USB)
>- ZTE_E8820S (USB)
>- MSG1500 (USB)
>- R6220 (USB)
>- NETGEAR-CHJ (R6260, R6350, R6850, WAC124)
>- NETGEAR-BZV (R6800, R6700-v2, R7200, Nighthawk AC2400)





- https://www.jianshu.com/p/cb51fb0fb2ac
- https://www.jianshu.com/p/6b8403cdea46

