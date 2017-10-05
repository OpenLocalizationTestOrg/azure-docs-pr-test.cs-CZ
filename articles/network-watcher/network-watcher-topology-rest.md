---
title: "Zobrazit topologii sledovací proces sítě Azure – REST API | Microsoft Docs"
description: "Tento článek popisuje postup použití rozhraní REST API pro dotaz topologii vaší sítě."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: de9af643-aea1-4c4c-89c5-21f1bf334c06
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 568f3060da372f4a08cec342e04359172522cb69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a><span data-ttu-id="af3f5-103">Zobrazení topologie sledovací proces sítě pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="af3f5-103">View Network Watcher topology with REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="af3f5-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="af3f5-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="af3f5-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="af3f5-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="af3f5-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="af3f5-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="af3f5-107">REST API</span><span class="sxs-lookup"><span data-stu-id="af3f5-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="af3f5-108">Topologie funkci sledovací proces sítě poskytuje vizuální reprezentace síťových prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="af3f5-108">The Topology feature of Network Watcher provides a visual representation of the network resources in a subscription.</span></span> <span data-ttu-id="af3f5-109">Na portálu pro tuto vizualizaci se zobrazují automaticky.</span><span class="sxs-lookup"><span data-stu-id="af3f5-109">In the portal, this visualization is presented to you automatically.</span></span> <span data-ttu-id="af3f5-110">Informace pro zobrazení topologie na portálu se dají získat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="af3f5-110">The information behind the topology view in the portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="af3f5-111">Díky této vlastnosti je rozmanitější jako data mohou být spotřebovávána další nástroje pro sestavení se vizualizaci informací o topologii.</span><span class="sxs-lookup"><span data-stu-id="af3f5-111">This capability makes the topology information more versatile as the data can be consumed by other tools to build out the visualization.</span></span>

<span data-ttu-id="af3f5-112">Propojení je modelován v dva vztahy.</span><span class="sxs-lookup"><span data-stu-id="af3f5-112">The interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="af3f5-113">**Členství ve skupině** – příklad: obsahuje podsíť virtuální sítě obsahuje síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="af3f5-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="af3f5-114">**Související** – příklad: síťový adaptér je přidružený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="af3f5-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="af3f5-115">V následujícím seznamu je vlastnosti, které jsou vráceny při dotazování topologie REST API.</span><span class="sxs-lookup"><span data-stu-id="af3f5-115">The following list is properties that are returned when querying the Topology REST API.</span></span>

* <span data-ttu-id="af3f5-116">**název** -název prostředku</span><span class="sxs-lookup"><span data-stu-id="af3f5-116">**name** - The name of the resource</span></span>
* <span data-ttu-id="af3f5-117">**ID** – identifikátor uri prostředku.</span><span class="sxs-lookup"><span data-stu-id="af3f5-117">**id** - The uri of the resource.</span></span>
* <span data-ttu-id="af3f5-118">**umístění** -umístění, kde existuje prostředek.</span><span class="sxs-lookup"><span data-stu-id="af3f5-118">**location** - The location where the resource exists.</span></span>
* <span data-ttu-id="af3f5-119">**přidružení** – seznam přidružení k odkazovaného objektu.</span><span class="sxs-lookup"><span data-stu-id="af3f5-119">**associations** - A list of associations to the referenced object.</span></span>
    * <span data-ttu-id="af3f5-120">**název** -název odkazovaného prostředku.</span><span class="sxs-lookup"><span data-stu-id="af3f5-120">**name** - The name of the referenced resource.</span></span>
    * <span data-ttu-id="af3f5-121">**resourceId** -resourceId je identifikátor uri prostředku, kterou se odkazuje v přidružení.</span><span class="sxs-lookup"><span data-stu-id="af3f5-121">**resourceId** - The resourceId is the uri of the resource referenced in the association.</span></span>
    * <span data-ttu-id="af3f5-122">**Třída associationType** – tato hodnota se odkazuje vztah mezi podřízený objekt a nadřazený.</span><span class="sxs-lookup"><span data-stu-id="af3f5-122">**associationType** - This value references the relationship between the child object and the parent.</span></span> <span data-ttu-id="af3f5-123">Platné hodnoty jsou **obsahuje** nebo **přidružené**.</span><span class="sxs-lookup"><span data-stu-id="af3f5-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="af3f5-124">Než začnete</span><span class="sxs-lookup"><span data-stu-id="af3f5-124">Before you begin</span></span>

<span data-ttu-id="af3f5-125">V tomto scénáři můžete načíst informace o topologii.</span><span class="sxs-lookup"><span data-stu-id="af3f5-125">In this scenario, you retrieve the topology information.</span></span> <span data-ttu-id="af3f5-126">ARMclient se používá k volání rozhraní REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="af3f5-126">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="af3f5-127">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="af3f5-127">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="af3f5-128">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="af3f5-128">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="af3f5-129">Scénář</span><span class="sxs-lookup"><span data-stu-id="af3f5-129">Scenario</span></span>

<span data-ttu-id="af3f5-130">Scénář popsaná v tomto článku načte odpověď topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="af3f5-130">The scenario covered in this article retrieves the topology response for a given resource group.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="af3f5-131">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="af3f5-131">Log in with ARMClient</span></span>

<span data-ttu-id="af3f5-132">Přihlaste se k armclient pomocí svých přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="af3f5-132">Log in to armclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a><span data-ttu-id="af3f5-133">Načtení topologie</span><span class="sxs-lookup"><span data-stu-id="af3f5-133">Retrieve topology</span></span>

<span data-ttu-id="af3f5-134">Následující příklad požádá topologii z rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="af3f5-134">The following example requests the topology from the REST API.</span></span>  <span data-ttu-id="af3f5-135">V příkladu je parametry umožňující flexibilitu při vytváření příklad.</span><span class="sxs-lookup"><span data-stu-id="af3f5-135">The example is parameterized to allow for flexibility in creating an example.</span></span>  <span data-ttu-id="af3f5-136">Nahraďte všechny hodnoty s \< \> které obaluje je.</span><span class="sxs-lookup"><span data-stu-id="af3f5-136">Replace all values with \< \> surrounding them.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name to run topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

<span data-ttu-id="af3f5-137">Je odpověď na následující příklad zkrácení odpovědi, která je vrácena při načtení topologie pro skupina prostředků:</span><span class="sxs-lookup"><span data-stu-id="af3f5-137">The following response is an example of a shortened response that is returned when retrieve topology for a resourcegroup:</span></span>

```json
{
  "id": "ecd6c860-9cf5-411a-bdba-512f8df7799f",
  "createdDateTime": "2017-01-18T04:13:07.1974591Z",
  "lastModified": "2017-01-17T22:11:52.3527348Z",
  "resources": [
    {
      "name": "virtualNetwork1",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}",
      "location": "westcentralus",
      "associations": [
        {
          "name": "{subnetName}",
          "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/(virtualNetworkName)/subnets
/{subnetName}",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "webtestnsg-wjplxls65qcto",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}
s65qcto",
      "associationType": "Associated"
    },
    ...
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="af3f5-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af3f5-138">Next steps</span></span>

<span data-ttu-id="af3f5-139">Zjistěte, jak toku protokolů NSG s Power BI vizualizovat navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="af3f5-139">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

