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
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a>Vytvoření zdroje událostí pro vaše prostředí časové řady statistika pomocí portálu Ibiza hello

Zdroj událostí Time Series Insights je odvozený od zprostředkovatele událostí, jako je například služba Azure Event Hubs. Časové řady Insights se připojuje přímo tooEvent zdrojů, příjem hello datový proud bez nutnosti uživatelé toowrite jeden řádek kódu. V současné době Time Series Insights podporuje Azure Event Hubs a Azure IoT Hubs. V budoucích hello se přidají další zdroje událostí.

## <a name="steps-tooadd-an-event-source-tooyour-environment"></a>Kroky tooadd prostředí tooyour zdroje událostí

1.  Přihlaste se toohello [portál Ibiza](https://portal.azure.com).
2.  V nabídce hello na levé straně hello portálu Ibiza hello klikněte na tlačítko "Všechny prostředky".
3.  Vyberte vaše prostředí Time Series Insights.

  ![Vytvořit zdroj události časové řady Statistika hello](media/add-event-source/getstarted-create-event-source-1.png)

4.  Vyberte „Zdroje událostí“ a klikněte na „+ Přidat“.

  ![Vytvořit zdroj události hello časové řady Statistika – podrobnosti](media/add-event-source/getstarted-create-event-source-2.png)

5.  Zadejte název hello hello zdroje událostí. Tento název je přidružený ke všem událostem, které přichází z tohoto zdroje událostí, a je dostupný v době zpracování dotazu.
6.  Centra událostí vyberte ze seznamu hello centra událostí prostředků v aktuálním předplatném hello. Jinak vyberte možnost import "nastavení centra událostí zadejte ručně" toospecify centra událostí v jiné předplatné. Události musí být publikovány ve formátu JSON.
7.  Vyberte zásadu, která má v Centru událostí hello oprávnění ke čtení.
8.  Zadejte skupinu příjemců centra událostí.

  > [!IMPORTANT]
  > Zajistěte, aby tuto skupinu příjemců nepoužívala žádná jiná služba (například úloha služby Stream Analytics nebo jiné prostředí Time Series Insights). Pokud je skupina uživatelů používají i jiné služby, přečtěte si, že operace je negativně ovlivňovat to pro toto prostředí a hello dalších služeb. Pokud používáte "$Default" jako hello skupiny příjemců, je by mohlo vést toopotential opakované použití jiných čtečky.

9.  Klikněte na Vytvořit.

Po vytvoření zdroje události hello časové řady Insights automaticky spustí, streamování dat do vašeho prostředí.

## <a name="next-steps"></a>Další kroky

* [Odesílání událostí](time-series-insights-send-events.md) toohello zdroj události
* Zobrazení prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)
