---
title: "aaaIntroduction toonext směrování v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled hello sledovací proces sítě další funkce směrování"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 916af736d0d52abadeafed746f0f8a0173b11033
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonext-hop-in-azure-network-watcher"></a><span data-ttu-id="ead33-103">Úvod toonext směrování v sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="ead33-103">Introduction toonext hop in Azure Network Watcher</span></span>

<span data-ttu-id="ead33-104">Přenosy z virtuálního počítače se budou odesílat tooa cíl v závislosti na hello účinné postupy spojené s síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="ead33-104">Traffic from a VM is sent tooa destination based on hello effective routes associated with a NIC.</span></span> <span data-ttu-id="ead33-105">Další směrování získá hello typ dalšího směrování a IP adresa z paketu z konkrétní virtuální počítač a síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="ead33-105">Next hop gets hello next hop type and IP address of a packet from a specific virtual machine and NIC.</span></span> <span data-ttu-id="ead33-106">To pomáhá toodetermine, pokud hello paketů se směrovanou toohello cílové nebo je dírkového hello provoz probíhá černé.</span><span class="sxs-lookup"><span data-stu-id="ead33-106">This helps toodetermine if hello packet is being directed toohello destination or is hello traffic being black holed.</span></span> <span data-ttu-id="ead33-107">Nesprávná konfigurace trasy uživatelem hello, kde je přenosem směrovanou tooan místní umístění nebo virtuální zařízení, může způsobit problémy s tooconnectivity.</span><span class="sxs-lookup"><span data-stu-id="ead33-107">An improper configuration of routes by hello user, where a traffic is directed tooan on-premises location or a virtual appliance, can lead tooconnectivity issues.</span></span> <span data-ttu-id="ead33-108">Další směrování také vrátí hodnotu hello směrovací tabulce přidružené k dalším místě směrování hello.</span><span class="sxs-lookup"><span data-stu-id="ead33-108">Next hop also returns hello route table associated with hello next hop.</span></span> <span data-ttu-id="ead33-109">Při dotazování další směrování, pokud trasu hello je definován jako trasy definované uživatelem, bude vrácen danou trasu.</span><span class="sxs-lookup"><span data-stu-id="ead33-109">When querying a next hop if hello route is defined as a user-defined route, that route will be returned.</span></span> <span data-ttu-id="ead33-110">V opačném případě vrátí další segment "Systémová trasa".</span><span class="sxs-lookup"><span data-stu-id="ead33-110">Otherwise Next hop returns "System Route".</span></span>

![dalšího směrování – přehled][1]

<span data-ttu-id="ead33-112">Hello následuje seznam hello další směrování typy, které mohou být vráceny při dotazování dalším místě směrování.</span><span class="sxs-lookup"><span data-stu-id="ead33-112">hello following is a list of hello next hop types that can be returned when querying Next hop.</span></span>

* <span data-ttu-id="ead33-113">Internet</span><span class="sxs-lookup"><span data-stu-id="ead33-113">Internet</span></span>
* <span data-ttu-id="ead33-114">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="ead33-114">VirtualAppliance</span></span>
* <span data-ttu-id="ead33-115">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="ead33-115">VirtualNetworkGateway</span></span>
* <span data-ttu-id="ead33-116">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="ead33-116">VnetLocal</span></span>
* <span data-ttu-id="ead33-117">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="ead33-117">HyperNetGateway</span></span>
* <span data-ttu-id="ead33-118">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="ead33-118">VnetPeering</span></span>
* <span data-ttu-id="ead33-119">Žádný</span><span class="sxs-lookup"><span data-stu-id="ead33-119">None</span></span>

### <a name="next-steps"></a><span data-ttu-id="ead33-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ead33-120">Next steps</span></span>

<span data-ttu-id="ead33-121">Zjistěte, jak toouse další směrování toofind problémy s připojením k síti navštivte stránky [kontrola hello dalšího přechodu na virtuálním počítači](network-watcher-check-next-hop-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ead33-121">Learn how toouse next hop toofind issues with network connectivity by visiting [Check hello next hop on a VM](network-watcher-check-next-hop-portal.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













