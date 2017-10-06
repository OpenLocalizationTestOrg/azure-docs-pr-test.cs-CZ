---
title: "aaaHow tooadd prostředí Statistika řady čas Azure IoT Hub událostí zdroj tooyour | Microsoft Docs"
description: "Tento kurz popisuje, jak tooadd událost zdroj, který je připojen tooan IoT Hub tooyour časové řady Přehled prostředí"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: c626f9653d1c012360120fa9fc3d211d7d5beb5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-an-iot-hub-event-source"></a>Jak tooadd zdroje událostí služby IoT Hub

Tento kurz popisuje, jak toouse hello Azure portálu tooadd zdroje událostí, který čte z prostředí IoT Hub tooyour Statistika časové řady.

## <a name="prerequisites"></a>Požadavky

Vytvoření služby IoT Hub a jsou zápis tooit události. Další informace o centra IoT najdete v tématu <https://azure.microsoft.com/services/iot-hub/>

> [Skupiny příjemců] Každý zdroj události Statistika časové řady musí toohave vlastní vyhrazenou skupinu spotřebitelů která není sdílena s dalšími uživateli. Pokud využívat více čtenářů událostí z hello stejnou skupinu uživatelů, pravděpodobně toosee selhání jsou všechny nástroje pro čtení. Podrobnosti najdete v tématu hello [Příručka vývojáře pro službu IoT Hub](../iot-hub/iot-hub-devguide.md).

## <a name="choose-an-import-option"></a>Zvolte možnost importovat

Hello nastavení pro zdroj události hello lze zadat ručně nebo centrum IoT je možné vybrat z centra IoT hello, které jsou k dispozici tooyou.
V hello **možnost importu** selektor, vyberte jednu z hello následující možnosti:

* Zadejte nastavení centra IoT ručně
* Pomocí služby IoT Hub z dostupných předplatných

### <a name="select-an-available-iot-hub"></a>Vyberte dostupné služby IoT Hub

Hello následující tabulka popisuje jednotlivé možnosti na kartě hello nový zdroj událostí s jeho popis při výběru Centrum IoT k dispozici jako zdroje událostí:

| NÁZEV VLASTNOSTI | POPIS |
| --- | --- |
| Název zdroje událostí | Hello název zdroje událostí. Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.
| Zdroj | Zvolte **IoT Hub** toocreate zdroje událostí služby IoT Hub.
| Id předplatného | Vyberte hello předplatné, ve kterém byla vytvořena toto centrum IoT.
| Název centra IoT | Vyberte název hello hello IoT Hub.
| Název zásady centra IoT | Vyberte hello sdíleného přístupu zásady, které lze najít v hello karta nastavení služby IoT Hub. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. Hello sdílené zásady přístupu pro váš zdroj událostí *musí* mít **služba připojit** oprávnění.
| Skupina uživatelů centra IoT | Hello skupiny příjemců tooread události z hello IoT Hub. Vysoce je doporučeno toouse vyhrazenou skupinu spotřebitelů zdroje událostí.

### <a name="provide-iot-hub-settings-manually"></a>Zadejte nastavení centra IoT ručně

Hello následující tabulka popisuje každou vlastnost v kartě hello nový zdroj událostí s jeho popis při zadávání nastavení ručně:

| NÁZEV VLASTNOSTI | POPIS |
| --- | --- |
| Název zdroje událostí | Hello název zdroje událostí. Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.
| Zdroj | Zvolte **IoT Hub** toocreate zdroje událostí služby IoT Hub.
| Id předplatného | Hello předplatné, ve kterém byla vytvořena toto centrum IoT.
| Skupina prostředků | Hello předplatné, ve kterém byla vytvořena toto centrum IoT.
| Název centra IoT | Hello název služby IoT Hub. Když vytvoříte Centrum IoT, dáte mu taky určitý název
| Název zásady centra IoT | zásady přístupu Hello sdílené, které se dají vytvořit na hello karta nastavení služby IoT Hub. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. Hello sdílené zásady přístupu pro váš zdroj událostí *musí* mít **služba připojit** oprávnění.
| Klíč zásad centra IoT | Sdílený přístupový klíč Hello používá tooauthenticate oboru názvů Service Bus toohello přístup. Hello primární nebo sekundární klíč zadejte sem.
| Skupina uživatelů centra IoT | Hello skupiny příjemců tooread události z hello IoT Hub. Vysoce je doporučeno toouse vyhrazenou skupinu spotřebitelů zdroje událostí.

## <a name="next-steps"></a>Další kroky

1. Přidání prostředí dat zásad přístupu tooyour [data definovat zásady přístupu](time-series-insights-data-access.md)
1. Přístup k prostředí v hello [portálu Statistika časové řady](https://insights.timeseries.azure.com)
