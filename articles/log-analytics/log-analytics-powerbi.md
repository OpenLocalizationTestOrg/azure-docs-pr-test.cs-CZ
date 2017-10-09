---
title: "aaaExport analýzy protokolů data tooPower BI | Microsoft Docs"
description: "Power BI je služba cloudové obchodní analýzy od společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.  Analýzy protokolů můžete nepřetržitě exportovat data z úložiště OMS hello do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.  Tento článek popisuje, jak tooconfigure dotazy v analýzy protokolů, které automaticky exportovat tooPower BI v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 4822f99677e5d1080c72e95cda410da81615bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="export-log-analytics-data-toopower-bi"></a>Export dat tooPower analýzy protokolů BI

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak tento proces pro export dat tooPower analýzy protokolů BI přestane fungovat.  Všechny existující plány, které jste vytvořili před upgradem bude zakázán. 
>
> Po upgradu, hello Azure Log Analytics používá stejnou platformu jako Application Insights a použijete stejný proces tooexport analýzy protokolů dotazy tooPower BI jako hello [hello proces tooexport Application Insights dotazuje tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  Můžete buď vyexportovat hello dotazu pomocí hello Analytics konzole, jak je popsáno v tomto článku, nebo můžete vybrat hello **Power BI** tlačítko hello horní části obrazovky hello hello hledání protokolů portálu.



[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) je cloudové obchodní analýzy služba společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.  Analýzy protokolů můžete automaticky exportovat data z úložiště OMS hello do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.

Když nakonfigurujete Power BI s analýzy protokolů, můžete vytvořit dotazy protokolu, které se export datových sad jejich výsledky toocorresponding v Power BI.  Hello dotazu a export pokračuje tooautomatically spustit podle plánu, který definujete datovou sadu hello tookeep až toodate s hello nejnovější data shromažďovaná společností analýzy protokolů.

![Log Analytics tooPower BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Plány Power BI
A *Power BI plán* zahrnuje hledání protokolů, který vyexportuje sadu dat z hello OMS úložiště tooa odpovídající datovou sadu v Power BI a plán, který definuje, jak často tohoto hledání běží tookeep hello dataset aktuální.

Hello pole v datové sadě hello bude odpovídat vlastnosti hello hello záznamů vrácených hledání protokolů hello.  Pokud hledání hello vrátí záznamy různých typů pak datovou sadu hello bude obsahovat všechny vlastnosti hello z každé hello zahrnuty typů záznamů.  

> [!NOTE]
> Je nejlepší postup toouse dotaz vyhledávání protokolu, který vrátí nezpracovaná data jako názvem na rozdíl od tooperforming žádné konsolidace pomocí příkazů jako třeba [měr](log-analytics-search-reference.md#measure).  Všechny agregace a výpočty můžete provádět v Power BI od nezpracovaných dat hello.
>
>

## <a name="connecting-oms-workspace-toopower-bi"></a>Připojení tooPower pracovní prostor OMS BI
Před exportem z analýzy protokolů tooPower BI, je nutné připojit účtu Power BI tooyour pracovní prostor OMS pomocí hello následující postup.  

1. V konzole OMS hello klikněte na tlačítko hello **nastavení** dlaždici.
2. Vyberte **účty**.
3. V hello **informace o pracovním prostoru** klikněte na část **připojit tooPower účet BI**.
4. Zadejte hello přihlašovací údaje pro svůj účet Power BI.

## <a name="create-a-power-bi-schedule"></a>Vytvoření plánu Power BI
Vytvořte plán Power BI pro každé datové sady pomocí hello následující postup.

1. V konzole OMS hello klikněte na tlačítko hello **hledání protokolů** dlaždici.
2. Zadejte nový dotaz nebo vyberte uloženého hledání, vrátí hello dat, které chcete tooexport příliš**Power BI**.  
3. Klikněte na tlačítko hello **Power BI** tlačítko hello horní části hello tooopen stránku hello **Power BI** dialogové okno.
4. Poskytování informací hello v hello následující tabulky a klikněte na tlačítko **Uložit**.

| Vlastnost | Popis |
|:--- |:--- |
| Name (Název) |Název tooidentify hello plán při zobrazení seznamu hello plány Power BI. |
| Uložené hledání |toorun vyhledávání protokolu Hello.  Můžete buď vybrat hello aktuální dotaz, nebo vyberte existující uložené hledání rozevíracím hello. |
| Plán |Jak často toorun hello uložené vyhledávání a export datovou sadu toohello Power BI.  Hello hodnota musí být mezi 15 minutami a 24 hodin. |
| Název datové sady |Název Hello hello datovou sadu v Power BI.  Bude vytvořen, pokud neexistuje a aktualizovat, pokud neexistuje. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Zobrazení a odebrání plány Power BI
Zobrazení seznamu hello existující plán Power BI s hello následující postup.

1. V konzole OMS hello klikněte na tlačítko hello **nastavení** dlaždici.
2. Vyberte **Power BI**.

Kromě toho toohello podrobnosti o hello naplánovat, hello počet pokusů, které hello plán byla spuštěna v hello minulého týdne a jsou zobrazeny hello stav poslední synchronizace hello.  Pokud hello synchronizace došlo k chybám, můžete kliknout na odkaz toorun hello hledání protokolů pro záznamy podrobné informace o této chybě hello.

Plán můžete odebrat tak, že kliknete na hello **X** v hello **odebrat sloupec**.  Plán můžete zakázat tak, že vyberete **vypnout**.  toomodify plánu, musíte ho odebrat a znovu ji vytvořte s novým nastavením hello.

![Plány Power BI](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Ukázka návod
Hello následující části vás provede příklad vytvoření plánu Power BI a pomocí jeho toocreate datovou sadu jednoduchou sestavu.  V tomto příkladu všechny údaje o výkonu pro skupinu počítačů, je exportovaný tooPower BI a pak spojnicový graf je vytvořen toodisplay využití procesoru.

### <a name="create-log-search"></a>Vytvoření hledání protokolů
Začneme vytvořením hledání protokolů pro hello data, že má být toosend toohello datovou sadu.  V tomto příkladu použijeme dotaz, který vrátí všechny údaje o výkonu pro počítače s názvem, které začíná *srv*.  

![Plány Power BI](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Vytvoření hledání Power BI
Kliknete na hello **Power BI** tlačítko tooopen hello Power BI dialogové okno a zadejte hello požadované informace.  Jsme chcete toto hledání toorun jednou za hodinu a vytvořit datovou sadu s názvem *Contoso výkonu*.  Vzhledem k tomu, že již máme otevřete vyhledávání hello, která vytvoří hello dat chceme, jsme zachovat výchozí hello *použít aktuální vyhledávací dotaz* pro **uložené výsledky hledání**.

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Ověřte Power BI vyhledávání
tooverify, že jsme vytvořili plán hello správně, nemůžeme zobrazit hello seznam hledání Power BI pod hello **nastavení** dlaždici na řídicím panelu hello OMS.  Jsme Počkejte několik minut a aktualizujte toto zobrazení, dokud hlásí, že byla spuštěna synchronizace hello.

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-hello-dataset-in-power-bi"></a>Ověření datové sady hello v Power BI
Jsme Přihlaste se k naší účtu v [powerbi.microsoft.com](http://powerbi.microsoft.com/) a posuňte se příliš**datové sady** dole hello v levém podokně hello.  Uvidíte, že hello *Contoso výkonu* datové sady je uveden indikující, že naše exportu byla úspěšně spuštěna.

![Datová sada Power BI](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Vytvoření sestavy založené na datové sady
Jsme vyberte hello **Contoso výkonu** datovou sadu a potom klikněte na **výsledky** v hello **pole** podokně na hello správné tooview hello pole, které jsou součástí této datové sady.  toocreate řádku graf zobrazující využití procesoru pro každý počítač, můžeme provést hello následující akce.

1. Vyberte hello řádku grafu vizualizace.
2. Přetáhněte **ObjectName** příliš**filtr úrovně sestav** a zkontrolujte **procesoru**.
3. Přetáhněte **název_čítače** příliš**filtr úrovně sestav** a zkontrolujte **% času procesoru**.
4. Přetáhněte **přepočtené** příliš**hodnoty**.
5. Přetáhněte **počítače** příliš**legendy**.
6. Přetáhněte **TimeGenerated** příliš**osy**.

Vidíme, že hello výsledné spojnicový graf se zobrazí hello daty z naší datové sadě.

![Power BI spojnicový graf](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-hello-report"></a>Uloží sestavu hello
Nemůžeme uložit hello sestavy kliknutím na hello tlačítko hello horní části obrazovky hello uložit a ověřit, že je teď uvedený v části sestavy hello v levém podokně hello.

![Sestavy Power BI](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) toobuild dotazy, které se dají exportovat tooPower BI.
* Další informace o [Power BI](http://powerbi.microsoft.com) toobuild vizualizace podle exportuje analýzy protokolů.
