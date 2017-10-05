---
title: "Použití Azure Stream Analytics Tools pro sadu Visual Studio | Microsoft Docs"
description: "Úvodní kurz pro Azure Stream Analytics Tools pro sadu Visual Studio"
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
ms.openlocfilehash: 618c1055795a75e0ed71dacddba3e076f81f4946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a>Použití Azure Stream Analytics Tools pro sadu Visual Studio
## <a name="introduction"></a>Úvod
V tomto kurzu zjistěte, jak používat Azure Stream Analytics Tools pro sadu Visual Studio k vytvoření, vytváření, testování místně, spravovat a ladit vaše úlohy Stream Analytics. 

Po dokončení tohoto kurzu, budete moci:
* Seznamte se s Stream Analytics Tools pro sadu Visual Studio.
* Nakonfigurujte a nasaďte úloha Stream Analytics.
* Otestujte úlohu místně s místní ukázková data.
* Monitorování použít k řešení potíží.
* Exportujte stávající úlohy na projekty.

## <a name="prerequisites"></a>Požadavky
Pro absolvování tohoto kurzu musí být splněné následující požadavky:
* Dokončit kroky, které předcházet "Vytvořit úlohu služby Stream Analytics" v [sestavení řešení IoT pomocí služby Stream Analytics kurzu](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics). 
* Pomocí sady Visual Studio 2015, Visual Studio 2013 update 4 nebo Visual Studio 2012. Enterprise (Ultimate nebo Premium), Professional a Community jsou podporované tyto edice. Express edition není podporován. Visual Studio 2017 není podporována. 
* Použití sady Azure SDK pro .NET verze 2.7.1 nebo novější. Nainstalujte ji pomocí [Instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).
* Nainstalujte [Stream Analytics Tools pro sadu Visual Studio](http://aka.ms/asatoolsvs).

## <a name="create-a-stream-analytics-project"></a>Vytvoření projektu Stream Analytics
1. V sadě Visual Studio, klikněte **soubor** nabídku a vyberte **nový projekt**. 

2. V seznamu šablon na levé straně vyberte **Stream Analytics** a pak klikněte na **Azure Stream Analytics aplikace**.

3. Zadejte projektu **název**, **umístění**, a **název řešení** jako u jiných projektů.

    ![Okno Nový projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    A **Projedou** projektu se generuje ve **Průzkumníku řešení**.

    ![Projedou projektu vygenerované v Průzkumníku řešení](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-the-correct-subscription"></a>Vyberte správné předplatné.
1. V sadě Visual Studio, klikněte **zobrazení** nabídky a otevřete **Průzkumníka serveru**.

2. Přihlaste se pomocí účtu Azure. 

## <a name="define-the-input-sources"></a>Definování vstupních zdrojů
1.  V **Průzkumníku řešení**, rozbalte **vstupy** uzlu a přejmenujte **Input.json** k **EntryStream.json**. Klikněte dvakrát na **EntryStream.json**.
2.  **Vstupní Alias** je nyní **EntryStream**. Vstupní alias se používá ve skriptu dotazu. 
3.  V **typ zdroje**, vyberte **datový proud**.
4.  V **zdroj**, vyberte **centra událostí**.
5.  V **Service Bus Namespace**, vyberte **TollData** možnost.
6.  V **název centra událostí**, vyberte **položka**.
7.  V **název zásady centra událostí**, vyberte **RootManageSharedAccessKey** (výchozí hodnota).
8.  V **formát serializace událostí**, vyberte **Json**. 
9.  V **kódování**, vyberte **UTF8**. Vaše nastavení by mělo vypadat jako na následujícím snímku obrazovky:

    ![Vstupní okna](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. Chcete-li dokončit průvodce, klikněte na tlačítko **Uložit**. Teď můžete přidat další vstupní zdroj pro vytvoření konec datového proudu. Klikněte pravým tlačítkem myši **vstupy** uzel a vyberte možnost **novou položku**.

    ![Nová položka](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. V okně vyberte **Azure Stream Analytics vstup**a změňte **název** k **ExitStream.json**. Klikněte na tlačítko **Přidat**.

    ![Přidat novou položku – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. Klikněte dvakrát na **ExitStream.json** v projektu a postupujte podle kroků stejná jako jste to udělali pro vstupní datový proud. Nezapomeňte zadat **ukončete** pro **název centra událostí** jak je znázorněno na následujícím snímku obrazovky:

    ![Okno ExitStream](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    Nyní jste definovali dva vstupní datové proudy:

    ![Vstupní a výstupní vstupní datové proudy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    Dál přidejte referenčního datového vstupu pro objekt blob, který obsahuje car registrační data.

13. Klikněte pravým tlačítkem myši **vstupy** uzlu v projektu a pak postupujte podle stejné kroky jako jste to udělali pro vstupy datového proudu. V **vstupní Alias**, zadejte **registrace**a v **typ zdroje**, vyberte **referenční data**.

    ![Okno registrace](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. V **účet úložiště**, vyberte **tolldata** možnost. V **kontejneru**, vyberte **tolldata**a v **vzorek cesty**, zadejte **registration.json**. Tento název souboru je velká a malá písmena a musí být psaný malými písmeny.
15. Chcete-li dokončit průvodce, klikněte na tlačítko **Uložit**.

Nyní jsou definovány všechny vstupy.

## <a name="define-the-output"></a>Definování výstup
1.  V **Průzkumníku řešení**, rozbalte **vstupy** uzel a poklikejte na soubor **Output.json**.

2.  V **Alias pro výstup**, zadejte **výstup**. 
3.  V **jímky**, vyberte **SQL Database**.
4.  V **databáze**, vyberte **TollDataDB**.
5.  V **uživatelské jméno**, zadejte **tolladmin**. 
6.  V **heslo**, zadejte **123toll!**.
7.  V **tabulky**, zadejte **TollDataRefJoin**.
8.  Klikněte na **Uložit**.

    ![Výstup – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a>Vytvořte dotaz služby Stream Analytics
V tomto kurzu se pokusí odpovězte několik obchodní otázky, které se vztahují k dat pro výběr poplatků. Vytvoří také Stream Analytics dotazy, které lze použít v Stream Analytics k poskytování odpovídající odpovědi.
Než začnete vaše první práce Stream Analytics, podíváme se na syntaxi dotazu a jednoduchého scénáře.

### <a name="introduction-to-the-stream-analytics-query-language"></a>Úvod do služby Stream Analytics dotazovací jazyk
Řekněme, že je potřeba počítat počet vozidel, které projedou stánek zadejte. V tomto příkladu je nepřetržitý proud událostí, a proto je nutné definovat v časovém intervalu. Upravte dotaz a "jak množství prostředků zadejte projedou stánek každé tři minuty?" Tímto způsobem počet dat se obvykle označuje jako počet přeskakující.

Podívejte se na dotaz služby Stream Analytics, na který odpoví na tuto otázku:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

Stream Analytics používá dotazovací jazyk, který je například SQL a přidává několik rozšíření k určení souvisejících s časem aspekty dotazu.

Další informace najdete v tématu [Správa času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [Oddílová](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstrukce použitý v dotazu z webu MSDN.

Teď, když jste napsali svůj první dotaz služby Stream Analytics, je čas to vyzkoušíte. Pomocí ukázkových datových souborů, umístěný ve složce TollApp v následující cestě:

.. \TollApp\TollApp\Data

Tato složka obsahuje následující soubory:
*   Entry.JSON
*   Exit.JSON
*   Registration.JSON

## <a name="count-the-number-of-vehicles-entering-a-toll-booth"></a>Počet vozidel zadávání stánek projedou
V projektu, klikněte dvakrát na **Script.asaql** otevřete skript v **Editor dotazů**. Zkopírujte a vložte skript v předchozí části do editoru. Editor dotazů podporuje technologii IntelliSense, barevné zvýrazňování syntaxe a chyba značky.

![Editor dotazů](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a>Testování dotazů Stream Analytics místně

1. Kompilace dotazu, který chcete zobrazit, pokud je chyba syntaxe, klikněte pravým tlačítkem na projekt a vyberte **sestavení**. 

2. K ověření tohoto dotazu proti ukázková data, můžete použít místní ukázková data. Klikněte pravým tlačítkem na vstupu a vyberte **přidat místní vstup**.

    ![Přidat místní vstup](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. V místním okně vyberte ukázková data z vaší místní cesta. Klikněte na **Uložit**.

    ![Přidejte místní vstupní okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    Soubor s názvem **local_EntryStream.json** je automaticky přidán do složky vstupy.

    ![Soubory přidané do složky vstupy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. V **Editor dotazů**, klikněte na tlačítko **spustit místně**. Nebo můžete stisknutím klávesy F5.

    ![Spusťte místně](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Místní spuštění výstupu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    Stisknutím libovolné klávesy zobrazte výstup v **ASA místní spuštění výsledek** oken v sadě Visual Studio. 

    ![Okno výsledků ASA místní spuštění](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. Klikněte na tlačítko **otevřít složku výsledek** zkontrolujte výstup souborů, oba ve formátu CSV a JSON.

    ![Otevřete složku výsledků výstupu](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-the-input-data"></a>Ukázka vstupních dat
Můžete také ukázková vstupní data z vstupního zdroje do místního souboru. 
1. Klikněte pravým tlačítkem na vstupní konfigurační soubor a vyberte **ukázková Data**. 

   ![Ukázková data](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    Teď můžete ukázkové pouze centra událostí nebo služby IoT hub. Jiných vstupních zdrojů nejsou podporovány.

2. V místním okně zadejte místní cestu používá pro uložení ukázková data. Klikněte na tlačítko **ukázka**.

    ![Okno ukázkových dat](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    Můžete sledovat průběh v **výstup** okno. 

    ![Výstup – okno](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-to-azure"></a>Odeslat dotaz do služby Azure Stream Analytics
1. V **Editor dotazů**, klikněte na tlačítko **odeslat do Azure** v editoru skriptů.

    ![Odeslat do Azure](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. Vyberte **vytvoření nové úlohy Azure Stream Analytics**. Zadejte **název úlohy**a vyberte správnou **předplatné**. Klikněte na tlačítko **odeslání**.

    ![Odeslání úlohy okna](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a>Spustit úlohu
Teď, když se vytvoří úlohu, se automaticky otevře zobrazení úloh. 
1. Chcete-li spustit úlohu, klikněte na tlačítko **zelenou šipku** tlačítko.

    ![Spustit úlohu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. Vyberte výchozí nastavení a klikněte na tlačítko **spustit**.
 
    ![Spusťte okno úlohy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    Úloha **stav** změny **systémem**, a **vstup události** a **výstupních událostech** zobrazí.

    ![Stav spuštění úlohy souhrnu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-the-results-in-visual-studio"></a>Zkontrolujte výsledky v sadě Visual Studio
1. V sadě Visual Studio otevřete **Průzkumníka serveru** a klikněte pravým tlačítkem **TollDataRefJoin** tabulky.
2. Vyberte **zobrazit Data tabulky** na zobrazení výstupu úlohy.
   
    ![Výběr dat v tabulce zobrazit v Průzkumníku serveru](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-the-job-metrics"></a>Zobrazení metriky úlohy
Statistikami základní úlohy lze nalézt v **úlohy metriky**. 

![Okno metriky úlohy](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-the-job-in-server-explorer"></a>Seznam úloh v Průzkumníku serveru
V **Průzkumníka serveru**, klikněte na tlačítko **úlohy Stream Analytics** a pak klikněte na **aktualizovat**. Úloha se zobrazí pod **úlohy Stream Analytics**.

![Úlohy Stream Analytics uvedené v Průzkumníku serveru](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-the-job-view"></a>Otevřete zobrazení úloh
K otevření zobrazení úlohy, rozbalte uzel vaší úlohy a dvakrát klikněte **zobrazení úloh** uzlu.

![Zobrazení úloh uzlu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-to-a-project"></a>Exportovat stávající úloze do projektu
Existují dva způsoby stávající úloze můžete exportovat do projektu.

V **Průzkumníka serveru**, klikněte pravým tlačítkem v uzlu úlohy **úlohy Stream Analytics** uzel a vyberte možnost **exportovat do nového projektu Stream Analytics**.

![Exportovat do nového projektu Stream Analytics](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

Projekt se generuje ve **Průzkumníku řešení**.

![Projektu vygenerované v Průzkumníku řešení](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
Také můžete použít zobrazení úloh a klikněte na tlačítko **generovat projektu**.

![Generování projektu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a>Známé problémy a omezení
 
- Neexistuje žádná podpora pro výstup Power BI a výstup Azure data Lake Store.
- Neexistuje žádná podpora editor pro přidání nebo změna uživatelem definované funkce jazyka JavaScript.

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme s použitím Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
