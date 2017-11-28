---
title: "aaaTroubleshoot skupin zabezpečení sítě - prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot skupin zabezpečení sítě v nasazení Azure Resource Manager hello modelu pomocí Azure PowerShell."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 4c732bb7-5cb1-40af-9e6d-a2a307c2a9c4
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 95fd60fa72cf6d17fa990e3c3eb7d980878f7c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a><span data-ttu-id="3bd54-103">Řešení potíží s skupin zabezpečení sítě pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3bd54-103">Troubleshoot Network Security Groups using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3bd54-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3bd54-104">Azure Portal</span></span>](virtual-network-nsg-troubleshoot-portal.md)
> * [<span data-ttu-id="3bd54-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3bd54-105">PowerShell</span></span>](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

<span data-ttu-id="3bd54-106">Pokud jste nakonfigurovali skupiny zabezpečení sítě (Nsg) ve virtuálním počítači (VM) a dochází problémy s připojením k virtuální počítač, tento článek obsahuje přehled možností diagnostiky pro řešení potíží s toohelp další skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="3bd54-106">If you configured Network Security Groups (NSGs) on your virtual machine (VM) and are experiencing VM connectivity issues, this article provides an overview of diagnostics capabilities for NSGs toohelp troubleshoot further.</span></span>

<span data-ttu-id="3bd54-107">Skupiny Nsg umožňují toocontrol hello typy přenosů s tokem do aplikace a z virtuálních počítačů (VM).</span><span class="sxs-lookup"><span data-stu-id="3bd54-107">NSGs enable you toocontrol hello types of traffic that flow in and out of your virtual machines (VMs).</span></span> <span data-ttu-id="3bd54-108">Skupiny Nsg můžou být použité toosubnets ve virtuální síti Azure (VNet), síťová rozhraní (NIC) nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="3bd54-108">NSGs can be applied toosubnets in an Azure Virtual Network (VNet), network interfaces (NIC), or both.</span></span> <span data-ttu-id="3bd54-109">Hello efektivní pravidla použít tooa síťový adaptér jsou hello pravidla, které existují v hello skupiny Nsg použije tooa síťových Adaptérů a hello podsíť, ve které je připojený k agregaci.</span><span class="sxs-lookup"><span data-stu-id="3bd54-109">hello effective rules applied tooa NIC are an aggregation of hello rules that exist in hello NSGs applied tooa NIC and hello subnet it is connected to.</span></span> <span data-ttu-id="3bd54-110">Pravidla v rámci těchto skupin Nsg můžete někdy vzájemném konfliktu a mít vliv na připojení k síti Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3bd54-110">Rules across these NSGs can sometimes conflict with each other and impact a VM's network connectivity.</span></span>  

<span data-ttu-id="3bd54-111">Můžete zobrazit všechna pravidla efektivní zabezpečení hello od vaší skupiny Nsg použije na síťové adaptéry Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3bd54-111">You can view all hello effective security rules from your NSGs, as applied on your VM's NICs.</span></span> <span data-ttu-id="3bd54-112">Tento článek ukazuje, jak hello problémy s připojením k tootroubleshoot virtuálních počítačů pomocí těchto pravidel v modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3bd54-112">This article shows how tootroubleshoot VM connectivity issues using these rules in hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="3bd54-113">Pokud si nejste obeznámeni s virtuální sítí a NSG koncepty, přečtěte si hello [virtuální síť](virtual-networks-overview.md) a [skupin zabezpečení sítě](virtual-networks-nsg.md) přehled články.</span><span class="sxs-lookup"><span data-stu-id="3bd54-113">If you're not familiar with VNet and NSG concepts, read hello [Virtual network](virtual-networks-overview.md) and [Network security groups](virtual-networks-nsg.md) overview articles.</span></span>

## <a name="using-effective-security-rules-tootroubleshoot-vm-traffic-flow"></a><span data-ttu-id="3bd54-114">Pomocí tok přenosů dat virtuálního počítače tootroubleshoot efektivní pravidla zabezpečení</span><span class="sxs-lookup"><span data-stu-id="3bd54-114">Using Effective Security Rules tootroubleshoot VM traffic flow</span></span>
<span data-ttu-id="3bd54-115">Hello scénář, který následuje je příkladem častých problémů připojení:</span><span class="sxs-lookup"><span data-stu-id="3bd54-115">hello scenario that follows is an example of a common connection problem:</span></span>

<span data-ttu-id="3bd54-116">Virtuální počítač s názvem *VM1* je součástí podsíť s názvem *Subnet1* v rámci virtuální sítě s názvem *WestUS VNet1*.</span><span class="sxs-lookup"><span data-stu-id="3bd54-116">A VM named *VM1* is part of a subnet named *Subnet1* within a VNet named *WestUS-VNet1*.</span></span> <span data-ttu-id="3bd54-117">Pokusu o tooconnect toohello virtuálního počítače pomocí protokolu RDP přes TCP port 3389 selže.</span><span class="sxs-lookup"><span data-stu-id="3bd54-117">An attempt tooconnect toohello VM using RDP over TCP port 3389 fails.</span></span> <span data-ttu-id="3bd54-118">Skupiny Nsg se použijí v obou hello seskupování *VM1 NIC1* a hello podsíť *Subnet1*.</span><span class="sxs-lookup"><span data-stu-id="3bd54-118">NSGs are applied at both hello NIC *VM1-NIC1* and hello subnet *Subnet1*.</span></span> <span data-ttu-id="3bd54-119">V hello NSG přidružená hello síťové rozhraní je povolen provoz tooTCP portu 3389 *VM1 NIC1*, ale TCP příkazem ping otestovat tooVM1 na portu 3389 selže.</span><span class="sxs-lookup"><span data-stu-id="3bd54-119">Traffic tooTCP port 3389 is allowed in hello NSG associated with hello network interface *VM1-NIC1*, however TCP ping tooVM1's port 3389 fails.</span></span>

<span data-ttu-id="3bd54-120">Při tomto příkladu používá TCP port 3389, hello následující postup se dá použít toodetermine selhání příchozí a odchozí připojení prostřednictvím libovolný port.</span><span class="sxs-lookup"><span data-stu-id="3bd54-120">While this example uses TCP port 3389, hello following steps can be used toodetermine inbound and outbound connection failures over any port.</span></span>

## <a name="detailed-troubleshooting-steps"></a><span data-ttu-id="3bd54-121">Podrobné kroky řešení potíží</span><span class="sxs-lookup"><span data-stu-id="3bd54-121">Detailed Troubleshooting Steps</span></span>
<span data-ttu-id="3bd54-122">Proveďte následující kroky tootroubleshoot skupiny Nsg pro virtuální počítač hello:</span><span class="sxs-lookup"><span data-stu-id="3bd54-122">Complete hello following steps tootroubleshoot NSGs for a VM:</span></span>

1. <span data-ttu-id="3bd54-123">Spusťte tooAzure relaci a přihlaste se prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3bd54-123">Start an Azure PowerShell session and login tooAzure.</span></span> <span data-ttu-id="3bd54-124">Pokud si nejste obeznámeni s používáním Azure PowerShell, přečtěte si hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="3bd54-124">If you're not familiar with using Azure PowerShell, read hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="3bd54-125">Zadejte následující příkaz tooreturn všechna pravidla NSG použije tooa síťový adaptér s názvem hello *VM1 NIC1* ve skupině prostředků hello *RG1*:</span><span class="sxs-lookup"><span data-stu-id="3bd54-125">Enter hello following command tooreturn all NSG rules applied tooa NIC named *VM1-NIC1* in hello resource group *RG1*:</span></span>
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > <span data-ttu-id="3bd54-126">Pokud neznáte název hello síťového adaptéru, zadejte následující příkaz tooretrieve hello názvy všech síťových adaptérů ve skupině prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="3bd54-126">If you don't know hello name of a NIC, enter hello following command tooretrieve hello names of all NICs in a resource group:</span></span> 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    <span data-ttu-id="3bd54-127">Hello následující text je ukázkový výstup hello efektivní pravidla pro hello *VM1 NIC1* síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="3bd54-127">hello following text is a sample of hello effective rules output returned for hello *VM1-NIC1* NIC:</span></span>
   
        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
   
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]
   
    <span data-ttu-id="3bd54-128">Vezměte na vědomí následující informace ve výstupu hello hello:</span><span class="sxs-lookup"><span data-stu-id="3bd54-128">Note hello following information in hello output:</span></span>
   
   * <span data-ttu-id="3bd54-129">Existují dva **skupinu zabezpečení sítě** částech: jeden je přidružená k podsíti (*Subnet1*) a jeden síťový adaptér přidružený (*VM1 NIC1*).</span><span class="sxs-lookup"><span data-stu-id="3bd54-129">There are two **NetworkSecurityGroup** sections: One is associated with a subnet (*Subnet1*) and one is associated with a NIC (*VM1-NIC1*).</span></span> <span data-ttu-id="3bd54-130">V tomto příkladu byla skupina NSG použitá tooeach.</span><span class="sxs-lookup"><span data-stu-id="3bd54-130">In this example, an NSG has been applied tooeach.</span></span>
   * <span data-ttu-id="3bd54-131">**Přidružení** ukazuje hello prostředků (podsíti nebo síťové KARTĚ) je přidružen daný NSG.</span><span class="sxs-lookup"><span data-stu-id="3bd54-131">**Association** shows hello resource (subnet or NIC) a given NSG is associated with.</span></span> <span data-ttu-id="3bd54-132">Pokud hello prostředek NSG přesunout nebo zrušit přidružení bezprostředně před spuštěním tohoto příkazu, může být nutné toowait pár sekund pro změnu tooreflect hello ve výstupu příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="3bd54-132">If hello NSG resource is moved/disassociated immediately before running this command, you may need toowait a few seconds for hello change tooreflect in hello command output.</span></span> 
   * <span data-ttu-id="3bd54-133">Hello názvy pravidel, které začínají *defaultSecurityRules*: když NSG se vytvoří, jsou v něm vytvořit několik výchozí pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3bd54-133">hello rule names that are prefaced with *defaultSecurityRules*: When an NSG is created, several default security rules are created within it.</span></span> <span data-ttu-id="3bd54-134">Výchozí pravidla nelze odebrat, ale je možné přepsat s vyšší prioritou pravidla.</span><span class="sxs-lookup"><span data-stu-id="3bd54-134">Default rules can't be removed, but they can be overridden with higher priority rules.</span></span>
     <span data-ttu-id="3bd54-135">Čtení hello [NSG přehled](virtual-networks-nsg.md#default-rules) článku toolearn Další informace o NSG výchozí pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3bd54-135">Read hello [NSG overview](virtual-networks-nsg.md#default-rules) article toolearn more about NSG default security rules.</span></span>
   * <span data-ttu-id="3bd54-136">**ExpandedAddressPrefix** rozšíří hello předpony adres pro NSG výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="3bd54-136">**ExpandedAddressPrefix** expands hello address prefixes for NSG default tags.</span></span> <span data-ttu-id="3bd54-137">Značky představují více předponami adresy.</span><span class="sxs-lookup"><span data-stu-id="3bd54-137">Tags represent multiple address prefixes.</span></span> <span data-ttu-id="3bd54-138">Rozšíření značek hello může být užitečné při řešení potíží s připojením virtuálního počítače z konkrétní předpony.</span><span class="sxs-lookup"><span data-stu-id="3bd54-138">Expansion of hello tags can be useful when troubleshooting VM connectivity to/from specific address prefixes.</span></span> <span data-ttu-id="3bd54-139">Například s partnerský vztah virtuální síť, značka VIRTUAL_NETWORK rozšíří tooshow peered předponami virtuální sítě v předchozí výstup hello.</span><span class="sxs-lookup"><span data-stu-id="3bd54-139">For example, with VNET peering, VIRTUAL_NETWORK tag expands tooshow peered VNet prefixes in hello previous output.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="3bd54-140">příkaz pouze ukazuje efektivní pravidla Hello, pokud je skupina NSG přidružená podsíť, síťový adaptér nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="3bd54-140">hello command only shows effective rules if an NSG is associated with either a subnet, a NIC, or both.</span></span> <span data-ttu-id="3bd54-141">Virtuální počítač může mít více síťových adaptérů se použít odlišné skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="3bd54-141">A VM may have multiple NICs with different NSGs applied.</span></span> <span data-ttu-id="3bd54-142">Při odstraňování problémů s, spusťte příkaz hello pro každý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="3bd54-142">When troubleshooting, run hello command for each NIC.</span></span>
     > 
     > 
3. <span data-ttu-id="3bd54-143">tooease filtrování přes větší počet pravidel NSG, zadejte následující příkazy tootroubleshoot další hello:</span><span class="sxs-lookup"><span data-stu-id="3bd54-143">tooease filtering over larger number of NSG rules, enter hello following commands tootroubleshoot further:</span></span> 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    <span data-ttu-id="3bd54-144">Filtr pro provoz protokolu RDP (TCP port 3389), je použita toohello zobrazení mřížky, jak je znázorněno v následujícím obrázku hello:</span><span class="sxs-lookup"><span data-stu-id="3bd54-144">A filter for RDP traffic (TCP port 3389), is applied toohello grid view, as shown in hello following picture:</span></span>
   
    ![Seznam pravidel](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. <span data-ttu-id="3bd54-146">Jak můžete vidět v zobrazení mřížky hello, jsou obě povolit a zakázat pravidla pro protokol RDP.</span><span class="sxs-lookup"><span data-stu-id="3bd54-146">As you can see in hello grid view, there are both allow and deny rules for RDP.</span></span> <span data-ttu-id="3bd54-147">Hello výstup z kroku 2 ukazuje, že hello *DenyRDP* pravidlo je v hello skupina NSG použitá toohello podsítě.</span><span class="sxs-lookup"><span data-stu-id="3bd54-147">hello output from step 2 shows that hello *DenyRDP* rule is in hello NSG applied toohello subnet.</span></span> <span data-ttu-id="3bd54-148">Pro příchozí pravidla Nsg použije toohello podsítě jsou zpracován jako první.</span><span class="sxs-lookup"><span data-stu-id="3bd54-148">For inbound rules, NSGs applied toohello subnet are processed first.</span></span> <span data-ttu-id="3bd54-149">Pokud je nalezena shoda, nebude zpracován hello skupina NSG použitá toohello síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3bd54-149">If a match is found, hello NSG applied toohello network interface is not processed.</span></span> <span data-ttu-id="3bd54-150">V takovém případě hello *DenyRDP* pravidlo z podsítě hello blokuje RDP toohello virtuálních počítačů (**VM1**).</span><span class="sxs-lookup"><span data-stu-id="3bd54-150">In this case, hello *DenyRDP* rule from hello subnet blocks RDP toohello VM (**VM1**).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3bd54-151">Virtuální počítač může mít více síťových adaptérů připojených tooit.</span><span class="sxs-lookup"><span data-stu-id="3bd54-151">A VM may have multiple NICs attached tooit.</span></span> <span data-ttu-id="3bd54-152">Každý může být připojené tooa jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="3bd54-152">Each may be connected tooa different subnet.</span></span> <span data-ttu-id="3bd54-153">Vzhledem k tomu, že hello příkazy v předchozích krocích hello se spouštějí na síťový adaptér, je důležité tooensure, který zadáte hello seskupování, budete mít hello připojení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="3bd54-153">Since hello commands in hello previous steps are run against a NIC, it's important tooensure that you specify hello NIC you're having hello connectivity failure to.</span></span> <span data-ttu-id="3bd54-154">Pokud si nejste jisti, můžete vždy spustit hello příkazy pro každou síťovou kartu připojenou toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3bd54-154">If you're not sure, you can always run hello commands against each NIC attached toohello VM.</span></span>
   > 
   > 
5. <span data-ttu-id="3bd54-155">tooRDP do VM1, změna hello *odepřít protokolu RDP (3389)* pravidlo příliš*povolit RDP(3389)* v hello **Subnet1 NSG** NSG.</span><span class="sxs-lookup"><span data-stu-id="3bd54-155">tooRDP into VM1, change hello *Deny RDP (3389)* rule too*Allow RDP(3389)* in hello **Subnet1-NSG** NSG.</span></span> <span data-ttu-id="3bd54-156">Zkontrolujte, zda TCP port 3389 otevřete otevřením toohello připojení RDP virtuálního počítače nebo pomocí nástroje Pspingu hello.</span><span class="sxs-lookup"><span data-stu-id="3bd54-156">Confirm that TCP port 3389 is open by opening an RDP connection toohello VM or using hello PsPing tool.</span></span> <span data-ttu-id="3bd54-157">Další informace o Pspingu ve čtení hello [stránky pro stažení Pspingu](https://technet.microsoft.com/sysinternals/psping.aspx)</span><span class="sxs-lookup"><span data-stu-id="3bd54-157">You can learn more about PsPing by reading hello [PsPing download page](https://technet.microsoft.com/sysinternals/psping.aspx)</span></span>
   
    <span data-ttu-id="3bd54-158">Můžete nebo odebrání pravidla skupinu NSG pomocí hello informace v hello výstup hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3bd54-158">You can or remove rules from an NSG by using hello information in hello output from hello following command:</span></span>
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a><span data-ttu-id="3bd54-159">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3bd54-159">Considerations</span></span>
<span data-ttu-id="3bd54-160">Mějte na paměti následující body při řešení potíží s potíže s připojením k hello:</span><span class="sxs-lookup"><span data-stu-id="3bd54-160">Consider hello following points when troubleshooting connectivity problems:</span></span>

* <span data-ttu-id="3bd54-161">Výchozí pravidla NSG se blokují příchozí přístup z hello Internetu a povolení jenom virtuální síť příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="3bd54-161">Default NSG rules will block inbound access from hello internet and only permit VNet inbound traffic.</span></span> <span data-ttu-id="3bd54-162">Pravidla musí být explicitně přidán tooallow příchozí přístup z Internetu, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3bd54-162">Rules should be explicitly added tooallow inbound access from Internet, as required.</span></span>
* <span data-ttu-id="3bd54-163">Pokud neexistují žádná pravidla zabezpečení NSG způsobuje toofail připojení k síti Virtuálního počítače, může být problém hello příčiny:</span><span class="sxs-lookup"><span data-stu-id="3bd54-163">If there are no NSG security rules causing a VM’s network connectivity toofail, hello problem may be due to:</span></span>
  * <span data-ttu-id="3bd54-164">Software brány firewall běžící v rámci operačního systému hello Virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3bd54-164">Firewall software running within hello VM's operating system</span></span>
  * <span data-ttu-id="3bd54-165">Trasy nakonfigurované pro virtuální zařízení nebo místní provoz.</span><span class="sxs-lookup"><span data-stu-id="3bd54-165">Routes configured for virtual appliances or on-premises traffic.</span></span> <span data-ttu-id="3bd54-166">Přenosy z Internetu může být přesměrovaného tooon místní prostřednictvím vynucené tunelování.</span><span class="sxs-lookup"><span data-stu-id="3bd54-166">Internet traffic can be redirected tooon-premises via forced-tunneling.</span></span> <span data-ttu-id="3bd54-167">Připojení k protokolu RDP/SSH z Internetu tooyour hello virtuálních počítačů nemusí fungovat s tímto nastavením, v závislosti na tom, jak hello místní síťový hardware zpracovává tento provoz.</span><span class="sxs-lookup"><span data-stu-id="3bd54-167">An RDP/SSH connection from hello Internet tooyour VM may not work with this setting, depending on how hello on-premises network hardware handles this traffic.</span></span> <span data-ttu-id="3bd54-168">Čtení hello [řešení potíží s trasy](virtual-network-routes-troubleshoot-powershell.md) toolearn článku jak toodiagnose trasy problémy, které může být zpomalovat hello tok přenosů dat do a z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3bd54-168">Read hello [Troubleshooting Routes](virtual-network-routes-troubleshoot-powershell.md) article toolearn how toodiagnose route problems that may be impeding hello flow of traffic in and out of hello VM.</span></span> 
* <span data-ttu-id="3bd54-169">Pokud máte peered virtuálních sítí, ve výchozím nastavení, rozbalte hello VIRTUAL_NETWORK značky automaticky tooinclude předpon pro peered virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3bd54-169">If you have peered VNets, by default, hello VIRTUAL_NETWORK tag will automatically expand tooinclude prefixes for peered VNets.</span></span> <span data-ttu-id="3bd54-170">Tyto předpony můžete zobrazit v hello **ExpandedAddressPrefix** seznamu, tootroubleshoot všechny problémy související tooVNet partnerský vztah připojení.</span><span class="sxs-lookup"><span data-stu-id="3bd54-170">You can view these prefixes in hello **ExpandedAddressPrefix** list, tootroubleshoot any issues related tooVNet peering connectivity.</span></span> 
* <span data-ttu-id="3bd54-171">Efektivní zabezpečení pravidla se zobrazují pouze pokud je, že skupina NSG přidružená hello Virtuálního počítače síťový adaptér a nebo podsíť.</span><span class="sxs-lookup"><span data-stu-id="3bd54-171">Effective security rules are only shown if there is an NSG associated with hello VM’s NIC and or subnet.</span></span> 
* <span data-ttu-id="3bd54-172">Pokud neexistují žádné skupiny Nsg přidružená hello síťový adaptér nebo podsíť a jestli máte veřejnou IP adresu přiřadit tooyour virtuálních počítačů, budou všechny porty otevřené pro příchozí a odchozí přístup.</span><span class="sxs-lookup"><span data-stu-id="3bd54-172">If there are no NSGs associated with hello NIC or subnet and you have a public IP address assigned tooyour VM, all ports will be open for inbound and outbound access.</span></span> <span data-ttu-id="3bd54-173">Pokud hello virtuálních počítačů má veřejnou IP adresu, použití skupin Nsg toohello síťový adaptér nebo podsíť, důrazně doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="3bd54-173">If hello VM has a public IP address, applying NSGs toohello NIC or subnet is strongly recommended.</span></span>  

