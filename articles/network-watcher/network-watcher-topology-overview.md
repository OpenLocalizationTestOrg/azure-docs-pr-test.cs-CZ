---
title: "Úvod do topologii sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled možností topologie sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e753a435-38e0-482b-846b-121cb547555c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 42443f614b76b8180ac163b9889163021adbf048
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-topology-in-azure-network-watcher"></a><span data-ttu-id="5523c-103">Úvod do topologii sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="5523c-103">Introduction to topology in Azure Network Watcher</span></span>

<span data-ttu-id="5523c-104">Topologie vrátí graf síťovým prostředkům ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="5523c-104">Topology returns a graph of network resources in a virtual network.</span></span> <span data-ttu-id="5523c-105">Graf znázorňuje propojení mezi prostředky představující koncové síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="5523c-105">The graph depicts the interconnection between the resources to represent the end to end network connectivity.</span></span>

![Přehled topologie][1]

<span data-ttu-id="5523c-107">Na portálu, vrátí topologie objektů prostředků v na základě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="5523c-107">In the portal, Topology returns the resource objects on a per virtual network basis.</span></span> <span data-ttu-id="5523c-108">Vztahy, jsou použité v ukázkách linkami mezi prostředky prostředky mimo oblast sledovací proces sítě, i když v prostředku skupiny se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="5523c-108">The relationships are depicted by lines between the resources Resources outside of the Network Watcher region, even if in the resource group will not be displayed.</span></span> <span data-ttu-id="5523c-109">Prostředky, vrátí se v zobrazení portálu, jsou podmnožinou síťové součásti, které vykreslovacích.</span><span class="sxs-lookup"><span data-stu-id="5523c-109">The resources returned in the portal view are a subset of the networking components that are graphed.</span></span> <span data-ttu-id="5523c-110">Pokud chcete zobrazit úplný seznam síťových prostředků, můžete použít [prostředí PowerShell](network-watcher-topology-powershell.md) nebo [REST](network-watcher-topology-rest.md)</span><span class="sxs-lookup"><span data-stu-id="5523c-110">To see the full list of networking resources you can use [PowerShell](network-watcher-topology-powershell.md) or [REST](network-watcher-topology-rest.md)</span></span>

> [!NOTE]
> <span data-ttu-id="5523c-111">V každé oblasti, kterou chcete spustit topologie na je vyžadován instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="5523c-111">An instance of Network Watcher is required in each region that you want to run Topology on.</span></span>

<span data-ttu-id="5523c-112">Prostředky jsou vrácený připojení mezi nimi jsou modelovat v dva vztahy.</span><span class="sxs-lookup"><span data-stu-id="5523c-112">As resources are returned the connection between them are modeled under two relationships.</span></span>

- <span data-ttu-id="5523c-113">**Členství ve skupině** – příklad: virtuální síť obsahuje podsítě, který obsahuje síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="5523c-113">**Containment** - Example: Virtual Network contains a Subnet which contains a NIC</span></span>
- <span data-ttu-id="5523c-114">**Související** – příklad: síťový adaptér A je přidružený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="5523c-114">**Associated** - Example: A NIC is associated with a VM</span></span>

### <a name="next-steps"></a><span data-ttu-id="5523c-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5523c-115">Next steps</span></span>

<span data-ttu-id="5523c-116">Další informace o použití prostředí PowerShell k načtení zobrazení topologie navštivte stránky [sledovací proces sítě topologie pomocí prostředí PowerShell](network-watcher-topology-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="5523c-116">Learn how to use PowerShell to retrieve the Topology view by visiting [Network Watcher topology with PowerShell](network-watcher-topology-powershell.md)</span></span>

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
