---
title: "aaaMonitor a Správa kanálů data - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello monitorování a správu toomonitor aplikace a správa Azure data Factory a kanály."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: 5e4ef6ec5fb8ebc9bda0be7899a39a51d58403d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-monitoring-and-management-app"></a>Monitorování a Správa kanálů služby Azure Data Factory pomocí monitorování a správu aplikace hello
> [!div class="op_single_selector"]
> * [Použití Azure portal nebo Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Pomocí monitorování a správu aplikací](data-factory-monitor-manage-app.md)
>
>

Tento článek popisuje, jak toouse hello monitorování a správu toomonitor aplikace, spravovat a ladit kanály Data Factory. Nabízí taky informace o tom, jak toocreate výstrahy tooget upozornění na selhání. Můžete začít s pomocí aplikace hello podle sledováním hello následující video:

> [!NOTE]
> uživatelské rozhraní Hello ukazuje hello video nemusí přesně odpovídat najdete v portálu hello. Je mírně starší, ale koncepty zůstanou hello stejné. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-hello-monitoring-and-management-app"></a>Spustit sledování a správu aplikace hello
toolaunch hello monitorování a správu aplikací, klikněte na tlačítko hello **monitorování a správa** na hello dlaždici **Data Factory** okno objektu pro vytváření dat.

![Monitorování dlaždice na domovské stránce objektu pro vytváření dat hello](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

Měli byste vidět monitorování a správu aplikace hello otevře v samostatném okně.  

![Monitorování a správa aplikací](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> Pokud se zobrazí, že hello webový prohlížeč zasekl ve fázi "autorizace …", zrušte hello **blokovat soubory cookie třetích stran a data lokality** políčko--nebo udržování je vybrána, vytvořte výjimku pro **login.microsoftonline.com** a akci opakujte tooopen hello aplikace.


V seznamu aktivity Windows hello v prostředním podokně hello zobrazí okno s aktivity pro každé spuštění aktivity. Například pokud máte pět hodin hello naplánované aktivity toorun každou hodinu, uvidíte pět okna aktivity spojené s pěti datové řezy. Pokud nevidíte aktivity windows hello seznamu dole v hello, hello následující:
 
- Aktualizace hello **počáteční čas** a **čas ukončení** filtrů v horní toomatch hello hello spuštění a ukončení vašeho kanálu a klikněte hello **použít** tlačítko.  
- seznam aktivity Windows Hello automaticky neobnoví. Klikněte na tlačítko hello **aktualizovat** tlačítka na panelu nástrojů hello v hello **aktivity Windows** seznamu.  

Pokud nemáte tootest aplikace Data Factory tyto kroky hello kurz: [kopírování dat z úložiště objektů Blob tooSQL databázi pomocí služby Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="understand-hello-monitoring-and-management-app"></a>Pochopení hello monitorování a Správa aplikací
Na levé straně hello jsou tři karty: **Průzkumníka prostředků**, **monitorování zobrazení**, a **výstrahy**. první kartě Hello (**Průzkumníka prostředků**) je standardně vybraná.

### <a name="resource-explorer"></a>Průzkumník prostředků
Zobrazí hello následující:

* Hello Průzkumníka prostředků **stromovém zobrazení** v levém podokně hello.
* Hello **zobrazení diagramu** v horní části hello v prostředním podokně hello.
* Hello **aktivity Windows** seznam v dolní části hello v prostředním podokně hello.
* Hello **vlastnosti**, **aktivity okno Průzkumníka**, a **skriptu** karty v pravém podokně hello.

V Průzkumníku prostředků zobrazí všechny prostředky (kanály, datové sady, propojených služeb) v objektu pro vytváření dat hello ve stromovém zobrazení. Když vyberete objekt v Průzkumníku prostředků:

* Hello související objekt pro vytváření dat entity je označený na hello zobrazení diagramu.
* [Související aktivity windows](data-factory-scheduling-and-execution.md) jsou vyznačené na seznamu aktivity Windows hello dolnímu hello.  
* Hello vlastnosti hello vybraný objekt jsou zobrazeny v okně Vlastnosti hello v pravém podokně hello.
* Hello definici JSON hello vybraného objektu se zobrazí, pokud je k dispozici. Příklad: propojené služby, datové sady nebo kanálu.

![Průzkumník prostředků](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

V tématu hello [plánování a provádění](data-factory-scheduling-and-execution.md) článku podrobné koncepční informace o windows aktivity.

### <a name="diagram-view"></a>Zobrazení diagramu
Hello zobrazení diagramu objektu pro vytváření dat poskytuje jedno podokno pohotovostní toomonitor a spravovat objekt pro vytváření dat a její prostředky. Když vyberete entity služby Data Factory (datové sady nebo kanál) v hello zobrazení diagramu:

* v zobrazení stromu hello je vybrána entity objektu pro vytváření dat Hello.
* Hello související aktivity, které windows jsou vyznačené na seznamu aktivity Windows hello.
* Hello vlastnosti vybraného objektu hello se zobrazí v okně Vlastnosti hello.

Když hello kanál je povolené (ne v pozastaveném stavu), zobrazí se zelený řádku:

![Spuštění kanálu](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Můžete pozastavit, obnovit nebo ukončit kanál tak, že ho vyberete v zobrazení diagramu hello a použití hello tlačítek na panelu příkazů hello.

![Pozastavení nebo obnovení na panelu příkazů hello](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
Existují tři panelu příkazů pro kanál hello v hello zobrazení diagramu. Můžete použít hello druhý tlačítko toopause hello kanálu. Pozastavení není ukončení aktuálně spuštěných aktivity hello a umožňuje jim pokračovat toocompletion. třetí tlačítko Hello pozastaví hello kanálu a ukončí její existující provádění aktivity. první tlačítko Hello obnoví hello kanálu. Pokud svůj kanál je pozastavena, změní barvu hello hello kanálu. Například pozastavený kanálu vypadá v hello následující bitové kopie: 

![Pozastavená kanálu](./media/data-factory-monitor-manage-app/PipelinePaused.png)

Pomocí klávesy Ctrl hello můžete vybrat víc dva nebo víc kanálů. Příkaz hello panelu tlačítka toopause/obnovení můžete použít více kanálů najednou.

Můžete také kliknout pravým tlačítkem kanálu a zvolit možnosti toosuspend obnovit nebo ukončit kanál. 

![Kontextové nabídky pro kanál](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

Klikněte na tlačítko hello **otevřít kanál** možnost toosee všechny hello aktivity v kanálu hello. 

![Nabídka Otevřít kanál](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

V zobrazení hello otevřít kanál zobrazí všechny aktivity v kanálu hello. V tomto příkladu je jenom jedna aktivita: aktivity kopírování. 

![Otevřenou kanálu](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

toogo zpět toohello předchozího zobrazení, klikněte na název objektu pro vytváření dat hello v nabídce hello s popisem cesty v horní části hello.

V zobrazení kanálu hello Pokud vyberete datovou sadu výstupů nebo když přesunutím ukazatele myši nad hello výstupní datovou sadu, zobrazí místní okno aktivity Windows hello pro tuto datovou sadu.

![Místní okno Windows aktivity](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Klepnutím podrobnosti o aktivitě okno toosee pro něj v hello **vlastnosti** okno v pravém podokně hello.

![Vlastnosti – okno](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

V pravém podokně hello přepínač toohello **aktivity okno Průzkumníka** kartě toosee další podrobnosti.

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

Zobrazí také **přeložit proměnné** pro každý pokus o spuštění pro aktivitu v hello **pokusy o** části.

![Vyřešený proměnné](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Přepínač toohello **skriptu** kartě toosee hello JSON skriptu definice pro vybraný objekt hello.   

![Karta skriptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

Zobrazí okna aktivity na třech místech:

* Hello aktivity Windows automaticky otevírané okno v hello zobrazení diagramu (střední podokno).
* v pravém podokně hello Hello aktivity okno Průzkumníka.
* seznam aktivity Windows Hello v dolním podokně hello.

V místní nabídce Aktivita Windows hello a aktivity okno Průzkumníka se toohello posunete předchozího týdne a hello příští týden pomocí hello levou a pravou šipku.

![Aktivita okno Průzkumníka levé nebo pravé šipky](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Na hello dolní části hello zobrazení diagramu, zobrazí tato tlačítka: Přiblížit, oddálit, tooFit přiblížení či oddálení, zvětšení 100 %, rozložení zámku. Hello **zámku rozložení** tlačítko zabraňuje nechtěnému přesunutí tabulek a kanálů v hello zobrazení diagramu. Ve výchozím nastavení je. Můžete ho vypnout a pohyb entity v diagramu hello. Pokud funkci vypnete, můžete použít hello poslední tlačítko tooautomatically pozice tabulky a kanály. Můžete také přiblížení nebo oddálení pomocí kolečka myši hello.

![Diagram příkazy přiblížení zobrazení](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a>Seznam aktivit Windows
Zobrazí se seznam aktivity Windows Hello dole hello v prostředním podokně hello všechny aktivity windows hello datové sady, který jste vybrali v hello Průzkumníka prostředků nebo hello zobrazení diagramu. Ve výchozím nastavení seznam hello je v sestupném pořadí, což znamená, najdete v části hello nejnovější okně aktivita v horní části hello.

![Seznam aktivit Windows](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Tento seznam neobnovuje automaticky, takže jej aktualizovat, použijte tlačítko Aktualizovat hello na panelu nástrojů toomanually hello.  

Okna aktivity může být v jednom z hello následující stavy:

<table>
<tr>
    <th align="left">Status</th><th align="left">Podřízený stav</th><th align="left">Popis</th>
</tr>
<tr>
    <td rowspan="8">Čekání</td><td>ScheduleTime</td><td>pro hello aktivity okno toorun ještě nenastal čas Hello.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Hello upstreamové závislosti nejsou připravené.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Hello výpočetní prostředky nejsou k dispozici.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Všechny instance aktivit hello je zaneprázdněn spouštěním další aktivity windows.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Hello aktivita je pozastavená a aktivity windows hello nelze spustit, dokud nebude obnovená.</td>
</tr>
<tr>
<td>Opakování</td><td>Probíhá pokus o spuštění aktivity Hello je zopakován.</td>
</tr>
<tr>
<td>Ověření</td><td>Ověření se ještě nespustilo.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Ověření je čekání toobe opakovat.</td>
</tr>
<tr>
<tr>
<td rowspan="2">InProgress</td><td>Probíhá ověřování</td><td>Probíhá ověřování.</td>
</tr>
<td>-</td>
<td>okno aktivity Hello je zpracovávána.</td>
</tr>
<tr>
<td rowspan="4">Se nezdařilo</td><td>TimedOut</td><td>provedení aktivity Hello trvalo déle, než je povolené aktivitou hello.</td>
</tr>
<tr>
<td>Zrušeno</td><td>okno aktivity Hello zrušil akce uživatele.</td>
</tr>
<tr>
<td>Ověření</td><td>Ověření se nezdařilo.</td>
</tr>
<tr>
<td>-</td><td>Hello aktivity okna se nezdařila toobe generování nebo ověřování.</td>
</tr>
<td>Připraveno</td><td>-</td><td>okno aktivity Hello je připraven ke spotřebování.</td>
</tr>
<tr>
<td>Přeskočena</td><td>-</td><td>okno aktivity Hello nebyla zpracována.</td>
</tr>
<tr>
<td>Žádný</td><td>-</td><td>Okno s aktivita používá tooexist jiný stav, ale byl obnoven.</td>
</tr>
</table>


Po kliknutí na tlačítko okno s aktivity v seznamu hello, můžete zobrazit podrobnosti o ho v hello **aktivity Windows Explorer** nebo hello **vlastnosti** okno na hello správné.

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Aktualizujte windows aktivity
Podrobnosti Hello se neobnovily automaticky, proto použijte tlačítko Aktualizovat hello (hello druhé tlačítko) na hello příkazovém řádku seznam windows toomanually aktualizace hello aktivit.  

### <a name="properties-window"></a>Vlastnosti – okno
v podokně nejvíce vpravo hello hello monitorování a správu aplikace je Hello vlastnosti – okno.

![Vlastnosti – okno](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Zobrazí vlastnosti pro hello položku, kterou jste vybrali v hello Průzkumníka prostředků (stromové zobrazení), zobrazení diagramu nebo seznamu aktivity Windows.

### <a name="activity-window-explorer"></a>Okno Průzkumníka aktivity
Hello **aktivity okno Průzkumníka** je okno hello pravé podokno hello monitorování a správu aplikace. Zobrazuje podrobnosti o okně hello aktivity, který jste vybrali v místním okně aktivity Windows hello nebo seznamu aktivity Windows hello.

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Kliknutím na zobrazení kalendáře hello v horní části hello můžete přepnout tooanother aktivity okno. Můžete také pomocí tlačítek hello šipku vlevo nebo vpravo v hello nejvyšší toosee aktivity windows z hello předchozího týdne nebo hello příští týden.

Můžete použít hello tlačítka panelu nástrojů v okně aktivita hello dolní podokno toorerun hello nebo aktualizovat hello podrobnosti v podokně hello.

### <a name="script"></a>Skript
Můžete použít hello **skriptu** kartě tooview hello JSON definice hello vybrané entity služby Data Factory (propojené služby, datové sady nebo kanál).

![Karta skriptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a>Pomocí zobrazení systému
Hello monitorování a správu aplikací obsahuje zobrazení předdefinovaných systému (**poslední aktivity windows**, **se nezdařilo aktivity windows**, **probíhající aktivity windows**), umožní tooview poslední nebo se nezdařilo nebo probíhající aktivity windows pro datovou továrnu.

Přepínač toohello **monitorování zobrazení** karty na levé straně hello kliknutím.

![Zobrazení Karta sledování](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

V současné době jsou tři zobrazení systému, které jsou podporovány. Vyberte toosee možnost poslední aktivity windows, windows neúspěšné aktivity nebo okna probíhající aktivity v seznamu aktivity Windows hello (dole hello prostředním podokně hello).

Když vyberete hello **poslední aktivity windows** možnost zobrazí všechny poslední aktivity windows v sestupném pořadí podle hello **čas posledního pokusu**.

Můžete použít hello **se nezdařilo aktivity windows** zobrazit v seznamu hello okna aktivity toosee se nezdařilo. Vyberte časové období neúspěšné aktivity v hello seznamu toosee jeho podrobnosti v hello **vlastnosti** okno nebo hello **aktivity okno Průzkumníka**. Můžete také stáhnout všechny protokoly pro okno neúspěšné aktivity.

## <a name="sort-and-filter-activity-windows"></a>Třídění a filtrování aktivity windows
Změna hello **počáteční čas** a **čas ukončení** nastavení v hello příkazovém řádku windows toofilter aktivity. Po změně hello počáteční a koncový čas, klikněte na tlačítko hello tlačítko Další toohello koncový čas toorefresh hello aktivity seznamu Windows.

![Počáteční a koncový čas](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> V současné době všechny časy jsou ve formátu UTC hello monitorování a správu aplikací.
>
>

V hello **seznamu aktivity Windows**, klikněte na název sloupce hello (například: stav).

![Aktivity Windows seznamu sloupec nabídky](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Můžete provést následující hello:

* Seřadit ve vzestupném pořadí.
* Řazení v sestupném pořadí.
* Filtrovat podle jednu nebo více hodnot (připravené, čekání a tak dále).

Když zadáte filtr na sloupci, zobrazí tlačítko Filtrovat hello povolené pro daný sloupec, který označuje, že hello hodnoty ve sloupci hello jsou filtrované hodnoty.

![Filtrování podle sloupce seznamu aktivity Windows hello](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Můžete použít hello stejné filtry tooclear automaticky otevírané okno. tooclear všechny filtry pro seznamu aktivity Windows hello, klikněte na tlačítko Vymazat filtr hello na panelu příkazů hello.

![Zruší všechny filtry pro seznam aktivity Windows hello](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a>Udělejte batch
### <a name="rerun-selected-activity-windows"></a>Znovu spustit vybranou aktivitou windows
Vyberte okno s aktivity, klikněte na tlačítko hello hello první příkazového řádku tlačítka šipka dolů a vyberte **spusťte znovu** / **znovu spustit s proti proudu v kanálu**. Když vyberete hello **znovu spustit s proti proudu v kanálu** možnost, se znovu spustí všechna okna nadřízené činnosti také.
    ![Spusťte okno s aktivity](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Můžete také vybrat více aktivity windows hello seznamu a znovu je v hello stejnou dobu. Můžete chtít windows toofilter aktivity na základě stavu hello (například: **se nezdařilo**) – a poté znovu spusťte windows hello se nezdařilo aktivity po vyřešení potíží hello, který způsobuje, že toofail windows hello aktivity. Najdete v následující části Podrobnosti o filtrování aktivity windows hello seznamu hello.  

### <a name="pauseresume-multiple-pipelines"></a>Pozastavení nebo obnovení více kanálů
Pomocí klávesy Ctrl hello můžete vícenásobného výběru dva nebo víc kanálů. Můžete použít tlačítka na panelu příkazů hello (které jsou vyznačené na hello red obdélníku v hello následující obrázek) toopause nebo obnovení je.

![Pozastavení nebo obnovení na panelu příkazů hello](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a>Vytváření upozornění
Hello **výstrahy** stránky umožňuje vytvářet výstrahy a zobrazení, úpravy nebo odstranění existující výstrahy. Vám může také zapnout/vypnout výstrahu. toosee hello stránky výstrah, klikněte na tlačítko hello **výstrahy** kartě.

![Karta výstrahy](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="toocreate-an-alert"></a>toocreate výstrahu
1. Klikněte na tlačítko **přidat výstraha** tooadd výstrahu. Zobrazí hello **podrobnosti** stránky.

    ![Vytvářet výstrahy - stránce s podrobnostmi o](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. Zadejte hello **název** a **popis** hello výstrahu a klikněte na **Další**. Měli byste vidět hello **filtry** stránky.

    ![Vytvořte upozornění – filtry stránky](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. Vyberte hello **událostí**, **stav**, a **substatus** (volitelné), které chcete toocreate služba Data Factory pro výstrahy a klikněte na tlačítko **Další**. Měli byste vidět hello **příjemce** stránky.

    ![Vytvořte upozornění – stránka příjemce](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. Vyberte hello **e-mailem správci předplatného** možnost a zadejte **další správce e-mailu**a klikněte na tlačítko **Dokončit**. Měli byste vidět hello výstrahu v seznamu hello.

    ![Seznam výstrah](./media/data-factory-monitor-manage-app/AlertsList.png)

V seznamu výstrah hello použijte hello tlačítka, které jsou přidruženy hello výstrahy tooedit/odstranění/zapnout/vypnout výstrahu.

### <a name="eventstatussubstatus"></a>Události, stav/substatus
Hello následující tabulka obsahuje seznam hello k dispozici události a stavy (a dílčí stavy).

| Název události | Status | Podřízený stav |
| --- | --- | --- |
| Aktivity při spuštění Začínáme |spuštění |Spouštění |
| Aktivity při spuštění bylo dokončeno |Úspěch |Úspěch |
| Aktivity při spuštění bylo dokončeno |Se nezdařilo |Přidělení prostředků se nezdařilo<br/><br/>Spuštění se nezdařilo<br/><br/>Vypršel časový limit<br/><br/>Ověření se nezdařilo<br/><br/>opuštění |
| Vytvoření clusteru HDI na vyžádání Začínáme |spuštění |-|
| Clusteru HDI na vyžádání úspěšně vytvořena. |Úspěch |-|
| Odstranit clusteru HDI na vyžádání |Úspěch |-|

### <a name="tooedit-delete-or-disable-an-alert"></a>tooedit, odstranit nebo zakázat výstrahy

Použijte následující tooedit tlačítka (zvýrazněné červeně), odstranit nebo zakázat výstrahu hello.

![Výstrahy tlačítka](./media/data-factory-monitor-manage-app/AlertButtons.png)
