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
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a><span data-ttu-id="e0684-103">Instalace ovladačů NVIDIA GPU v N-series virtuální počítače se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="e0684-103">Install NVIDIA GPU drivers on N-series VMs running Linux</span></span>

<span data-ttu-id="e0684-104">tootake využívat funkce GPU hello Azure N-series virtuální počítače se systémem Linux, instalace podporované NVIDIA grafické ovladače.</span><span class="sxs-lookup"><span data-stu-id="e0684-104">tootake advantage of hello GPU capabilities of Azure N-series VMs running Linux, install supported NVIDIA graphics drivers.</span></span> <span data-ttu-id="e0684-105">Tento článek obsahuje kroky instalace ovladačů po nasadit virtuální počítač s N-series.</span><span class="sxs-lookup"><span data-stu-id="e0684-105">This article provides driver setup steps after you deploy an N-series VM.</span></span> <span data-ttu-id="e0684-106">Informace o instalaci ovladačů je také k dispozici pro [virtuálních počítačů Windows](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0684-106">Driver setup information is also available for [Windows VMs](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


<span data-ttu-id="e0684-107">Virtuální počítač N-series specifikace, kapacity úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačů Linux GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0684-107">For N-series VM specs, storage capacities, and disk details, see [GPU Linux VM sizes](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a><span data-ttu-id="e0684-108">Instalace ovladačů mřížky pro virtuální počítače vs</span><span class="sxs-lookup"><span data-stu-id="e0684-108">Install GRID drivers for NV VMs</span></span>

<span data-ttu-id="e0684-109">ovladače NVIDIA mřížky tooinstall na virtuálních počítačích vs, ujistěte se, tooeach připojení SSH virtuálního počítače a postupujte podle kroků hello pro distribuční Linux.</span><span class="sxs-lookup"><span data-stu-id="e0684-109">tooinstall NVIDIA GRID drivers on NV VMs, make an SSH connection tooeach VM and follow hello steps for your Linux distribution.</span></span> 

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="e0684-110">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="e0684-110">Ubuntu 16.04 LTS</span></span>

1. <span data-ttu-id="e0684-111">Spustit hello `lspci` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e0684-111">Run hello `lspci` command.</span></span> <span data-ttu-id="e0684-112">Ověřte, zda karta hello NVIDIA M60 nebo karty jsou viditelné jako PCI zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0684-112">Verify that hello NVIDIA M60 card or cards are visible as PCI devices.</span></span>

2. <span data-ttu-id="e0684-113">Nainstalujte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e0684-113">Install updates.</span></span>

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. <span data-ttu-id="e0684-114">Zakážete hello Nouveau jádra ovladač, který není kompatibilní s hello NVIDIA ovladačů.</span><span class="sxs-lookup"><span data-stu-id="e0684-114">Disable hello Nouveau kernel driver, which is incompatible with hello NVIDIA driver.</span></span> <span data-ttu-id="e0684-115">(Použijte pouze hello NVIDIA ovladač na virtuálních počítačích vs.) toodo, vytvořte soubor v `/etc/modprobe.d `s názvem `nouveau.conf` s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="e0684-115">(Only use hello NVIDIA driver on NV VMs.) toodo this, create a file in `/etc/modprobe.d `named `nouveau.conf` with hello following contents:</span></span>

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. <span data-ttu-id="e0684-116">Restartujte hello virtuálních počítačů a znovu připojit.</span><span class="sxs-lookup"><span data-stu-id="e0684-116">Reboot hello VM and reconnect.</span></span> <span data-ttu-id="e0684-117">Ukončení X server:</span><span class="sxs-lookup"><span data-stu-id="e0684-117">Exit X server:</span></span>

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. <span data-ttu-id="e0684-118">Stáhněte a nainstalujte ovladač mřížky hello:</span><span class="sxs-lookup"><span data-stu-id="e0684-118">Download and install hello GRID driver:</span></span>

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 

6. <span data-ttu-id="e0684-119">Jakmile se zobrazí výzva, jestli chcete toorun hello nvidia xconfig nástroj tooupdate X konfiguračním souboru, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="e0684-119">When you're asked whether you want toorun hello nvidia-xconfig utility tooupdate your X configuration file, select **Yes**.</span></span>

7. <span data-ttu-id="e0684-120">Po dokončení instalace, zkopírujte /etc/nvidia/gridd.conf.template tooa gridd.conf na nový soubor v umístění/etc/nvidia /</span><span class="sxs-lookup"><span data-stu-id="e0684-120">After installation completes, copy /etc/nvidia/gridd.conf.template tooa new file gridd.conf at location /etc/nvidia/</span></span>

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. <span data-ttu-id="e0684-121">Přidejte následující hello příliš`/etc/nvidia/gridd.conf`:</span><span class="sxs-lookup"><span data-stu-id="e0684-121">Add hello following too`/etc/nvidia/gridd.conf`:</span></span>
 
  ```
  IgnoreSP=TRUE
  ```
9. <span data-ttu-id="e0684-122">Restartujte hello virtuálních počítačů a tooverify hello instalace pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e0684-122">Reboot hello VM and proceed tooverify hello installation.</span></span>


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="e0684-123">Na základě centOS 7.3 nebo Red Hat Enterprise Linux 7.3</span><span class="sxs-lookup"><span data-stu-id="e0684-123">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>


1. <span data-ttu-id="e0684-124">Aktualizace hello jádra a DKMS.</span><span class="sxs-lookup"><span data-stu-id="e0684-124">Update hello kernel and DKMS.</span></span>
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. <span data-ttu-id="e0684-125">Zakážete hello Nouveau jádra ovladač, který není kompatibilní s hello NVIDIA ovladačů.</span><span class="sxs-lookup"><span data-stu-id="e0684-125">Disable hello Nouveau kernel driver, which is incompatible with hello NVIDIA driver.</span></span> <span data-ttu-id="e0684-126">(Použijte pouze hello NVIDIA ovladač na virtuálních počítačích vs.) toodo, vytvořte soubor v `/etc/modprobe.d `s názvem `nouveau.conf` s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="e0684-126">(Only use hello NVIDIA driver on NV VMs.) toodo this, create a file in `/etc/modprobe.d `named `nouveau.conf` with hello following contents:</span></span>

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. <span data-ttu-id="e0684-127">Hello virtuální počítač restartovat, znovu připojit a nainstalovat nejnovější integrační služby Linuxu pro Hyper-V: hello</span><span class="sxs-lookup"><span data-stu-id="e0684-127">Reboot hello VM, reconnect, and install hello latest Linux Integration Services for Hyper-V:</span></span>
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
 
  tar xvzf lis-rpms-4.2.2-2.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
4. <span data-ttu-id="e0684-128">Připojte se znovu toohello virtuálního počítače a spustit hello `lspci` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e0684-128">Reconnect toohello VM and run hello `lspci` command.</span></span> <span data-ttu-id="e0684-129">Ověřte, zda karta hello NVIDIA M60 nebo karty jsou viditelné jako PCI zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0684-129">Verify that hello NVIDIA M60 card or cards are visible as PCI devices.</span></span>
 
5. <span data-ttu-id="e0684-130">Stáhněte a nainstalujte ovladač mřížky hello:</span><span class="sxs-lookup"><span data-stu-id="e0684-130">Download and install hello GRID driver:</span></span>

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 
6. <span data-ttu-id="e0684-131">Jakmile se zobrazí výzva, jestli chcete toorun hello nvidia xconfig nástroj tooupdate X konfiguračním souboru, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="e0684-131">When you're asked whether you want toorun hello nvidia-xconfig utility tooupdate your X configuration file, select **Yes**.</span></span>

7. <span data-ttu-id="e0684-132">Po dokončení instalace, zkopírujte /etc/nvidia/gridd.conf.template tooa gridd.conf na nový soubor v umístění/etc/nvidia /</span><span class="sxs-lookup"><span data-stu-id="e0684-132">After installation completes, copy /etc/nvidia/gridd.conf.template tooa new file gridd.conf at location /etc/nvidia/</span></span>
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. <span data-ttu-id="e0684-133">Přidejte následující hello příliš`/etc/nvidia/gridd.conf`:</span><span class="sxs-lookup"><span data-stu-id="e0684-133">Add hello following too`/etc/nvidia/gridd.conf`:</span></span>
 
  ```
  IgnoreSP=TRUE
  ```
9. <span data-ttu-id="e0684-134">Restartujte hello virtuálních počítačů a tooverify hello instalace pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e0684-134">Reboot hello VM and proceed tooverify hello installation.</span></span>

### <a name="verify-driver-installation"></a><span data-ttu-id="e0684-135">Ověření instalace ovladačů</span><span class="sxs-lookup"><span data-stu-id="e0684-135">Verify driver installation</span></span>


<span data-ttu-id="e0684-136">tooquery hello GPU stavu zařízení, SSH toohello virtuální počítač a spusťte hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) s ovladačem hello nainstalovaný nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e0684-136">tooquery hello GPU device state, SSH toohello VM and run hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with hello driver.</span></span> 

<span data-ttu-id="e0684-137">Zobrazí se výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="e0684-137">Output similar toohello following appears:</span></span>

![Stav zařízení NVIDIA](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a><span data-ttu-id="e0684-139">X11 serveru</span><span class="sxs-lookup"><span data-stu-id="e0684-139">X11 server</span></span>
<span data-ttu-id="e0684-140">Pokud budete potřebovat X11 server pro vzdálená připojení tooan vs virtuálních počítačů, [x11vnc](http://www.karlrunge.com/x11vnc/) se doporučuje, protože umožňuje hardwarovou akceleraci grafiky.</span><span class="sxs-lookup"><span data-stu-id="e0684-140">If you need an X11 server for remote connections tooan NV VM, [x11vnc](http://www.karlrunge.com/x11vnc/) is recommended because it allows hardware acceleration of graphics.</span></span> <span data-ttu-id="e0684-141">Hello BusID hello M60 zařízení musí ručně přidat soubor xconfig toohello (`etc/X11/xorg.conf` na Ubuntu 16.04 LTS, `/etc/X11/XF86config` na CentOS 7.3 nebo Red Hat Enterprise Server 7.3).</span><span class="sxs-lookup"><span data-stu-id="e0684-141">hello BusID of hello M60 device must be manually added toohello xconfig file (`etc/X11/xorg.conf` on Ubuntu 16.04 LTS, `/etc/X11/XF86config` on CentOS 7.3 or Red Hat Enterprise Server 7.3).</span></span> <span data-ttu-id="e0684-142">Přidat `"Device"` podobné toohello následující části:</span><span class="sxs-lookup"><span data-stu-id="e0684-142">Add a `"Device"` section similar toohello following:</span></span>
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
<span data-ttu-id="e0684-143">Kromě toho aktualizovat vaše `"Screen"` části toouse toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0684-143">Additionally, update your `"Screen"` section toouse this device.</span></span>
 
<span data-ttu-id="e0684-144">Hello BusID můžete najít spuštěním</span><span class="sxs-lookup"><span data-stu-id="e0684-144">hello BusID can be found by running</span></span>

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
<span data-ttu-id="e0684-145">Hello BusID lze změnit, pokud virtuální počítač získá znovu přidělit, nebo restartovat.</span><span class="sxs-lookup"><span data-stu-id="e0684-145">hello BusID can change when a VM gets reallocated or rebooted.</span></span> <span data-ttu-id="e0684-146">Proto můžete chtít toouse hello tooupdate skriptu BusID v konfiguraci hello X11 při po restartu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e0684-146">Therefore, you may want toouse a script tooupdate hello BusID in hello X11 configuration when a VM is rebooted.</span></span> <span data-ttu-id="e0684-147">Například:</span><span class="sxs-lookup"><span data-stu-id="e0684-147">For example:</span></span>

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed too${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

<span data-ttu-id="e0684-148">Tento soubor nelze vyvolat jako kořenová na spouštěcí tak, že vytvoříte položku pro něj v `/etc/rc.d/rc3.d`.</span><span class="sxs-lookup"><span data-stu-id="e0684-148">This file can be invoked as root on boot by creating an entry for it in `/etc/rc.d/rc3.d`.</span></span>


## <a name="install-cuda-drivers-for-nc-vms"></a><span data-ttu-id="e0684-149">Instalace ovladačů CUDA NC virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="e0684-149">Install CUDA drivers for NC VMs</span></span>

<span data-ttu-id="e0684-150">Tady jsou kroky tooinstall NVIDIA ovladače na virtuální počítače s Linuxem NC z hello NVIDIA CUDA Toolkit 8.0.</span><span class="sxs-lookup"><span data-stu-id="e0684-150">Here are steps tooinstall NVIDIA drivers on Linux NC VMs from hello NVIDIA CUDA Toolkit 8.0.</span></span> 

<span data-ttu-id="e0684-151">C a C++ vývojáři můžete volitelně nainstalovat hello úplné Toolkit toobuild GPU accelerated aplikace.</span><span class="sxs-lookup"><span data-stu-id="e0684-151">C and C++ developers can optionally install hello full Toolkit toobuild GPU-accelerated applications.</span></span> <span data-ttu-id="e0684-152">Další informace najdete v tématu hello [Průvodce instalací CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).</span><span class="sxs-lookup"><span data-stu-id="e0684-152">For more information, see hello [CUDA Installation Guide](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).</span></span>


> [!NOTE]
> <span data-ttu-id="e0684-153">Tady jsou aktuální v době publikace k dispozici odkazy stahování ovladačů CUDA.</span><span class="sxs-lookup"><span data-stu-id="e0684-153">CUDA driver download links provided here are current at time of publication.</span></span> <span data-ttu-id="e0684-154">Hello nejnovější ovladače CUDA, najdete v článku hello [NVIDIA](http://www.nvidia.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="e0684-154">For hello latest CUDA drivers, visit hello [NVIDIA](http://www.nvidia.com/) website.</span></span>
>

<span data-ttu-id="e0684-155">tooinstall CUDA Toolkit, ujistěte se, tooeach připojení SSH virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e0684-155">tooinstall CUDA Toolkit, make an SSH connection tooeach VM.</span></span> <span data-ttu-id="e0684-156">tooverify, který hello systému má podporující CUDA grafického procesoru, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e0684-156">tooverify that hello system has a CUDA-capable GPU, run hello following command:</span></span>

```bash
lspci | grep -i NVIDIA
```
<span data-ttu-id="e0684-157">Zobrazí se výstup podobný toohello následující příklad (zobrazující pomocí karty NVIDIA tesla – měrná K80):</span><span class="sxs-lookup"><span data-stu-id="e0684-157">You will see output similar toohello following example (showing an NVIDIA Tesla K80 card):</span></span>

![výstup příkazu lspci](./media/n-series-driver-setup/lspci.png)

<span data-ttu-id="e0684-159">Potom spusťte instalaci příkazy, které jsou specifické pro distribuční.</span><span class="sxs-lookup"><span data-stu-id="e0684-159">Then run installation commands specific for your distribution.</span></span>

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="e0684-160">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="e0684-160">Ubuntu 16.04 LTS</span></span>

1. <span data-ttu-id="e0684-161">Stáhněte a nainstalujte ovladače CUDA hello.</span><span class="sxs-lookup"><span data-stu-id="e0684-161">Download and install hello CUDA drivers.</span></span>
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  <span data-ttu-id="e0684-162">Hello instalace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="e0684-162">hello installation can take several minutes.</span></span>

2. <span data-ttu-id="e0684-163">toooptionally instalace hello dokončení CUDA toolkit, typ:</span><span class="sxs-lookup"><span data-stu-id="e0684-163">toooptionally install hello complete CUDA toolkit, type:</span></span>

  ```bash
  sudo apt-get install cuda
  ```

3. <span data-ttu-id="e0684-164">Restartujte hello virtuálních počítačů a tooverify hello instalace pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e0684-164">Reboot hello VM and proceed tooverify hello installation.</span></span>

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="e0684-165">Na základě centOS 7.3 nebo Red Hat Enterprise Linux 7.3</span><span class="sxs-lookup"><span data-stu-id="e0684-165">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>

1. <span data-ttu-id="e0684-166">Získáte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e0684-166">Get updates.</span></span> 

  ```bash
  sudo yum update

  sudo reboot
  ```
2. <span data-ttu-id="e0684-167">Připojte se znovu toohello virtuálních počítačů a instalace hello nejnovější integrační služby Linuxu pro Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="e0684-167">Reconnect toohello VM and install hello latest Linux Integration Services for Hyper-V.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="e0684-168">Pokud jste nainstalovali bitovou kopii na základě CentOS HPC ve virtuálním počítači NC24r, přeskočte tooStep 3.</span><span class="sxs-lookup"><span data-stu-id="e0684-168">If you installed a CentOS-based HPC image on an NC24r VM, skip tooStep 3.</span></span> <span data-ttu-id="e0684-169">Vzhledem k tomu, že Azure RDMA ovladače a integrační služby Linuxu jsou předem nainstalovány v bitové kopii hello, by neměl být upgradovány LIS a jádra aktualizace jsou ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="e0684-169">Because Azure RDMA drivers and Linux Integration Services are pre-installed in hello image, LIS should not be upgraded, and kernel updates are disabled by default.</span></span>
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1.tar.gz
 
  tar xvzf lis-rpms-4.2.1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. <span data-ttu-id="e0684-170">Připojte se znovu toohello virtuálních počítačů a pokračovat v instalaci s hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e0684-170">Reconnect toohello VM and continue installation with hello following commands:</span></span>

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

  <span data-ttu-id="e0684-171">Hello instalace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="e0684-171">hello installation can take several minutes.</span></span> 

4. <span data-ttu-id="e0684-172">toooptionally instalace hello dokončení CUDA toolkit, typ:</span><span class="sxs-lookup"><span data-stu-id="e0684-172">toooptionally install hello complete CUDA toolkit, type:</span></span>

  ```bash
  sudo yum install cuda
  ```

5. <span data-ttu-id="e0684-173">Restartujte hello virtuálních počítačů a tooverify hello instalace pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e0684-173">Reboot hello VM and proceed tooverify hello installation.</span></span>


### <a name="verify-driver-installation"></a><span data-ttu-id="e0684-174">Ověření instalace ovladačů</span><span class="sxs-lookup"><span data-stu-id="e0684-174">Verify driver installation</span></span>


<span data-ttu-id="e0684-175">tooquery hello GPU stavu zařízení, SSH toohello virtuální počítač a spusťte hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) s ovladačem hello nainstalovaný nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e0684-175">tooquery hello GPU device state, SSH toohello VM and run hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with hello driver.</span></span> 

<span data-ttu-id="e0684-176">Zobrazí se výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="e0684-176">Output similar toohello following appears:</span></span>

![Stav zařízení NVIDIA](./media/n-series-driver-setup/smi.png)


### <a name="cuda-driver-updates"></a><span data-ttu-id="e0684-178">Aktualizace ovladačů CUDA</span><span class="sxs-lookup"><span data-stu-id="e0684-178">CUDA driver updates</span></span>

<span data-ttu-id="e0684-179">Doporučujeme pravidelně aktualizovat ovladače CUDA po nasazení.</span><span class="sxs-lookup"><span data-stu-id="e0684-179">We recommend that you periodically update CUDA drivers after deployment.</span></span>

#### <a name="ubuntu-1604-lts"></a><span data-ttu-id="e0684-180">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="e0684-180">Ubuntu 16.04 LTS</span></span>

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```


#### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="e0684-181">Na základě centOS 7.3 nebo Red Hat Enterprise Linux 7.3</span><span class="sxs-lookup"><span data-stu-id="e0684-181">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>

```bash
sudo yum update

sudo reboot
```



## <a name="troubleshooting"></a><span data-ttu-id="e0684-182">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="e0684-182">Troubleshooting</span></span>

* <span data-ttu-id="e0684-183">Je známý problém s ovladači CUDA na virtuálních počítačích Azure N-series systémem Ubuntu 16.04 LTS hello 4.4.0-75 Linux jádra.</span><span class="sxs-lookup"><span data-stu-id="e0684-183">There is a known issue with CUDA drivers on Azure N-series VMs running hello 4.4.0-75 Linux kernel on Ubuntu 16.04 LTS.</span></span> <span data-ttu-id="e0684-184">Pokud provádíte upgrade ze starší verze jádra, upgradujte tooat minimálně verze 4.4.0-77 jádra.</span><span class="sxs-lookup"><span data-stu-id="e0684-184">If you are upgrading from an earlier kernel version, upgrade tooat least kernel version 4.4.0-77.</span></span> 



## <a name="next-steps"></a><span data-ttu-id="e0684-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0684-185">Next steps</span></span>

* <span data-ttu-id="e0684-186">Další informace o hello NVIDIA grafickými procesory na hello N-series virtuálních počítačů najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="e0684-186">For more information about hello NVIDIA GPUs on hello N-series VMs, see:</span></span>
    * <span data-ttu-id="e0684-187">[Tesla – měrná K80 NVIDIA](http://www.nvidia.com/object/tesla-k80.html) (pro virtuální počítače Azure NC)</span><span class="sxs-lookup"><span data-stu-id="e0684-187">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (for Azure NC VMs)</span></span>
    * <span data-ttu-id="e0684-188">[Tesla – měrná M60 NVIDIA](http://www.nvidia.com/object/tesla-m60.html) (pro virtuální počítače Azure vs)</span><span class="sxs-lookup"><span data-stu-id="e0684-188">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (for Azure NV VMs)</span></span>

* <span data-ttu-id="e0684-189">toocapture bitovou kopii virtuálního počítače s Linuxem pomocí nainstalované ovladače NVIDIA, najdete v části [jak toogeneralize a zachytit virtuální počítač s Linuxem](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0684-189">toocapture a Linux VM image with your installed NVIDIA drivers, see [How toogeneralize and capture a Linux virtual machine](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
