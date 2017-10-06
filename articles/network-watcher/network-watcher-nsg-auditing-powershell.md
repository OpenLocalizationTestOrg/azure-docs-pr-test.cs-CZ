---
title: "aaaAutomate NSG auditování s zobrazení skupiny zabezpečení sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje pokyny, jak tooconfigure auditování skupinu zabezpečení sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 24fc418c433fceaf55a74b7c3b0e354dc46c8729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a><span data-ttu-id="43a17-103">Automatizovat NSG auditování s zobrazení skupiny zabezpečení sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="43a17-103">Automate NSG auditing with Azure Network Watcher Security group view</span></span>

<span data-ttu-id="43a17-104">Zákazníci se často potýkají s hello výzvy ověřování postavení zabezpečení hello svoji infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="43a17-104">Customers are often faced with hello challenge of verifying hello security posture of their infrastructure.</span></span> <span data-ttu-id="43a17-105">Tento problém se neliší pro jejich virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="43a17-105">This challenge is no different for their VMs in Azure.</span></span> <span data-ttu-id="43a17-106">Je důležité toohave podobné profil zabezpečení na základě hello skupina zabezpečení sítě (NSG) pravidel použít.</span><span class="sxs-lookup"><span data-stu-id="43a17-106">It is important toohave a similar security profile based on hello Network Security Group (NSG) rules applied.</span></span> <span data-ttu-id="43a17-107">Pomocí hello zobrazení skupiny zabezpečení, můžete nyní získat hello seznam pravidla použít tooa virtuálních počítačů v rámci skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="43a17-107">Using hello Security Group View, you can now get hello list of rules applied tooa VM within an NSG.</span></span> <span data-ttu-id="43a17-108">Můžete definovat zlaté profil zabezpečení NSG a zahájit zobrazení skupiny zabezpečení na týdenní cadence a porovnání hello výstup toohello zlaté profil a vytvořit sestavu.</span><span class="sxs-lookup"><span data-stu-id="43a17-108">You can define a golden NSG security profile and initiate Security Group View on a weekly cadence and compare hello output toohello golden profile and create a report.</span></span> <span data-ttu-id="43a17-109">Tímto způsobem můžete identifikovat snadno všechny hello virtuálních počítačů, které nejsou v souladu s toohello předepsané profil zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="43a17-109">This way you can identify with ease all hello VMs that do not conform toohello prescribed security profile.</span></span>

<span data-ttu-id="43a17-110">Pokud jste obeznámeni s skupin zabezpečení sítě, navštivte [Přehled zabezpečení sítě](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="43a17-110">If you are unfamiliar with Network Security Groups, visit [Network Security Overview](../virtual-network/virtual-networks-nsg.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="43a17-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="43a17-111">Before you begin</span></span>

<span data-ttu-id="43a17-112">V tomto scénáři můžete porovnat skupiny zabezpečení toohello známé dobré směrného plánu zobrazit výsledky vrácené pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="43a17-112">In this scenario, you compare a known good baseline toohello security group view results returned for a virtual machine.</span></span>

<span data-ttu-id="43a17-113">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="43a17-113">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="43a17-114">scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.</span><span class="sxs-lookup"><span data-stu-id="43a17-114">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="43a17-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="43a17-115">Scenario</span></span>

<span data-ttu-id="43a17-116">scénář Hello popsaná v tomto článku získá hello zobrazení skupiny zabezpečení pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="43a17-116">hello scenario covered in this article gets hello security group view for a virtual machine.</span></span>

<span data-ttu-id="43a17-117">V tomto scénáři provedete následující:</span><span class="sxs-lookup"><span data-stu-id="43a17-117">In this scenario, you will:</span></span>

- <span data-ttu-id="43a17-118">Načíst sadu známé dobré pravidel</span><span class="sxs-lookup"><span data-stu-id="43a17-118">Retrieve a known good rule set</span></span>
- <span data-ttu-id="43a17-119">Načíst virtuální počítač s Rest API</span><span class="sxs-lookup"><span data-stu-id="43a17-119">Retrieve a virtual machine with Rest API</span></span>
- <span data-ttu-id="43a17-120">Získat zobrazení skupiny zabezpečení pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="43a17-120">Get security group view for virtual machine</span></span>
- <span data-ttu-id="43a17-121">Vyhodnocení odpovědi</span><span class="sxs-lookup"><span data-stu-id="43a17-121">Evaluate Response</span></span>

## <a name="retrieve-rule-set"></a><span data-ttu-id="43a17-122">Načtení sady pravidel</span><span class="sxs-lookup"><span data-stu-id="43a17-122">Retrieve rule set</span></span>

<span data-ttu-id="43a17-123">prvním krokem Hello v tomto příkladu je toowork s stávajících standardních hodnot.</span><span class="sxs-lookup"><span data-stu-id="43a17-123">hello first step in this example is toowork with an existing baseline.</span></span> <span data-ttu-id="43a17-124">Hello následující příklad je některé json extrahoval z existující skupinu zabezpečení sítě pomocí hello `Get-AzureRmNetworkSecurityGroup` rutinu, která je použit jako účaří hello v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="43a17-124">hello following example is some json extracted from an existing Network Security Group using hello `Get-AzureRmNetworkSecurityGroup` cmdlet that is used as hello baseline for this example.</span></span>

```json
[
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "3389",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1000,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "default-allow-rdp",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/default-allow-rdp"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "111",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1010,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "MyRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/MyRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "112",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1020,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "My2ndRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/My2ndRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "5672",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Deny",
        "Priority":  1030,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "ThisRuleNeedsToStay",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/ThisRuleNeedsToStay"
    }
]
```

## <a name="convert-rule-set-toopowershell-objects"></a><span data-ttu-id="43a17-125">Převést objekty tooPowerShell sad pravidel</span><span class="sxs-lookup"><span data-stu-id="43a17-125">Convert rule set tooPowerShell objects</span></span>

<span data-ttu-id="43a17-126">V tomto kroku jsme jsou čtení souboru json, který byl vytvořen již dříve hello pravidla, která jsou očekávané toobe na hello skupinu zabezpečení sítě v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="43a17-126">In this step, we are reading a json file that was created earlier with hello rules that are expected toobe on hello Network Security Group for this example.</span></span>

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a><span data-ttu-id="43a17-127">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="43a17-127">Retrieve Network Watcher</span></span>

<span data-ttu-id="43a17-128">dalším krokem Hello je tooretrieve hello sledovací proces sítě instance.</span><span class="sxs-lookup"><span data-stu-id="43a17-128">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="43a17-129">Hello `$networkWatcher` proměnné je předán toohello `AzureRmNetworkWatcherSecurityGroupView` rutiny.</span><span class="sxs-lookup"><span data-stu-id="43a17-129">hello `$networkWatcher` variable is passed toohello `AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="43a17-130">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="43a17-130">Get a VM</span></span>

<span data-ttu-id="43a17-131">Virtuální počítač je požadovaná toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` rutiny proti.</span><span class="sxs-lookup"><span data-stu-id="43a17-131">A virtual machine is required toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="43a17-132">Následující ukázka Hello získá objektu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="43a17-132">hello following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="43a17-133">Načtení zobrazení skupiny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="43a17-133">Retrieve security group view</span></span>

<span data-ttu-id="43a17-134">dalším krokem Hello je tooretrieve hello zabezpečení skupiny zobrazení výsledků.</span><span class="sxs-lookup"><span data-stu-id="43a17-134">hello next step is tooretrieve hello security group view result.</span></span> <span data-ttu-id="43a17-135">Tento výsledek je json "základní" porovnání toohello, zobrazená dříve.</span><span class="sxs-lookup"><span data-stu-id="43a17-135">This result is compared toohello "baseline" json that was shown earlier.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-hello-results"></a><span data-ttu-id="43a17-136">Analýza výsledků hello</span><span class="sxs-lookup"><span data-stu-id="43a17-136">Analyzing hello results</span></span>

<span data-ttu-id="43a17-137">odpověď Hello se seskupují po síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="43a17-137">hello response is grouped by Network interfaces.</span></span> <span data-ttu-id="43a17-138">Hello různých typů pravidel vrátil jsou platné a výchozí pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="43a17-138">hello different types of rules returned are effective and default security rules.</span></span> <span data-ttu-id="43a17-139">výsledek Hello je další členěné podle jak se používají, buď na podsíť, nebo virtuální síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="43a17-139">hello result is further broken down by how it is applied, either on a subnet or a virtual NIC.</span></span>

<span data-ttu-id="43a17-140">Hello následující skript prostředí PowerShell porovná hello výsledky hello zobrazení skupiny zabezpečení tooan existující výstup skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="43a17-140">hello following PowerShell script compares hello results of hello Security Group View tooan existing output of an NSG.</span></span> <span data-ttu-id="43a17-141">Hello následující příklad je jednoduchý příklad, jak je možné porovnávat hello výsledky s `Compare-Object` rutiny.</span><span class="sxs-lookup"><span data-stu-id="43a17-141">hello following example is a simple example of how hello results can be compared with `Compare-Object` cmdlet.</span></span>

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

<span data-ttu-id="43a17-142">Následující ukázka Hello je výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="43a17-142">hello following example is hello result.</span></span> <span data-ttu-id="43a17-143">Uvidíte dvě hello pravidel, které byly první sada pravidel hello nebyly nalezeny v porovnání hello.</span><span class="sxs-lookup"><span data-stu-id="43a17-143">You can see two of hello rules that were in hello first rule set were not present in hello comparison.</span></span>

```
Name                     : My2ndRuleDoNotDelete
Description              : 
Protocol                 : *
SourcePortRange          : *
DestinationPortRange     : 112
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Allow
Priority                 : 1020
Direction                : Inbound
SideIndicator            : <=

Name                     : ThisRuleNeedsToStay
Description              : 
Protocol                 : TCP
SourcePortRange          : *
DestinationPortRange     : 5672
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Deny
Priority                 : 1030
Direction                : Inbound
SideIndicator            : <=
```

## <a name="next-steps"></a><span data-ttu-id="43a17-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43a17-144">Next steps</span></span>

<span data-ttu-id="43a17-145">Pokud se změnila nastavení, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které jsou v.</span><span class="sxs-lookup"><span data-stu-id="43a17-145">If settings have been changed, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are in question.</span></span>













