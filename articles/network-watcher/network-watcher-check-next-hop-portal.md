---
title: "Další směrování aaaFind s Azure sítě sledovacích procesů další segment - portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak můžete najít co hello dalšího směrování typ je a hello ip adresu pomocí další směrování pomocí portálu Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b64a2a5275c15aa8bdb10601de4ae1504a9ab551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-hello-portal"></a><span data-ttu-id="8dbfd-103">Zjistěte, jaký typ hello dalšího směrování je díky funkci další segment hello v sledovací proces sítě Azure pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="8dbfd-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using hello portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="8dbfd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8dbfd-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="8dbfd-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8dbfd-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="8dbfd-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8dbfd-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="8dbfd-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8dbfd-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="8dbfd-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="8dbfd-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="8dbfd-109">Další směrování je funkce sledovací proces sítě, která poskytuje možnost hello získat hello typ dalšího směrování a IP adresu podle zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="8dbfd-110">Tato funkce je užitečná při určování, zda je brána, internet nebo cílové tooits tooget virtuální sítě prochází přes odchozího provozu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8dbfd-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8dbfd-111">Before you begin</span></span>

<span data-ttu-id="8dbfd-112">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-112">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="8dbfd-113">scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-113">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="8dbfd-114">Scénář</span><span class="sxs-lookup"><span data-stu-id="8dbfd-114">Scenario</span></span>

<span data-ttu-id="8dbfd-115">scénář Hello popsaná v tomto článku používá další směrování toofind hello typ dalšího směrování a IP adresu pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-115">hello scenario covered in this article uses Next hop toofind out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="8dbfd-116">toolearn Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8dbfd-116">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

<span data-ttu-id="8dbfd-117">V tomto scénáři provedete následující:</span><span class="sxs-lookup"><span data-stu-id="8dbfd-117">In this scenario, you will:</span></span>

* <span data-ttu-id="8dbfd-118">Další směrování hello načíst z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-118">Retrieve hello next hop from a virtual machine.</span></span>

## <a name="get-next-hop"></a><span data-ttu-id="8dbfd-119">Získat další směrování</span><span class="sxs-lookup"><span data-stu-id="8dbfd-119">Get Next Hop</span></span>

### <a name="step-1"></a><span data-ttu-id="8dbfd-120">Krok 1</span><span class="sxs-lookup"><span data-stu-id="8dbfd-120">Step 1</span></span>

<span data-ttu-id="8dbfd-121">Přejděte tooyour sledovací proces sítě prostředku v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-121">Navigate tooyour Network Watcher resource in hello Azure portal.</span></span>

### <a name="step-2"></a><span data-ttu-id="8dbfd-122">Krok 2</span><span class="sxs-lookup"><span data-stu-id="8dbfd-122">Step 2</span></span>

<span data-ttu-id="8dbfd-123">Klikněte na tlačítko **dalšího směrování** hello navigačním podokně, vyberte hello virtuálního počítače a síťové rozhraní, vyplňte hello zdrojové a cílové IP a klikněte na tlačítko hello **dalšího směrování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-123">Click **Next hop** in hello navigation pane, select hello virtual machine and network interface, fill out hello source and destination IP, and click hello **Next hop** button.</span></span>

> [!NOTE]
> <span data-ttu-id="8dbfd-124">Další směrování vyžaduje, aby hello prostředků virtuálního počítače je přidělená toorun.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-124">Next hop requires that hello VM resource is allocated toorun.</span></span>

![získat další směrování – přehled][1]

### <a name="step-3"></a><span data-ttu-id="8dbfd-126">Krok 3</span><span class="sxs-lookup"><span data-stu-id="8dbfd-126">Step 3</span></span>

<span data-ttu-id="8dbfd-127">Po dokončení úloh hello jsou uvedené výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-127">Once hello task is complete, hello results are provided.</span></span> <span data-ttu-id="8dbfd-128">Hello IP adresy a typ zařízení hello dalšího směrování je, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-128">hello IP address and type of device hello next hop is, is displayed.</span></span> <span data-ttu-id="8dbfd-129">Hello následující tabulce jsou uvedeny dostupné vrácené hodnoty hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-129">hello following table shows hello available returned values in hello portal.</span></span>

<span data-ttu-id="8dbfd-130">**Typ dalšího směrování**</span><span class="sxs-lookup"><span data-stu-id="8dbfd-130">**Next Hop Type**</span></span>

* <span data-ttu-id="8dbfd-131">Internet</span><span class="sxs-lookup"><span data-stu-id="8dbfd-131">Internet</span></span>
* <span data-ttu-id="8dbfd-132">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="8dbfd-132">VirtualAppliance</span></span>
* <span data-ttu-id="8dbfd-133">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="8dbfd-133">VirtualNetworkGateway</span></span>
* <span data-ttu-id="8dbfd-134">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="8dbfd-134">VnetLocal</span></span>
* <span data-ttu-id="8dbfd-135">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="8dbfd-135">HyperNetGateway</span></span>
* <span data-ttu-id="8dbfd-136">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="8dbfd-136">VnetPeering</span></span>
* <span data-ttu-id="8dbfd-137">Žádný</span><span class="sxs-lookup"><span data-stu-id="8dbfd-137">None</span></span>

<span data-ttu-id="8dbfd-138">Pokud vlastní trasy použité tooroute tento provoz hello trasy definované uživatelem (UDR) se také zobrazí s výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="8dbfd-138">If a custom route was used tooroute this traffic, hello User-defined route (UDR) is shown as well with hello results.</span></span>

![získat další směrování výsledky][2]

## <a name="next-steps"></a><span data-ttu-id="8dbfd-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8dbfd-140">Next steps</span></span>

<span data-ttu-id="8dbfd-141">Zjistěte, jak tooreview nastavení skupiny zabezpečení sítě prostřednictvím kódu programu přechodem [NSG auditování s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="8dbfd-141">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png














