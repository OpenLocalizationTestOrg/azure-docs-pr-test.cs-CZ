---
title: "aaaProtecting síti v Azure Security Center | Microsoft Docs"
description: "Tento dokument adresy doporučení v Azure Security Center, které vám pomůžou chránit vaši síť Azure a zůstat souladu se zásadami zabezpečení."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 053738da432edf13b40172fb44d2044702dd8211
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-network-in-azure-security-center"></a><span data-ttu-id="aa4f6-103">Ochrana sítě v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="aa4f6-103">Protecting your network in Azure Security Center</span></span>
<span data-ttu-id="aa4f6-104">Azure Security Center analyzuje stav zabezpečení hello vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-104">Azure Security Center analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="aa4f6-105">Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří se doporučení, která vás provede procesem hello konfigurace hello potřebné ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-105">When Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through hello process of configuring hello needed controls.</span></span>  <span data-ttu-id="aa4f6-106">Doporučení se týkají tooAzure typy prostředků: virtuální počítače (VM), sítě, SQL a aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-106">Recommendations apply tooAzure resource types: virtual machines (VMs), networking, SQL, and applications.</span></span>

<span data-ttu-id="aa4f6-107">Tento článek se zaměřuje na doporučení, které se vztahují tooyour sítě.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-107">This article addresses recommendations that apply tooyour network.</span></span>  <span data-ttu-id="aa4f6-108">Síťového střediska doporučení ohledně další brány firewall generace, skupiny zabezpečení sítě, konfiguraci pravidla pro příchozí provoz a další.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-108">Network recommendations center around next generation firewalls, Network Security Groups, configuring inbound traffic rules, and more.</span></span>  <span data-ttu-id="aa4f6-109">Použití hello tabulce jako referenční toohelp porozumíte hello dostupná síťová doporučení a co každé z nich dělá, pokud ji použijete.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-109">Use hello table below as a reference toohelp you understand hello available network recommendations and what each one does if you apply it.</span></span>

## <a name="available-network-recommendations"></a><span data-ttu-id="aa4f6-110">Doporučení k dispozici sítě</span><span class="sxs-lookup"><span data-stu-id="aa4f6-110">Available network recommendations</span></span>
| <span data-ttu-id="aa4f6-111">Doporučení</span><span class="sxs-lookup"><span data-stu-id="aa4f6-111">Recommendation</span></span> | <span data-ttu-id="aa4f6-112">Popis</span><span class="sxs-lookup"><span data-stu-id="aa4f6-112">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="aa4f6-113">Přidání brány firewall příští generace</span><span class="sxs-lookup"><span data-stu-id="aa4f6-113">Add a Next Generation Firewall</span></span>](security-center-add-next-generation-firewall.md) |<span data-ttu-id="aa4f6-114">Doporučuje se, že přidáte brány Firewall pro další generace (NGFW) z tooincrease partnera Microsoft vaše ochranu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-114">Recommends that you add a Next Generation Firewall (NGFW) from a Microsoft partner tooincrease your security protections.</span></span> |
| [<span data-ttu-id="aa4f6-115">Směrování provozu jenom přes NGFW</span><span class="sxs-lookup"><span data-stu-id="aa4f6-115">Route traffic through NGFW only</span></span>](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |<span data-ttu-id="aa4f6-116">Doporučuje konfiguraci pravidla zabezpečení skupiny (NSG) sítě, které vynutí tooyour příchozí přenosy virtuálních počítačů prostřednictvím vaší NGFW.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-116">Recommends that you configure network security group (NSG) rules that force inbound traffic tooyour VM through your NGFW.</span></span> |
| [<span data-ttu-id="aa4f6-117">Povolení skupin zabezpečení sítě pro podsítě nebo virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="aa4f6-117">Enable Network Security Groups on subnets or virtual machines</span></span>](security-center-enable-network-security-groups.md) |<span data-ttu-id="aa4f6-118">Doporučuje, abyste povolili skupiny Nsg na podsítě nebo virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-118">Recommends that you enable NSGs on subnets or VMs.</span></span> |
| [<span data-ttu-id="aa4f6-119">Omezení přístupu prostřednictvím internetové koncový bod</span><span class="sxs-lookup"><span data-stu-id="aa4f6-119">Restrict access through Internet facing endpoint</span></span>](security-center-restrict-access-through-internet-facing-endpoints.md) |<span data-ttu-id="aa4f6-120">Doporučuje konfigurace pravidla pro příchozí provoz pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-120">Recommends that you configure inbound traffic rules for NSGs.</span></span> |

## <a name="see-also"></a><span data-ttu-id="aa4f6-121">Viz také</span><span class="sxs-lookup"><span data-stu-id="aa4f6-121">See also</span></span>
<span data-ttu-id="aa4f6-122">toolearn Další informace o doporučení, které se vztahují tooother typy prostředků Azure, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="aa4f6-122">toolearn more about recommendations that apply tooother Azure resource types, see hello following:</span></span>

* [<span data-ttu-id="aa4f6-123">Ochrana virtuálních počítačů v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="aa4f6-123">Protecting your virtual machines in Azure Security Center</span></span>](security-center-virtual-machine-recommendations.md)
* [<span data-ttu-id="aa4f6-124">Ochrana aplikací v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="aa4f6-124">Protecting your applications in Azure Security Center</span></span>](security-center-application-recommendations.md)
* [<span data-ttu-id="aa4f6-125">Ochrana služby Azure SQL v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="aa4f6-125">Protecting your Azure SQL service in Azure Security Center</span></span>](security-center-sql-service-recommendations.md)

<span data-ttu-id="aa4f6-126">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="aa4f6-126">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="aa4f6-127">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-127">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="aa4f6-128">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-128">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="aa4f6-129">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="aa4f6-129">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
