---
title: "Azure RemoteApp šířka pásma sítě – obecné pokyny | Microsoft Docs"
description: "Pochopte některé základní síťové šířky pásma pokyny pro vaše kolekce vzdálené aplikace RemoteApp Azure a aplikace."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 529bf318-ae37-4c14-a11c-43fa703d68a3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 0b6521cc240556186890f0b797c6d80e431ac4be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a><span data-ttu-id="121b4-103">Azure RemoteApp šířka pásma sítě – obecné pokyny (Pokud nelze testovat vlastní)</span><span class="sxs-lookup"><span data-stu-id="121b4-103">Azure RemoteApp network bandwidth - general guidelines (if you can't test your own)</span></span>
> [!IMPORTANT]
> <span data-ttu-id="121b4-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="121b4-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="121b4-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="121b4-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="121b4-106">Pokud nemáte čas nebo možnost Spustit [testy šířky pásma sítě](remoteapp-bandwidthtests.md) pro Azure RemoteApp, tady jsou některé poměrně obecné pokyny, které vám může pomoct odhad šířky pásma sítě na uživatele.</span><span class="sxs-lookup"><span data-stu-id="121b4-106">If you do not have the time or capability to run the [network bandwidth tests](remoteapp-bandwidthtests.md) for Azure RemoteApp, here are some fairly generic guidelines that can help you estimate network bandwidth per user.</span></span>

<span data-ttu-id="121b4-107">Pokud máte kombinaci těchto scénářů, nedoporučujeme nic menší než (nebo rovno) 10 MB/s jako minimální šířka pásma sítě pro moderní aplikace připojené k Internetu ve vzdáleném prostředí.</span><span class="sxs-lookup"><span data-stu-id="121b4-107">If you have a mix of these scenarios, we don't recommend anything less than (or equal to) 10 MB/s as the MINIMUM network bandwidth for modern Internet-connected apps in a remote environment.</span></span> <span data-ttu-id="121b4-108">(I když popsané, to nebude zajistit vyšší než průměrná uživatelské prostředí.)</span><span class="sxs-lookup"><span data-stu-id="121b4-108">(Although, as discussed, this will not guarantee a better than average user experience.)</span></span>

## <a name="complex-powerpoint-simple-powerpoint"></a><span data-ttu-id="121b4-109">Komplexní PowerPoint jednoduché PowerPoint</span><span class="sxs-lookup"><span data-stu-id="121b4-109">Complex PowerPoint, simple PowerPoint</span></span>
<span data-ttu-id="121b4-110">Azure RemoteApp nepodporuje nejlépe v místní síti 100 MB.</span><span class="sxs-lookup"><span data-stu-id="121b4-110">Azure RemoteApp does best on 100 MB LAN.</span></span> <span data-ttu-id="121b4-111">V profilu sítě 10 MB/s kolísání výše 120 ms po více než 5 %, uživatel uvidí průměrná prostředí.</span><span class="sxs-lookup"><span data-stu-id="121b4-111">At the 10 MB/s network profile, when jitter above 120 ms is more than 5%, the user will see an average experience.</span></span> <span data-ttu-id="121b4-112">S rychlostí 1 MB/s, různými je výrazná – například prezentace, které uživatel nemusí zobrazit animovaný přechody vůbec protože rámce se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="121b4-112">At 1 MB/s the different is glaring - for example, in a slide show, the user might not see animated transitions at all because frames are skipped.</span></span>

## <a name="internet-explorer-mixed-pdf-pdf-text"></a><span data-ttu-id="121b4-113">Internet Explorer ve smíšeném, PDF, PDF, Text</span><span class="sxs-lookup"><span data-stu-id="121b4-113">Internet Explorer, mixed PDF, PDF, Text</span></span>
<span data-ttu-id="121b4-114">Profil síťového 10 MB/s je blízko LAN v většinu aspektů.</span><span class="sxs-lookup"><span data-stu-id="121b4-114">10 MB/s network profile is close to LAN in most aspects.</span></span> <span data-ttu-id="121b4-115">1 MB/s poskytne OK prostředí, i když existují některé zpoždění, co uživatel posune Přestože jsou bitové kopie na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="121b4-115">1 MB/s will provide an OK experience, although there may be some jitter when a user scrolls while there are images on the screen.</span></span>

## <a name="flash-video-youtube"></a><span data-ttu-id="121b4-116">Flash video (YouTube)</span><span class="sxs-lookup"><span data-stu-id="121b4-116">Flash video (YouTube)</span></span>
<span data-ttu-id="121b4-117">100 MB/s LAN přináší nejlepší výsledky, zatímco 10 MB/s je přijatelné (tj. jsme udržovat tempo s snímků za sekundu, ale zmenší se zvyšuje).</span><span class="sxs-lookup"><span data-stu-id="121b4-117">100 MB/s LAN provides the best experience, while 10 MB/s is acceptable (meaning we keep up with the frame rate but jitter increases).</span></span> <span data-ttu-id="121b4-118">Zpoždění v 1 MB/s, je velmi vysoké a znatelné.</span><span class="sxs-lookup"><span data-stu-id="121b4-118">At 1 MB/s, jitter is very high and noticeable.</span></span>

## <a name="word-typing-word-remote-input"></a><span data-ttu-id="121b4-119">Aplikace Word zadáním (Word vzdálené vstup)</span><span class="sxs-lookup"><span data-stu-id="121b4-119">Word typing (Word remote input)</span></span>
<span data-ttu-id="121b4-120">Toto je scénáři použití malou šířkou pásma.</span><span class="sxs-lookup"><span data-stu-id="121b4-120">This is a low-bandwidth usage scenario.</span></span> <span data-ttu-id="121b4-121">Na 256 KB/s poskytujeme stejně dobře z prostředí jako LAN.</span><span class="sxs-lookup"><span data-stu-id="121b4-121">At 256 KB/s we provide as good of an experience as LAN.</span></span>

## <a name="learn-more"></a><span data-ttu-id="121b4-122">Další informace</span><span class="sxs-lookup"><span data-stu-id="121b4-122">Learn more</span></span>
* [<span data-ttu-id="121b4-123">Odhadnout využití šířky pásma sítě Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="121b4-123">Estimate Azure RemoteApp network bandwidth usage</span></span>](remoteapp-bandwidth.md)
* [<span data-ttu-id="121b4-124">Azure RemoteApp – jak šířku pásma sítě a kvalitu prostředí pracovní společně?</span><span class="sxs-lookup"><span data-stu-id="121b4-124">Azure RemoteApp - how do network bandwidth and quality of experience work together?</span></span>](remoteapp-bandwidthexperience.md)
* [<span data-ttu-id="121b4-125">Azure RemoteApp - tseting využití šířky pásma sítě s některé běžné scénáře</span><span class="sxs-lookup"><span data-stu-id="121b4-125">Azure RemoteApp - tseting your network bandwidth usage with some common scenarios</span></span>](remoteapp-bandwidthtests.md)
