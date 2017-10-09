---
title: "instalace ovladačů aaaAzure N-series pro Windows | Microsoft Docs"
description: "Jak tooset až NVIDIA GPU ovladače pro N-series virtuální počítače se systémem Windows v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/07/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2acce57d4f9eb1d02bf3bc0303bcb9690475698c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a><span data-ttu-id="d7546-103">Nastavení GPU ovladače pro N-series virtuální počítače se systémem Windows Server</span><span class="sxs-lookup"><span data-stu-id="d7546-103">Set up GPU drivers for N-series VMs running Windows Server</span></span>
<span data-ttu-id="d7546-104">tootake využívat funkce GPU hello Azure N-series virtuální počítače se systémem Windows Server 2016 nebo Windows Server 2012 R2, nainstalujte podporované NVIDIA grafické ovladače.</span><span class="sxs-lookup"><span data-stu-id="d7546-104">tootake advantage of hello GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, install supported NVIDIA graphics drivers.</span></span> <span data-ttu-id="d7546-105">Tento článek obsahuje kroky instalace ovladačů po nasadit virtuální počítač s N-series.</span><span class="sxs-lookup"><span data-stu-id="d7546-105">This article provides driver setup steps after you deploy an N-series VM.</span></span> <span data-ttu-id="d7546-106">Informace o instalaci ovladačů je také k dispozici pro [virtuální počítače s Linuxem](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7546-106">Driver setup information is also available for [Linux VMs](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="d7546-107">Základní specifikace, kapacity úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačů Windows GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7546-107">For basic specs, storage capacities, and disk details, see [GPU Windows VM sizes](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a><span data-ttu-id="d7546-108">Instalace ovladače</span><span class="sxs-lookup"><span data-stu-id="d7546-108">Driver installation</span></span>

1. <span data-ttu-id="d7546-109">Připojte pomocí vzdálené plochy tooeach N-series virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d7546-109">Connect by Remote Desktop tooeach N-series VM.</span></span>

2. <span data-ttu-id="d7546-110">Stáhněte, extrahovat a nainstalujte ovladač hello podporováno pro operační systém Windows.</span><span class="sxs-lookup"><span data-stu-id="d7546-110">Download, extract, and install hello supported driver for your Windows operating system.</span></span>

<span data-ttu-id="d7546-111">Na virtuálních počítačích Azure vs je nutné provést restart po instalaci ovladačů.</span><span class="sxs-lookup"><span data-stu-id="d7546-111">On Azure NV VMs, a restart is required after driver installation.</span></span> <span data-ttu-id="d7546-112">Na virtuálních počítačích NC není nutné restartovat počítač.</span><span class="sxs-lookup"><span data-stu-id="d7546-112">On NC VMs, a restart is not required.</span></span>

## <a name="verify-driver-installation"></a><span data-ttu-id="d7546-113">Ověření instalace ovladačů</span><span class="sxs-lookup"><span data-stu-id="d7546-113">Verify driver installation</span></span>

<span data-ttu-id="d7546-114">Můžete ověřit, instalace ovladačů ve Správci zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7546-114">You can verify driver installation in Device Manager.</span></span> <span data-ttu-id="d7546-115">Hello následující příklad ukazuje úspěšná konfigurace hello tesla – měrná K80 karty ve virtuálním počítači Azure názvového kontextu.</span><span class="sxs-lookup"><span data-stu-id="d7546-115">hello following example shows successful configuration of hello Tesla K80 card on an Azure NC VM.</span></span>

![Vlastnosti ovladač GPU](./media/n-series-driver-setup/GPU_driver_properties.png)

<span data-ttu-id="d7546-117">hello tooquery GPU stavu zařízení, spusťte hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) s ovladačem hello nainstalovaný nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d7546-117">tooquery hello GPU device state, run hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with hello driver.</span></span>

1. <span data-ttu-id="d7546-118">Otevřete příkazový řádek a změňte toohello **C:\Program Files\NVIDIA Corporation\NVSMI** adresáře.</span><span class="sxs-lookup"><span data-stu-id="d7546-118">Open a command prompt and change toohello **C:\Program Files\NVIDIA Corporation\NVSMI** directory.</span></span>

2. <span data-ttu-id="d7546-119">Spustit **nvidia smi**.</span><span class="sxs-lookup"><span data-stu-id="d7546-119">Run **nvidia-smi**.</span></span> <span data-ttu-id="d7546-120">Pokud je nainstalovaný ovladač hello zobrazí se výstup podobný toobelow.</span><span class="sxs-lookup"><span data-stu-id="d7546-120">If hello driver is installed you will see output similar toobelow.</span></span> <span data-ttu-id="d7546-121">Všimněte si, že **GPU Util** ukazuje **0 %** Pokud aktuálně používáte GPU zatížení na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d7546-121">Note that **GPU-Util** shows **0%** unless you are currently running a GPU workload on hello VM.</span></span>

![Stav zařízení NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a><span data-ttu-id="d7546-123">RDMA sítě pro virtuální počítače NC24r</span><span class="sxs-lookup"><span data-stu-id="d7546-123">RDMA network for NC24r VMs</span></span>

<span data-ttu-id="d7546-124">Připojení k síti RDMA můžete povolit pro NC24r virtuálních počítačích nasazených v hello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="d7546-124">RDMA network connectivity can be enabled on NC24r VMs deployed in hello same availability set.</span></span> <span data-ttu-id="d7546-125">Hello HpcVmDrivers rozšíření je nutné přidat tooinstall ovladače zařízení sítě v systému Windows, které umožňují připojení RDMA.</span><span class="sxs-lookup"><span data-stu-id="d7546-125">hello HpcVmDrivers extension must be added tooinstall Windows network device drivers that enable RDMA connectivity.</span></span> <span data-ttu-id="d7546-126">tooadd hello virtuálního počítače rozšíření tooan NC24r virtuální počítač, použijte [prostředí Azure PowerShell](/powershell/azure/overview) rutiny pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d7546-126">tooadd hello VM extension tooan NC24r VM, use [Azure PowerShell](/powershell/azure/overview) cmdlets for Azure Resource Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="d7546-127">V současné době pouze Windows Server 2012 R2 podporuje hello RDMA sítě na virtuálních počítačích NC24r.</span><span class="sxs-lookup"><span data-stu-id="d7546-127">Currently, only Windows Server 2012 R2 supports hello RDMA network on NC24r VMs.</span></span>
> 

<span data-ttu-id="d7546-128">tooinstall hello nejnovější verze 1.1 rozšíření HpcVMDrivers na existující podporující RDMA virtuální počítač s názvem Můjvp v oblasti západní USA hello:</span><span class="sxs-lookup"><span data-stu-id="d7546-128">tooinstall hello latest version 1.1 HpcVMDrivers extension on an existing RDMA-capable VM named myVM in hello West US region:</span></span>
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  <span data-ttu-id="d7546-129">Další informace najdete v tématu [rozšíření virtuálního počítače a funkce ve Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7546-129">For more information, see [Virtual machine extensions and features for Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="d7546-130">síť Hello RDMA podporuje rozhraní MPI (Message Passing) provozu pro aplikace spuštěné s [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) nebo Intel MPI 5.x.</span><span class="sxs-lookup"><span data-stu-id="d7546-130">hello RDMA network supports Message Passing Interface (MPI) traffic for applications running with [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) or Intel MPI 5.x.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="d7546-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7546-131">Next steps</span></span>

* <span data-ttu-id="d7546-132">Další informace o hello NVIDIA grafickými procesory na hello N-series virtuálních počítačů najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="d7546-132">For more information about hello NVIDIA GPUs on hello N-series VMs, see:</span></span>
    * <span data-ttu-id="d7546-133">[Tesla – měrná K80 NVIDIA](http://www.nvidia.com/object/tesla-k80.html) (pro virtuální počítače Azure NC)</span><span class="sxs-lookup"><span data-stu-id="d7546-133">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (for Azure NC VMs)</span></span>
    * <span data-ttu-id="d7546-134">[Tesla – měrná M60 NVIDIA](http://www.nvidia.com/object/tesla-m60.html) (pro virtuální počítače Azure vs)</span><span class="sxs-lookup"><span data-stu-id="d7546-134">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (for Azure NV VMs)</span></span>

* <span data-ttu-id="d7546-135">Vývojářům tvorbu GPU accelerated aplikací pro hello NVIDIA tesla – měrná grafickými procesory můžete také stáhnout a nainstalovat hello CUDA Toolkit 8 pro [systému Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) nebo [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe).</span><span class="sxs-lookup"><span data-stu-id="d7546-135">Developers building GPU-accelerated applications for hello NVIDIA Tesla GPUs can also download and install hello CUDA Toolkit 8 for [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) or [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe).</span></span> <span data-ttu-id="d7546-136">Další informace najdete v tématu hello [Průvodce instalací CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).</span><span class="sxs-lookup"><span data-stu-id="d7546-136">For more information, see hello [CUDA Installation Guide](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).</span></span>


