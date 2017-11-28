---
title: "sledovací proces sítě Azure topologii aaaView – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje jak toouse prostředí PowerShell tooquery topologii vaší sítě."
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
ms.openlocfilehash: 2bc0ecf5baa81a68be53f55c74f362a7bc97116f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-powershell"></a><span data-ttu-id="fbf72-103">Zobrazit sledovací proces sítě topologie pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fbf72-103">View Network Watcher topology with PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="fbf72-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fbf72-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="fbf72-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fbf72-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="fbf72-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fbf72-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="fbf72-107">REST API</span><span class="sxs-lookup"><span data-stu-id="fbf72-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="fbf72-108">Funkce topologie Hello sledovací proces sítě poskytuje vizuální reprezentace hello síťových prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="fbf72-108">hello Topology feature of Network Watcher provides a visual representation of hello network resources in a subscription.</span></span> <span data-ttu-id="fbf72-109">V portálu hello tuto vizualizaci zobrazí tooyou automaticky.</span><span class="sxs-lookup"><span data-stu-id="fbf72-109">In hello portal, this visualization is presented tooyou automatically.</span></span> <span data-ttu-id="fbf72-110">informace o Hello za zobrazení topologie hello portálu hello se dají získat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fbf72-110">hello information behind hello topology view in hello portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="fbf72-111">Díky této vlastnosti je informace o topologii hello rozmanitější jako hello data mohou být spotřebovávána jiné nástroje toobuild out hello vizualizace.</span><span class="sxs-lookup"><span data-stu-id="fbf72-111">This capability makes hello topology information more versatile as hello data can be consumed by other tools toobuild out hello visualization.</span></span>

<span data-ttu-id="fbf72-112">propojení Hello je modelován v dva vztahy.</span><span class="sxs-lookup"><span data-stu-id="fbf72-112">hello interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="fbf72-113">**Členství ve skupině** – příklad: obsahuje podsíť virtuální sítě obsahuje síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="fbf72-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="fbf72-114">**Související** – příklad: síťový adaptér je přidružený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="fbf72-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="fbf72-115">Hello následujícím seznamu jsou uvedeny vlastnosti, které jsou vráceny při dotazování hello topologie REST API.</span><span class="sxs-lookup"><span data-stu-id="fbf72-115">hello following list is properties that are returned when querying hello Topology REST API.</span></span>

* <span data-ttu-id="fbf72-116">**název** – hello název prostředku hello</span><span class="sxs-lookup"><span data-stu-id="fbf72-116">**name** - hello name of hello resource</span></span>
* <span data-ttu-id="fbf72-117">**ID** -hello identifikátor uri prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="fbf72-117">**id** - hello uri of hello resource.</span></span>
* <span data-ttu-id="fbf72-118">**umístění** -hello umístění, kde existuje prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="fbf72-118">**location** - hello location where hello resource exists.</span></span>
* <span data-ttu-id="fbf72-119">**přidružení** – seznam přidružení toohello odkazuje objekt.</span><span class="sxs-lookup"><span data-stu-id="fbf72-119">**associations** - A list of associations toohello referenced object.</span></span>
    * <span data-ttu-id="fbf72-120">**název** -hello název hello odkazuje prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbf72-120">**name** - hello name of hello referenced resource.</span></span>
    * <span data-ttu-id="fbf72-121">**resourceId** -hello resourceId je identifikátor uri hello hello prostředku, kterou se odkazuje v hello přidružení.</span><span class="sxs-lookup"><span data-stu-id="fbf72-121">**resourceId** - hello resourceId is hello uri of hello resource referenced in hello association.</span></span>
    * <span data-ttu-id="fbf72-122">**Třída associationType** – tato hodnota se odkazuje hello vztah mezi hello podřízený objekt a nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="fbf72-122">**associationType** - This value references hello relationship between hello child object and hello parent.</span></span> <span data-ttu-id="fbf72-123">Platné hodnoty jsou **obsahuje** nebo **přidružené**.</span><span class="sxs-lookup"><span data-stu-id="fbf72-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fbf72-124">Než začnete</span><span class="sxs-lookup"><span data-stu-id="fbf72-124">Before you begin</span></span>

<span data-ttu-id="fbf72-125">V tomto scénáři použijete hello `Get-AzureRmNetworkWatcherTopology` informace o topologii rutiny tooretrieve hello.</span><span class="sxs-lookup"><span data-stu-id="fbf72-125">In this scenario, you use hello `Get-AzureRmNetworkWatcherTopology` cmdlet tooretrieve hello topology information.</span></span> <span data-ttu-id="fbf72-126">K dispozici je také článek o příliš[načíst topologie sítě pomocí rozhraní REST API](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="fbf72-126">There is also an article on how too[retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="fbf72-127">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="fbf72-127">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="fbf72-128">Scénář</span><span class="sxs-lookup"><span data-stu-id="fbf72-128">Scenario</span></span>

<span data-ttu-id="fbf72-129">scénář Hello popsaná v tomto článku načte odpověď hello topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbf72-129">hello scenario covered in this article retrieves hello topology response for a given resource group.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="fbf72-130">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="fbf72-130">Retrieve Network Watcher</span></span>

<span data-ttu-id="fbf72-131">prvním krokem Hello je tooretrieve hello sledovací proces sítě instance.</span><span class="sxs-lookup"><span data-stu-id="fbf72-131">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="fbf72-132">Hello `$networkWatcher` proměnné je předán toohello `Get-AzureRmNetworkWatcherTopology` rutiny.</span><span class="sxs-lookup"><span data-stu-id="fbf72-132">hello `$networkWatcher` variable is passed toohello `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="retrieve-topology"></a><span data-ttu-id="fbf72-133">Načtení topologie</span><span class="sxs-lookup"><span data-stu-id="fbf72-133">Retrieve topology</span></span>

<span data-ttu-id="fbf72-134">Hello `Get-AzureRmNetworkWatcherTopology` rutina načte hello topologie pro danou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="fbf72-134">hello `Get-AzureRmNetworkWatcherTopology` cmdlet retrieves hello topology for a given resource group.</span></span>

```powershell
Get-AzureRmNetworkWatcherTopology -NetworkWatcher $networkWatcher -TargetResourceGroupName testrg
```

## <a name="results"></a><span data-ttu-id="fbf72-135">Výsledky</span><span class="sxs-lookup"><span data-stu-id="fbf72-135">Results</span></span>

<span data-ttu-id="fbf72-136">Hello výsledky vrácené mít vlastnost název "prostředky", které obsahuje hello těla odpovědi json pro hello `Get-AzureRmNetworkWatcherTopology` rutiny.</span><span class="sxs-lookup"><span data-stu-id="fbf72-136">hello results returned have a property name "Resources", which contains hello json response body for hello `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>  <span data-ttu-id="fbf72-137">Hello odpovědi obsahuje hello prostředky v hello skupinu zabezpečení sítě a jejich přidružení (který je obsahuje, přidružené).</span><span class="sxs-lookup"><span data-stu-id="fbf72-137">hello response contains hello resources in hello Network Security Group and their associations (that is, Contains, Associated).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="fbf72-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fbf72-138">Next steps</span></span>

<span data-ttu-id="fbf72-139">Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="fbf72-139">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


