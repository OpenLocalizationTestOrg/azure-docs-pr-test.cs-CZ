---
title: "aaaPredict vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti hello Cortana Intelligence toogain v reálném čase a prediktivní Statistika na vehicle stavu a řídí zvyklosti."
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
ms.openlocfilehash: 54cc890ff39493bc040bb809721388349665720f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a><span data-ttu-id="c5aff-103">Scénář řešení analýzy telemetrie vozidla</span><span class="sxs-lookup"><span data-stu-id="c5aff-103">Vehicle telemetry analytics solution playbook</span></span>
<span data-ttu-id="c5aff-104">To **nabídky** odkazy kapitolám toohello v této scénářem.</span><span class="sxs-lookup"><span data-stu-id="c5aff-104">This **menu** links toohello chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a><span data-ttu-id="c5aff-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="c5aff-105">Overview</span></span>
<span data-ttu-id="c5aff-106">Super počítače byl přesunut mimo hello testovací prostředí a jsou nyní parkujících v našem Garážový!</span><span class="sxs-lookup"><span data-stu-id="c5aff-106">Super computers have moved out of hello lab and are now parked in our garage!</span></span> <span data-ttu-id="c5aff-107">Tyto nejmodernější automobilů obsahovat velkého počtu senzorů, že jim poskytuje možnost tootrack hello a monitorovat miliony událostí za sekundu.</span><span class="sxs-lookup"><span data-stu-id="c5aff-107">These cutting-edge automobiles contain a myriad of sensors, giving them hello ability tootrack and monitor millions of events every second.</span></span> <span data-ttu-id="c5aff-108">Očekáváme, že do roku 2020, většinu těchto automobilů bude byla připojené toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="c5aff-108">We expect that by 2020, most of these cars will have been connected toohello Internet.</span></span> <span data-ttu-id="c5aff-109">Představte si klepnutím na do této zadávané data tooprovide větší zabezpečení, spolehlivost a lepší řízení prostředí!</span><span class="sxs-lookup"><span data-stu-id="c5aff-109">Imagine tapping into this wealth of data tooprovide greater safety, reliability and a better driving experience!</span></span> <span data-ttu-id="c5aff-110">Společnost Microsoft vyvinula to sní realitou s Cortana Intelligence.</span><span class="sxs-lookup"><span data-stu-id="c5aff-110">Microsoft has made this dream a reality with Cortana Intelligence.</span></span>

<span data-ttu-id="c5aff-111">Cortana Intelligence společnosti Microsoft je plně spravovaná velkých objemů dat a pokročilé analýzy sada, která vám umožní tootransform vaše data do inteligentního akce.</span><span class="sxs-lookup"><span data-stu-id="c5aff-111">Microsoft’s Cortana Intelligence is a fully managed big data and advanced analytics suite that enables you tootransform your data into intelligent action.</span></span> <span data-ttu-id="c5aff-112">Chceme toointroduce toohello Cortana Intelligence Vehicle Telemetrie Analytics řešení šablony.</span><span class="sxs-lookup"><span data-stu-id="c5aff-112">We want toointroduce you toohello Cortana Intelligence Vehicle Telemetry Analytics Solution Template.</span></span> <span data-ttu-id="c5aff-113">Toto řešení ukazuje, jak dealerům car, automobilů výrobců a pojištění společností můžete použít možnosti hello Cortana Intelligence toogain v reálném čase a zvyklosti prediktivní Statistika na vehicle stavu a řídí.</span><span class="sxs-lookup"><span data-stu-id="c5aff-113">This solution demonstrates how car dealerships, automobile manufacturers, and insurance companies can use hello capabilities of Cortana Intelligence toogain real-time and predictive insights on vehicle health and driving habits.</span></span> 

<span data-ttu-id="c5aff-114">řešení Hello je implementovaný jako [lambda architektura vzor](https://en.wikipedia.org/wiki/Lambda_architecture) zobrazující hello úplné potenciální hello Cortana Intelligence platformy pro v reálném čase a dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="c5aff-114">hello solution is implemented as a [lambda architecture pattern](https://en.wikipedia.org/wiki/Lambda_architecture) showing hello full potential of hello Cortana Intelligence platform for real-time and batch processing.</span></span> <span data-ttu-id="c5aff-115">Hello řešení:</span><span class="sxs-lookup"><span data-stu-id="c5aff-115">hello solution:</span></span> 

* <span data-ttu-id="c5aff-116">poskytuje Vehicle telematika simulátoru</span><span class="sxs-lookup"><span data-stu-id="c5aff-116">provides a Vehicle Telematics simulator</span></span>
* <span data-ttu-id="c5aff-117">využívá pro příjem miliony událostí simulované vehicle telemetrická data do Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c5aff-117">leverages Event Hubs for ingesting millions of simulated vehicle telemetry events into Azure</span></span> 
* <span data-ttu-id="c5aff-118">přehledy v reálném čase toogain Stream Analytics používá na vehicle stavu</span><span class="sxs-lookup"><span data-stu-id="c5aff-118">uses Stream Analytics toogain real-time insights on vehicle health</span></span>
* <span data-ttu-id="c5aff-119">dál hello data do dlouhodobého úložiště pro širší batch analýzu.</span><span class="sxs-lookup"><span data-stu-id="c5aff-119">persists hello data into long-term storage for richer batch analytics.</span></span> 
* <span data-ttu-id="c5aff-120">využívá Machine Learning pro zjišťování anomálií v reálném čase a dávkové zpracování toogain prediktivní statistiky.</span><span class="sxs-lookup"><span data-stu-id="c5aff-120">takes advantage of Machine Learning for anomaly detection in real-time and batch processing toogain predictive insights.</span></span>
* <span data-ttu-id="c5aff-121">využívá HDInsight tootransform data na škálování a objektu pro vytváření dat toohandle orchestration, plánování, správy prostředků a monitorování hello dávkové zpracování kanálu</span><span class="sxs-lookup"><span data-stu-id="c5aff-121">leverages HDInsight tootransform data at scale and Data Factory toohandle orchestration, scheduling, resource management, and monitoring of hello batch processing pipeline</span></span> 
* <span data-ttu-id="c5aff-122">Toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="c5aff-122">gives this solution a rich dashboard for real-time data and predictive analytics visualizations using Power BI</span></span>

## <a name="architecture"></a><span data-ttu-id="c5aff-123">Architektura</span><span class="sxs-lookup"><span data-stu-id="c5aff-123">Architecture</span></span>
<span data-ttu-id="c5aff-124">![Diagram architektury řešení](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*obrázek 1 – Architektura řešení Analytics Vehicle Telemetrie*</span><span class="sxs-lookup"><span data-stu-id="c5aff-124">![Solution architecture diagram](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Figure 1 – Vehicle Telemetry Analytics Solution Architecture*</span></span>

<span data-ttu-id="c5aff-125">Toto řešení zahrnuje následující hello **komponenty Cortana Intelligence** a umožňující prezentovat jejich integraci tooend end:</span><span class="sxs-lookup"><span data-stu-id="c5aff-125">This solution includes hello following **Cortana Intelligence components** and showcases their end tooend integration:</span></span>

* <span data-ttu-id="c5aff-126">**Služba Event Hubs** pro příjem miliony událostí vehicle telemetrická data do Azure.</span><span class="sxs-lookup"><span data-stu-id="c5aff-126">**Event Hubs** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="c5aff-127">**Stream Analytics** pro získání přehledy v reálném čase na vehicle stavu a přetrvává tato data do dlouhodobého úložiště pro širší batch analýzu.</span><span class="sxs-lookup"><span data-stu-id="c5aff-127">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="c5aff-128">**Strojového učení** pro zjišťování anomálií v reálném čase a dávkového zpracování toogain prediktivní statistiky.</span><span class="sxs-lookup"><span data-stu-id="c5aff-128">**Machine Learning** for anomaly detection in real-time and batch processing toogain predictive insights.</span></span>
* <span data-ttu-id="c5aff-129">**HDInsight** data tootransform využít ve velkém měřítku</span><span class="sxs-lookup"><span data-stu-id="c5aff-129">**HDInsight** is leveraged tootransform data at scale</span></span>
* <span data-ttu-id="c5aff-130">**Objekt pro vytváření dat** zpracovává orchestration, plánování, správy prostředků a monitorování hello dávkové zpracování kanálu.</span><span class="sxs-lookup"><span data-stu-id="c5aff-130">**Data Factory** handles orchestration, scheduling, resource management and monitoring of hello batch processing pipeline.</span></span>
* <span data-ttu-id="c5aff-131">**Power BI** toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="c5aff-131">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span>

<span data-ttu-id="c5aff-132">Toto řešení používá dva různé **zdroje dat**:</span><span class="sxs-lookup"><span data-stu-id="c5aff-132">This solution accesses two different **data sources**:</span></span> 

* <span data-ttu-id="c5aff-133">**Simulated vehicle signály a Diagnostika**: simulátoru telematika vehicle vysílá diagnostické informace a signály, které odpovídají toohello stav hello vehicle a hello řídí vzor k danému bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="c5aff-133">**Simulated vehicle signals and diagnostics**: A vehicle telematics simulator emits diagnostic information and signals that correspond toohello state of hello vehicle and hello driving pattern at a given point in time.</span></span> 
* <span data-ttu-id="c5aff-134">**Vehicle katalogu**: referenční datová sada obsahující VIN toomodel mapování.</span><span class="sxs-lookup"><span data-stu-id="c5aff-134">**Vehicle catalog**: A reference dataset containing a VIN toomodel mapping.</span></span>

