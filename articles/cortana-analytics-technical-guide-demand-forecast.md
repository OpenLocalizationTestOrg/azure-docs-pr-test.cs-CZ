---
title: "aaaDemand prognózy v technické příručce energie | Microsoft Docs"
description: "Technické příručce toohello šablona řešení s Microsoft Cortana Intelligence pro vyžádání prognózy v energie."
services: cortana-analytics
documentationcenter: 
author: yijichen
manager: ilanr9
editor: yijichen
ms.assetid: 7f1a866b-79b7-4b97-ae3e-bc6bebe8c756
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: inqiu;yijichen;ilanr9
ms.openlocfilehash: c97b7c19c9e3a317aecc329e61a0692d2f1ec53e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="technical-guide-toohello-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Technické příručce toohello Cortana Intelligence řešení šablony pro vyžádání prognózy v energie
## <a name="overview"></a>**Přehled**
Řešení šablony jsou určeny tooaccelerate hello proces vytváření ukázkové E2E nad Cortana Intelligence Suite. Nasazené šablona bude zřídit předplatné s nezbytné součásti Cortana Intelligence a sestavení hello vztahy mezi. Je také doplňuje hello datovém kanálu s ukázkovými daty generováno z aplikace simulace data. Stáhnout simulátoru hello data z uvedeného odkazu hello a nainstalujte ji na místním počítači, naleznete v souboru readme.txt toohello pokyny k použití simulátoru hello. Data vygenerovaná ze hello simulátoru bude hydrate hello datovém kanálu a spusťte generování machine learning předpovědi, které lze následně vizualizovat na řídicí panel Power BI hello.

Šablona řešení Hello naleznete [sem](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1)

proces nasazení Hello vás provede několik kroků tooset si přihlašovací údaje řešení. Ujistěte se, že záznam tyto přihlašovací údaje, jako je například název řešení, uživatelské jméno a heslo, které zadáte během nasazení hello.

Hello cílem tohoto dokumentu je tooexplain hello referenční architektura a různé součásti, které jsou zřízené v rámci vašeho předplatného v rámci této šablony řešení. dokument Hello také o tom, jak tooreplace hello ukázková data s reálná data z vlastní toobe možné toosee statistiky nebo předpovědi od vás won dat komunikuje. Kromě toho hello hello části hello šablona řešení, které byste potřebovali změnit, pokud chcete toocustomize hello řešení s vlastními daty toobe rozhovory dokumentu. Pokyny, jak toobuild hello řídicí panel Power BI pro tuto šablonu řešení najdete na konci hello.

## <a name="big-picture"></a>**Velký obrázek**
![](media/cortana-analytics-technical-guide-demand-forecast/ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Architektura vysvětlené
Při nasazení hello řešení, jsou aktivované různé služby Azure v rámci Cortana Analytics Suite (*tj.* Centra událostí, Stream Analytics, HDInsight, Data Factory strojového učení, *atd*). diagram architektury Hello výše ukazuje na vysoké úrovni, jak sestavený hello vyžádání prognózy energie řešení šablony ze začátku do konce. Bude moct tooinvestigate tyto služby klepnutím na ně na hello diagram řešení šablony vytvořené pomocí nasazení hello hello řešení. Hello následující oddíly popisují každou část.

## <a name="data-source-and-ingestion"></a>**Zdroj dat a přijímání**
### <a name="synthetic-data-source"></a>Zdroj dat syntetického
Pro tato data hello šablony zdroje využívaného generují z aplikace pracovní plochy, který bude stáhnout a spustit místně po úspěšné nasazení. Bude najít toodownload hello pokyny a nainstalovat aplikaci panelu hello vlastnosti, když vyberete hello první uzel s názvem simulátoru dat prognózy energie v diagramu šablonu řešení hello. Tuto aplikaci kanály hello [centra událostí Azure](#azure-event-hub) služby s datových bodů nebo událostí, které se použijí v hello zbytek hello řešení toku.

aplikace generování událostí Hello naplnit hello centra událostí Azure pouze tehdy, když je prováděna v počítači.

### <a name="azure-event-hub"></a>Centra událostí Azure
Hello [centra událostí Azure](https://azure.microsoft.com/services/event-hubs/) služba je hello příjemce hello vstup hello syntetické zdroje dat popsané výše.

## <a name="data-preparation-and-analysis"></a>**Příprava dat a analýzy**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) služba je použité tooprovide téměř v reálném čase analýzy hello vstupního datového proudu z hello [centra událostí Azure](#azure-event-hub) služby a publikovat výsledky do [Power BI](https://powerbi.microsoft.com) řídicí panel, jakož i archivace všechny nezpracované příchozí události toohello [Azure Storage](https://azure.microsoft.com/services/storage/) služby pro pozdější zpracování pomocí hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) služby.

### <a name="hd-insights-custom-aggregation"></a>Vlastní agregační HD statistiky
Hello služby Azure HD Insight je použité toorun [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) agregace tooprovide skripty (řízená Azure Data Factory) na hello nezpracované události, které byly archivovat pomocí služby Azure Stream Analytics hello.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (řízená Azure Data Factory) se používá služba toomake prognózy na budoucí spotřebu konkrétní oblasti zadané vstupy hello přijata.

## <a name="data-publishing"></a>**Publikování dat**
### <a name="azure-sql-database-service"></a>Služba Azure SQL Database
Hello [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) služba je použité toostore (spravované službou Azure Data Factory) hello předpovědi přijatých hello službu Azure Machine Learning, které bude potřeba v hello [Power BI](https://powerbi.microsoft.com) řídicího panelu.

## <a name="data-consumption"></a>**Spotřeba dat**
### <a name="power-bi"></a>Power BI
Hello [Power BI](https://powerbi.microsoft.com) služba je použité tooshow řídicí panel, který obsahuje agregace poskytované hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) služby a také vyžádání prognózy výsledky uložené v [Azure SQL Databáze](https://azure.microsoft.com/services/sql-database/) které byly vytvořeny pomocí hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) služby. Návod, jak toobuild hello řídicí panel Power BI pro tuto šablonu řešení najdete v části toohello níže.

## <a name="how-toobring-in-your-own-data"></a>**Jak toobring v svoje vlastní data**
Tato část popisuje, jak toobring tooAzure vlastní data a co by vyžadovaly oblasti změny dat hello můžete převést do této architektury.

Není pravděpodobné, že žádné datové sady, které přepnutím odpovídá datovou sadu hello používá pro tuto šablonu řešení. Seznámení s daty a požadavky na bude velmi důležitý v tom, jak upravit tuto šablonu toowork s vlastními daty. Pokud je to první ohrožení toohello službu Azure Machine Learning, můžete získat Úvod tooit pomocí příkladu hello [jak toocreate prvního experimentu](machine-learning/machine-learning-create-experiment.md).

Následující části Hello zabývat hello části hello šablony, který bude vyžadovat změny, pokud je zavedená nová datová sada.

### <a name="azure-event-hub"></a>Centra událostí Azure
Hello [centra událostí Azure](https://azure.microsoft.com/services/event-hubs/) je velmi obecná služba, tak, aby se data můžou být publikované toohello rozbočovače ve formátu CSV nebo formátu JSON. Žádné speciální zpracování dojde v hello centra událostí Azure, ale je důležité, že rozumíte hello data, která je dodáni do ní.

Tento dokument nepopisuje, jak tooingest vaše data, ale jeden můžete snadno odesílat události nebo data tooan centra událostí Azure pomocí hello [API centra událostí](event-hubs/event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) služby je použité tooprovide téměř v reálném čase analytics čtení z datových proudů a výstupy data tooany počet zdrojů.

Pro hello vyžádání prognózy pro šablonu řešení energie hello Azure Stream Analytics dotazu se skládá z dvě poddotazech, každý náročné události z hello centra událostí Azure služby jako vstupy a výstupy tootwo odlišné umístění s. Tyto výstupy obsahovat jednu datovou sadu Power BI a jedno umístění úložiště Azure.

Hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) dotazu lze najít:

* Protokolování do hello [portál pro správu Azure](https://manage.windowsazure.com/)
* Vyhledání úlohy stream analytics hello ![](media/cortana-analytics-technical-guide-demand-forecast/icon-stream-analytics.png) které byly generovány při řešení hello byl nasazen. Jeden pro vkládání dat tooblob úložiště (například mytest1streaming432822asablob) a hello druhou jeden je pro vkládání dat tooPower BI (např. mytest1streaming432822asapbi).
* Výběr

  * ***VSTUPY*** tooview hello dotazu vstup
  * ***DOTAZ*** samotný dotaz tooview hello
  * ***VÝSTUPY*** tooview hello různých výstupy

Informace o vytváření dotazů Azure Stream Analytics lze najít v hello [referenční příručka Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx) na webu MSDN.

V tomto řešení hello úlohy Azure Stream Analytics, která výstupy, že datovou sadu s téměř v reálném čase analytics informace o hello příchozí datový proud tooa řídicí panel Power BI je dodáván jako součást této šablony řešení. Vzhledem k tomu, že je implicitní znalosti o hello příchozí data formátu, tyto dotazy zapotřebí toobe změněna na základě formátu vaše data.

Hello jiné úlohy Azure Stream Analytics vypíše všechny [centra událostí](https://azure.microsoft.com/services/event-hubs/) události [Azure Storage](https://azure.microsoft.com/services/storage/) a proto vyžaduje žádné změnou bez ohledu na formát vaše data, jako je odesílání informací o úplné události hello toostorage.

### <a name="azure-data-factory"></a>Azure Data Factory
Hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) služeb orchestruje hello pohybů a zpracování dat. V hello vyžádání prognózy šablona řešení energie hello data factory se skládá z číslo dvanáct [kanály](data-factory/data-factory-create-pipelines.md) , přesunout a zpracování dat hello pomocí různých technologií.

  Objekt pro vytváření dat může přístup otevřením uzlu služby Data Factory hello dole hello v diagramu šablonu řešení hello vytvořené pomocí nasazení hello hello řešení. Bude to trvat můžete toohello objekt pro vytváření dat na portálu správy Azure. Pokud se zobrazí chyby v rámci vaší datové sady, ty můžete ignorovat jsou z důvodu toodata factory nasazuje před hello generátor dat byla spuštěna. Tyto chyby, nebudou bránit datovou továrnu nebude fungovat.

Tato část popisuje hello potřeby [kanály](data-factory/data-factory-create-pipelines.md) a [aktivity](data-factory/data-factory-create-pipelines.md) obsažené v hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Níže je zobrazení diagramu hello hello řešení.

![](media/cortana-analytics-technical-guide-demand-forecast/ADF2.png)

Pět hello kanály tohoto objektu pro vytváření obsahovat [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripty, které jsou používané toopartition a hello agregovaná data. Pokud už jsme zmínili, hello skripty se bude nacházet v hello [Azure Storage](https://azure.microsoft.com/services/storage/) účet vytvořený během instalace. Jejich umístění bude: demandforecasting\\\\skriptu\\\\hive\\ \\ (nebo name].blob.core.windows.net/demandforecasting https://[Your řešení).

Podobně jako toohello [Azure Stream Analytics](#azure-stream-analytics-1) dotazy, [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripty mají implicitní znalosti o hello příchozí data formátu, tyto dotazy potřebovat toobe změnit na základě formátu data a [konstruování](machine-learning/machine-learning-feature-selection-and-engineering.md) požadavky.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*
To [kanálu](data-factory/data-factory-create-pipelines.md) kanál obsahuje jediné aktivity – [HDInsightHive](data-factory/data-factory-hive-activity.md) aktivity pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , která se spouští [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx)skriptu tooaggregate hello každých 10 sekund datovým proudem, vyžádání dat na úrovni oblasti úrovně toohourly transformovny a umístí [Azure Storage](https://azure.microsoft.com/services/storage/) prostřednictvím úlohy Azure Stream Analytics hello.

[Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript pro toto rozdělení úloh je ***AggregateDemandRegion1Hr.hql***

#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*
To [kanálu](data-factory/data-factory-create-pipelines.md) obsahuje dvě aktivity:

* [HDInsightHive](data-factory/data-factory-hive-activity.md) aktivity pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , spustí podregistru skriptu tooaggregate hello hodinové vyžádání data o historii transformovny úrovně toohourly oblast úrovni a umístí ve službě Azure Storage během hello Azure Úlohy Stream Analytics
* [Kopírování](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity, která přemísťuje hello agregovaná data z úložiště Azure blob toohello Azure SQL Database, která zřízenou jako součást instalace šablony hello řešení.

Hello [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript pro tuto úlohu je ***AggregateDemandHistoryRegion.hql***.

#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*
Tyto [kanály](data-factory/data-factory-create-pipelines.md) obsahují několik aktivity a jehož konečný výsledek je hello skóre predikcím ze experimentu Azure Machine Learning hello přidružená k této šabloně řešení. Jsou téměř shodné s tím rozdílem, každý z nich zpracovává jenom hello jiné oblasti, která se právě provádí jiný RegionID předaná hello ADF kanálu a skriptu hive hello pro každou oblast.  
Hello aktivity obsažené v tomto jsou:

* [HDInsightHive](data-factory/data-factory-hive-activity.md) aktivity pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , spustí podregistru skriptu tooperform agregací a konstruování potřebné pro hello Azure Machine Learning experimentu. Hello Hive skripty pro tuto úlohu se příslušné ***PrepareMLInputRegionX.hql***.
* [Kopírování](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity, která přechází hello výsledky z hello [HDInsightHive](data-factory/data-factory-hive-activity.md) aktivity tooa jednoho Azure blob Storage, může být přístup hello [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivity.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivity, která volá experimentu hello Azure Machine Learning, což vede k hello výsledky se umístit do jediného objektu blob úložiště Azure.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Tyto [kanály](data-factory/data-factory-create-pipelines.md) obsahovat jediné aktivity - [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity, která přechází hello výsledky hello Azure Machine Learning experimentu z příslušných hello ***MLScoringRegionXPipeline *** toohello Azure SQL Database, která zřízenou jako součást instalace šablony hello řešení.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
To [kanály](data-factory/data-factory-create-pipelines.md) obsahovat jediné aktivity - [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity, která přemísťuje hello agregovaná data probíhající vyžádání z ***LoadHistoryDemandDataPipeline*** toohello Azure Databáze SQL, která zřízenou jako součást instalace šablony hello řešení.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyTopologyDataPipeline CopyRegionDataPipeline, CopySubstationDataPipeline,*
Tyto [kanály](data-factory/data-factory-create-pipelines.md) obsahovat jediné aktivity - [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity, která přemísťuje hello referenční data z oblasti nebo transformovny/Topologygeo, které jsou nahrát objekt blob úložiště tooAzure jako součást řešení hello instalace toohello šablony Azure SQL Database, která zřízenou jako součást instalace šablony hello řešení.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimentovat použité pro tuto šablonu řešení poskytuje hello předpovědi vyžádání oblasti. Hello experimentu je konkrétní toohello datové sady spotřebované a proto bude vyžadovat změny nebo nahrazení toohello konkrétní data, která je převeden do stavu v.

## <a name="monitor-progress"></a>**Monitorování průběhu**
Po spuštění hello generátor dat hello kanálu začne tooget HYDRATOVANÝ a hello různé komponenty řešení spusťte si to chtěl / do akce následující příkazy hello vystavený hello Data Factory. Existují dva způsoby, jak můžete monitorovat kanál hello.

1. Zkontrolujte, zda text hello data z Azure Blob Storage.

    Jeden z úlohy služby Stream Analytics hello zapíše hello příchozí data tooblob úložiště. Pokud kliknete na **Azure Blob Storage** součást řešení z hello obrazovce můžete byla úspěšně nasazena hello řešení a potom klikněte na **otevřete** v pravém panelu hello, bude trvat jste toohello [Portál pro správu azure](https://portal.azure.com). Potom klikněte na **objekty BLOB**. Další panelu hello zobrazí se seznam kontejnerů. Klikněte na **"energysadata"**. Další panelu hello, uvidíte hello **"demandongoing"** složky. Hello rawdata ve složce, zobrazí se složky s názvy, například datum = 2016-01-28 atd. Pokud se zobrazí tyto složky, znamená to nezpracovaná data hello je úspěšně probíhá vygenerované v počítači a uložené v úložišti objektů blob. Měli byste vidět soubory, které by měl mít konečné velikosti v MB v těchto složkách.
2. Zkontrolujte, zda text hello data z databáze SQL Azure.

    posledním krokem Hello hello kanálu je toowrite data (např. predikcím ze strojového učení) do databáze SQL. Toowait může mít maximálně 2 hodiny pro tooappear hello dat v databázi SQL. Jedním ze způsobů toomonitor množství dat, je k dispozici ve vaší databázi SQL je prostřednictvím [portál pro správu Azure](https://manage.windowsazure.com/). V levém panelu hello vyhledejte databází SQL![](media/cortana-analytics-technical-guide-demand-forecast/SQLicon2.png) a klikněte na něj. Poté vyhledejte databázi (tj. demo123456db) a klepněte na ni. Na další stránku hello pod **"Connect tooyour databáze"** klikněte na tlačítko **"Dotazy na spuštění Transact-SQL pro svoji databázi SQL"**.

    Zde můžete kliknutím na nový dotaz a dotaz pro hello počet řádků (například "Vyberte count(*) z DemandRealHourly)" s růstem databáze hello počet řádků v tabulce hello měli zvýšit.)
3. Zkontrolujte, zda text hello data z řídicí panel Power BI.

    Můžete nastavit Power BI aktivní trase řídicí panel toomonitor hello nezpracovaná příchozí data. Postupujte podle instrukcí hello v části "Řídicí panel Power BI" hello.

## <a name="power-bi-dashboard"></a>**Řídicí panel Power BI**
### <a name="overview"></a>Přehled
Tato část popisuje, jak tooset až toovisualize řídicí panel Power BI vaší dat v reálném čase ze služby Azure stream analytics (aktivní trase), jako dobře prognózy výsledky z Azure machine learning (neaktivní trase).

### <a name="setup-hot-path-dashboard"></a>Instalační program aktivní trase řídicí panel
Hello následující kroky vás jak dat v reálném čase toovisualize výstup ze služby Stream Analytics úlohy, které byly vygenerovány během hello nasazení řešení. A [Power BI online](http://www.powerbi.com/) účtu je požadovaná tooperform hello následující kroky. Pokud účet nemáte, můžete [vytvořit](https://powerbi.microsoft.com/pricing).

1. Přidáte výstup Power BI v Azure Stream Analytics (ASA).

   * Budete potřebovat pokyny hello toofollow v [Azure Stream Analytics & Power BI: řídicí panel analýzy v reálném čase pro streamování dat v reálném čase viditelnost](stream-analytics/stream-analytics-power-bi-dashboard.md) tooset si výstup hello úlohy Azure Stream Analytics jako vaše napájení BI řídicího panelu.
   * Vyhledejte hello úlohy stream analytics v vaše [portál pro správu Azure](https://manage.windowsazure.com). Název Hello hello úlohy by měl být: YourSolutionName + "streamingjob" + náhodné číslo + "asapbi" (tj. demostreamingjob123456asapbi).
   * Přidání PowerBI výstupu úlohy ASA hello. Sada hello **Alias pro výstup** jako **'PBIoutput'**. Nastavit vaše **název datové sady** a **název tabulky** jako **'EnergyStreamData'**. Po přidání výstup hello, klikněte na tlačítko **"Start"** dole hello v úloze Stream Analytics hello toostart stránku hello. Měli byste obdržet zprávu s potvrzením (*například*, "úlohy analýzy datového proudu počáteční myteststreamingjob12345asablob bylo úspěšné").
2. Přihlaste se příliš[Power BI online](http://www.powerbi.com)

   * Na hello levé části datové sady panelů v části pracovní prostor musí být schopný toosee nové zobrazující datové sady na levém panelu hello Power BI. Toto je hello streamování data, která se instaluje z Azure Stream Analytics v předchozím kroku hello.
   * Ujistěte se, zda text hello ***vizualizace*** podokně je otevřený a je zobrazena na pravé straně úvodní obrazovka.
3. Vytvořte hello dlaždici "Vyžádání podle časového razítka":

   * Klikněte na datovou sadu **'EnergyStreamData'** na hello levý panel oddíl datové sady.
   * Klikněte na tlačítko **"Spojnicový graf"** ikonu ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic8.png).
   * Klikněte na tlačítko 'EnergyStreamData' v **pole** panelu.
   * Klikněte na tlačítko **"Časové razítko"** a zajistěte, aby se zobrazí v části "Osou". Klikněte na tlačítko **"Zatížení"** a zajistěte, aby se zobrazí v části "Hodnoty".
   * Klikněte na tlačítko **Uložit** hello horní a název sestavy hello jako "EnergyStreamDataReport". Hello sestavy s názvem "EnergyStreamDataReport" se zobrazí v sestavách hello Navigátor podokna na levé straně.
   * Klikněte na tlačítko **"Pin Visual"** ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) ikonu v pravém horním rohu tento spojnicový graf, okno "Pin tooDashboard", pravděpodobně zobrazí pro toochoose je řídicí panel. Vyberte "EnergyStreamDataReport" a poté klikněte na tlačítko "Pin".
   * Najeďte myší hello přes tuto dlaždici na řídicím panelu hello, klikněte na položku "upravit" ikonu v pravém horním rohu toochange její název jako "Vyžádání podle časového razítka"
4. Vytvořte další dlaždice řídicího panelu podle příslušné datové sady. zobrazení řídicího panelu konečné Hello jsou uvedeny níže.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PBIFullScreen.png)

### <a name="setup-cold-path-dashboard"></a>Instalační program neaktivní trase řídicí panel
V datovém kanálu neaktivní trase hello nezbytné cílem je, že tooget hello vyžádání prognózy každé oblasti. Power BI se připojí tooan Azure SQL database jako svůj zdroj dat, kde jsou uložené výsledky předpovědi hello.

> [!NOTE]
> 1) Trvá několik hodin toocollect dostatečně prognózy výsledky pro řídicí panel hello. Doporučujeme, aby že tento proces zahájíte 2 – 3 hodiny po něco generátor dat hello. 2) v tomto kroku hello předpokladem je, toodownload a nainstalujte hello bezplatný software [Power BI desktop](https://powerbi.microsoft.com/desktop).
>
>

1. Získáte přihlašovací údaje databáze hello.

   Budete potřebovat **databáze název serveru, název databáze, uživatelské jméno a heslo** před přesunutím toonext kroky. Tady jsou kroky tooguide hello můžete jak toofind je.

   * Jednou **"Azure SQL Database"** v šabloně řešení změní zelená diagramu, klikněte na něj a potom klikněte na **"Otevřené"**. Bude portál pro správu s průvodcem tooAzure a otevře se také stránka informace o vaší databáze.
   * Na stránce hello můžete najít oddíl "Databáze". Vypíše se hello databáze, kterou jste vytvořili. Hello název vaší databáze by měl být **"Váš název řešení + náhodné číslo +"db""** (například "mytest12345db").
   * Klikněte na databázi, v hello nové že POP na panelu, můžete najít název databáze serveru hello nahoře. Název serveru databáze by měla být **"Váš název řešení + náhodné číslo + 'database.windows.net,1433'"** (například "mytest12345.database.windows.net,1433").
   * Vaše databáze **uživatelské jméno** a **heslo** jsou hello stejná hodnota jako hello uživatelské jméno a heslo si dříve poznamenali během nasazení řešení hello.
2. Aktualizovat zdroj dat hello hello neaktivní trase soubor Power BI

   * Ujistěte se, že máte nainstalovanou nejnovější verzi hello [Power BI desktop](https://powerbi.microsoft.com/desktop).
   * V hello **"DemandForecastingDataGeneratorv1.0"** složky jste stáhli, klikněte dvakrát na hello **'Power BI Template\DemandForecastPowerBI.pbix'** souboru. Počáteční vizualizace Hello jsou založené na fiktivní data. **Poznámka:** Pokud se zobrazí chyba masáže, Zkontrolujte prosím, že máte nainstalovanou hello nejnovější verzi nástroje Power BI Desktop.

     Jakmile otevřete, nahoře hello hello souboru, klikněte na tlačítko **'upravit dotazy**. V okně pop hello, poklikejte na **'Source'** na pravém panelu hello.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic1.png)
   * V okně pop hello, nahraďte **"Server"** a **"Databáze"** s vlastními názvy serveru a databáze a pak klikněte na **"OK"**. Pro název serveru, je nutné zadat hello port 1433 (**YourSolutionName.database.windows.net, 1433**). Ignorujte upozornění hello, které se zobrazí na úvodní obrazovka.
   * V další pop hello okně, uvidíte dvě možnosti, v levém podokně hello (**Windows** a **databáze**). Klikněte na tlačítko **"Databáze"**, vyplňte vaše **"Username"** a **"Password"** (Toto je hello uživatelské jméno a heslo, které jste zadali při prvním nasazení hello řešení a vytvoření Azure SQL database). V ***vyberte, které úrovně tooapply těchto nastavení se***, zkontrolujte úrovně možnost databáze. Pak klikněte na tlačítko **"Připojit"**.
   * Jakmile jste na předchozí stránku s průvodcem back toohello, zavřete okno hello. Objeví se zpráva out – klikněte na tlačítko **použít**. Nakonec klikněte na tlačítko hello **Uložit** tlačítko toosave hello změny. Váš soubor Power BI teď bylo navázáno připojení toohello serveru. Pokud vaše vizualizace jsou prázdné, ujistěte se, všechna data hello vymazat výběry hello na hello vizualizace toovisualize kliknutím na ikonu mazání hello na hello pravého horního rohu hello legendy. Použijte hello aktualizace tlačítko tooreflect nová data na hello vizualizace. Na začátku pouze uvidíte hello počáteční hodnoty dat na vaše vizualizace hello data factory je naplánované toorefresh každé 3 hodiny. Po 3 hodiny zobrazí se nové předpovědi promítnuta vaší vizualizace při aktualizaci dat hello.
3. (Volitelné) Publikovat řídicí panel neaktivní trase hello příliš[Power BI online](http://www.powerbi.com/). Všimněte si, že tento krok vyžaduje účet Power BI (nebo účtu Office 365).

   * Klikněte na tlačítko **"Publikovat"** a později několik sekund zobrazí se okno zobrazování "Publikování tooPower BI úspěchu!" s zelená značka zaškrtnutí. Klikněte na odkaz hello níže "Otevřete demoprediction.pbix v Power BI". toofind podrobné pokyny najdete v tématu [publikování z Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * toocreate novým řídicím panelem: klikněte na tlačítko hello  **+**  přihlášení další toothe **řídicí panely** část v levém podokně hello. Zadejte název hello "Vyžádání prognózy ukázku" pro tento nový řídicí panel.
   * Jakmile otevřete hello sestavy, klikněte na tlačítko ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) toopin všechny vizualizace tooyour řídicího panelu. toofind podrobné pokyny najdete v tématu [připnout na řídicí panel dlaždice tooa Power BI ze sestavy](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Přejděte na stránku řídicího panelu toohello a upravte hello velikost a umístění vaší vizualizací a upravit jejich názvy. podrobné pokyny, jak tooedit dlaždice, najdete v části toofind [Upravit vedle sebe - změny velikosti, přesunout, přejmenovat, kódu pin, odstranit, přidání hypertextového odkazu](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Zde je příklad řídicí panel se některé tooit vizualizace připnutý neaktivní trase.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic7.png)
4. (Volitelné) Plán aktualizace zdroje dat hello.

   * aktualizace tooschedule hello dat, umístěte ukazatel myši nad hello **EnergyBPI konečné** datovou sadu, klikněte na tlačítko ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic3.png) a potom zvolte **naplánovat aktualizaci**.
     **Poznámka:** Pokud se zobrazí upozornění masáž, klikněte na tlačítko **upravit pověření** a zajistěte, aby vaše přihlašovací údaje databáze jsou hello stejné jako ty popsané v kroku 1.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic4.png)
   * Rozbalte hello **naplánovat aktualizaci** části. Zapněte "zachovávat aktuálnost dat".
   * Naplánovat aktualizaci hello na základě potřeb. toofind Další informace najdete v tématu [aktualizace dat v Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).

## <a name="how-toodelete-your-solution"></a>**Jak toodelete řešení**
Ověřte, zda zastavit hello generátor dat, při použití nejsou aktivně hello řešení jako spuštění hello generátoru dat bude způsobit vyšší náklady. Pokud nepoužíváte ji odstraňte hello řešení. Odstranění řešení odstraní všechny součásti hello zřízené v rámci vašeho předplatného, při nasazení řešení hello. řešení hello toodelete klikněte na název vašeho řešení v levém panelu hello hello řešení šablony a klikněte na odstranit.

## <a name="cost-estimation-tools"></a>**Náklady odhad nástroje**
Hello následující dva nástroje jsou k dispozici toohelp lépe pochopit celkové náklady spojené s hello vyžádání prognózy pro šablonu řešení energie ve vašem předplatném:

* [Nástroj odhadu náklady na Microsoft Azure (online)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure náklady odhadu Tool (desktop)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Potvrzení**
Tento článek je dílem vědecký data pracovník Svoboda Yijing a softwaru pracovníka Qiu Min v Microsoftu.
