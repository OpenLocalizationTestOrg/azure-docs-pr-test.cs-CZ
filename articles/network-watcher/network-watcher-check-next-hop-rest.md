---
title: "Najít další segment s síť Azure sledovacích procesů další segment - REST | Microsoft Docs"
description: "Tento článek popisuje, jak můžete najít, co je typ dalšího směrování a ip adresu pomocí dalšího přechodu pomocí REST API služby Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2216c059-45ba-4214-8304-e56769b779a6
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 644713d365191bf5e51517d0cc565efbc2abc144
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a><span data-ttu-id="33e5c-103">Zjistěte, co typ dalšího směrování je pomocí funkce další směrování v sledovací proces sítě Azure pomocí rozhraní REST API Azure</span><span class="sxs-lookup"><span data-stu-id="33e5c-103">Find out what the next hop type is using the Next Hop capability in Aure Network Watcher using Azure REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="33e5c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="33e5c-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="33e5c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="33e5c-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="33e5c-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="33e5c-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="33e5c-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="33e5c-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="33e5c-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="33e5c-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="33e5c-109">Další směrování je funkce sledovací proces sítě, která poskytuje možnost get typ dalšího směrování a IP adresy, které jsou založené na zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="33e5c-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="33e5c-110">Tato možnost je užitečná při určování, zda prochází odchozího provozu z virtuálního počítače brány, internet nebo virtuální sítě získat do cíle.</span><span class="sxs-lookup"><span data-stu-id="33e5c-110">This capability is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="33e5c-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="33e5c-111">Before you begin</span></span>

<span data-ttu-id="33e5c-112">ARMclient se používá k volání rozhraní REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33e5c-112">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="33e5c-113">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="33e5c-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="33e5c-114">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="33e5c-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="33e5c-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="33e5c-115">Scenario</span></span>

<span data-ttu-id="33e5c-116">Scénář popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá typ dalšího směrování a IP adresu pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="33e5c-116">The scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="33e5c-117">Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="33e5c-117">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

<span data-ttu-id="33e5c-118">V tomto scénáři provedete následující:</span><span class="sxs-lookup"><span data-stu-id="33e5c-118">In this scenario, you will:</span></span>

* <span data-ttu-id="33e5c-119">Načtěte další směrování pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="33e5c-119">Retrieve the next hop for a virtual machine.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="33e5c-120">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="33e5c-120">Log in with ARMClient</span></span>

<span data-ttu-id="33e5c-121">Přihlaste se k armclient pomocí svých přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="33e5c-121">Log in to armclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="33e5c-122">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="33e5c-122">Retrieve a virtual machine</span></span>

<span data-ttu-id="33e5c-123">Spusťte následující skript pro vrácení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="33e5c-123">Run the following script to return a virtual machine.</span></span> <span data-ttu-id="33e5c-124">Tyto informace budete potřebovat pro spuštění dalším místě směrování.</span><span class="sxs-lookup"><span data-stu-id="33e5c-124">This information is needed for running next hop.</span></span>

<span data-ttu-id="33e5c-125">Následující kód potřebuje hodnoty pro následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="33e5c-125">The following code needs values for the following variables:</span></span>

- <span data-ttu-id="33e5c-126">**ID předplatného** -Id použít předplatného.</span><span class="sxs-lookup"><span data-stu-id="33e5c-126">**subscriptionId** - The subscription Id to use.</span></span>
- <span data-ttu-id="33e5c-127">**Název skupiny prostředků** -název skupiny prostředků, která obsahuje virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="33e5c-127">**resourceGroupName** - The name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="33e5c-128">Z výstupu v následujícím id virtuálního počítače se používá v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="33e5c-128">From the following output, the id of the virtual machine is used in the following example:</span></span>

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="get-next-hop"></a><span data-ttu-id="33e5c-129">Získat další směrování</span><span class="sxs-lookup"><span data-stu-id="33e5c-129">Get Next Hop</span></span>

<span data-ttu-id="33e5c-130">Po vytvoření hlavičku autorizace je možné načíst dalšího přechodu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="33e5c-130">Once the authorization header is created, the next hop from a virtual machine can be retrieved.</span></span> <span data-ttu-id="33e5c-131">Pro tento příklad kódu pro práci se musí nahradit následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="33e5c-131">The following values must be replaced for the code example to work.</span></span>

> [!Important]
> <span data-ttu-id="33e5c-132">Pro volání rozhraní API REST sledovací proces sítě, že název skupiny prostředků v identifikátoru URI požadavku je skupina prostředků, který obsahuje sledovací proces sítě ne prostředky provádíte diagnostiky akce na.</span><span class="sxs-lookup"><span data-stu-id="33e5c-132">For Network Watcher REST API calls the resource group name in the request URI is the resource group that contains the Network Watcher, not the resources you are performing the diagnostic actions on.</span></span>

```powershell
$sourceIP = "10.0.0.4"
$destinationIP = "8.8.8.8"
$resourceGroupName = <resource group name>
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'sourceIpAddress': '${sourceIP}',
    'destinationIpAddress': '${destinationIP}',
    'targetNicResourceId' : '${targetNic}'
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/nextHop?api-version=2016-12-01" $requestBody
```

> [!NOTE]
> <span data-ttu-id="33e5c-133">Další směrování vyžaduje, aby prostředků virtuálního počítače je přidělená ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="33e5c-133">Next hop requires that the VM resource is allocated to run.</span></span>

## <a name="results"></a><span data-ttu-id="33e5c-134">Výsledky</span><span class="sxs-lookup"><span data-stu-id="33e5c-134">Results</span></span>

<span data-ttu-id="33e5c-135">Následující fragment kódu je příklad výstupu přijata.</span><span class="sxs-lookup"><span data-stu-id="33e5c-135">The following snippet is an example of the output received.</span></span> <span data-ttu-id="33e5c-136">Výsledky obsahují následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="33e5c-136">The results contain the following values:</span></span>

* <span data-ttu-id="33e5c-137">**nextHopType** – tato hodnota je jedním z následujících hodnot: Internet, VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway nebo žádný.</span><span class="sxs-lookup"><span data-stu-id="33e5c-137">**nextHopType** - This value is one of the following values: Internet, VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway, or None.</span></span>
* <span data-ttu-id="33e5c-138">**nextHopIpAddress** – IP adresa dalšího směrování.</span><span class="sxs-lookup"><span data-stu-id="33e5c-138">**nextHopIpAddress** - The IP address of the next hop.</span></span>
* <span data-ttu-id="33e5c-139">**routeTableId** – hodnota je buď identifikátor uri pro směrovací tabulka přiřazené k postupu, nebo když není uživatelem definované trasy je definována hodnota *systémová trasa* je vrácen.</span><span class="sxs-lookup"><span data-stu-id="33e5c-139">**routeTableId** - The value is either a uri for the route table associated with the route, or if no user-defined route is defined the value of *System Route* is returned.</span></span>

<span data-ttu-id="33e5c-140">Níže jsou výsledky ve formátu json.</span><span class="sxs-lookup"><span data-stu-id="33e5c-140">The following are the results in json format.</span></span>

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a><span data-ttu-id="33e5c-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33e5c-141">Next steps</span></span>

<span data-ttu-id="33e5c-142">Jakmile jste moct zjistit další směrování pro virtuální počítač, můžete zobrazit zabezpečení síťových prostředků navštívíte [zobrazení zabezpečení – přehled](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="33e5c-142">Once you have been able to find out the next hop for a virtual machine, you can view the security of your network resources by visiting [Security View overview](network-watcher-security-group-view-overview.md)</span></span>














