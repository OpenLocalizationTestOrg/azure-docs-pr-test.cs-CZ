---
title: "aaaIntroduction tootopology v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled možností topologie hello sledovací proces sítě"
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
ms.openlocfilehash: 7fa1c5518e4a25a5db999d898a9ee19fd0121db7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tootopology-in-azure-network-watcher"></a><span data-ttu-id="f56ea-103">Úvod tootopology v sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="f56ea-103">Introduction tootopology in Azure Network Watcher</span></span>

<span data-ttu-id="f56ea-104">Topologie vrátí graf síťovým prostředkům ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="f56ea-104">Topology returns a graph of network resources in a virtual network.</span></span> <span data-ttu-id="f56ea-105">Hello graf znázorňuje hello propojení mezi hello prostředky toorepresent hello end tooend připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="f56ea-105">hello graph depicts hello interconnection between hello resources toorepresent hello end tooend network connectivity.</span></span>

![Přehled topologie][1]

<span data-ttu-id="f56ea-107">Topologie hello portálu, vrátí hello objektů prostředků v na základě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="f56ea-107">In hello portal, Topology returns hello resource objects on a per virtual network basis.</span></span> <span data-ttu-id="f56ea-108">vztahy Hello, jsou použité v ukázkách linkami mezi prostředky hello prostředky mimo oblast hello sledovací proces sítě, i v případě, že v hello prostředku skupiny se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="f56ea-108">hello relationships are depicted by lines between hello resources Resources outside of hello Network Watcher region, even if in hello resource group will not be displayed.</span></span> <span data-ttu-id="f56ea-109">Hello prostředky, vrátí se v zobrazení portálu hello jsou podmnožinou hello síťové součásti, které vykreslovacích.</span><span class="sxs-lookup"><span data-stu-id="f56ea-109">hello resources returned in hello portal view are a subset of hello networking components that are graphed.</span></span> <span data-ttu-id="f56ea-110">Úplný seznam toosee hello síťové prostředky můžete použít [prostředí PowerShell](network-watcher-topology-powershell.md) nebo [REST](network-watcher-topology-rest.md)</span><span class="sxs-lookup"><span data-stu-id="f56ea-110">toosee hello full list of networking resources you can use [PowerShell](network-watcher-topology-powershell.md) or [REST](network-watcher-topology-rest.md)</span></span>

> [!NOTE]
> <span data-ttu-id="f56ea-111">V každé oblasti, které chcete toorun topologie na je vyžadován instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="f56ea-111">An instance of Network Watcher is required in each region that you want toorun Topology on.</span></span>

<span data-ttu-id="f56ea-112">Prostředky jsou vrácený hello připojení mezi nimi jsou modelovat v dva vztahy.</span><span class="sxs-lookup"><span data-stu-id="f56ea-112">As resources are returned hello connection between them are modeled under two relationships.</span></span>

- <span data-ttu-id="f56ea-113">**Členství ve skupině** – příklad: virtuální síť obsahuje podsítě, který obsahuje síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="f56ea-113">**Containment** - Example: Virtual Network contains a Subnet which contains a NIC</span></span>
- <span data-ttu-id="f56ea-114">**Související** – příklad: síťový adaptér A je přidružený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="f56ea-114">**Associated** - Example: A NIC is associated with a VM</span></span>

### <a name="next-steps"></a><span data-ttu-id="f56ea-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f56ea-115">Next steps</span></span>

<span data-ttu-id="f56ea-116">Zjistěte, jak toouse prostředí PowerShell tooretrieve hello topologie zobrazit tak, že navštívíte [sledovací proces sítě topologie pomocí prostředí PowerShell](network-watcher-topology-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="f56ea-116">Learn how toouse PowerShell tooretrieve hello Topology view by visiting [Network Watcher topology with PowerShell](network-watcher-topology-powershell.md)</span></span>

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
