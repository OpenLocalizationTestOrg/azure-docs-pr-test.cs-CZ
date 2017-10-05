---
title: Jak funguje Azure App Service
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
ms.openlocfilehash: 2d830963d3d2adba71a6ca99f79eac0fc8cbfb12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-app-service-works"></a><span data-ttu-id="2ebab-104">Jak funguje App Service</span><span class="sxs-lookup"><span data-stu-id="2ebab-104">How App Service works</span></span>
<span data-ttu-id="2ebab-105">Azure App Service je cloudová služba navržená k řešení praktických problémů, kterým čelí dnešní vývojáři.</span><span class="sxs-lookup"><span data-stu-id="2ebab-105">Azure App Service is a cloud service that's designed to solve the practical problems that engineers face today.</span></span>
<span data-ttu-id="2ebab-106">App Service se zaměřuje na zajištění vysoké produktivity při vývoji bez kompromisů při zajišťování aplikací v cloudovém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="2ebab-106">App Service focuses on providing superior developer productivity without compromising on the need to deliver applications at cloud scale.</span></span> 

<span data-ttu-id="2ebab-107">App Service navíc poskytuje funkce a rozhraní, které jsou nezbytné pro vytváření podnikových obchodních aplikací.</span><span class="sxs-lookup"><span data-stu-id="2ebab-107">App Service also provides the features and frameworks that are necessary for creating enterprise line-of-business applications.</span></span> <span data-ttu-id="2ebab-108">App Service umožňuje vyvíjet aplikace v nejoblíbenějších vývojových jazycích, včetně Javy, PHP, Node.js, Pythonu a jazyků Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="2ebab-108">App Service lets you develop apps in most popular development languages, including Java, PHP, Node.js, Python, and the Microsoft .NET languages.</span></span> <span data-ttu-id="2ebab-109">App Service umožňuje:</span><span class="sxs-lookup"><span data-stu-id="2ebab-109">With App Service, you can:</span></span>

* <span data-ttu-id="2ebab-110">Vytvářet vysoce škálovatelné webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2ebab-110">Build highly scalable web apps.</span></span>
* <span data-ttu-id="2ebab-111">Rychle vytvářet back-endy mobilních aplikací pomocí sady snadno použitelných mobilních funkcí, jako jsou datové back-endy, ověřování uživatelů nebo nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="2ebab-111">Quickly build Mobile Apps back ends with a set of easy-to-use mobile capabilities such as data back ends, user authentication, and push notifications.</span></span>
* <span data-ttu-id="2ebab-112">Implementovat, nasazovat a publikovat rozhraní API pomocí funkce API Apps</span><span class="sxs-lookup"><span data-stu-id="2ebab-112">Implement, deploy, and publish APIs with API Apps.</span></span>
* <span data-ttu-id="2ebab-113">Spojovat obchodní aplikace do kombinovaných workflow a transformovat data pomocí Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="2ebab-113">Tie business applications together into workflows and transform data with Logic Apps.</span></span>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

<span data-ttu-id="2ebab-114">Všechny typy aplikací se opírají o škálovatelnou, flexibilní platformu webových aplikací, která vývojářům umožňuje vytvářet optimalizované kompletní životní cykly: od návrhu aplikace až po její údržbu.</span><span class="sxs-lookup"><span data-stu-id="2ebab-114">All app types rely on the scalable and flexible Web Apps platform, which enables developers to have an optimized full lifecycle experience from app design to app maintenance.</span></span> <span data-ttu-id="2ebab-115">Funkce životních cyklů umožňují:</span><span class="sxs-lookup"><span data-stu-id="2ebab-115">The lifecycle capabilities enable the following:</span></span>

* <span data-ttu-id="2ebab-116">**Rychlé vytváření aplikací**.</span><span class="sxs-lookup"><span data-stu-id="2ebab-116">**Quick app creation**.</span></span> <span data-ttu-id="2ebab-117">Můžete začít od nuly nebo využít vhodný balíček OSS z webu Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="2ebab-117">Start from scratch or pick an operational support system (OSS) package from the Azure Marketplace.</span></span>
* <span data-ttu-id="2ebab-118">**Průběžné nasazování**.</span><span class="sxs-lookup"><span data-stu-id="2ebab-118">**Continuous deployment**.</span></span> <span data-ttu-id="2ebab-119">Automaticky nasadíte nový kód z oblíbených řešení pro řízení zdrojů, jako je například TFS, GitHub nebo BitBucket, a můžete synchronizovat obsah ze služeb online úložiště, jako je například OneDrive nebo DropBox.</span><span class="sxs-lookup"><span data-stu-id="2ebab-119">Automatically deploy new code from popular source control solutions such as TFS, GitHub, and Bitbucket, and sync content from online storage services such as OneDrive and Dropbox.</span></span>
* <span data-ttu-id="2ebab-120">**Testování v produkčním prostředí**.</span><span class="sxs-lookup"><span data-stu-id="2ebab-120">**Test in production**.</span></span> <span data-ttu-id="2ebab-121">Hladce vytvoříte předprodukční prostředí a zajistíte správu provozu, který v něm probíhá.</span><span class="sxs-lookup"><span data-stu-id="2ebab-121">Smoothly create pre-production environments and manage the amount of traffic that's going to them.</span></span> <span data-ttu-id="2ebab-122">Pokud je potřeba, odladíte nasazení v cloudu a jestliže se objeví chyby, můžete ho vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="2ebab-122">Debug in the cloud when needed, and roll back when issues are found.</span></span>
* <span data-ttu-id="2ebab-123">**Spuštění asynchronních a dávkových úloh**.</span><span class="sxs-lookup"><span data-stu-id="2ebab-123">**Running asynchronous tasks and batch jobs**.</span></span> <span data-ttu-id="2ebab-124">Kód můžete spouštět v procesech na pozadí nebo aktivovat na základě událostí (například doručení zpráv do fronty Azure Storage) a plánovaných časů (CRON).</span><span class="sxs-lookup"><span data-stu-id="2ebab-124">Run code in a background process or activate your code based on events (such as messages landing in an Azure Storage queue) and scheduled times (CRON).</span></span>
* <span data-ttu-id="2ebab-125">**Škálování aplikací**.</span><span class="sxs-lookup"><span data-stu-id="2ebab-125">**Scaling the app**.</span></span> <span data-ttu-id="2ebab-126">K dispozici je mnoho možností automatického škálování služby – horizontálního i vertikálního – podle provozu a využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="2ebab-126">Use one of many options to automatically scale your service horizontally and vertically based on traffic and resource utilization.</span></span> <span data-ttu-id="2ebab-127">Můžete konfigurovat privátní prostředí vyhrazená pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ebab-127">Configure private environments that are dedicated to your apps.</span></span>   
* <span data-ttu-id="2ebab-128">**Údržba aplikací**.</span><span class="sxs-lookup"><span data-stu-id="2ebab-128">**Maintaining the app**.</span></span> <span data-ttu-id="2ebab-129">V nabídce je mnoho funkcí ladění a diagnostiky umožňujících předcházet problémům a efektivně je řešit, ať už v reálném čase (pomocí funkcí, jako je samoopravení nebo živé ladění), nebo po jejich výskytu prostřednictvím analýzy protokolů a výpisů stavu paměti.</span><span class="sxs-lookup"><span data-stu-id="2ebab-129">Use many of the debugging and diagnostics features to stay ahead of problems and to efficiently resolve them either in real time (with features such as auto-healing and live debugging) or after the fact by analyzing logs and memory dumps.</span></span>

<span data-ttu-id="2ebab-130">V kostce řečeno, funkce App Service umožňují vývojářům soustředit se na kód a rychle dosáhnout stabilního a vysoce škálovatelného provozního stavu.</span><span class="sxs-lookup"><span data-stu-id="2ebab-130">As a whole, App Service capabilities enable developers to focus on their code and quickly reach a stable and highly scalable production state.</span></span> <span data-ttu-id="2ebab-131">Pomocí funkcí API Apps a Logic Apps můžou vývojáři vytvářet odolné podnikové aplikace překračující bariéry mezi různými obchodními řešeními i překážky v integraci místních prostředků s cloudem.</span><span class="sxs-lookup"><span data-stu-id="2ebab-131">With the API Apps and Logic Apps features, developers can build real-world enterprise applications that bridge barriers between business solutions and on-premises to cloud integration.</span></span> 

## <a name="videos"></a><span data-ttu-id="2ebab-132">Videa</span><span class="sxs-lookup"><span data-stu-id="2ebab-132">Videos</span></span>
* [<span data-ttu-id="2ebab-133">Architektura služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2ebab-133">Azure App Service Architecture</span></span>](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)

## <a name="next-steps"></a><span data-ttu-id="2ebab-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ebab-134">Next steps</span></span>

<span data-ttu-id="2ebab-135">Víc se o App Service dozvíte v jednom z následujících témat:</span><span class="sxs-lookup"><span data-stu-id="2ebab-135">Learn more about App Service in one of the following topics:</span></span>

* [<span data-ttu-id="2ebab-136">Co je Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="2ebab-136">What is Azure App Service?</span></span>](app-service-value-prop-what-is.md)
  * [<span data-ttu-id="2ebab-137">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="2ebab-137">Web App</span></span>](../app-service-web/app-service-web-overview.md)
  * [<span data-ttu-id="2ebab-138">Mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="2ebab-138">Mobile App</span></span>](../app-service-mobile/app-service-mobile-value-prop.md)
  * [<span data-ttu-id="2ebab-139">Aplikace API</span><span class="sxs-lookup"><span data-stu-id="2ebab-139">API App</span></span>](../app-service-api/app-service-api-apps-why-best-platform.md)
* [<span data-ttu-id="2ebab-140">Architektura služby Azure App Service (prezentace)</span><span class="sxs-lookup"><span data-stu-id="2ebab-140">Azure App Service Architecture (presentation)</span></span>](http://www.slideshare.net/maartenba/windows-azure-web-sites-things-they-dont-teach-kids-in-school-comunity-day-2013)
* [<span data-ttu-id="2ebab-141">Porovnání služby Azure App Service, služby Cloud Services a služby Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="2ebab-141">Azure App Service, Cloud Services, and Virtual Machines comparison</span></span>](../app-service-web/choose-web-site-cloud-service-vm.md)
* [<span data-ttu-id="2ebab-142">Pochopení plánů služby App Service</span><span class="sxs-lookup"><span data-stu-id="2ebab-142">Understanding App Service Plans</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
* [<span data-ttu-id="2ebab-143">Úvod do prostředí App Service</span><span class="sxs-lookup"><span data-stu-id="2ebab-143">Introduction to App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
  * [<span data-ttu-id="2ebab-144">Cvičení: Vytvoření prostředí služby App Service</span><span class="sxs-lookup"><span data-stu-id="2ebab-144">Exercise: Create an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="2ebab-145">Podpora vývojových balíků služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2ebab-145">Azure App Service Development Stacks Support</span></span>](https://azure.microsoft.com/blog/windows-azure-websites-development-stacks-support/)



