---
title: "aaaI používat mobilní služby, jak mi pomůže App Service?"
description: "Zjistěte, jaké výhody přináší služba App Service tooyour existující projekty Mobile Services."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 26b68a11-8352-4f78-acd2-e4e0ec177781
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 315cc6eedcdca6c3f9f9bb9fd5ec7baf655b7e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="e153e-103"><a name="getting-started"></a>Používám Mobile Services, jak mi pomůže App Service?</span><span class="sxs-lookup"><span data-stu-id="e153e-103"><a name="getting-started"> </a>I use Mobile Services, how does App Service help?</span></span>
## <a name="overview"></a><span data-ttu-id="e153e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e153e-104">Overview</span></span>
<span data-ttu-id="e153e-105">Vaše stávající služba Mobile Service je v bezpečí a bude i nadále podporována.</span><span class="sxs-lookup"><span data-stu-id="e153e-105">Your existing Mobile Service is safe and will remain supported.</span></span> <span data-ttu-id="e153e-106">Ale existují počet výhody hello *Azure App Service* poskytuje platformu pro své mobilní aplikace, které nejsou dostupné dnes v Mobile Services:</span><span class="sxs-lookup"><span data-stu-id="e153e-106">However there are number of advantages hello *Azure App Service* platform provides for your mobile app that are not available today with Mobile Services:</span></span>

* <span data-ttu-id="e153e-107">Jednodušší, snazší a nákladově efektivní nabídka pro aplikace, které obsahují jak webové, tak mobilní klienty</span><span class="sxs-lookup"><span data-stu-id="e153e-107">Simpler, easier and more cost effective offering for apps that include both web and mobile clients</span></span>
* <span data-ttu-id="e153e-108">Nové hostitelské funkce včetně webových úloh, vlastní záznamy CNAME, lepší monitorování</span><span class="sxs-lookup"><span data-stu-id="e153e-108">New host features including Web Jobs, custom CNames, better monitoring</span></span>
* <span data-ttu-id="e153e-109">Předpřipravená integrace s nástrojem Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="e153e-109">Turnkey integration with Traffic Manager</span></span>
* <span data-ttu-id="e153e-110">Připojení tooyour místních prostředků a připojení VPN pomocí virtuální sítě v přidání tooHybrid připojení</span><span class="sxs-lookup"><span data-stu-id="e153e-110">Connectivity tooyour on-premises resources and VPNs using VNet in addition tooHybrid Connections</span></span>
* <span data-ttu-id="e153e-111">Monitorování, výstrahy a řešení potíží pro aplikace pomocí nástrojů NewRelic a AppInsights</span><span class="sxs-lookup"><span data-stu-id="e153e-111">Monitoring, alerting and  troubleshooting for your app using NewRelic or AppInsights</span></span>
* <span data-ttu-id="e153e-112">Širší spektrum hello základní výpočetní prostředky a ceny</span><span class="sxs-lookup"><span data-stu-id="e153e-112">Richer spectrum of hello underlying compute resources and pricing</span></span>
* <span data-ttu-id="e153e-113">Integrované automatické škálování, vyrovnávání zatížení a monitorování výkonu</span><span class="sxs-lookup"><span data-stu-id="e153e-113">Built-in auto scale, load balancing, and performance monitoring.</span></span>
* <span data-ttu-id="e153e-114">Integrované funkce pro fázování, zálohování, vrácení zpět a testování za provozu</span><span class="sxs-lookup"><span data-stu-id="e153e-114">Built-in staging, backup, roll-back, and testing-in-production capabilities</span></span>

## <a name="new-hosting-features"></a><span data-ttu-id="e153e-115">Nové hostitelské funkce</span><span class="sxs-lookup"><span data-stu-id="e153e-115">New hosting features</span></span>
<span data-ttu-id="e153e-116">V *Azure App Service* hello *mobilní aplikace* hello back-end kód běží ve stejném kontejneru jako webové aplikace a aplikace API.</span><span class="sxs-lookup"><span data-stu-id="e153e-116">In *Azure App Service* hello *Mobile App* backend code runs in hello same container as Web App and API App.</span></span> <span data-ttu-id="e153e-117">Takto můžete využít všech funkcí hello v tomto kontejneru, včetně některých funkcí, které nejsou aktuálně dostupné v Mobile Services:</span><span class="sxs-lookup"><span data-stu-id="e153e-117">As such you can take advantage of all hello features in this container, including some of those that are not currently present in Mobile Services:</span></span>

* <span data-ttu-id="e153e-118">Přidání neustále běžící back-end logiky přes webové úlohy</span><span class="sxs-lookup"><span data-stu-id="e153e-118">Add continuously running backend logic via Web Jobs</span></span>
* <span data-ttu-id="e153e-119">Zajištění, že back-end kód bude vždy spuštěn</span><span class="sxs-lookup"><span data-stu-id="e153e-119">Ensure your backend code is always running</span></span>
* <span data-ttu-id="e153e-120">Použití vlastních záznamů CNAME tooprovide popisné a stabilní názvy tooyour koncových bodů mobilního back-endu</span><span class="sxs-lookup"><span data-stu-id="e153e-120">Use custom CNames tooprovide friendly and stable names tooyour mobile backend endpoints</span></span>
* <span data-ttu-id="e153e-121">Geografické škálování aplikace pomocí nástroje Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="e153e-121">Geo-scale your app with Traffic Manager</span></span>
* <span data-ttu-id="e153e-122">Zahrnutí jakýchkoli knihoven nebo balíčků, které chcete použít</span><span class="sxs-lookup"><span data-stu-id="e153e-122">Include any libraries and packages you want.</span></span>
* <span data-ttu-id="e153e-123">(Pro .NET) Využití jakékoli funkce technologie ASP.NET, včetně MVC</span><span class="sxs-lookup"><span data-stu-id="e153e-123">(For .NET) Leverage any feature of ASP.NET, including MVC</span></span>
* <span data-ttu-id="e153e-124">(Pro platformu Node.js) Využívejte všechny čistý JavaScript library hello uzlu ekosystém, včetně běžné knihovny MVC.</span><span class="sxs-lookup"><span data-stu-id="e153e-124">(For Node.js) Leverage any pure JavaScript library of hello Node ecosystem, including common MVC libraries.</span></span>

## <a name="access-on-premises-data-using-vnet"></a><span data-ttu-id="e153e-125">Přístup k lokálním datům přes virtuální síť</span><span class="sxs-lookup"><span data-stu-id="e153e-125">Access on-premises data using VNet</span></span>
<span data-ttu-id="e153e-126">S Mobile Services Dnes už můžete tooaccess hybridní připojení místních prostředků.</span><span class="sxs-lookup"><span data-stu-id="e153e-126">With Mobile Services today you can already use Hybrid Connections tooaccess on-premises resources.</span></span> <span data-ttu-id="e153e-127">Existují však situace, kdy je lepší použít řešení se sítí VPN.</span><span class="sxs-lookup"><span data-stu-id="e153e-127">However there are situations where a VPN solution is preferred.</span></span> <span data-ttu-id="e153e-128">S *Azure App Services* můžete pro back-end mobilní aplikace použít Azure VNet.</span><span class="sxs-lookup"><span data-stu-id="e153e-128">With *Azure App Service* you can use Azure VNet for your Mobile App backend code.</span></span>

## <a name="use-your-favorite-backend-language"></a><span data-ttu-id="e153e-129">Využití oblíbeného jazyka pro back-end</span><span class="sxs-lookup"><span data-stu-id="e153e-129">Use your favorite backend language</span></span>
<span data-ttu-id="e153e-130">*Aplikační služba Azure* nabízí širší a bohatší podporu platforem ASP.NET a Node.js, včetně přístup k nejnovějším modulům runtime toohello.</span><span class="sxs-lookup"><span data-stu-id="e153e-130">*Azure App Service* offers broader and richer support for ASP.NET and Node.js platforms, including access toohello latest runtimes.</span></span>

## <a name="set-up-automatic-scale"></a><span data-ttu-id="e153e-131">Nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="e153e-131">Set up automatic scale</span></span>
<span data-ttu-id="e153e-132">S Mobile Services běžely všechny instance back-end kódu na malých virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="e153e-132">With Mobile Services, all instances of your backend code were running on Small VMs.</span></span> <span data-ttu-id="e153e-133">*Aplikační služba Azure* vám umožní tooselect hello velikost virtuálního počítače z mnohem širší možnosti.</span><span class="sxs-lookup"><span data-stu-id="e153e-133">*Azure App Service* enables you tooselect hello size of the VMs from a much richer set of options.</span></span> <span data-ttu-id="e153e-134">Můžete také rychle škálovat nahoru nebo na toohandle všechny příchozí zákazníka zatížení, podle různých metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="e153e-134">You can also  quickly scale up or out toohandle any incoming customer load, based on various performance metrics.</span></span>

## <a name="be-in-hello-know"></a><span data-ttu-id="e153e-135">Být v hello "vědět"</span><span class="sxs-lookup"><span data-stu-id="e153e-135">Be in hello “know”</span></span>
<span data-ttu-id="e153e-136">Tooissues reagovat v reálném čase díky monitorování a výstrahám tooautomatically oznámit vy a váš tým.</span><span class="sxs-lookup"><span data-stu-id="e153e-136">React tooissues in real-time with monitoring and alerts tooautomatically notify you and your team.</span></span> <span data-ttu-id="e153e-137">Integrate pokročilé analýzy aplikací a monitorování funkcí z nástrojů New Relic a AppInsights tooget ještě podrobnější informace o výkonu mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e153e-137">Integrate advanced app analytics and monitoring functionality from New Relic and AppInsights tooget even richer insight into how your Mobile app is performing.</span></span> <span data-ttu-id="e153e-138">S *Azure App Service* nyní můžete nastavit upozornění na základě různých metrik výkonu, buď programově nebo prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e153e-138">With *Azure App Service* you can now setup alerts based on variety of performance metrics, either programatically and via hello Azure Portal.</span></span>

## <a name="keep-your-assets-safe"></a><span data-ttu-id="e153e-139">Zabezpečení prostředků</span><span class="sxs-lookup"><span data-stu-id="e153e-139">Keep your assets safe</span></span>
<span data-ttu-id="e153e-140">Back-end a databáze je možné automaticky zálohovat.</span><span class="sxs-lookup"><span data-stu-id="e153e-140">Automatically back up your backend and database.</span></span> <span data-ttu-id="e153e-141">Kód a data jsou v bezpečí po havárii a snadno obnovený, což vám toorun vaší firmě s jistotou.</span><span class="sxs-lookup"><span data-stu-id="e153e-141">Your code and data is secure from disaster and easily restored, allowing you toorun your business with confidence.</span></span>

## <a name="ready-stage-go"></a><span data-ttu-id="e153e-142">Připravit, naplánovat, start!</span><span class="sxs-lookup"><span data-stu-id="e153e-142">Ready, Stage, Go!</span></span>
<span data-ttu-id="e153e-143">S *Azure App Service* je nyní možné pro své mobilní aplikace vytvořit několik privátních testovacích a přípravných prostředí.</span><span class="sxs-lookup"><span data-stu-id="e153e-143">With *Azure App Service* you can now create multiple private testing and staging environments for your mobile apps.</span></span> <span data-ttu-id="e153e-144">Pomocí těchto tooperform před nasazením.</span><span class="sxs-lookup"><span data-stu-id="e153e-144">Use them tooperform testing before you deploy.</span></span> <span data-ttu-id="e153e-145">Swap tooproduction bez výpadků.</span><span class="sxs-lookup"><span data-stu-id="e153e-145">Swap tooproduction with no downtime.</span></span> <span data-ttu-id="e153e-146">Webové aplikace se načítají předem, zajištění hello mají zákazníci maximální pohodlí.</span><span class="sxs-lookup"><span data-stu-id="e153e-146">Web apps are pre-loaded, ensuring hello best customer experience.</span></span>

<span data-ttu-id="e153e-147">Výhody *App Service* pro existující Mobile Service můžete začít využívat tak, že si projdete tento [kurz](app-service-mobile-migrating-from-mobile-services.md).</span><span class="sxs-lookup"><span data-stu-id="e153e-147">You can get start taking advantage of *App Service* for your existing Mobile Service by following this [tutorial](app-service-mobile-migrating-from-mobile-services.md).</span></span>
