---
title: "Správa virtuálních počítačů pomocí Azure PowerShell | Microsoft Docs"
description: "Další příkazy, které můžete použít k automatizaci úloh v správu virtuálních počítačů."
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
ms.openlocfilehash: fd2df7e1029ced11974d0b832258bed2cf3bbb27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a><span data-ttu-id="b23a5-103">Správa virtuálních počítačů pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="b23a5-103">Manage your virtual machines by using Azure PowerShell</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="b23a5-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b23a5-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b23a5-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="b23a5-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="b23a5-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b23a5-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="b23a5-107">Pro běžné příkazy prostředí PowerShell pomocí modelu Resource Manager, najdete v části [zde](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b23a5-107">For common PowerShell commands using the Resource Manager model, see [here](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="b23a5-108">Mnoho úloh, které se každý den ke správě virtuálních počítačů je možné automatizovat pomocí rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b23a5-108">Many tasks you do each day to manage your VMs can be automated by using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="b23a5-109">Tento článek vám příklady příkazů pro jednodušší úlohy a odkazy na články, které se zobrazí příkazy pro složitější úlohy.</span><span class="sxs-lookup"><span data-stu-id="b23a5-109">This article gives you example commands for simpler tasks, and links to articles that show the commands for more complex tasks.</span></span>

> [!NOTE]
> <span data-ttu-id="b23a5-110">Pokud ještě nainstalován a nakonfigurován prostředí Azure PowerShell ještě můžete získat pokyny v článku [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b23a5-110">If you haven't installed and configured Azure PowerShell yet, you can get instructions in the article [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
> 
> 

## <a name="how-to-use-the-example-commands"></a><span data-ttu-id="b23a5-111">Jak používat příkazy v příkladu</span><span class="sxs-lookup"><span data-stu-id="b23a5-111">How to use the example commands</span></span>
<span data-ttu-id="b23a5-112">Budete muset část textu v příkazech nahraďte text, který je vhodný pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="b23a5-112">You'll need to replace some of the text in the commands with text that's appropriate for your environment.</span></span> <span data-ttu-id="b23a5-113">< a > symboly uvést, budete muset nahraďte text.</span><span class="sxs-lookup"><span data-stu-id="b23a5-113">The < and > symbols indicate text you need to replace.</span></span> <span data-ttu-id="b23a5-114">Při nahrazení textu odebrat symboly, ale ponechte uvozovek nahoře na místě.</span><span class="sxs-lookup"><span data-stu-id="b23a5-114">When you replace the text, remove the symbols but leave the quote marks in place.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="b23a5-115">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="b23a5-115">Get a VM</span></span>
<span data-ttu-id="b23a5-116">Toto je základní úlohy, které budete používat často.</span><span class="sxs-lookup"><span data-stu-id="b23a5-116">This is a basic task you'll use often.</span></span> <span data-ttu-id="b23a5-117">Ji použijte k získání informací o virtuální počítač, provádějí úlohy na virtuálním počítači nebo získat výstup pro uložit jako proměnnou.</span><span class="sxs-lookup"><span data-stu-id="b23a5-117">Use it to get information about a VM, perform tasks on a VM, or get output to store in a variable.</span></span>

<span data-ttu-id="b23a5-118">Chcete-li získat informace o virtuálním počítači, spusťte tento příkaz, nahraďte vše v uvozovkách, včetně < a > znaky:</span><span class="sxs-lookup"><span data-stu-id="b23a5-118">To get information about the VM, run this command, replacing everything in the quotes, including the < and > characters:</span></span>

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

<span data-ttu-id="b23a5-119">Chcete-li uložit výstup do proměnné $vm, spusťte:</span><span class="sxs-lookup"><span data-stu-id="b23a5-119">To store the output in a $vm variable, run:</span></span>

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a><span data-ttu-id="b23a5-120">Přihlaste se k systému Windows virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b23a5-120">Log on to a Windows-based VM</span></span>
<span data-ttu-id="b23a5-121">Spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="b23a5-121">Run these commands:</span></span>

> [!NOTE]
> <span data-ttu-id="b23a5-122">Název virtuálního počítače a cloudové služby můžete získat z výstupu **Get-AzureVM** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b23a5-122">You can get the virtual machine and cloud service name from the display of the **Get-AzureVM** command.</span></span>
> 
> <span data-ttu-id="b23a5-123">$svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "< jednotku a složku umístění a uložte stažený soubor RDP, například: c:\temp >" $localFile = $localPath + "\" + $vmname +".rdp"Get-AzureRemoteDesktopFile - ServiceName $svcName-název $vmName - Místnícesta $localFile – spuštění</span><span class="sxs-lookup"><span data-stu-id="b23a5-123">$svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >" $localFile = $localPath + "\" + $vmname + ".rdp" Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch</span></span>
> 
> 

## <a name="stop-a-vm"></a><span data-ttu-id="b23a5-124">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b23a5-124">Stop a VM</span></span>
<span data-ttu-id="b23a5-125">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b23a5-125">Run this command:</span></span>

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> <span data-ttu-id="b23a5-126">Tento parametr použijte v případě, že je poslední virtuální počítač v rámci této cloudové služby zachovat virtuální IP (VIP) cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b23a5-126">Use this parameter to keep the virtual IP (VIP) of the cloud service in case it's the last VM in that cloud service.</span></span> <br><br> <span data-ttu-id="b23a5-127">Pokud použijete parametr StayProvisioned, stále platit budete pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b23a5-127">If you use the StayProvisioned parameter, you'll still be billed for the VM.</span></span>
> 
> 

## <a name="start-a-vm"></a><span data-ttu-id="b23a5-128">Spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b23a5-128">Start a VM</span></span>
<span data-ttu-id="b23a5-129">Spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b23a5-129">Run this command:</span></span>

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a><span data-ttu-id="b23a5-130">Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="b23a5-130">Attach a data disk</span></span>
<span data-ttu-id="b23a5-131">Tato úloha vyžaduje několik kroků.</span><span class="sxs-lookup"><span data-stu-id="b23a5-131">This task requires a few steps.</span></span> <span data-ttu-id="b23a5-132">Nejprve pomocí *** Add-AzureDataDisk *** rutiny disk přidat do objektu $vm.</span><span class="sxs-lookup"><span data-stu-id="b23a5-132">First, you use the ****Add-AzureDataDisk**** cmdlet to add the disk to the $vm object.</span></span> <span data-ttu-id="b23a5-133">Potom použít **aktualizace-AzureVM** rutiny se aktualizovat konfiguraci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b23a5-133">Then, you use **Update-AzureVM** cmdlet to update the configuration of the VM.</span></span>

<span data-ttu-id="b23a5-134">Musíte se také rozhodnout, jestli se má připojit nový disk, nebo disk, který obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="b23a5-134">You'll also need to decide whether to attach a new disk or one that contains data.</span></span> <span data-ttu-id="b23a5-135">Příkaz pro nový disk, vytvoří soubor VHD a připojí jej.</span><span class="sxs-lookup"><span data-stu-id="b23a5-135">For a new disk, the command creates the .vhd file and attaches it.</span></span>

<span data-ttu-id="b23a5-136">Pokud chcete připojit nový disk, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b23a5-136">To attach a new disk, run this command:</span></span>

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

<span data-ttu-id="b23a5-137">Pokud chcete připojit stávající datový disk, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b23a5-137">To attach an existing data disk, run this command:</span></span>

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

<span data-ttu-id="b23a5-138">Chcete-li datových disků připojit z existujícího souboru VHD v úložišti objektů blob, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b23a5-138">To attach data disks from an existing .vhd file in blob storage, run this command:</span></span>

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a><span data-ttu-id="b23a5-139">Vytvoření virtuálního počítače založené na systému Windows</span><span class="sxs-lookup"><span data-stu-id="b23a5-139">Create a Windows-based VM</span></span>
<span data-ttu-id="b23a5-140">K vytvoření nového virtuálního počítače založené na systému Windows v Azure, postupujte podle pokynů v [pomocí prostředí Azure PowerShell k vytvoření a nastavení virtuálních počítačích se systémem Windows](create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b23a5-140">To create a new Windows-based virtual machine in Azure, use the instructions in [Use Azure PowerShell to create and preconfigure Windows-based virtual machines](create-powershell.md).</span></span> <span data-ttu-id="b23a5-141">Tento postup téma vám pomůže vytvořit prostředí Azure PowerShell sady příkazů, který vytvoří virtuální počítač systému Windows, který může být předkonfigurované:</span><span class="sxs-lookup"><span data-stu-id="b23a5-141">This topic steps you through the creation of an Azure PowerShell command set that creates a Windows-based VM that can be preconfigured:</span></span>

* <span data-ttu-id="b23a5-142">S členství v doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b23a5-142">With Active Directory domain membership.</span></span>
* <span data-ttu-id="b23a5-143">S další disky.</span><span class="sxs-lookup"><span data-stu-id="b23a5-143">With additional disks.</span></span>
* <span data-ttu-id="b23a5-144">Jako člen existující Vyrovnávání zatížení sítě nastavit.</span><span class="sxs-lookup"><span data-stu-id="b23a5-144">As a member of an existing load-balanced set.</span></span>
* <span data-ttu-id="b23a5-145">Pomocí statické IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b23a5-145">With a static IP address.</span></span>

