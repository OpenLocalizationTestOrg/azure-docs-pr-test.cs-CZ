---
title: "aaaAdd prostředí Statistika řady čas Azure tooyour zdroje událostí | Microsoft Docs"
description: "V tomto kurzu připojíte prostředí časové řady Statistika tooyour zdroje událostí"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: 817df5e81cb4dc3d7376914a4651aabebadbcc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a><span data-ttu-id="9b427-103">Vytvoření zdroje událostí pro vaše prostředí časové řady statistika pomocí portálu Ibiza hello</span><span class="sxs-lookup"><span data-stu-id="9b427-103">Create an event source for your Time Series Insights environment using hello Ibiza portal</span></span>

<span data-ttu-id="9b427-104">Zdroj událostí Time Series Insights je odvozený od zprostředkovatele událostí, jako je například služba Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="9b427-104">Time Series Insights Event Source is derived from an event broker, like Azure Event Hubs.</span></span> <span data-ttu-id="9b427-105">Časové řady Insights se připojuje přímo tooEvent zdrojů, příjem hello datový proud bez nutnosti uživatelé toowrite jeden řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="9b427-105">Time Series Insights connects directly tooEvent Sources, ingesting hello data stream without requiring users toowrite a single line of code.</span></span> <span data-ttu-id="9b427-106">V současné době Time Series Insights podporuje Azure Event Hubs a Azure IoT Hubs.</span><span class="sxs-lookup"><span data-stu-id="9b427-106">Currently, Time Series Insights supports Azure Event Hubs and Azure IoT Hubs.</span></span> <span data-ttu-id="9b427-107">V budoucích hello se přidají další zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="9b427-107">In hello future, more Event Sources will be added.</span></span>

## <a name="steps-tooadd-an-event-source-tooyour-environment"></a><span data-ttu-id="9b427-108">Kroky tooadd prostředí tooyour zdroje událostí</span><span class="sxs-lookup"><span data-stu-id="9b427-108">Steps tooadd an event source tooyour environment</span></span>

1.  <span data-ttu-id="9b427-109">Přihlaste se toohello [portál Ibiza](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9b427-109">Sign in toohello [Ibiza portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="9b427-110">V nabídce hello na levé straně hello portálu Ibiza hello klikněte na tlačítko "Všechny prostředky".</span><span class="sxs-lookup"><span data-stu-id="9b427-110">Click “All resources” in hello menu on hello left side of hello Ibiza portal.</span></span>
3.  <span data-ttu-id="9b427-111">Vyberte vaše prostředí Time Series Insights.</span><span class="sxs-lookup"><span data-stu-id="9b427-111">Select your Time Series Insights environment.</span></span>

  ![Vytvořit zdroj události časové řady Statistika hello](media/add-event-source/getstarted-create-event-source-1.png)

4.  <span data-ttu-id="9b427-113">Vyberte „Zdroje událostí“ a klikněte na „+ Přidat“.</span><span class="sxs-lookup"><span data-stu-id="9b427-113">Select “Event Sources”, click “+ Add.”</span></span>

  ![Vytvořit zdroj události hello časové řady Statistika – podrobnosti](media/add-event-source/getstarted-create-event-source-2.png)

5.  <span data-ttu-id="9b427-115">Zadejte název hello hello zdroje událostí.</span><span class="sxs-lookup"><span data-stu-id="9b427-115">Specify hello name of hello event source.</span></span> <span data-ttu-id="9b427-116">Tento název je přidružený ke všem událostem, které přichází z tohoto zdroje událostí, a je dostupný v době zpracování dotazu.</span><span class="sxs-lookup"><span data-stu-id="9b427-116">This name is associated with all events coming from this event source and is available at query time.</span></span>
6.  <span data-ttu-id="9b427-117">Centra událostí vyberte ze seznamu hello centra událostí prostředků v aktuálním předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="9b427-117">Select an event hub from hello list of Event Hub resources in hello current subscription.</span></span> <span data-ttu-id="9b427-118">Jinak vyberte možnost import "nastavení centra událostí zadejte ručně" toospecify centra událostí v jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="9b427-118">Otherwise choose import option "Provide Event Hub settings manually” toospecify an event hub in another subscription.</span></span> <span data-ttu-id="9b427-119">Události musí být publikovány ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="9b427-119">Events must be published in JSON format.</span></span>
7.  <span data-ttu-id="9b427-120">Vyberte zásadu, která má v Centru událostí hello oprávnění ke čtení.</span><span class="sxs-lookup"><span data-stu-id="9b427-120">Select policy that has read permission in hello event hub.</span></span>
8.  <span data-ttu-id="9b427-121">Zadejte skupinu příjemců centra událostí.</span><span class="sxs-lookup"><span data-stu-id="9b427-121">Specify event hub consumer group.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="9b427-122">Zajistěte, aby tuto skupinu příjemců nepoužívala žádná jiná služba (například úloha služby Stream Analytics nebo jiné prostředí Time Series Insights).</span><span class="sxs-lookup"><span data-stu-id="9b427-122">Make sure this consumer group is not used by any other service (such as Stream Analytics job or another Time Series Insights environment).</span></span> <span data-ttu-id="9b427-123">Pokud je skupina uživatelů používají i jiné služby, přečtěte si, že operace je negativně ovlivňovat to pro toto prostředí a hello dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="9b427-123">If consumer group is used by other services, read operation is negatively affected for this environment and hello other services.</span></span> <span data-ttu-id="9b427-124">Pokud používáte "$Default" jako hello skupiny příjemců, je by mohlo vést toopotential opakované použití jiných čtečky.</span><span class="sxs-lookup"><span data-stu-id="9b427-124">If you are using “$Default” as hello consumer group, it could lead toopotential reuse by other readers.</span></span>

9.  <span data-ttu-id="9b427-125">Klikněte na Vytvořit.</span><span class="sxs-lookup"><span data-stu-id="9b427-125">Click “Create.”</span></span>

<span data-ttu-id="9b427-126">Po vytvoření zdroje události hello časové řady Insights automaticky spustí, streamování dat do vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="9b427-126">After creation of hello event source, Time Series Insights will automatically start streaming data into your environment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b427-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b427-127">Next steps</span></span>

* <span data-ttu-id="9b427-128">[Odesílání událostí](time-series-insights-send-events.md) toohello zdroj události</span><span class="sxs-lookup"><span data-stu-id="9b427-128">[Send events](time-series-insights-send-events.md) toohello event source</span></span>
* <span data-ttu-id="9b427-129">Zobrazení prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="9b427-129">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
