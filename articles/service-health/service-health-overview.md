---
title: "Přehled stavu služby aaaAzure | Microsoft Docs"
description: "Přizpůsobené informace o tom, jak aplikace Azure ovlivněny problémy aktuálních a budoucích služby Azure a údržby."
services: Resource health
documentationcenter: 
author: rboucher
manager: 
editor: 
ms.assetid: 
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/07/2017
ms.author: robb
ms.openlocfilehash: 2b536ee2f19757d4f2baf5529866c3d159a4670c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-health"></a>Azure Service Health
Azure služba stavu poskytuje informace o včas a přizpůsobené, při problémy na portálu služby Azure vliv na vaše služby.  Pomáhá také můžete připravit na nadcházející plánované údržby.

## <a name="service-health-events"></a>Události stavu služby
Stav služby sleduje tři druhy zdraví události, které může mít vliv na vaše prostředky:
1. **Služba problémy** -problémy v hello služby Azure, které teď můžete ovlivnit. 
2. **Plánované údržby** -nadcházející údržby, který může mít vliv na dostupnost hello vašich služeb v budoucnu hello.  
3. **Stav zpravodaje** -změny v služby Azure, které vyžadují vaši pozornost. Mezi příklady patří, když jsou zastaralé funkce Azure, nebo pokud překročí kvótu využití.

    ![Události stavu služby](./media/service-health-overview/azure-service-health-overview-7.png)

## <a name="get-started-with-service-health"></a>Začínáme s stav služby
toolaunch na řídicího panelu portálu dlaždici řídicího panelu stavu služby, vyberte hello stav služby. Pokud jste dříve odebrali hello dlaždice nebo používáte vlastní řídicí panel, vyhledejte službu Service Health v "Další služby" (vpravo na řídicím panelu dolů).
![Začínáme s stav služby](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Najdete v části aktuální problémy, což ovlivňuje vašim službám
Hello **služby problémy** jsou zobrazeny všechny probíhající problémy v služby Azure, které, které mají vliv vašich prostředků. Rozumíte zahájení hello problém a jaké služby a oblastí, dopad. Můžete si také přečíst nejnovější aktualizace toounderstand hello co Azure provádí tooresolve hello problém. 
![Správa služby problém](./media/service-health-overview/azure-service-health-overview-2.png)

Zvolte hello **potenciální dopad** kartě toosee hello konkrétní seznam prostředky, které vlastníte, které může být ovlivněno hello problém. Můžete stáhnout seznam CSV tooshare tyto prostředky se svým týmem.
![Spravovat potíže služby – dopad](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Získat odkazy a vysvětlení ke stažení 
V systému správy problému můžete získat odkaz pro toouse problém hello. S uživateli, kteří nemají toohello přístup k portálu Azure můžete si stáhnout PDF a někdy tooshare souborů CSV.   
![Správa služby potíže – Správa problémů](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Získat podporu od společnosti Microsoft
Pokud prostředek je ve špatném stavu i po hello problém se vyřeší, obraťte se na podporu.  Pomocí hello podporu odkazů na hello napravo od stránku hello.  

## <a name="pin-a-personalized-health-map-tooyour-dashboard"></a>Připnout řídicí panel přizpůsobený stavu mapy tooyour
Stav služby tooshow filtrovat vaší důležitých odběry, oblasti a typy prostředků. Uložte hello filtru a kód pin přizpůsobené world mapy tooyour portálu řídicí panel stavu. 
![Mapa přizpůsobené stavu filtru](./media/service-health-overview/azure-service-health-overview-6a.png)
![připnout mapu přizpůsobené stavu](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Konfigurace výstrah stavu služby
Azure stavu služby se integruje s Azure monitorování tooalert jste prostřednictvím e-mailů, textové zprávy a oznámení webhook při dopad na vaše podnikové prostředky. Nastavte výstrahu aktivity protokolu události stavu služby příslušné hello. Trasy, která výstrahu toohello příslušné osoby ve vaší organizaci pomocí akce skupin. Další informace najdete v tématu [konfigurovat výstrahy pro stav služby](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md)

# <a name="next-steps"></a>Další kroky
Nastavení výstrah, takže je znázorněna problémy v oblasti stavu. Další informace najdete v tématu [konfigurovat výstrahy pro stav služby](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 