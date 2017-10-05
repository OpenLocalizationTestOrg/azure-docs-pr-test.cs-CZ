---
title: "Podrobný přehled plánů služby Azure App Service | Microsoft Docs"
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
ms.openlocfilehash: f97be571d104e3cc1c6ee732886fa7133ba0dc83
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a><span data-ttu-id="35644-104">Podrobný přehled plánů Azure App Service</span><span class="sxs-lookup"><span data-stu-id="35644-104">Azure App Service plans in-depth overview</span></span>

<span data-ttu-id="35644-105">Plány aplikační služby představují kolekci fyzické prostředky, které jsou použity k hostování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="35644-105">App Service plans represent the collection of physical resources used to host your apps.</span></span>

<span data-ttu-id="35644-106">Plány služby App Service definují:</span><span class="sxs-lookup"><span data-stu-id="35644-106">App Service plans define:</span></span>

- <span data-ttu-id="35644-107">Oblast (západní USA, Východ USA, atd.)</span><span class="sxs-lookup"><span data-stu-id="35644-107">Region (West US, East US, etc.)</span></span>
- <span data-ttu-id="35644-108">Počet škálování (jeden, dva, tři instance atd.)</span><span class="sxs-lookup"><span data-stu-id="35644-108">Scale count (one, two, three instances, etc.)</span></span>
- <span data-ttu-id="35644-109">Velikost instance (malé, střední, velké)</span><span class="sxs-lookup"><span data-stu-id="35644-109">Instance size (Small, Medium, Large)</span></span>
- <span data-ttu-id="35644-110">SKU (Free, Shared, Basic, Standard, Premium)</span><span class="sxs-lookup"><span data-stu-id="35644-110">SKU (Free, Shared, Basic, Standard, Premium)</span></span>

<span data-ttu-id="35644-111">Webové aplikace, mobilní aplikace, aplikace API, funkce aplikace (nebo funkce), v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) všechny spuštění v rámci plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="35644-111">Web Apps, Mobile Apps, API Apps, Function Apps (or Functions), in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) all run in an App Service plan.</span></span>  <span data-ttu-id="35644-112">Plán služby App Service můžete sdílet aplikací ve stejném předplatném a oblast.</span><span class="sxs-lookup"><span data-stu-id="35644-112">Apps in the same subscription, and region can share an App Service plan.</span></span> 

<span data-ttu-id="35644-113">Všechny aplikace, které jsou přiřazené **plán služby App Service** sdílet prostředky definované ho.</span><span class="sxs-lookup"><span data-stu-id="35644-113">All applications assigned to an **App Service plan** share the resources defined by it.</span></span> <span data-ttu-id="35644-114">Tato sdílení úsporu nákladů při hostování více aplikacemi v jednom plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="35644-114">This sharing saves money when hosting multiple apps in a single App Service plan.</span></span>

<span data-ttu-id="35644-115">Váš **plán služby App Service** se může škálovat od skladových jednotek (SKU) úrovní **Free** a **Shared** po SKU úrovní **Basic**, **Standard** a **Premium** a vy při tom získáte přístup k dalším prostředkům a funkcím.</span><span class="sxs-lookup"><span data-stu-id="35644-115">Your **App Service plan** can scale from **Free** and **Shared** SKUs to **Basic**, **Standard**, and **Premium** SKUs giving you access to more resources and features along the way.</span></span>

<span data-ttu-id="35644-116">Pokud váš plán služby App Service je nastavený na **základní** SKU nebo vyšší, pak můžete řídit **velikost** a škálování počtu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="35644-116">If your App Service plan is set to **Basic** SKU or higher, then you can control the **size** and scale count of the VMs.</span></span>

<span data-ttu-id="35644-117">Například pokud váš plán je nakonfigurovaný na použití dvě "malá" instance ve vrstvě služby na úrovni standard, všechny aplikace, které jsou přidruženy tento plán spustit na obou instancí.</span><span class="sxs-lookup"><span data-stu-id="35644-117">For example, if your plan is configured to use two "small" instances in the standard service tier, all apps that are associated with that plan run on both instances.</span></span> <span data-ttu-id="35644-118">Aplikace také mají přístup k funkcím vrstvy služby na úrovni standard.</span><span class="sxs-lookup"><span data-stu-id="35644-118">Apps also have access to the standard service tier features.</span></span> <span data-ttu-id="35644-119">Instancích plánu, na kterých běží aplikace jsou plně spravovaná a vysoce dostupné.</span><span class="sxs-lookup"><span data-stu-id="35644-119">Plan instances on which apps are running are fully managed and highly available.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="35644-120">**SKU** a **Škálování** plánu služby App Service určují cenu, ne počet aplikací hostovaných ve službě.</span><span class="sxs-lookup"><span data-stu-id="35644-120">The **SKU** and **Scale** of the App Service plan determines the cost and not the number of apps hosted in it.</span></span>

<span data-ttu-id="35644-121">Tento článek popisuje klíčové vlastnosti, například vrstvy a škále plán služby App Service a jak se do play při správě aplikací.</span><span class="sxs-lookup"><span data-stu-id="35644-121">This article explores the key characteristics, such as tier and scale, of an App Service plan and how they come into play while managing your apps.</span></span>

## <a name="apps-and-app-service-plans"></a><span data-ttu-id="35644-122">Aplikace a plány služby App Service</span><span class="sxs-lookup"><span data-stu-id="35644-122">Apps and App Service plans</span></span>

<span data-ttu-id="35644-123">Aplikace ve službě App Service může být přidružen pouze jeden plán služby App Service v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="35644-123">An app in App Service can be associated with only one App Service plan at any given time.</span></span>

<span data-ttu-id="35644-124">Aplikace a plány jsou součástí **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="35644-124">Both apps and plans are contained in a **resource group**.</span></span> <span data-ttu-id="35644-125">Skupiny prostředků slouží jako hranice životního cyklu pro každý prostředek, který je v něm.</span><span class="sxs-lookup"><span data-stu-id="35644-125">A resource group serves as the lifecycle boundary for every resource that's within it.</span></span> <span data-ttu-id="35644-126">Skupiny prostředků můžete spravovat všechny součásti aplikace společně.</span><span class="sxs-lookup"><span data-stu-id="35644-126">You can use resource groups to manage all the pieces of an application together.</span></span>

<span data-ttu-id="35644-127">Vzhledem k tomu, že jedna skupina prostředků může mít víc plány služby App Service, kterou můžete přidělit různé aplikace na různé fyzické prostředky.</span><span class="sxs-lookup"><span data-stu-id="35644-127">Because a single resource group can have multiple App Service plans, you can allocate different apps to different physical resources.</span></span>

<span data-ttu-id="35644-128">Například můžete oddělit prostředků mezi vývoj, testování a provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="35644-128">For example, you can separate resources among dev, test, and production environments.</span></span> <span data-ttu-id="35644-129">S samostatných prostředí pro provoz a vývoj nebo testování umožňuje izolovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="35644-129">Having separate environments for production and dev/test lets you isolate resources.</span></span> <span data-ttu-id="35644-130">Tímto způsobem není testování proti nové verze aplikace zatížení pokouší o stejné prostředky jako produkční aplikací, které slouží skutečnou zákazníků.</span><span class="sxs-lookup"><span data-stu-id="35644-130">In this way, load testing against a new version of your apps does not compete for the same resources as your production apps, which are serving real customers.</span></span>

<span data-ttu-id="35644-131">Pokud máte více plánů v jedna skupina prostředků, můžete také definovat aplikaci, která přesahuje zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="35644-131">When you have multiple plans in a single resource group, you can also define an application that spans geographical regions.</span></span>

<span data-ttu-id="35644-132">Například vysokou dostupností aplikaci spuštěnou ve dvou oblastech obsahuje alespoň dva plánům, jeden pro každou oblast a jedna aplikace, které jsou spojené s každou plánu.</span><span class="sxs-lookup"><span data-stu-id="35644-132">For example, a highly available app running in two regions includes at least two plans, one for each region, and one app associated with each plan.</span></span> <span data-ttu-id="35644-133">V takovém případě jsou všechny kopie aplikace pak obsažené v jedna skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="35644-133">In such a situation, all the copies of the app are then contained in a single resource group.</span></span> <span data-ttu-id="35644-134">Skupinu prostředků s více schématy a víc aplikací mít usnadňuje správu, řízení a zobrazení stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="35644-134">Having a resource group with multiple plans and multiple apps makes it easy to manage, control, and view the health of the application.</span></span>

## <a name="create-an-app-service-plan-or-use-existing-one"></a><span data-ttu-id="35644-135">Vytvořit plán služby App Service nebo použít existující</span><span class="sxs-lookup"><span data-stu-id="35644-135">Create an App Service plan or use existing one</span></span>

<span data-ttu-id="35644-136">Když vytvoříte aplikaci, měli byste zvážit vytvoření skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="35644-136">When you create an app, you should consider creating a resource group.</span></span> <span data-ttu-id="35644-137">Na druhé straně Pokud je tato aplikace součástí větší aplikace, vytvořte ho v rámci skupiny prostředků, který je přidělen pro větší aplikace.</span><span class="sxs-lookup"><span data-stu-id="35644-137">On the other hand, if this app is a component for a larger application, create it within the resource group that's allocated for that larger application.</span></span>

<span data-ttu-id="35644-138">Zda aplikace je zcela nové aplikace nebo součást většího, můžete použít existující plán ji hostovat nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="35644-138">Whether the app is an altogether new application or part of a larger one, you can choose to use an existing plan to host it or create a new one.</span></span> <span data-ttu-id="35644-139">Toto rozhodnutí se více dotaz kapacitu a očekávané zatížení.</span><span class="sxs-lookup"><span data-stu-id="35644-139">This decision is more a question of capacity and expected load.</span></span>

<span data-ttu-id="35644-140">Doporučujeme, abyste izoluje aplikace do nové služby App Service při plánování:</span><span class="sxs-lookup"><span data-stu-id="35644-140">We recommend isolating your app into a new App Service plan when:</span></span>

- <span data-ttu-id="35644-141">Aplikace je náročná.</span><span class="sxs-lookup"><span data-stu-id="35644-141">App is resource-intensive.</span></span>
- <span data-ttu-id="35644-142">Aplikace má různých faktorech škálování z jiných aplikací hostovaná v existující plán.</span><span class="sxs-lookup"><span data-stu-id="35644-142">App has different scaling factors from the other apps hosted in an existing plan.</span></span>
- <span data-ttu-id="35644-143">Aplikace musí prostředků v jiné zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="35644-143">App needs resource in a different geographical region.</span></span>

<span data-ttu-id="35644-144">Tímto způsobem můžete přidělit novou sadu prostředků pro vaši aplikaci a získáte větší kontrolu nad vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="35644-144">This way you can allocate a new set of resources for your app and gain greater control of your apps.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="35644-145">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="35644-145">Create an App Service plan</span></span>

> [!TIP]
> <span data-ttu-id="35644-146">Pokud máte služby App Service Environment, najdete v dokumentaci specifické pro prostředí App Service zde: [vytvořit plán služby App Service ve službě App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span><span class="sxs-lookup"><span data-stu-id="35644-146">If you have an App Service Environment, you can review the documentation specific to App Service Environments here: [Create an App Service plan in an App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span></span>

<span data-ttu-id="35644-147">Prázdný plán služby App Service můžete vytvořit z prostředí procházení plán aplikační služby nebo jako součást vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="35644-147">You can create an empty App Service plan from the App Service plan browse experience or as part of app creation.</span></span>

<span data-ttu-id="35644-148">V [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní**a potom vyberte **webové aplikace** nebo jiného typu aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="35644-148">In the [Azure portal](https://portal.azure.com), click **New** > **Web + mobile**, and then select **Web App** or other App Service app kind.</span></span>

![Vytvoření aplikace v portálu Azure.][createWebApp]

<span data-ttu-id="35644-150">Pak můžete vybrat nebo vytvořit plán služby App Service pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="35644-150">You can then select or create the App Service plan for the new app.</span></span>

 ![Vytvořte plán služby App Service.][createASP]

<span data-ttu-id="35644-152">Chcete-li vytvořit plán služby App Service, klikněte na tlačítko **[+] vytvořit nové**, typ **plán služby App Service** název a potom vyberte odpovídající **umístění**.</span><span class="sxs-lookup"><span data-stu-id="35644-152">To create an App Service plan, click **[+] Create New**, type the **App Service plan** name, and then select an appropriate **Location**.</span></span> <span data-ttu-id="35644-153">Klikněte na tlačítko **cenová úroveň**a potom vyberte příslušné cenovou úroveň pro službu.</span><span class="sxs-lookup"><span data-stu-id="35644-153">Click **Pricing tier**, and then select an appropriate pricing tier for the service.</span></span> <span data-ttu-id="35644-154">Vyberte **zobrazit všechny** do zobrazení více ceny možnosti, jako například **volné** a **sdílené**.</span><span class="sxs-lookup"><span data-stu-id="35644-154">Select **View all** to view more pricing options, such as **Free** and **Shared**.</span></span> <span data-ttu-id="35644-155">Až vyberete cenovou úroveň, klikněte na **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="35644-155">After you have selected the pricing tier, click the **Select** button.</span></span>

## <a name="move-an-app-to-a-different-app-service-plan"></a><span data-ttu-id="35644-156">Přesuňte aplikace na jiný plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="35644-156">Move an app to a different App Service plan</span></span>

<span data-ttu-id="35644-157">Aplikace můžete přesunout na jiný plán služby App Service v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="35644-157">You can move an app to a different App Service plan in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="35644-158">Aplikace můžete přesouvat mezi plány také plány jsou ve stejné skupině prostředků a geografické oblasti.</span><span class="sxs-lookup"><span data-stu-id="35644-158">You can move apps between plans as long as the plans are in the same resource group and geographical region.</span></span>

<span data-ttu-id="35644-159">Přesunutí aplikace do jiného plánu:</span><span class="sxs-lookup"><span data-stu-id="35644-159">To move an app to another plan:</span></span>

- <span data-ttu-id="35644-160">Přejděte do aplikace, který chcete přesunout.</span><span class="sxs-lookup"><span data-stu-id="35644-160">Navigate to the app that you want to move.</span></span>
- <span data-ttu-id="35644-161">V **nabídky**, vyhledejte **plán služby App Service** části.</span><span class="sxs-lookup"><span data-stu-id="35644-161">In the **Menu**, look for the **App Service Plan** section.</span></span>
- <span data-ttu-id="35644-162">Vyberte **plán služby App Service změnu** ke spuštění procesu.</span><span class="sxs-lookup"><span data-stu-id="35644-162">Select **Change App Service plan** to start the process.</span></span>

<span data-ttu-id="35644-163">**Plán služby App Service změnu** otevře **plán služby App Service** selektor.</span><span class="sxs-lookup"><span data-stu-id="35644-163">**Change App Service plan** opens the **App Service plan** selector.</span></span> <span data-ttu-id="35644-164">V tomto okamžiku můžete vybrat existující plán přesunout do této aplikace.</span><span class="sxs-lookup"><span data-stu-id="35644-164">At this point, you can pick an existing plan to move this app into.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="35644-165">Vyberte plán služby App Service uživatelského rozhraní se filtrují podle následujících kritérií:</span><span class="sxs-lookup"><span data-stu-id="35644-165">The select App Service plan UI is filtered by the following criteria:</span></span>
> - <span data-ttu-id="35644-166">Existuje v rámci stejné skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="35644-166">Exists within the same Resource Group</span></span>
> - <span data-ttu-id="35644-167">Existuje ve stejné zeměpisné oblasti</span><span class="sxs-lookup"><span data-stu-id="35644-167">Exists in the same Geographical Region</span></span>
> - <span data-ttu-id="35644-168">Existuje v rámci stejné webový prostor</span><span class="sxs-lookup"><span data-stu-id="35644-168">Exists within the same Webspace</span></span>
>
> <span data-ttu-id="35644-169">Webový prostor je logická konstrukce v App Service, která definuje seskupení prostředků serveru.</span><span class="sxs-lookup"><span data-stu-id="35644-169">A Webspace is a logical construct within App Service which defines a grouping of server resources.</span></span> <span data-ttu-id="35644-170">Geografické oblasti (například západní USA) obsahuje mnoho Webspaces aby bylo možné přidělit zákazníky používající službu aplikace.</span><span class="sxs-lookup"><span data-stu-id="35644-170">A Geographical region (such as West US) contains many Webspaces in order to allocate customers using App Service.</span></span> <span data-ttu-id="35644-171">V současné době není možné přesunout mezi Webspaces prostředky služby App Service.</span><span class="sxs-lookup"><span data-stu-id="35644-171">Currently, App Service resources aren’t able to be moved between Webspaces.</span></span>
>

![Selektor plán služby App Service.][change]

<span data-ttu-id="35644-173">Každý plán má svou vlastní cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="35644-173">Each plan has its own pricing tier.</span></span> <span data-ttu-id="35644-174">Například přesun lokalitu z úrovně Free plán úrovně Standard, umožňuje všechny aplikace, které jsou přiřazené k použití funkcí a prostředků na plán úrovně Standard.</span><span class="sxs-lookup"><span data-stu-id="35644-174">For example, moving a site from a Free tier to a Standard tier, enables all apps assigned to it to use the features and resources of the Standard tier.</span></span>

## <a name="clone-an-app-to-a-different-app-service-plan"></a><span data-ttu-id="35644-175">Klonování aplikace na jiný plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="35644-175">Clone an app to a different App Service plan</span></span>

<span data-ttu-id="35644-176">Pokud chcete přesunout aplikace v jiné oblasti, jeden alternativou je aplikace klonování.</span><span class="sxs-lookup"><span data-stu-id="35644-176">If you want to move the app to a different region, one alternative is app cloning.</span></span> <span data-ttu-id="35644-177">Klonování zhotoví kopii aplikaci v nové nebo existující plán služby App Service v libovolné oblasti.</span><span class="sxs-lookup"><span data-stu-id="35644-177">Cloning makes a copy of your app in a new or existing App Service plan in any region.</span></span>

<span data-ttu-id="35644-178">Můžete najít **klonování aplikace** v **nástroje pro vývoj** části nabídky.</span><span class="sxs-lookup"><span data-stu-id="35644-178">You can find **Clone App** in the **Development Tools** section of the menu.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="35644-179">Klonování má určitá omezení, které si můžete přečíst o na [klonování aplikace Azure App Service pomocí portálu Azure](../app-service-web/app-service-web-app-cloning-portal.md).</span><span class="sxs-lookup"><span data-stu-id="35644-179">Cloning has some limitations that you can read about at [Azure App Service App cloning using Azure portal](../app-service-web/app-service-web-app-cloning-portal.md).</span></span>

## <a name="scale-an-app-service-plan"></a><span data-ttu-id="35644-180">Škálovat plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="35644-180">Scale an App Service plan</span></span>

<span data-ttu-id="35644-181">Existují tři způsoby, jak škálovat plán:</span><span class="sxs-lookup"><span data-stu-id="35644-181">There are three ways to scale a plan:</span></span>

- <span data-ttu-id="35644-182">**Změna plánu je cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="35644-182">**Change the plan’s pricing tier**.</span></span> <span data-ttu-id="35644-183">Plán v základní vrstvě lze převést na Standard a všechny aplikace, které jsou přiřazené k použití funkcí na plán úrovně Standard.</span><span class="sxs-lookup"><span data-stu-id="35644-183">A plan in the Basic tier can be converted to Standard, and all apps assigned to it to use the features of the Standard tier.</span></span>
- <span data-ttu-id="35644-184">**Změňte velikost instance plánu**.</span><span class="sxs-lookup"><span data-stu-id="35644-184">**Change the plan’s instance size**.</span></span> <span data-ttu-id="35644-185">Jako příklad můžete změnit plán v základní vrstvě, který používá malé instance na používání velké instancí.</span><span class="sxs-lookup"><span data-stu-id="35644-185">As an example, a plan in the Basic tier that uses small instances can be changed to use large instances.</span></span> <span data-ttu-id="35644-186">Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť a prostředky procesoru, které nabízí větší velikost instance.</span><span class="sxs-lookup"><span data-stu-id="35644-186">All apps that are associated with that plan now can use the additional memory and CPU resources that the larger instance size offers.</span></span>
- <span data-ttu-id="35644-187">**Změnit počet instancí plánu**.</span><span class="sxs-lookup"><span data-stu-id="35644-187">**Change the plan’s instance count**.</span></span> <span data-ttu-id="35644-188">Například standardní plán, který je na tři instance škálovat na více systémů můžete škálovat na 10 instancí.</span><span class="sxs-lookup"><span data-stu-id="35644-188">For example, a Standard plan that's scaled out to three instances can be scaled to 10 instances.</span></span> <span data-ttu-id="35644-189">Plán Premium můžete škálovat na více systémů na 20 instance (přičemž podléhá dostupnosti).</span><span class="sxs-lookup"><span data-stu-id="35644-189">A Premium plan can be scaled out to 20 instances (subject to availability).</span></span> <span data-ttu-id="35644-190">Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť a prostředky procesoru, které nabízí větší počet instancí.</span><span class="sxs-lookup"><span data-stu-id="35644-190">All apps that are associated with that plan now can use the additional memory and CPU resources that the larger instance count offers.</span></span>

<span data-ttu-id="35644-191">Můžete změnit cenovou úroveň a instance velikost klepnutím **vertikálně navýšit kapacitu** položky v části nastavení pro aplikace nebo plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="35644-191">You can change the pricing tier and instance size by clicking the **Scale Up** item under settings for either the app or the App Service plan.</span></span> <span data-ttu-id="35644-192">Změny použít na plán služby App Service a ovlivňují všechny aplikace, které je hostitelem.</span><span class="sxs-lookup"><span data-stu-id="35644-192">Changes apply to the App Service plan and affect all apps that it hosts.</span></span>

 ![Nastavte hodnoty škálování aplikace.][pricingtier]

## <a name="app-service-plan-cleanup"></a><span data-ttu-id="35644-194">Vyčištění plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="35644-194">App Service plan cleanup</span></span>

> [!IMPORTANT]
> <span data-ttu-id="35644-195">**Plánů služby App Service** mají žádné aplikace přidružené k je stále platit poplatky vzhledem k tomu, že budou nadále záložní kapacita výpočty.</span><span class="sxs-lookup"><span data-stu-id="35644-195">**App Service plans** that have no apps associated to them still incur charges since they continue to reserve the compute capacity.</span></span>

<span data-ttu-id="35644-196">Nechcete neočekávané poplatky, když se odstraní poslední aplikace hostovaná v plán služby App Service, je taky odstranit výsledné prázdný plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="35644-196">To avoid unexpected charges, when the last app hosted in an App Service plan is deleted, the resulting empty App Service plan is also deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="35644-197">Souhrn</span><span class="sxs-lookup"><span data-stu-id="35644-197">Summary</span></span>

<span data-ttu-id="35644-198">Plány aplikační služby představují sadu funkcí a kapacity, které můžete sdílet mezi aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="35644-198">App Service plans represent a set of features and capacity that you can share across your apps.</span></span> <span data-ttu-id="35644-199">Plány služby App Service poskytují flexibilitu pro přidělovat konkrétní aplikace sadu prostředků a dále optimalizovat využití vaší prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="35644-199">App Service plans give you the flexibility to allocate specific apps to a set of resources and further optimize your Azure resource utilization.</span></span> <span data-ttu-id="35644-200">Tímto způsobem, pokud chcete ušetřit peníze u svého testovacího prostředí, můžete plán sdílet mezi více aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="35644-200">This way, if you want to save money on your testing environment, you can share a plan across multiple apps.</span></span> <span data-ttu-id="35644-201">Pomocí příjmu v několika oblastech a plány a také můžete maximalizovat propustnost pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="35644-201">You can also maximize throughput for your production environment by scaling it across multiple regions and plans.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="35644-202">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="35644-202">What's changed</span></span>

- <span data-ttu-id="35644-203">Průvodce změnou z webů na službu App Service, najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="35644-203">For a guide to the change from Websites to App Service, see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
