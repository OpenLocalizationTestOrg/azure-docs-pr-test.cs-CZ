---
title: "Optimalizovat propustnost sítě virtuálních počítačů | Microsoft Docs"
description: "Zjistěte, jak optimalizovat propustnost sítě virtuálního počítače Azure."
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
ms.openlocfilehash: 914747983d4d974810836be66d6c6af343f58b60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a><span data-ttu-id="d2fce-103">Optimalizovat propustnost sítě pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="d2fce-103">Optimize network throughput for Azure virtual machines</span></span>

<span data-ttu-id="d2fce-104">Azure virtuální počítače (VM) mají výchozí nastavení sítě, které lze dále optimalizovat pro propustnost sítě.</span><span class="sxs-lookup"><span data-stu-id="d2fce-104">Azure virtual machines (VM) have default network settings that can be further optimized for network throughput.</span></span> <span data-ttu-id="d2fce-105">Tento článek popisuje, jak optimalizovat propustnost sítě pro Microsoft Azure Windows a virtuální počítače s Linuxem, včetně hlavních distribuce například Ubuntu a CentOS Red Hat.</span><span class="sxs-lookup"><span data-stu-id="d2fce-105">This article describes how to optimize network throughput for Microsoft Azure Windows and Linux VMs, including major distributions such as Ubuntu, CentOS and Red Hat.</span></span>

## <a name="windows-vm"></a><span data-ttu-id="d2fce-106">Virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="d2fce-106">Windows VM</span></span>

<span data-ttu-id="d2fce-107">Pokud vaše virtuální počítač s Windows je podporovaná s [Accelerated sítě](virtual-network-create-vm-accelerated-networking.md), povolení této funkce by být optimální konfigurace propustnost.</span><span class="sxs-lookup"><span data-stu-id="d2fce-107">If your Windows VM is supported with [Accelerated Networking](virtual-network-create-vm-accelerated-networking.md), enabling that feature would be the optimal configuration for throughput.</span></span> <span data-ttu-id="d2fce-108">Pro všechny ostatní virtuální počítače Windows škálování na straně příjmu (RSS) pomocí dosáhnout vyšší maximální propustnost než virtuální počítač bez RSS.</span><span class="sxs-lookup"><span data-stu-id="d2fce-108">For all other Windows VMs, using Receive Side Scaling (RSS) can reach higher maximal throughput than a VM without RSS.</span></span> <span data-ttu-id="d2fce-109">RSS může být nepovolený ve výchozím nastavení v systému Windows virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d2fce-109">RSS may be disabled by default in a Windows VM.</span></span> <span data-ttu-id="d2fce-110">Proveďte následující kroky k určení, zda je povoleno RSS a povolit, pokud je zakázána.</span><span class="sxs-lookup"><span data-stu-id="d2fce-110">Complete the following steps to determine whether RSS is enabled and to enable it if it's disabled.</span></span>

1. <span data-ttu-id="d2fce-111">Zadejte `Get-NetAdapterRss` příkaz prostředí PowerShell, které chcete zobrazit, pokud byla povolená technologie RSS pro síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="d2fce-111">Enter the `Get-NetAdapterRss` PowerShell command to see if RSS is enabled for a network adapter.</span></span> <span data-ttu-id="d2fce-112">Ve výstupu v následujícím příkladu vrácená z `Get-NetAdapterRss`, RSS není povoleno.</span><span class="sxs-lookup"><span data-stu-id="d2fce-112">In the following example output returned from the `Get-NetAdapterRss`, RSS is not enabled.</span></span>

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. <span data-ttu-id="d2fce-113">Zadejte následující příkaz, který RSS povolte:</span><span class="sxs-lookup"><span data-stu-id="d2fce-113">Enter the following command to enable RSS:</span></span>

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    <span data-ttu-id="d2fce-114">Předchozí příkaz nemá výstup.</span><span class="sxs-lookup"><span data-stu-id="d2fce-114">The previous command does not have an output.</span></span> <span data-ttu-id="d2fce-115">Příkaz změnit nastavení síťový adaptér, což by způsobilo ztrátě dočasné připojení přibližně jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="d2fce-115">The command changed NIC settings, causing temporary connectivity loss for about one minute.</span></span> <span data-ttu-id="d2fce-116">Při ztrátě připojení se zobrazí dialogové okno Reconnecting.</span><span class="sxs-lookup"><span data-stu-id="d2fce-116">A Reconnecting dialog box appears during the connectivity loss.</span></span> <span data-ttu-id="d2fce-117">Obvykle se obnoví připojení k po třetí pokus.</span><span class="sxs-lookup"><span data-stu-id="d2fce-117">Connectivity is typically restored after the third attempt.</span></span>
3. <span data-ttu-id="d2fce-118">Potvrďte, že byla povolená technologie RSS ve virtuálním počítači tak, že zadáte `Get-NetAdapterRss` příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="d2fce-118">Confirm that RSS is enabled in the VM by entering the `Get-NetAdapterRss` command again.</span></span> <span data-ttu-id="d2fce-119">V případě úspěchu se vrátí výstupu v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d2fce-119">If successful, the following example output is returned:</span></span>

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a><span data-ttu-id="d2fce-120">Virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="d2fce-120">Linux VM</span></span>

<span data-ttu-id="d2fce-121">RSS je vždy povolena ve výchozím nastavení v systému Linux virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fce-121">RSS is always enabled by default in an Azure Linux VM.</span></span> <span data-ttu-id="d2fce-122">Linux jádra vydané od ledna 2017 zahrnují nové možnosti optimalizace sítě, které umožňují virtuálního počítače s Linuxem k dosažení vyšší propustnost sítě.</span><span class="sxs-lookup"><span data-stu-id="d2fce-122">Linux kernels released since January, 2017 include new network optimization options that enable a Linux VM to achieve higher network throughput.</span></span>

### <a name="ubuntu"></a><span data-ttu-id="d2fce-123">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="d2fce-123">Ubuntu</span></span>

<span data-ttu-id="d2fce-124">Chcete-li získat optimalizace, nejprve aktualizovat na nejnovější podporovanou verzi, od června 2017, což je:</span><span class="sxs-lookup"><span data-stu-id="d2fce-124">In order to get the optimization, first update to the latest supported version, as of June 2017, which is:</span></span>
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
<span data-ttu-id="d2fce-125">Po dokončení aktualizace zadejte následující příkazy a získat nejnovější jádra:</span><span class="sxs-lookup"><span data-stu-id="d2fce-125">After the update is complete, enter the following commands to get the latest kernel:</span></span>

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

<span data-ttu-id="d2fce-126">Volitelný příkaz:</span><span class="sxs-lookup"><span data-stu-id="d2fce-126">Optional command:</span></span>

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a><span data-ttu-id="d2fce-127">Ubuntu Azure Preview jádra</span><span class="sxs-lookup"><span data-stu-id="d2fce-127">Ubuntu Azure Preview kernel</span></span>
> [!WARNING]
> <span data-ttu-id="d2fce-128">Tato jádra Azure Linux Preview nemusí mít stejnou úroveň dostupnost a spolehlivost jako obrázky Marketplace a verzí jádra, která jsou obecné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="d2fce-128">This Azure Linux Preview kernel may not have the same level of availability and reliability as Marketplace images and kernels that are in general availability release.</span></span> <span data-ttu-id="d2fce-129">Funkce není podporována, může mít omezené možnosti a nemusí být stejně spolehlivá jako výchozí jádra.</span><span class="sxs-lookup"><span data-stu-id="d2fce-129">The feature is not supported, may have constrained capabilities, and may not be as reliable as the default kernel.</span></span> <span data-ttu-id="d2fce-130">Nepoužívejte tuto jádra pro úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d2fce-130">Do not use this kernel for production workloads.</span></span>

<span data-ttu-id="d2fce-131">Nainstalováním navrhované jádra Azure Linux jde dosáhnout výrazné propustnost.</span><span class="sxs-lookup"><span data-stu-id="d2fce-131">Significant throughput performance can be achieved by installing the proposed Azure Linux kernel.</span></span> <span data-ttu-id="d2fce-132">Pokud chcete vyzkoušet tuto jádra, přidejte následující řádek na /etc/apt/sources.list</span><span class="sxs-lookup"><span data-stu-id="d2fce-132">To try this kernel, add this line to /etc/apt/sources.list</span></span>

```bash
#add this to the end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

<span data-ttu-id="d2fce-133">Potom spuštěním těchto příkazů jako kořenového příkazu.</span><span class="sxs-lookup"><span data-stu-id="d2fce-133">Then run these commands as root.</span></span>
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a><span data-ttu-id="d2fce-134">CentOS</span><span class="sxs-lookup"><span data-stu-id="d2fce-134">CentOS</span></span>

<span data-ttu-id="d2fce-135">Chcete-li získat optimalizace, nejprve aktualizovat na nejnovější podporovanou verzi, od července 2017, což je:</span><span class="sxs-lookup"><span data-stu-id="d2fce-135">In order to get the optimization, first update to the latest supported version, as of July 2017, which is:</span></span>
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
<span data-ttu-id="d2fce-136">Po dokončení aktualizace nainstalujte nejnovější Linux integrační služby (LIS).</span><span class="sxs-lookup"><span data-stu-id="d2fce-136">After the update is complete, install the latest Linux Integration Services (LIS).</span></span>
<span data-ttu-id="d2fce-137">Optimalizace propustnosti se LIS, od 4.2.2-2.</span><span class="sxs-lookup"><span data-stu-id="d2fce-137">The throughput optimization is in LIS, starting from 4.2.2-2.</span></span> <span data-ttu-id="d2fce-138">Zadejte následující příkazy pro instalaci LIS:</span><span class="sxs-lookup"><span data-stu-id="d2fce-138">Enter the following commands to install LIS:</span></span>

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a><span data-ttu-id="d2fce-139">Red Hat</span><span class="sxs-lookup"><span data-stu-id="d2fce-139">Red Hat</span></span>

<span data-ttu-id="d2fce-140">Chcete-li získat optimalizace, nejprve aktualizovat na nejnovější podporovanou verzi, od července 2017, což je:</span><span class="sxs-lookup"><span data-stu-id="d2fce-140">In order to get the optimization, first update to the latest supported version, as of July 2017, which is:</span></span>
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
<span data-ttu-id="d2fce-141">Po dokončení aktualizace nainstalujte nejnovější Linux integrační služby (LIS).</span><span class="sxs-lookup"><span data-stu-id="d2fce-141">After the update is complete, install the latest Linux Integration Services (LIS).</span></span>
<span data-ttu-id="d2fce-142">Optimalizace propustnosti se LIS, od 4.2.</span><span class="sxs-lookup"><span data-stu-id="d2fce-142">The throughput optimization is in LIS, starting from 4.2.</span></span> <span data-ttu-id="d2fce-143">Zadejte následující příkazy ke stažení a instalaci LIS:</span><span class="sxs-lookup"><span data-stu-id="d2fce-143">Enter the following commands to download and install LIS:</span></span>

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

<span data-ttu-id="d2fce-144">Další informace o Linux integrační služby verzi 4.2 pro Hyper-V zobrazením [stránce pro stažení](https://www.microsoft.com/download/details.aspx?id=55106).</span><span class="sxs-lookup"><span data-stu-id="d2fce-144">Learn more about Linux Integration Services Version 4.2 for Hyper-V by viewing the [download page](https://www.microsoft.com/download/details.aspx?id=55106).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2fce-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2fce-145">Next steps</span></span>
* <span data-ttu-id="d2fce-146">Teď, když virtuální počítač je optimalizovaná, najdete v části výsledků [šířky pásma nebo propustnost testování virtuální počítač Azure](virtual-network-bandwidth-testing.md) pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="d2fce-146">Now that the VM is optimized, see the result with [Bandwidth/Throughput testing Azure VM](virtual-network-bandwidth-testing.md) for your scenario.</span></span>
* <span data-ttu-id="d2fce-147">Další informace s [Azure Virtual Network nejčastější dotazy (FAQ)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="d2fce-147">Learn more with [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
