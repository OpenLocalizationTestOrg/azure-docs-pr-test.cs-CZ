---
title: "Aplikace Proxy aplikací trvá příliš dlouho načíst | Microsoft Docs"
description: "Řešení potíží s výkonem načítání stránky s Azure AD Application Proxy"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: ce462c90746e6af0dc201686557121665b82b93d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a><span data-ttu-id="82171-103">Aplikace Proxy aplikací trvá příliš dlouho zatížení</span><span class="sxs-lookup"><span data-stu-id="82171-103">An Application Proxy application takes too long to load</span></span>

<span data-ttu-id="82171-104">Tento článek vám pomůže lépe porozumět proč Azure AD Application Proxy aplikace může trvat dlouhou dobu zatížení, a můžete provést k vyřešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="82171-104">This article help you to understand why an Azure AD Application Proxy application may take a long time to load, and what you can do to resolve this issue.</span></span>

## <a name="overview"></a><span data-ttu-id="82171-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="82171-105">Overview</span></span>
<span data-ttu-id="82171-106">Pokud vaše aplikace pracují, ale zobrazí dlouho latence, může být některé dílčí vylepšení v topologii sítě, můžete zvážit urychlit.</span><span class="sxs-lookup"><span data-stu-id="82171-106">If your applications are working but you see a long latency, there may be some minor tweaks in your network topology that you can consider to improve the speed.</span></span> <span data-ttu-id="82171-107">Vyhodnocení topologiemi, najdete v článku [sítě aspekty dokumentu](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span><span class="sxs-lookup"><span data-stu-id="82171-107">For an evaluation of different topologies, see the [network considerations document](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span></span>

<span data-ttu-id="82171-108">Pokud nemáte vám tyto aspekty, bohužel mít nemáme aktuálně Další doporučení pro optimalizaci výkonu.</span><span class="sxs-lookup"><span data-stu-id="82171-108">If those considerations don’t help, we unfortunately don’t have currently have further recommendations for performance tuning.</span></span> <span data-ttu-id="82171-109">Jak služba Proxy aplikace zasahuje do více datových center, které mohou být blíž, můžete začít přímo zobrazíte zlepšení latence.</span><span class="sxs-lookup"><span data-stu-id="82171-109">As the Application Proxy service expands to more data centers that may be closer to you, you may start to see improved latency directly.</span></span> <span data-ttu-id="82171-110">Pokud chcete zobrazit úplný seznam datových center Azure, uvidíte [latence zkušební stránku](http://www.azurespeed.com/Azure/Latency).</span><span class="sxs-lookup"><span data-stu-id="82171-110">To see the full list of Azure data centers, you can see the [latency test page](http://www.azurespeed.com/Azure/Latency).</span></span> 

<span data-ttu-id="82171-111">Datových centrech s Proxy aplikace služby lze najít pomocí [nástroj pro testování porty konektor](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span><span class="sxs-lookup"><span data-stu-id="82171-111">The data centers with the Application Proxy service can be found with the [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span></span> 

## <a name="feedback-on-application-proxy-data-center-locations"></a><span data-ttu-id="82171-112">Zpětná vazba týkající se umístění Proxy aplikací datových center</span><span class="sxs-lookup"><span data-stu-id="82171-112">Feedback on Application Proxy data center locations</span></span> 
<span data-ttu-id="82171-113">Může být Azure datových center, které zatím neobsahují Proxy aplikace, ale vedlo ke zlepšování skvělé latence pro vás.</span><span class="sxs-lookup"><span data-stu-id="82171-113">There may be Azure data centers that don’t as yet include Application Proxy but would lead to a great latency improvement for you.</span></span> <span data-ttu-id="82171-114">Datovém centru umístění v < aadapfeedback@microsoft.com > , můžeme použít svůj názor na plánu, jak jsme rozbalte.</span><span class="sxs-lookup"><span data-stu-id="82171-114">The data center location at <aadapfeedback@microsoft.com> so we can use your feedback to plan as we expand.</span></span>

<span data-ttu-id="82171-115">Jsme práce na některé další možnosti, které pomáhají zlepšit latenci pro klienty, kteří aktuálně najdete v části dlouho latenci a nezapomeňte sdílet dokumentaci, jakmile je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="82171-115">We are working on some additional capabilities that help improve the latency for tenants that currently see long latencies, and be sure to share documentation once available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="82171-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82171-116">Next steps</span></span>
[<span data-ttu-id="82171-117">Práce s existující místní proxy servery</span><span class="sxs-lookup"><span data-stu-id="82171-117">Work with existing on-premises proxy servers</span></span>](application-proxy-working-with-proxy-servers.md)
