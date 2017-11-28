---
title: "topologie sledovací proces sítě Azure aaaView - REST API | Microsoft Docs"
description: "Tento článek popisuje, jak toouse REST API tooquery topologii vaší sítě."
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
ms.openlocfilehash: 39292025bcd561f007c9e31271b1389be48ea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a><span data-ttu-id="78d64-103">Zobrazení topologie sledovací proces sítě pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="78d64-103">View Network Watcher topology with REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="78d64-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="78d64-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="78d64-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="78d64-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="78d64-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="78d64-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="78d64-107">REST API</span><span class="sxs-lookup"><span data-stu-id="78d64-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="78d64-108">Funkce topologie Hello sledovací proces sítě poskytuje vizuální reprezentace hello síťových prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="78d64-108">hello Topology feature of Network Watcher provides a visual representation of hello network resources in a subscription.</span></span> <span data-ttu-id="78d64-109">V portálu hello tuto vizualizaci zobrazí tooyou automaticky.</span><span class="sxs-lookup"><span data-stu-id="78d64-109">In hello portal, this visualization is presented tooyou automatically.</span></span> <span data-ttu-id="78d64-110">informace o Hello za zobrazení topologie hello portálu hello se dají získat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="78d64-110">hello information behind hello topology view in hello portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="78d64-111">Díky této vlastnosti je informace o topologii hello rozmanitější jako hello data mohou být spotřebovávána jiné nástroje toobuild out hello vizualizace.</span><span class="sxs-lookup"><span data-stu-id="78d64-111">This capability makes hello topology information more versatile as hello data can be consumed by other tools toobuild out hello visualization.</span></span>

<span data-ttu-id="78d64-112">propojení Hello je modelován v dva vztahy.</span><span class="sxs-lookup"><span data-stu-id="78d64-112">hello interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="78d64-113">**Členství ve skupině** – příklad: obsahuje podsíť virtuální sítě obsahuje síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="78d64-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="78d64-114">**Související** – příklad: síťový adaptér je přidružený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="78d64-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="78d64-115">Hello následujícím seznamu jsou uvedeny vlastnosti, které jsou vráceny při dotazování hello topologie REST API.</span><span class="sxs-lookup"><span data-stu-id="78d64-115">hello following list is properties that are returned when querying hello Topology REST API.</span></span>

* <span data-ttu-id="78d64-116">**název** – hello název prostředku hello</span><span class="sxs-lookup"><span data-stu-id="78d64-116">**name** - hello name of hello resource</span></span>
* <span data-ttu-id="78d64-117">**ID** -hello identifikátor uri prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="78d64-117">**id** - hello uri of hello resource.</span></span>
* <span data-ttu-id="78d64-118">**umístění** -hello umístění, kde existuje prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="78d64-118">**location** - hello location where hello resource exists.</span></span>
* <span data-ttu-id="78d64-119">**přidružení** – seznam přidružení toohello odkazuje objekt.</span><span class="sxs-lookup"><span data-stu-id="78d64-119">**associations** - A list of associations toohello referenced object.</span></span>
    * <span data-ttu-id="78d64-120">**název** -hello název hello odkazuje prostředků.</span><span class="sxs-lookup"><span data-stu-id="78d64-120">**name** - hello name of hello referenced resource.</span></span>
    * <span data-ttu-id="78d64-121">**resourceId** -hello resourceId je identifikátor uri hello hello prostředku, kterou se odkazuje v hello přidružení.</span><span class="sxs-lookup"><span data-stu-id="78d64-121">**resourceId** - hello resourceId is hello uri of hello resource referenced in hello association.</span></span>
    * <span data-ttu-id="78d64-122">**Třída associationType** – tato hodnota se odkazuje hello vztah mezi hello podřízený objekt a nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="78d64-122">**associationType** - This value references hello relationship between hello child object and hello parent.</span></span> <span data-ttu-id="78d64-123">Platné hodnoty jsou **obsahuje** nebo **přidružené**.</span><span class="sxs-lookup"><span data-stu-id="78d64-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="78d64-124">Než začnete</span><span class="sxs-lookup"><span data-stu-id="78d64-124">Before you begin</span></span>

<span data-ttu-id="78d64-125">V tomto scénáři můžete načíst informace o topologii hello.</span><span class="sxs-lookup"><span data-stu-id="78d64-125">In this scenario, you retrieve hello topology information.</span></span> <span data-ttu-id="78d64-126">ARMclient je použité toocall hello REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="78d64-126">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="78d64-127">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="78d64-127">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="78d64-128">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="78d64-128">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="78d64-129">Scénář</span><span class="sxs-lookup"><span data-stu-id="78d64-129">Scenario</span></span>

<span data-ttu-id="78d64-130">scénář Hello popsaná v tomto článku načte odpověď hello topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="78d64-130">hello scenario covered in this article retrieves hello topology response for a given resource group.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="78d64-131">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="78d64-131">Log in with ARMClient</span></span>

<span data-ttu-id="78d64-132">Přihlaste se pomocí svých přihlašovacích údajů Azure tooarmclient.</span><span class="sxs-lookup"><span data-stu-id="78d64-132">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a><span data-ttu-id="78d64-133">Načtení topologie</span><span class="sxs-lookup"><span data-stu-id="78d64-133">Retrieve topology</span></span>

<span data-ttu-id="78d64-134">Následující ukázka Hello požadavků hello topologie hello REST API.</span><span class="sxs-lookup"><span data-stu-id="78d64-134">hello following example requests hello topology from hello REST API.</span></span>  <span data-ttu-id="78d64-135">je třeba Hello parametrizované tooallow flexibilitu při vytváření příklad.</span><span class="sxs-lookup"><span data-stu-id="78d64-135">hello example is parameterized tooallow for flexibility in creating an example.</span></span>  <span data-ttu-id="78d64-136">Nahraďte všechny hodnoty s \< \> které obaluje je.</span><span class="sxs-lookup"><span data-stu-id="78d64-136">Replace all values with \< \> surrounding them.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name toorun topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

<span data-ttu-id="78d64-137">Hello následující odpověď je příkladem zkrácení odpovědi, která je vrácena při načtení topologie pro skupina prostředků:</span><span class="sxs-lookup"><span data-stu-id="78d64-137">hello following response is an example of a shortened response that is returned when retrieve topology for a resourcegroup:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="78d64-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78d64-138">Next steps</span></span>

<span data-ttu-id="78d64-139">Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="78d64-139">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

