---
title: aaaHow funguje Azure App Service
description: "Seznámení s fungováním služby App Service"
keywords: "app service, azure app service, škálování, škálovatelné, plán služby App Service, náklady služby App Service"
services: app-service
documentationcenter: 
author: yochay
manager: erikre
editor: 
ms.assetid: ae74fc32-969e-4580-8d61-02c922f1f184
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/23/2017
ms.author: yochayk
ms.openlocfilehash: b20733ec8844773d063e2b6918605c4a48db1f5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-app-service-works"></a><span data-ttu-id="c3bd3-104">Jak funguje App Service</span><span class="sxs-lookup"><span data-stu-id="c3bd3-104">How App Service works</span></span>
<span data-ttu-id="c3bd3-105">Aplikační služba Azure je Cloudová služba, která je hello navrženou toosolve praktické se problémy, které technici čelí dnešní.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-105">Azure App Service is a cloud service that's designed toosolve hello practical problems that engineers face today.</span></span>
<span data-ttu-id="c3bd3-106">Služby App Service se zaměřuje na zajištění vysoké produktivity bez kompromisů v hello potřebovat toodeliver aplikace v cloudovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-106">App Service focuses on providing superior developer productivity without compromising on hello need toodeliver applications at cloud scale.</span></span> 

<span data-ttu-id="c3bd3-107">Služby App Service také nabízí hello funkce a rozhraní, které jsou potřebné pro vytvoření enterprise-obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-107">App Service also provides hello features and frameworks that are necessary for creating enterprise line-of-business applications.</span></span> <span data-ttu-id="c3bd3-108">App Service umožňuje vyvíjet aplikace v nejoblíbenějších vývojových jazyků, včetně Java, PHP, Node.js, Python a jazyků, hello rozhraní Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-108">App Service lets you develop apps in most popular development languages, including Java, PHP, Node.js, Python, and hello Microsoft .NET languages.</span></span> <span data-ttu-id="c3bd3-109">App Service umožňuje:</span><span class="sxs-lookup"><span data-stu-id="c3bd3-109">With App Service, you can:</span></span>

* <span data-ttu-id="c3bd3-110">Vytvářet vysoce škálovatelné webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c3bd3-110">Build highly scalable web apps.</span></span>
* <span data-ttu-id="c3bd3-111">Rychle vytvářet back-endy mobilních aplikací pomocí sady snadno použitelných mobilních funkcí, jako jsou datové back-endy, ověřování uživatelů nebo nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="c3bd3-111">Quickly build Mobile Apps back ends with a set of easy-to-use mobile capabilities such as data back ends, user authentication, and push notifications.</span></span>
* <span data-ttu-id="c3bd3-112">Implementovat, nasazovat a publikovat rozhraní API pomocí funkce API Apps</span><span class="sxs-lookup"><span data-stu-id="c3bd3-112">Implement, deploy, and publish APIs with API Apps.</span></span>
* <span data-ttu-id="c3bd3-113">Spojovat obchodní aplikace do kombinovaných workflow a transformovat data pomocí Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-113">Tie business applications together into workflows and transform data with Logic Apps.</span></span>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

<span data-ttu-id="c3bd3-114">Všechny typy aplikací spoléhají na hello škálovatelná a flexibilní webové aplikace platformy, která umožňuje vývojářům toohave optimalizované kompletní životní prostředí z údržby tooapp návrhu aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-114">All app types rely on hello scalable and flexible Web Apps platform, which enables developers toohave an optimized full lifecycle experience from app design tooapp maintenance.</span></span> <span data-ttu-id="c3bd3-115">Možnosti životního cyklu Hello povolit hello následující:</span><span class="sxs-lookup"><span data-stu-id="c3bd3-115">hello lifecycle capabilities enable hello following:</span></span>

* <span data-ttu-id="c3bd3-116">**Rychlé vytváření aplikací**.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-116">**Quick app creation**.</span></span> <span data-ttu-id="c3bd3-117">Začátek od nuly nebo vyberte balíček operačního systému pro podporu (OSS) z hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-117">Start from scratch or pick an operational support system (OSS) package from hello Azure Marketplace.</span></span>
* <span data-ttu-id="c3bd3-118">**Průběžné nasazování**.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-118">**Continuous deployment**.</span></span> <span data-ttu-id="c3bd3-119">Automaticky nasadíte nový kód z oblíbených řešení pro řízení zdrojů, jako je například TFS, GitHub nebo BitBucket, a můžete synchronizovat obsah ze služeb online úložiště, jako je například OneDrive nebo DropBox.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-119">Automatically deploy new code from popular source control solutions such as TFS, GitHub, and Bitbucket, and sync content from online storage services such as OneDrive and Dropbox.</span></span>
* <span data-ttu-id="c3bd3-120">**Testování v produkčním prostředí**.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-120">**Test in production**.</span></span> <span data-ttu-id="c3bd3-121">Plynule vytvořit předprovozní prostředí a spravovat hello objem provozu, který se bude toothem.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-121">Smoothly create pre-production environments and manage hello amount of traffic that's going toothem.</span></span> <span data-ttu-id="c3bd3-122">Ladění v cloudu hello v případě potřeby, vrácení zpět, když se objeví chyby.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-122">Debug in hello cloud when needed, and roll back when issues are found.</span></span>
* <span data-ttu-id="c3bd3-123">**Spuštění asynchronních a dávkových úloh**.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-123">**Running asynchronous tasks and batch jobs**.</span></span> <span data-ttu-id="c3bd3-124">Kód můžete spouštět v procesech na pozadí nebo aktivovat na základě událostí (například doručení zpráv do fronty Azure Storage) a plánovaných časů (CRON).</span><span class="sxs-lookup"><span data-stu-id="c3bd3-124">Run code in a background process or activate your code based on events (such as messages landing in an Azure Storage queue) and scheduled times (CRON).</span></span>
* <span data-ttu-id="c3bd3-125">**Škálování aplikace hello**.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-125">**Scaling hello app**.</span></span> <span data-ttu-id="c3bd3-126">Použijte jeden z mnoha možností tooautomatically škálování služby, vodorovně a svisle podle provozu a využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-126">Use one of many options tooautomatically scale your service horizontally and vertically based on traffic and resource utilization.</span></span> <span data-ttu-id="c3bd3-127">Konfigurace privátních prostředí, které jsou vyhrazené tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-127">Configure private environments that are dedicated tooyour apps.</span></span>   
* <span data-ttu-id="c3bd3-128">**Údržba aplikace hello**.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-128">**Maintaining hello app**.</span></span> <span data-ttu-id="c3bd3-129">Použijte řadu hello ladění a diagnostiky funkce toostay před problémy a tooefficiently řešení buď v reálném čase (pomocí funkcí, jako je samoopravení nebo živé ladění) nebo po hello fakt prostřednictvím analýzy protokolů a paměti výpisy paměti.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-129">Use many of hello debugging and diagnostics features toostay ahead of problems and tooefficiently resolve them either in real time (with features such as auto-healing and live debugging) or after hello fact by analyzing logs and memory dumps.</span></span>

<span data-ttu-id="c3bd3-130">Možnosti služby App Service jako celek, povolte vývojáři toofocus na kód a rychle dosáhnout stabilního a vysoce škálovatelné provozního stavu.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-130">As a whole, App Service capabilities enable developers toofocus on their code and quickly reach a stable and highly scalable production state.</span></span> <span data-ttu-id="c3bd3-131">Hello aplikace API a funkcí Logic Apps vývojáři mohou vytvářet reálného podnikové aplikace, které přemostění překážek mezi obchodními řešeními a místní toocloud integraci.</span><span class="sxs-lookup"><span data-stu-id="c3bd3-131">With hello API Apps and Logic Apps features, developers can build real-world enterprise applications that bridge barriers between business solutions and on-premises toocloud integration.</span></span> 

## <a name="videos"></a><span data-ttu-id="c3bd3-132">Videa</span><span class="sxs-lookup"><span data-stu-id="c3bd3-132">Videos</span></span>
* [<span data-ttu-id="c3bd3-133">Architektura služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c3bd3-133">Azure App Service Architecture</span></span>](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)

## <a name="next-steps"></a><span data-ttu-id="c3bd3-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c3bd3-134">Next steps</span></span>

<span data-ttu-id="c3bd3-135">Další informace o App Service v jednom z hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="c3bd3-135">Learn more about App Service in one of hello following topics:</span></span>

* [<span data-ttu-id="c3bd3-136">Co je Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="c3bd3-136">What is Azure App Service?</span></span>](app-service-value-prop-what-is.md)
  * [<span data-ttu-id="c3bd3-137">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="c3bd3-137">Web App</span></span>](../app-service-web/app-service-web-overview.md)
  * [<span data-ttu-id="c3bd3-138">Mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="c3bd3-138">Mobile App</span></span>](../app-service-mobile/app-service-mobile-value-prop.md)
  * [<span data-ttu-id="c3bd3-139">Aplikace API</span><span class="sxs-lookup"><span data-stu-id="c3bd3-139">API App</span></span>](../app-service-api/app-service-api-apps-why-best-platform.md)
* [<span data-ttu-id="c3bd3-140">Architektura služby Azure App Service (prezentace)</span><span class="sxs-lookup"><span data-stu-id="c3bd3-140">Azure App Service Architecture (presentation)</span></span>](http://www.slideshare.net/maartenba/windows-azure-web-sites-things-they-dont-teach-kids-in-school-comunity-day-2013)
* [<span data-ttu-id="c3bd3-141">Porovnání služby Azure App Service, služby Cloud Services a služby Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="c3bd3-141">Azure App Service, Cloud Services, and Virtual Machines comparison</span></span>](../app-service-web/choose-web-site-cloud-service-vm.md)
* [<span data-ttu-id="c3bd3-142">Pochopení plánů služby App Service</span><span class="sxs-lookup"><span data-stu-id="c3bd3-142">Understanding App Service Plans</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
* [<span data-ttu-id="c3bd3-143">Úvod tooApp Service Environment</span><span class="sxs-lookup"><span data-stu-id="c3bd3-143">Introduction tooApp Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
  * [<span data-ttu-id="c3bd3-144">Cvičení: Vytvoření prostředí služby App Service</span><span class="sxs-lookup"><span data-stu-id="c3bd3-144">Exercise: Create an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="c3bd3-145">Podpora vývojových balíků služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c3bd3-145">Azure App Service Development Stacks Support</span></span>](https://azure.microsoft.com/blog/windows-azure-websites-development-stacks-support/)



