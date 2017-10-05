---
title: "Automatizovat NSG auditování s zobrazení skupiny zabezpečení sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje pokyny, jak nakonfigurovat auditování skupinu zabezpečení sítě"
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
ms.openlocfilehash: a91da330e677c85f16f6f4e506613576b6507d7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a><span data-ttu-id="84123-103">Automatizovat NSG auditování s zobrazení skupiny zabezpečení sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="84123-103">Automate NSG auditing with Azure Network Watcher Security group view</span></span>

<span data-ttu-id="84123-104">Zákazníci jsou často potýkají s otázkou, ověřovat postavení zabezpečení svoji infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="84123-104">Customers are often faced with the challenge of verifying the security posture of their infrastructure.</span></span> <span data-ttu-id="84123-105">Tento problém se neliší pro jejich virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="84123-105">This challenge is no different for their VMs in Azure.</span></span> <span data-ttu-id="84123-106">Je důležité mají podobné profil zabezpečení na základě pravidel skupiny zabezpečení sítě (NSG) použít.</span><span class="sxs-lookup"><span data-stu-id="84123-106">It is important to have a similar security profile based on the Network Security Group (NSG) rules applied.</span></span> <span data-ttu-id="84123-107">Pomocí zobrazení skupiny zabezpečení, můžete nyní získat seznam pravidel, které u virtuálních počítačů v rámci skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="84123-107">Using the Security Group View, you can now get the list of rules applied to a VM within an NSG.</span></span> <span data-ttu-id="84123-108">Můžete definovat zlaté profil zabezpečení NSG a zahájit zobrazení skupiny zabezpečení na týdenní cadence a porovnání výstup zlaté profil a vytvořit sestavu.</span><span class="sxs-lookup"><span data-stu-id="84123-108">You can define a golden NSG security profile and initiate Security Group View on a weekly cadence and compare the output to the golden profile and create a report.</span></span> <span data-ttu-id="84123-109">Tímto způsobem můžete identifikovat snadno všechny virtuální počítače, které nejsou v souladu s profilem předepsaných zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="84123-109">This way you can identify with ease all the VMs that do not conform to the prescribed security profile.</span></span>

<span data-ttu-id="84123-110">Pokud jste obeznámeni s skupin zabezpečení sítě, navštivte [Přehled zabezpečení sítě](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="84123-110">If you are unfamiliar with Network Security Groups, visit [Network Security Overview](../virtual-network/virtual-networks-nsg.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="84123-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="84123-111">Before you begin</span></span>

<span data-ttu-id="84123-112">V tomto scénáři můžete porovnat známé dobré standardní hodnoty pro zobrazení výsledků skupiny zabezpečení pro virtuální počítač vrátí.</span><span class="sxs-lookup"><span data-stu-id="84123-112">In this scenario, you compare a known good baseline to the security group view results returned for a virtual machine.</span></span>

<span data-ttu-id="84123-113">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="84123-113">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="84123-114">Tento scénář také předpokládá, že skupina prostředků se platný virtuální počítač existuje má být použit.</span><span class="sxs-lookup"><span data-stu-id="84123-114">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="84123-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="84123-115">Scenario</span></span>

<span data-ttu-id="84123-116">Scénář popsaná v tomto článku získá zobrazení skupiny zabezpečení pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="84123-116">The scenario covered in this article gets the security group view for a virtual machine.</span></span>

<span data-ttu-id="84123-117">V tomto scénáři provedete následující:</span><span class="sxs-lookup"><span data-stu-id="84123-117">In this scenario, you will:</span></span>

- <span data-ttu-id="84123-118">Načíst sadu známé dobré pravidel</span><span class="sxs-lookup"><span data-stu-id="84123-118">Retrieve a known good rule set</span></span>
- <span data-ttu-id="84123-119">Načíst virtuální počítač s Rest API</span><span class="sxs-lookup"><span data-stu-id="84123-119">Retrieve a virtual machine with Rest API</span></span>
- <span data-ttu-id="84123-120">Získat zobrazení skupiny zabezpečení pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="84123-120">Get security group view for virtual machine</span></span>
- <span data-ttu-id="84123-121">Vyhodnocení odpovědi</span><span class="sxs-lookup"><span data-stu-id="84123-121">Evaluate Response</span></span>

## <a name="retrieve-rule-set"></a><span data-ttu-id="84123-122">Načtení sady pravidel</span><span class="sxs-lookup"><span data-stu-id="84123-122">Retrieve rule set</span></span>

<span data-ttu-id="84123-123">Prvním krokem v tomto příkladu je pro práci s stávajících standardních hodnot.</span><span class="sxs-lookup"><span data-stu-id="84123-123">The first step in this example is to work with an existing baseline.</span></span> <span data-ttu-id="84123-124">Následující příklad je některé json extrahovat z existující skupinu zabezpečení sítě pomocí `Get-AzureRmNetworkSecurityGroup` rutinu, která se používá jako Směrný plán pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="84123-124">The following example is some json extracted from an existing Network Security Group using the `Get-AzureRmNetworkSecurityGroup` cmdlet that is used as the baseline for this example.</span></span>

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

## <a name="convert-rule-set-to-powershell-objects"></a><span data-ttu-id="84123-125">Převést sada pravidel pro objekty prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="84123-125">Convert rule set to PowerShell objects</span></span>

<span data-ttu-id="84123-126">V tomto kroku jsme čtete soubor json, který jste vytvořili pravidla, která se očekává, že se na skupinu zabezpečení sítě v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="84123-126">In this step, we are reading a json file that was created earlier with the rules that are expected to be on the Network Security Group for this example.</span></span>

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a><span data-ttu-id="84123-127">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="84123-127">Retrieve Network Watcher</span></span>

<span data-ttu-id="84123-128">Dalším krokem je pro získání instance sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="84123-128">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="84123-129">`$networkWatcher` Proměnné je předána `AzureRmNetworkWatcherSecurityGroupView` rutiny.</span><span class="sxs-lookup"><span data-stu-id="84123-129">The `$networkWatcher` variable is passed to the `AzureRmNetworkWatcherSecurityGroupView` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="84123-130">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="84123-130">Get a VM</span></span>

<span data-ttu-id="84123-131">Virtuální počítač je potřeba spustit `Get-AzureRmNetworkWatcherSecurityGroupView` rutiny proti.</span><span class="sxs-lookup"><span data-stu-id="84123-131">A virtual machine is required to run the `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet against.</span></span> <span data-ttu-id="84123-132">Následující příklad načte objektu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84123-132">The following example gets a VM object.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a><span data-ttu-id="84123-133">Načtení zobrazení skupiny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="84123-133">Retrieve security group view</span></span>

<span data-ttu-id="84123-134">Dalším krokem je načíst výsledky zobrazení skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="84123-134">The next step is to retrieve the security group view result.</span></span> <span data-ttu-id="84123-135">Tento výsledek se porovnává se "základní" formátu json, který byl dříve vidět.</span><span class="sxs-lookup"><span data-stu-id="84123-135">This result is compared to the "baseline" json that was shown earlier.</span></span>

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-the-results"></a><span data-ttu-id="84123-136">Analýza výsledků</span><span class="sxs-lookup"><span data-stu-id="84123-136">Analyzing the results</span></span>

<span data-ttu-id="84123-137">Odpověď se seskupují po síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="84123-137">The response is grouped by Network interfaces.</span></span> <span data-ttu-id="84123-138">Různé typy pravidel vrátil jsou platné a výchozí pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="84123-138">The different types of rules returned are effective and default security rules.</span></span> <span data-ttu-id="84123-139">Výsledkem je další členěné podle jak se používají, buď na podsíť, nebo virtuální síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="84123-139">The result is further broken down by how it is applied, either on a subnet or a virtual NIC.</span></span>

<span data-ttu-id="84123-140">Následující skript prostředí PowerShell porovná výsledky zobrazení skupiny zabezpečení k existující výstup skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="84123-140">The following PowerShell script compares the results of the Security Group View to an existing output of an NSG.</span></span> <span data-ttu-id="84123-141">Následující příklad je jednoduchý příklad, jak je možné porovnávat výsledky s `Compare-Object` rutiny.</span><span class="sxs-lookup"><span data-stu-id="84123-141">The following example is a simple example of how the results can be compared with `Compare-Object` cmdlet.</span></span>

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

<span data-ttu-id="84123-142">V následujícím příkladu je výsledek.</span><span class="sxs-lookup"><span data-stu-id="84123-142">The following example is the result.</span></span> <span data-ttu-id="84123-143">Uvidíte dvě pravidla, které byly v první pravidlo nastavené nebyly nalezeny v porovnání.</span><span class="sxs-lookup"><span data-stu-id="84123-143">You can see two of the rules that were in the first rule set were not present in the comparison.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="84123-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84123-144">Next steps</span></span>

<span data-ttu-id="84123-145">Pokud se změnila nastavení, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) sledovat pravidla zabezpečení sítě skupiny a zabezpečení, které jsou v.</span><span class="sxs-lookup"><span data-stu-id="84123-145">If settings have been changed, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are in question.</span></span>













