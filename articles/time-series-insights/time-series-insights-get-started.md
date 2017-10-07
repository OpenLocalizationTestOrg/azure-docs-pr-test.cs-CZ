---
title: "aaaCreate prostředí Azure časové řady Insights | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak toocreate časové řady prostředí, připojte ho zdroj události tooan a připravené tooanalyze vaše data události v minutách."
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
ms.openlocfilehash: 7120fc9a6e4d4a4972f8cb37e4d9945cfb746fd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-new-time-series-insights-environment-in-hello-azure-portal"></a><span data-ttu-id="8940b-103">Vytvoření nového prostředí časové řady statistiky v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8940b-103">Create a new Time Series Insights environment in hello Azure portal</span></span>

<span data-ttu-id="8940b-104">Prostředí Time Series Insights je prostředek Azure s kapacitou úložiště a příchozího přenosu dat.</span><span class="sxs-lookup"><span data-stu-id="8940b-104">Time Series Insights environment is an Azure resource with ingress and storage capacity.</span></span> <span data-ttu-id="8940b-105">Zákazníci zřídit s kapacitou hello požadované prostředí prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8940b-105">Customers provision environments via hello Azure portal with hello required capacity.</span></span>

## <a name="steps-toocreate-hello-environment"></a><span data-ttu-id="8940b-106">Kroky toocreate hello prostředí</span><span class="sxs-lookup"><span data-stu-id="8940b-106">Steps toocreate hello environment</span></span>

<span data-ttu-id="8940b-107">Postupujte podle těchto kroků toocreate prostředí:</span><span class="sxs-lookup"><span data-stu-id="8940b-107">Follow these steps toocreate your environment:</span></span>

1.  <span data-ttu-id="8940b-108">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8940b-108">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="8940b-109">V horní části hello levého horního rohu klikněte na tlačítko hello plus přihlašovací ("+").</span><span class="sxs-lookup"><span data-stu-id="8940b-109">Click hello plus sign (“+”) in hello top left corner.</span></span>
3.  <span data-ttu-id="8940b-110">Vyhledejte "Časové řady Insights" hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="8940b-110">Search for “Time Series Insights” in hello search box.</span></span>

  ![Vytvoření prostředí časové řady Statistika hello](media/get-started/getstarted-create-environment1.png)

4.  <span data-ttu-id="8940b-112">Vyberte Time Series Insights a klikněte na Vytvořit.</span><span class="sxs-lookup"><span data-stu-id="8940b-112">Select “Time Series Insights”, click “Create”.</span></span>

  ![Vytvořte skupinu prostředků časové řady Statistika hello](media/get-started/getstarted-create-environment2.png)

5.  <span data-ttu-id="8940b-114">Zadejte název prostředí.</span><span class="sxs-lookup"><span data-stu-id="8940b-114">Specify environment name.</span></span> <span data-ttu-id="8940b-115">Tento název bude reprezentovat hello prostředí v [časové řady explorer](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8940b-115">This name will represent hello environment in [time series explorer](https://insights.timeseries.azure.com).</span></span>
6.  <span data-ttu-id="8940b-116">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="8940b-116">Select a subscription.</span></span> <span data-ttu-id="8940b-117">Vyberte to, které obsahuje váš zdroj událostí.</span><span class="sxs-lookup"><span data-stu-id="8940b-117">Choose one that contains your event source.</span></span> <span data-ttu-id="8940b-118">Statistika časové řady může automaticky rozpoznat Azure IoT Hub a prostředky centra událostí existující v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="8940b-118">Time Series Insights can auto-detect Azure IoT Hub and Event Hub resources existing in hello same subscription.</span></span>
7.  <span data-ttu-id="8940b-119">Vyberte nebo vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="8940b-119">Select or create a resource group.</span></span> <span data-ttu-id="8940b-120">Skupina prostředků je kolekce společně používaných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="8940b-120">A resource group is a collection of Azure resources used together.</span></span>
8.  <span data-ttu-id="8940b-121">Vyberte umístění pro hostování.</span><span class="sxs-lookup"><span data-stu-id="8940b-121">Select a hosting location.</span></span> <span data-ttu-id="8940b-122">tooavoid přesunutí dat mezi datového centra, vyberte umístění, které obsahuje váš zdroj událostí.</span><span class="sxs-lookup"><span data-stu-id="8940b-122">tooavoid moving data across data centers, choose location that contains your event source.</span></span>
9.  <span data-ttu-id="8940b-123">Vyberte cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="8940b-123">Select a pricing tier.</span></span>
10. <span data-ttu-id="8940b-124">Vyberte kapacitu.</span><span class="sxs-lookup"><span data-stu-id="8940b-124">Select capacity.</span></span> <span data-ttu-id="8940b-125">Kapacitu prostředí můžete po vytvoření změnit.</span><span class="sxs-lookup"><span data-stu-id="8940b-125">You can change capacity of an environment after creation.</span></span>
11. <span data-ttu-id="8940b-126">Vytvořte prostředí.</span><span class="sxs-lookup"><span data-stu-id="8940b-126">Create your environment.</span></span> <span data-ttu-id="8940b-127">Můžete taky připnout řídicího panelu toohello prostředí pro snadný přístup při každém přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8940b-127">You can also pin your environment toohello dashboard for easy access whenever you sign in.</span></span>

  ![Vytvoření toodashboard pin časové řady Statistika hello](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a><span data-ttu-id="8940b-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8940b-129">Next steps</span></span>

* <span data-ttu-id="8940b-130">[Definovat zásady přístupu k datům](time-series-insights-data-access.md) tooaccess prostředí v [portálu Statistika časové řady](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="8940b-130">[Define data access policies](time-series-insights-data-access.md) tooaccess your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
* [<span data-ttu-id="8940b-131">Vytvoření zdroje událostí</span><span class="sxs-lookup"><span data-stu-id="8940b-131">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="8940b-132">[Odesílání událostí](time-series-insights-send-events.md) toohello zdroj události</span><span class="sxs-lookup"><span data-stu-id="8940b-132">[Send events](time-series-insights-send-events.md) toohello event source</span></span>
