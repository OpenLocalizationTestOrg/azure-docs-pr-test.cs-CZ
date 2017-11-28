---
title: "aaaManage virtuálních počítačů pomocí Azure PowerShell | Microsoft Docs"
description: "Další příkazy, které můžete použít tooautomate úkoly při správě virtuálních počítačů."
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
ms.openlocfilehash: e4ca6f098519243a321eac98b6692790fe18c22c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a><span data-ttu-id="c640b-103">Správa virtuálních počítačů pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="c640b-103">Manage your virtual machines by using Azure PowerShell</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="c640b-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c640b-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c640b-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="c640b-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="c640b-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="c640b-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="c640b-107">Pro běžné příkazy prostředí PowerShell pomocí modelu Resource Manager hello, najdete v části [zde](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c640b-107">For common PowerShell commands using hello Resource Manager model, see [here](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="c640b-108">Pomocí rutin prostředí Azure PowerShell je možné automatizovat celou řadu úloh dělat každý den toomanage virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c640b-108">Many tasks you do each day toomanage your VMs can be automated by using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="c640b-109">Tento článek vám příklady příkazů pro jednodušší úlohy a tooarticles odkazy, který zobrazí hello příkazy pro složitější úlohy.</span><span class="sxs-lookup"><span data-stu-id="c640b-109">This article gives you example commands for simpler tasks, and links tooarticles that show hello commands for more complex tasks.</span></span>

> [!NOTE]
> <span data-ttu-id="c640b-110">Pokud ještě nainstalován a nakonfigurován prostředí Azure PowerShell ještě můžete získat pokyny v článku hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c640b-110">If you haven't installed and configured Azure PowerShell yet, you can get instructions in hello article [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
> 
> 

## <a name="how-toouse-hello-example-commands"></a><span data-ttu-id="c640b-111">Jak toouse hello příklady příkazů</span><span class="sxs-lookup"><span data-stu-id="c640b-111">How toouse hello example commands</span></span>
<span data-ttu-id="c640b-112">Budete potřebovat tooreplace některé z textu hello v hello příkazy pomocí text, který je vhodný pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="c640b-112">You'll need tooreplace some of hello text in hello commands with text that's appropriate for your environment.</span></span> <span data-ttu-id="c640b-113">Hello < a > symboly indikovat potřebujete tooreplace text.</span><span class="sxs-lookup"><span data-stu-id="c640b-113">hello < and > symbols indicate text you need tooreplace.</span></span> <span data-ttu-id="c640b-114">Při nahrazení textu hello odebrat hello symboly, ale ponechte hello uvozovek nahoře na místě.</span><span class="sxs-lookup"><span data-stu-id="c640b-114">When you replace hello text, remove hello symbols but leave hello quote marks in place.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="c640b-115">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="c640b-115">Get a VM</span></span>
<span data-ttu-id="c640b-116">Toto je základní úlohy, které budete používat často.</span><span class="sxs-lookup"><span data-stu-id="c640b-116">This is a basic task you'll use often.</span></span> <span data-ttu-id="c640b-117">Používat tooget informace o virtuální počítač, provádět úlohy na virtuálním počítači nebo získat výstup toostore v proměnné.</span><span class="sxs-lookup"><span data-stu-id="c640b-117">Use it tooget information about a VM, perform tasks on a VM, or get output toostore in a variable.</span></span>

<span data-ttu-id="c640b-118">tooget informace o hello virtuálního počítače, spusťte tento příkaz, přičemž vše v uvozovkách hello, včetně hello < a > znaků:</span><span class="sxs-lookup"><span data-stu-id="c640b-118">tooget information about hello VM, run this command, replacing everything in hello quotes, including hello < and > characters:</span></span>

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

<span data-ttu-id="c640b-119">hello toostore výstup do proměnné $vm, spusťte:</span><span class="sxs-lookup"><span data-stu-id="c640b-119">toostore hello output in a $vm variable, run:</span></span>

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-tooa-windows-based-vm"></a><span data-ttu-id="c640b-120">Přihlaste se tooa virtuálních počítačů na bázi Windows</span><span class="sxs-lookup"><span data-stu-id="c640b-120">Log on tooa Windows-based VM</span></span>
<span data-ttu-id="c640b-121">Spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="c640b-121">Run these commands:</span></span>

> [!NOTE]
> <span data-ttu-id="c640b-122">Hello virtuálního počítače a název cloudové služby můžete získat z hello zobrazení hello **Get-AzureVM** příkaz.</span><span class="sxs-lookup"><span data-stu-id="c640b-122">You can get hello virtual machine and cloud service name from hello display of hello **Get-AzureVM** command.</span></span>
> 
> <span data-ttu-id="c640b-123">$svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "< jednotku a složku umístění toostore hello stáhnout soubor RDP, například: c:\temp >" $localFile = $localPath + "\" + $vmname +".rdp"Get-AzureRemoteDesktopFile - ServiceName $svcName-název $vmName - Místnícesta $localFile – spuštění</span><span class="sxs-lookup"><span data-stu-id="c640b-123">$svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "<drive and folder location toostore hello downloaded RDP file, example: c:\temp >" $localFile = $localPath + "\" + $vmname + ".rdp" Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch</span></span>
> 
> 

## <a name="stop-a-vm"></a><span data-ttu-id="c640b-124">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c640b-124">Stop a VM</span></span>
<span data-ttu-id="c640b-125">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="c640b-125">Run this command:</span></span>

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> <span data-ttu-id="c640b-126">Použijte tento parametr tookeep hello virtuální IP (VIP) hello cloudu služeb v případě, že je hello poslední virtuální počítač v rámci této cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c640b-126">Use this parameter tookeep hello virtual IP (VIP) of hello cloud service in case it's hello last VM in that cloud service.</span></span> <br><br> <span data-ttu-id="c640b-127">Pokud použijete parametr StayProvisioned hello, stále platit budete pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c640b-127">If you use hello StayProvisioned parameter, you'll still be billed for hello VM.</span></span>
> 
> 

## <a name="start-a-vm"></a><span data-ttu-id="c640b-128">Spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c640b-128">Start a VM</span></span>
<span data-ttu-id="c640b-129">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="c640b-129">Run this command:</span></span>

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a><span data-ttu-id="c640b-130">Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="c640b-130">Attach a data disk</span></span>
<span data-ttu-id="c640b-131">Tato úloha vyžaduje několik kroků.</span><span class="sxs-lookup"><span data-stu-id="c640b-131">This task requires a few steps.</span></span> <span data-ttu-id="c640b-132">Nejprve pomocí hello *** Add-AzureDataDisk *** tooadd hello disku toohello $vm a jeho objekt rutiny.</span><span class="sxs-lookup"><span data-stu-id="c640b-132">First, you use hello ****Add-AzureDataDisk**** cmdlet tooadd hello disk toohello $vm object.</span></span> <span data-ttu-id="c640b-133">Potom použít **aktualizace-AzureVM** rutiny tooupdate hello konfiguraci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c640b-133">Then, you use **Update-AzureVM** cmdlet tooupdate hello configuration of hello VM.</span></span>

<span data-ttu-id="c640b-134">Budete také potřebovat toodecide zda tooattach nový disk nebo jeden, který obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="c640b-134">You'll also need toodecide whether tooattach a new disk or one that contains data.</span></span> <span data-ttu-id="c640b-135">Pro nový disk hello příkaz vytvoří soubor VHD hello a připojí jej.</span><span class="sxs-lookup"><span data-stu-id="c640b-135">For a new disk, hello command creates hello .vhd file and attaches it.</span></span>

<span data-ttu-id="c640b-136">tooattach nový disk, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="c640b-136">tooattach a new disk, run this command:</span></span>

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

<span data-ttu-id="c640b-137">tooattach stávající datový disk, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="c640b-137">tooattach an existing data disk, run this command:</span></span>

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

<span data-ttu-id="c640b-138">tooattach datové disky z existujícího souboru VHD v úložišti objektů blob, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="c640b-138">tooattach data disks from an existing .vhd file in blob storage, run this command:</span></span>

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a><span data-ttu-id="c640b-139">Vytvoření virtuálního počítače založené na systému Windows</span><span class="sxs-lookup"><span data-stu-id="c640b-139">Create a Windows-based VM</span></span>
<span data-ttu-id="c640b-140">toocreate nového systému Windows virtuálního počítače v Azure, použijte pokyny hello v [toocreate pomocí Azure PowerShell a předem nakonfigurujte virtuálních počítačích se systémem Windows](create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c640b-140">toocreate a new Windows-based virtual machine in Azure, use hello instructions in [Use Azure PowerShell toocreate and preconfigure Windows-based virtual machines](create-powershell.md).</span></span> <span data-ttu-id="c640b-141">Tento postup téma vás provede hello vytvoření prostředí Azure PowerShell sady příkazů, který vytvoří virtuální počítač systému Windows, který může být předkonfigurované:</span><span class="sxs-lookup"><span data-stu-id="c640b-141">This topic steps you through hello creation of an Azure PowerShell command set that creates a Windows-based VM that can be preconfigured:</span></span>

* <span data-ttu-id="c640b-142">S členství v doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c640b-142">With Active Directory domain membership.</span></span>
* <span data-ttu-id="c640b-143">S další disky.</span><span class="sxs-lookup"><span data-stu-id="c640b-143">With additional disks.</span></span>
* <span data-ttu-id="c640b-144">Jako člen existující Vyrovnávání zatížení sítě nastavit.</span><span class="sxs-lookup"><span data-stu-id="c640b-144">As a member of an existing load-balanced set.</span></span>
* <span data-ttu-id="c640b-145">Pomocí statické IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c640b-145">With a static IP address.</span></span>

