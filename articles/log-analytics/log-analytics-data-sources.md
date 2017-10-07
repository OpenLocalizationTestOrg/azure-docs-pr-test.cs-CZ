---
title: zdroje dat aaaConfigure v OMS Log Analytics | Microsoft Docs
description: "Zdroje dat zadat hello data, analýzy protokolů shromažďuje z agentů a dalších připojené zdroje.  Tento článek popisuje koncept hello analýzy protokolů použití zdrojů dat, vysvětluje hello podrobnosti o tom, tooconfigure a poskytuje souhrn hello různé zdroje dat. k dispozici."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: ebe8d29a2442a654b98004f624181ff406868e2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-sources-in-log-analytics"></a>Zdroje dat v analýzy protokolů
Analýzy protokolů shromažďuje data z hello připojené zdroje v pracovním prostoru OMS a ukládá je v úložišti OMS.  Hello data, která se shromažďují z každé je definována hello zdroje dat, který nakonfigurujete.  Data v úložišti OMS hello se ukládají jako sada záznamů.  Všechny zdroje dat, vytvoří záznamy určitého typu pomocí jednotlivých typů má svou vlastní sadu vlastností.

![Přihlaste se shromažďování dat Analytics](./media/log-analytics-data-sources/overview.png)

Zdroje dat se liší od OMS řešení, které také shromažďovat data z připojené zdroje a vytvářet záznamy v úložišti OMS hello.  Řešení mohou být přidány tooyour prostoru z hello Galerie řešení a obvykle poskytne nástrojů pro další analýzu na portálu OMS hello.  

## <a name="summary-of-data-sources"></a>Souhrn datových zdrojů
Hello datových zdrojů, které jsou aktuálně k dispozici v analýzy protokolů jsou uvedeny v následující tabulce hello.  Každá z nich má samostatný článek v tooa odkaz poskytuje podrobnosti pro tento zdroj dat..

| Zdroj dat | Typ události | Popis |
|:--- |:--- |:--- |
| [Vlastní protokoly](log-analytics-data-sources-custom-logs.md) |\<Název protokolu\>_CL |Textové soubory v systému Windows nebo Linux agenty obsahujícího informace o protokolu. |
| [Protokoly událostí systému Windows](log-analytics-data-sources-windows-events.md) |Událost |Události shromážděné z protokolu událostí hello na počítačích s Windows. |
| [Čítače výkonu systému Windows](log-analytics-data-sources-performance-counters.md) |Výkonu |Čítače výkonu shromážděných z počítače se systémem Windows. |
| [Čítače výkonu systému Linux](log-analytics-data-sources-performance-counters.md) |Výkonu |Čítače výkonu shromážděné z počítače se systémem Linux. |
| [Protokoly IIS](log-analytics-data-sources-iis-logs.md) |W3CIISLog |Internetová informační služba protokoly ve formátu W3C. |
| [Syslog](log-analytics-data-sources-syslog.md) |Syslog |Události procesu Syslog na počítačích systému Windows nebo Linux. |

## <a name="configuring-data-sources"></a>Konfigurace zdroje dat
Konfigurace zdroje dat z hello **Data** nabídky v analýzy protokolů **nastavení**.  Jakákoli konfigurace doručuje tooall připojené zdroje v pracovním prostoru OMS.  Veškeré agenty nelze aktuálně vyloučit z této konfigurace.

![Konfigurace události systému Windows](./media/log-analytics-data-sources/configure-events.png)

1. V konzole OMS hello klikněte na tlačítko hello **nastavení** dlaždice nebo hello **nastavení** tlačítko hello horní části úvodní obrazovka.
2. Vyberte **Data**.
3. Klikněte na tooconfigure zdroje dat hello.
4. Postupujte podle dokumentace toohello hello odkaz pro každý zdroj dat v hello výše tabulky Podrobnosti na jejich konfiguraci.

> [!NOTE]
> Momentálně nelze nakonfigurovat zdroje dat analýzy protokolů v hello portálu Azure.

## <a name="data-collection"></a>Shromažďování dat
Konfigurace zdroje dat se dodávají tooagents, které jsou přímo připojené tooLog Analytics během několika minut.  Hello zadaná data se shromažďují z hello agenta a doručeny přímo tooLog Analytics na zdroj dat konkrétní tooeach intervalech.  Každý zdroj dat pro tyto podrobnosti jsou uvedeny v dokumentaci hello.

Konfigurace zdroje dat pro System Center Operations Manager (SCOM) agenty v připojené skupiny pro správu, jsou přeložit na sady management Pack a doručit toohello skupiny pro správu ve výchozím nastavení každých 5 minut.  Hello agent stáhne hello sady management pack jako libovolný jiný a shromažďuje hello zadaná data. V závislosti na hello zdroje dat hello data budou buď odesílání server pro správu tooa, který předává data toohello hello analýzy protokolů nebo hello agent odešle hello data tooLog Analytics bez průchodu přes server pro správu hello. Odkazovat příliš[podrobnosti kolekce dat pro OMS funkcí a řešení](log-analytics-add-solutions.md#data-collection-details) podrobnosti.  Další informace o podrobnosti o připojení SCOM a OMS a úprava frekvence hello tato konfigurace je dodána na [konfigurace integrace s nástrojem System Center Operations Manager](log-analytics-om-agents.md).

Pokud je hello agent nelze tooconnect tooLog analýzy nebo Operations Manager, bude pokračovat toocollect data, která bude poskytovat, když se naváže připojení.  Data mohou být ztracena, pokud hello maximální velikost mezipaměti klienta hello dosáhne hello množství dat, nebo pokud hello agent není možné tooestablish připojení do 24 hodin.

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Všechna data shromažďovaná společností analýzy protokolů je uložen v úložišti OMS hello jako záznamy.  Zaznamenává shromážděné z různých zdrojů dat. bude mít vlastní sadu vlastností a identifikovat podle jejich **typ** vlastnost.  Najdete v dokumentaci hello pro každý zdroj dat a řešení podrobnosti na jednotlivých typů záznamu.

## <a name="next-steps"></a>Další kroky
* Další informace o [řešení](log-analytics-add-solutions.md) , přidejte tooLog funkce analýzy a také shromažďovat data do úložiště OMS hello.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.  
* Konfigurace [výstrahy](log-analytics-alerts.md) tooproactively upozorňují na důležité data shromážděná ze zdrojů dat a řešení.
