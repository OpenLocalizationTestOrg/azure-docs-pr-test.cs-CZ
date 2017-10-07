---
title: "aaaGet začít s Azure monitorování | Microsoft Docs"
description: "Začněte používat Azure monitorování toogain přehled o operaci hello vašich prostředků a proveďte akce na základě data."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: johnkem
ms.openlocfilehash: 49e7105621a9e60c9fa14c4911c4fe45e0d197a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-monitor"></a>Začínáme se službou Azure Monitor
Azure monitorování je služba hello platformy, která poskytuje jednoho zdroje pro monitorování prostředků Azure. S Azure monitorování můžete vizualizovat, dotaz, směrování, archivaci a provádět příslušné akce hello metriky a protokoly pocházejících z prostředků ve službě Azure. Můžete pracovat s těmito daty pomocí portálu okně monitorování hello [rutiny prostředí PowerShell monitorování](insights-powershell-samples.md), [rozhraní příkazového řádku a platformy](insights-cli-samples.md), nebo [rozhraní REST API Azure monitorování](https://msdn.microsoft.com/library/dn931943.aspx). V tomto článku jsme provede několik hello klíčové komponenty Azure monitorování, pro demonstrační pomocí portálu hello.

1. Hello portálu, přejděte příliš**další služby** a najde hello **monitorování** možnost. Klikněte na ikonu hvězdičky tooadd hello seznamu oblíbených položek tooyour tuto možnost tak, aby se vždy snadno dostupné v levém navigačním panelu hello.
   
    ![Sledování v seznamu služeb hello](./media/monitoring-get-started/monitor-more-services.png)
2. Klikněte na tlačítko hello **monitorování** tooopen možnost si hello **monitorování** okno. V tomto okně jsou uvedená veškerá vaše nastavení monitorování a data v jednom konsolidovaném zobrazení. Nejprve otevře toohello **protokol aktivit** části.
   
    ![Navigace v okně Monitor](./media/monitoring-get-started/monitor-blade-nav.png)
   
    Azure monitorování má tři základní kategorie dat monitorování: hello **protokol aktivit**, **metriky**, a **diagnostické protokoly**.
3. Klikněte na tlačítko **protokol aktivit** tooensure, který hello aktivity protokolu části se zobrazí.
   
    ![Okno Protokol aktivit](./media/monitoring-get-started/monitor-act-log-blade.png)
   
    Hello [ **protokol aktivit** ](monitoring-overview-activity-logs.md) popisuje všechny operace provedené na prostředky v rámci vašeho předplatného. Pomocí hello protokolu činnosti, můžete určit hello ", kdo a kdy se pro vytvořené, aktualizovat nebo odstranit operací s prostředky v rámci vašeho předplatného. Například hello aktivity protokolu poznáte, když webové aplikace byla zastavena a kdo byla zastavena. Události protokolu aktivit jsou uloženy v hello platformy a k dispozici tooquery 90 dní.
   
    Můžete vytvořit a uložit dotazy pro běžné filtry a pak pin hello nejdůležitější dotazy tooa řídicí panel portálu, takže budete vždy vědět, pokud došlo k události, které splňují kritéria.
4. Filtrovat hello zobrazení tooa konkrétní skupině zdrojů přes hello minulého týdne a pak klikněte na hello **Uložit** tlačítko.
   
    ![Uložení dotazu protokolu aktivit](./media/monitoring-get-started/monitor-act-log-save.png)
5. Nyní, klikněte na tlačítko hello **Pin** tlačítko.
   
    ![Kliknutí na tlačítko Pin (Připnout) pro protokol aktivit](./media/monitoring-get-started/monitor-act-log-pin.png)
   
    Většina hello zobrazení v tomto názorném postupu může být definovaného tooa řídicího panelu. Díky tomu si můžete vytvořit jediný zdroj informací o provozních datech pro služby. 
6. Vrátí tooyour řídicího panelu. Nyní můžete vidět, že dotaz hello (a počet výsledků) se zobrazí v řídicím panelu. To je užitečné, pokud chcete tooquickly najdete všechny vysoce profil akce, které došlo v poslední době v rámci vašeho předplatného, např. k přiřazení nové role nebo k odstranění virtuálního počítače.
   
    ![Toodashboard protokolu připnutý aktivity](./media/monitoring-get-started/monitor-act-log-db.png)
7. Vrátí toohello **monitorování** dlaždici a klikněte na tlačítko hello **metriky** části. Je nutné nejprve tooselect prostředek filtrování a výběrem pomocí hello rozevíracího seznamu možnosti hello horní části okna hello.
   
    ![Filtr prostředků pro metriky](./media/monitoring-get-started/monitor-met-filter.png)
   
    Všechny prostředky Azure generují [**metriky**](monitoring-overview-metrics.md). V toto zobrazení jsou všechny metriky uvedeny společně v podokně ze skla. Díky tomu je možné snadno zjistit, jak prostředky fungují.
8. Po výběru prostředku, zobrazí se všechny dostupné metriky na levé straně okna hello hello. Můžete najednou grafu více metriky výběrem metriky a upravit graf hello typu a časový rozsah. Můžete také zobrazit všechna upozornění na metriky, která jsou pro tento prostředek nastavená.
   
    ![Okno Metrika](./media/monitoring-get-started/monitor-metric-blade.png)
   
   > [!NOTE]
   > Některé metriky jsou k dispozici jen po povolení [Application Insights](../application-insights/app-insights-overview.md) a případně diagnostiky Azure pro Windows nebo Linux pro příslušný prostředek.
   > 
   > 
9. Pokud vyhovují grafu, můžete použít hello **Pin** toopin tlačítko ji tooyour řídicího panelu.
10. Vrátí toohello **monitorování** a klikněte na **diagnostické protokoly**.
    
    ![Okno Diagnostické protokoly](./media/monitoring-get-started/monitor-diaglogs-blade.png)
    
    [**Diagnostické protokoly** ](monitoring-overview-of-diagnostic-logs.md) protokoly vygenerované *podle* prostředků, které poskytují data o hello operace danému zdroji. Mezi typy diagnostických protokolů patří například počítadla pravidel skupin zabezpečení sítě nebo protokoly pracovního postupu aplikací logiky. Tyto protokoly můžete uložený v účtu storage, přenášené datovými proudy tooan centra událostí nebo odeslat příliš[analýzy protokolů](../log-analytics/log-analytics-overview.md). Log Analytics je produkt Microsoftu, který pracuje s provozními analytickými informacemi a umožňuje pokročilé hledání a generování upozornění.
    
    Hello portálu můžete zobrazit a filtrovat seznam všech prostředků v tooidentify vaše předplatné, pokud mají povolené diagnostické protokoly.
11. Klikněte na prostředek v okně diagnostické protokoly hello. Pokud se diagnostické protokoly ukládají v rámci účtu úložiště, zobrazí se seznam protokolů po hodinách. Tyto protokoly můžete přímo stahovat.
    
    ![Diagnostické protokoly pro jeden prostředek](./media/monitoring-get-started/monitor-diaglogs-detail.png)
    
    Můžete také kliknout na **nastavení pro diagnostiku**, která vám umožní tooset nahoru nebo upravit nastavení pro účet úložiště archivace tooa, streamování tooEvent centra nebo odeslání tooa pracovní prostor analýzy protokolů.
    
    ![Povolení diagnostických protokolů](./media/monitoring-get-started/monitor-diaglogs-enable.png)
    
    Pokud jste nastavili tooLog diagnostické protokoly analýzy, můžete vyhledávat je v hello **hledání protokolů** části portálu hello.
12. Přejděte toohello **výstrahy** části okna monitorování hello.
    
    ![Okno upozornění pro veřejnost](./media/monitoring-get-started/monitor-alerts-nopp.png)
    
    Tady můžete spravovat všechna [**upozornění**](monitoring-overview-alerts.md) týkající se vašich prostředků Azure. To zahrnuje výstrahy na metriky, aktivity protokolu události, testy webu Application Insights (umístění) a proaktivní diagnostics Application Insights. Výstrahy můžete aktivovat e-mailu toobe odeslané nebo adresu URL webhooku tooa HTTP POST.
13. Klikněte na tlačítko **přidat metriky upozornění** toocreate výstrahu.
    
    ![Přidání upozornění metriky](./media/monitoring-get-started/monitor-alerts-add.png)
    
    Můžete pak kódu pin tooeasily výstrahy tooyour řídicí panel stavu najdete v části kdykoli.
14. Hello monitorování část taky obsahuje odkazy příliš[Application Insights](../application-insights/app-insights-overview.md) aplikace a [analýzy protokolů](../log-analytics/log-analytics-overview.md) řešení pro správu. Tyto produkty společnosti Microsoft jsou hluboce integrovány se službou Azure Monitor.
15. Pokud nepoužíváte Application Insights nebo Log Analytics, je pravděpodobné, že je služba Azure Monitor propojená s vašimi aktuálními produkty zajišťujícími monitorování, protokolování a generování upozornění. V tématu naše [partnerů](monitoring-partners.md) pro úplný seznam a pokyny, jak toointegrate.

Pomocí následujících kroků a přichycování všechny řídicí panel tooa příslušné dlaždice, můžete vytvořit komplexní zobrazení vaší aplikace a infrastruktury, jako jsou tato:

![Řídicí panel služby Azure Monitor](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Další kroky
* Čtení hello [monitorování Azure – přehled](monitoring-overview.md)

