---
title: "sledovací proces sítě Azure topologii aaaView – 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje jak toouse Azure CLI 1.0 tooquery topologii vaší sítě."
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
ms.openlocfilehash: 30679d6dc74e85bacfc86c63bd1afa873893c772
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-azure-cli-10"></a><span data-ttu-id="89435-103">Zobrazit sledovací proces sítě topologie s Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="89435-103">View Network Watcher topology with Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="89435-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="89435-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="89435-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="89435-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="89435-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="89435-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="89435-107">REST API</span><span class="sxs-lookup"><span data-stu-id="89435-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="89435-108">Funkce topologie Hello sledovací proces sítě poskytuje vizuální reprezentace hello síťových prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="89435-108">hello Topology feature of Network Watcher provides a visual representation of hello network resources in a subscription.</span></span> <span data-ttu-id="89435-109">V portálu hello tuto vizualizaci zobrazí tooyou automaticky.</span><span class="sxs-lookup"><span data-stu-id="89435-109">In hello portal, this visualization is presented tooyou automatically.</span></span> <span data-ttu-id="89435-110">informace o Hello za zobrazení topologie hello portálu hello se dají získat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="89435-110">hello information behind hello topology view in hello portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="89435-111">Díky této vlastnosti je informace o topologii hello rozmanitější jako hello data mohou být spotřebovávána jiné nástroje toobuild out hello vizualizace.</span><span class="sxs-lookup"><span data-stu-id="89435-111">This capability makes hello topology information more versatile as hello data can be consumed by other tools toobuild out hello visualization.</span></span>

<span data-ttu-id="89435-112">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="89435-112">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> 

<span data-ttu-id="89435-113">propojení Hello je modelován v dva vztahy.</span><span class="sxs-lookup"><span data-stu-id="89435-113">hello interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="89435-114">**Členství ve skupině** – příklad: obsahuje podsíť virtuální sítě obsahuje síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="89435-114">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="89435-115">**Související** – příklad: síťový adaptér je přidružený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="89435-115">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="89435-116">Hello následujícím seznamu jsou uvedeny vlastnosti, které jsou vráceny při dotazování hello topologie REST API.</span><span class="sxs-lookup"><span data-stu-id="89435-116">hello following list is properties that are returned when querying hello Topology REST API.</span></span>

* <span data-ttu-id="89435-117">**název** – hello název prostředku hello</span><span class="sxs-lookup"><span data-stu-id="89435-117">**name** - hello name of hello resource</span></span>
* <span data-ttu-id="89435-118">**ID** -hello identifikátor uri prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="89435-118">**id** - hello uri of hello resource.</span></span>
* <span data-ttu-id="89435-119">**umístění** -hello umístění, kde existuje prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="89435-119">**location** - hello location where hello resource exists.</span></span>
* <span data-ttu-id="89435-120">**přidružení** – seznam přidružení toohello odkazuje objekt.</span><span class="sxs-lookup"><span data-stu-id="89435-120">**associations** - A list of associations toohello referenced object.</span></span>
    * <span data-ttu-id="89435-121">**název** -hello název hello odkazuje prostředků.</span><span class="sxs-lookup"><span data-stu-id="89435-121">**name** - hello name of hello referenced resource.</span></span>
    * <span data-ttu-id="89435-122">**resourceId** -hello resourceId je identifikátor uri hello hello prostředku, kterou se odkazuje v hello přidružení.</span><span class="sxs-lookup"><span data-stu-id="89435-122">**resourceId** - hello resourceId is hello uri of hello resource referenced in hello association.</span></span>
    * <span data-ttu-id="89435-123">**Třída associationType** – tato hodnota se odkazuje hello vztah mezi hello podřízený objekt a nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="89435-123">**associationType** - This value references hello relationship between hello child object and hello parent.</span></span> <span data-ttu-id="89435-124">Platné hodnoty jsou **obsahuje** nebo **přidružené**.</span><span class="sxs-lookup"><span data-stu-id="89435-124">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="89435-125">Než začnete</span><span class="sxs-lookup"><span data-stu-id="89435-125">Before you begin</span></span>

<span data-ttu-id="89435-126">V tomto scénáři použijete hello `network watcher topology` informace o topologii rutiny tooretrieve hello.</span><span class="sxs-lookup"><span data-stu-id="89435-126">In this scenario, you use hello `network watcher topology` cmdlet tooretrieve hello topology information.</span></span> <span data-ttu-id="89435-127">K dispozici je také článek o příliš[načíst topologie sítě pomocí rozhraní REST API](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="89435-127">There is also an article on how too[retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="89435-128">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="89435-128">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="89435-129">Scénář</span><span class="sxs-lookup"><span data-stu-id="89435-129">Scenario</span></span>

<span data-ttu-id="89435-130">scénář Hello popsaná v tomto článku načte odpověď hello topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="89435-130">hello scenario covered in this article retrieves hello topology response for a given resource group.</span></span>

## <a name="retrieve-topology"></a><span data-ttu-id="89435-131">Načtení topologie</span><span class="sxs-lookup"><span data-stu-id="89435-131">Retrieve topology</span></span>

<span data-ttu-id="89435-132">Hello `network watcher topology` rutina načte hello topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="89435-132">hello `network watcher topology` cmdlet retrieves hello topology for a given resource group.</span></span> <span data-ttu-id="89435-133">Přidat hello argument "--json" hello oput tooview ve formátu json</span><span class="sxs-lookup"><span data-stu-id="89435-133">Add hello argument "--json" tooview hello oput in json format</span></span>

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a><span data-ttu-id="89435-134">Výsledky</span><span class="sxs-lookup"><span data-stu-id="89435-134">Results</span></span>

<span data-ttu-id="89435-135">Hello výsledky vrácené mít vlastnost název "prostředky", které obsahuje hello těla odpovědi json pro hello `network watcher topology` rutiny.</span><span class="sxs-lookup"><span data-stu-id="89435-135">hello results returned have a property name "Resources", which contains hello json response body for hello `network watcher topology` cmdlet.</span></span>  <span data-ttu-id="89435-136">Hello odpovědi obsahuje hello prostředky v hello skupinu zabezpečení sítě a jejich přidružení (který je obsahuje, přidružené).</span><span class="sxs-lookup"><span data-stu-id="89435-136">hello response contains hello resources in hello Network Security Group and their associations (that is, Contains, Associated).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="89435-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="89435-137">Next steps</span></span>

<span data-ttu-id="89435-138">Další informace o pravidlech hello zabezpečení, které jsou použité tooyour síťovým prostředkům, navštivte stránky [přehled zobrazení skupiny zabezpečení](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="89435-138">Learn more about hello security rules that are applied tooyour network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
