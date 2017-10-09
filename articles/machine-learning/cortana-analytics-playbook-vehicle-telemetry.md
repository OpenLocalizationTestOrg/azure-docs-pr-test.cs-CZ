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
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Scénář řešení analýzy telemetrie vozidla
To **nabídky** odkazy kapitolám toohello v této scénářem. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Přehled
Super počítače byl přesunut mimo hello testovací prostředí a jsou nyní parkujících v našem Garážový! Tyto nejmodernější automobilů obsahovat velkého počtu senzorů, že jim poskytuje možnost tootrack hello a monitorovat miliony událostí za sekundu. Očekáváme, že do roku 2020, většinu těchto automobilů bude byla připojené toohello Internetu. Představte si klepnutím na do této zadávané data tooprovide větší zabezpečení, spolehlivost a lepší řízení prostředí! Společnost Microsoft vyvinula to sní realitou s Cortana Intelligence.

Cortana Intelligence společnosti Microsoft je plně spravovaná velkých objemů dat a pokročilé analýzy sada, která vám umožní tootransform vaše data do inteligentního akce. Chceme toointroduce toohello Cortana Intelligence Vehicle Telemetrie Analytics řešení šablony. Toto řešení ukazuje, jak dealerům car, automobilů výrobců a pojištění společností můžete použít možnosti hello Cortana Intelligence toogain v reálném čase a zvyklosti prediktivní Statistika na vehicle stavu a řídí. 

řešení Hello je implementovaný jako [lambda architektura vzor](https://en.wikipedia.org/wiki/Lambda_architecture) zobrazující hello úplné potenciální hello Cortana Intelligence platformy pro v reálném čase a dávkové zpracování. Hello řešení: 

* poskytuje Vehicle telematika simulátoru
* využívá pro příjem miliony událostí simulované vehicle telemetrická data do Azure Event Hubs 
* přehledy v reálném čase toogain Stream Analytics používá na vehicle stavu
* dál hello data do dlouhodobého úložiště pro širší batch analýzu. 
* využívá Machine Learning pro zjišťování anomálií v reálném čase a dávkové zpracování toogain prediktivní statistiky.
* využívá HDInsight tootransform data na škálování a objektu pro vytváření dat toohandle orchestration, plánování, správy prostředků a monitorování hello dávkové zpracování kanálu 
* Toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy pomocí Power BI

## <a name="architecture"></a>Architektura
![Diagram architektury řešení](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*obrázek 1 – Architektura řešení Analytics Vehicle Telemetrie*

Toto řešení zahrnuje následující hello **komponenty Cortana Intelligence** a umožňující prezentovat jejich integraci tooend end:

* **Služba Event Hubs** pro příjem miliony událostí vehicle telemetrická data do Azure.
* **Stream Analytics** pro získání přehledy v reálném čase na vehicle stavu a přetrvává tato data do dlouhodobého úložiště pro širší batch analýzu.
* **Strojového učení** pro zjišťování anomálií v reálném čase a dávkového zpracování toogain prediktivní statistiky.
* **HDInsight** data tootransform využít ve velkém měřítku
* **Objekt pro vytváření dat** zpracovává orchestration, plánování, správy prostředků a monitorování hello dávkové zpracování kanálu.
* **Power BI** toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy.

Toto řešení používá dva různé **zdroje dat**: 

* **Simulated vehicle signály a Diagnostika**: simulátoru telematika vehicle vysílá diagnostické informace a signály, které odpovídají toohello stav hello vehicle a hello řídí vzor k danému bodu v čase. 
* **Vehicle katalogu**: referenční datová sada obsahující VIN toomodel mapování.

