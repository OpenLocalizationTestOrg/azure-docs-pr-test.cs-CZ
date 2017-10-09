---
title: "aaaOpen porty tooa virtuálního počítače pomocí prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak tooopen port / create tooyour koncový bod virtuálního počítače s Windows pomocí nasazení režimu hello Azure resource Manageru a prostředí Azure PowerShell"
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
ms.openlocfilehash: c1817a0c447ae4ce7a1ce2a1fc6927bedf2dacb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-and-endpoints-tooa-vm-in-azure-using-powershell"></a><span data-ttu-id="5fb85-103">Jak tooopen koncové body a porty tooa virtuálního počítače v Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5fb85-103">How tooopen ports and endpoints tooa VM in Azure using PowerShell</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="5fb85-104">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="5fb85-104">Quick commands</span></span>
<span data-ttu-id="5fb85-105">Skupina zabezpečení sítě toocreate a pravidla seznamu ACL, musíte [hello nejnovější verzi prostředí Azure PowerShell nainstalovaný](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="5fb85-105">toocreate a Network Security Group and ACL rules you need [hello latest version of Azure PowerShell installed](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="5fb85-106">Můžete také [proveďte tyto kroky, pomocí portálu Azure hello](nsg-quickstart-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5fb85-106">You can also [perform these steps using hello Azure portal](nsg-quickstart-portal.md).</span></span>

<span data-ttu-id="5fb85-107">Přihlaste se tooyour účet Azure:</span><span class="sxs-lookup"><span data-stu-id="5fb85-107">Log in tooyour Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="5fb85-108">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="5fb85-108">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="5fb85-109">Názvy parametrů příklad zahrnuté *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="5fb85-109">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="5fb85-110">Vytvořit pravidlo s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="5fb85-110">Create a rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="5fb85-111">Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* tooallow *tcp* přenosy na portu *80*:</span><span class="sxs-lookup"><span data-stu-id="5fb85-111">hello following example creates a rule named *myNetworkSecurityGroupRule* tooallow *tcp* traffic on port *80*:</span></span>

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

<span data-ttu-id="5fb85-112">Dále vytvořte skupině zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) a pravidlo přiřazení hello HTTP jste právě vytvořili, se následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5fb85-112">Next, create your Network Security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) and assign hello HTTP rule you just created as follows.</span></span> <span data-ttu-id="5fb85-113">Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="5fb85-113">hello following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

<span data-ttu-id="5fb85-114">Teď umožňuje přiřadit podsíť tooa skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="5fb85-114">Now let's assign your Network Security Group tooa subnet.</span></span> <span data-ttu-id="5fb85-115">Hello následující příklad přiřadí existující virtuální síť s názvem *myVnet* toohello proměnná *$vnet* s [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="5fb85-115">hello following example assigns an existing virtual network named *myVnet* toohello variable *$vnet* with [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

<span data-ttu-id="5fb85-116">Vaše skupina zabezpečení sítě přidružit podsíť s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="5fb85-116">Associate your Network Security Group with your subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="5fb85-117">Hello následující příklad přiřadí hello podsíť s názvem *mySubnet* s vaší skupiny zabezpečení sítě:</span><span class="sxs-lookup"><span data-stu-id="5fb85-117">hello following example associates hello subnet named *mySubnet* with your Network Security Group:</span></span>

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

<span data-ttu-id="5fb85-118">Nakonec aktualizujte virtuální sítě s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) v pořadí pro vaše změny tootake vliv:</span><span class="sxs-lookup"><span data-stu-id="5fb85-118">Finally, update your virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) in order for your changes tootake effect:</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="5fb85-119">Další informace o skupinách zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="5fb85-119">More information on Network Security Groups</span></span>
<span data-ttu-id="5fb85-120">Hello zde rychlé příkazy umožní tooget nahoru a spuštěna s průchodu tooyour přenosy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5fb85-120">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="5fb85-121">Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu tooyour prostředky.</span><span class="sxs-lookup"><span data-stu-id="5fb85-121">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="5fb85-122">Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](tutorial-virtual-network.md#manage-internal-traffic).</span><span class="sxs-lookup"><span data-stu-id="5fb85-122">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#manage-internal-traffic).</span></span>

<span data-ttu-id="5fb85-123">Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="5fb85-123">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="5fb85-124">Nástroj pro vyrovnávání zatížení Hello distribuuje provoz tooVMs ke skupině zabezpečení sítě, která poskytuje filtrování provozu.</span><span class="sxs-lookup"><span data-stu-id="5fb85-124">hello load balancer distributes traffic tooVMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="5fb85-125">Další informace najdete v tématu [jak tooload vyrovnávání Linux virtuálního počítače v Azure toocreate vysoce dostupné aplikace](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="5fb85-125">For more information, see [How tooload balance Linux virtual machines in Azure toocreate a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5fb85-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5fb85-126">Next steps</span></span>
<span data-ttu-id="5fb85-127">V tomto příkladu jste vytvořili přenosem tooallow HTTP jednoduché pravidlo.</span><span class="sxs-lookup"><span data-stu-id="5fb85-127">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="5fb85-128">Můžete najít informace o vytváření podrobnější prostředí v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="5fb85-128">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="5fb85-129">Přehled Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="5fb85-129">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="5fb85-130">Co je skupina zabezpečení sítě (NSG)?</span><span class="sxs-lookup"><span data-stu-id="5fb85-130">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="5fb85-131">Přehled Azure Resource Manageru pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="5fb85-131">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

