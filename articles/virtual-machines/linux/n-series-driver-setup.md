---
title: "Instalace nástroje Azure ovladač N-series pro Linux | Microsoft Docs"
description: "Jak nastavit NVIDIA GPU ovladače pro N-series virtuální počítače s Linuxem v Azure"
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
ms.openlocfilehash: bdeb4d5ca1d9ff4d7dfd0961690412dd7530572a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a><span data-ttu-id="4338e-103">Instalace ovladačů NVIDIA GPU v N-series virtuální počítače se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="4338e-103">Install NVIDIA GPU drivers on N-series VMs running Linux</span></span>

<span data-ttu-id="4338e-104">Abyste mohli využívat možnosti GPU Azure N-series virtuální počítače se systémem Linux, nainstalujte podporované NVIDIA grafické ovladače.</span><span class="sxs-lookup"><span data-stu-id="4338e-104">To take advantage of the GPU capabilities of Azure N-series VMs running Linux, install supported NVIDIA graphics drivers.</span></span> <span data-ttu-id="4338e-105">Tento článek obsahuje kroky instalace ovladačů po nasadit virtuální počítač s N-series.</span><span class="sxs-lookup"><span data-stu-id="4338e-105">This article provides driver setup steps after you deploy an N-series VM.</span></span> <span data-ttu-id="4338e-106">Informace o instalaci ovladačů je také k dispozici pro [virtuálních počítačů Windows](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4338e-106">Driver setup information is also available for [Windows VMs](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


<span data-ttu-id="4338e-107">Virtuální počítač N-series specifikace, kapacity úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačů Linux GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4338e-107">For N-series VM specs, storage capacities, and disk details, see [GPU Linux VM sizes](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a><span data-ttu-id="4338e-108">Instalace ovladačů mřížky pro virtuální počítače vs</span><span class="sxs-lookup"><span data-stu-id="4338e-108">Install GRID drivers for NV VMs</span></span>

<span data-ttu-id="4338e-109">Instalace ovladačů NVIDIA mřížky na virtuálních počítačích vs, proveďte připojení SSH pro každý virtuální počítač a postupujte podle kroků pro vaše distribuci systému Linux.</span><span class="sxs-lookup"><span data-stu-id="4338e-109">To install NVIDIA GRID drivers on NV VMs, make an SSH connection to each VM and follow the steps for your Linux distribution.</span></span> 

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="4338e-110">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="4338e-110">Ubuntu 16.04 LTS</span></span>

1. <span data-ttu-id="4338e-111">Spustit `lspci` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4338e-111">Run the `lspci` command.</span></span> <span data-ttu-id="4338e-112">Ověřte, zda karta NVIDIA M60 nebo karty jsou viditelné jako PCI zařízení.</span><span class="sxs-lookup"><span data-stu-id="4338e-112">Verify that the NVIDIA M60 card or cards are visible as PCI devices.</span></span>

2. <span data-ttu-id="4338e-113">Nainstalujte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="4338e-113">Install updates.</span></span>

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. <span data-ttu-id="4338e-114">Zakážete Nouveau ovladač jádra, který není kompatibilní s NVIDIA ovladače.</span><span class="sxs-lookup"><span data-stu-id="4338e-114">Disable the Nouveau kernel driver, which is incompatible with the NVIDIA driver.</span></span> <span data-ttu-id="4338e-115">(Použijte pouze NVIDIA ovladač na virtuálních počítačích vs.) Chcete-li to provést, vytvořte soubor v `/etc/modprobe.d `s názvem `nouveau.conf` s tímto obsahem:</span><span class="sxs-lookup"><span data-stu-id="4338e-115">(Only use the NVIDIA driver on NV VMs.) To do this, create a file in `/etc/modprobe.d `named `nouveau.conf` with the following contents:</span></span>

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. <span data-ttu-id="4338e-116">Restartujte virtuální počítač a znovu připojit.</span><span class="sxs-lookup"><span data-stu-id="4338e-116">Reboot the VM and reconnect.</span></span> <span data-ttu-id="4338e-117">Ukončení X server:</span><span class="sxs-lookup"><span data-stu-id="4338e-117">Exit X server:</span></span>

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. <span data-ttu-id="4338e-118">Stáhněte a nainstalujte ovladač mřížky:</span><span class="sxs-lookup"><span data-stu-id="4338e-118">Download and install the GRID driver:</span></span>

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 

6. <span data-ttu-id="4338e-119">Pokud se dotaz, zda chcete spustit nástroj nvidia xconfig aktualizovat vaše X konfigurační soubor, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="4338e-119">When you're asked whether you want to run the nvidia-xconfig utility to update your X configuration file, select **Yes**.</span></span>

7. <span data-ttu-id="4338e-120">Po dokončení instalace, zkopírujte do nové gridd.conf soubor v umístění/etc/nvidia//etc/nvidia/gridd.conf.template</span><span class="sxs-lookup"><span data-stu-id="4338e-120">After installation completes, copy /etc/nvidia/gridd.conf.template to a new file gridd.conf at location /etc/nvidia/</span></span>

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. <span data-ttu-id="4338e-121">Přidejte následující `/etc/nvidia/gridd.conf`:</span><span class="sxs-lookup"><span data-stu-id="4338e-121">Add the following to `/etc/nvidia/gridd.conf`:</span></span>
 
  ```
  IgnoreSP=TRUE
  ```
9. <span data-ttu-id="4338e-122">Restartujte virtuální počítač a přejděte k ověření instalace.</span><span class="sxs-lookup"><span data-stu-id="4338e-122">Reboot the VM and proceed to verify the installation.</span></span>


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="4338e-123">Na základě centOS 7.3 nebo Red Hat Enterprise Linux 7.3</span><span class="sxs-lookup"><span data-stu-id="4338e-123">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>


1. <span data-ttu-id="4338e-124">Aktualizace jádra a DKMS.</span><span class="sxs-lookup"><span data-stu-id="4338e-124">Update the kernel and DKMS.</span></span>
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. <span data-ttu-id="4338e-125">Zakážete Nouveau ovladač jádra, který není kompatibilní s NVIDIA ovladače.</span><span class="sxs-lookup"><span data-stu-id="4338e-125">Disable the Nouveau kernel driver, which is incompatible with the NVIDIA driver.</span></span> <span data-ttu-id="4338e-126">(Použijte pouze NVIDIA ovladač na virtuálních počítačích vs.) Chcete-li to provést, vytvořte soubor v `/etc/modprobe.d `s názvem `nouveau.conf` s tímto obsahem:</span><span class="sxs-lookup"><span data-stu-id="4338e-126">(Only use the NVIDIA driver on NV VMs.) To do this, create a file in `/etc/modprobe.d `named `nouveau.conf` with the following contents:</span></span>

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. <span data-ttu-id="4338e-127">Restartovat virtuální počítač, připojte se znovu a nainstalujte nejnovější integrační služby Linuxu pro Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="4338e-127">Reboot the VM, reconnect, and install the latest Linux Integration Services for Hyper-V:</span></span>
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
 
  tar xvzf lis-rpms-4.2.2-2.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
4. <span data-ttu-id="4338e-128">Znovu se připojte k virtuální počítač a spustit `lspci` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4338e-128">Reconnect to the VM and run the `lspci` command.</span></span> <span data-ttu-id="4338e-129">Ověřte, zda karta NVIDIA M60 nebo karty jsou viditelné jako PCI zařízení.</span><span class="sxs-lookup"><span data-stu-id="4338e-129">Verify that the NVIDIA M60 card or cards are visible as PCI devices.</span></span>
 
5. <span data-ttu-id="4338e-130">Stáhněte a nainstalujte ovladač mřížky:</span><span class="sxs-lookup"><span data-stu-id="4338e-130">Download and install the GRID driver:</span></span>

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 
6. <span data-ttu-id="4338e-131">Pokud se dotaz, zda chcete spustit nástroj nvidia xconfig aktualizovat vaše X konfigurační soubor, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="4338e-131">When you're asked whether you want to run the nvidia-xconfig utility to update your X configuration file, select **Yes**.</span></span>

7. <span data-ttu-id="4338e-132">Po dokončení instalace, zkopírujte do nové gridd.conf soubor v umístění/etc/nvidia//etc/nvidia/gridd.conf.template</span><span class="sxs-lookup"><span data-stu-id="4338e-132">After installation completes, copy /etc/nvidia/gridd.conf.template to a new file gridd.conf at location /etc/nvidia/</span></span>
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. <span data-ttu-id="4338e-133">Přidejte následující `/etc/nvidia/gridd.conf`:</span><span class="sxs-lookup"><span data-stu-id="4338e-133">Add the following to `/etc/nvidia/gridd.conf`:</span></span>
 
  ```
  IgnoreSP=TRUE
  ```
9. <span data-ttu-id="4338e-134">Restartujte virtuální počítač a přejděte k ověření instalace.</span><span class="sxs-lookup"><span data-stu-id="4338e-134">Reboot the VM and proceed to verify the installation.</span></span>

### <a name="verify-driver-installation"></a><span data-ttu-id="4338e-135">Ověření instalace ovladačů</span><span class="sxs-lookup"><span data-stu-id="4338e-135">Verify driver installation</span></span>


<span data-ttu-id="4338e-136">K dotazování na GPU zařízení stav, SSH pro virtuální počítač a spusťte [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) pomocí ovladače nainstalovaný nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4338e-136">To query the GPU device state, SSH to the VM and run the [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with the driver.</span></span> 

<span data-ttu-id="4338e-137">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="4338e-137">Output similar to the following appears:</span></span>

![Stav zařízení NVIDIA](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a><span data-ttu-id="4338e-139">X11 serveru</span><span class="sxs-lookup"><span data-stu-id="4338e-139">X11 server</span></span>
<span data-ttu-id="4338e-140">Pokud budete potřebovat X11 serveru pro vzdálená připojení na virtuální počítač vs, [x11vnc](http://www.karlrunge.com/x11vnc/) se doporučuje, protože umožňuje hardwarovou akceleraci grafiky.</span><span class="sxs-lookup"><span data-stu-id="4338e-140">If you need an X11 server for remote connections to an NV VM, [x11vnc](http://www.karlrunge.com/x11vnc/) is recommended because it allows hardware acceleration of graphics.</span></span> <span data-ttu-id="4338e-141">BusID M60 zařízení musí ručně přidat do souboru xconfig (`etc/X11/xorg.conf` na Ubuntu 16.04 LTS, `/etc/X11/XF86config` na CentOS 7.3 nebo Red Hat Enterprise Server 7.3).</span><span class="sxs-lookup"><span data-stu-id="4338e-141">The BusID of the M60 device must be manually added to the xconfig file (`etc/X11/xorg.conf` on Ubuntu 16.04 LTS, `/etc/X11/XF86config` on CentOS 7.3 or Red Hat Enterprise Server 7.3).</span></span> <span data-ttu-id="4338e-142">Přidat `"Device"` části podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="4338e-142">Add a `"Device"` section similar to the following:</span></span>
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
<span data-ttu-id="4338e-143">Kromě toho aktualizovat vaše `"Screen"` části k použití tohoto zařízení.</span><span class="sxs-lookup"><span data-stu-id="4338e-143">Additionally, update your `"Screen"` section to use this device.</span></span>
 
<span data-ttu-id="4338e-144">BusID můžete najít spuštěním</span><span class="sxs-lookup"><span data-stu-id="4338e-144">The BusID can be found by running</span></span>

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
<span data-ttu-id="4338e-145">BusID lze změnit, pokud virtuální počítač získá znovu přidělit, nebo restartovat.</span><span class="sxs-lookup"><span data-stu-id="4338e-145">The BusID can change when a VM gets reallocated or rebooted.</span></span> <span data-ttu-id="4338e-146">Proto můžete chtít použít skript k aktualizaci BusID v X11 konfigurace, pokud je virtuální počítač restartovat.</span><span class="sxs-lookup"><span data-stu-id="4338e-146">Therefore, you may want to use a script to update the BusID in the X11 configuration when a VM is rebooted.</span></span> <span data-ttu-id="4338e-147">Například:</span><span class="sxs-lookup"><span data-stu-id="4338e-147">For example:</span></span>

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed to ${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

<span data-ttu-id="4338e-148">Tento soubor nelze vyvolat jako kořenová na spouštěcí tak, že vytvoříte položku pro něj v `/etc/rc.d/rc3.d`.</span><span class="sxs-lookup"><span data-stu-id="4338e-148">This file can be invoked as root on boot by creating an entry for it in `/etc/rc.d/rc3.d`.</span></span>


## <a name="install-cuda-drivers-for-nc-vms"></a><span data-ttu-id="4338e-149">Instalace ovladačů CUDA NC virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4338e-149">Install CUDA drivers for NC VMs</span></span>

<span data-ttu-id="4338e-150">Tady jsou kroky pro instalaci ovladače NVIDIA na virtuální počítače s Linuxem NC z NVIDIA CUDA Toolkit 8.0.</span><span class="sxs-lookup"><span data-stu-id="4338e-150">Here are steps to install NVIDIA drivers on Linux NC VMs from the NVIDIA CUDA Toolkit 8.0.</span></span> 

<span data-ttu-id="4338e-151">Jazyk C a C++ vývojáři Volitelně můžete nainstalovat úplnou sadu nástrojů k vytváření aplikací GPU accelerated.</span><span class="sxs-lookup"><span data-stu-id="4338e-151">C and C++ developers can optionally install the full Toolkit to build GPU-accelerated applications.</span></span> <span data-ttu-id="4338e-152">Další informace najdete v tématu [Průvodce instalací CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).</span><span class="sxs-lookup"><span data-stu-id="4338e-152">For more information, see the [CUDA Installation Guide](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).</span></span>


> [!NOTE]
> <span data-ttu-id="4338e-153">Tady jsou aktuální v době publikace k dispozici odkazy stahování ovladačů CUDA.</span><span class="sxs-lookup"><span data-stu-id="4338e-153">CUDA driver download links provided here are current at time of publication.</span></span> <span data-ttu-id="4338e-154">Nejnovější ovladače CUDA, najdete v článku [NVIDIA](http://www.nvidia.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="4338e-154">For the latest CUDA drivers, visit the [NVIDIA](http://www.nvidia.com/) website.</span></span>
>

<span data-ttu-id="4338e-155">K instalaci nástrojů CUDA, zkontrolujte připojení SSH pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4338e-155">To install CUDA Toolkit, make an SSH connection to each VM.</span></span> <span data-ttu-id="4338e-156">Pokud chcete ověřit, že systém má podporující CUDA grafického procesoru, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4338e-156">To verify that the system has a CUDA-capable GPU, run the following command:</span></span>

```bash
lspci | grep -i NVIDIA
```
<span data-ttu-id="4338e-157">Zobrazí se výstup podobný v následujícím příkladu (zobrazující pomocí karty NVIDIA tesla – měrná K80):</span><span class="sxs-lookup"><span data-stu-id="4338e-157">You will see output similar to the following example (showing an NVIDIA Tesla K80 card):</span></span>

![výstup příkazu lspci](./media/n-series-driver-setup/lspci.png)

<span data-ttu-id="4338e-159">Potom spusťte instalaci příkazy, které jsou specifické pro distribuční.</span><span class="sxs-lookup"><span data-stu-id="4338e-159">Then run installation commands specific for your distribution.</span></span>

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="4338e-160">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="4338e-160">Ubuntu 16.04 LTS</span></span>

1. <span data-ttu-id="4338e-161">Stáhněte a nainstalujte CUDA ovladače.</span><span class="sxs-lookup"><span data-stu-id="4338e-161">Download and install the CUDA drivers.</span></span>
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  <span data-ttu-id="4338e-162">Instalace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="4338e-162">The installation can take several minutes.</span></span>

2. <span data-ttu-id="4338e-163">Volitelně můžete nainstalovat úplnou sadu CUDA, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4338e-163">To optionally install the complete CUDA toolkit, type:</span></span>

  ```bash
  sudo apt-get install cuda
  ```

3. <span data-ttu-id="4338e-164">Restartujte virtuální počítač a přejděte k ověření instalace.</span><span class="sxs-lookup"><span data-stu-id="4338e-164">Reboot the VM and proceed to verify the installation.</span></span>

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="4338e-165">Na základě centOS 7.3 nebo Red Hat Enterprise Linux 7.3</span><span class="sxs-lookup"><span data-stu-id="4338e-165">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>

1. <span data-ttu-id="4338e-166">Získáte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="4338e-166">Get updates.</span></span> 

  ```bash
  sudo yum update

  sudo reboot
  ```
2. <span data-ttu-id="4338e-167">Znovu připojte k virtuálnímu počítači a nainstalovat nejnovější integrační služby Linuxu pro Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="4338e-167">Reconnect to the VM and install the latest Linux Integration Services for Hyper-V.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="4338e-168">Pokud jste nainstalovali bitovou kopii na základě CentOS HPC ve virtuálním počítači NC24r, přejděte ke kroku 3.</span><span class="sxs-lookup"><span data-stu-id="4338e-168">If you installed a CentOS-based HPC image on an NC24r VM, skip to Step 3.</span></span> <span data-ttu-id="4338e-169">Vzhledem k tomu, že Azure RDMA ovladače a integrační služby Linuxu jsou předem nainstalovaná v bitové kopii, by neměl být upgradovány LIS a jádra aktualizace jsou ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="4338e-169">Because Azure RDMA drivers and Linux Integration Services are pre-installed in the image, LIS should not be upgraded, and kernel updates are disabled by default.</span></span>
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1.tar.gz
 
  tar xvzf lis-rpms-4.2.1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. <span data-ttu-id="4338e-170">Připojení k virtuálnímu počítači a pokračujte v instalaci pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="4338e-170">Reconnect to the VM and continue installation with the following commands:</span></span>

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

  <span data-ttu-id="4338e-171">Instalace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="4338e-171">The installation can take several minutes.</span></span> 

4. <span data-ttu-id="4338e-172">Volitelně můžete nainstalovat úplnou sadu CUDA, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4338e-172">To optionally install the complete CUDA toolkit, type:</span></span>

  ```bash
  sudo yum install cuda
  ```

5. <span data-ttu-id="4338e-173">Restartujte virtuální počítač a přejděte k ověření instalace.</span><span class="sxs-lookup"><span data-stu-id="4338e-173">Reboot the VM and proceed to verify the installation.</span></span>


### <a name="verify-driver-installation"></a><span data-ttu-id="4338e-174">Ověření instalace ovladačů</span><span class="sxs-lookup"><span data-stu-id="4338e-174">Verify driver installation</span></span>


<span data-ttu-id="4338e-175">K dotazování na GPU zařízení stav, SSH pro virtuální počítač a spusťte [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) pomocí ovladače nainstalovaný nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4338e-175">To query the GPU device state, SSH to the VM and run the [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with the driver.</span></span> 

<span data-ttu-id="4338e-176">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="4338e-176">Output similar to the following appears:</span></span>

![Stav zařízení NVIDIA](./media/n-series-driver-setup/smi.png)


### <a name="cuda-driver-updates"></a><span data-ttu-id="4338e-178">Aktualizace ovladačů CUDA</span><span class="sxs-lookup"><span data-stu-id="4338e-178">CUDA driver updates</span></span>

<span data-ttu-id="4338e-179">Doporučujeme pravidelně aktualizovat ovladače CUDA po nasazení.</span><span class="sxs-lookup"><span data-stu-id="4338e-179">We recommend that you periodically update CUDA drivers after deployment.</span></span>

#### <a name="ubuntu-1604-lts"></a><span data-ttu-id="4338e-180">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="4338e-180">Ubuntu 16.04 LTS</span></span>

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```


#### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="4338e-181">Na základě centOS 7.3 nebo Red Hat Enterprise Linux 7.3</span><span class="sxs-lookup"><span data-stu-id="4338e-181">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>

```bash
sudo yum update

sudo reboot
```



## <a name="troubleshooting"></a><span data-ttu-id="4338e-182">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4338e-182">Troubleshooting</span></span>

* <span data-ttu-id="4338e-183">Je známý problém s ovladači CUDA na virtuálních počítačích Azure N-series systémem Ubuntu 16.04 LTS Linux jádra 4.4.0-75.</span><span class="sxs-lookup"><span data-stu-id="4338e-183">There is a known issue with CUDA drivers on Azure N-series VMs running the 4.4.0-75 Linux kernel on Ubuntu 16.04 LTS.</span></span> <span data-ttu-id="4338e-184">Pokud provádíte upgrade ze starší verze jádra, upgradujte alespoň 4.4.0-77 verze jádra.</span><span class="sxs-lookup"><span data-stu-id="4338e-184">If you are upgrading from an earlier kernel version, upgrade to at least kernel version 4.4.0-77.</span></span> 



## <a name="next-steps"></a><span data-ttu-id="4338e-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4338e-185">Next steps</span></span>

* <span data-ttu-id="4338e-186">Další informace o grafickými procesory NVIDIA na virtuálních počítačích N-series najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="4338e-186">For more information about the NVIDIA GPUs on the N-series VMs, see:</span></span>
    * <span data-ttu-id="4338e-187">[Tesla – měrná K80 NVIDIA](http://www.nvidia.com/object/tesla-k80.html) (pro virtuální počítače Azure NC)</span><span class="sxs-lookup"><span data-stu-id="4338e-187">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (for Azure NC VMs)</span></span>
    * <span data-ttu-id="4338e-188">[Tesla – měrná M60 NVIDIA](http://www.nvidia.com/object/tesla-m60.html) (pro virtuální počítače Azure vs)</span><span class="sxs-lookup"><span data-stu-id="4338e-188">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (for Azure NV VMs)</span></span>

* <span data-ttu-id="4338e-189">K zachycení bitové kopie virtuálního počítače s Linuxem s vaší nainstalované ovladače NVIDIA, najdete v části [generalize a zachycení virtuální počítač s Linuxem](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4338e-189">To capture a Linux VM image with your installed NVIDIA drivers, see [How to generalize and capture a Linux virtual machine](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
