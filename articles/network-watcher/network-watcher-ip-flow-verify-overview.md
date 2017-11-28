---
title: "tok tooIP aaaIntroduction ověřit v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled o hello IP sledovací proces sítě ověřte tok funkce"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b648a4816a7ffdc6ca54462944b574e2395e8298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooip-flow-verify-in-azure-network-watcher"></a><span data-ttu-id="5a2d2-103">Úvod tooIP toku ověřit v sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="5a2d2-103">Introduction tooIP flow verify in Azure Network Watcher</span></span>

<span data-ttu-id="5a2d2-104">Tok IP ověřte kontroluje, zda paket povolený nebo zakázaný tooor z virtuálního počítače na základě informací o 5 řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-104">IP flow verify checks if a packet is allowed or denied tooor from a virtual machine based on 5-tuple information.</span></span> <span data-ttu-id="5a2d2-105">Tyto informace se skládá z směr, protokol, místní IP, vzdálené IP, místního portu a vzdáleného portu.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-105">This information consists of direction, protocol, local IP, remote IP, local port, and remote port.</span></span> <span data-ttu-id="5a2d2-106">Pokud paket hello je zakázané skupiny zabezpečení, je vrácen název hello hello pravidlo, které odepřen hello paketů.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-106">If hello packet is denied by a security group, hello name of hello rule that denied hello packet is returned.</span></span> <span data-ttu-id="5a2d2-107">Zatímco můžete vybrat všechny zdrojové i cílové adresy IP, tato funkce vám pomůže rychle diagnostikovat problémy s připojením z nebo toohello správci internet a z nebo toohello v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-107">While any source or destination IP can be chosen, this feature helps administrators quickly diagnose connectivity issues from or toohello internet and from or toohello on-premises environment.</span></span>

<span data-ttu-id="5a2d2-108">Tok IP ověřte cílem síťové rozhraní virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-108">IP flow verify targets a network interface of a virtual machine.</span></span> <span data-ttu-id="5a2d2-109">Tok přenosů dat je pak ověřit podle hello nakonfigurované nastavení tooor z rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-109">Traffic flow is then verified based on hello configured settings tooor from that network interface.</span></span> <span data-ttu-id="5a2d2-110">Tato možnost je užitečná při potvrzení, zda pravidla v skupinu zabezpečení sítě neblokuje inzerování vstupních nebo výstupních tooor provoz z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-110">This capability is useful in confirming if a rule in a Network Security Group is blocking ingress or egress traffic tooor from a virtual machine.</span></span>

<span data-ttu-id="5a2d2-111">Instance toobe potřebám sledovací proces sítě vytvořené v všech oblastech, abyste naplánovali toorun IP tok ověření.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-111">An instance of Network Watcher needs toobe created in all regions that you plan toorun IP flow verify.</span></span> <span data-ttu-id="5a2d2-112">Sledovací proces sítě je služba, místní a může být spustili jenom s prostředky v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-112">Network Watcher is a regional service and can only be ran against resources in hello same region.</span></span> <span data-ttu-id="5a2d2-113">To nemá vliv hello výsledky IP toku ověřte jako hello trasy přidružené hello síťový adaptér bude stále vrácen.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-113">This does not affect hello results of IP flow verify as hello route associated with hello NIC will still be returned.</span></span>

![1][1]

## <a name="next-steps"></a><span data-ttu-id="5a2d2-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a2d2-115">Next steps</span></span>

<span data-ttu-id="5a2d2-116">Navštivte hello následující článek toolearn, pokud je paket povolený nebo zakázaný pro konkrétní virtuální počítač prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="5a2d2-116">Visit hello following article toolearn if a packet is allowed or denied for a specific virtual machine through hello portal.</span></span> [<span data-ttu-id="5a2d2-117">Zkontrolovat, pokud provoz je povolené na virtuálním počítači s IP tok ověření pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="5a2d2-117">Check if traffic is allowed on a VM with IP Flow Verify using hello portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












