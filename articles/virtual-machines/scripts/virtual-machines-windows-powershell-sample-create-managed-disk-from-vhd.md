---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření spravovaného disku ze souboru VHD v účtu úložiště v předplatném stejný nebo jiný | Microsoft Docs"
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
ms.openlocfilehash: 47acff274cdf79d6fc3cd685cda01cad3d14ca8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a><span data-ttu-id="1bf63-103">Vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejné nebo jiné předplatné pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1bf63-103">Create a managed disk from a VHD file in a storage account in same or different subscription with PowerShell</span></span>

<span data-ttu-id="1bf63-104">Tento skript vytvoří se spravovaným diskem z soubor virtuálního pevného disku v účtu úložiště ve stejné nebo jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="1bf63-104">This script creates a managed disk from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="1bf63-105">Použijte tento skript tooimport specializované (není zobecněný/Sysprep) virtuálního pevného disku toomanaged operačního systému disku toocreate virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1bf63-105">Use this script tooimport a specialized (not generalized/sysprepped) VHD toomanaged OS disk toocreate a virtual machine.</span></span> <span data-ttu-id="1bf63-106">Ji použijte i tooimport data virtuálního pevného disku toomanaged datový disk.</span><span class="sxs-lookup"><span data-stu-id="1bf63-106">Also, use it tooimport a data VHD toomanaged data disk.</span></span> 

<span data-ttu-id="1bf63-107">Nevytvářejte více identické spravovaných disků ze souboru virtuálního pevného disku v malé množství času.</span><span class="sxs-lookup"><span data-stu-id="1bf63-107">Don't create multiple identical managed disks from a VHD file in small amount of time.</span></span> <span data-ttu-id="1bf63-108">toocreate spravovat disky ze souboru virtuálního pevného disku, vytvoření snímku objektu blob souboru vhd hello a pak je použité toocreate spravované disky.</span><span class="sxs-lookup"><span data-stu-id="1bf63-108">toocreate managed disks from a vhd file, blob snapshot of hello vhd file is created and then it is used toocreate managed disks.</span></span> <span data-ttu-id="1bf63-109">Můžete vytvořit pouze jeden objekt blob snímek za minutu, která způsobuje chyby při vytváření disku kvůli toothrottling.</span><span class="sxs-lookup"><span data-stu-id="1bf63-109">Only one blob snapshot can be created in a minute that causes disk creation failures due toothrottling.</span></span> <span data-ttu-id="1bf63-110">tooavoid toto omezení, vytvořit [spravované snímku ze souboru virtuálního pevného disku hello](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) a potom použijte hello spravovat snímku toocreate více spravovaných disků v krátkém čase.</span><span class="sxs-lookup"><span data-stu-id="1bf63-110">tooavoid this throttling, create a [managed snapshot from hello vhd file](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) and then use hello managed snapshot toocreate multiple managed disks in short amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1bf63-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="1bf63-111">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a><span data-ttu-id="1bf63-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="1bf63-112">Script explanation</span></span>

<span data-ttu-id="1bf63-113">Tento skript používá tyto příkazy toocreate spravovaného disku z disku VHD v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="1bf63-113">This script uses following commands toocreate a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="1bf63-114">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1bf63-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1bf63-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1bf63-115">Command</span></span> | <span data-ttu-id="1bf63-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1bf63-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1bf63-117">Nové AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="1bf63-117">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="1bf63-118">Vytvoří konfigurace disku, který slouží k vytváření disků.</span><span class="sxs-lookup"><span data-stu-id="1bf63-118">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="1bf63-119">Obsahuje typ úložiště, umístění, prostředků Id hello účtu úložiště uloží hello nadřazený virtuální pevný disk, identifikátor URI virtuálního pevného disku hello nadřazený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="1bf63-119">It includes storage type, location, resource Id of hello storage account where hello parent VHD is stored, VHD URI of hello parent VHD.</span></span> |
| [<span data-ttu-id="1bf63-120">Nové AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="1bf63-120">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="1bf63-121">Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="1bf63-121">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1bf63-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1bf63-122">Next steps</span></span>

[<span data-ttu-id="1bf63-123">Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="1bf63-123">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="1bf63-124">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1bf63-124">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1bf63-125">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1bf63-125">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
