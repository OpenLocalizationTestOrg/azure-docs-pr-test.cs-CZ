---
title: "podrobný přehled plánů služby App Service aaaAzure | Microsoft Docs"
description: "Zjistěte, jak plánů služby App Service pro pracovní Azure App Service a jak budou využívat vaše prostředí pro správu."
keywords: "app service, azure app service, škálování, škálovatelné, plán služby App Service, náklady služby App Service"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: b384790d9e69b234ca69ac591164c48a4b6ed210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a><span data-ttu-id="a9957-104">Podrobný přehled plánů Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a9957-104">Azure App Service plans in-depth overview</span></span>

<span data-ttu-id="a9957-105">Plány aplikační služby představují hello kolekce toohost fyzické prostředky, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9957-105">App Service plans represent hello collection of physical resources used toohost your apps.</span></span>

<span data-ttu-id="a9957-106">Plány služby App Service definují:</span><span class="sxs-lookup"><span data-stu-id="a9957-106">App Service plans define:</span></span>

- <span data-ttu-id="a9957-107">Oblast (západní USA, Východ USA, atd.)</span><span class="sxs-lookup"><span data-stu-id="a9957-107">Region (West US, East US, etc.)</span></span>
- <span data-ttu-id="a9957-108">Počet škálování (jeden, dva, tři instance atd.)</span><span class="sxs-lookup"><span data-stu-id="a9957-108">Scale count (one, two, three instances, etc.)</span></span>
- <span data-ttu-id="a9957-109">Velikost instance (malé, střední, velké)</span><span class="sxs-lookup"><span data-stu-id="a9957-109">Instance size (Small, Medium, Large)</span></span>
- <span data-ttu-id="a9957-110">SKU (Free, Shared, Basic, Standard, Premium)</span><span class="sxs-lookup"><span data-stu-id="a9957-110">SKU (Free, Shared, Basic, Standard, Premium)</span></span>

<span data-ttu-id="a9957-111">Webové aplikace, mobilní aplikace, aplikace API, funkce aplikace (nebo funkce), v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) všechny spuštění v rámci plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="a9957-111">Web Apps, Mobile Apps, API Apps, Function Apps (or Functions), in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) all run in an App Service plan.</span></span>  <span data-ttu-id="a9957-112">Aplikace v hello stejném předplatném, oblasti můžete sdílet plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="a9957-112">Apps in hello same subscription, and region can share an App Service plan.</span></span> 

<span data-ttu-id="a9957-113">Všechny aplikace přiřazené tooan **plán služby App Service** sdílet prostředky hello definovaná tímto.</span><span class="sxs-lookup"><span data-stu-id="a9957-113">All applications assigned tooan **App Service plan** share hello resources defined by it.</span></span> <span data-ttu-id="a9957-114">Tato sdílení úsporu nákladů při hostování více aplikacemi v jednom plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="a9957-114">This sharing saves money when hosting multiple apps in a single App Service plan.</span></span>

<span data-ttu-id="a9957-115">Vaše **plán služby App Service** možné škálovat od **volné** a **sdílené** SKU příliš**základní**, **standardní**, a **Premium** SKU, která poskytuje přístup k prostředkům toomore a funkcí společně hello způsobem.</span><span class="sxs-lookup"><span data-stu-id="a9957-115">Your **App Service plan** can scale from **Free** and **Shared** SKUs too**Basic**, **Standard**, and **Premium** SKUs giving you access toomore resources and features along hello way.</span></span>

<span data-ttu-id="a9957-116">Pokud váš plán služby App Service je nastaven příliš**základní** SKU nebo vyšší, pak můžete řídit hello **velikost** a škálovat počet hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a9957-116">If your App Service plan is set too**Basic** SKU or higher, then you can control hello **size** and scale count of hello VMs.</span></span>

<span data-ttu-id="a9957-117">Například pokud váš plán je nakonfigurované toouse dva "malá" instance ve vrstvě služby na úrovni standard hello, všechny aplikace, které jsou přidruženy tento plán spustit na obou instancí.</span><span class="sxs-lookup"><span data-stu-id="a9957-117">For example, if your plan is configured toouse two "small" instances in hello standard service tier, all apps that are associated with that plan run on both instances.</span></span> <span data-ttu-id="a9957-118">Aplikací mít také funkce úrovně služby na úrovni standard toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="a9957-118">Apps also have access toohello standard service tier features.</span></span> <span data-ttu-id="a9957-119">Instancích plánu, na kterých běží aplikace jsou plně spravovaná a vysoce dostupné.</span><span class="sxs-lookup"><span data-stu-id="a9957-119">Plan instances on which apps are running are fully managed and highly available.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9957-120">Hello **SKU** a **škálování** z hello služby App Service určuje plán hello náklady a není hello počet aplikací, které jsou v ní umístěné.</span><span class="sxs-lookup"><span data-stu-id="a9957-120">hello **SKU** and **Scale** of hello App Service plan determines hello cost and not hello number of apps hosted in it.</span></span>

<span data-ttu-id="a9957-121">Tento článek popisuje hello klíčové vlastnosti, například vrstvy a škále plán služby App Service a jak se do play při správě aplikací.</span><span class="sxs-lookup"><span data-stu-id="a9957-121">This article explores hello key characteristics, such as tier and scale, of an App Service plan and how they come into play while managing your apps.</span></span>

## <a name="apps-and-app-service-plans"></a><span data-ttu-id="a9957-122">Aplikace a plány služby App Service</span><span class="sxs-lookup"><span data-stu-id="a9957-122">Apps and App Service plans</span></span>

<span data-ttu-id="a9957-123">Aplikace ve službě App Service může být přidružen pouze jeden plán služby App Service v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="a9957-123">An app in App Service can be associated with only one App Service plan at any given time.</span></span>

<span data-ttu-id="a9957-124">Aplikace a plány jsou součástí **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="a9957-124">Both apps and plans are contained in a **resource group**.</span></span> <span data-ttu-id="a9957-125">Skupiny prostředků slouží jako hranice životního cyklu hello každý prostředek, který je v něm.</span><span class="sxs-lookup"><span data-stu-id="a9957-125">A resource group serves as hello lifecycle boundary for every resource that's within it.</span></span> <span data-ttu-id="a9957-126">Můžete použít toomanage skupiny prostředků všechny hello částí aplikace společně.</span><span class="sxs-lookup"><span data-stu-id="a9957-126">You can use resource groups toomanage all hello pieces of an application together.</span></span>

<span data-ttu-id="a9957-127">Vzhledem k tomu, že jedna skupina prostředků může mít víc plány služby App Service, kterou můžete přidělit různé aplikace toodifferent fyzické prostředky.</span><span class="sxs-lookup"><span data-stu-id="a9957-127">Because a single resource group can have multiple App Service plans, you can allocate different apps toodifferent physical resources.</span></span>

<span data-ttu-id="a9957-128">Například můžete oddělit prostředků mezi vývoj, testování a provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a9957-128">For example, you can separate resources among dev, test, and production environments.</span></span> <span data-ttu-id="a9957-129">S samostatných prostředí pro provoz a vývoj nebo testování umožňuje izolovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="a9957-129">Having separate environments for production and dev/test lets you isolate resources.</span></span> <span data-ttu-id="a9957-130">Tímto způsobem zatížení testování proti nové verze aplikace není pokouší o hello stejné prostředky jako produkční aplikací, které slouží skutečnou zákazníků.</span><span class="sxs-lookup"><span data-stu-id="a9957-130">In this way, load testing against a new version of your apps does not compete for hello same resources as your production apps, which are serving real customers.</span></span>

<span data-ttu-id="a9957-131">Pokud máte více plánů v jedna skupina prostředků, můžete také definovat aplikaci, která přesahuje zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="a9957-131">When you have multiple plans in a single resource group, you can also define an application that spans geographical regions.</span></span>

<span data-ttu-id="a9957-132">Například vysokou dostupností aplikaci spuštěnou ve dvou oblastech obsahuje alespoň dva plánům, jeden pro každou oblast a jedna aplikace, které jsou spojené s každou plánu.</span><span class="sxs-lookup"><span data-stu-id="a9957-132">For example, a highly available app running in two regions includes at least two plans, one for each region, and one app associated with each plan.</span></span> <span data-ttu-id="a9957-133">V takovém případě jsou všechny kopie hello aplikace hello pak obsažené v jedna skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="a9957-133">In such a situation, all hello copies of hello app are then contained in a single resource group.</span></span> <span data-ttu-id="a9957-134">Skupinu prostředků s více schématy a víc aplikací mít umožňuje snadno toomanage, řízení a zobrazit stav hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a9957-134">Having a resource group with multiple plans and multiple apps makes it easy toomanage, control, and view hello health of hello application.</span></span>

## <a name="create-an-app-service-plan-or-use-existing-one"></a><span data-ttu-id="a9957-135">Vytvořit plán služby App Service nebo použít existující</span><span class="sxs-lookup"><span data-stu-id="a9957-135">Create an App Service plan or use existing one</span></span>

<span data-ttu-id="a9957-136">Když vytvoříte aplikaci, měli byste zvážit vytvoření skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="a9957-136">When you create an app, you should consider creating a resource group.</span></span> <span data-ttu-id="a9957-137">Na hello druhé straně, pokud je tato aplikace součástí větší aplikace, vytvořte ji v rámci skupiny prostředků hello, který je přidělen pro větší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9957-137">On hello other hand, if this app is a component for a larger application, create it within hello resource group that's allocated for that larger application.</span></span>

<span data-ttu-id="a9957-138">Zda je aplikace hello zcela nové aplikace nebo součást většího, můžete toouse existující plán toohost ji nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="a9957-138">Whether hello app is an altogether new application or part of a larger one, you can choose toouse an existing plan toohost it or create a new one.</span></span> <span data-ttu-id="a9957-139">Toto rozhodnutí se více dotaz kapacitu a očekávané zatížení.</span><span class="sxs-lookup"><span data-stu-id="a9957-139">This decision is more a question of capacity and expected load.</span></span>

<span data-ttu-id="a9957-140">Doporučujeme, abyste izoluje aplikace do nové služby App Service při plánování:</span><span class="sxs-lookup"><span data-stu-id="a9957-140">We recommend isolating your app into a new App Service plan when:</span></span>

- <span data-ttu-id="a9957-141">Aplikace je náročná.</span><span class="sxs-lookup"><span data-stu-id="a9957-141">App is resource-intensive.</span></span>
- <span data-ttu-id="a9957-142">Aplikace má různých faktorech škálování z hello jiné aplikace hostované v existující plán.</span><span class="sxs-lookup"><span data-stu-id="a9957-142">App has different scaling factors from hello other apps hosted in an existing plan.</span></span>
- <span data-ttu-id="a9957-143">Aplikace musí prostředků v jiné zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="a9957-143">App needs resource in a different geographical region.</span></span>

<span data-ttu-id="a9957-144">Tímto způsobem můžete přidělit novou sadu prostředků pro vaši aplikaci a získáte větší kontrolu nad vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9957-144">This way you can allocate a new set of resources for your app and gain greater control of your apps.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="a9957-145">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="a9957-145">Create an App Service plan</span></span>

> [!TIP]
> <span data-ttu-id="a9957-146">Pokud máte služby App Service Environment, můžete zkontrolovat hello dokumentace konkrétní tooApp prostředí Service zde: [vytvořit plán služby App Service ve službě App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span><span class="sxs-lookup"><span data-stu-id="a9957-146">If you have an App Service Environment, you can review hello documentation specific tooApp Service Environments here: [Create an App Service plan in an App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span></span>

<span data-ttu-id="a9957-147">Prázdný plán služby App Service můžete vytvořit z hello prostředí procházení plán aplikační služby nebo jako součást vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9957-147">You can create an empty App Service plan from hello App Service plan browse experience or as part of app creation.</span></span>

<span data-ttu-id="a9957-148">V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní**a potom vyberte **webové aplikace** nebo jiného typu aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="a9957-148">In hello [Azure portal](https://portal.azure.com), click **New** > **Web + mobile**, and then select **Web App** or other App Service app kind.</span></span>

![Vytvoření aplikace v hello portálu Azure.][createWebApp]

<span data-ttu-id="a9957-150">Pak můžete vybrat nebo vytvořit hello plán služby App Service pro novou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="a9957-150">You can then select or create hello App Service plan for hello new app.</span></span>

 ![Vytvořte plán služby App Service.][createASP]

<span data-ttu-id="a9957-152">Klikněte na tlačítko toocreate plán služby App Service **[+] vytvořit nové**, typ hello **plán služby App Service** název a potom vyberte odpovídající **umístění**.</span><span class="sxs-lookup"><span data-stu-id="a9957-152">toocreate an App Service plan, click **[+] Create New**, type hello **App Service plan** name, and then select an appropriate **Location**.</span></span> <span data-ttu-id="a9957-153">Klikněte na tlačítko **cenová úroveň**a potom vyberte příslušné cenovou úroveň služby hello.</span><span class="sxs-lookup"><span data-stu-id="a9957-153">Click **Pricing tier**, and then select an appropriate pricing tier for hello service.</span></span> <span data-ttu-id="a9957-154">Vyberte **zobrazit všechny** tooview více ceny možnosti, jako například **volné** a **sdílené**.</span><span class="sxs-lookup"><span data-stu-id="a9957-154">Select **View all** tooview more pricing options, such as **Free** and **Shared**.</span></span> <span data-ttu-id="a9957-155">Po výběru hello cenové úrovně, klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a9957-155">After you have selected hello pricing tier, click hello **Select** button.</span></span>

## <a name="move-an-app-tooa-different-app-service-plan"></a><span data-ttu-id="a9957-156">Přesunutí aplikace tooa jiný plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="a9957-156">Move an app tooa different App Service plan</span></span>

<span data-ttu-id="a9957-157">Aplikace tooa jiný plán služby App Service můžete přesunout v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a9957-157">You can move an app tooa different App Service plan in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="a9957-158">Aplikace můžete přesunout mezi plány také plány hello jsou v hello stejnou skupinu prostředků a geografické oblasti.</span><span class="sxs-lookup"><span data-stu-id="a9957-158">You can move apps between plans as long as hello plans are in hello same resource group and geographical region.</span></span>

<span data-ttu-id="a9957-159">toomove plán tooanother aplikace:</span><span class="sxs-lookup"><span data-stu-id="a9957-159">toomove an app tooanother plan:</span></span>

- <span data-ttu-id="a9957-160">Přejděte toohello aplikace, které chcete toomove.</span><span class="sxs-lookup"><span data-stu-id="a9957-160">Navigate toohello app that you want toomove.</span></span>
- <span data-ttu-id="a9957-161">V hello **nabídky**, vyhledejte hello **plán služby App Service** části.</span><span class="sxs-lookup"><span data-stu-id="a9957-161">In hello **Menu**, look for hello **App Service Plan** section.</span></span>
- <span data-ttu-id="a9957-162">Vyberte **plán služby App Service změnu** toostart hello procesu.</span><span class="sxs-lookup"><span data-stu-id="a9957-162">Select **Change App Service plan** toostart hello process.</span></span>

<span data-ttu-id="a9957-163">**Plán služby App Service změnu** otevře hello **plán služby App Service** selektor.</span><span class="sxs-lookup"><span data-stu-id="a9957-163">**Change App Service plan** opens hello **App Service plan** selector.</span></span> <span data-ttu-id="a9957-164">V tomto okamžiku můžete vybrat existující plán toomove do této aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9957-164">At this point, you can pick an existing plan toomove this app into.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9957-165">plán služby App Service vyberte Hello uživatelského rozhraní se filtrují podle hello následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="a9957-165">hello select App Service plan UI is filtered by hello following criteria:</span></span>
> - <span data-ttu-id="a9957-166">Existuje v rámci hello stejné skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a9957-166">Exists within hello same Resource Group</span></span>
> - <span data-ttu-id="a9957-167">Existuje v hello stejné zeměpisné oblasti</span><span class="sxs-lookup"><span data-stu-id="a9957-167">Exists in hello same Geographical Region</span></span>
> - <span data-ttu-id="a9957-168">Existuje v rámci hello stejný webový prostor</span><span class="sxs-lookup"><span data-stu-id="a9957-168">Exists within hello same Webspace</span></span>
>
> <span data-ttu-id="a9957-169">Webový prostor je logická konstrukce v App Service, která definuje seskupení prostředků serveru.</span><span class="sxs-lookup"><span data-stu-id="a9957-169">A Webspace is a logical construct within App Service which defines a grouping of server resources.</span></span> <span data-ttu-id="a9957-170">Geografické oblasti (například západní USA) obsahuje mnoho Webspaces v pořadí tooallocate zákazníky používající službu aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9957-170">A Geographical region (such as West US) contains many Webspaces in order tooallocate customers using App Service.</span></span> <span data-ttu-id="a9957-171">V současné době prostředky služby App Service nejsou možné toobe přesouvat mezi Webspaces.</span><span class="sxs-lookup"><span data-stu-id="a9957-171">Currently, App Service resources aren’t able toobe moved between Webspaces.</span></span>
>

![Selektor plán služby App Service.][change]

<span data-ttu-id="a9957-173">Každý plán má svou vlastní cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="a9957-173">Each plan has its own pricing tier.</span></span> <span data-ttu-id="a9957-174">Přesun lokalitu z úrovně Standard tooa úroveň Free, například umožňuje všechny aplikace, které jsou přiřazeny tooit toouse hello funkce a prostředky hello úrovně Standard.</span><span class="sxs-lookup"><span data-stu-id="a9957-174">For example, moving a site from a Free tier tooa Standard tier, enables all apps assigned tooit toouse hello features and resources of hello Standard tier.</span></span>

## <a name="clone-an-app-tooa-different-app-service-plan"></a><span data-ttu-id="a9957-175">Klonování aplikace tooa jiný plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="a9957-175">Clone an app tooa different App Service plan</span></span>

<span data-ttu-id="a9957-176">Pokud chcete toomove hello aplikace tooa jiné oblasti, jeden alternativou je aplikace klonování.</span><span class="sxs-lookup"><span data-stu-id="a9957-176">If you want toomove hello app tooa different region, one alternative is app cloning.</span></span> <span data-ttu-id="a9957-177">Klonování zhotoví kopii aplikaci v nové nebo existující plán služby App Service v libovolné oblasti.</span><span class="sxs-lookup"><span data-stu-id="a9957-177">Cloning makes a copy of your app in a new or existing App Service plan in any region.</span></span>

<span data-ttu-id="a9957-178">Můžete najít **klonování aplikace** v hello **nástroje pro vývoj** hello nabídky.</span><span class="sxs-lookup"><span data-stu-id="a9957-178">You can find **Clone App** in hello **Development Tools** section of hello menu.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9957-179">Klonování má určitá omezení, které si můžete přečíst o na [klonování aplikace Azure App Service pomocí portálu Azure](../app-service-web/app-service-web-app-cloning-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a9957-179">Cloning has some limitations that you can read about at [Azure App Service App cloning using Azure portal](../app-service-web/app-service-web-app-cloning-portal.md).</span></span>

## <a name="scale-an-app-service-plan"></a><span data-ttu-id="a9957-180">Škálovat plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="a9957-180">Scale an App Service plan</span></span>

<span data-ttu-id="a9957-181">Existují tři způsoby tooscale plánu:</span><span class="sxs-lookup"><span data-stu-id="a9957-181">There are three ways tooscale a plan:</span></span>

- <span data-ttu-id="a9957-182">**Změnit plán hello je cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="a9957-182">**Change hello plan’s pricing tier**.</span></span> <span data-ttu-id="a9957-183">Plán v základní vrstvě hello může být převedená tooStandard a všechny aplikace přiřazené tooit toouse hello funkce úrovně Standard hello.</span><span class="sxs-lookup"><span data-stu-id="a9957-183">A plan in hello Basic tier can be converted tooStandard, and all apps assigned tooit toouse hello features of hello Standard tier.</span></span>
- <span data-ttu-id="a9957-184">**Změňte velikost instance plánu hello**.</span><span class="sxs-lookup"><span data-stu-id="a9957-184">**Change hello plan’s instance size**.</span></span> <span data-ttu-id="a9957-185">Jako příklad plán v základní vrstvě hello, který používá malé instance může být změněné toouse velké instancí.</span><span class="sxs-lookup"><span data-stu-id="a9957-185">As an example, a plan in hello Basic tier that uses small instances can be changed toouse large instances.</span></span> <span data-ttu-id="a9957-186">Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť hello a prostředky procesoru, které hello větší velikost nabídky instanci.</span><span class="sxs-lookup"><span data-stu-id="a9957-186">All apps that are associated with that plan now can use hello additional memory and CPU resources that hello larger instance size offers.</span></span>
- <span data-ttu-id="a9957-187">**Změnit počet instancí plánu hello**.</span><span class="sxs-lookup"><span data-stu-id="a9957-187">**Change hello plan’s instance count**.</span></span> <span data-ttu-id="a9957-188">Například standardní plán, který je škálovat instance toothree lze škálovat too10 instance.</span><span class="sxs-lookup"><span data-stu-id="a9957-188">For example, a Standard plan that's scaled out toothree instances can be scaled too10 instances.</span></span> <span data-ttu-id="a9957-189">Plán Premium můžete škálovat instance too20 (subjektu tooavailability).</span><span class="sxs-lookup"><span data-stu-id="a9957-189">A Premium plan can be scaled out too20 instances (subject tooavailability).</span></span> <span data-ttu-id="a9957-190">Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť hello a prostředky procesoru, které hello větší počet nabídky instanci.</span><span class="sxs-lookup"><span data-stu-id="a9957-190">All apps that are associated with that plan now can use hello additional memory and CPU resources that hello larger instance count offers.</span></span>

<span data-ttu-id="a9957-191">Můžete změnit hello cenové úrovně a instance velikosti kliknutím hello **vertikálně navýšit kapacitu** položky v části nastavení pro aplikace hello nebo hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="a9957-191">You can change hello pricing tier and instance size by clicking hello **Scale Up** item under settings for either hello app or hello App Service plan.</span></span> <span data-ttu-id="a9957-192">Změny použít toohello plán služby App Service a ovlivňují všechny aplikace, které ji hostuje.</span><span class="sxs-lookup"><span data-stu-id="a9957-192">Changes apply toohello App Service plan and affect all apps that it hosts.</span></span>

 ![Nastavte hodnoty tooscale aplikaci.][pricingtier]

## <a name="app-service-plan-cleanup"></a><span data-ttu-id="a9957-194">Vyčištění plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="a9957-194">App Service plan cleanup</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a9957-195">**Plánů služby App Service** mají žádné aplikace přidružené k toothem stále platit poplatky vzhledem k tomu, že budou pokračovat v práci tooreserve hello výpočetní kapacitu.</span><span class="sxs-lookup"><span data-stu-id="a9957-195">**App Service plans** that have no apps associated toothem still incur charges since they continue tooreserve hello compute capacity.</span></span>

<span data-ttu-id="a9957-196">tooavoid neočekávané poplatky, když se odstraní poslední aplikace hello hostovaná v plán služby App Service hello výsledné prázdný, je taky odstranit plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="a9957-196">tooavoid unexpected charges, when hello last app hosted in an App Service plan is deleted, hello resulting empty App Service plan is also deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="a9957-197">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a9957-197">Summary</span></span>

<span data-ttu-id="a9957-198">Plány aplikační služby představují sadu funkcí a kapacity, které můžete sdílet mezi aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="a9957-198">App Service plans represent a set of features and capacity that you can share across your apps.</span></span> <span data-ttu-id="a9957-199">Udělte hello flexibilitu tooallocate konkrétní aplikace tooa sadu prostředků a dále optimalizovat využití vaší prostředků Azure plánů služby App Service.</span><span class="sxs-lookup"><span data-stu-id="a9957-199">App Service plans give you hello flexibility tooallocate specific apps tooa set of resources and further optimize your Azure resource utilization.</span></span> <span data-ttu-id="a9957-200">Tímto způsobem, pokud chcete toosave peníze u svého testovacího prostředí, můžete plán sdílet mezi více aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="a9957-200">This way, if you want toosave money on your testing environment, you can share a plan across multiple apps.</span></span> <span data-ttu-id="a9957-201">Pomocí příjmu v několika oblastech a plány a také můžete maximalizovat propustnost pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="a9957-201">You can also maximize throughput for your production environment by scaling it across multiple regions and plans.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="a9957-202">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="a9957-202">What's changed</span></span>

- <span data-ttu-id="a9957-203">Průvodce toohello změnu z tooApp weby služby, najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="a9957-203">For a guide toohello change from Websites tooApp Service, see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
