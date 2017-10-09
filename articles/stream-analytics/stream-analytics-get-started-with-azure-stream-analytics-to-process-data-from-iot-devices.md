---
title: "aaaIoT v reálném čase datové proudy a Azure Stream Analytics | Microsoft Docs"
description: "Zařízení SensorTag pro IoT, proudy dat, analytické funkce pro analýzu proudů dat a zpracování dat v reálném čase"
keywords: "řešení iot, začínáme s iot"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 3e829055-75ed-469f-91f5-f0dc95046bdb
ms.service: stream-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 422e6b719d0289880aa7f17fdc585e2b768c63d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-stream-analytics-tooprocess-data-from-iot-devices"></a>Začínáme s Azure Stream Analytics tooprocess daty ze zařízení IoT
V tomto kurzu se dozvíte, jak toocreate datového proudu zpracování logiky toogather data ze zařízení, Internet věcí (IoT). Jak použijeme případu toodemonstrate reálného, Internet věcí (IoT) pomocí toobuild řešení rychlého a ekonomického.

## <a name="prerequisites"></a>Požadavky
* [Předplatné Azure](https://azure.microsoft.com/pricing/free-trial/)
* Ukázkový dotaz a datové soubory ke stažení z webu [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Scénář
Contoso, který je společnost v prostoru hello automatizace průmyslu, má zcela automatizovat svůj výrobní proces. Hello stroje v továrně společnosti má senzorů, které podporují vytváření datových proudů dat v reálném čase. Manažer provozovny v tomto scénáři chce přehledy v reálném čase toohave z hello senzor data toolook pro vzory a provést akce v nich. Použijeme hello Stream Analytics dotazu jazyka SAQL () přes hello senzor data toofind zajímavých vzorců z hello, příchozí datový proud.

Samotná data jsou generována zařízením Texas Instruments SensorTag.

![Texas Instruments SensorTag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

hello datových částí Hello je ve formátu JSON a vypadá hello následující:

    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

V podobném scénáři z reálného prostředí by se jednalo o stovky podobných snímačů generujících události jako datový proud. V ideálním případě by zařízení brány by spustit kód toopush tyto události příliš[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) nebo [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/). Úlohu Stream Analytics by ingestování tyto události ze služby Event Hubs a spouštění dotazů analýzu v reálném čase na datové proudy hello. Potom může odeslat hello tooone výsledky z hello [podporované výstupy](stream-analytics-define-outputs.md).

Pro snadnější použití tato příručka Začínáme poskytuje soubor ukázkových dat, která byla zaznamenána skutečnými zařízeními SensorTag. Můžete spouštět dotazy na hello ukázkových dat a zobrazte výsledky. V následujících kurzech se dozvíte, jak tooconnect vaše úlohy tooinputs a výstupy a nasadit je toohello služby Azure.

## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics
1. V hello [portál Azure](http://portal.azure.com), klikněte na znaménko plus hello a pak zadejte **STREAM ANALYTICS** v hello text okno toohello doprava. Potom vyberte **úlohy služby Stream Analytics** v seznamu výsledků hello.
   
    ![Vytvoření nové úlohy Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)
2. Zadejte název jedinečné úlohy a ověřte předplatné hello je hello správné jednu pro úlohu. Potom vytvořte novou skupinu prostředků nebo vyberte existující skupinu v rámci svého předplatného.
3. Pak vyberte umístění pro úlohu. Rychlost zpracování a snížení nákladů ve výběru hello přenos dat se doporučuje stejné umístění jako skupina prostředků hello a účet určený úložiště.
   
    ![Podrobnosti o vytvoření nové úlohy Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)
   
   > [!NOTE]
   > Tento účet úložiště byste měli pro každý region vytvořit pouze jednou. Toto úložiště bude sdílené napříč všemi úlohami Stream Analytics vytvořenými v příslušné oblasti.
   > 
   > 
4. Pole tooplace hello úlohu na řídicím panelu a pak klikněte na **vytvořit**.
   
    ![průběh vytváření úlohy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)
5. Měli byste vidět, nasazení zahájen' zobrazených v hello pravém horním rohu okna prohlížeče. Brzy změní okno tooa dokončit, jak je uvedeno níže.
   
    ![průběh vytváření úlohy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Vytvoření dotazu služby Stream Analytics
Úlohu po dobu tooopen jej vytvořil a sestavení dotazu. Kliknutím na dlaždici hello ho můžete snadný přístup k vaší práce.

![Dlaždice úlohy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

V hello **úlohy topologie** podokně klikněte na tlačítko hello **dotazu** pole toogo toohello editoru dotazů. Hello **dotazu** editoru vám umožní tooenter T-SQL dotaz, který provádí transformaci hello přes hello příchozích dat událostí.

![Pole Dotaz](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Dotaz: Archivace nezpracovaných dat
Hello Nejjednodušším typem dotazu je průchozí dotaz, který archivy všechny vstupní data tooits určené výstup. Stáhnout hello ukázkový datový soubor z [Githubu](https://aka.ms/azure-stream-analytics-get-started-iot) tooa umístění v počítači. 

1. Vložte hello dotaz ze souboru PassThrough.txt hello. 
   
    ![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)
2. Klikněte na vstup další tooyour hello třemi tečkami a vyberte **nahrát ukázková data ze souboru** pole.
   
    ![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)
3. Podokno otevře na hello práva jako výsledek, v ní vyberte hello HelloWorldASA-InputStream.json data z vaší stažené umístění a klikněte na tlačítko **OK** dolnímu hello hello podokna.
   
    ![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)
4. Pak klikněte na tlačítko hello **testování** zařízení v hello nahoře vlevo oblast okna hello a zpracování dotazu testovací oproti hello ukázkovou datovou sadu. Po dokončení zpracování hello, otevře se okno výsledků níže dotazu.
   
    ![Výsledky testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-hello-data-based-on-a-condition"></a>Dotaz: Filtrování dat hello na základě podmínky
Nyní si vyzkoušíte toofilter hello výsledků na základě podmínky. Rádi bychom znali tooshow výsledky pro jenom události, které pocházejí z "sensorA." Hello dotaz je v souboru Filtering.txt hello.

![Filtrování datového proudu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Všimněte si, že hello malá a velká písmena dotazu porovnává hodnotu řetězce. Klikněte na tlačítko hello **Test** zařízení tooexecute hello dotaz znovu. Hello dotaz by měl vrátit řádky 389 z 1860 událostí.

![Druhé výsledky výstupu z testu dotazu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-tootrigger-a-business-workflow"></a>Dotaz: Výstrahy tootrigger pracovní postup společnosti
Vytvoříme podrobnější dotaz. Pro každý typ senzor jsme chcete toomonitor průměrnou teplotu okno 30 sekund a zobrazit výsledky pouze v případě, že je hello teplota překročí 100 stupňů. Jsme zapíše hello následující dotaz a pak klikněte na tlačítko **Test** toosee hello výsledky. Hello dotaz je v souboru ThresholdAlerting.txt hello.

![30sekundový dotaz filtru](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Měli byste vidět výsledky, které obsahují pouze 245 řádků a názvy senzorů kde hello teplota je vyšší než 100. Tento dotaz skupiny hello datového proudu událostí podle **hodnoty dspl**, což je název senzoru hello, než **Přeskakujícího okna** 30 sekund. Dočasných dotazů musí stavu, jakým způsobem se bude tooprogress čas. Pomocí hello **TIMESTAMP BY** klauzuli jsme určili hello **OUTPUTTIME** časy tooassociate sloupec se všechny dočasné výpočty. Podrobné informace najdete v tématu hello MSDN články o [Správa času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [Oddílová funkce](https://msdn.microsoft.com/library/azure/dn835019.aspx).

### <a name="query-detect-absence-of-events"></a>Dotaz: Zjištění neexistence událostí
Jak můžete jsme napsat dotaz toofind chybějících vstupní události? Umožňuje najít hello poslední čas, kdy senzor odesílají data a pak události pro hello příští minuty neodeslal. Hello dotaz je v souboru AbsenseOfEvent.txt hello.

![Zjištění neexistence událostí](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Tady používáme **LEVÉ vnější** připojení toohello stejný datový proud (spojení sama na sebe). Při použití příkazu **INNER JOIN** se vrátí výsledek, pouze když je nalezena shoda.  Pro **LEVÉ vnější** připojení, pokud je nalezena shoda, událost z levé strany spojení hello hello řádek, který má hodnotu NULL pro všechny hello sloupce hello pravé straně, je vrácena. Tato technika je velmi užitečná toofind absence událostí. Další informace o příkazu [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) najdete v dokumentaci MSDN.

## <a name="conclusion"></a>Závěr
Hello účelu tohoto kurzu je toodemonstrate jak toowrite různé Stream Analytics Query Language dotazy a zjistit, výsledkem hello prohlížeče. Je to však jenom začátek. Pomocí Stream Analytics toho můžete dělat mnohem víc. Stream Analytics podporuje celou řadu vstupy a výstupy a můžete i pomocí funkcí v Azure Machine Learning toomake ho robustní nástroj pro analýzu datových proudů. Další informace o Stream Analytics tooexplore můžete spustit pomocí našich [mapa kurzů](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Další informace o tom, toowrite dotazy, přečtěte si článek hello o [běžné typy dotazů](stream-analytics-stream-analytics-query-patterns.md).

