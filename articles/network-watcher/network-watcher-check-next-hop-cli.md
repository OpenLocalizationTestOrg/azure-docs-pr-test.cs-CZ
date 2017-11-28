---
title: "Další směrování aaaFind s Azure sítě sledovacích procesů další segment - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak zjistíte, jaké hello dalšího směrování typ je a pomocí ip adresa dalšího směrování pomocí rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: 77c2bde51274bd5c64e7a2467f95139af620ca30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="27f18-103">Zjistěte, jaký typ hello dalšího směrování je díky funkci další segment hello v sledovací proces sítě Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="27f18-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="27f18-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="27f18-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="27f18-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="27f18-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="27f18-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="27f18-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="27f18-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="27f18-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="27f18-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="27f18-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="27f18-109">Další směrování je funkce sledovací proces sítě, která poskytuje možnost hello získat hello typ dalšího směrování a IP adresu podle zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="27f18-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="27f18-110">Tato funkce je užitečná při určování, zda je brána, internet nebo cílové tooits tooget virtuální sítě prochází přes odchozího provozu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="27f18-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

<span data-ttu-id="27f18-111">Tento článek používá naší nové generace rozhraní příkazového řádku pro model nasazení hello prostředků management, Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="27f18-111">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="27f18-112">tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="27f18-112">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="27f18-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="27f18-113">Before you begin</span></span>

<span data-ttu-id="27f18-114">V tomto scénáři použijete hello rozhraní příkazového řádku Azure toofind hello typ dalšího směrování a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="27f18-114">In this scenario, you will use hello Azure CLI toofind hello next hop type and IP address.</span></span>

<span data-ttu-id="27f18-115">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="27f18-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="27f18-116">scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.</span><span class="sxs-lookup"><span data-stu-id="27f18-116">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="27f18-117">Scénář</span><span class="sxs-lookup"><span data-stu-id="27f18-117">Scenario</span></span>

<span data-ttu-id="27f18-118">scénář Hello popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá hello typ dalšího směrování a IP adresu pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="27f18-118">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="27f18-119">toolearn Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27f18-119">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>


## <a name="get-next-hop"></a><span data-ttu-id="27f18-120">Získat další směrování</span><span class="sxs-lookup"><span data-stu-id="27f18-120">Get Next Hop</span></span>

<span data-ttu-id="27f18-121">tooget hello další segment říkáme hello `az network watcher show-next-hop` rutiny.</span><span class="sxs-lookup"><span data-stu-id="27f18-121">tooget hello next hop we call hello `az network watcher show-next-hop` cmdlet.</span></span> <span data-ttu-id="27f18-122">Jsme předat skupinu prostředků hello rutiny hello sledovací proces sítě, hello NetworkWatcher, virtuálního počítače Id, zdrojové IP adresy a cílové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="27f18-122">We pass hello cmdlet hello Network Watcher resource group, hello NetworkWatcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="27f18-123">V tomto příkladu je hello cílové IP adresy tooa virtuálních počítačů v jiné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="27f18-123">In this example, hello destination IP address is tooa VM in another virtual network.</span></span> <span data-ttu-id="27f18-124">Mezi hello dvě virtuální sítě je bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="27f18-124">There is a virtual network gateway between hello two virtual networks.</span></span>

<span data-ttu-id="27f18-125">Pokud nebyly dosud, nainstalujete a nakonfigurujete hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="27f18-125">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="27f18-126">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="27f18-126">Then run hello following command:</span></span>

```azurecli
az network watcher show-next-hop --resource-group <resourcegroupName> --vm <vmNameorID> --source-ip <source-ip> --dest-ip <destination-ip>

```

> [!NOTE]
<span data-ttu-id="27f18-127">Pokud hello virtuální počítač má několik síťových adaptérů a předávání IP je povolena na žádném z hello síťové adaptéry, pak hello parametr síťový adaptér (-i síťový adaptér id) musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="27f18-127">If hello VM has multiple NICs and IP forwarding is enabled on any of hello NICs, then hello NIC parameter (-i nic-id) must be specified.</span></span> <span data-ttu-id="27f18-128">V opačném případě je volitelný.</span><span class="sxs-lookup"><span data-stu-id="27f18-128">Otherwise it is optional.</span></span>

## <a name="review-results"></a><span data-ttu-id="27f18-129">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="27f18-129">Review results</span></span>

<span data-ttu-id="27f18-130">Po dokončení, jsou uvedené výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="27f18-130">When complete, hello results are provided.</span></span> <span data-ttu-id="27f18-131">IP adresa dalšího směrování Hello je i hello typ prostředku, který je vrácen.</span><span class="sxs-lookup"><span data-stu-id="27f18-131">hello next hop IP address is returned as well as hello type of resource it is.</span></span>

```azurecli
{
    "nextHopIpAddress": null,
    "nextHopType": "Internet",
    "routeTableId": "System Route"
}
```

<span data-ttu-id="27f18-132">Hello následující seznam obsahuje hello aktuálně k dispozici NextHopType hodnoty:</span><span class="sxs-lookup"><span data-stu-id="27f18-132">hello following list shows hello currently available NextHopType values:</span></span>

<span data-ttu-id="27f18-133">**Typ dalšího směrování**</span><span class="sxs-lookup"><span data-stu-id="27f18-133">**Next Hop Type**</span></span>

* <span data-ttu-id="27f18-134">Internet</span><span class="sxs-lookup"><span data-stu-id="27f18-134">Internet</span></span>
* <span data-ttu-id="27f18-135">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="27f18-135">VirtualAppliance</span></span>
* <span data-ttu-id="27f18-136">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="27f18-136">VirtualNetworkGateway</span></span>
* <span data-ttu-id="27f18-137">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="27f18-137">VnetLocal</span></span>
* <span data-ttu-id="27f18-138">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="27f18-138">HyperNetGateway</span></span>
* <span data-ttu-id="27f18-139">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="27f18-139">VnetPeering</span></span>
* <span data-ttu-id="27f18-140">Žádný</span><span class="sxs-lookup"><span data-stu-id="27f18-140">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="27f18-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27f18-141">Next steps</span></span>

<span data-ttu-id="27f18-142">Zjistěte, jak tooreview nastavení skupiny zabezpečení sítě prostřednictvím kódu programu přechodem [NSG auditování s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="27f18-142">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>
