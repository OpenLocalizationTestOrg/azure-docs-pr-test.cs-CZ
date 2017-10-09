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
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>Nastavení GPU ovladače pro N-series virtuální počítače se systémem Windows Server
tootake využívat funkce GPU hello Azure N-series virtuální počítače se systémem Windows Server 2016 nebo Windows Server 2012 R2, nainstalujte podporované NVIDIA grafické ovladače. Tento článek obsahuje kroky instalace ovladačů po nasadit virtuální počítač s N-series. Informace o instalaci ovladačů je také k dispozici pro [virtuální počítače s Linuxem](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Základní specifikace, kapacity úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačů Windows GPU](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a>Instalace ovladače

1. Připojte pomocí vzdálené plochy tooeach N-series virtuálních počítačů.

2. Stáhněte, extrahovat a nainstalujte ovladač hello podporováno pro operační systém Windows.

Na virtuálních počítačích Azure vs je nutné provést restart po instalaci ovladačů. Na virtuálních počítačích NC není nutné restartovat počítač.

## <a name="verify-driver-installation"></a>Ověření instalace ovladačů

Můžete ověřit, instalace ovladačů ve Správci zařízení. Hello následující příklad ukazuje úspěšná konfigurace hello tesla – měrná K80 karty ve virtuálním počítači Azure názvového kontextu.

![Vlastnosti ovladač GPU](./media/n-series-driver-setup/GPU_driver_properties.png)

hello tooquery GPU stavu zařízení, spusťte hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) s ovladačem hello nainstalovaný nástroj příkazového řádku.

1. Otevřete příkazový řádek a změňte toohello **C:\Program Files\NVIDIA Corporation\NVSMI** adresáře.

2. Spustit **nvidia smi**. Pokud je nainstalovaný ovladač hello zobrazí se výstup podobný toobelow. Všimněte si, že **GPU Util** ukazuje **0 %** Pokud aktuálně používáte GPU zatížení na hello virtuálních počítačů.

![Stav zařízení NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a>RDMA sítě pro virtuální počítače NC24r

Připojení k síti RDMA můžete povolit pro NC24r virtuálních počítačích nasazených v hello stejné skupině dostupnosti. Hello HpcVmDrivers rozšíření je nutné přidat tooinstall ovladače zařízení sítě v systému Windows, které umožňují připojení RDMA. tooadd hello virtuálního počítače rozšíření tooan NC24r virtuální počítač, použijte [prostředí Azure PowerShell](/powershell/azure/overview) rutiny pro Azure Resource Manager.

> [!NOTE]
> V současné době pouze Windows Server 2012 R2 podporuje hello RDMA sítě na virtuálních počítačích NC24r.
> 

tooinstall hello nejnovější verze 1.1 rozšíření HpcVMDrivers na existující podporující RDMA virtuální počítač s názvem Můjvp v oblasti západní USA hello:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Další informace najdete v tématu [rozšíření virtuálního počítače a funkce ve Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

síť Hello RDMA podporuje rozhraní MPI (Message Passing) provozu pro aplikace spuštěné s [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) nebo Intel MPI 5.x. 


## <a name="next-steps"></a>Další kroky

* Další informace o hello NVIDIA grafickými procesory na hello N-series virtuálních počítačů najdete v tématu:
    * [Tesla – měrná K80 NVIDIA](http://www.nvidia.com/object/tesla-k80.html) (pro virtuální počítače Azure NC)
    * [Tesla – měrná M60 NVIDIA](http://www.nvidia.com/object/tesla-m60.html) (pro virtuální počítače Azure vs)

* Vývojářům tvorbu GPU accelerated aplikací pro hello NVIDIA tesla – měrná grafickými procesory můžete také stáhnout a nainstalovat hello CUDA Toolkit 8 pro [systému Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) nebo [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe). Další informace najdete v tématu hello [Průvodce instalací CUDA](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


