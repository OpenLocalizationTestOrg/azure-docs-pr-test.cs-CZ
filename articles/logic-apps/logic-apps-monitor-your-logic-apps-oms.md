---
title: "aaaMonitor a získání přehledy o aplikaci logiky spouští pomocí OMS - Azure Logic Apps | Microsoft Docs"
description: "Monitorování vaší logiku aplikace běží s insights tooget analýzy protokolů a Operations Management Suite (OMS) a bohatší ladění podrobnosti o řešení potíží a Diagnostika"
author: divyaswarnkar
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/9/2017
ms.author: LADocs; divswa
ms.openlocfilehash: a76fd6d1ff5c0010550be0f991514ce95f659fd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a>Monitorovat a získat přehled o logiku aplikace běží s Operations Management Suite (OMS) a analýzy protokolů

Pro monitorování a bohatší informace o ladění, můžete zapnout analýzy protokolů v hello stejnou dobu, po vytvoření aplikace logiky. Analýzy protokolů poskytuje možnosti pro diagnostiku protokolování a monitorování pro svou aplikaci logiky spouští prostřednictvím portálu hello Operations Management Suite (OMS). Když přidáte tooOMS řešení správy Logic Apps hello, zobrazí agregovaný stav pro logiku aplikace běží a konkrétní podrobnosti, jako je stav, čas spuštění, opakované odeslání stavu a ID korelace.

Toto téma ukazuje, jak tooturn na analýzy protokolů nebo nainstalujte hello řešení pro správu aplikace logiky v OMS, můžete zobrazit události modulu runtime a spusťte data pro svou aplikaci logiky.

 > [!TIP]
 > toomonitor existující logic apps, postupujte podle těchto kroků příliš [zapnout protokolování diagnostiky a odeslání logiky aplikace runtime data tooOMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Požadavky

Než začnete, je nutné toohave pracovním prostorem OMS. Další informace [jak toocreate pracovním prostorem OMS](../log-analytics/log-analytics-get-started.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Zapnutí protokolování diagnostiky při vytváření aplikací logiky

1. V [portál Azure](https://portal.azure.com), vytvoření aplikace logiky. Zvolte **nové** > **Enterprise integrace** > **aplikace logiky** > **vytvořit**.

   ![Vytvoření aplikace logiky](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. V hello **vytvořit aplikaci logiky** proveďte tyto úlohy, jak je znázorněno:

   1. Zadejte název pro svou aplikaci logiky a vyberte předplatné Azure. 
   2. Vytvořte nebo vyberte skupinu prostředků Azure.
   3. Nastavit **analýzy protokolů** příliš**na**. 
   Pracovní prostor OMS hello vyberte místo, kam chcete příliš odesílat data pro svou aplikaci logiky spustí. 
   4. Až budete připraveni, zvolte **Pin toodashboard** > **vytvořit**.

      ![Vytvoření aplikace logiky](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Po dokončení tohoto kroku, Azure vytvoří aplikace logiky, která je teď spojené s vaším pracovním prostorem OMS. 
      Tento krok navíc také automaticky nainstaluje řešení pro správu aplikace logiky hello v pracovním prostoru OMS.

3. tooview v OMS, které běží vaše aplikace logiky [pokračovat v těchto krocích](#view-logic-app-runs-oms).

## <a name="install-hello-logic-apps-management-solution-in-oms"></a>Nainstalujte řešení pro správu aplikace logiky hello v OMS

Pokud jste již zapnuli analýzy protokolů při vytvoření aplikace logiky, tento krok přeskočte. Už máte řešení pro správu aplikace logiky hello nainstalované v OMS.

1. V hello [portál Azure](https://portal.azure.com), zvolte **více služeb**. Vyhledejte "analýzy protokolů" jako filtr a vyberte **analýzy protokolů** znázorněné:

   ![Zvolte "Analýzy protokolů"](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. V části **analýzy protokolů**, najděte a vyberte pracovní prostor OMS. 

   ![Vyberte pracovní prostor OMS](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. V části **správy**, zvolte **portálu OMS**.

   ![Zvolte "Portál OMS"](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. Na domovské stránce OMS Pokud se zobrazí nápis upgradu hello, zvolte hello banner, aby nejprve upgradovat vaším pracovním prostorem OMS. Zvolte **řešení Galerie**.

   ![Zvolte "Galerie řešení"](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. V části **všechna řešení**, najít a vyberte dlaždici hello hello **správy Logic Apps** řešení.

   ![Zvolte "Správa aplikace logiky"](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. Zvolte řešení hello tooinstall v vaším pracovním prostorem OMS **přidat**.

   ![Zvolte "Přidat" pro "Správa aplikace logiky"](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a>Zobrazení, které běží vaše aplikace logiky v pracovním prostoru OMS

1. počet hello tooview a stav pro svou aplikaci logiky spustí, přejděte toohello přehledová stránka pracovního prostoru OMS. Zkontrolujte podrobnosti hello na hello **správy Logic Apps** dlaždici.

   ![Dlaždice přehledu se zobrazuje počet logiku aplikace spustit a stav](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > Pokud místo hello správy Logic Apps dlaždice se zobrazí banner upgradu, zvolte hello banner tak, aby nejprve upgradovat vaším pracovním prostorem OMS.
  
   > ![Upgrade "Pracovním prostorem OMS"](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. tooview souhrn s dalšími podrobnostmi o že logiku aplikace běží, zvolte hello **správy Logic Apps** dlaždici.

   Zde že logiku aplikace běží jsou seskupené podle názvu nebo stav spuštění.

   ![Souhrn stavu pro svou aplikaci logiky běží](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. tooview, které všechny hello se spouští pro určitou logiku aplikace nebo stav, vyberte hello řádek pro aplikace logiky nebo stavu.

   Tady je příklad, který zobrazuje všechny hello spuštění pro určitou logiku aplikace:

   ![Spuštění zobrazení pro aplikaci logiky nebo ve stavu](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   > [!NOTE]
   > Hello **opakované odeslání** sloupci se zobrazuje "Ano" pro spuštění, které jsou výsledkem znovu přijala spustit.

4. toofilter těchto výsledků, můžete provést, klientské a serverové filtrování.

   * Filtr na straně klienta: pro každý sloupec zvolte hello filtry, které chcete. 
   Zde je několik příkladů:

     ![Příklad filtry sloupců](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * Filtr na straně serveru: toochoose určité časové období nebo toolimit hello několik spustí, které se zobrazují, použijte ovládací prvek oboru hello hello horní části stránky hello. 
   Ve výchozím nastavení zobrazí pouze 1000 záznamů najednou. 
   
     ![Změna hello časový interval](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. všechny tooview hello akcí a jejich podrobnosti o konkrétní spustit, vyberte řádek, otevře se stránka hledání protokolů hello. 

   * Vyberte tyto informace v tabulce tooview **tabulky**.
   * dotaz hello toochange, můžete upravit řetězec dotazu hello v panelu vyhledávání hello. 
   Pro lepší podmínky, zvolte **pokročilé analýzy**.

     ![Zobrazit akce a podrobnosti pro spuštění aplikace logiky](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)

     V tomto poli na stránce hello analýzy protokolů Azure, můžete aktualizovat dotazů a zobrazení hello výsledky z tabulky hello. 
     Tento dotaz používá [Kusto dotazovací jazyk](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), který můžete upravit, pokud chcete, aby tooview odlišné výsledky. 

     ![Azure Log Analytics - zobrazení dotazu](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Další kroky

* [Monitorování zpráv B2B](../logic-apps/logic-apps-monitor-b2b-message.md)
