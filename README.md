# RTL8821CU_RTL8811CU_driver
RTL8811CU driver

The Realtek RTL8811CU-CG is a highly integrated single-chip that supports 1-stream 802.11ac solutions with Multi-user MIMO (Multiple-Input, Multiple-Output) and Wireless LAN (WLAN) USB interface controller. It combines a WLAN MAC, a 1T1R capable WLAN baseband, and RF in a single chip. The RTL8811CU-CG provides an outstanding solution for a high-performance integrated wireless device.:

    USB high speed interface
    802.11ac/abgn, 802.11ac
    2.4 GHz Support
    5.8 GHz Support
    Supports concurrent mode (operates as two virtual WLAN interfaces)
    MIMO config - 1x1
    MU-MIMO
    AC wave2
    256 QAM


Drivers for rtl8811CU and rtl8821CU Wi-Fi chipsets. This repository is based on soruce code found on a CD shipped with a rtl8811CU based card. It's updated to build on newer kernel versions.


## Linux driver support
Linux uses 8821cu.ko for driver. Check if the USB sub-system recognizes the device.

    lsusb | grep Realtek
    Bus 001 Device 003: ID 0bda:c811 Realtek Semiconductor Corp.
    
    lsmod | grep 8821cu
    8821cu               2260992  0
    cfg80211              589824  1 8821cu
    usbcore               253952  6 ehci_hcd,xhci_pci,btusb,8821cu,xhci_hcd,ehci_pci
If 8821cu is present, everything is good to go!


## Install build tools

    # Install build tools
    sudo apt-get install build-essential -y
    sudo apt-get install bc -y
    sudo apt-get install unzip git -y

    # install kernel headers
    sudo apt-get install linux-headers-$(uname -r)

    # check
    apt search linux-headers-$(uname -r)
    ls -l /usr/src/linux-headers-$(uname -r)


    git clone https://github.com/Logi2c/rtl8821CU.git
    cd rtl8821CU


## Build and install with DKMS

DKMS is a system which will automatically recompile and install a kernel module when a new kernel gets installed or updated. To make use of DKMS, install the dkms package, which on Debian (based) systems is done like this:

    apt-get install dkms

To make use of the DKMS feature with this project, do the following:

    DRV_NAME=rtl8821CU
    DRV_VERSION=5.2.5.3
    sudo mkdir /usr/src/${DRV_NAME}-${DRV_VERSION}
    git archive master | sudo tar -x -C /usr/src/${DRV_NAME}-${DRV_VERSION}
    sudo dkms add -m ${DRV_NAME} -v ${DRV_VERSION}
    sudo dkms build -m ${DRV_NAME} -v ${DRV_VERSION}
    sudo dkms install -m ${DRV_NAME} -v ${DRV_VERSION}

If you later on want to remove it again, do the following:

    DRV_NAME=rtl8821CU
    DRV_VERSION=5.2.5.3
    sudo dkms remove ${DRV_NAME}/${DRV_VERSION} --all

## Build and install without DKMS
Use following commands in source directory:
```
make
sudo make install
sudo modprobe 8821cu
```
## Raspberry Pi
To build this driver on Raspberry Pi you need to set correct platform in Makefile.
Change
```
CONFIG_PLATFORM_I386_PC = y
CONFIG_PLATFORM_ARM_RPI = n
CONFIG_PLATFORM_ARM_RPI3 = n
```
to
```
CONFIG_PLATFORM_I386_PC = n
CONFIG_PLATFORM_ARM_RPI = y
CONFIG_PLATFORM_ARM_RPI3 = n
```
For the Raspberry Pi 3 you need to change it to
```
CONFIG_PLATFORM_I386_PC = n
CONFIG_PLATFORM_ARM_RPI = n
CONFIG_PLATFORM_ARM_RPI3 = y
```
