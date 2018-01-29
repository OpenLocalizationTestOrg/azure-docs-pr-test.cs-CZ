---
title: "Datové proudy IoT v reálném čase a služba Azure Stream Analytics | Dokumentace Microsoftu"
description: "Zařízení SensorTag pro IoT, proudy dat, analytické funkce pro analýzu proudů dat a zpracování dat v reálném čase"
keywords: "řešení iot, začínáme s iot"
services: stream-analytics
documentationcenter: 
author: SnehaGunda
manager: jhubbard
editor: cgronlun
ms.assetid: 3e829055-75ed-469f-91f5-f0dc95046bdb
ms.service: stream-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: sngun
ms.openlocfilehash: a4b2fda6c5cc5ea341618ec5fa8638a5c887bf84
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Začínáme se zpracováním dat ze zařízení IoT pomocí služby Azure Stream Analytics
V tomto kurzu se naučíte vytvořit logiku zpracování datového proudu ke shromáždění dat ze zařízení s platformou IoT (Internet věcí). Na skutečném případu použití platformy IoT si budete moci prohlédnout postup rychlého a ekonomického sestavení potřebného řešení.

## <a name="prerequisites"></a>Požadavky
* [Předplatné Azure](https://azure.microsoft.com/pricing/free-trial/)
* Ukázkový dotaz a datové soubory ke stažení z webu [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Scénář
Contoso, společnost působící v oboru automatizace průmyslu, zcela automatizovala svůj výrobní proces. Stroje v továrně společnosti mají snímače, které dokáží vysílat datové proudy v reálném čase. V tomto scénáři chce manažer provozovny v reálném čase získávat informace na základě dat ze senzorů, hledat jejich pomocí různé vzorce a na jejich základě provádět potřebné akce. K identifikaci zajímavých vzorců v příchozím datovém proudu ze senzorů použije jazyk SAQL (Stream Analytics Query Language).

Samotná data jsou generována zařízením Texas Instruments SensorTag. Datová část dat je ve formátu JSON a vypadá takto:

    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

V podobném scénáři z reálného prostředí by se jednalo o stovky podobných snímačů generujících události jako datový proud. V ideálním případě by bylo také nainstalováno nějaké zařízení sloužící jako brána k odesílání událostí do služby [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) nebo [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/). Úlohu Stream Analytics tyto události ze služby Event Hubs ingestuje a spustí dotazy na analýzu datových proudů v reálném čase. Poté můžete výsledky odeslat do jednoho z [podporovaných výstupů](stream-analytics-define-outputs.md).

Pro snadnější použití tato příručka Začínáme poskytuje soubor ukázkových dat, která byla zaznamenána skutečnými zařízeními SensorTag. Na těchto ukázkových datech můžete spouštět dotazy a zobrazit výsledky. V následujících kurzech se naučíte připojovat své úlohy ke vstupům a výstupům a nasazovat je ve službě Azure.

## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics
1. Na webu [Azure Portal](http://portal.azure.com) klikněte na symbol plus a do textového pole vpravo napište **STREAM ANALYTICS**. Ve výsledcích hledání vyberte **Úloha Stream Analytics**.
   
    ![Vytvoření nové úlohy Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)
2. Zadejte jedinečný název úlohy a ověřte, že je předplatné pro vaši úlohu správné. Potom vytvořte novou skupinu prostředků nebo vyberte existující skupinu v rámci svého předplatného.
3. Pak vyberte umístění pro úlohu. Pro urychlení zpracování a snížení nákladů přenosů dat se doporučuje výběr stejného umístění, ve kterém je skupina prostředků a zamýšlený účet úložiště.
   
    ![Podrobnosti o vytvoření nové úlohy Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)
   
   > [!NOTE]
   > Tento účet úložiště byste měli pro každý region vytvořit pouze jednou. Toto úložiště bude sdílené napříč všemi úlohami Stream Analytics vytvořenými v příslušné oblasti.
   > 
   > 
4. Zaškrtněte políčko pro umístění úlohy do řídicího panelu a klikněte na **VYTVOŘIT**.
   
    ![průběh vytváření úlohy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)
5. V pravém horním rohu okna prohlížeče by se měla zobrazit zpráva Nasazení začalo... Brzy se změní na okno s informací o dokončení, jak je vidět dále.
   
    ![průběh vytváření úlohy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

## <a name="create-an-azure-stream-analytics-query"></a>Vytvoření dotazu služby Stream Analytics
Po vytvoření úlohy nastal čas ji otevřít a vytvořit dotaz. K úloze můžete jednoduše přejít tak, že kliknete na její dlaždici.

![Dlaždice úlohy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

V podokně **Topologie úlohy** klikněte do pole **DOTAZ**. Tím přejdete do Editoru dotazů. Editor **DOTAZŮ** umožňuje zadání dotazu T-SQL, který provádí transformaci příchozích dat událostí.

![Pole Dotaz](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Dotaz: Archivace nezpracovaných dat
Nejjednodušším typem dotazu je průchozí dotaz, který archivuje veškerá vstupní data do definovaného výstupního umístění. Teď si z webu [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) stáhněte soubor ukázkových dat do umístění ve svém počítači. 

1. Vložte dotaz ze souboru PassThrough.txt. 
   
    ![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)
2. Klikněte na tlačítko se třemi tečkami vedle vstupního datového proudu a zaškrtněte políčko **Odeslat ukázková data ze souboru**.
   
    ![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)
3. Vpravo se otevře podokno. V něm vyberte datový soubor HelloWorldASA-InputStream.json ze staženého umístění a v dolní části podokna klikněte na **OK**.
   
    ![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)
4. Potom klikněte na ikonu ozubeného kola **Test** vlevo nahoře a zpracujte zkušební dotaz s ukázkovou datovou sadou. Po dokončení zpracování se pod dotazem otevře okno s výsledky.
   
    ![Výsledky testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Dotaz: Filtrování dat na základě podmínky
Nyní si vyzkoušíte filtrování výsledků na základě podmínky. Cílem je zobrazit výsledky pouze pro události přicházející ze zařízení SensorA. Dotaz je umístěný v souboru Filtering.txt.

![Filtrování datového proudu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Všimněte si, že hodnotu řetězce porovnává dotaz rozlišující velká a malá písmena. Opětovným kliknutím na ikonu ozubeného kola **Test** proveďte dotaz. Dotaz by měl vrátit 389 řádků z 1860 událostí.

![Druhé výsledky výstupu z testu dotazu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Dotaz: Upozornění spouštějící pracovní postup společnosti
Vytvoříme podrobnější dotaz. Pro každý typ snímače chceme monitorovat průměrnou teplotu v 30sekundovém intervalu a výsledky zobrazit pouze v případě, že teplota překročí 100 stupňů. Napíšeme následující dotaz a zobrazíme výsledky kliknutím na **Test**. Dotaz je umístěný v souboru ThresholdAlerting.txt.

![30sekundový dotaz filtru](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Teď byste měli vidět výsledky, které obsahují pouze 245 řádků a uvádějí jenom názvy snímačů, u kterých průměrná teplota překročila 100 stupňů. Tento dotaz seskupí toky událostí podle hodnoty **dspl**, což je název snímače, a pomocí 30sekundového **přeskakujícího okna**. Dočasné dotazy musí stanovit, jakým způsobem se má načasovat postup. Pomocí klauzule **TIMESTAMP BY** jsme jako klíč přidružení časů ke všem dočasným výpočtům určili sloupec **OUTPUTTIME**. Podrobné informace najdete v článcích na webu MSDN o [správě času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [funkcích práce s okny](https://msdn.microsoft.com/library/azure/dn835019.aspx).

### <a name="query-detect-absence-of-events"></a>Dotaz: Zjištění neexistence událostí
Jak napsat dotaz, abychom dokázali najít chybějící vstupní události? Můžete například zjistit, kdy snímač naposledy odeslal data a následně po dobu 5 sekund neodeslal žádné události. Dotaz je umístěný v souboru AbsenseOfEvent.txt.

![Zjištění neexistence událostí](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

V dotazu se používá příkaz **LEFT OUTER JOIN** na stejný datový proud (spojení sama na sebe). Při použití příkazu **INNER JOIN** se vrátí výsledek, pouze když je nalezena shoda.  Příkaz **LEFT OUTER JOIN** však v případě, že pro událost z levé strany spojení není nalezena shoda, vrátí řádek s hodnotami NULL pro všechny sloupce pravé strany. Tato technika je velmi užitečná k vyhledání absence událostí. Další informace o příkazu [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) najdete v dokumentaci MSDN.

## <a name="conclusion"></a>Závěr
Účelem tohoto kurzu je předvést způsob psaní různých dotazů v jazyku Stream Analytics Query Language a zobrazení výsledků v prohlížeči. Je to však jenom začátek. Pomocí Stream Analytics toho můžete dělat mnohem víc. Stream Analytics podporuje celou řadu vstupů a výstupů a může dokonce využít i funkce ve službě Azure Machine Learning. To z něj dělá robustní nástroj pro analýzu datových proudů. Můžete začít s bližším prozkoumáváním Stream Analytics pomocí našich [výukových materiálů](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Další informace o tom, jak psát dotazy, najdete v článku o [běžných vzorech dotazů](stream-analytics-stream-analytics-query-patterns.md).

