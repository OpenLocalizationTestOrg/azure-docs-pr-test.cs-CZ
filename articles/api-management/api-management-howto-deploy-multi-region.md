---
title: aaaDeploy Azure API Management services toomultiple Azure oblasti | Microsoft Docs
description: "Zjistěte, jak toodeploy službě Azure API Management služby toomultiple instanci Azure oblasti."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 47389ad6-f865-4706-833f-846115e22e4d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 04a3e762261237d73a769320a21363f99f1d20cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-an-azure-api-management-service-instance-toomultiple-azure-regions"></a><span data-ttu-id="7cf5f-103">Jak toodeploy službě Azure API Management služby toomultiple instanci Azure oblastí</span><span class="sxs-lookup"><span data-stu-id="7cf5f-103">How toodeploy an Azure API Management service instance toomultiple Azure regions</span></span>
<span data-ttu-id="7cf5f-104">API Management podporuje nasazení s více oblast, což umožňuje rozhraní API vydavatelů toodistribute jeden služby API management napříč jakékoli číslo požadované oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-104">API Management supports multi-region deployment which enables API publishers toodistribute a single API management service across any number of desired Azure regions.</span></span> <span data-ttu-id="7cf5f-105">To pomáhá zkrátit žádosti o latence, jak jej distribuovat geograficky spotřebitelé rozhraní API a také zvyšuje dostupnost služby, pokud jedna oblast přejde do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-105">This helps reduce request latency perceived by geographically distributed API consumers and also improves service availability if one region goes offline.</span></span> 

<span data-ttu-id="7cf5f-106">Když služby API Management je původně vytvořen, obsahuje pouze jeden [jednotky] [ unit] a se nachází v jedné oblasti Azure, který je určený jako primární oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-106">When an API Management service is created initially, it contains only one [unit][unit] and resides in a single Azure region, which is designated as hello Primary Region.</span></span> <span data-ttu-id="7cf5f-107">Pomocí hello portálu Azure můžete snadno přidat další oblasti.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-107">Additional regions can be easily added through hello Azure Portal.</span></span> <span data-ttu-id="7cf5f-108">API Management server brány je nasazené tooeach oblasti a provoz volání bude směrované toohello nejbližší brány.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-108">An API Management gateway server is deployed tooeach region and call traffic will be routed toohello closest gateway.</span></span> <span data-ttu-id="7cf5f-109">V oblasti přejde do režimu offline, hello je-li automaticky znovu směrovanou toohello další nejbližší brány.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-109">If a region goes offline, hello traffic is automatically re-directed toohello next closest gateway.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="7cf5f-110">Nasazení s více oblasti je dostupná v hello jenom  **[Premium] [ Premium]**  vrstvy.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-110">Multi-region deployment is only available in hello **[Premium][Premium]** tier.</span></span>
> 
> 

## <span data-ttu-id="7cf5f-111"><a name="add-region"></a>Nasazení rozhraní API správy služby instance tooa novou oblast</span><span class="sxs-lookup"><span data-stu-id="7cf5f-111"><a name="add-region"> </a>Deploy an API Management service instance tooa new region</span></span>
> [!NOTE]
> <span data-ttu-id="7cf5f-112">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="7cf5f-113">V hello portálu Azure přejděte toohello **škálování a ceny** stránky pro instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-113">In hello Azure Portal navigate toohello **Scale and pricing** page for your API Management service instance.</span></span> 

![Karta škálování][api-management-scale-service]

<span data-ttu-id="7cf5f-115">toodeploy tooa novou oblast, klikněte na **+ přidat oblast** hello panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-115">toodeploy tooa new region, click on **+ Add region** from hello toolbar.</span></span>

![Přidat oblast][api-management-add-region]

<span data-ttu-id="7cf5f-117">Vyberte umístění hello hello rozevíracího seznamu a nastavte hello počet jednotek pro s hello posuvníku.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-117">Select hello location from hello drop-down list and set hello number of units for with hello slider.</span></span>

![Zadejte jednotky][api-management-select-location-units]

<span data-ttu-id="7cf5f-119">Klikněte na tlačítko **přidat** tooplace výběr v tabulce umístění hello.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-119">Click **Add** tooplace your selection in hello Locations table.</span></span> 

<span data-ttu-id="7cf5f-120">Tento postup opakujte, dokud máte nakonfigurované všechny umístění a klikněte na tlačítko **Uložit** z procesu nasazení hello nástrojů toostart hello.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-120">Repeat this process until you have all locations configured and click **Save** from hello toolbar toostart hello deployment process.</span></span>

## <span data-ttu-id="7cf5f-121"><a name="remove-region"></a>Odstranit z umístění instanci služby API Management</span><span class="sxs-lookup"><span data-stu-id="7cf5f-121"><a name="remove-region"> </a>Delete an API Management service instance from a location</span></span>
<span data-ttu-id="7cf5f-122">V hello portálu Azure přejděte toohello **škálování a ceny** stránky pro instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-122">In hello Azure Portal navigate toohello **Scale and pricing** page for your API Management service instance.</span></span> 

![Karta škálování][api-management-scale-service]

<span data-ttu-id="7cf5f-124">Pro umístění hello chcete tooremove otevřete nabídku kontextu hello pomocí hello **...**  tlačítko na pravém konci hello hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-124">For hello location you would like tooremove open hello context menu using hello **...** button at hello right end of hello table.</span></span> <span data-ttu-id="7cf5f-125">Vyberte hello **odstranit** možnost.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-125">Select hello **Delete** option.</span></span>

![Odebrat oblast][api-management-remove-region]

<span data-ttu-id="7cf5f-127">Potvrdit odstranění hello a klikněte na tlačítko **Uložit** tooapply hello změny.</span><span class="sxs-lookup"><span data-stu-id="7cf5f-127">Confirm hello deletion and click **Save** tooapply hello changes.</span></span>

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance tooa new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

