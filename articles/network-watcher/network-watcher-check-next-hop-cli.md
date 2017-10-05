---
title: "Najít další segment s Azure sítě sledovacích procesů další segment - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak můžete najít, co je typ dalšího směrování a ip adresu pomocí dalšího přechodu pomocí rozhraní příkazového řádku Azure."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: d1ee6870ba0188ff2c473e4cca12a5bdc1f97d3d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="4ecb2-103">Zjistěte, co typ dalšího směrování je pomocí funkce další směrování v sledovací proces sítě Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4ecb2-103">Find out what the next hop type is using the Next Hop capability in Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="4ecb2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4ecb2-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="4ecb2-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4ecb2-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="4ecb2-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4ecb2-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="4ecb2-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4ecb2-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="4ecb2-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="4ecb2-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="4ecb2-109">Další směrování je funkce sledovací proces sítě, která poskytuje možnost get typ dalšího směrování a IP adresy, které jsou založené na zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="4ecb2-110">Tato funkce je užitečná při určování, zda prochází odchozího provozu z virtuálního počítače brány, internet nebo virtuální sítě získat do cíle.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

<span data-ttu-id="4ecb2-111">Tento článek používá naší nové generace rozhraní příkazového řádku pro model nasazení prostředků management, Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-111">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="4ecb2-112">Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="4ecb2-112">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4ecb2-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4ecb2-113">Before you begin</span></span>

<span data-ttu-id="4ecb2-114">V tomto scénáři budete používat rozhraní příkazového řádku Azure se najít typ dalšího směrování a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-114">In this scenario, you will use the Azure CLI to find the next hop type and IP address.</span></span>

<span data-ttu-id="4ecb2-115">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-115">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="4ecb2-116">Tento scénář také předpokládá, že skupina prostředků se platný virtuální počítač existuje má být použit.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-116">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="4ecb2-117">Scénář</span><span class="sxs-lookup"><span data-stu-id="4ecb2-117">Scenario</span></span>

<span data-ttu-id="4ecb2-118">Scénář popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá typ dalšího směrování a IP adresu pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-118">The scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="4ecb2-119">Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4ecb2-119">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>


## <a name="get-next-hop"></a><span data-ttu-id="4ecb2-120">Získat další směrování</span><span class="sxs-lookup"><span data-stu-id="4ecb2-120">Get Next Hop</span></span>

<span data-ttu-id="4ecb2-121">Chcete-li získat další segment říkáme `az network watcher show-next-hop` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-121">To get the next hop we call the `az network watcher show-next-hop` cmdlet.</span></span> <span data-ttu-id="4ecb2-122">Jsme předat rutinu skupině prostředků sledovací proces sítě, NetworkWatcher, virtuální počítač Id, zdrojové IP adresy a cílové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-122">We pass the cmdlet the Network Watcher resource group, the NetworkWatcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="4ecb2-123">V tomto příkladu je cílovou IP adresu pro virtuální počítač v jiné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-123">In this example, the destination IP address is to a VM in another virtual network.</span></span> <span data-ttu-id="4ecb2-124">Mezi dvě virtuální sítě je bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-124">There is a virtual network gateway between the two virtual networks.</span></span>

<span data-ttu-id="4ecb2-125">Pokud nebyly dosud, nainstalovat a nakonfigurovat nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se k Azure účet pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4ecb2-125">If you haven't yet, install and configure the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="4ecb2-126">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4ecb2-126">Then run the following command:</span></span>

```azurecli
az network watcher show-next-hop --resource-group <resourcegroupName> --vm <vmNameorID> --source-ip <source-ip> --dest-ip <destination-ip>

```

> [!NOTE]
<span data-ttu-id="4ecb2-127">Pokud virtuální počítač má několik síťových adaptérů a předávání IP je povolena na žádném síťové karty, pak parametr síťový adaptér (-i síťový adaptér id) musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-127">If the VM has multiple NICs and IP forwarding is enabled on any of the NICs, then the NIC parameter (-i nic-id) must be specified.</span></span> <span data-ttu-id="4ecb2-128">V opačném případě je volitelný.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-128">Otherwise it is optional.</span></span>

## <a name="review-results"></a><span data-ttu-id="4ecb2-129">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="4ecb2-129">Review results</span></span>

<span data-ttu-id="4ecb2-130">Po dokončení, jsou uvedené výsledky.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-130">When complete, the results are provided.</span></span> <span data-ttu-id="4ecb2-131">IP adresa dalšího směrování je vrácen a také typ prostředku, který je.</span><span class="sxs-lookup"><span data-stu-id="4ecb2-131">The next hop IP address is returned as well as the type of resource it is.</span></span>

```azurecli
{
    "nextHopIpAddress": null,
    "nextHopType": "Internet",
    "routeTableId": "System Route"
}
```

<span data-ttu-id="4ecb2-132">V následujícím seznamu jsou aktuálně dostupné hodnoty NextHopType:</span><span class="sxs-lookup"><span data-stu-id="4ecb2-132">The following list shows the currently available NextHopType values:</span></span>

<span data-ttu-id="4ecb2-133">**Typ dalšího směrování**</span><span class="sxs-lookup"><span data-stu-id="4ecb2-133">**Next Hop Type**</span></span>

* <span data-ttu-id="4ecb2-134">Internet</span><span class="sxs-lookup"><span data-stu-id="4ecb2-134">Internet</span></span>
* <span data-ttu-id="4ecb2-135">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="4ecb2-135">VirtualAppliance</span></span>
* <span data-ttu-id="4ecb2-136">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="4ecb2-136">VirtualNetworkGateway</span></span>
* <span data-ttu-id="4ecb2-137">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="4ecb2-137">VnetLocal</span></span>
* <span data-ttu-id="4ecb2-138">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="4ecb2-138">HyperNetGateway</span></span>
* <span data-ttu-id="4ecb2-139">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="4ecb2-139">VnetPeering</span></span>
* <span data-ttu-id="4ecb2-140">Žádný</span><span class="sxs-lookup"><span data-stu-id="4ecb2-140">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ecb2-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4ecb2-141">Next steps</span></span>

<span data-ttu-id="4ecb2-142">Zjistěte, jak zkontrolovat nastavení skupiny zabezpečení sítě prostřednictvím kódu programu, navštivte stránky [NSG auditování s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="4ecb2-142">Learn how to review your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>
