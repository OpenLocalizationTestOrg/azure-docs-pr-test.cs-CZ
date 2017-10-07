---
title: "aaaAzure ukázkový skript prostředí PowerShell - OMS | Microsoft Docs"
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
ms.openlocfilehash: 1eeafbe743013e97bf3fcefb5ce87f72cb503a4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a><span data-ttu-id="6e9fc-103">Vytvoření virtuálního počítače pomocí prostředí PowerShell monitorovat služby Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="6e9fc-103">Create an Operations Management Suite monitored VM with PowerShell</span></span>

<span data-ttu-id="6e9fc-104">Tento skript vytvoří virtuální počítač Azure, nainstaluje hello agenta Operations Management Suite (OMS) a zaregistruje hello systém s pracovní prostor služby OMS.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-104">This script creates an Azure Virtual Machine, installs hello Operations Management Suite (OMS) agent, and enrolls hello system with an OMS workspace.</span></span> <span data-ttu-id="6e9fc-105">Po spuštění skriptu hello se nebude zobrazovat v konzole OMS hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-105">Once hello script has run, hello virtual machine will be visible in hello OMS console.</span></span> <span data-ttu-id="6e9fc-106">Navíc musíte tooupdate hello OMS klíč pracovního prostoru ID a pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-106">Also, you need tooupdate hello OMS workspace ID and workspace key.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="6e9fc-107">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="6e9fc-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-detailed-oms.ps1 "Create VM OMS")]

## <a name="clean-up-deployment"></a><span data-ttu-id="6e9fc-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="6e9fc-108">Clean up deployment</span></span> 

<span data-ttu-id="6e9fc-109">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="6e9fc-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="6e9fc-110">Script explanation</span></span>

<span data-ttu-id="6e9fc-111">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-111">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="6e9fc-112">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-112">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="6e9fc-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6e9fc-113">Command</span></span> | <span data-ttu-id="6e9fc-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6e9fc-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6e9fc-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="6e9fc-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="6e9fc-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6e9fc-117">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="6e9fc-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="6e9fc-118">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-118">Creates a subnet configuration.</span></span> <span data-ttu-id="6e9fc-119">Tato konfigurace se používá s procesem vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-119">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="6e9fc-120">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="6e9fc-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="6e9fc-121">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="6e9fc-122">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="6e9fc-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="6e9fc-123">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="6e9fc-124">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="6e9fc-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="6e9fc-125">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="6e9fc-126">Tato konfigurace je použité toocreate pravidlo NSG při vytvoření hello NSG.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-126">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="6e9fc-127">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="6e9fc-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="6e9fc-128">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="6e9fc-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="6e9fc-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="6e9fc-130">Získá informace o podsíti.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-130">Gets subnet information.</span></span> <span data-ttu-id="6e9fc-131">Tato informace se používají při vytváření síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="6e9fc-132">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="6e9fc-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="6e9fc-133">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="6e9fc-134">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="6e9fc-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="6e9fc-135">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-135">Creates a VM configuration.</span></span> <span data-ttu-id="6e9fc-136">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="6e9fc-137">Konfigurace Hello se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-137">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="6e9fc-138">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="6e9fc-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="6e9fc-139">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-139">Create a virtual machine.</span></span> |
| [<span data-ttu-id="6e9fc-140">Set-AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="6e9fc-140">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="6e9fc-141">Přidáte virtuální počítač toohello rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-141">Add a VM extension toohello virtual machine.</span></span> <span data-ttu-id="6e9fc-142">V takovém případě hello rozšíření agenta Operations Management Suite je agent OMS hello použité tooinstall a zaregistrovat hello virtuálních počítačů v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-142">In this case, hello Operations Management Suite agent extension is used tooinstall hello OMS agent and enroll hello VM in an OMS workspace.</span></span> |
|[<span data-ttu-id="6e9fc-143">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="6e9fc-143">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="6e9fc-144">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="6e9fc-144">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6e9fc-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e9fc-145">Next steps</span></span>

<span data-ttu-id="6e9fc-146">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6e9fc-146">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="6e9fc-147">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6e9fc-147">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
