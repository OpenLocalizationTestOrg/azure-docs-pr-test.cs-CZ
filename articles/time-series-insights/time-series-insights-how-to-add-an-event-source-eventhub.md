---
title: "aaaHow tooadd prostředí centra událostí tooyour zdroje událostí Statistika řady čas Azure | Microsoft Docs"
description: "Tento kurz popisuje, jak tooadd událost zdroje který je připojený tooan centra událostí tooyour časové řady Přehled prostředí"
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
ms.openlocfilehash: a98cd91deb51c829084a00c5f2a80b39d0fc511c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-an-event-hub-event-source"></a>Jak tooadd zdroje událostí centra událostí

Tento kurz popisuje, jak toouse hello Azure portálu tooadd zdroje událostí, který čte z prostředí časové řady Statistika tooyour centra událostí.

## <a name="prerequisites"></a>Požadavky

Vytvoření centra událostí a jsou zápis tooit události. Další informace o službě Event Hubs naleznete v tématu <https://azure.microsoft.com/services/event-hubs/>

> [Skupiny příjemců] Každý zdroj události Statistika časové řady musí toohave vlastní vyhrazenou skupinu spotřebitelů která není sdílena s dalšími uživateli. Pokud využívat více čtenářů událostí z hello stejnou skupinu uživatelů, pravděpodobně toosee selhání jsou všechny nástroje pro čtení. Všimněte si, že také maximální počet 20 skupiny příjemců za centra událostí. Podrobnosti najdete v tématu hello [Průvodce programováním centra událostí](../event-hubs/event-hubs-programming-guide.md).

## <a name="choose-an-import-option"></a>Zvolte možnost importovat

Hello nastavení pro zdroj události hello lze zadat ručně nebo centra událostí je možné vybrat z hello centra událostí, které jsou k dispozici tooyou.
V hello **možnost importu** selektor, vyberte jednu z hello následující možnosti:

* Určit nastavení centra událostí ručně
* Použít Centrum událostí z dostupných předplatných

### <a name="select-an-available-event-hub"></a>Vyberte dostupný centra událostí

Hello následující tabulka popisuje jednotlivé možnosti na kartě hello nový zdroj událostí s jeho popis při výběru dostupná centra událostí jako zdroje událostí:

| NÁZEV VLASTNOSTI | POPIS |
| --- | --- |
| Název zdroje událostí | Hello název zdroje událostí. Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.
| Zdroj | Zvolte **centra událostí** toocreate zdroje událostí centra událostí.
| Id předplatného | Vyberte hello předplatné, ve kterém byla vytvořena toto Centrum událostí.
| Obor názvů sběrnice | Vyberte hello oboru názvů Service Bus, která obsahuje hello centra událostí.
| Název centra událostí | Vyberte název hello hello centra událostí.
| Název zásady centra událostí | Vyberte zásady hello sdílený přístup, který lze vytvořit na kartě Konfigurace centra událostí hello. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. Hello sdílené zásady přístupu pro váš zdroj událostí *musí* mít **číst** oprávnění.
| Skupina uživatelů centra událostí | Hello skupiny příjemců tooread události z hello centra událostí. Vysoce je doporučeno toouse vyhrazenou skupinu spotřebitelů zdroje událostí.

### <a name="provide-event-hub-settings-manually"></a>Určit nastavení centra událostí ručně

Hello následující tabulka popisuje každou vlastnost v kartě hello nový zdroj událostí s jeho popis při zadávání nastavení ručně:

| NÁZEV VLASTNOSTI | POPIS |
| --- | --- |
| Název zdroje událostí | Hello název zdroje událostí. Tento název musí být jedinečný v rámci vašeho prostředí Statistika časové řady.
| Zdroj | Zvolte **centra událostí** toocreate zdroje událostí centra událostí.
| Id předplatného | Hello předplatné, ve kterém byla vytvořena toto Centrum událostí.
| Skupina prostředků | Hello předplatné, ve kterém byla vytvořena toto Centrum událostí.
| Obor názvů sběrnice | Obor názvů sběrnice je kontejner sady entit pro zasílání zpráv. Při vytváření nového centra událostí, vytvoříte tím taky obor názvů sběrnice.
| Název centra událostí | Hello název vašeho centra událostí. Když vytvoříte Centrum událostí, dáte mu taky určitý název
| Název zásady centra událostí | Hello zásady sdíleného přístupu, které se dají vytvořit na kartě Konfigurace centra událostí hello. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. Hello sdílené zásady přístupu pro váš zdroj událostí *musí* mít **číst** oprávnění.
| Klíč události rozbočovače zásad | Sdílený přístupový klíč Hello používá tooauthenticate oboru názvů Service Bus toohello přístup. Hello primární nebo sekundární klíč zadejte sem.
| Skupina uživatelů centra událostí | Hello skupiny příjemců tooread události z hello centra událostí. Vysoce je doporučeno toouse vyhrazenou skupinu spotřebitelů zdroje událostí.

## <a name="next-steps"></a>Další kroky

1. Přidání prostředí dat zásad přístupu tooyour [data definovat zásady přístupu](time-series-insights-data-access.md)
1. Přístup k prostředí v hello [portálu Statistika časové řady](https://insights.timeseries.azure.com)
