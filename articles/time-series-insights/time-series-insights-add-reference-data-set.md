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
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a>Vytvořit odkaz na sadu dat pro vaše prostředí časové řady statistika pomocí portálu Ibiza hello

Referenční datové sady je kolekce položek, které jsou rozšířen s událostmi hello ze zdroje událostí. Modul příchozího přenosu dat Time Series Insights se připojí k události ze zdroje událostí s položkou v referenční sadě dat. Tato rozšířená událost je pak k dispozici pro dotaz. Toto připojení je založena na definované v odkaz na sadu dat hello klíče.

## <a name="steps-tooadd-a-reference-data-set-tooyour-environment"></a>Kroky tooadd prostředí tooyour referenční datové sady

1. Přihlaste se toohello [portál Ibiza](https://portal.azure.com).
2. V nabídce hello na levé straně hello portálu Ibiza hello klikněte na tlačítko "Všechny prostředky".
3. Vyberte vaše prostředí Time Series Insights.

    ![Vytvoření hello časové řady Statistika referenční datové sady](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Vyberte možnost Referenční sady dat a klikněte na Přidat.

    ![Vytvoření hello časové řady Statistika referenční datové sady – podrobnosti](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. Zadejte název hello hello referenční datové sady.
6. Zadejte název klíče hello a jeho typu. Tento název a typ je použité toopick hello správná vlastnost z hello události ve zdroji událostí. Například pokud zadáte název klíče jako "DeviceId" a zadejte jako "Řetězec", pak hello čas řady Statistika příchozího modul hledat vlastnost s názvem hello "DeviceId" typu "Řetězec" v hello příchozí události. Můžete zadat více než jeden klíč toojoin s hello událostí. Hello vlastnost název shoda rozlišuje velká a malá písmena.

     ![Vytvoření hello časové řady Statistika referenční datové sady – podrobnosti](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. Klikněte na Vytvořit.

## <a name="next-steps"></a>Další kroky

* [Spravujte referenční data](time-series-insights-manage-reference-data-csharp.md) prostřednictvím kódu programu.
* Hello úplný referenční dokumentace rozhraní API, najdete v části [dat referenční dokumentace rozhraní API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) dokumentu.
