---
title: "Ukázka Azure skriptu prostředí PowerShell - OMS | Microsoft Docs"
description: "Ukázka Azure skriptu prostředí PowerShell - OMS"
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
ms.date: 03/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9f876be46f5214b12d6a46e54509ba3541f819c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a><span data-ttu-id="01d1c-103">Vytvoření virtuálního počítače pomocí prostředí PowerShell monitorovat služby Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="01d1c-103">Create an Operations Management Suite monitored VM with PowerShell</span></span>

<span data-ttu-id="01d1c-104">Tento skript vytvoří virtuální počítač Azure, nainstaluje agenta nástroje Operations Management Suite (OMS) a zaregistruje systém s pracovní prostor služby OMS.</span><span class="sxs-lookup"><span data-stu-id="01d1c-104">This script creates an Azure Virtual Machine, installs the Operations Management Suite (OMS) agent, and enrolls the system with an OMS workspace.</span></span> <span data-ttu-id="01d1c-105">Po spuštění skriptu se nebude zobrazovat v konzole OMS virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="01d1c-105">Once the script has run, the virtual machine will be visible in the OMS console.</span></span> <span data-ttu-id="01d1c-106">Také je potřeba aktualizovat klíč ID a pracovního prostoru pracovní prostor OMS.</span><span class="sxs-lookup"><span data-stu-id="01d1c-106">Also, you need to update the OMS workspace ID and workspace key.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="01d1c-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="01d1c-107">Sample script</span></span>

<span data-ttu-id="01d1c-108">[!code-powershell[hlavní](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-detailed-oms.ps1 "OMS vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="01d1c-108">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-detailed-oms.ps1 "Create VM OMS")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="01d1c-109">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="01d1c-109">Clean up deployment</span></span> 

<span data-ttu-id="01d1c-110">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="01d1c-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="01d1c-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="01d1c-111">Script explanation</span></span>

<span data-ttu-id="01d1c-112">Tento skript používá následující příkazy k vytvoření nasazení.</span><span class="sxs-lookup"><span data-stu-id="01d1c-112">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="01d1c-113">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="01d1c-113">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="01d1c-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="01d1c-114">Command</span></span> | <span data-ttu-id="01d1c-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="01d1c-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="01d1c-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="01d1c-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="01d1c-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="01d1c-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="01d1c-118">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="01d1c-118">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="01d1c-119">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="01d1c-119">Creates a subnet configuration.</span></span> <span data-ttu-id="01d1c-120">Tato konfigurace se používá s procesem vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="01d1c-120">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="01d1c-121">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="01d1c-121">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="01d1c-122">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="01d1c-122">Creates a virtual network.</span></span> |
| [<span data-ttu-id="01d1c-123">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="01d1c-123">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="01d1c-124">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="01d1c-124">Creates a public IP address.</span></span> |
| [<span data-ttu-id="01d1c-125">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="01d1c-125">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="01d1c-126">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="01d1c-126">Creates a network security group rule configuration.</span></span> <span data-ttu-id="01d1c-127">Tato konfigurace slouží k vytvoření pravidla NSG, když je vytvořen NSG.</span><span class="sxs-lookup"><span data-stu-id="01d1c-127">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="01d1c-128">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="01d1c-128">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="01d1c-129">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="01d1c-129">Creates a network security group.</span></span> |
| [<span data-ttu-id="01d1c-130">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="01d1c-130">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="01d1c-131">Získá informace o podsíti.</span><span class="sxs-lookup"><span data-stu-id="01d1c-131">Gets subnet information.</span></span> <span data-ttu-id="01d1c-132">Tato informace se používají při vytváření síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="01d1c-132">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="01d1c-133">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="01d1c-133">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="01d1c-134">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="01d1c-134">Creates a network interface.</span></span> |
| [<span data-ttu-id="01d1c-135">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="01d1c-135">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="01d1c-136">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="01d1c-136">Creates a VM configuration.</span></span> <span data-ttu-id="01d1c-137">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="01d1c-137">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="01d1c-138">Konfigurace se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="01d1c-138">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="01d1c-139">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="01d1c-139">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="01d1c-140">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="01d1c-140">Create a virtual machine.</span></span> |
| [<span data-ttu-id="01d1c-141">Set-AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="01d1c-141">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="01d1c-142">Přidání rozšíření virtuálního počítače do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="01d1c-142">Add a VM extension to the virtual machine.</span></span> <span data-ttu-id="01d1c-143">V takovém případě rozšíření agenta Operations Management Suite se používá pro instalaci agenta OMS a registraci virtuálních počítačů v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="01d1c-143">In this case, the Operations Management Suite agent extension is used to install the OMS agent and enroll the VM in an OMS workspace.</span></span> |
|[<span data-ttu-id="01d1c-144">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="01d1c-144">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="01d1c-145">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="01d1c-145">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="01d1c-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01d1c-146">Next steps</span></span>

<span data-ttu-id="01d1c-147">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="01d1c-147">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="01d1c-148">Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="01d1c-148">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
