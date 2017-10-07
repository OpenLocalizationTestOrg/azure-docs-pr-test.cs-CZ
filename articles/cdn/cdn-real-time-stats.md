---
title: "statistiky aaaReal čas v Azure CDN | Microsoft Docs"
description: "Statistiky v reálném čase poskytuje data o výkonu hello Azure CDN v reálném čase, při doručování obsahu tooyour klientů."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 68900a5092b767e45c1fdf9cef2cd03f55f38a6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Statistiky v reálném čase v Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Přehled
Tento dokument popisuje statistiky v reálném čase v Microsoft Azure CDN.  Tato funkce poskytuje data v reálném čase, například šířku pásma, mezipaměti stavy a souběžných připojení tooyour CDN profilu při doručování obsahu tooyour klientů. To umožňuje nepřetržité monitorování stavu hello služby kdykoli, včetně přejděte živé události.

k dispozici jsou následující grafy Hello:

* [Šířka pásma](#bandwidth)
* [Stavové kódy](#status-codes)
* [Stavy mezipaměti](#cache-statuses)
* [Připojení](#connections)

## <a name="accessing-real-time-stats"></a>Přístup k statistiky v reálném čase
1. V hello [portálu Azure](https://portal.azure.com), procházet tooyour profil CDN.
   
    ![Okno profil CDN](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    Otevře portál pro správu Hello CDN.
3. Hover přes hello **Analytics** kartu a potom hover přes hello **statistiky v reálném čase** plovoucím panelem.  Klikněte na **velkého objektu HTTP**.
   
    ![Portál pro správu CDN](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    Zobrazí se grafy Hello statistiky v reálném čase.

Každý z hello grafy zobrazuje v reálném čase statistiku pro hello vybrané časové období, spuštění, pokud stránka hello načte.  grafy Hello aktualizovat automaticky každých několik sekund.  Hello **aktualizovat graf** tlačítko, pokud existuje, dojde k vymazání hello grafu, po které se zobrazí jenom hello vybraná data.

## <a name="bandwidth"></a>Šířka pásma
![Graf šířky pásma](./media/cdn-real-time-stats/cdn-bandwidth.png)

Hello **šířky pásma** grafu zobrazí hello šířku pásma použít pro aktuální platformě hello přes hello vybrané časové období. Hello šedou barvou část grafu hello označuje využití šířky pásma. přesné množství šířky pásma, které jsou právě používány Hello se zobrazí pod hello spojnicový graf.

## <a name="status-codes"></a>Stavové kódy
![Graf kódu stavu](./media/cdn-real-time-stats/cdn-status-codes.png)

Hello **stavové kódy** grafu určuje, jak často se provádí některé kódy odpovědi HTTP přes hello vybrané časové období.

> [!TIP]
> Popis jednotlivých možností kód stavu HTTP najdete v tématu [stavové kódy HTTP CDN Azure](https://msdn.microsoft.com/library/mt759238.aspx).
> 
> 

Zobrazí se seznam stavové kódy HTTP přímo nad hello grafu. Tento seznam označuje každý kód stavu, které můžou být součástí hello spojnicový graf a hello aktuální počet výskytů za sekundu pro tento stavový kód. Ve výchozím nastavení zobrazí se řádek pro každou z těchto stavové kódy v grafu hello. Můžete však zvolit tooonly monitorování hello stavové kódy, které mají zvláštní význam pro konfiguraci CDN. toodo, zkontrolujte hello potřeby stavové kódy a zrušte zaškrtnutí všech ostatních možností a potom klikněte na tlačítko **aktualizovat graf**. 

Můžete dočasně skrýt data protokolu pro konkrétní stavový kód.  Z hello legendy přímo pod hello grafu klikněte na tlačítko chcete toohide hello stavový kód. Hello stavový kód bude okamžitě skrytá z hello grafu. Kliknutím na tento stavový kód znovu způsobí, že tuto možnost toobe zobrazí znovu.

## <a name="cache-statuses"></a>Stavy mezipaměti
![Stavy graf do mezipaměti](./media/cdn-real-time-stats/cdn-cache-status.png)

Hello **mezipaměti stavy** grafu určuje, jak často se provádí některé typy stavů mezipaměti přes hello vybrané časové období. 

> [!TIP]
> Popis jednotlivých možností kód stavu mezipaměti najdete v tématu [Azure CDN mezipaměti stavové kódy](https://msdn.microsoft.com/library/mt759237.aspx).
> 
> 

Zobrazí se seznam mezipaměti stavové kódy přímo nad hello grafu. Tento seznam označuje každý kód stavu, které můžou být součástí hello spojnicový graf a hello aktuální počet výskytů za sekundu pro tento stavový kód. Ve výchozím nastavení zobrazí se řádek pro každou z těchto stavové kódy v grafu hello. Můžete však zvolit tooonly monitorování hello stavové kódy, které mají zvláštní význam pro konfiguraci CDN. toodo, zkontrolujte hello potřeby stavové kódy a zrušte zaškrtnutí všech ostatních možností a potom klikněte na tlačítko **aktualizovat graf**. 

Můžete dočasně skrýt data protokolu pro konkrétní stavový kód.  Z hello legendy přímo pod hello grafu klikněte na tlačítko chcete toohide hello stavový kód. Hello stavový kód bude okamžitě skrytá z hello grafu. Kliknutím na tento stavový kód znovu způsobí, že tuto možnost toobe zobrazí znovu.

## <a name="connections"></a>Připojení
![Graf připojení](./media/cdn-real-time-stats/cdn-connections.png)

Tento graf Určuje, kolik připojení byly zavedené tooyour hraniční servery. Každý požadavek pro určitý prostředek, který prochází výsledky našich CDN v připojení.

## <a name="next-steps"></a>Další kroky
* Upozorňování pomocí [výstrah v reálném čase v Azure CDN](cdn-real-time-alerts.md)
* Dig hlubší s [Rozšířené sestavy HTTP](cdn-advanced-http-reports.md)
* Analýza [vzorce používání](cdn-analyze-usage-patterns.md)

