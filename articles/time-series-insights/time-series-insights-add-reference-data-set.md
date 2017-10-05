---
title: "Přidání referenční sady dat do prostředí Azure Time Series Insights | Dokumentace Microsoftu"
description: "V tomto kurzu přidáte referenční sadu dat k prostředí Time Series Insights."
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.openlocfilehash: 23444297b5231e6a026bcd9ce3ee9f943bf05867
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-ibiza-portal"></a><span data-ttu-id="67e34-103">Vytvoření referenční sady dat pro prostředí Time Series Insights pomocí portálu Ibiza</span><span class="sxs-lookup"><span data-stu-id="67e34-103">Create a reference data set for your Time Series Insights environment using the Ibiza portal</span></span>

<span data-ttu-id="67e34-104">Referenční sada dat je kolekce položek rozšířená o události ze zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="67e34-104">A Reference Data Set is a collection of items that are augmented with the events from your event source.</span></span> <span data-ttu-id="67e34-105">Modul příchozího přenosu dat Time Series Insights se připojí k události ze zdroje událostí s položkou v referenční sadě dat.</span><span class="sxs-lookup"><span data-stu-id="67e34-105">Time Series Insights ingress engine joins an event from your event source with an item in your reference data set.</span></span> <span data-ttu-id="67e34-106">Tato rozšířená událost je pak k dispozici pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="67e34-106">This augmented event is then available for query.</span></span> <span data-ttu-id="67e34-107">Toto připojení je založené na klíčích definovaných v referenční sadě dat.</span><span class="sxs-lookup"><span data-stu-id="67e34-107">This join is based on the keys defined in your reference data set.</span></span>

## <a name="steps-to-add-a-reference-data-set-to-your-environment"></a><span data-ttu-id="67e34-108">Postup přidání referenční sady dat do prostředí</span><span class="sxs-lookup"><span data-stu-id="67e34-108">Steps to add a reference data set to your environment</span></span>

1. <span data-ttu-id="67e34-109">Přihlaste se k [portálu Ibiza](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="67e34-109">Sign in to the [Ibiza portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="67e34-110">V nabídce na levé straně portálu Ibiza klikněte na Všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="67e34-110">Click “All resources” in the menu on the left side of the Ibiza portal.</span></span>
3. <span data-ttu-id="67e34-111">Vyberte vaše prostředí Time Series Insights.</span><span class="sxs-lookup"><span data-stu-id="67e34-111">Select your Time Series Insights environment.</span></span>

    ![Vytvoření referenční sady dat Time Series Insights](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. <span data-ttu-id="67e34-113">Vyberte možnost Referenční sady dat a klikněte na Přidat.</span><span class="sxs-lookup"><span data-stu-id="67e34-113">Select “Reference Data Sets”, click “+ Add.”</span></span>

    ![Vytvoření referenční sady dat Time Series Insights – podrobnosti](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. <span data-ttu-id="67e34-115">Zadejte název referenční sady dat.</span><span class="sxs-lookup"><span data-stu-id="67e34-115">Specify the name of the reference data set.</span></span>
6. <span data-ttu-id="67e34-116">Zadejte název klíče a jeho typ.</span><span class="sxs-lookup"><span data-stu-id="67e34-116">Specify the key name and its type.</span></span> <span data-ttu-id="67e34-117">Tento název a typ se použijí k výběr správné vlastnosti z události ve zdroji událostí.</span><span class="sxs-lookup"><span data-stu-id="67e34-117">This name and type is used to pick the correct property from the event in your event source.</span></span> <span data-ttu-id="67e34-118">Pokud například zadáte „DeviceId“ jako název klíče a „String“ jako typ, pak bude modul příchozího přenosu dat Time Series Insights v příchozí události vyhledávat vlastnost s názvem DeviceId typu String.</span><span class="sxs-lookup"><span data-stu-id="67e34-118">For instance, if you provide key name as “DeviceId” and type as “String”, then the Time Series Insights ingress engine looks for a property with the name “DeviceId” of type “String” in the incoming event.</span></span> <span data-ttu-id="67e34-119">K události je možné připojit více než jeden klíč.</span><span class="sxs-lookup"><span data-stu-id="67e34-119">You can provide more than one key to join with the event.</span></span> <span data-ttu-id="67e34-120">Při hledání shodného názvu vlastnosti se rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="67e34-120">The property name match is case-sensitive.</span></span>

     ![Vytvoření referenční sady dat Time Series Insights – podrobnosti](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. <span data-ttu-id="67e34-122">Klikněte na Vytvořit.</span><span class="sxs-lookup"><span data-stu-id="67e34-122">Click “Create.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="67e34-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67e34-123">Next steps</span></span>

* <span data-ttu-id="67e34-124">[Spravujte referenční data](time-series-insights-manage-reference-data-csharp.md) prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="67e34-124">[Manage reference data](time-series-insights-manage-reference-data-csharp.md) programmatically.</span></span>
* <span data-ttu-id="67e34-125">Úplnou referenční dokumentaci k rozhraní API najdete v dokumentu [Rozhraní API referenčních dat](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).</span><span class="sxs-lookup"><span data-stu-id="67e34-125">For the complete API reference, see [Reference Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) document.</span></span>