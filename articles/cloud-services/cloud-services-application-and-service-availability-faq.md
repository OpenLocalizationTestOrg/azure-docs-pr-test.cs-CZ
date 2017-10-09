---
title: "problémy s dostupností aaaApplication a služby pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "V tomto článku jsou uvedené nejčastější dotazy o aplikaci a dostupnost služeb pro Microsoft Azure Cloud Services hello."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: cd0d9ba0beb1dc72d4761f8b89c2ecadb51c7e48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a><span data-ttu-id="66a63-103">Aplikace a problémy s dostupností služby pro Azure Cloud Services: Časté otázky (FAQ)</span><span class="sxs-lookup"><span data-stu-id="66a63-103">Application and service availability issues for Azure Cloud Services: Frequently asked questions (FAQs)</span></span>

<span data-ttu-id="66a63-104">Tento článek obsahuje nejčastější dotazy týkající se aplikace a problémy s dostupností služby pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services).</span><span class="sxs-lookup"><span data-stu-id="66a63-104">This article includes frequently asked questions about application and service availability issues for [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services).</span></span> <span data-ttu-id="66a63-105">Můžete také obrátit hello [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.</span><span class="sxs-lookup"><span data-stu-id="66a63-105">You can also consult hello [Cloud Services VM Size page](cloud-services-sizes-specs.md) for size information.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a><span data-ttu-id="66a63-106">Moje role tu recykluje.</span><span class="sxs-lookup"><span data-stu-id="66a63-106">My role got recycled.</span></span> <span data-ttu-id="66a63-107">Se nějakou aktualizaci nasazuje pro moje Cloudová služba?</span><span class="sxs-lookup"><span data-stu-id="66a63-107">Was there any update rolled out for my cloud service?</span></span>
<span data-ttu-id="66a63-108">Přibližně jednou za měsíc, společnost Microsoft vydá nové verze hostovaného operačního systému pro virtuální počítače Windows Azure PaaS.</span><span class="sxs-lookup"><span data-stu-id="66a63-108">Roughly once a month, Microsoft releases a new Guest OS version for Windows Azure PaaS VMs.</span></span><span data-ttu-id="66a63-109"> Hostovaného operačního systému je jenom jedna taková aktualizace v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="66a63-109"> The Guest OS is only one such update in hello pipeline.</span></span> <span data-ttu-id="66a63-110">Verze může mít vliv mnoho dalších faktorů.</span><span class="sxs-lookup"><span data-stu-id="66a63-110">A release can be affected by many other factors.</span></span> <span data-ttu-id="66a63-111">Kromě toho Azure je spuštěná na stovky tisíc počítačů.</span><span class="sxs-lookup"><span data-stu-id="66a63-111">In addition, Azure runs on hundreds of thousands of machines.</span></span> <span data-ttu-id="66a63-112">Proto je možné toopredict hello přesné datum a čas, kdy se restartuje vaše role.</span><span class="sxs-lookup"><span data-stu-id="66a63-112">Therefore, it's impossible toopredict hello exact date and time when your roles will reboot.</span></span> <span data-ttu-id="66a63-113">Aktualizujeme s hello nejnovější informace, které jsme hello hostovaného operačního systému aktualizujte informačního kanálu RSS, ale byste měli zvážit, hlášena čas toobe přibližná hodnota.</span><span class="sxs-lookup"><span data-stu-id="66a63-113">We update hello Guest OS Update RSS Feed with hello latest information that we have, but you should consider that reported time toobe an approximate value.</span></span> <span data-ttu-id="66a63-114">Jsme víte, že se jedná o problém pro zákazníky a pracují toolimit plán nebo přesněji čas restartování.</span><span class="sxs-lookup"><span data-stu-id="66a63-114">We are aware that this is problematic for customers and are working on a plan toolimit or precisely time reboots.</span></span>

<span data-ttu-id="66a63-115">Kompletní informace o nejnovějších aktualizacích hostovaného operačního systému najdete v tématu [verze hostovaného operačního systému Azure a kompatibilních sad SDK](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="66a63-115">For complete details about recent Guest OS updates, see [Azure Guest OS releases and SDK compatibility matrix](cloud-services-guestos-update-matrix.md).</span></span>

<span data-ttu-id="66a63-116">Užitečné informace o restartování a ukazatele tootechnical podrobnosti aktualizace hosta a hostitelským operačním systémem, najdete v příspěvku blogu MSDN hello [restartuje kvůli Role Instance upgrady tooOS](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).</span><span class="sxs-lookup"><span data-stu-id="66a63-116">For helpful information on restarts and pointers tootechnical details of Guest and Host OS updates, see hello MSDN blog post [Role Instance Restarts Due tooOS Upgrades](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).</span></span>

## <a name="why-does-hello-first-request-toomy-cloud-service-after-hello-service-has-been-idle-for-some-time-take-longer-than-usual"></a><span data-ttu-id="66a63-117">Proč hello první požadavek toomy cloudové služby po hello služby bylo nečinné po určitý čas trvat déle než obvykle?</span><span class="sxs-lookup"><span data-stu-id="66a63-117">Why does hello first request toomy cloud service after hello service has been idle for some time take longer than usual?</span></span>
<span data-ttu-id="66a63-118">Jakmile hello Webový Server obdrží požadavek na první hello, nejprve znovu zkompiluje hello kód a potom zpracuje žádost o hello.</span><span class="sxs-lookup"><span data-stu-id="66a63-118">When hello Web Server receives hello first request, it first recompiles hello code and then processes hello request.</span></span> <span data-ttu-id="66a63-119">Který je proč hello první požadavek trvá déle než hello ostatní.</span><span class="sxs-lookup"><span data-stu-id="66a63-119">That's why hello first request takes longer than hello others.</span></span> <span data-ttu-id="66a63-120">Ve výchozím nastavení získá hello fondu aplikací v případech nečinnosti uživatele vypnout.</span><span class="sxs-lookup"><span data-stu-id="66a63-120">By default, hello app pool gets shut down in cases of user inactivity.</span></span> <span data-ttu-id="66a63-121">fond aplikací Hello bude taky recyklaci ve výchozím nastavení každých 1,740 minut (29 hodiny).</span><span class="sxs-lookup"><span data-stu-id="66a63-121">hello app pool will also recycle by default every 1,740 minutes (29 hours).</span></span>

<span data-ttu-id="66a63-122">Fondy aplikací Internetové informační služby (IIS) může být pravidelně recykluje tooavoid nestabilním stavy, které můžou vést tooapplication havárie, zablokování nebo nevracení paměti.</span><span class="sxs-lookup"><span data-stu-id="66a63-122">Internet Information Services (IIS) application pools can be periodically recycled tooavoid unstable states that can lead tooapplication crashes, hangs, or memory leaks.</span></span>

<span data-ttu-id="66a63-123">Následující dokumenty Hello se vám pomůžou pochopit a zmírnění tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="66a63-123">hello following documents will help you understand and mitigate this issue:</span></span>
* [<span data-ttu-id="66a63-124">Oprava pomalé počáteční zatížení pro služby IIS</span><span class="sxs-lookup"><span data-stu-id="66a63-124">Fixing slow initial load for IIS</span></span>](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [<span data-ttu-id="66a63-125">Aplikace webové služby IIS 7.5 po recyklaci fondu aplikací je velmi pomalé první požadavek.</span><span class="sxs-lookup"><span data-stu-id="66a63-125">IIS 7.5 web application first request after app-pool recycle very slow</span></span>](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

<span data-ttu-id="66a63-126">Pokud chcete toochange hello výchozí chování služby IIS, budete potřebovat toouse spuštění úlohy, protože pokud použijete ručně instance webových rolí toohello změn, změny hello budou nakonec ztraceny.</span><span class="sxs-lookup"><span data-stu-id="66a63-126">If you want toochange hello default behavior of IIS, you will need toouse startup tasks, because if you manually apply changes toohello Web Role instances, hello changes will eventually be lost.</span></span>

<span data-ttu-id="66a63-127">Další informace najdete v tématu [jak tooconfigure a spusťte spuštění úlohy pro cloudové služby](cloud-services-startup-tasks.md).</span><span class="sxs-lookup"><span data-stu-id="66a63-127">For more information, see [How tooconfigure and run startup tasks for a cloud service](cloud-services-startup-tasks.md).</span></span>
