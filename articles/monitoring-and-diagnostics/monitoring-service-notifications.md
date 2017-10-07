---
title: "aaaWhat jsou oznámení o stavu služby | Microsoft Docs"
description: "Oznámení o stavu služby povolených že zprávy o stavu služby tooview publikujete pomocí Microsoft Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 6f2fe72154c3e80d85062655c49dd1799b718e3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-health-notifications"></a>Oznámení o stavu služby
## <a name="overview"></a>Přehled

Tento článek ukazuje, jak hello oznámení o stavu tooview služby pomocí portálu Azure.

Oznámení o stavu služby povolit jste tooview služby stavu zpráv, které zveřejnil hello tým Azure, které mohou ovlivňovat hello prostředky v rámci svého předplatného. Tato oznámení jsou podtřídou třídy aktivity protokolu události a naleznete také v okně protokolu hello aktivity. Oznámení o stavu služby může být informační nebo níž lze provést akci v závislosti na třídu hello.

Existují pět třídy oznámení o stavu služby:  

- **Je vyžadována akce:** z času tootime jsme si povšimnout nějakou neobvyklou akci na vašem účtu. Může třeba toowork s tooremedy vám to. Pošleme vám oznámení buď s podrobnostmi o hello akce budete potřebovat tootake nebo s podrobnostmi o tom, technici toocontact Azure nebo podporovat.  
- **Odbornou obnovení:** došlo k události a techniky potvrdili, že se stále setkáváte dopad. Technici potřebovat toowork s vámi přímo toobring toorestoration vaší služby.  
- **Incident:** služby ovlivňující událostí aktuálně ovlivňuje jednu nebo více prostředků hello ve vašem předplatném.  
- **Údržba:** Toto je oznámení, která vás informuje o plánované údržbě aktivity, která může mít vliv na jeden nebo více prostředků hello v rámci svého předplatného.  
- **Informace o:** z času tootime může vám můžeme poslat oznámení, že komunikace tooyou o potenciální optimalizace, které mohou pomoci zlepšit váš využití prostředků.  
- **Zabezpečení:** naléhavé zabezpečení související informace týkající se vaší solution(s) spuštěné v Azure.

Každý oznámení o stavu služby bude vykonávat hello oboru a dopad tooyour prostředky podrobnosti. Podrobnosti o bude zahrnovat:

Název vlastnosti | Popis
-------- | -----------
Kanály | Patří mezi hello následující hodnoty: "Admin", "Operace"
correlationId | Obvykle je identifikátor GUID ve formátu řetězce hello. Události, patří toohello obvykle sdílet stejnou akci uber hello stejné correlationId.
eventDataId | Je jedinečný identifikátor hello události
EventName | Je název hello hello události
úroveň | Úroveň události hello. Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné"
resourceProviderName | Název zprostředkovatele prostředků hello hello dopad prostředků
Typ prostředku| Hello typ prostředku hello dopad prostředků
Podřízený stav | Obvykle hello stavový kód HTTP hello odpovídající volání REST, ale můžou taky patřit jiných řetězců popisující podřízeného stavu, jako jsou tyto hodnoty běžné: OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (HTTP Stavový kód: 204), chybný požadavek (kód stavu HTTP: 400), nebyl nalezen (kód stavu HTTP: 404), konflikt (kód stavu HTTP: 409), vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503), vypršel časový limit brány (kód stavu HTTP: 504).
eventTimestamp | Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost.
submissionTimestamp |   Časové razítko, když hello události jsou k dispozici pro zadávání dotazů.
subscriptionId | předplatné Azure, ve kterém se tato událost byla zaznamenána Hello
status | Řetězec s popisem hello stav operace hello. Některé běžné hodnoty jsou: spuštění v průběhu, bylo úspěšné, neúspěšné, aktivní, vyřešeno.
operationName | Název operace hello.
category | "ServiceHealth"
resourceId | Id prostředku hello dopad prostředků.
Properties.Title | Hello lokalizovaný název pro tuto komunikaci. Hello výchozím jazykem je angličtina.
Properties.Communication | Hello lokalizované podrobnosti hello komunikace s značka jazyka HTML. Výchozí hello je angličtina.
Properties.incidentType | Možné hodnoty: AssistedRecovery, ActionRequired, informace, incidentů, údržby, zabezpečení
Properties.trackingId | Identifikuje hello incident, které se tato událost je přidružen. Pomocí tohoto toocorrelate hello události související tooan incidentu.
Properties.impactedServices | Uvozený blob JSON, který popisuje hello služeb a oblasti, které jsou ovlivněny incidentem hello. Seznam služeb, z nichž každá má ServiceName a seznam ImpactedRegions, z nichž každá má RegionName.
Properties.defaultLanguageTitle | Hello komunikace v angličtině
Properties.defaultLanguageContent | Hello komunikace v angličtině jako značka jazyka html nebo prostý text
Properties.Stage | Možné hodnoty pro AssistedRecovery, ActionRequired, informace, incidentů, zabezpečení: jsou aktivní, vyřešeno. Za účelem údržby jsou: aktivní, plánovaná, InProgress, zrušení, Rescheduled, vyřešeno, dokončeno
Properties.communicationId | komunikace Hello Tato událost souvisí.


## <a name="viewing-your-service-health-notifications-in-hello-azure-portal"></a>Zobrazení oznámení o stavu služby v hello portálu Azure
1.  V hello [portál](https://portal.azure.com), přejděte toohello **monitorování** služby

    ![Monitorování](./media/monitoring-service-notifications/home-monitor.png)
2.  Klikněte na tlačítko hello **monitorování** možnost tooopen do okna Sledování hello. V tomto okně jsou uvedená veškerá vaše nastavení monitorování a data v jednom konsolidovaném zobrazení. Nejprve otevře toohello **protokol aktivit** části.

3.  Nyní klikněte na **oznámení o službách** části

    ![Monitorování](./media/monitoring-service-notifications/service-health-summary.png)
4.  Klikněte na některé z hello řádku položky tooview další podrobnosti

5. Klikněte na hello **+ přidat aktivitu protokolu výstrahy** operace tooreceive oznámení tooensure upozornění se zobrazí oznámení budoucí služby tohoto typu. Další informace o konfiguraci výstrah na oznámení o službách toolearn [, klikněte sem](monitoring-activity-log-alerts-on-service-notifications.md)

## <a name="next-steps"></a>Další kroky:
Přijímat [výstrahy oznámení pokaždé, když oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md) odeslání  
Další informace o [aktivity protokolu výstrahy](monitoring-activity-log-alerts.md)
