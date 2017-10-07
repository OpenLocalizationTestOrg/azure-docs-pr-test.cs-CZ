---
title: "aaaAn aplikace Proxy aplikací trvá příliš dlouho tooload | Microsoft Docs"
description: "Řešení potíží s výkonem načítání stránky s hello proxy aplikace služby Azure AD"
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
ms.openlocfilehash: 4c7a51f96840966a1d88933fa4e30f39479d8a5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="an-application-proxy-application-takes-too-long-tooload"></a><span data-ttu-id="141da-103">Aplikace Proxy aplikací trvá příliš dlouho tooload</span><span class="sxs-lookup"><span data-stu-id="141da-103">An Application Proxy application takes too long tooload</span></span>

<span data-ttu-id="141da-104">Tento článek vám pomůže toounderstand proč Azure AD Application Proxy aplikace může trvat dlouhou dobu tooload a co můžete provést tooresolve tento problém.</span><span class="sxs-lookup"><span data-stu-id="141da-104">This article help you toounderstand why an Azure AD Application Proxy application may take a long time tooload, and what you can do tooresolve this issue.</span></span>

## <a name="overview"></a><span data-ttu-id="141da-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="141da-105">Overview</span></span>
<span data-ttu-id="141da-106">Pokud vaše aplikace pracují, ale zobrazí dlouho latence, může být některé dílčí vylepšení v topologii vaší sítě, které můžete zvážit rychlost tooimprove hello.</span><span class="sxs-lookup"><span data-stu-id="141da-106">If your applications are working but you see a long latency, there may be some minor tweaks in your network topology that you can consider tooimprove hello speed.</span></span> <span data-ttu-id="141da-107">Vyhodnocení topologiemi, najdete v části hello [sítě aspekty dokumentu](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span><span class="sxs-lookup"><span data-stu-id="141da-107">For an evaluation of different topologies, see hello [network considerations document](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span></span>

<span data-ttu-id="141da-108">Pokud nemáte vám tyto aspekty, bohužel mít nemáme aktuálně Další doporučení pro optimalizaci výkonu.</span><span class="sxs-lookup"><span data-stu-id="141da-108">If those considerations don’t help, we unfortunately don’t have currently have further recommendations for performance tuning.</span></span> <span data-ttu-id="141da-109">Jako hello Proxy aplikace služby rozšíří toomore datových center, které mohou být blíže tooyou, může toosee vylepšené latence spustit přímo.</span><span class="sxs-lookup"><span data-stu-id="141da-109">As hello Application Proxy service expands toomore data centers that may be closer tooyou, you may start toosee improved latency directly.</span></span> <span data-ttu-id="141da-110">centra toosee hello úplný seznam dat Azure, uvidíte hello [latence zkušební stránku](http://www.azurespeed.com/Azure/Latency).</span><span class="sxs-lookup"><span data-stu-id="141da-110">toosee hello full list of Azure data centers, you can see hello [latency test page](http://www.azurespeed.com/Azure/Latency).</span></span> 

<span data-ttu-id="141da-111">Hello datových centrech s hello Proxy aplikace služby lze najít pomocí hello [nástroj pro testování porty konektor](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span><span class="sxs-lookup"><span data-stu-id="141da-111">hello data centers with hello Application Proxy service can be found with hello [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span></span> 

## <a name="feedback-on-application-proxy-data-center-locations"></a><span data-ttu-id="141da-112">Zpětná vazba týkající se umístění Proxy aplikací datových center</span><span class="sxs-lookup"><span data-stu-id="141da-112">Feedback on Application Proxy data center locations</span></span> 
<span data-ttu-id="141da-113">Může být Azure datových center, které zatím neobsahují Proxy aplikace, ale povede tooa skvělé latence zlepšování za vás.</span><span class="sxs-lookup"><span data-stu-id="141da-113">There may be Azure data centers that don’t as yet include Application Proxy but would lead tooa great latency improvement for you.</span></span> <span data-ttu-id="141da-114">Hello datového centra umístění v < aadapfeedback@microsoft.com > tak, jak jsme rozbalte jsme mohli používat tooplan vaší zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="141da-114">hello data center location at <aadapfeedback@microsoft.com> so we can use your feedback tooplan as we expand.</span></span>

<span data-ttu-id="141da-115">Jsme práce na některé další možnosti, které pomáhají zlepšit latenci hello pro klienty, kteří aktuálně najdete v části dlouho latenci a být zda tooshare dokumentaci, jakmile je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="141da-115">We are working on some additional capabilities that help improve hello latency for tenants that currently see long latencies, and be sure tooshare documentation once available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="141da-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="141da-116">Next steps</span></span>
[<span data-ttu-id="141da-117">Práce s existující místní proxy servery</span><span class="sxs-lookup"><span data-stu-id="141da-117">Work with existing on-premises proxy servers</span></span>](application-proxy-working-with-proxy-servers.md)
