---
title: "Referenční dokumentace pro navigace na portálu Azure"
description: "Přečtěte si informace různých uživatelského prostředí pro webové aplikace služby mezi portálu pro správu a portálu Azure"
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
ms.openlocfilehash: d1ef6e87d82df0840e49412154df40cc937b320c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reference-for-navigating-the-azure-portal"></a><span data-ttu-id="98f24-103">Referenční dokumentace pro navigace na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="98f24-103">Reference for navigating the Azure portal</span></span>
<span data-ttu-id="98f24-104">Weby Azure se nyní nazývají [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="98f24-104">Azure Websites are now called [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="98f24-105">Aktualizujeme všechny naší dokumentaci, aby odrážela tuto změnu názvu a poskytují pokyny k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="98f24-105">We're updating all of our documentation to reflect this name change and to provide instructions for the Azure Portal.</span></span> <span data-ttu-id="98f24-106">Dokud nebude tento proces probíhá, můžete tento dokument použít jako vodítko pro práci s webovými aplikacemi na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="98f24-106">Until that process is done, you can use this document as a guide for working with Web Apps in the Azure portal.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="the-future-of-the-azure-classic-portal"></a><span data-ttu-id="98f24-107">Budoucí portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="98f24-107">The future of the Azure Classic Portal</span></span>
<span data-ttu-id="98f24-108">Když si všimnete změny brandingu na portálu Azure Classic, tohoto portálu právě nahrazují portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="98f24-108">While you will notice the branding changes on the Azure Classic Portal, that portal is in the process of being replaced by the Azure Portal.</span></span> <span data-ttu-id="98f24-109">Jak na portálu classic je vyřazován, je fokus pro nový vývoj přejdou k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="98f24-109">As the classic portal is being phased out, the focus for new development is shifting to the Azure Portal.</span></span> <span data-ttu-id="98f24-110">Vrátí všechny nadcházející nové funkce pro webové aplikace se na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="98f24-110">All upcoming new features for Web Apps will come in the Azure Portal.</span></span> <span data-ttu-id="98f24-111">Začít používat portál Azure využívat nejnovější a největší splnit webové aplikace na nabídku.</span><span class="sxs-lookup"><span data-stu-id="98f24-111">Start using the Azure Portal to take advantage of the latest and greatest that Web Apps have to offer.</span></span>

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a><span data-ttu-id="98f24-112">Rozložení rozdíly mezi portálu Azure Classic a portálu Azure</span><span class="sxs-lookup"><span data-stu-id="98f24-112">Layout differences between the Azure Classic Portal and Azure Portal</span></span>
<span data-ttu-id="98f24-113">Na portálu classic jsou uvedeny všech služeb Azure na levé straně.</span><span class="sxs-lookup"><span data-stu-id="98f24-113">In the classic portal, all the Azure services are listed on the left hand side.</span></span> <span data-ttu-id="98f24-114">Navigace na portálu classic řídí stromová struktura, kde můžete spustit ze služby a přejděte na každý element.</span><span class="sxs-lookup"><span data-stu-id="98f24-114">Navigation in the classic portal follows a tree structure, where you start from the service and navigate into each element.</span></span> <span data-ttu-id="98f24-115">Tato struktura dobře funguje při správě nezávislé komponenty.</span><span class="sxs-lookup"><span data-stu-id="98f24-115">This structure works well when managing independent components.</span></span> <span data-ttu-id="98f24-116">Aplikace založené na Azure se však kolekce vzájemně propojené služby, a tato struktura stromu není ideální pro práci s kolekcí služby.</span><span class="sxs-lookup"><span data-stu-id="98f24-116">However, applications built on Azure are a collection of interconnected services, and this tree structure isn't ideal for working with collections of services.</span></span> 

<span data-ttu-id="98f24-117">Portál Azure lze snadno vytvářet aplikace pro kompletní s komponentami z více služeb.</span><span class="sxs-lookup"><span data-stu-id="98f24-117">The Azure portal makes it easy to build applications end-to-end with components from multiple services.</span></span> <span data-ttu-id="98f24-118">Na portálu jsou uspořádána jako *cesty*.</span><span class="sxs-lookup"><span data-stu-id="98f24-118">The portal is organized as *journeys*.</span></span> <span data-ttu-id="98f24-119">A *cesty* je řada *okna*, které jsou kontejnery pro různé součásti.</span><span class="sxs-lookup"><span data-stu-id="98f24-119">A *journey* is a series of *blades*, which are containers for the different components.</span></span> <span data-ttu-id="98f24-120">Například nastavení automatického škálování pro webové aplikace je *cesty* kterého přejdete několika oknech jak je znázorněno v následujícím příkladu: **webu** (aby nadpis okna nebyla aktualizována použití nového okna terminologie), **nastavení** okně a **škálovat** okno.</span><span class="sxs-lookup"><span data-stu-id="98f24-120">For example, setting up auto-scaling for a web app is a *journey* which takes you several blades as shown in the following example: the **web-site** blade (that blade title has not yet been updated to use the new terminology), the **Settings** blade, and the **Scale out** blade.</span></span> <span data-ttu-id="98f24-121">V příkladu automatické škálování se nastavuje na závisí na využití procesoru, takže je zde také **procento využití procesoru** okno.</span><span class="sxs-lookup"><span data-stu-id="98f24-121">In the example, auto scaling is being set up to depend on CPU usage, so there is also a **CPU Percentage** blade.</span></span> <span data-ttu-id="98f24-122">Součásti v rámci *okna* se nazývají *částí*, který vypadat podobně jako dlaždice.</span><span class="sxs-lookup"><span data-stu-id="98f24-122">The components within the *blades* are called *parts*, which look like tiles.</span></span> 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a><span data-ttu-id="98f24-123">Navigace příklad: vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="98f24-123">Navigation example: create a web app</span></span>
<span data-ttu-id="98f24-124">Vytvoření nové webové aplikace je stále stejně snadná jako 1-2-3.</span><span class="sxs-lookup"><span data-stu-id="98f24-124">Creating new web apps is still as easy as 1-2-3.</span></span> <span data-ttu-id="98f24-125">Následující obrázek ukazuje na klasickém portálu a portálu – souběžného prokázat, že není mnohem došlo ke změně počtu kroky potřebné ke zprovoznění webové aplikace a systémem.</span><span class="sxs-lookup"><span data-stu-id="98f24-125">The following image shows the classic portal and the portal side-by-side to demonstrate that not much has changed in the number of steps needed to get a web app up and running.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

<span data-ttu-id="98f24-126">Na portálu můžete z nejběžnějších typů webových aplikací, včetně aplikací oblíbených galerie, např. WordPress.</span><span class="sxs-lookup"><span data-stu-id="98f24-126">In the portal you can choose from the most common types of web apps, including popular gallery applications like WordPress.</span></span> <span data-ttu-id="98f24-127">Úplný seznam dostupných aplikací, najdete v článku [Azure Marketplace].</span><span class="sxs-lookup"><span data-stu-id="98f24-127">For a full list of available applications, visit the [Azure Marketplace].</span></span>

<span data-ttu-id="98f24-128">Když vytvoříte webovou aplikaci, je třeba zadat adresu URL, plán služby App Service a umístění na portálu, stejně jako se provádí v portálu classic.</span><span class="sxs-lookup"><span data-stu-id="98f24-128">When you create a web app, you specify URL, App Service plan, and location in the portal just as you do in the classic portal.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

<span data-ttu-id="98f24-129">Kromě toho portálu umožňuje definovat další obecná nastavení.</span><span class="sxs-lookup"><span data-stu-id="98f24-129">In addition, the portal lets you define other common settings.</span></span> <span data-ttu-id="98f24-130">Například [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) usnadňují prohlížení a správu souvisejících prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="98f24-130">For example, [resource groups](../azure-resource-manager/resource-group-overview.md) make it simple to see and manage related Azure resources.</span></span> 

## <a name="navigation-example-settings-and-features"></a><span data-ttu-id="98f24-131">Příklad navigace: nastavení a funkcí</span><span class="sxs-lookup"><span data-stu-id="98f24-131">Navigation example: settings and features</span></span>
<span data-ttu-id="98f24-132">Nastavení a funkcí jsou nyní logicky seskupeny v jednom okně, ze kterého můžete přejít.</span><span class="sxs-lookup"><span data-stu-id="98f24-132">All the settings and features are now logically grouped in a single blade, from which you can navigate.</span></span>

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

<span data-ttu-id="98f24-133">Například můžete vytvořit vlastní domény kliknutím **vlastní domény a SSL** v **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="98f24-133">For example, you can create custom domains by clicking **Custom domains and SSL** in the **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

<span data-ttu-id="98f24-134">Chcete-li nastavit upozornění na monitorování, klikněte na tlačítko **požadavky a chyby** a potom **přidat výstraha**.</span><span class="sxs-lookup"><span data-stu-id="98f24-134">To set up a monitoring alert, click **Requests and errors** and then **Add Alert**.</span></span>

![](./media/app-service-web-app-azure-portal/Monitoring.png)

<span data-ttu-id="98f24-135">Chcete-li povolit diagnostiky, klikněte na tlačítko **protokolů diagnostiky** v **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="98f24-135">To enable diagnostics, click **Diagnostics logs** in the **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

<span data-ttu-id="98f24-136">Chcete-li nakonfigurovat nastavení aplikace, klikněte na tlačítko **nastavení aplikace** v **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="98f24-136">To configure application settings, click **Application settings** in the **Settings** blade.</span></span> 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

<span data-ttu-id="98f24-137">Jiný než název značky mít bylo přejmenováno nebo jinak seskupené, aby bylo snazší najít je pár věcí na portálu.</span><span class="sxs-lookup"><span data-stu-id="98f24-137">Other than the brand name, a few things in the portal have been renamed or grouped differently to make it easier to find them.</span></span> <span data-ttu-id="98f24-138">Například dál je snímek odpovídající stránce pro nastavení aplikace (**konfigurace**) na portálu classic.</span><span class="sxs-lookup"><span data-stu-id="98f24-138">For example, below is a screenshot of the corresponding page for app settings (**Configure**) in the classic portal.</span></span>

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a><span data-ttu-id="98f24-139">Další materiály</span><span class="sxs-lookup"><span data-stu-id="98f24-139">More Resources</span></span>
[Azure Portal]: https://portal.azure.com
<span data-ttu-id="98f24-140">[Azure Marketplace]: /marketplace/</span><span class="sxs-lookup"><span data-stu-id="98f24-140">[Azure Marketplace]: /marketplace/</span></span>

> [!NOTE]
> <span data-ttu-id="98f24-141">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="98f24-141">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="98f24-142">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="98f24-142">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="98f24-143">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="98f24-143">What's changed</span></span>
* <span data-ttu-id="98f24-144">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="98f24-144">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

