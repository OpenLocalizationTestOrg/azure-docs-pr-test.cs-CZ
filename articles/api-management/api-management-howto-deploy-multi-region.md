---
title: "Nasazení služby Azure API Management do několika oblastmi Azure | Microsoft Docs"
description: "Zjistěte, jak nasadit instanci služby Azure API Management na několika oblastmi Azure."
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
ms.openlocfilehash: 1c39fee739c2f5fd4b928e1e76e1ea57f072b5f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a><span data-ttu-id="314c4-103">Postup nasazení instanci služby Azure API Management na několika oblastmi Azure</span><span class="sxs-lookup"><span data-stu-id="314c4-103">How to deploy an Azure API Management service instance to multiple Azure regions</span></span>
<span data-ttu-id="314c4-104">API Management podporuje nasazení s více oblast, což umožňuje rozhraní API vydavatelů distribuci jedné služby pro správu rozhraní API přes libovolný počet požadovaných oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="314c4-104">API Management supports multi-region deployment which enables API publishers to distribute a single API management service across any number of desired Azure regions.</span></span> <span data-ttu-id="314c4-105">To pomáhá zkrátit žádosti o latence, jak jej distribuovat geograficky spotřebitelé rozhraní API a také zvyšuje dostupnost služby, pokud jedna oblast přejde do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="314c4-105">This helps reduce request latency perceived by geographically distributed API consumers and also improves service availability if one region goes offline.</span></span> 

<span data-ttu-id="314c4-106">Když služby API Management je původně vytvořen, obsahuje pouze jeden [jednotky] [ unit] a se nachází v jedné oblasti Azure, který je určený jako primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="314c4-106">When an API Management service is created initially, it contains only one [unit][unit] and resides in a single Azure region, which is designated as the Primary Region.</span></span> <span data-ttu-id="314c4-107">Prostřednictvím portálu Azure lze snadno přidat další oblasti.</span><span class="sxs-lookup"><span data-stu-id="314c4-107">Additional regions can be easily added through the Azure Portal.</span></span> <span data-ttu-id="314c4-108">Nasazení serveru služby Brána API Management na každou oblast a volání provoz, budou směrovány na nejbližší bránu.</span><span class="sxs-lookup"><span data-stu-id="314c4-108">An API Management gateway server is deployed to each region and call traffic will be routed to the closest gateway.</span></span> <span data-ttu-id="314c4-109">Pokud oblast přejde do režimu offline, je automaticky znovu směrovanou k bráně další nejbližší provoz.</span><span class="sxs-lookup"><span data-stu-id="314c4-109">If a region goes offline, the traffic is automatically re-directed to the next closest gateway.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="314c4-110">Nasazení s více oblasti je dostupná v jenom  **[Premium] [ Premium]**  vrstvy.</span><span class="sxs-lookup"><span data-stu-id="314c4-110">Multi-region deployment is only available in the **[Premium][Premium]** tier.</span></span>
> 
> 

## <span data-ttu-id="314c4-111"><a name="add-region"></a>Nasadit do nové oblasti instanci služby API Management</span><span class="sxs-lookup"><span data-stu-id="314c4-111"><a name="add-region"> </a>Deploy an API Management service instance to a new region</span></span>
> [!NOTE]
> <span data-ttu-id="314c4-112">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="314c4-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="314c4-113">Na portálu Azure přejděte do **škálování a ceny** stránky pro instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="314c4-113">In the Azure Portal navigate to the **Scale and pricing** page for your API Management service instance.</span></span> 

![Karta škálování][api-management-scale-service]

<span data-ttu-id="314c4-115">Chcete-li nasadit do nové oblasti, klikněte na **+ přidat oblast** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="314c4-115">To deploy to a new region, click on **+ Add region** from the toolbar.</span></span>

![Přidat oblast][api-management-add-region]

<span data-ttu-id="314c4-117">V rozevíracím seznamu vyberte umístění a nastavit počet jednotek pro pomocí posuvníku.</span><span class="sxs-lookup"><span data-stu-id="314c4-117">Select the location from the drop-down list and set the number of units for with the slider.</span></span>

![Zadejte jednotky][api-management-select-location-units]

<span data-ttu-id="314c4-119">Klikněte na tlačítko **přidat** umístit výběr v tabulce umístění.</span><span class="sxs-lookup"><span data-stu-id="314c4-119">Click **Add** to place your selection in the Locations table.</span></span> 

<span data-ttu-id="314c4-120">Tento postup opakujte, dokud máte nakonfigurované všechny umístění a klikněte na tlačítko **Uložit** na panelu nástrojů k zahájení procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="314c4-120">Repeat this process until you have all locations configured and click **Save** from the toolbar to start the deployment process.</span></span>

## <span data-ttu-id="314c4-121"><a name="remove-region"></a>Odstranit z umístění instanci služby API Management</span><span class="sxs-lookup"><span data-stu-id="314c4-121"><a name="remove-region"> </a>Delete an API Management service instance from a location</span></span>
<span data-ttu-id="314c4-122">Na portálu Azure přejděte do **škálování a ceny** stránky pro instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="314c4-122">In the Azure Portal navigate to the **Scale and pricing** page for your API Management service instance.</span></span> 

![Karta škálování][api-management-scale-service]

<span data-ttu-id="314c4-124">Pro umístění, kterou chcete odebrat otevřete kontextu nabídku pomocí **...**  tlačítko na pravém konci v tabulce.</span><span class="sxs-lookup"><span data-stu-id="314c4-124">For the location you would like to remove open the context menu using the **...** button at the right end of the table.</span></span> <span data-ttu-id="314c4-125">Vyberte **odstranit** možnost.</span><span class="sxs-lookup"><span data-stu-id="314c4-125">Select the **Delete** option.</span></span>

![Odebrat oblast][api-management-remove-region]

<span data-ttu-id="314c4-127">Potvrzení odstranění a klikněte na tlačítko **Uložit** aby se změny projevily.</span><span class="sxs-lookup"><span data-stu-id="314c4-127">Confirm the deletion and click **Save** to apply the changes.</span></span>

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

