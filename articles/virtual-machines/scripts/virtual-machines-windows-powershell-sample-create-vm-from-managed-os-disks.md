---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: ec532811e94647c8a04b9faf9474f6749969f83e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a><span data-ttu-id="68e3c-103">Vytvoření virtuálního počítače pomocí existujícího spravovaného disku operačního systému pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="68e3c-103">Create a virtual machine using an existing managed OS disk with PowerShell</span></span>

<span data-ttu-id="68e3c-104">Tento skript vytvoří virtuální počítač připojením existující spravované disk jako disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="68e3c-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="68e3c-105">Pomocí tohoto skriptu v předchozích scénáře:</span><span class="sxs-lookup"><span data-stu-id="68e3c-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="68e3c-106">Vytvoření virtuálního počítače z existujícího spravovaného disku operačního systému, který jste zkopírovali ze spravovaných disků v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="68e3c-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="68e3c-107">Vytvoření virtuálního počítače z existujícího spravovaného disku, který byl vytvořen z specializované soubor virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="68e3c-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="68e3c-108">Vytvoření virtuálního počítače z existujícího spravované disku operačního systému, který byl vytvořen ze snímku</span><span class="sxs-lookup"><span data-stu-id="68e3c-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="68e3c-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="68e3c-109">Sample script</span></span>

<span data-ttu-id="68e3c-110">[!code-powershell[hlavní](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "vytvoření virtuálního počítače ze snímku")]</span><span class="sxs-lookup"><span data-stu-id="68e3c-110">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="68e3c-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="68e3c-111">Clean up deployment</span></span> 

<span data-ttu-id="68e3c-112">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="68e3c-112">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="68e3c-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="68e3c-113">Script explanation</span></span>

<span data-ttu-id="68e3c-114">Tento skript používá následující příkazy získat vlastnosti spravovaných disků, připojte spravovaných disků na nový virtuální počítač a vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68e3c-114">This script uses the following commands to get managed disk properties, attach a managed disk to a new VM and create a VM.</span></span> <span data-ttu-id="68e3c-115">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="68e3c-115">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="68e3c-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="68e3c-116">Command</span></span> | <span data-ttu-id="68e3c-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="68e3c-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="68e3c-118">Get-AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="68e3c-118">Get-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/Get-AzureRmDisk) | <span data-ttu-id="68e3c-119">Získá objekt disku na základě názvu a skupině prostředků disku.</span><span class="sxs-lookup"><span data-stu-id="68e3c-119">Gets disk object based on the name and the resource group of a disk.</span></span> <span data-ttu-id="68e3c-120">Vlastnost ID objektu vrácený disku se používá k připojení disku k vytvoření nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="68e3c-120">Id property of the returned disk object is used to attach the disk to a new VM</span></span> |
| [<span data-ttu-id="68e3c-121">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="68e3c-121">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="68e3c-122">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68e3c-122">Creates a VM configuration.</span></span> <span data-ttu-id="68e3c-123">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="68e3c-123">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="68e3c-124">Konfigurace se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="68e3c-124">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="68e3c-125">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="68e3c-125">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="68e3c-126">Připojí se spravovaným diskem pomocí vlastnosti Id disku jako disk operačního systému na nový virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="68e3c-126">Attaches a managed disk using the Id property of the disk as OS disk to a new virtual machine</span></span> |
| [<span data-ttu-id="68e3c-127">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="68e3c-127">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="68e3c-128">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="68e3c-128">Creates a public IP address.</span></span> |
| [<span data-ttu-id="68e3c-129">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="68e3c-129">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="68e3c-130">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="68e3c-130">Creates a network interface.</span></span> |
| [<span data-ttu-id="68e3c-131">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="68e3c-131">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="68e3c-132">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68e3c-132">Create a virtual machine.</span></span> |
|[<span data-ttu-id="68e3c-133">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="68e3c-133">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="68e3c-134">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="68e3c-134">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="68e3c-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68e3c-135">Next steps</span></span>

<span data-ttu-id="68e3c-136">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="68e3c-136">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="68e3c-137">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="68e3c-137">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>