---
title: "Předpověď vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti Cortana Intelligence získat přehledy v reálném čase a prediktivní na vehicle stavu a řídí zvyklosti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: d202d314c61416cf306f760f93e0a4a88a1ab42b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a><span data-ttu-id="d3460-103">Scénář řešení analýzy telemetrie vozidla</span><span class="sxs-lookup"><span data-stu-id="d3460-103">Vehicle telemetry analytics solution playbook</span></span>
<span data-ttu-id="d3460-104">To **nabídky** odkazy na kapitoly v této scénářem.</span><span class="sxs-lookup"><span data-stu-id="d3460-104">This **menu** links to the chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a><span data-ttu-id="d3460-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="d3460-105">Overview</span></span>
<span data-ttu-id="d3460-106">Super počítače byl přesunut mimo testovací prostředí a jsou nyní parkujících v našem Garážový!</span><span class="sxs-lookup"><span data-stu-id="d3460-106">Super computers have moved out of the lab and are now parked in our garage!</span></span> <span data-ttu-id="d3460-107">Tyto nejmodernější automobilů obsahovat velkého počtu senzorů, bude mít možnost ke sledování a monitorování miliony událostí za sekundu.</span><span class="sxs-lookup"><span data-stu-id="d3460-107">These cutting-edge automobiles contain a myriad of sensors, giving them the ability to track and monitor millions of events every second.</span></span> <span data-ttu-id="d3460-108">Očekáváme, že do roku 2020, většinu těchto aut bude mít byl připojený k Internetu.</span><span class="sxs-lookup"><span data-stu-id="d3460-108">We expect that by 2020, most of these cars will have been connected to the Internet.</span></span> <span data-ttu-id="d3460-109">Představte si klepnutím na do této zadávané data pro zajištění větší zabezpečení, spolehlivost a lepší řízení prostředí!</span><span class="sxs-lookup"><span data-stu-id="d3460-109">Imagine tapping into this wealth of data to provide greater safety, reliability and a better driving experience!</span></span> <span data-ttu-id="d3460-110">Společnost Microsoft vyvinula to sní realitou s Cortana Intelligence.</span><span class="sxs-lookup"><span data-stu-id="d3460-110">Microsoft has made this dream a reality with Cortana Intelligence.</span></span>

<span data-ttu-id="d3460-111">Cortana Intelligence společnosti Microsoft je plně spravovaná velkých objemů dat a suite pokročilou analýzu, který umožňuje transformovat data do inteligentního akce.</span><span class="sxs-lookup"><span data-stu-id="d3460-111">Microsoft’s Cortana Intelligence is a fully managed big data and advanced analytics suite that enables you to transform your data into intelligent action.</span></span> <span data-ttu-id="d3460-112">Chceme, které vám představí šablona Cortana Intelligence Vehicle Telemetrie Analytics řešení.</span><span class="sxs-lookup"><span data-stu-id="d3460-112">We want to introduce you to the Cortana Intelligence Vehicle Telemetry Analytics Solution Template.</span></span> <span data-ttu-id="d3460-113">Toto řešení ukazuje, jak dealerům car, automobilů výrobců a pojištění společností můžete použít možnosti Cortana Intelligence k získání v reálném čase a zvyklosti prediktivní Statistika na vehicle stavu a řídí.</span><span class="sxs-lookup"><span data-stu-id="d3460-113">This solution demonstrates how car dealerships, automobile manufacturers, and insurance companies can use the capabilities of Cortana Intelligence to gain real-time and predictive insights on vehicle health and driving habits.</span></span> 

<span data-ttu-id="d3460-114">Řešení je implementovaný jako [lambda architektura vzor](https://en.wikipedia.org/wiki/Lambda_architecture) zobrazující potenciální Cortana Intelligence platformy pro celý v reálném čase a dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="d3460-114">The solution is implemented as a [lambda architecture pattern](https://en.wikipedia.org/wiki/Lambda_architecture) showing the full potential of the Cortana Intelligence platform for real-time and batch processing.</span></span> <span data-ttu-id="d3460-115">Řešení:</span><span class="sxs-lookup"><span data-stu-id="d3460-115">The solution:</span></span> 

* <span data-ttu-id="d3460-116">poskytuje Vehicle telematika simulátoru</span><span class="sxs-lookup"><span data-stu-id="d3460-116">provides a Vehicle Telematics simulator</span></span>
* <span data-ttu-id="d3460-117">využívá pro příjem miliony událostí simulované vehicle telemetrická data do Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d3460-117">leverages Event Hubs for ingesting millions of simulated vehicle telemetry events into Azure</span></span> 
* <span data-ttu-id="d3460-118">Stream Analytics používá k získání přehledy v reálném čase na vehicle stavu</span><span class="sxs-lookup"><span data-stu-id="d3460-118">uses Stream Analytics to gain real-time insights on vehicle health</span></span>
* <span data-ttu-id="d3460-119">trvá data do dlouhodobého úložiště pro širší batch analýzu.</span><span class="sxs-lookup"><span data-stu-id="d3460-119">persists the data into long-term storage for richer batch analytics.</span></span> 
* <span data-ttu-id="d3460-120">využívá Machine Learning pro zjišťování anomálií v reálném čase a zpracování a získáte přehled o prediktivní služby batch.</span><span class="sxs-lookup"><span data-stu-id="d3460-120">takes advantage of Machine Learning for anomaly detection in real-time and batch processing to gain predictive insights.</span></span>
* <span data-ttu-id="d3460-121">využívá HDInsight k transformaci dat ve velkém měřítku a Data Factory pro zpracování orchestration, plánování, správy prostředků a monitorování dávkové zpracování kanálu</span><span class="sxs-lookup"><span data-stu-id="d3460-121">leverages HDInsight to transform data at scale and Data Factory to handle orchestration, scheduling, resource management, and monitoring of the batch processing pipeline</span></span> 
* <span data-ttu-id="d3460-122">Toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="d3460-122">gives this solution a rich dashboard for real-time data and predictive analytics visualizations using Power BI</span></span>

## <a name="architecture"></a><span data-ttu-id="d3460-123">Architektura</span><span class="sxs-lookup"><span data-stu-id="d3460-123">Architecture</span></span>
<span data-ttu-id="d3460-124">![Diagram architektury řešení](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*obrázek 1 – Architektura řešení Analytics Vehicle Telemetrie*</span><span class="sxs-lookup"><span data-stu-id="d3460-124">![Solution architecture diagram](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Figure 1 – Vehicle Telemetry Analytics Solution Architecture*</span></span>

<span data-ttu-id="d3460-125">Toto řešení zahrnuje následující **komponenty Cortana Intelligence** a umožňující prezentovat jejich koncové integrace:</span><span class="sxs-lookup"><span data-stu-id="d3460-125">This solution includes the following **Cortana Intelligence components** and showcases their end to end integration:</span></span>

* <span data-ttu-id="d3460-126">**Služba Event Hubs** pro příjem miliony událostí vehicle telemetrická data do Azure.</span><span class="sxs-lookup"><span data-stu-id="d3460-126">**Event Hubs** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="d3460-127">**Stream Analytics** pro získání přehledy v reálném čase na vehicle stavu a přetrvává tato data do dlouhodobého úložiště pro širší batch analýzu.</span><span class="sxs-lookup"><span data-stu-id="d3460-127">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="d3460-128">**Strojového učení** pro zjišťování anomálií v reálném čase a dávkové zpracování a získáte přehled o prediktivní.</span><span class="sxs-lookup"><span data-stu-id="d3460-128">**Machine Learning** for anomaly detection in real-time and batch processing to gain predictive insights.</span></span>
* <span data-ttu-id="d3460-129">**HDInsight** je využít k transformaci dat ve velkém měřítku</span><span class="sxs-lookup"><span data-stu-id="d3460-129">**HDInsight** is leveraged to transform data at scale</span></span>
* <span data-ttu-id="d3460-130">**Objekt pro vytváření dat** zpracovává orchestration, plánování, správy prostředků a monitorování dávkové zpracování kanálu.</span><span class="sxs-lookup"><span data-stu-id="d3460-130">**Data Factory** handles orchestration, scheduling, resource management and monitoring of the batch processing pipeline.</span></span>
* <span data-ttu-id="d3460-131">**Power BI** toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="d3460-131">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span>

<span data-ttu-id="d3460-132">Toto řešení používá dva různé **zdroje dat**:</span><span class="sxs-lookup"><span data-stu-id="d3460-132">This solution accesses two different **data sources**:</span></span> 

* <span data-ttu-id="d3460-133">**Simulated vehicle signály a Diagnostika**: simulátoru telematika vehicle vysílá diagnostické informace a signály, které odpovídají stavu nástroj a podporovat jeho vzor k danému bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="d3460-133">**Simulated vehicle signals and diagnostics**: A vehicle telematics simulator emits diagnostic information and signals that correspond to the state of the vehicle and the driving pattern at a given point in time.</span></span> 
* <span data-ttu-id="d3460-134">**Vehicle katalogu**: referenční datová sada obsahující VIN pro mapování modelu.</span><span class="sxs-lookup"><span data-stu-id="d3460-134">**Vehicle catalog**: A reference dataset containing a VIN to model mapping.</span></span>

