---
title: "Úvod do dalšího směrování v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled sledovací proces sítě další funkce směrování"
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
ms.openlocfilehash: 5dd65d2418cae206965a13013dd990b916ad0733
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-next-hop-in-azure-network-watcher"></a><span data-ttu-id="2fb90-103">Úvod do dalšího směrování v sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="2fb90-103">Introduction to next hop in Azure Network Watcher</span></span>

<span data-ttu-id="2fb90-104">Přenosy z virtuálního počítače se odesílají do cílového umístění v závislosti na účinné postupy spojené s síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="2fb90-104">Traffic from a VM is sent to a destination based on the effective routes associated with a NIC.</span></span> <span data-ttu-id="2fb90-105">Další směrování získá typ dalšího směrování a IP adresa z paketu z konkrétní virtuální počítač a síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="2fb90-105">Next hop gets the next hop type and IP address of a packet from a specific virtual machine and NIC.</span></span> <span data-ttu-id="2fb90-106">To pomůže určete, jestli je paket směrovat do cílového umístění nebo je provoz probíhá černým dírkového.</span><span class="sxs-lookup"><span data-stu-id="2fb90-106">This helps to determine if the packet is being directed to the destination or is the traffic being black holed.</span></span> <span data-ttu-id="2fb90-107">Nesprávná konfigurace trasy uživatelem, kde je přesměrován na místní umístění nebo virtuální zařízení provoz, může způsobit problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="2fb90-107">An improper configuration of routes by the user, where a traffic is directed to an on-premises location or a virtual appliance, can lead to connectivity issues.</span></span> <span data-ttu-id="2fb90-108">Další směrování také vrátí hodnotu směrovací tabulka přidružené k dalším místě směrování.</span><span class="sxs-lookup"><span data-stu-id="2fb90-108">Next hop also returns the route table associated with the next hop.</span></span> <span data-ttu-id="2fb90-109">Při dotazování další směrování, pokud trasy, která je definována jako trasy definované uživatelem, bude vrácen danou trasu.</span><span class="sxs-lookup"><span data-stu-id="2fb90-109">When querying a next hop if the route is defined as a user-defined route, that route will be returned.</span></span> <span data-ttu-id="2fb90-110">V opačném případě vrátí další segment "Systémová trasa".</span><span class="sxs-lookup"><span data-stu-id="2fb90-110">Otherwise Next hop returns "System Route".</span></span>

![dalšího směrování – přehled][1]

<span data-ttu-id="2fb90-112">Následuje seznam další typy směrování, které mohou být vráceny při dotazování dalším místě směrování.</span><span class="sxs-lookup"><span data-stu-id="2fb90-112">The following is a list of the next hop types that can be returned when querying Next hop.</span></span>

* <span data-ttu-id="2fb90-113">Internet</span><span class="sxs-lookup"><span data-stu-id="2fb90-113">Internet</span></span>
* <span data-ttu-id="2fb90-114">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="2fb90-114">VirtualAppliance</span></span>
* <span data-ttu-id="2fb90-115">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="2fb90-115">VirtualNetworkGateway</span></span>
* <span data-ttu-id="2fb90-116">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="2fb90-116">VnetLocal</span></span>
* <span data-ttu-id="2fb90-117">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="2fb90-117">HyperNetGateway</span></span>
* <span data-ttu-id="2fb90-118">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="2fb90-118">VnetPeering</span></span>
* <span data-ttu-id="2fb90-119">Žádný</span><span class="sxs-lookup"><span data-stu-id="2fb90-119">None</span></span>

### <a name="next-steps"></a><span data-ttu-id="2fb90-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2fb90-120">Next steps</span></span>

<span data-ttu-id="2fb90-121">Další informace o použití další segment najít problémy s připojením k síti, navštivte stránky [zkontrolujte další směrování na virtuálním počítači](network-watcher-check-next-hop-portal.md)</span><span class="sxs-lookup"><span data-stu-id="2fb90-121">Learn how to use next hop to find issues with network connectivity by visiting [Check the next hop on a VM](network-watcher-check-next-hop-portal.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













