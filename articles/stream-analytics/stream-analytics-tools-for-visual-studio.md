---
title: aaaUse Azure Stream Analytics Tools pro sadu Visual Studio | Microsoft Docs
description: "Úvodní kurz pro hello Azure Stream Analytics Tools pro sadu Visual Studio"
keywords: Visual studio
documentationcenter: 
services: stream-analytics
author: 
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 
ms.author: 
ms.openlocfilehash: bda8e548040509a6f29f1b713268bc38f73228fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a>Použití Azure Stream Analytics Tools pro sadu Visual Studio
## <a name="introduction"></a>Úvod
V tomto kurzu zjistíte, jak vytvářet toouse Azure Stream Analytics Tools pro Visual Studio toocreate, lokálně otestovat, spravovat a ladit vaše úlohy Stream Analytics. 

Po dokončení tohoto kurzu, budete moci:
* Seznamte se s Stream Analytics Tools pro sadu Visual Studio.
* Nakonfigurujte a nasaďte úloha Stream Analytics.
* Otestujte úlohu místně s místní ukázková data.
* Pomocí monitorování tootroubleshoot problémy.
* Exportujte stávající tooprojects úlohy.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující požadavky:
* Dokončit hello kroky, které předcházet "Vytvořit úlohu služby Stream Analytics" v hello [sestavení řešení IoT pomocí služby Stream Analytics kurzu](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics). 
* Pomocí sady Visual Studio 2015, Visual Studio 2013 update 4 nebo Visual Studio 2012. Enterprise (Ultimate nebo Premium), Professional a Community jsou podporované tyto edice. Express edition není podporován. Visual Studio 2017 není podporována. 
* Použití hello Azure SDK pro .NET verze 2.7.1 nebo novější. Nainstalujte ji pomocí hello [instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).
* Nainstalujte hello [Stream Analytics Tools pro sadu Visual Studio](http://aka.ms/asatoolsvs).

## <a name="create-a-stream-analytics-project"></a>Vytvoření projektu Stream Analytics
1. V sadě Visual Studio, klikněte na tlačítko hello **soubor** nabídku a vyberte **nový projekt**. 

2. V seznamu šablon hello na levé straně hello, vyberte **Stream Analytics** a pak klikněte na **Azure Stream Analytics aplikace**.

3. Zadejte hello projektu **název**, **umístění**, a **název řešení** jako u jiných projektů.

    ![Okno Nový projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    A **Projedou** projektu se generuje ve **Průzkumníku řešení**.

    ![Projedou projektu vygenerované v Průzkumníku řešení](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-hello-correct-subscription"></a>Vyberte správné předplatné hello
1. V sadě Visual Studio, klikněte na tlačítko hello **zobrazení** nabídky a otevřete **Průzkumníka serveru**.

2. Přihlaste se pomocí účtu Azure. 

## <a name="define-hello-input-sources"></a>Definování vstupních zdrojů hello
1.  V **Průzkumníku řešení**, rozbalte položku hello **vstupy** uzlu a přejmenujte **Input.json** příliš**EntryStream.json**. Klikněte dvakrát na **EntryStream.json**.
2.  Hello **vstupní Alias** je nyní **EntryStream**. Hello vstupní alias se používá ve skriptu hello dotazu. 
3.  V **typ zdroje**, vyberte **datový proud**.
4.  V **zdroj**, vyberte **centra událostí**.
5.  V **Service Bus Namespace**, vyberte hello **TollData** možnost.
6.  V **název centra událostí**, vyberte **položka**.
7.  V **název zásady centra událostí**, vyberte **RootManageSharedAccessKey** (hello výchozí hodnota).
8.  V **formát serializace událostí**, vyberte **Json**. 
9.  V **kódování**, vyberte **UTF8**. Vaše nastavení by mělo vypadat jako hello následující snímek obrazovky:

    ![Vstupní okna](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. toofinish hello průvodce, klikněte na tlačítko **Uložit**. Teď můžete přidat další vstupní zdroj toocreate hello konec datového proudu. Klikněte pravým tlačítkem na hello **vstupy** uzel a vyberte možnost **novou položku**.

    ![Nová položka](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. V okně hello vyberte **Azure Stream Analytics vstup**a změňte hello **název** příliš**ExitStream.json**. Klikněte na tlačítko **Přidat**.

    ![Přidat novou položku – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. Klikněte dvakrát na **ExitStream.json** v projektu hello a postupujte podle hello stejné kroky jako jste to udělali pro hello vstupní datový proud. Být jisti tooenter **ukončete** pro hello **název centra událostí** jak ukazuje následující snímek obrazovky hello:

    ![Okno ExitStream](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    Nyní jste definovali dva vstupní datové proudy:

    ![Vstupní a výstupní vstupní datové proudy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    Dál přidejte referenčního datového vstupu pro soubor hello objektů blob, který obsahuje car registrační data.

13. Klikněte pravým tlačítkem na hello **vstupy** uzlu hello projektu a potom postupujte podle hello stejné kroky jako jste to udělali pro vstupy hello datového proudu. V **vstupní Alias**, zadejte **registrace**a v **typ zdroje**, vyberte **referenční data**.

    ![Okno registrace](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. V **účet úložiště**, vyberte hello **tolldata** možnost. V **kontejneru**, vyberte **tolldata**a v **vzorek cesty**, zadejte **registration.json**. Tento název souboru je velká a malá písmena a musí být psaný malými písmeny.
15. toofinish hello průvodce, klikněte na tlačítko **Uložit**.

Nyní jsou definovány všechny vstupy hello.

## <a name="define-hello-output"></a>Definování výstup hello
1.  V **Průzkumníku řešení**, rozbalte položku hello **vstupy** uzel a poklikejte na soubor **Output.json**.

2.  V **Alias pro výstup**, zadejte **výstup**. 
3.  V **jímky**, vyberte **SQL Database**.
4.  V **databáze**, vyberte **TollDataDB**.
5.  V **uživatelské jméno**, zadejte **tolladmin**. 
6.  V **heslo**, zadejte **123toll!**.
7.  V **tabulky**, zadejte **TollDataRefJoin**.
8.  Klikněte na **Uložit**.

    ![Výstup – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a>Vytvořte dotaz služby Stream Analytics
V tomto kurzu pokusí tooanswer několik obchodní otázky, které jsou tootoll související data. Vytvoří také Stream Analytics dotazy, které lze použít v odpovědi relevantní tooprovide Stream Analytics.
Než začnete vaše první práce Stream Analytics, podíváme se na jednoduchý scénář a hello syntaxe dotazu.

### <a name="introduction-toohello-stream-analytics-query-language"></a>Úvod toohello Stream Analytics query language
Řekněme, že je potřeba toocount hello počet vozidel, které projedou stánek zadejte. V tomto příkladu je nepřetržitý proud událostí, a proto máte toodefine a časové období. Upravit toobe otázku hello "kolik vozidel zadejte projedou stánek každé tři minuty?" Tento způsob toocount, data se běžně označuje počet přeskakující tooas hello.

Podívejte se na hello Stream Analytics dotaz, který odpoví na tuto otázku:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

Stream Analytics používá dotazovací jazyk, který je například SQL a přidává několik rozšíření toospecify souvisejících s časem aspektů hello dotazu.

Další informace najdete v tématu [Správa času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [Oddílová](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstrukce používaných v dotazu hello z webu MSDN.

Teď, když jste napsali svůj první dotaz služby Stream Analytics, je čas tootest ho. Použijte hello ukázkových datových souborů umístěné ve složce TollApp aplikace hello následující cesty:

.. \TollApp\TollApp\Data

Tato složka obsahuje hello následující soubory:
*   Entry.JSON
*   Exit.JSON
*   Registration.JSON

## <a name="count-hello-number-of-vehicles-entering-a-toll-booth"></a>Počet hello vozidel zadávání stánek projedou
V projektu hello, klikněte dvakrát na **Script.asaql** tooopen hello skript v hello **Editor dotazů**. Zkopírujte a vložte hello skript v předchozí části hello do editoru hello. Hello Editor dotazů podporuje technologii IntelliSense, barevné zvýrazňování syntaxe a hello chyba značky.

![Editor dotazů](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a>Testování dotazů Stream Analytics místně

1. Klikněte pravým tlačítkem na projekt hello toocompile hello dotazu toosee Pokud je chyba syntaxe a vyberte **sestavení**. 

2. toovalidate tento dotaz proti ukázková data, můžete použít místní ukázková data. Klikněte pravým tlačítkem na vstup hello a vyberte **přidat místní vstup**.

    ![Přidat místní vstup](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. V místním okně hello vyberte hello ukázková data z vaší místní cesta. Klikněte na **Uložit**.

    ![Přidejte místní vstupní okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    Soubor s názvem **local_EntryStream.json** se automaticky přidá tooyour vstupy složky.

    ![Soubor přidané tooinputs složku](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. V hello **Editor dotazů**, klikněte na tlačítko **spustit místně**. Nebo můžete stisknutím klávesy F5 hello.

    ![Spusťte místně](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Místní spuštění výstupu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    Stiskněte klávesu žádný výstup hello klíče tooview v hello **ASA místní spuštění výsledek** oken v sadě Visual Studio. 

    ![Okno výsledků ASA místní spuštění](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. Klikněte na tlačítko **otevřít složku výsledek** toocheck hello výstupní soubory oba ve formátu CSV a JSON.

    ![Otevřete složku výsledků výstupu](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-hello-input-data"></a>Ukázka hello vstupních dat
Můžete také ukázková vstupní data z místního souboru tooa vstupního zdroje. 
1. Klikněte pravým tlačítkem na hello vstupní konfigurační soubor a vyberte **ukázková Data**. 

   ![Ukázková data](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    Teď můžete ukázkové pouze centra událostí nebo služby IoT hub. Jiných vstupních zdrojů nejsou podporovány.

2. V místním okně hello zadejte hello místní cestu použitou toosave hello ukázková data. Klikněte na tlačítko **ukázka**.

    ![Okno ukázkových dat](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    Zobrazí se průběh hello hello **výstup** okno. 

    ![Výstup – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-tooazure"></a>Odeslání tooAzure dotazů Stream Analytics
1. V hello **Editor dotazů**, klikněte na tlačítko **odeslání tooAzure** v editor skriptů hello.

    ![Odeslání tooAzure](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. Vyberte **vytvoření nové úlohy Azure Stream Analytics**. Zadejte hello **název úlohy**a vyberte správnou hello **předplatné**. Klikněte na tlačítko **odeslání**.

    ![Odeslání úlohy okna](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a>Spustit úlohu
Teď, když se vytvoří úlohu, se automaticky otevře zobrazení úloh hello. 
1. toostart hello úlohy, klikněte na tlačítko hello **zelenou šipku** tlačítko.

    ![Spustit úlohu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. Vyberte hello výchozí nastavení a klikněte na **spustit**.
 
    ![Spusťte okno úlohy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    Úloha Hello **stav** změní příliš**systémem**, a **vstup události** a **výstupních událostech** zobrazí.

    ![Stav spuštění úlohy souhrnu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-hello-results-in-visual-studio"></a>Výsledky kontroly hello v sadě Visual Studio
1. V sadě Visual Studio otevřete **Průzkumníka serveru** a klikněte pravým tlačítkem na hello **TollDataRefJoin** tabulky.
2. Vyberte **zobrazit Data tabulky** toosee hello výstupu úlohy.
   
    ![Výběr dat v tabulce zobrazit v Průzkumníku serveru](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-hello-job-metrics"></a>Zobrazit hello úlohy metriky
Statistikami základní úlohy lze nalézt v **úlohy metriky**. 

![Okno metriky úlohy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-hello-job-in-server-explorer"></a>Seznam hello úlohy v Průzkumníku serveru
V **Průzkumníka serveru**, klikněte na tlačítko **úlohy Stream Analytics** a pak klikněte na **aktualizovat**. Hello úlohy se zobrazí pod **úlohy Stream Analytics**.

![Úlohy Stream Analytics uvedené v Průzkumníku serveru](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-hello-job-view"></a>Zobrazení úloh otevřete hello
zobrazení úloh hello tooopen, rozbalte uzel vaší úlohy a dvakrát klikněte na hello **zobrazení úloh** uzlu.

![Zobrazení úloh uzlu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-tooa-project"></a>Export existujícího projektu tooa úlohy
Existují dva způsoby, můžete exportovat existujícího projektu tooa úlohy.

V **Průzkumníka serveru**, klikněte pravým tlačítkem na hello úlohy uzlu v hello **úlohy Stream Analytics** uzel a vyberte možnost **exportovat tooNew Stream Analytics projektu**.

![Export tooNew Stream Analytics projektu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

projekt Hello se generuje ve **Průzkumníku řešení**.

![Projektu vygenerované v Průzkumníku řešení](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
Můžete také použít hello zobrazení úloh a klikněte na tlačítko **generovat projektu**.

![Generování projektu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a>Známé problémy a omezení
 
- Neexistuje žádná podpora pro výstup Power BI a výstup Azure data Lake Store.
- Neexistuje žádná podpora editor pro přidání nebo změna uživatelem definované funkce jazyka JavaScript.

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme s použitím Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
