---
title: "aaaReference pro navigaci hello portálu Azure"
description: "Přečtěte si informace hello jiné uživatelské prostředí pro webové aplikace služby mezi hello portálu pro správu a hello portálu Azure"
services: app-service
documentationcenter: 
author: jaime-espinosa
manager: erikre
editor: jimbe
ms.assetid: 0cc6a3cc-bd89-4a96-9177-d25f6fb737bb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: jaime-espinosa
ms.openlocfilehash: dcf7c1fc17f9a0c31005ad0f2fd53723d2966058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reference-for-navigating-hello-azure-portal"></a><span data-ttu-id="99e37-103">Referenční dokumentace pro navigaci hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="99e37-103">Reference for navigating hello Azure portal</span></span>
<span data-ttu-id="99e37-104">Weby Azure se nyní nazývají [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="99e37-104">Azure Websites are now called [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="99e37-105">Aktualizujeme všechny naše dokumentace tooreflect tento název změn a tooprovide pokyny pro hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="99e37-105">We're updating all of our documentation tooreflect this name change and tooprovide instructions for hello Azure Portal.</span></span> <span data-ttu-id="99e37-106">Až do dokončení tohoto procesu můžete použít tento dokument jako vodítko pro práci s webovými aplikacemi v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="99e37-106">Until that process is done, you can use this document as a guide for working with Web Apps in hello Azure portal.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="hello-future-of-hello-azure-classic-portal"></a><span data-ttu-id="99e37-107">Hello budoucnosti hello portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="99e37-107">hello future of hello Azure Classic Portal</span></span>
<span data-ttu-id="99e37-108">Když si všimnete hello branding změny hello portálu Azure Classic, je tohoto portálu v hello proces nahrazují hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="99e37-108">While you will notice hello branding changes on hello Azure Classic Portal, that portal is in hello process of being replaced by hello Azure Portal.</span></span> <span data-ttu-id="99e37-109">Protože portál classic hello je vyřazován, je hello fokus pro nový vývoj posunem toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="99e37-109">As hello classic portal is being phased out, hello focus for new development is shifting toohello Azure Portal.</span></span> <span data-ttu-id="99e37-110">Vrátí všechny nadcházející nové funkce pro webové aplikace se v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="99e37-110">All upcoming new features for Web Apps will come in hello Azure Portal.</span></span> <span data-ttu-id="99e37-111">Začít používat hello portálu Azure tootake výhod hello nejnovější a největší webové aplikace musí mít toooffer.</span><span class="sxs-lookup"><span data-stu-id="99e37-111">Start using hello Azure Portal tootake advantage of hello latest and greatest that Web Apps have toooffer.</span></span>

## <a name="layout-differences-between-hello-azure-classic-portal-and-azure-portal"></a><span data-ttu-id="99e37-112">Rozložení rozdíly mezi hello portálu Azure Classic a portálu Azure</span><span class="sxs-lookup"><span data-stu-id="99e37-112">Layout differences between hello Azure Classic Portal and Azure Portal</span></span>
<span data-ttu-id="99e37-113">Na portálu classic hello všechny hello Azure services jsou uvedeny na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="99e37-113">In hello classic portal, all hello Azure services are listed on hello left hand side.</span></span> <span data-ttu-id="99e37-114">Navigace na portálu classic hello následuje stromová struktura, kde můžete spustit ze služby hello a přejděte na každý element.</span><span class="sxs-lookup"><span data-stu-id="99e37-114">Navigation in hello classic portal follows a tree structure, where you start from hello service and navigate into each element.</span></span> <span data-ttu-id="99e37-115">Tato struktura dobře funguje při správě nezávislé komponenty.</span><span class="sxs-lookup"><span data-stu-id="99e37-115">This structure works well when managing independent components.</span></span> <span data-ttu-id="99e37-116">Aplikace založené na Azure se však kolekce vzájemně propojené služby, a tato struktura stromu není ideální pro práci s kolekcí služby.</span><span class="sxs-lookup"><span data-stu-id="99e37-116">However, applications built on Azure are a collection of interconnected services, and this tree structure isn't ideal for working with collections of services.</span></span> 

<span data-ttu-id="99e37-117">Hello portál Azure umožňuje snadno toobuild aplikace na kompletní s komponentami z více služeb.</span><span class="sxs-lookup"><span data-stu-id="99e37-117">hello Azure portal makes it easy toobuild applications end-to-end with components from multiple services.</span></span> <span data-ttu-id="99e37-118">portál Hello jsou uspořádána jako *cesty*.</span><span class="sxs-lookup"><span data-stu-id="99e37-118">hello portal is organized as *journeys*.</span></span> <span data-ttu-id="99e37-119">A *cesty* je řada *okna*, které jsou kontejnery pro hello různé součásti.</span><span class="sxs-lookup"><span data-stu-id="99e37-119">A *journey* is a series of *blades*, which are containers for hello different components.</span></span> <span data-ttu-id="99e37-120">Například nastavení automatického škálování pro webovou aplikaci *cesty* kterého přejdete několika oknech jak ukazuje následující příklad hello: hello **webu** okno (aby nadpis okna ještě nebylo aktualizované toouse Hello nové terminologie), hello **nastavení** okno a hello **škálovat** okno.</span><span class="sxs-lookup"><span data-stu-id="99e37-120">For example, setting up auto-scaling for a web app is a *journey* which takes you several blades as shown in hello following example: hello **web-site** blade (that blade title has not yet been updated toouse hello new terminology), hello **Settings** blade, and hello **Scale out** blade.</span></span> <span data-ttu-id="99e37-121">V příkladu hello automatické škálování se nastavuje toodepend podle využití procesoru, takže je zde také **procento využití procesoru** okno.</span><span class="sxs-lookup"><span data-stu-id="99e37-121">In hello example, auto scaling is being set up toodepend on CPU usage, so there is also a **CPU Percentage** blade.</span></span> <span data-ttu-id="99e37-122">Hello součásti v rámci hello *okna* se nazývají *částí*, které vypadají dlaždice.</span><span class="sxs-lookup"><span data-stu-id="99e37-122">hello components within hello *blades* are called *parts*, which look like tiles.</span></span> 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a><span data-ttu-id="99e37-123">Navigace příklad: vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="99e37-123">Navigation example: create a web app</span></span>
<span data-ttu-id="99e37-124">Vytvoření nové webové aplikace je stále stejně snadná jako 1-2-3.</span><span class="sxs-lookup"><span data-stu-id="99e37-124">Creating new web apps is still as easy as 1-2-3.</span></span> <span data-ttu-id="99e37-125">Následující obrázek ukazuje hello classic portál a hello portálu vedle sebe toodemonstrate není mnohem změněných v hello počet kroků Hello potřeba tooget webové aplikace nahoru a spuštěna.</span><span class="sxs-lookup"><span data-stu-id="99e37-125">hello following image shows hello classic portal and hello portal side-by-side toodemonstrate that not much has changed in hello number of steps needed tooget a web app up and running.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

<span data-ttu-id="99e37-126">Hello portálu můžete z nejběžnějších typů webových aplikací, včetně aplikací oblíbených galerie, např. WordPress hello.</span><span class="sxs-lookup"><span data-stu-id="99e37-126">In hello portal you can choose from hello most common types of web apps, including popular gallery applications like WordPress.</span></span> <span data-ttu-id="99e37-127">Úplný seznam dostupných aplikací, najdete v článku hello [Azure Marketplace].</span><span class="sxs-lookup"><span data-stu-id="99e37-127">For a full list of available applications, visit hello [Azure Marketplace].</span></span>

<span data-ttu-id="99e37-128">Když vytvoříte webovou aplikaci, je třeba zadat adresu URL, plán služby App Service a umístění portálu hello stejně jako se provádí v portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="99e37-128">When you create a web app, you specify URL, App Service plan, and location in hello portal just as you do in hello classic portal.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

<span data-ttu-id="99e37-129">Kromě toho hello portál umožňuje definovat další obecná nastavení.</span><span class="sxs-lookup"><span data-stu-id="99e37-129">In addition, hello portal lets you define other common settings.</span></span> <span data-ttu-id="99e37-130">Například [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) nastavit jej jako jednoduchý toosee a správu souvisejících prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="99e37-130">For example, [resource groups](../azure-resource-manager/resource-group-overview.md) make it simple toosee and manage related Azure resources.</span></span> 

## <a name="navigation-example-settings-and-features"></a><span data-ttu-id="99e37-131">Příklad navigace: nastavení a funkcí</span><span class="sxs-lookup"><span data-stu-id="99e37-131">Navigation example: settings and features</span></span>
<span data-ttu-id="99e37-132">Všechny hello nastavení a funkce jsou nyní logicky seskupeny v jednom okně, ze kterého můžete přejít.</span><span class="sxs-lookup"><span data-stu-id="99e37-132">All hello settings and features are now logically grouped in a single blade, from which you can navigate.</span></span>

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

<span data-ttu-id="99e37-133">Například můžete vytvořit vlastní domény kliknutím **vlastní domény a SSL** v hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="99e37-133">For example, you can create custom domains by clicking **Custom domains and SSL** in hello **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

<span data-ttu-id="99e37-134">tooset si upozornění, klikněte na tlačítko **požadavky a chyby** a potom **přidat výstraha**.</span><span class="sxs-lookup"><span data-stu-id="99e37-134">tooset up a monitoring alert, click **Requests and errors** and then **Add Alert**.</span></span>

![](./media/app-service-web-app-azure-portal/Monitoring.png)

<span data-ttu-id="99e37-135">Klikněte na tlačítko tooenable diagnostiky, **protokolů diagnostiky** v hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="99e37-135">tooenable diagnostics, click **Diagnostics logs** in hello **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

<span data-ttu-id="99e37-136">Klikněte na tlačítko Nastavení aplikace tooconfigure, **nastavení aplikace** v hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="99e37-136">tooconfigure application settings, click **Application settings** in hello **Settings** blade.</span></span> 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

<span data-ttu-id="99e37-137">Jiný než název značky hello, bylo přejmenováno pár věcí hello portálu nebo jinak seskupené toomake je snazší toofind je.</span><span class="sxs-lookup"><span data-stu-id="99e37-137">Other than hello brand name, a few things in hello portal have been renamed or grouped differently toomake it easier toofind them.</span></span> <span data-ttu-id="99e37-138">Například dál je snímek hello odpovídající stránce pro nastavení aplikace (**konfigurace**) na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="99e37-138">For example, below is a screenshot of hello corresponding page for app settings (**Configure**) in hello classic portal.</span></span>

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a><span data-ttu-id="99e37-139">Další materiály</span><span class="sxs-lookup"><span data-stu-id="99e37-139">More Resources</span></span>
[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

> [!NOTE]
> <span data-ttu-id="99e37-141">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="99e37-141">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="99e37-142">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="99e37-142">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="99e37-143">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="99e37-143">What's changed</span></span>
* <span data-ttu-id="99e37-144">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="99e37-144">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

