---
title: "Zobrazit topologii sledovací proces sítě Azure – 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje postup použití Azure CLI 1.0 k dotazování topologii vaší sítě."
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
ms.openlocfilehash: 9178c485a92e04564c95dae8073f045b5c639bb7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-network-watcher-topology-with-azure-cli-10"></a><span data-ttu-id="6f248-103">Zobrazit sledovací proces sítě topologie s Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="6f248-103">View Network Watcher topology with Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="6f248-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6f248-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="6f248-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="6f248-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="6f248-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="6f248-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="6f248-107">REST API</span><span class="sxs-lookup"><span data-stu-id="6f248-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="6f248-108">Topologie funkci sledovací proces sítě poskytuje vizuální reprezentace síťových prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="6f248-108">The Topology feature of Network Watcher provides a visual representation of the network resources in a subscription.</span></span> <span data-ttu-id="6f248-109">Na portálu pro tuto vizualizaci se zobrazují automaticky.</span><span class="sxs-lookup"><span data-stu-id="6f248-109">In the portal, this visualization is presented to you automatically.</span></span> <span data-ttu-id="6f248-110">Informace pro zobrazení topologie na portálu se dají získat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6f248-110">The information behind the topology view in the portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="6f248-111">Díky této vlastnosti je rozmanitější jako data mohou být spotřebovávána další nástroje pro sestavení se vizualizaci informací o topologii.</span><span class="sxs-lookup"><span data-stu-id="6f248-111">This capability makes the topology information more versatile as the data can be consumed by other tools to build out the visualization.</span></span>

<span data-ttu-id="6f248-112">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="6f248-112">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> 

<span data-ttu-id="6f248-113">Propojení je modelován v dva vztahy.</span><span class="sxs-lookup"><span data-stu-id="6f248-113">The interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="6f248-114">**Členství ve skupině** – příklad: obsahuje podsíť virtuální sítě obsahuje síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="6f248-114">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="6f248-115">**Související** – příklad: síťový adaptér je přidružený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="6f248-115">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="6f248-116">V následujícím seznamu je vlastnosti, které jsou vráceny při dotazování topologie REST API.</span><span class="sxs-lookup"><span data-stu-id="6f248-116">The following list is properties that are returned when querying the Topology REST API.</span></span>

* <span data-ttu-id="6f248-117">**název** -název prostředku</span><span class="sxs-lookup"><span data-stu-id="6f248-117">**name** - The name of the resource</span></span>
* <span data-ttu-id="6f248-118">**ID** – identifikátor uri prostředku.</span><span class="sxs-lookup"><span data-stu-id="6f248-118">**id** - The uri of the resource.</span></span>
* <span data-ttu-id="6f248-119">**umístění** -umístění, kde existuje prostředek.</span><span class="sxs-lookup"><span data-stu-id="6f248-119">**location** - The location where the resource exists.</span></span>
* <span data-ttu-id="6f248-120">**přidružení** – seznam přidružení k odkazovaného objektu.</span><span class="sxs-lookup"><span data-stu-id="6f248-120">**associations** - A list of associations to the referenced object.</span></span>
    * <span data-ttu-id="6f248-121">**název** -název odkazovaného prostředku.</span><span class="sxs-lookup"><span data-stu-id="6f248-121">**name** - The name of the referenced resource.</span></span>
    * <span data-ttu-id="6f248-122">**resourceId** -resourceId je identifikátor uri prostředku, kterou se odkazuje v přidružení.</span><span class="sxs-lookup"><span data-stu-id="6f248-122">**resourceId** - The resourceId is the uri of the resource referenced in the association.</span></span>
    * <span data-ttu-id="6f248-123">**Třída associationType** – tato hodnota se odkazuje vztah mezi podřízený objekt a nadřazený.</span><span class="sxs-lookup"><span data-stu-id="6f248-123">**associationType** - This value references the relationship between the child object and the parent.</span></span> <span data-ttu-id="6f248-124">Platné hodnoty jsou **obsahuje** nebo **přidružené**.</span><span class="sxs-lookup"><span data-stu-id="6f248-124">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6f248-125">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6f248-125">Before you begin</span></span>

<span data-ttu-id="6f248-126">V tomto scénáři použijete `network watcher topology` rutiny k načtení informací o topologii.</span><span class="sxs-lookup"><span data-stu-id="6f248-126">In this scenario, you use the `network watcher topology` cmdlet to retrieve the topology information.</span></span> <span data-ttu-id="6f248-127">O tom, jak je také článek [načíst topologie sítě pomocí rozhraní REST API](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="6f248-127">There is also an article on how to [retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="6f248-128">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="6f248-128">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="6f248-129">Scénář</span><span class="sxs-lookup"><span data-stu-id="6f248-129">Scenario</span></span>

<span data-ttu-id="6f248-130">Scénář popsaná v tomto článku načte odpověď topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f248-130">The scenario covered in this article retrieves the topology response for a given resource group.</span></span>

## <a name="retrieve-topology"></a><span data-ttu-id="6f248-131">Načtení topologie</span><span class="sxs-lookup"><span data-stu-id="6f248-131">Retrieve topology</span></span>

<span data-ttu-id="6f248-132">`network watcher topology` Rutina načte topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f248-132">The `network watcher topology` cmdlet retrieves the topology for a given resource group.</span></span> <span data-ttu-id="6f248-133">Přidat argument "--json" zobrazíte oput ve formátu json</span><span class="sxs-lookup"><span data-stu-id="6f248-133">Add the argument "--json" to view the oput in json format</span></span>

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a><span data-ttu-id="6f248-134">Výsledky</span><span class="sxs-lookup"><span data-stu-id="6f248-134">Results</span></span>

<span data-ttu-id="6f248-135">Výsledky vrácené mít vlastnost name "zdroje", které obsahuje těla odpovědi json pro `network watcher topology` rutiny.</span><span class="sxs-lookup"><span data-stu-id="6f248-135">The results returned have a property name "Resources", which contains the json response body for the `network watcher topology` cmdlet.</span></span>  <span data-ttu-id="6f248-136">Odpověď obsahuje prostředky ve skupině zabezpečení sítě a jejich přidružení (který je obsahuje, přidružené).</span><span class="sxs-lookup"><span data-stu-id="6f248-136">The response contains the resources in the Network Security Group and their associations (that is, Contains, Associated).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="6f248-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f248-137">Next steps</span></span>

<span data-ttu-id="6f248-138">Další informace o zabezpečení pravidla, která se použijí k síťovým prostředkům, navštivte stránky [přehled zobrazení skupiny zabezpečení](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="6f248-138">Learn more about the security rules that are applied to your network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
