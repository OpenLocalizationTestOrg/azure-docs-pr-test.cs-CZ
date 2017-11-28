---
title: "propustnost sítě virtuálních počítačů aaaOptimize | Microsoft Docs"
description: "Zjistěte, jak toooptimize virtuální počítač Azure sítě propustnost."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: steveesp
ms.openlocfilehash: a5cff2d0ab6e3553c3f90d99629521a431477de0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a><span data-ttu-id="73bb8-103">Optimalizovat propustnost sítě pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="73bb8-103">Optimize network throughput for Azure virtual machines</span></span>

<span data-ttu-id="73bb8-104">Azure virtuální počítače (VM) mají výchozí nastavení sítě, které lze dále optimalizovat pro propustnost sítě.</span><span class="sxs-lookup"><span data-stu-id="73bb8-104">Azure virtual machines (VM) have default network settings that can be further optimized for network throughput.</span></span> <span data-ttu-id="73bb8-105">Tento článek popisuje, jak toooptimize propustnost sítě pro Microsoft Azure Windows a virtuální počítače s Linuxem, včetně hlavních distribuce například Ubuntu a CentOS Red Hat.</span><span class="sxs-lookup"><span data-stu-id="73bb8-105">This article describes how toooptimize network throughput for Microsoft Azure Windows and Linux VMs, including major distributions such as Ubuntu, CentOS and Red Hat.</span></span>

## <a name="windows-vm"></a><span data-ttu-id="73bb8-106">Virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="73bb8-106">Windows VM</span></span>

<span data-ttu-id="73bb8-107">Pokud vaše virtuální počítač s Windows je podporovaná s [Accelerated sítě](virtual-network-create-vm-accelerated-networking.md), povolení této funkce by být hello optimální konfigurace pro propustnost.</span><span class="sxs-lookup"><span data-stu-id="73bb8-107">If your Windows VM is supported with [Accelerated Networking](virtual-network-create-vm-accelerated-networking.md), enabling that feature would be hello optimal configuration for throughput.</span></span> <span data-ttu-id="73bb8-108">Pro všechny ostatní virtuální počítače Windows škálování na straně příjmu (RSS) pomocí dosáhnout vyšší maximální propustnost než virtuální počítač bez RSS.</span><span class="sxs-lookup"><span data-stu-id="73bb8-108">For all other Windows VMs, using Receive Side Scaling (RSS) can reach higher maximal throughput than a VM without RSS.</span></span> <span data-ttu-id="73bb8-109">RSS může být nepovolený ve výchozím nastavení v systému Windows virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="73bb8-109">RSS may be disabled by default in a Windows VM.</span></span> <span data-ttu-id="73bb8-110">Dokončete následující kroky toodetermine, zda byla povolená technologie RSS hello a tooenable ji, pokud je zakázána.</span><span class="sxs-lookup"><span data-stu-id="73bb8-110">Complete hello following steps toodetermine whether RSS is enabled and tooenable it if it's disabled.</span></span>

1. <span data-ttu-id="73bb8-111">Zadejte hello `Get-NetAdapterRss` toosee příkaz prostředí PowerShell, pokud byla povolená technologie RSS pro síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="73bb8-111">Enter hello `Get-NetAdapterRss` PowerShell command toosee if RSS is enabled for a network adapter.</span></span> <span data-ttu-id="73bb8-112">Hello následující příklad výstupu vrácených hello `Get-NetAdapterRss`, RSS není povoleno.</span><span class="sxs-lookup"><span data-stu-id="73bb8-112">In hello following example output returned from hello `Get-NetAdapterRss`, RSS is not enabled.</span></span>

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. <span data-ttu-id="73bb8-113">Zadejte následující příkaz tooenable RSS hello:</span><span class="sxs-lookup"><span data-stu-id="73bb8-113">Enter hello following command tooenable RSS:</span></span>

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    <span data-ttu-id="73bb8-114">předchozí příkaz Hello nemá výstup.</span><span class="sxs-lookup"><span data-stu-id="73bb8-114">hello previous command does not have an output.</span></span> <span data-ttu-id="73bb8-115">příkaz Hello změnit nastavení síťový adaptér, což by způsobilo ztrátě dočasné připojení přibližně jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="73bb8-115">hello command changed NIC settings, causing temporary connectivity loss for about one minute.</span></span> <span data-ttu-id="73bb8-116">Při ztrátě připojení hello se zobrazí dialogové okno Reconnecting.</span><span class="sxs-lookup"><span data-stu-id="73bb8-116">A Reconnecting dialog box appears during hello connectivity loss.</span></span> <span data-ttu-id="73bb8-117">Obvykle se obnoví připojení k po pokusu o třetí hello.</span><span class="sxs-lookup"><span data-stu-id="73bb8-117">Connectivity is typically restored after hello third attempt.</span></span>
3. <span data-ttu-id="73bb8-118">Zkontrolujte, zda je RSS povoleno v hello virtuálních počítačů tak, že zadáte hello `Get-NetAdapterRss` příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="73bb8-118">Confirm that RSS is enabled in hello VM by entering hello `Get-NetAdapterRss` command again.</span></span> <span data-ttu-id="73bb8-119">V případě úspěchu se vrátí hello následující příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="73bb8-119">If successful, hello following example output is returned:</span></span>

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a><span data-ttu-id="73bb8-120">Virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="73bb8-120">Linux VM</span></span>

<span data-ttu-id="73bb8-121">RSS je vždy povolena ve výchozím nastavení v systému Linux virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="73bb8-121">RSS is always enabled by default in an Azure Linux VM.</span></span> <span data-ttu-id="73bb8-122">Linux jádra vydané od ledna 2017 zahrnují nové možnosti optimalizace sítě, které umožňují virtuální počítač s Linuxem tooachieve vyšší propustnost sítě.</span><span class="sxs-lookup"><span data-stu-id="73bb8-122">Linux kernels released since January, 2017 include new network optimization options that enable a Linux VM tooachieve higher network throughput.</span></span>

### <a name="ubuntu"></a><span data-ttu-id="73bb8-123">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="73bb8-123">Ubuntu</span></span>

<span data-ttu-id="73bb8-124">V modulu snap-in optimalizace hello tooget pořadí aktualizace toohello podporované nejnovější verzi, od června 2017, což je:</span><span class="sxs-lookup"><span data-stu-id="73bb8-124">In order tooget hello optimization, first update toohello latest supported version, as of June 2017, which is:</span></span>
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
<span data-ttu-id="73bb8-125">Po dokončení aktualizace hello zadejte následující příkazy tooget hello nejnovější jádra hello:</span><span class="sxs-lookup"><span data-stu-id="73bb8-125">After hello update is complete, enter hello following commands tooget hello latest kernel:</span></span>

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

<span data-ttu-id="73bb8-126">Volitelný příkaz:</span><span class="sxs-lookup"><span data-stu-id="73bb8-126">Optional command:</span></span>

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a><span data-ttu-id="73bb8-127">Ubuntu Azure Preview jádra</span><span class="sxs-lookup"><span data-stu-id="73bb8-127">Ubuntu Azure Preview kernel</span></span>
> [!WARNING]
> <span data-ttu-id="73bb8-128">Tato verze Preview Linux Azure nemusí mít jádra hello stejnou úroveň dostupnost a spolehlivost jako obrázky Marketplace a jádra, která jsou obecně dostupnosti verze.</span><span class="sxs-lookup"><span data-stu-id="73bb8-128">This Azure Linux Preview kernel may not have hello same level of availability and reliability as Marketplace images and kernels that are in general availability release.</span></span> <span data-ttu-id="73bb8-129">Hello funkce nepodporuje, může mít omezené možnosti a nemusí být stejně spolehlivá jako hello výchozí jádra.</span><span class="sxs-lookup"><span data-stu-id="73bb8-129">hello feature is not supported, may have constrained capabilities, and may not be as reliable as hello default kernel.</span></span> <span data-ttu-id="73bb8-130">Nepoužívejte tuto jádra pro úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="73bb8-130">Do not use this kernel for production workloads.</span></span>

<span data-ttu-id="73bb8-131">Nainstalováním hello navrhované jádra Azure Linux jde dosáhnout výrazné propustnost.</span><span class="sxs-lookup"><span data-stu-id="73bb8-131">Significant throughput performance can be achieved by installing hello proposed Azure Linux kernel.</span></span> <span data-ttu-id="73bb8-132">tootry tento jádra, přidejte tento řádek too/etc/apt/sources.list</span><span class="sxs-lookup"><span data-stu-id="73bb8-132">tootry this kernel, add this line too/etc/apt/sources.list</span></span>

```bash
#add this toohello end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

<span data-ttu-id="73bb8-133">Potom spuštěním těchto příkazů jako kořenového příkazu.</span><span class="sxs-lookup"><span data-stu-id="73bb8-133">Then run these commands as root.</span></span>
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a><span data-ttu-id="73bb8-134">CentOS</span><span class="sxs-lookup"><span data-stu-id="73bb8-134">CentOS</span></span>

<span data-ttu-id="73bb8-135">V pořadí tooget hello optimalizace aktualizace verze toohello nejnovější podporované od července 2017, což je:</span><span class="sxs-lookup"><span data-stu-id="73bb8-135">In order tooget hello optimization, first update toohello latest supported version, as of July 2017, which is:</span></span>
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
<span data-ttu-id="73bb8-136">Po dokončení aktualizace hello hello instalovat nejnovější Linux integrační služby (LIS).</span><span class="sxs-lookup"><span data-stu-id="73bb8-136">After hello update is complete, install hello latest Linux Integration Services (LIS).</span></span>
<span data-ttu-id="73bb8-137">Optimalizace propustnosti Hello se LIS, od 4.2.2-2.</span><span class="sxs-lookup"><span data-stu-id="73bb8-137">hello throughput optimization is in LIS, starting from 4.2.2-2.</span></span> <span data-ttu-id="73bb8-138">Zadejte následující příkazy tooinstall LIS hello:</span><span class="sxs-lookup"><span data-stu-id="73bb8-138">Enter hello following commands tooinstall LIS:</span></span>

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a><span data-ttu-id="73bb8-139">Red Hat</span><span class="sxs-lookup"><span data-stu-id="73bb8-139">Red Hat</span></span>

<span data-ttu-id="73bb8-140">V pořadí tooget hello optimalizace aktualizace verze toohello nejnovější podporované od července 2017, což je:</span><span class="sxs-lookup"><span data-stu-id="73bb8-140">In order tooget hello optimization, first update toohello latest supported version, as of July 2017, which is:</span></span>
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
<span data-ttu-id="73bb8-141">Po dokončení aktualizace hello hello instalovat nejnovější Linux integrační služby (LIS).</span><span class="sxs-lookup"><span data-stu-id="73bb8-141">After hello update is complete, install hello latest Linux Integration Services (LIS).</span></span>
<span data-ttu-id="73bb8-142">Optimalizace propustnosti Hello se LIS, od 4.2.</span><span class="sxs-lookup"><span data-stu-id="73bb8-142">hello throughput optimization is in LIS, starting from 4.2.</span></span> <span data-ttu-id="73bb8-143">Zadejte následující příkazy toodownload hello a instalovat:</span><span class="sxs-lookup"><span data-stu-id="73bb8-143">Enter hello following commands toodownload and install LIS:</span></span>

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

<span data-ttu-id="73bb8-144">Další informace o Linux integrační služby verzi 4.2 pro Hyper-V zobrazením hello [stránce pro stažení](https://www.microsoft.com/download/details.aspx?id=55106).</span><span class="sxs-lookup"><span data-stu-id="73bb8-144">Learn more about Linux Integration Services Version 4.2 for Hyper-V by viewing hello [download page](https://www.microsoft.com/download/details.aspx?id=55106).</span></span>

## <a name="next-steps"></a><span data-ttu-id="73bb8-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73bb8-145">Next steps</span></span>
* <span data-ttu-id="73bb8-146">Teď, když hello virtuálního počítače je optimalizovaná, najdete v části hello výsledek [šířky pásma nebo propustnost testování virtuální počítač Azure](virtual-network-bandwidth-testing.md) pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="73bb8-146">Now that hello VM is optimized, see hello result with [Bandwidth/Throughput testing Azure VM](virtual-network-bandwidth-testing.md) for your scenario.</span></span>
* <span data-ttu-id="73bb8-147">Další informace s [Azure Virtual Network nejčastější dotazy (FAQ)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="73bb8-147">Learn more with [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
