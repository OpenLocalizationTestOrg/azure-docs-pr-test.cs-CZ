---
title: "Aplikace a problémy s dostupností služby pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "V tomto článku jsou uvedené nejčastější dotazy o aplikaci a dostupnost služeb pro Microsoft Azure Cloud Services."
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
ms.openlocfilehash: a893a6d009dd018ad440964419e0a5a1963084b4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a><span data-ttu-id="7fa08-103">Aplikace a problémy s dostupností služby pro Azure Cloud Services: Časté otázky (FAQ)</span><span class="sxs-lookup"><span data-stu-id="7fa08-103">Application and service availability issues for Azure Cloud Services: Frequently asked questions (FAQs)</span></span>

<span data-ttu-id="7fa08-104">Tento článek obsahuje nejčastější dotazy týkající se aplikace a problémy s dostupností služby pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services).</span><span class="sxs-lookup"><span data-stu-id="7fa08-104">This article includes frequently asked questions about application and service availability issues for [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services).</span></span> <span data-ttu-id="7fa08-105">Můžete také obrátit [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.</span><span class="sxs-lookup"><span data-stu-id="7fa08-105">You can also consult the [Cloud Services VM Size page](cloud-services-sizes-specs.md) for size information.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a><span data-ttu-id="7fa08-106">Moje role tu recykluje.</span><span class="sxs-lookup"><span data-stu-id="7fa08-106">My role got recycled.</span></span> <span data-ttu-id="7fa08-107">Se nějakou aktualizaci nasazuje pro moje Cloudová služba?</span><span class="sxs-lookup"><span data-stu-id="7fa08-107">Was there any update rolled out for my cloud service?</span></span>
<span data-ttu-id="7fa08-108">Přibližně jednou za měsíc, společnost Microsoft vydá nové verze hostovaného operačního systému pro virtuální počítače Windows Azure PaaS.</span><span class="sxs-lookup"><span data-stu-id="7fa08-108">Roughly once a month, Microsoft releases a new Guest OS version for Windows Azure PaaS VMs.</span></span><span data-ttu-id="7fa08-109"> Hostovaného operačního systému je jenom jedna taková aktualizace v kanálu.</span><span class="sxs-lookup"><span data-stu-id="7fa08-109"> The Guest OS is only one such update in the pipeline.</span></span> <span data-ttu-id="7fa08-110">Verze může mít vliv mnoho dalších faktorů.</span><span class="sxs-lookup"><span data-stu-id="7fa08-110">A release can be affected by many other factors.</span></span> <span data-ttu-id="7fa08-111">Kromě toho Azure je spuštěná na stovky tisíc počítačů.</span><span class="sxs-lookup"><span data-stu-id="7fa08-111">In addition, Azure runs on hundreds of thousands of machines.</span></span> <span data-ttu-id="7fa08-112">Proto není možné předpovědět přesné datum a čas, kdy se restartuje vaše role.</span><span class="sxs-lookup"><span data-stu-id="7fa08-112">Therefore, it's impossible to predict the exact date and time when your roles will reboot.</span></span> <span data-ttu-id="7fa08-113">Aktualizujeme s nejnovější informace, které jsme hostovaného operačního systému aktualizujte informačního kanálu RSS, ale měli byste zvážit, hlášena čas přibližnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7fa08-113">We update the Guest OS Update RSS Feed with the latest information that we have, but you should consider that reported time to be an approximate value.</span></span> <span data-ttu-id="7fa08-114">Jsme víte, že se jedná o problém pro zákazníky a práce na plán limit nebo přesněji čas restartování.</span><span class="sxs-lookup"><span data-stu-id="7fa08-114">We are aware that this is problematic for customers and are working on a plan to limit or precisely time reboots.</span></span>

<span data-ttu-id="7fa08-115">Kompletní informace o nejnovějších aktualizacích hostovaného operačního systému najdete v tématu [verze hostovaného operačního systému Azure a kompatibilních sad SDK](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="7fa08-115">For complete details about recent Guest OS updates, see [Azure Guest OS releases and SDK compatibility matrix](cloud-services-guestos-update-matrix.md).</span></span>

<span data-ttu-id="7fa08-116">Užitečné informace o restartování a odkazy na technické informace o aktualizace hosta a hostitelským operačním systémem, najdete v příspěvku blogu MSDN [Role Instance restartuje kvůli upgrady operačního systému](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).</span><span class="sxs-lookup"><span data-stu-id="7fa08-116">For helpful information on restarts and pointers to technical details of Guest and Host OS updates, see the MSDN blog post [Role Instance Restarts Due to OS Upgrades](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).</span></span>

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a><span data-ttu-id="7fa08-117">Proč první požadavek na můj cloudové služby po služby bylo nečinné po určitý čas trvat déle než obvykle?</span><span class="sxs-lookup"><span data-stu-id="7fa08-117">Why does the first request to my cloud service after the service has been idle for some time take longer than usual?</span></span>
<span data-ttu-id="7fa08-118">Když webový Server obdrží požadavek na první, nejprve znovu zkompiluje kód a potom zpracuje žádost.</span><span class="sxs-lookup"><span data-stu-id="7fa08-118">When the Web Server receives the first request, it first recompiles the code and then processes the request.</span></span> <span data-ttu-id="7fa08-119">To je důvod, proč první žádost trvá déle než jiné.</span><span class="sxs-lookup"><span data-stu-id="7fa08-119">That's why the first request takes longer than the others.</span></span> <span data-ttu-id="7fa08-120">Ve výchozím nastavení získá fondu aplikací v případech nečinnosti uživatele vypnout.</span><span class="sxs-lookup"><span data-stu-id="7fa08-120">By default, the app pool gets shut down in cases of user inactivity.</span></span> <span data-ttu-id="7fa08-121">Fond aplikací bude taky recyklaci ve výchozím nastavení každých 1,740 minut (29 hodiny).</span><span class="sxs-lookup"><span data-stu-id="7fa08-121">The app pool will also recycle by default every 1,740 minutes (29 hours).</span></span>

<span data-ttu-id="7fa08-122">Aplikace Internetové informační služby (IIS), který fondy může být pravidelně recykluje, aby se zabránilo nestabilním stavy, které můžou vést k aplikaci spadne, zablokuje, nebo nevracení paměti.</span><span class="sxs-lookup"><span data-stu-id="7fa08-122">Internet Information Services (IIS) application pools can be periodically recycled to avoid unstable states that can lead to application crashes, hangs, or memory leaks.</span></span>

<span data-ttu-id="7fa08-123">V následujících dokumentech vám pomůže pochopit a zmírnění tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="7fa08-123">The following documents will help you understand and mitigate this issue:</span></span>
* [<span data-ttu-id="7fa08-124">Oprava pomalé počáteční zatížení pro služby IIS</span><span class="sxs-lookup"><span data-stu-id="7fa08-124">Fixing slow initial load for IIS</span></span>](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [<span data-ttu-id="7fa08-125">Aplikace webové služby IIS 7.5 po recyklaci fondu aplikací je velmi pomalé první požadavek.</span><span class="sxs-lookup"><span data-stu-id="7fa08-125">IIS 7.5 web application first request after app-pool recycle very slow</span></span>](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

<span data-ttu-id="7fa08-126">Pokud chcete změnit výchozí chování služby IIS, musíte pro použití při spuštění úloh, protože pokud ručně použít změny instance webových rolí, změny se nakonec ztratí.</span><span class="sxs-lookup"><span data-stu-id="7fa08-126">If you want to change the default behavior of IIS, you will need to use startup tasks, because if you manually apply changes to the Web Role instances, the changes will eventually be lost.</span></span>

<span data-ttu-id="7fa08-127">Další informace najdete v tématu [postupy pro konfiguraci a spuštění úlohy spuštění pro cloudové služby](cloud-services-startup-tasks.md).</span><span class="sxs-lookup"><span data-stu-id="7fa08-127">For more information, see [How to configure and run startup tasks for a cloud service](cloud-services-startup-tasks.md).</span></span>
