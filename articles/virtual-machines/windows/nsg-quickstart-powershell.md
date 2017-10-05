---
title: "Otevřete porty, které se virtuální počítač pomocí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak otevřít port / create koncového bodu váš virtuální počítač s Windows pomocí režimu nasazení Azure resource Manageru a prostředí Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: e818e3b3c707e1471d6f580f8379a277d3575b89
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a><span data-ttu-id="bb628-103">Postup otevření portů a koncové body k virtuálnímu počítači v Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="bb628-103">How to open ports and endpoints to a VM in Azure using PowerShell</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="bb628-104">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="bb628-104">Quick commands</span></span>
<span data-ttu-id="bb628-105">Vytvořte skupinu zabezpečení sítě a seznamu ACL pravidel potřebujete [nejnovější verzi prostředí Azure PowerShell nainstalovaný](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="bb628-105">To create a Network Security Group and ACL rules you need [the latest version of Azure PowerShell installed](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="bb628-106">Můžete také [proveďte tyto kroky, pomocí webu Azure portal](nsg-quickstart-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bb628-106">You can also [perform these steps using the Azure portal](nsg-quickstart-portal.md).</span></span>

<span data-ttu-id="bb628-107">Přihlaste se k účtu Azure:</span><span class="sxs-lookup"><span data-stu-id="bb628-107">Log in to your Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="bb628-108">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bb628-108">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="bb628-109">Názvy parametrů příklad zahrnuté *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="bb628-109">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="bb628-110">Vytvořit pravidlo s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="bb628-110">Create a rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="bb628-111">Následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* umožňující *tcp* přenosy na portu *80*:</span><span class="sxs-lookup"><span data-stu-id="bb628-111">The following example creates a rule named *myNetworkSecurityGroupRule* to allow *tcp* traffic on port *80*:</span></span>

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

<span data-ttu-id="bb628-112">Dále vytvořte skupině zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) a přiřaďte pravidlo HTTP jste právě vytvořili, se následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="bb628-112">Next, create your Network Security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) and assign the HTTP rule you just created as follows.</span></span> <span data-ttu-id="bb628-113">Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="bb628-113">The following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

<span data-ttu-id="bb628-114">Teď umožňuje přiřadit skupině zabezpečení sítě k podsíti.</span><span class="sxs-lookup"><span data-stu-id="bb628-114">Now let's assign your Network Security Group to a subnet.</span></span> <span data-ttu-id="bb628-115">Následující příklad přiřadí existující virtuální síť s názvem *myVnet* proměnnou *$vnet* s [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="bb628-115">The following example assigns an existing virtual network named *myVnet* to the variable *$vnet* with [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

<span data-ttu-id="bb628-116">Vaše skupina zabezpečení sítě přidružit podsíť s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="bb628-116">Associate your Network Security Group with your subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="bb628-117">Následující příklad přiřadí podsíť s názvem *mySubnet* s vaší skupiny zabezpečení sítě:</span><span class="sxs-lookup"><span data-stu-id="bb628-117">The following example associates the subnet named *mySubnet* with your Network Security Group:</span></span>

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

<span data-ttu-id="bb628-118">Nakonec aktualizujte virtuální sítě s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) aby změny vstoupily v platnost:</span><span class="sxs-lookup"><span data-stu-id="bb628-118">Finally, update your virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) in order for your changes to take effect:</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="bb628-119">Další informace o skupinách zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="bb628-119">More information on Network Security Groups</span></span>
<span data-ttu-id="bb628-120">Rychlé příkazy umožňují zprovoznění s provoz do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bb628-120">The quick commands here allow you to get up and running with traffic flowing to your VM.</span></span> <span data-ttu-id="bb628-121">Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="bb628-121">Network Security Groups provide many great features and granularity for controlling access to your resources.</span></span> <span data-ttu-id="bb628-122">Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](tutorial-virtual-network.md#manage-internal-traffic).</span><span class="sxs-lookup"><span data-stu-id="bb628-122">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#manage-internal-traffic).</span></span>

<span data-ttu-id="bb628-123">Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="bb628-123">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="bb628-124">Nástroje pro vyrovnávání zatížení distribuuje provoz do virtuálních počítačů s skupinu zabezpečení sítě, která poskytuje filtrování provozu.</span><span class="sxs-lookup"><span data-stu-id="bb628-124">The load balancer distributes traffic to VMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="bb628-125">Další informace najdete v tématu [jak načíst vyvážit virtuální počítače s Linuxem v Azure k vytvoření vysoce dostupné aplikace](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="bb628-125">For more information, see [How to load balance Linux virtual machines in Azure to create a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb628-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bb628-126">Next steps</span></span>
<span data-ttu-id="bb628-127">V tomto příkladu jste vytvořili jednoduché pravidlo umožňující přenos HTTP.</span><span class="sxs-lookup"><span data-stu-id="bb628-127">In this example, you created a simple rule to allow HTTP traffic.</span></span> <span data-ttu-id="bb628-128">Můžete najít informace o vytváření podrobnější prostředí v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="bb628-128">You can find information on creating more detailed environments in the following articles:</span></span>

* [<span data-ttu-id="bb628-129">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="bb628-129">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="bb628-130">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="bb628-130">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="bb628-131">Přehled Azure Resource Manageru pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="bb628-131">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

