---
title: "Vytvoření prostředí Azure Time Series Insights | Dokumentace Microsoftu"
description: "V tomto kurzu se naučíte, jak vytvořit prostředí Time Series Insights, jak ho propojit se zdrojem událostí a připravit pro analýzu dat událostí během pár minut."
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
ms.openlocfilehash: eb710795916a2d7beea75a6408a0982fb4dc8750
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a><span data-ttu-id="1fb37-103">Vytvoření nového prostředí Time Series Insights na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1fb37-103">Create a new Time Series Insights environment in the Azure portal</span></span>

<span data-ttu-id="1fb37-104">Prostředí Time Series Insights je prostředek Azure s kapacitou úložiště a příchozího přenosu dat.</span><span class="sxs-lookup"><span data-stu-id="1fb37-104">Time Series Insights environment is an Azure resource with ingress and storage capacity.</span></span> <span data-ttu-id="1fb37-105">Zákazníci zřizují prostředí s požadovanou kapacitou přes Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1fb37-105">Customers provision environments via the Azure portal with the required capacity.</span></span>

## <a name="steps-to-create-the-environment"></a><span data-ttu-id="1fb37-106">Postup vytvoření prostředí</span><span class="sxs-lookup"><span data-stu-id="1fb37-106">Steps to create the environment</span></span>

<span data-ttu-id="1fb37-107">Prostředí vytvoříte podle těchto pokynů:</span><span class="sxs-lookup"><span data-stu-id="1fb37-107">Follow these steps to create your environment:</span></span>

1.  <span data-ttu-id="1fb37-108">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1fb37-108">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="1fb37-109">Klikněte na symbol plus (+) v levém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="1fb37-109">Click the plus sign (“+”) in the top left corner.</span></span>
3.  <span data-ttu-id="1fb37-110">Do vyhledávacího pole zadejte „Time Series Insights“.</span><span class="sxs-lookup"><span data-stu-id="1fb37-110">Search for “Time Series Insights” in the search box.</span></span>

  ![Vytvoření prostředí Time Series Insights](media/get-started/getstarted-create-environment1.png)

4.  <span data-ttu-id="1fb37-112">Vyberte Time Series Insights a klikněte na Vytvořit.</span><span class="sxs-lookup"><span data-stu-id="1fb37-112">Select “Time Series Insights”, click “Create”.</span></span>

  ![Vytvoření skupiny prostředků Time Series Insights](media/get-started/getstarted-create-environment2.png)

5.  <span data-ttu-id="1fb37-114">Zadejte název prostředí.</span><span class="sxs-lookup"><span data-stu-id="1fb37-114">Specify environment name.</span></span> <span data-ttu-id="1fb37-115">Tento název bude reprezentovat prostředí v [průzkumníku Time Series Insights](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1fb37-115">This name will represent the environment in [time series explorer](https://insights.timeseries.azure.com).</span></span>
6.  <span data-ttu-id="1fb37-116">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="1fb37-116">Select a subscription.</span></span> <span data-ttu-id="1fb37-117">Vyberte to, které obsahuje váš zdroj událostí.</span><span class="sxs-lookup"><span data-stu-id="1fb37-117">Choose one that contains your event source.</span></span> <span data-ttu-id="1fb37-118">Time Series Insights dokáže automaticky rozpoznat prostředky služby Azure IoT Hub a centra událostí, které existují v rámci stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="1fb37-118">Time Series Insights can auto-detect Azure IoT Hub and Event Hub resources existing in the same subscription.</span></span>
7.  <span data-ttu-id="1fb37-119">Vyberte nebo vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="1fb37-119">Select or create a resource group.</span></span> <span data-ttu-id="1fb37-120">Skupina prostředků je kolekce společně používaných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="1fb37-120">A resource group is a collection of Azure resources used together.</span></span>
8.  <span data-ttu-id="1fb37-121">Vyberte umístění pro hostování.</span><span class="sxs-lookup"><span data-stu-id="1fb37-121">Select a hosting location.</span></span> <span data-ttu-id="1fb37-122">Abyste se vyhnuli přenášení dat mezi datovými centry, zvolte umístění, které obsahuje váš zdroj událostí.</span><span class="sxs-lookup"><span data-stu-id="1fb37-122">To avoid moving data across data centers, choose location that contains your event source.</span></span>
9.  <span data-ttu-id="1fb37-123">Vyberte cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="1fb37-123">Select a pricing tier.</span></span>
10. <span data-ttu-id="1fb37-124">Vyberte kapacitu.</span><span class="sxs-lookup"><span data-stu-id="1fb37-124">Select capacity.</span></span> <span data-ttu-id="1fb37-125">Kapacitu prostředí můžete po vytvoření změnit.</span><span class="sxs-lookup"><span data-stu-id="1fb37-125">You can change capacity of an environment after creation.</span></span>
11. <span data-ttu-id="1fb37-126">Vytvořte prostředí.</span><span class="sxs-lookup"><span data-stu-id="1fb37-126">Create your environment.</span></span> <span data-ttu-id="1fb37-127">Můžete také připnout prostředí na řídicí panel pro usnadnění přístupu po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1fb37-127">You can also pin your environment to the dashboard for easy access whenever you sign in.</span></span>

  ![Vytvoření služby Time Series Insights – připnutí na řídicí panel](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a><span data-ttu-id="1fb37-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1fb37-129">Next steps</span></span>

* <span data-ttu-id="1fb37-130">[Definování zásad přístupu k datům](time-series-insights-data-access.md) pro přístup k prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="1fb37-130">[Define data access policies](time-series-insights-data-access.md) to access your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
* [<span data-ttu-id="1fb37-131">Vytvoření zdroje událostí</span><span class="sxs-lookup"><span data-stu-id="1fb37-131">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="1fb37-132">[Odesílání událostí](time-series-insights-send-events.md) do zdroje událostí</span><span class="sxs-lookup"><span data-stu-id="1fb37-132">[Send events](time-series-insights-send-events.md) to the event source</span></span>
