---
title: "aaaAdd referenční datové sady tooyour Azure časové řady Přehled prostředí | Microsoft Docs"
description: "V tomto kurzu přidáte odkaz na datovou sadu tooyour časové řady Přehled prostředí"
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
ms.openlocfilehash: 05e626ed81a22f2a8710b23a931ccd17c0f38ca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a><span data-ttu-id="20154-103">Vytvořit odkaz na sadu dat pro vaše prostředí časové řady statistika pomocí portálu Ibiza hello</span><span class="sxs-lookup"><span data-stu-id="20154-103">Create a reference data set for your Time Series Insights environment using hello Ibiza portal</span></span>

<span data-ttu-id="20154-104">Referenční datové sady je kolekce položek, které jsou rozšířen s událostmi hello ze zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="20154-104">A Reference Data Set is a collection of items that are augmented with hello events from your event source.</span></span> <span data-ttu-id="20154-105">Modul příchozího přenosu dat Time Series Insights se připojí k události ze zdroje událostí s položkou v referenční sadě dat.</span><span class="sxs-lookup"><span data-stu-id="20154-105">Time Series Insights ingress engine joins an event from your event source with an item in your reference data set.</span></span> <span data-ttu-id="20154-106">Tato rozšířená událost je pak k dispozici pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="20154-106">This augmented event is then available for query.</span></span> <span data-ttu-id="20154-107">Toto připojení je založena na definované v odkaz na sadu dat hello klíče.</span><span class="sxs-lookup"><span data-stu-id="20154-107">This join is based on hello keys defined in your reference data set.</span></span>

## <a name="steps-tooadd-a-reference-data-set-tooyour-environment"></a><span data-ttu-id="20154-108">Kroky tooadd prostředí tooyour referenční datové sady</span><span class="sxs-lookup"><span data-stu-id="20154-108">Steps tooadd a reference data set tooyour environment</span></span>

1. <span data-ttu-id="20154-109">Přihlaste se toohello [portál Ibiza](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="20154-109">Sign in toohello [Ibiza portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="20154-110">V nabídce hello na levé straně hello portálu Ibiza hello klikněte na tlačítko "Všechny prostředky".</span><span class="sxs-lookup"><span data-stu-id="20154-110">Click “All resources” in hello menu on hello left side of hello Ibiza portal.</span></span>
3. <span data-ttu-id="20154-111">Vyberte vaše prostředí Time Series Insights.</span><span class="sxs-lookup"><span data-stu-id="20154-111">Select your Time Series Insights environment.</span></span>

    ![Vytvoření hello časové řady Statistika referenční datové sady](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. <span data-ttu-id="20154-113">Vyberte možnost Referenční sady dat a klikněte na Přidat.</span><span class="sxs-lookup"><span data-stu-id="20154-113">Select “Reference Data Sets”, click “+ Add.”</span></span>

    ![Vytvoření hello časové řady Statistika referenční datové sady – podrobnosti](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. <span data-ttu-id="20154-115">Zadejte název hello hello referenční datové sady.</span><span class="sxs-lookup"><span data-stu-id="20154-115">Specify hello name of hello reference data set.</span></span>
6. <span data-ttu-id="20154-116">Zadejte název klíče hello a jeho typu.</span><span class="sxs-lookup"><span data-stu-id="20154-116">Specify hello key name and its type.</span></span> <span data-ttu-id="20154-117">Tento název a typ je použité toopick hello správná vlastnost z hello události ve zdroji událostí.</span><span class="sxs-lookup"><span data-stu-id="20154-117">This name and type is used toopick hello correct property from hello event in your event source.</span></span> <span data-ttu-id="20154-118">Například pokud zadáte název klíče jako "DeviceId" a zadejte jako "Řetězec", pak hello čas řady Statistika příchozího modul hledat vlastnost s názvem hello "DeviceId" typu "Řetězec" v hello příchozí události.</span><span class="sxs-lookup"><span data-stu-id="20154-118">For instance, if you provide key name as “DeviceId” and type as “String”, then hello Time Series Insights ingress engine looks for a property with hello name “DeviceId” of type “String” in hello incoming event.</span></span> <span data-ttu-id="20154-119">Můžete zadat více než jeden klíč toojoin s hello událostí.</span><span class="sxs-lookup"><span data-stu-id="20154-119">You can provide more than one key toojoin with hello event.</span></span> <span data-ttu-id="20154-120">Hello vlastnost název shoda rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="20154-120">hello property name match is case-sensitive.</span></span>

     ![Vytvoření hello časové řady Statistika referenční datové sady – podrobnosti](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. <span data-ttu-id="20154-122">Klikněte na Vytvořit.</span><span class="sxs-lookup"><span data-stu-id="20154-122">Click “Create.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="20154-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20154-123">Next steps</span></span>

* <span data-ttu-id="20154-124">[Spravujte referenční data](time-series-insights-manage-reference-data-csharp.md) prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="20154-124">[Manage reference data](time-series-insights-manage-reference-data-csharp.md) programmatically.</span></span>
* <span data-ttu-id="20154-125">Hello úplný referenční dokumentace rozhraní API, najdete v části [dat referenční dokumentace rozhraní API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="20154-125">For hello complete API reference, see [Reference Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) document.</span></span>
