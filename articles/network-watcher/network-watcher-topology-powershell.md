---
title: "Zobrazit topologii sledovací proces sítě Azure – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak k dotazování vaší topologie sítě pomocí prostředí PowerShell."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: bd0e882d-8011-45e8-a7ce-de231a69fb85
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 40e01a7a6a2ea6127ab725f04649cec47b9d9422
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-network-watcher-topology-with-powershell"></a><span data-ttu-id="65e73-103">Zobrazit sledovací proces sítě topologie pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="65e73-103">View Network Watcher topology with PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="65e73-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="65e73-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="65e73-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="65e73-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="65e73-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="65e73-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="65e73-107">REST API</span><span class="sxs-lookup"><span data-stu-id="65e73-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="65e73-108">Topologie funkci sledovací proces sítě poskytuje vizuální reprezentace síťových prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="65e73-108">The Topology feature of Network Watcher provides a visual representation of the network resources in a subscription.</span></span> <span data-ttu-id="65e73-109">Na portálu pro tuto vizualizaci se zobrazují automaticky.</span><span class="sxs-lookup"><span data-stu-id="65e73-109">In the portal, this visualization is presented to you automatically.</span></span> <span data-ttu-id="65e73-110">Informace pro zobrazení topologie na portálu se dají získat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="65e73-110">The information behind the topology view in the portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="65e73-111">Díky této vlastnosti je rozmanitější jako data mohou být spotřebovávána další nástroje pro sestavení se vizualizaci informací o topologii.</span><span class="sxs-lookup"><span data-stu-id="65e73-111">This capability makes the topology information more versatile as the data can be consumed by other tools to build out the visualization.</span></span>

<span data-ttu-id="65e73-112">Propojení je modelován v dva vztahy.</span><span class="sxs-lookup"><span data-stu-id="65e73-112">The interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="65e73-113">**Členství ve skupině** – příklad: obsahuje podsíť virtuální sítě obsahuje síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="65e73-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="65e73-114">**Související** – příklad: síťový adaptér je přidružený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="65e73-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="65e73-115">V následujícím seznamu je vlastnosti, které jsou vráceny při dotazování topologie REST API.</span><span class="sxs-lookup"><span data-stu-id="65e73-115">The following list is properties that are returned when querying the Topology REST API.</span></span>

* <span data-ttu-id="65e73-116">**název** -název prostředku</span><span class="sxs-lookup"><span data-stu-id="65e73-116">**name** - The name of the resource</span></span>
* <span data-ttu-id="65e73-117">**ID** – identifikátor uri prostředku.</span><span class="sxs-lookup"><span data-stu-id="65e73-117">**id** - The uri of the resource.</span></span>
* <span data-ttu-id="65e73-118">**umístění** -umístění, kde existuje prostředek.</span><span class="sxs-lookup"><span data-stu-id="65e73-118">**location** - The location where the resource exists.</span></span>
* <span data-ttu-id="65e73-119">**přidružení** – seznam přidružení k odkazovaného objektu.</span><span class="sxs-lookup"><span data-stu-id="65e73-119">**associations** - A list of associations to the referenced object.</span></span>
    * <span data-ttu-id="65e73-120">**název** -název odkazovaného prostředku.</span><span class="sxs-lookup"><span data-stu-id="65e73-120">**name** - The name of the referenced resource.</span></span>
    * <span data-ttu-id="65e73-121">**resourceId** -resourceId je identifikátor uri prostředku, kterou se odkazuje v přidružení.</span><span class="sxs-lookup"><span data-stu-id="65e73-121">**resourceId** - The resourceId is the uri of the resource referenced in the association.</span></span>
    * <span data-ttu-id="65e73-122">**Třída associationType** – tato hodnota se odkazuje vztah mezi podřízený objekt a nadřazený.</span><span class="sxs-lookup"><span data-stu-id="65e73-122">**associationType** - This value references the relationship between the child object and the parent.</span></span> <span data-ttu-id="65e73-123">Platné hodnoty jsou **obsahuje** nebo **přidružené**.</span><span class="sxs-lookup"><span data-stu-id="65e73-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="65e73-124">Než začnete</span><span class="sxs-lookup"><span data-stu-id="65e73-124">Before you begin</span></span>

<span data-ttu-id="65e73-125">V tomto scénáři použijete `Get-AzureRmNetworkWatcherTopology` rutiny k načtení informací o topologii.</span><span class="sxs-lookup"><span data-stu-id="65e73-125">In this scenario, you use the `Get-AzureRmNetworkWatcherTopology` cmdlet to retrieve the topology information.</span></span> <span data-ttu-id="65e73-126">O tom, jak je také článek [načíst topologie sítě pomocí rozhraní REST API](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="65e73-126">There is also an article on how to [retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="65e73-127">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="65e73-127">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="65e73-128">Scénář</span><span class="sxs-lookup"><span data-stu-id="65e73-128">Scenario</span></span>

<span data-ttu-id="65e73-129">Scénář popsaná v tomto článku načte odpověď topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="65e73-129">The scenario covered in this article retrieves the topology response for a given resource group.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="65e73-130">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="65e73-130">Retrieve Network Watcher</span></span>

<span data-ttu-id="65e73-131">Prvním krokem je pro získání instance sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="65e73-131">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="65e73-132">`$networkWatcher` Proměnné je předána `Get-AzureRmNetworkWatcherTopology` rutiny.</span><span class="sxs-lookup"><span data-stu-id="65e73-132">The `$networkWatcher` variable is passed to the `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="retrieve-topology"></a><span data-ttu-id="65e73-133">Načtení topologie</span><span class="sxs-lookup"><span data-stu-id="65e73-133">Retrieve topology</span></span>

<span data-ttu-id="65e73-134">`Get-AzureRmNetworkWatcherTopology` Rutina načte topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="65e73-134">The `Get-AzureRmNetworkWatcherTopology` cmdlet retrieves the topology for a given resource group.</span></span>

```powershell
Get-AzureRmNetworkWatcherTopology -NetworkWatcher $networkWatcher -TargetResourceGroupName testrg
```

## <a name="results"></a><span data-ttu-id="65e73-135">Výsledky</span><span class="sxs-lookup"><span data-stu-id="65e73-135">Results</span></span>

<span data-ttu-id="65e73-136">Výsledky vrácené mít vlastnost name "zdroje", které obsahuje těla odpovědi json pro `Get-AzureRmNetworkWatcherTopology` rutiny.</span><span class="sxs-lookup"><span data-stu-id="65e73-136">The results returned have a property name "Resources", which contains the json response body for the `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>  <span data-ttu-id="65e73-137">Odpověď obsahuje prostředky ve skupině zabezpečení sítě a jejich přidružení (který je obsahuje, přidružené).</span><span class="sxs-lookup"><span data-stu-id="65e73-137">The response contains the resources in the Network Security Group and their associations (that is, Contains, Associated).</span></span>

```json
Id              : 00000000-0000-0000-0000-000000000000
CreatedDateTime : 2/1/2017 7:52:21 PM
LastModified    : 2/1/2017 7:46:18 PM
Resources       : [
                    {
                      "Name": "testrg-vnet",
                      "Id":
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet/subnets/default"
                        }
                      ]
                    },
                    {
                      "Name": "default",
                      "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testr
                  g-vnet/subnets/default",
                      "Location": "westcentralus",
                      "Associations": []
                    },
                    {
                      "Name": "testrg-vnet2",
                      "Id": 
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet2",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/default"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "GatewaySubnet",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/GatewaySubnet"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "gateway2",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworkGateways/gateway2"
                        }
                      ]
                    },
                    ...
                  ]
```

## <a name="next-steps"></a><span data-ttu-id="65e73-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65e73-138">Next steps</span></span>

<span data-ttu-id="65e73-139">Zjistěte, jak toku protokolů NSG s Power BI vizualizovat navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="65e73-139">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


