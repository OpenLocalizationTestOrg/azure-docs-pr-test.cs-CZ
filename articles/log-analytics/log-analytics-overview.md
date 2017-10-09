---
title: "aaaWhat je analýzy protokolů v Operations Management Suite (OMS)? | Dokumentace Microsoftu"
description: "Log Analytics je služba v rámci sady Operations Management Suite (OMS), která pomáhá shromažďovat a analyzovat provozní data vygenerovaná prostředky ve vašem cloudovém a místním prostředí.  Tento článek poskytuje stručný přehled různých komponent hello analýzy protokolů a obsahu toodetailed odkazy."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: bd90b460-bacf-4345-ae31-26e155beac0e
ms.service: log-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2017
ms.author: bwren
ms.openlocfilehash: b35da77e3782f7c1fe54cc34142f8cd9f2eb57d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-log-analytics"></a>Co je služba Log Analytics?
Analýzy protokolů je služba v [Operations Management Suite \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) , sleduje vaše cloudové a místní prostředí toomaintain jejich dostupnost a výkon.  Shromáždí data generována prostředky ve vašich cloudových a místních prostředích a z dalších monitorování tooprovide analysis nástroje napříč více zdrojů.  Tento článek obsahuje stručný popis hello hodnotu, že Log Analytics poskytuje, základní informace o tom, jak funguje, a odkazy toomore podrobné obsah, můžete dále prozkoumat.

## <a name="is-log-analytics-for-you"></a>Je pro vás služba Log Analytics vhodná?
Pokud ještě nemáte pro prostředí Azure nastavené žádné monitorování, měli byste začít s [Azure Monitorem](../monitoring-and-diagnostics/monitoring-overview.md), který shromažďuje a analyzuje data monitorování prostředků Azure.  Analýzy protokolů můžete [shromažďování dat z Azure monitorování](log-analytics-azure-storage.md) toocorrelate ji s ostatními daty a poskytnout další analýzu.

Pokud chcete toomonitor v místním prostředí nebo máte existující monitorování pomocí služby, například Azure monitorování nebo System Center Operations Manager, můžete přidat analýzy protokolů významné zlepšení.  Může do jednoho úložiště shromažďovat data přímo z agentů a také z těchto dalších nástrojů.  Analytické nástroje služby Log Analytics, jako je prohledávání protokolu, zobrazení a řešení, využívají všechna shromážděná data a poskytují centralizovanou analýzu celého prostředí.


## <a name="using-log-analytics"></a>Použití Log Analytics
Analýzy protokolů můžete přistupovat prostřednictvím portálu OMS hello nebo hello portálu Azure, který běží v libovolného prohlížeče a poskytnout vám s nastavením tooconfiguration přístup a více tooanalyze nástroje a fungovat na shromážděná data.  Z portálu hello můžete využít [protokolu hledání](log-analytics-log-searches.md) tam, kde vytvoříte dotazy tooanalyze shromažďovaných dat, [řídicí panely](log-analytics-dashboards.md) které můžete přizpůsobit pomocí grafické zobrazení hledání nejvíc hodí v situaci, a [řešení](log-analytics-add-solutions.md) která poskytnout další funkce a analytických nástrojích.

Následující obrázek Hello je z portálu OMS hello hello řídicí panel které ukazuje, který zobrazí souhrnné informace o hello [řešení](#add-functionality-with-management-solutions) , které se nainstalují v prostoru hello.  Kliknutím na další toodrill všechny dlaždice do hello data pro toto řešení.

![Portál OMS](media/log-analytics-overview/portal.png)

Analýzy protokolů zahrnuje načtení tooquickly jazyk dotazu a slučování dat v úložišti hello.  Můžete vytvořit a uložit [protokolu hledání](log-analytics-log-searches.md) toodirectly analyzovat data v portálu hello nebo protokolu hledání spustili automaticky toocreate výstrahu v případě hello výsledky dotazu hello naznačují podmínku důležité.

![Prohledávání protokolů](media/log-analytics-overview/log-search.png)

tooget rychlé grafické zobrazení stavu hello celkové prostředí, můžete přidat vizualizace pro uložený protokol hledání tooyour [řídicí panel](log-analytics-dashboards.md).   

![Řídicí panel](media/log-analytics-overview/dashboard.png)

V pořadí tooanalyze data mimo analýzy protokolů, můžete exportovat hello dat z úložiště OMS hello do nástroje, jako [Power BI](log-analytics-powerbi.md) nebo v aplikaci Excel.  Můžete také využít hello [rozhraní API pro vyhledávání protokolu](log-analytics-log-search-api.md) toobuild vlastní řešení, které využívají data Log Analytics nebo toointegrate s jinými systémy.

## <a name="add-functionality-with-management-solutions"></a>Přidání funkcí s využitím řešení pro správu
[Řešení pro správu](log-analytics-add-solutions.md) přidat funkce tooOMS, poskytuje další data a analýza nástroje tooLog Analytics.  Také se může definovat nové typy záznamů toobe shromážděných, který lze analyzovat pomocí protokolu hledání nebo další uživatelské rozhraní služby hello řešení hello řídicím panelu.  Následující obrázek Hello příklad ukazuje hello [řešení pro sledování změn](log-analytics-change-tracking.md)

![Řešení pro sledování změn](media/log-analytics-overview/change-tracking.png)

Řešení jsou k dispozici pro celou řadu funkcí a průběžně se přidávají další řešení.  Můžete snadno procházet dostupná řešení a [je přidat pracovní prostor OMS tooyour](log-analytics-add-solutions.md) z Galerie řešení hello nebo Azure Marketplace.  Řada jich bude nasazených automaticky a začnou fungovat hned, zatímco jiná můžou vyžadovat určitou konfiguraci.

![Galerie řešení](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-components"></a>Komponenty služby Log Analytics
V hello center Log Analytics je hello OMS úložiště, který je hostován v hello cloudu Azure.  Data jsou shromažďována do úložiště hello z připojených zdrojů tak, že konfigurace zdroje dat a přidání předplatného tooyour řešení.  Zdroje dat a řešení se vytvoří každý jiný záznam typy, které mají své vlastní sadu vlastností, ale může stále analyzovat společně v úložišti toohello dotazy.  To vám umožní toouse hello stejné nástroje a metody toowork s různými druhy dat shromažďovaných pomocí různých zdrojů.

![Úložiště OMS](media/log-analytics-overview/overview.png)

Připojené zdroje jsou hello počítače a další prostředky, které generují data shromažďovaná společností analýzy protokolů.  Můžou sem patřit agenti nainstalovaní na počítačích s [Windows](log-analytics-windows-agents.md) a [Linuxem](log-analytics-linux-agents.md), kteří se připojují přímo, nebo agenti v [připojené skupině pro správu System Center Operations Manageru](log-analytics-om-agents.md).  V případě prostředků Azure služba Log Analytics shromažďuje data z [Azure Monitoru a Azure Diagnostics](log-analytics-azure-storage.md).

[Zdroje dat](log-analytics-data-sources.md) jsou hello různé druhy data shromážděná z každého připojeného zdroje.  To zahrnuje [události](log-analytics-data-sources-windows-events.md) a [údaje o výkonu](log-analytics-data-sources-performance-counters.md) z [Windows](log-analytics-data-sources-windows-events.md) a agenty Linux v přidání toosources jako [protokoly služby IIS](log-analytics-data-sources-iis-logs.md)a [vlastní textové protokoly](log-analytics-data-sources-custom-logs.md).  Nakonfigurujete každý zdroj dat, že chcete toocollect a konfigurace hello je automaticky doručené tooeach připojené zdroje.

Pokud máte vlastní požadavky, pak můžete použít hello [rozhraní API sady kolekcí dat protokolu HTTP](log-analytics-data-collector-api.md) úložiště toohello toowrite data z klienta pro REST API.

## <a name="log-analytics-architecture"></a>Architektura služby Log Analytics
požadavky na nasazení Hello Log Analytics jsou minimální, protože hello centrální součásti jsou hostované v cloudu Azure hello.  To zahrnuje hello úložiště v přidání toohello služby, které umožňují toocorrelate a analyzovat shromážděná data.  Hello portálu budou mít přístup z libovolného prohlížeče, takže není nutné pro klientský software.

Musíte nainstalovat agenty na počítače s [Windows](log-analytics-windows-agents.md) a [Linuxem](log-analytics-linux-agents.md), ale nepožaduje se žádný další agent pro počítače, které jsou už členy [připojené skupiny pro správu SCOM](log-analytics-om-agents.md).  Agenti SCOM bude toocommunicate se servery pro správu, které bude předávat jejich data tooLog Analytics.  Některá řešení, když bude vyžadovat agenty toocommunicate přímo s analýzy protokolů.  Hello dokumentace pro každé řešení určit své požadavky na komunikaci.

Když [se zaregistrujete ke službě Log Analytics](log-analytics-get-started.md), vytvoří se pracovní prostor OMS.  Pracovní prostor hello si lze představit jako jedinečné prostředí analýzy protokolů s vlastní úložiště dat, zdroje dat a řešení. Může vytvořit více pracovní prostory v vaše předplatné toosupport více prostředích, jako je produkční a testování.

![Architektura služby Log Analytics](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Další kroky
* [Zaregistrovat bezplatný účet analýzy protokolů](log-analytics-get-started.md) tootest ve svém vlastním prostředí.
* Zobrazení hello různých [zdroje dat](log-analytics-data-sources.md) dostupné toocollect data do úložiště OMS hello.
* [Procházet hello k dispozici řešení v hello řešení Galerie](log-analytics-add-solutions.md) tooadd funkce tooLog Analytics.

