---
title: "Ukázka Azure skriptu prostředí PowerShell - IIS s DSC | Microsoft Docs"
description: "Ukázka Azure skriptu prostředí PowerShell - IIS s DSC"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: nepeters
ms.openlocfilehash: 38476119c88aa7d4f6578fc1e3756e11951e804a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-iis-vm-with-powershell"></a><span data-ttu-id="1a1bf-103">Vytvoření virtuálních počítačů služby IIS pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1a1bf-103">Create an IIS VM with PowerShell</span></span>

<span data-ttu-id="1a1bf-104">Tento skript vytvoří virtuální počítač Azure systémem Windows Server 2016 a pak používá k instalaci IIS rozšíření DSC virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-104">This script creates an Azure Virtual Machine running Windows Server 2016, and then uses the Azure Virtual Machine DSC Extension to install IIS.</span></span> <span data-ttu-id="1a1bf-105">Po spuštění skriptu, můžete přejít na výchozí web služby IIS na veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-105">After running the script, you can access the default IIS website on the public IP address of the virtual machine.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1a1bf-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="1a1bf-106">Sample script</span></span>

<span data-ttu-id="1a1bf-107">[!code-powershell[hlavní](../../../powershell_scripts/virtual-machine/create-vm-dsc/create-windows-vm-iis-dsc.ps1 "vytvoření virtuálních počítačů DSC služby IIS")]</span><span class="sxs-lookup"><span data-stu-id="1a1bf-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-dsc/create-windows-vm-iis-dsc.ps1 "Create VM IIS DSC")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="1a1bf-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="1a1bf-108">Clean up deployment</span></span> 

<span data-ttu-id="1a1bf-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="1a1bf-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="1a1bf-110">Script explanation</span></span>

<span data-ttu-id="1a1bf-111">Tento skript používá následující příkazy k vytvoření nasazení.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="1a1bf-112">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1a1bf-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1a1bf-113">Command</span></span> | <span data-ttu-id="1a1bf-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1a1bf-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1a1bf-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1a1bf-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="1a1bf-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1a1bf-117">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="1a1bf-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="1a1bf-118">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-118">Creates a subnet configuration.</span></span> <span data-ttu-id="1a1bf-119">Tato konfigurace se používá s procesem vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-119">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="1a1bf-120">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="1a1bf-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="1a1bf-121">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="1a1bf-122">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="1a1bf-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="1a1bf-123">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="1a1bf-124">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="1a1bf-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="1a1bf-125">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="1a1bf-126">Tato konfigurace slouží k vytvoření pravidla NSG, když je vytvořen NSG.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-126">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="1a1bf-127">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="1a1bf-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="1a1bf-128">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="1a1bf-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="1a1bf-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="1a1bf-130">Získá informace o podsíti.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-130">Gets subnet information.</span></span> <span data-ttu-id="1a1bf-131">Tato informace se používají při vytváření síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="1a1bf-132">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="1a1bf-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="1a1bf-133">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="1a1bf-134">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="1a1bf-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="1a1bf-135">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-135">Creates a VM configuration.</span></span> <span data-ttu-id="1a1bf-136">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="1a1bf-137">Konfigurace se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="1a1bf-138">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="1a1bf-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="1a1bf-139">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-139">Create a virtual machine.</span></span> |
| [<span data-ttu-id="1a1bf-140">Set-AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="1a1bf-140">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="1a1bf-141">Přidání rozšíření virtuálního počítače do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-141">Add a VM extension to the virtual machine.</span></span> <span data-ttu-id="1a1bf-142">V této ukázce se používá rozšíření vlastních skriptů instalace služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-142">In this sample, the custom script extension is used to install IIS.</span></span> |
|[<span data-ttu-id="1a1bf-143">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1a1bf-143">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="1a1bf-144">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="1a1bf-144">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1a1bf-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a1bf-145">Next steps</span></span>

<span data-ttu-id="1a1bf-146">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1a1bf-146">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1a1bf-147">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1a1bf-147">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>