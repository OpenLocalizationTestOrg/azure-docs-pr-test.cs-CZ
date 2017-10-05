---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření spravovaného disku ze souboru VHD v účtu úložiště v předplatném stejný nebo jiný | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejné nebo jiné předplatné"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 728def40a3eb132537decbd099fa71f4544c6b87
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a><span data-ttu-id="1d56e-103">Vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejné nebo jiné předplatné pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d56e-103">Create a managed disk from a VHD file in a storage account in same or different subscription with PowerShell</span></span>

<span data-ttu-id="1d56e-104">Tento skript vytvoří se spravovaným diskem z soubor virtuálního pevného disku v účtu úložiště ve stejné nebo jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="1d56e-104">This script creates a managed disk from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="1d56e-105">Pomocí tohoto skriptu k importu specializované (není zobecněný/Sysprep) virtuálního pevného disku na disk spravovaný operačního systému k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1d56e-105">Use this script to import a specialized (not generalized/sysprepped) VHD to managed OS disk to create a virtual machine.</span></span> <span data-ttu-id="1d56e-106">Navíc můžete importovat data virtuálního pevného disku na disk spravovaný data.</span><span class="sxs-lookup"><span data-stu-id="1d56e-106">Also, use it to import a data VHD to managed data disk.</span></span> 

<span data-ttu-id="1d56e-107">Nevytvářejte více identické spravovaných disků ze souboru virtuálního pevného disku v malé množství času.</span><span class="sxs-lookup"><span data-stu-id="1d56e-107">Don't create multiple identical managed disks from a VHD file in small amount of time.</span></span> <span data-ttu-id="1d56e-108">Vytvoření spravované disky z soubor virtuálního pevného disku, je vytvořit snímek objektu blob souboru vhd a pak se používá k vytvoření spravovaného disky.</span><span class="sxs-lookup"><span data-stu-id="1d56e-108">To create managed disks from a vhd file, blob snapshot of the vhd file is created and then it is used to create managed disks.</span></span> <span data-ttu-id="1d56e-109">Za minutu, která způsobuje chyby při vytváření disku z důvodu omezení lze vytvořit pouze jeden objekt blob snímku.</span><span class="sxs-lookup"><span data-stu-id="1d56e-109">Only one blob snapshot can be created in a minute that causes disk creation failures due to throttling.</span></span> <span data-ttu-id="1d56e-110">Nechcete-li toto omezení, vytvořit [spravované snímku ze souboru virtuálního pevného disku](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) a potom pomocí spravovaný spravované snímek, který chcete vytvořit několik disků v krátkém množství času.</span><span class="sxs-lookup"><span data-stu-id="1d56e-110">To avoid this throttling, create a [managed snapshot from the vhd file](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) and then use the managed snapshot to create multiple managed disks in short amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1d56e-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="1d56e-111">Sample script</span></span>

<span data-ttu-id="1d56e-112">[!code-powershell[hlavní](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "vytvořit spravovaného disku z virtuálního pevného disku")]</span><span class="sxs-lookup"><span data-stu-id="1d56e-112">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="1d56e-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="1d56e-113">Script explanation</span></span>

<span data-ttu-id="1d56e-114">Tento skript používá následující příkazy k vytvoření spravovaného disku z disku VHD v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="1d56e-114">This script uses following commands to create a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="1d56e-115">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="1d56e-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1d56e-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1d56e-116">Command</span></span> | <span data-ttu-id="1d56e-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1d56e-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1d56e-118">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="1d56e-118">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="1d56e-119">Vytvoří konfigurace disku, který slouží k vytváření disků.</span><span class="sxs-lookup"><span data-stu-id="1d56e-119">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="1d56e-120">Obsahuje typ úložiště, umístění, prostředek Id účtu úložiště, kde je uložený nadřazený virtuální pevný disk, identifikátor URI virtuálního pevného disku nadřazený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="1d56e-120">It includes storage type, location, resource Id of the storage account where the parent VHD is stored, VHD URI of the parent VHD.</span></span> |
| [<span data-ttu-id="1d56e-121">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="1d56e-121">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="1d56e-122">Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="1d56e-122">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1d56e-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d56e-123">Next steps</span></span>

[<span data-ttu-id="1d56e-124">Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="1d56e-124">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="1d56e-125">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1d56e-125">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1d56e-126">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1d56e-126">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>