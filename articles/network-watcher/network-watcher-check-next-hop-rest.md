---
title: "Další směrování s Azure sítě sledovacích procesů další směrování - REST aaaFind | Microsoft Docs"
description: "Tento článek popisuje, jak zjistíte, jaké hello dalšího směrování typ je a ip adresu pomocí další směrování pomocí hello REST API služby Azure"
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
ms.openlocfilehash: a2b61b355aae8ae513ebd44837184fbc6cfd668c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a><span data-ttu-id="f4845-103">Zjistěte, jaký typ hello dalšího směrování je díky funkci další segment hello v sledovací proces sítě Azure pomocí rozhraní REST API Azure</span><span class="sxs-lookup"><span data-stu-id="f4845-103">Find out what hello next hop type is using hello Next Hop capability in Aure Network Watcher using Azure REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="f4845-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f4845-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="f4845-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f4845-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="f4845-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f4845-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="f4845-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f4845-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="f4845-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="f4845-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="f4845-109">Další směrování je funkce sledovací proces sítě, která poskytuje možnost hello získat hello typ dalšího směrování a IP adresu podle zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f4845-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="f4845-110">Tato možnost je užitečná při určení toho, jestli je brána, internet nebo cílové tooits tooget virtuální sítě prochází přes odchozího provozu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f4845-110">This capability is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f4845-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f4845-111">Before you begin</span></span>

<span data-ttu-id="f4845-112">ARMclient je použité toocall hello REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f4845-112">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="f4845-113">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="f4845-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="f4845-114">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="f4845-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="f4845-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="f4845-115">Scenario</span></span>

<span data-ttu-id="f4845-116">scénář Hello popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá hello typ dalšího směrování a IP adresu pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="f4845-116">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="f4845-117">toolearn Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f4845-117">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

<span data-ttu-id="f4845-118">V tomto scénáři provedete následující:</span><span class="sxs-lookup"><span data-stu-id="f4845-118">In this scenario, you will:</span></span>

* <span data-ttu-id="f4845-119">Načtěte hello další směrování pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f4845-119">Retrieve hello next hop for a virtual machine.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="f4845-120">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="f4845-120">Log in with ARMClient</span></span>

<span data-ttu-id="f4845-121">Přihlaste se pomocí svých přihlašovacích údajů Azure tooarmclient.</span><span class="sxs-lookup"><span data-stu-id="f4845-121">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="f4845-122">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="f4845-122">Retrieve a virtual machine</span></span>

<span data-ttu-id="f4845-123">Spusťte následující skript tooreturn hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f4845-123">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="f4845-124">Tyto informace budete potřebovat pro spuštění dalším místě směrování.</span><span class="sxs-lookup"><span data-stu-id="f4845-124">This information is needed for running next hop.</span></span>

<span data-ttu-id="f4845-125">Následující kód Hello potřebuje hodnoty pro hello následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="f4845-125">hello following code needs values for hello following variables:</span></span>

- <span data-ttu-id="f4845-126">**ID předplatného** -hello toouse Id předplatného.</span><span class="sxs-lookup"><span data-stu-id="f4845-126">**subscriptionId** - hello subscription Id toouse.</span></span>
- <span data-ttu-id="f4845-127">**Název skupiny prostředků** – hello název skupiny prostředků, která obsahuje virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f4845-127">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="f4845-128">Id hello hello virtuálního počítače hello následující výstup, se používá v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f4845-128">From hello following output, hello id of hello virtual machine is used in hello following example:</span></span>

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

## <a name="get-next-hop"></a><span data-ttu-id="f4845-129">Získat další směrování</span><span class="sxs-lookup"><span data-stu-id="f4845-129">Get Next Hop</span></span>

<span data-ttu-id="f4845-130">Po vytvoření hello autorizační hlavičky je možné načíst hello dalšího přechodu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f4845-130">Once hello authorization header is created, hello next hop from a virtual machine can be retrieved.</span></span> <span data-ttu-id="f4845-131">Hello následující hodnoty se musí nahradit pro toowork příklad kódu hello.</span><span class="sxs-lookup"><span data-stu-id="f4845-131">hello following values must be replaced for hello code example toowork.</span></span>

> [!Important]
> <span data-ttu-id="f4845-132">Volání rozhraní API REST sledovací proces sítě hello název skupiny prostředků v žádosti o hello, že je identifikátor URI hello skupinu prostředků, která obsahuje hello sledovací proces sítě, není hello prostředků, kterou provádíte hello diagnostiky akce na.</span><span class="sxs-lookup"><span data-stu-id="f4845-132">For Network Watcher REST API calls hello resource group name in hello request URI is hello resource group that contains hello Network Watcher, not hello resources you are performing hello diagnostic actions on.</span></span>

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
> <span data-ttu-id="f4845-133">Další směrování vyžaduje, aby hello prostředků virtuálního počítače je přidělená toorun.</span><span class="sxs-lookup"><span data-stu-id="f4845-133">Next hop requires that hello VM resource is allocated toorun.</span></span>

## <a name="results"></a><span data-ttu-id="f4845-134">Výsledky</span><span class="sxs-lookup"><span data-stu-id="f4845-134">Results</span></span>

<span data-ttu-id="f4845-135">Hello následující fragment kódu je příklad výstupu hello přijata.</span><span class="sxs-lookup"><span data-stu-id="f4845-135">hello following snippet is an example of hello output received.</span></span> <span data-ttu-id="f4845-136">Hello výsledky obsahují hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f4845-136">hello results contain hello following values:</span></span>

* <span data-ttu-id="f4845-137">**nextHopType** – tato hodnota je jedním z následujících hodnot hello: Internet, VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway nebo žádný.</span><span class="sxs-lookup"><span data-stu-id="f4845-137">**nextHopType** - This value is one of hello following values: Internet, VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway, or None.</span></span>
* <span data-ttu-id="f4845-138">**nextHopIpAddress** -hello IP adresa dalšího směrování hello.</span><span class="sxs-lookup"><span data-stu-id="f4845-138">**nextHopIpAddress** - hello IP address of hello next hop.</span></span>
* <span data-ttu-id="f4845-139">**routeTableId** – hodnota hello je buď identifikátoru uri hello směrovací tabulka, která je spojená s trasou hello, nebo když není uživatelem definované trasy je hodnota definovaná hello *systémová trasa* je vrácen.</span><span class="sxs-lookup"><span data-stu-id="f4845-139">**routeTableId** - hello value is either a uri for hello route table associated with hello route, or if no user-defined route is defined hello value of *System Route* is returned.</span></span>

<span data-ttu-id="f4845-140">Hello následují výsledky hello ve formátu json.</span><span class="sxs-lookup"><span data-stu-id="f4845-140">hello following are hello results in json format.</span></span>

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a><span data-ttu-id="f4845-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4845-141">Next steps</span></span>

<span data-ttu-id="f4845-142">Jakmile jste možnost toofind out hello další směrování pro virtuální počítač, můžete zobrazit hello zabezpečení síťových prostředků navštívíte [zobrazení zabezpečení – přehled](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f4845-142">Once you have been able toofind out hello next hop for a virtual machine, you can view hello security of your network resources by visiting [Security View overview](network-watcher-security-group-view-overview.md)</span></span>














