---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému | Microsoft Docs"
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
ms.openlocfilehash: 8ae5b14df3977a4af91b92692cb925199cfb8058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a><span data-ttu-id="cf0ef-103">Vytvoření virtuálního počítače pomocí existujícího spravovaného disku operačního systému pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cf0ef-103">Create a virtual machine using an existing managed OS disk with PowerShell</span></span>

<span data-ttu-id="cf0ef-104">Tento skript vytvoří virtuální počítač připojením existující spravované disk jako disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="cf0ef-105">Pomocí tohoto skriptu v předchozích scénáře:</span><span class="sxs-lookup"><span data-stu-id="cf0ef-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="cf0ef-106">Vytvoření virtuálního počítače z existujícího spravovaného disku operačního systému, který jste zkopírovali ze spravovaných disků v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="cf0ef-107">Vytvoření virtuálního počítače z existujícího spravovaného disku, který byl vytvořen z specializované soubor virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="cf0ef-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="cf0ef-108">Vytvoření virtuálního počítače z existujícího spravované disku operačního systému, který byl vytvořen ze snímku</span><span class="sxs-lookup"><span data-stu-id="cf0ef-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="cf0ef-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="cf0ef-109">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a><span data-ttu-id="cf0ef-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="cf0ef-110">Clean up deployment</span></span> 

<span data-ttu-id="cf0ef-111">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="cf0ef-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="cf0ef-112">Script explanation</span></span>

<span data-ttu-id="cf0ef-113">Tento skript používá hello následující vlastnosti disku tooget spravovat příkazů, připojte tooa spravovaných disků na nový virtuální počítač a vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-113">This script uses hello following commands tooget managed disk properties, attach a managed disk tooa new VM and create a VM.</span></span> <span data-ttu-id="cf0ef-114">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-114">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="cf0ef-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="cf0ef-115">Command</span></span> | <span data-ttu-id="cf0ef-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cf0ef-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cf0ef-117">Get-AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="cf0ef-117">Get-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/Get-AzureRmDisk) | <span data-ttu-id="cf0ef-118">Získá objekt disku na základě hello název a skupina prostředků hello disku.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-118">Gets disk object based on hello name and hello resource group of a disk.</span></span> <span data-ttu-id="cf0ef-119">Vlastnost ID hello vrátil objekt disku je použité tooattach hello disku tooa nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cf0ef-119">Id property of hello returned disk object is used tooattach hello disk tooa new VM</span></span> |
| [<span data-ttu-id="cf0ef-120">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="cf0ef-120">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="cf0ef-121">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-121">Creates a VM configuration.</span></span> <span data-ttu-id="cf0ef-122">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-122">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="cf0ef-123">Konfigurace Hello se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-123">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="cf0ef-124">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="cf0ef-124">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="cf0ef-125">Připojí se spravovaným diskem pomocí vlastnosti Id hello hello disk jako disk operačního systému tooa nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cf0ef-125">Attaches a managed disk using hello Id property of hello disk as OS disk tooa new virtual machine</span></span> |
| [<span data-ttu-id="cf0ef-126">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="cf0ef-126">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="cf0ef-127">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-127">Creates a public IP address.</span></span> |
| [<span data-ttu-id="cf0ef-128">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="cf0ef-128">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="cf0ef-129">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-129">Creates a network interface.</span></span> |
| [<span data-ttu-id="cf0ef-130">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="cf0ef-130">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="cf0ef-131">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-131">Create a virtual machine.</span></span> |
|[<span data-ttu-id="cf0ef-132">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cf0ef-132">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="cf0ef-133">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="cf0ef-133">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cf0ef-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cf0ef-134">Next steps</span></span>

<span data-ttu-id="cf0ef-135">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cf0ef-135">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="cf0ef-136">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cf0ef-136">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
