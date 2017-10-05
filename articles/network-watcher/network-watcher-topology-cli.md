---
title: "Zobrazit topologii sledovací proces sítě Azure – rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje postup používání rozhraní příkazového řádku Azure k dotazování topologii vaší sítě."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 5cd279d7-3ab0-4813-aaa4-6a648bf74e7b
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 5be8e103f9a1f32117a4ed3be73bff021db1186d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-network-watcher-topology-with-azure-cli"></a><span data-ttu-id="daac6-103">Zobrazení topologie sledovací proces sítě pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="daac6-103">View Network Watcher topology with Azure CLI</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="daac6-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="daac6-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="daac6-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="daac6-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="daac6-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="daac6-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="daac6-107">REST API</span><span class="sxs-lookup"><span data-stu-id="daac6-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="daac6-108">Topologie funkci sledovací proces sítě poskytuje vizuální reprezentace síťových prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="daac6-108">The Topology feature of Network Watcher provides a visual representation of the network resources in a subscription.</span></span> <span data-ttu-id="daac6-109">Na portálu pro tuto vizualizaci se zobrazují automaticky.</span><span class="sxs-lookup"><span data-stu-id="daac6-109">In the portal, this visualization is presented to you automatically.</span></span> <span data-ttu-id="daac6-110">Informace pro zobrazení topologie na portálu se dají získat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="daac6-110">The information behind the topology view in the portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="daac6-111">Díky této vlastnosti je rozmanitější jako data mohou být spotřebovávána další nástroje pro sestavení se vizualizaci informací o topologii.</span><span class="sxs-lookup"><span data-stu-id="daac6-111">This capability makes the topology information more versatile as the data can be consumed by other tools to build out the visualization.</span></span>

<span data-ttu-id="daac6-112">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="daac6-112">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="daac6-113">Sledovací proces sítě aktuálně používá pro podporu rozhraní příkazového řádku Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="daac6-113">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

<span data-ttu-id="daac6-114">Propojení je modelován v dva vztahy.</span><span class="sxs-lookup"><span data-stu-id="daac6-114">The interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="daac6-115">**Členství ve skupině** – příklad: obsahuje podsíť virtuální sítě obsahuje síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="daac6-115">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="daac6-116">**Související** – příklad: síťový adaptér je přidružený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="daac6-116">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="daac6-117">V následujícím seznamu je vlastnosti, které jsou vráceny při dotazování topologie REST API.</span><span class="sxs-lookup"><span data-stu-id="daac6-117">The following list is properties that are returned when querying the Topology REST API.</span></span>

* <span data-ttu-id="daac6-118">**název** -název prostředku</span><span class="sxs-lookup"><span data-stu-id="daac6-118">**name** - The name of the resource</span></span>
* <span data-ttu-id="daac6-119">**ID** – identifikátor uri prostředku.</span><span class="sxs-lookup"><span data-stu-id="daac6-119">**id** - The uri of the resource.</span></span>
* <span data-ttu-id="daac6-120">**umístění** -umístění, kde existuje prostředek.</span><span class="sxs-lookup"><span data-stu-id="daac6-120">**location** - The location where the resource exists.</span></span>
* <span data-ttu-id="daac6-121">**přidružení** – seznam přidružení k odkazovaného objektu.</span><span class="sxs-lookup"><span data-stu-id="daac6-121">**associations** - A list of associations to the referenced object.</span></span>
    * <span data-ttu-id="daac6-122">**název** -název odkazovaného prostředku.</span><span class="sxs-lookup"><span data-stu-id="daac6-122">**name** - The name of the referenced resource.</span></span>
    * <span data-ttu-id="daac6-123">**resourceId** -resourceId je identifikátor uri prostředku, kterou se odkazuje v přidružení.</span><span class="sxs-lookup"><span data-stu-id="daac6-123">**resourceId** - The resourceId is the uri of the resource referenced in the association.</span></span>
    * <span data-ttu-id="daac6-124">**Třída associationType** – tato hodnota se odkazuje vztah mezi podřízený objekt a nadřazený.</span><span class="sxs-lookup"><span data-stu-id="daac6-124">**associationType** - This value references the relationship between the child object and the parent.</span></span> <span data-ttu-id="daac6-125">Platné hodnoty jsou **obsahuje** nebo **přidružené**.</span><span class="sxs-lookup"><span data-stu-id="daac6-125">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="daac6-126">Než začnete</span><span class="sxs-lookup"><span data-stu-id="daac6-126">Before you begin</span></span>

<span data-ttu-id="daac6-127">V tomto scénáři použijete `network watcher topology` rutiny k načtení informací o topologii.</span><span class="sxs-lookup"><span data-stu-id="daac6-127">In this scenario, you use the `network watcher topology` cmdlet to retrieve the topology information.</span></span> <span data-ttu-id="daac6-128">O tom, jak je také článek [načíst topologie sítě pomocí rozhraní REST API](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="daac6-128">There is also an article on how to [retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="daac6-129">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="daac6-129">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="daac6-130">Scénář</span><span class="sxs-lookup"><span data-stu-id="daac6-130">Scenario</span></span>

<span data-ttu-id="daac6-131">Scénář popsaná v tomto článku načte odpověď topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="daac6-131">The scenario covered in this article retrieves the topology response for a given resource group.</span></span>

## <a name="retrieve-topology"></a><span data-ttu-id="daac6-132">Načtení topologie</span><span class="sxs-lookup"><span data-stu-id="daac6-132">Retrieve topology</span></span>

<span data-ttu-id="daac6-133">`network watcher topology` Rutina načte topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="daac6-133">The `network watcher topology` cmdlet retrieves the topology for a given resource group.</span></span> <span data-ttu-id="daac6-134">Přidat argument "--json" zobrazíte oput ve formátu json</span><span class="sxs-lookup"><span data-stu-id="daac6-134">Add the argument "--json" to view the oput in json format</span></span>

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a><span data-ttu-id="daac6-135">Výsledky</span><span class="sxs-lookup"><span data-stu-id="daac6-135">Results</span></span>

<span data-ttu-id="daac6-136">Výsledky vrácené mít vlastnost name "zdroje", které obsahuje těla odpovědi json pro `network watcher topology` rutiny.</span><span class="sxs-lookup"><span data-stu-id="daac6-136">The results returned have a property name "Resources", which contains the json response body for the `network watcher topology` cmdlet.</span></span>  <span data-ttu-id="daac6-137">Odpověď obsahuje prostředky ve skupině zabezpečení sítě a jejich přidružení (který je obsahuje, přidružené).</span><span class="sxs-lookup"><span data-stu-id="daac6-137">The response contains the resources in the Network Security Group and their associations (that is, Contains, Associated).</span></span>

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "createdDateTime": "2017-02-17T22:20:59.461Z",
  "lastModified": "2016-12-19T22:23:02.546Z",
  "resources": [
    {
      "name": "testrg-vnet",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
      "location": "westcentralus",
      "associations": [
        {
          "name": "default",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "default",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
      "location": "westcentralus",
      "associations": []
    },
    {
      "name": "testclient",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testclient",
      "location": "westcentralus",
      "associations": [
        {
          "name": "testNic",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testNic",
          "associationType": "Contains"
        }
      ]
    },
    ...    
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="daac6-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="daac6-138">Next steps</span></span>

<span data-ttu-id="daac6-139">Další informace o zabezpečení pravidla, která se použijí k síťovým prostředkům, navštivte stránky [přehled zobrazení skupiny zabezpečení](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="daac6-139">Learn more about the security rules that are applied to your network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
