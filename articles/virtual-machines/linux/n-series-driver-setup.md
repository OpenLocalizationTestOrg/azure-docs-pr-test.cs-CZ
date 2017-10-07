---
title: "instalace ovladačů aaaAzure N-series pro Linux | Microsoft Docs"
description: "Jak tooset až NVIDIA GPU ovladače pro N-series virtuální počítače s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7db1b3859f9075c6d9f0319f39418946ea08743f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>Instalace ovladačů NVIDIA GPU v N-series virtuální počítače se systémem Linux

tootake využívat funkce GPU hello Azure N-series virtuální počítače se systémem Linux, instalace podporované NVIDIA grafické ovladače. Tento článek obsahuje kroky instalace ovladačů po nasadit virtuální počítač s N-series. Informace o instalaci ovladačů je také k dispozici pro [virtuálních počítačů Windows](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


Virtuální počítač N-series specifikace, kapacity úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačů Linux GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a>Instalace ovladačů mřížky pro virtuální počítače vs

ovladače NVIDIA mřížky tooinstall na virtuálních počítačích vs, ujistěte se, tooeach připojení SSH virtuálního počítače a postupujte podle kroků hello pro distribuční Linux. 

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Spustit hello `lspci` příkaz. Ověřte, zda karta hello NVIDIA M60 nebo karty jsou viditelné jako PCI zařízení.

2. Nainstalujte aktualizace.

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. Zakážete hello Nouveau jádra ovladač, který není kompatibilní s hello NVIDIA ovladačů. (Použijte pouze hello NVIDIA ovladač na virtuálních počítačích vs.) toodo, vytvořte soubor v `/etc/modprobe.d `s názvem `nouveau.conf` s hello následující obsah:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. Restartujte hello virtuálních počítačů a znovu připojit. Ukončení X server:

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. Stáhněte a nainstalujte ovladač mřížky hello:

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 

6. Jakmile se zobrazí výzva, jestli chcete toorun hello nvidia xconfig nástroj tooupdate X konfiguračním souboru, vyberte **Ano**.

7. Po dokončení instalace, zkopírujte /etc/nvidia/gridd.conf.template tooa gridd.conf na nový soubor v umístění/etc/nvidia /

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. Přidejte následující hello příliš`/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Restartujte hello virtuálních počítačů a tooverify hello instalace pokračovat.


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>Na základě centOS 7.3 nebo Red Hat Enterprise Linux 7.3


1. Aktualizace hello jádra a DKMS.
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. Zakážete hello Nouveau jádra ovladač, který není kompatibilní s hello NVIDIA ovladačů. (Použijte pouze hello NVIDIA ovladač na virtuálních počítačích vs.) toodo, vytvořte soubor v `/etc/modprobe.d `s názvem `nouveau.conf` s hello následující obsah:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. Hello virtuální počítač restartovat, znovu připojit a nainstalovat nejnovější integrační služby Linuxu pro Hyper-V: hello
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
 
  tar xvzf lis-rpms-4.2.2-2.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
4. Připojte se znovu toohello virtuálního počítače a spustit hello `lspci` příkaz. Ověřte, zda karta hello NVIDIA M60 nebo karty jsou viditelné jako PCI zařízení.
 
5. Stáhněte a nainstalujte ovladač mřížky hello:

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 
6. Jakmile se zobrazí výzva, jestli chcete toorun hello nvidia xconfig nástroj tooupdate X konfiguračním souboru, vyberte **Ano**.

7. Po dokončení instalace, zkopírujte /etc/nvidia/gridd.conf.template tooa gridd.conf na nový soubor v umístění/etc/nvidia /
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. Přidejte následující hello příliš`/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Restartujte hello virtuálních počítačů a tooverify hello instalace pokračovat.

### <a name="verify-driver-installation"></a>Ověření instalace ovladačů


tooquery hello GPU stavu zařízení, SSH toohello virtuální počítač a spusťte hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) s ovladačem hello nainstalovaný nástroj příkazového řádku. 

Zobrazí se výstup podobný toohello následující:

![Stav zařízení NVIDIA](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11 serveru
Pokud budete potřebovat X11 server pro vzdálená připojení tooan vs virtuálních počítačů, [x11vnc](http://www.karlrunge.com/x11vnc/) se doporučuje, protože umožňuje hardwarovou akceleraci grafiky. Hello BusID hello M60 zařízení musí ručně přidat soubor xconfig toohello (`etc/X11/xorg.conf` na Ubuntu 16.04 LTS, `/etc/X11/XF86config` na CentOS 7.3 nebo Red Hat Enterprise Server 7.3). Přidat `"Device"` podobné toohello následující části:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
Kromě toho aktualizovat vaše `"Screen"` části toouse toto zařízení.
 
Hello BusID můžete najít spuštěním

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
Hello BusID lze změnit, pokud virtuální počítač získá znovu přidělit, nebo restartovat. Proto můžete chtít toouse hello tooupdate skriptu BusID v konfiguraci hello X11 při po restartu virtuálního počítače. Například:

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed too${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

Tento soubor nelze vyvolat jako kořenová na spouštěcí tak, že vytvoříte položku pro něj v `/etc/rc.d/rc3.d`.


## <a name="install-cuda-drivers-for-nc-vms"></a>Instalace ovladačů CUDA NC virtuálních počítačů

Tady jsou kroky tooinstall NVIDIA ovladače na virtuální počítače s Linuxem NC z hello NVIDIA CUDA Toolkit 8.0. 

C a C++ vývojáři můžete volitelně nainstalovat hello úplné Toolkit toobuild GPU accelerated aplikace. Další informace najdete v tématu hello [Průvodce instalací CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).


> [!NOTE]
> Tady jsou aktuální v době publikace k dispozici odkazy stahování ovladačů CUDA. Hello nejnovější ovladače CUDA, najdete v článku hello [NVIDIA](http://www.nvidia.com/) webu.
>

tooinstall CUDA Toolkit, ujistěte se, tooeach připojení SSH virtuálních počítačů. tooverify, který hello systému má podporující CUDA grafického procesoru, spusťte následující příkaz hello:

```bash
lspci | grep -i NVIDIA
```
Zobrazí se výstup podobný toohello následující příklad (zobrazující pomocí karty NVIDIA tesla – měrná K80):

![výstup příkazu lspci](./media/n-series-driver-setup/lspci.png)

Potom spusťte instalaci příkazy, které jsou specifické pro distribuční.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Stáhněte a nainstalujte ovladače CUDA hello.
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  Hello instalace může trvat několik minut.

2. toooptionally instalace hello dokončení CUDA toolkit, typ:

  ```bash
  sudo apt-get install cuda
  ```

3. Restartujte hello virtuálních počítačů a tooverify hello instalace pokračovat.

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>Na základě centOS 7.3 nebo Red Hat Enterprise Linux 7.3

1. Získáte aktualizace. 

  ```bash
  sudo yum update

  sudo reboot
  ```
2. Připojte se znovu toohello virtuálních počítačů a instalace hello nejnovější integrační služby Linuxu pro Hyper-V.

  > [!IMPORTANT]
  > Pokud jste nainstalovali bitovou kopii na základě CentOS HPC ve virtuálním počítači NC24r, přeskočte tooStep 3. Vzhledem k tomu, že Azure RDMA ovladače a integrační služby Linuxu jsou předem nainstalovány v bitové kopii hello, by neměl být upgradovány LIS a jádra aktualizace jsou ve výchozím nastavení zakázané.
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1.tar.gz
 
  tar xvzf lis-rpms-4.2.1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. Připojte se znovu toohello virtuálních počítačů a pokračovat v instalaci s hello následující příkazy:

  ```bash
  sudo yum install kernel-devel

  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

  sudo yum install dkms

  CUDA_REPO_PKG=cuda-repo-rhel7-8.0.61-1.x86_64.rpm

  wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

  sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo yum install cuda-drivers
  ```

  Hello instalace může trvat několik minut. 

4. toooptionally instalace hello dokončení CUDA toolkit, typ:

  ```bash
  sudo yum install cuda
  ```

5. Restartujte hello virtuálních počítačů a tooverify hello instalace pokračovat.


### <a name="verify-driver-installation"></a>Ověření instalace ovladačů


tooquery hello GPU stavu zařízení, SSH toohello virtuální počítač a spusťte hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) s ovladačem hello nainstalovaný nástroj příkazového řádku. 

Zobrazí se výstup podobný toohello následující:

![Stav zařízení NVIDIA](./media/n-series-driver-setup/smi.png)


### <a name="cuda-driver-updates"></a>Aktualizace ovladačů CUDA

Doporučujeme pravidelně aktualizovat ovladače CUDA po nasazení.

#### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```


#### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>Na základě centOS 7.3 nebo Red Hat Enterprise Linux 7.3

```bash
sudo yum update

sudo reboot
```



## <a name="troubleshooting"></a>Řešení potíží

* Je známý problém s ovladači CUDA na virtuálních počítačích Azure N-series systémem Ubuntu 16.04 LTS hello 4.4.0-75 Linux jádra. Pokud provádíte upgrade ze starší verze jádra, upgradujte tooat minimálně verze 4.4.0-77 jádra. 



## <a name="next-steps"></a>Další kroky

* Další informace o hello NVIDIA grafickými procesory na hello N-series virtuálních počítačů najdete v tématu:
    * [Tesla – měrná K80 NVIDIA](http://www.nvidia.com/object/tesla-k80.html) (pro virtuální počítače Azure NC)
    * [Tesla – měrná M60 NVIDIA](http://www.nvidia.com/object/tesla-m60.html) (pro virtuální počítače Azure vs)

* toocapture bitovou kopii virtuálního počítače s Linuxem pomocí nainstalované ovladače NVIDIA, najdete v části [jak toogeneralize a zachytit virtuální počítač s Linuxem](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
