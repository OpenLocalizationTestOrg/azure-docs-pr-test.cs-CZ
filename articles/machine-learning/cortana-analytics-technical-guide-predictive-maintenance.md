---
title: "Údržba aaaPredictive letecký s Azure - Cortana Intelligence řešení technické příručce | Microsoft Docs"
description: "Technické příručce toohello šablona řešení s Microsoft Cortana Intelligence pro prediktivní údržby v letecký, nástrojů a Transport."
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2c4d2147-0f05-4705-8748-9527c2c1f033
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: 30ddc1c101007546ae1b303bccebae3ecdacb442
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="technical-guide-toohello-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Technické příručce toohello Cortana Intelligence řešení šablony pro prediktivní údržby v letecký a jiné firmy

## <a name="important"></a>**Důležité upozornění**
Tento článek je zastaralá. Hello informace se stále relevantní toohello problém, tj. prediktivní údržby v letecký, ale hello nejnovější článek s hello nejvíce až toodate informace můžete najít [zde](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace). 

## <a name="acknowledgements"></a>**Potvrzení**
Tento článek je dílem datových vědců Jan Potokar Gauher Shaheen, Fidan Boylu Uz a softwaru pracovníka Dana Grecoe ve společnosti Microsoft.

## <a name="overview"></a>**Přehled**
Řešení šablony jsou určeny tooaccelerate hello proces vytváření ukázkové E2E nad Cortana Intelligence Suite. Nasazené šablona bude zřídit předplatné s nezbytnými součástmi Cortana Intelligence a sestavení hello vztahy mezi nimi. Je také doplňuje hello datovém kanálu s ukázkovými daty vygenerovaná aplikací generátor dat, které můžete stáhnout a nainstalují na místním počítači po nasazení řešení šablony hello. data Hello vygenerovaná ze hello generátor bude hydrate hello datovém kanálu a spusťte generování předpovědi strojové učení, které lze následně vizualizovat na řídicí panel Power BI hello. proces nasazení Hello vás provede několik kroků tooset si přihlašovací údaje řešení. Ujistěte se, že záznam tyto přihlašovací údaje, jako je například název řešení, uživatelské jméno a heslo, které zadáte během nasazení hello.  

Hello cílem tohoto dokumentu je tooexplain hello referenční architektura a různé součásti, které jsou zřízené v rámci vašeho předplatného v rámci této šablony řešení. dokument Hello také pojednává o tom, tooreplace ukázková data s reálná data toobe možné toosee insights a predikcím ze svoje vlastní data. Kromě toho hello dokumentu popisuje části hello šablona řešení, které byste potřebovali změnit, pokud byste chtěli toocustomize hello řešení s vlastními daty toobe. Pokyny, jak toobuild hello řídicí panel Power BI pro tuto šablonu řešení najdete na konci hello.

> [!TIP]
> Můžete stáhnout a vytisknout [PDF verzi tohoto dokumentu](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).
> 
> 

## <a name="big-picture"></a>**Celkový přehled**
![Architektura prediktivní údržby](media/cortana-analytics-technical-guide-predictive-maintenance/predictive-maintenance-architecture.png)

Při nasazení hello řešení, jsou aktivované různé služby Azure v rámci Cortana Analytics Suite (*tj.* Centra událostí, Stream Analytics, HDInsight, Data Factory strojového učení, *atd*). diagram architektury Hello výše ukazuje na vysoké úrovni, jak sestavený hello prediktivní Údržba leteckých řešení šablony ze začátku do konce. Bude moct tooinvestigate tyto služby na portálu hello azure klepnutím na ně v diagramu šablonu řešení hello vytvořené pomocí nasazení hello hello řešení s výjimkou hello HDInsight jako tato služba je zřízený na vyžádání při související hello kanál aktivity jsou požadované toorun a později odstranit.
Můžete si stáhnout [plné velikosti verzi hello diagram](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

Hello následující oddíly popisují každou část.

## <a name="data-source-and-ingestion"></a>**Zdroj dat a přijímání**
### <a name="synthetic-data-source"></a>Zdroj dat syntetického
Pro tato data hello šablony zdroje využívaného generují z aplikace pracovní plochy, který bude stáhnout a spustit místně po úspěšné nasazení. Bude najít toodownload hello pokyny a nainstalovat aplikaci panelu hello vlastnosti, když vyberete hello první uzel s názvem generátor dat prediktivní údržby v diagramu šablonu řešení hello. Tuto aplikaci kanály hello [centra událostí Azure](#azure-event-hub) služby s datových bodů nebo událostí, které se použijí v hello zbytek hello řešení toku. Tento zdroj dat je tvořená nebo odvozené od veřejně dostupnými daty z [datové úložiště NASA](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) pomocí hello [Turbofan Engine Degradation Simulation Data Set](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

aplikace generování událostí Hello naplnit hello centra událostí Azure pouze tehdy, když je prováděna v počítači.

### <a name="azure-event-hub"></a>Centra událostí Azure
Hello [centra událostí Azure](https://azure.microsoft.com/services/event-hubs/) služba je hello příjemce hello vstup hello syntetické zdroje dat popsané výše.

## <a name="data-preparation-and-analysis"></a>**Příprava dat a analýzy**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) služba je použité tooprovide téměř v reálném čase analýzy hello vstupního datového proudu z hello [centra událostí Azure](#azure-event-hub) služby a publikovat výsledky do [Power BI](https://powerbi.microsoft.com) řídicí panel, jakož i archivace všechny nezpracované příchozí události toohello [Azure Storage](https://azure.microsoft.com/services/storage/) služby pro pozdější zpracování pomocí hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) služby.

### <a name="hd-insights-custom-aggregation"></a>Vlastní agregační HD statistiky
Hello služby Azure HD Insight je použité toorun [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) agregace tooprovide skripty (řízená Azure Data Factory) na hello nezpracované události, které byly archivovat pomocí služby Azure Stream Analytics hello.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (řízená Azure Data Factory) se používá služba toomake předpovědi na hello zbývající dobu životnosti (RUL) konkrétní leteckého motoru, zadané vstupy hello přijata.

## <a name="data-publishing"></a>**Publikování dat**
### <a name="azure-sql-database-service"></a>Služba Azure SQL Database
Hello [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) služba je použité toostore (spravované službou Azure Data Factory) hello předpovědi přijatých hello službu Azure Machine Learning, které bude potřeba v hello [Power BI](https://powerbi.microsoft.com) řídicího panelu.

## <a name="data-consumption"></a>**Spotřeba dat**
### <a name="power-bi"></a>Power BI
Hello [Power BI](https://powerbi.microsoft.com) služba je použité tooshow řídicí panel, který obsahuje agregace a výstrahy poskytované hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) služby a také RUL předpovědi uložené v [Azure SQL Databáze](https://azure.microsoft.com/services/sql-database/) které byly vytvořeny pomocí hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) služby. Návod, jak toobuild hello řídicí panel Power BI pro tuto šablonu řešení najdete v části toohello níže.

## <a name="how-toobring-in-your-own-data"></a>**Jak toobring v svoje vlastní data**
Tato část popisuje, jak toobring tooAzure vlastní data a co by vyžadovaly oblasti změny dat hello můžete převést do této architektury.

Není pravděpodobné, že žádné datové sady přepnutím odpovídá hello datové sady používané hello [Turbofan Engine Degradation Simulation Data Set](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) použít pro tuto šablonu řešení. Seznámení s daty a požadavky na bude velmi důležitý v tom, jak upravit tuto šablonu toowork s vlastními daty. Pokud je to první ohrožení toohello službu Azure Machine Learning, můžete získat Úvod tooit pomocí příkladu hello [jak toocreate prvního experimentu](machine-learning-create-experiment.md).

Následující části Hello zabývat hello části hello šablony, který bude vyžadovat změny, pokud je zavedená nová datová sada.

### <a name="azure-event-hub"></a>Centra událostí Azure
Hello centra událostí Azure service je velmi obecná tak, aby se data můžou být publikované toohello rozbočovače ve formátu CSV nebo formátu JSON. Žádné speciální zpracování dojde v hello centra událostí Azure, ale je důležité, abyste rozuměli tomu hello data, která je dodáni do ní.

Tento dokument nepopisuje, jak tooingest vaše data, ale můžete snadno odesílat události nebo data tooan centra událostí Azure pomocí hello API centra událostí.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hello služby Azure Stream Analytics, je použít tooprovide téměř v reálném čase analytics čtení z datových proudů a výstupy data tooany počet zdrojů.

Pro hello prediktivní Údržba leteckých řešení šablony Azure Stream Analytics dotazu se skládá z čtyři poddotazech, každý náročné události z hello centra událostí Azure service která mají výstupy do čtyř různých umístění. Tyto výstupy obsahovat tři datové sady Power BI a jedno umístění úložiště Azure.

Hello Azure Stream Analytics dotazu lze najít:

* Protokolování do hello portálu Azure
* Vyhledání úlohy Stream Analytics hello ![Stream Analytics ikonu](media/cortana-analytics-technical-guide-predictive-maintenance/icon-stream-analytics.png) které byly generovány při řešení hello nasazená (*například*, **maintenancesa02asapbi** a **maintenancesa02asablob** pro řešení prediktivní údržby)
* Výběr
  
  * ***VSTUPY*** tooview hello dotazu vstup
  * ***DOTAZ*** samotný dotaz tooview hello
  * ***VÝSTUPY*** tooview hello různých výstupy

Informace o vytváření dotazů Azure Stream Analytics lze najít v hello [referenční příručka Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx) na webu MSDN.

V tomto řešení výstup hello dotazy tři datové sady s téměř analýzu v reálném čase informace o hello příchozí data datového proudu tooa Power BI řídicí panel, který je dodáván jako součást této šablony řešení. Vzhledem k tomu, že je implicitní znalosti o hello příchozí data formátu, tyto dotazy zapotřebí toobe změněna na základě formátu vaše data.

Hello dotazu v druhé úloze Stream Analytics hello **maintenancesa02asablob** jednoduše všechny výstupy [centra událostí](https://azure.microsoft.com/services/event-hubs/) události [Azure Storage](https://azure.microsoft.com/services/storage/) a proto vyžaduje žádné změnou bez ohledu na formát vaše data jako hello je informací o úplné události Streamovat toostorage.

### <a name="azure-data-factory"></a>Azure Data Factory
Hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) služeb orchestruje hello pohybů a zpracování dat. V prediktivní Údržba leteckých šablona řešení hello data objektu pro vytváření se skládá ze tří [kanály](../data-factory/data-factory-create-pipelines.md) , přesunout a zpracování dat hello pomocí různých technologií.  Objekt pro vytváření dat může přístup otevřením hello hello Data Factory uzlu dole hello v diagramu šablonu řešení hello vytvořené pomocí nasazení hello hello řešení. Bude to trvat můžete toohello objekt pro vytváření dat na portálu Azure. Pokud se zobrazí chyby v rámci vaší datové sady, ty můžete ignorovat jsou z důvodu toodata factory nasazuje před hello generátor dat byla spuštěna. Tyto chyby, nebudou bránit datovou továrnu nebude fungovat.

![Chyby datové sady objektu pro vytváření dat](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Tato část popisuje hello potřeby [kanály](../data-factory/data-factory-create-pipelines.md) a [aktivity](../data-factory/data-factory-create-pipelines.md) obsažené v hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Níže je zobrazení diagramu hello hello řešení.

![Azure Data Factory](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Dva kanály hello tohoto objektu pro vytváření obsahovat [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripty, které jsou používané toopartition a hello agregovaná data. Pokud už jsme zmínili, hello skripty se bude nacházet v hello [Azure Storage](https://azure.microsoft.com/services/storage/) účet vytvořený během instalace. Jejich umístění bude: maintenancesascript\\\\skriptu\\\\hive\\ \\ (nebo name].blob.core.windows.net/maintenancesascript https://[Your řešení).

Podobně jako toohello [Azure Stream Analytics](#azure-stream-analytics-1) dotazy, [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripty mají implicitní znalosti o hello příchozí data formátu, tyto dotazy potřebovat toobe změnit na základě formátu data a [konstruování](machine-learning-feature-selection-and-engineering.md) požadavky.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*
To [kanálu](../data-factory/data-factory-create-pipelines.md) obsahuje jediné aktivity – [HDInsightHive](../data-factory/data-factory-hive-activity.md) aktivity pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , která se spouští [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) data hello toopartition skriptu umístit do [Azure Storage](https://azure.microsoft.com/services/storage/) během [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) úlohy.

[Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript pro toto rozdělení úloh je ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
To [kanálu](../data-factory/data-factory-create-pipelines.md) obsahuje několik aktivit a jehož konečný výsledek je hello skóre pro magnitudu predikcím ze hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimentu přidružená k této šabloně řešení.

Hello aktivity obsažené v tomto jsou:

* [HDInsightHive](../data-factory/data-factory-hive-activity.md) aktivity pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , která se spouští [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skriptu tooperform agregací a konstruování potřebné pro hello [Azure Strojového učení](https://azure.microsoft.com/services/machine-learning/) experiment.
  [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript pro toto rozdělení úloh je ***PrepareMLInput.hql***.
* [Kopírování](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity, která přemísťuje hello výsledky z [HDInsightHive](../data-factory/data-factory-hive-activity.md) jedné aktivity tooa [Azure Storage](https://azure.microsoft.com/services/storage/) objekt blob, který může být přístup [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivity.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivity, která volá hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment, což vede k hello výsledky provádí požadavek put v jediném [Azure Storage](https://azure.microsoft.com/services/storage/) objektů blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
To [kanálu](../data-factory/data-factory-create-pipelines.md) obsahuje jediné aktivity - [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity, která přemísťuje hello výsledky hello [Azure Machine Learning](#azure-machine-learning) experimentovat z *** MLScoringPipeline*** toohello [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) který zřízenou jako součást instalace šablony řešení.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment používá pro tato šablona řešení poskytuje hello zbývající doby životnosti (RUL) leteckého motoru. Hello experimentu je konkrétní toohello datové sady spotřebované a proto bude vyžadovat změny nebo nahrazení toohello konkrétní data, která je převeden do stavu v.

Informace o vytváření hello Azure Machine Learning experimentu najdete v tématu [prediktivní údržby: Krok 1 ze 3, Příprava dat a funkce konstruování](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Monitorování průběhu**
 Po spuštění hello generátor dat hello kanálu začne tooget HYDRATOVANÝ a hello různé komponenty řešení spusťte si to chtěl / do akce následující příkazy hello vystavený hello Data Factory. Existují dva způsoby, jak můžete monitorovat kanál hello.

1. Jeden z úlohy služby Stream Analytics hello zapíše hello příchozí data tooblob úložiště. Pokud klepnete na úložiště objektů Blob součást řešení z úvodní obrazovka vám byla úspěšně nasazena hello řešení a potom klikněte na Otevřít v pravém panelu hello, bude trvat jste toohello [portálu pro správu](https://portal.azure.com/). Potom klikněte na objekty BLOB. Další panelu hello zobrazí se seznam kontejnerů. Klikněte na **maintenancesadata**. Další panelu hello, uvidíte hello **rawdata** složky. Hello rawdata ve složce, zobrazí se složky s názvy, například hodinu = 17 hodinu = 18 atd. Pokud se zobrazí tyto složky, znamená to nezpracovaná data hello je úspěšně probíhá vygenerované v počítači a uložené v úložišti objektů blob. Měli byste vidět soubory sdíleného svazku clusteru, které by měl mít konečné velikosti v MB v těchto složkách.
2. posledním krokem Hello hello kanálu je toowrite data (např. predikcím ze strojového učení) do databáze SQL. Toowait může mít maximálně tři hodiny pro tooappear hello dat v databázi SQL. Jedním ze způsobů toomonitor množství dat, je k dispozici ve vaší databázi SQL je prostřednictvím [portál azure](https://manage.windowsazure.com/). V levém panelu hello vyhledejte databází SQL ![SQL ikonu](media/cortana-analytics-technical-guide-predictive-maintenance/icon-SQL-databases.png) a klikněte na něj. Vyhledejte databázi **pmaintenancedb** a klepněte na ni. Na další stránku hello dolnímu hello klikněte na SPRAVOVAT
   
    ![Ikona Správa](media/cortana-analytics-technical-guide-predictive-maintenance/icon-manage.png).
   
    Zde můžete kliknutím na nový dotaz a dotaz pro hello počet řádků (například vyberte count(*) z PMResult). S růstem databáze hello počet řádků v tabulce hello měli zvýšit.

## <a name="power-bi-dashboard"></a>**Řídicí panel Power BI**
### <a name="overview"></a>Přehled
Tato část popisuje, jak tooset až toovisualize řídicí panel Power BI vaší dat v reálném čase z Azure Stream Analytics (aktivní trase), jakož i předpovědi batch výsledky z Azure machine learning (neaktivní trase).

### <a name="setup-cold-path-dashboard"></a>Instalační program neaktivní trase řídicí panel
V datovém kanálu neaktivní trase hello hello nezbytné cílem je tooget prediktivní RUL (zbývající životnost) každý motor letadla po jeho dokončení letu (cyklus). výsledek předpovědi Hello se aktualizuje každých 3 hodiny pro predikci hello letecké motory, které dokončily letu během hello posledních 3 hodiny.

Power BI se připojí tooan Azure SQL database jako svůj zdroj dat, kde jsou uložené výsledky předpovědi. Poznámka: 1) při nasazení řešení, skutečné předpovědi se zobrazí v databázi hello v rámci 3 hodiny.
soubor pbix Hello, dodané s hello generátor stažení obsahuje některá data počáteční hodnoty, takže můžete hned vytvořit řídicí panel Power BI hello. 2) v tomto kroku hello předpokladem je, toodownload a nainstalujte hello bezplatný software [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Hello následující kroky vám pomohou na jak soubor tooconnect hello pbix do hello SQL Database, která se spouštějí v době hello řešení nasazení obsahující data (*například*. výsledky předpovědi) pro vizualizaci.

1. Získáte přihlašovací údaje databáze hello.
   
   Budete potřebovat **databáze název serveru, název databáze, uživatelské jméno a heslo** před přesunutím toonext kroky. Tady jsou kroky tooguide hello můžete jak toofind je.
   
   * Jednou **databáze SQL Azure** v šabloně řešení změní zelená diagramu, klikněte na něj a potom klikněte na **, otevřete'**.
   * Zobrazí se nové kartě nebo okno prohlížeče, který zobrazí hello stránky portálu Azure. Klikněte na tlačítko **'skupiny prostředků,** na levém panelu hello.
   * Vyberte předplatné hello používáte pro nasazení hello řešení a potom vyberte **' YourSolutionName\_ResourceGroup'**.
   * V nové pop hello se panel, klikněte na tlačítko hello ![SQL ikonu](media/cortana-analytics-technical-guide-predictive-maintenance/icon-sql.png) ikonu tooaccess vaší databáze. Název databáze je další toohello tato ikona (*například*, **'pmaintenancedb'**) a hello **název databázového serveru** je uveden v části hello vlastnost název serveru a by měl vypadat Podobně jako příliš**YourSoutionName.database.windows.net**.
   * Vaše databáze **uživatelské jméno** a **heslo** jsou hello stejná hodnota jako hello uživatelské jméno a heslo si dříve poznamenali během nasazení řešení hello.
2. Aktualizujte zdroj dat hello hello neaktivní trase sestavy souboru s Power BI Desktop.
   
   * Ve složce hello ve vašem počítači, které jste stáhli a unzipped soubor generátor, dvakrát klikněte **PowerBI\\PredictiveMaintenanceAerospace.pbix** souboru. Pokud se zobrazí všechny zprávy upozornění při otevření souboru hello, je ignorujte. V horní části hello hello souboru, klikněte na tlačítko **'upravit dotazy**.
     
     ![Úpravy dotazů](media/cortana-analytics-technical-guide-predictive-maintenance/edit-queries.png)
   * Uvidíte dvě tabulky **RemainingUsefulLife** a **PMResult**. Vyberte hello první tabulky a klikněte na ![ikona nastavení dotazu](media/cortana-analytics-technical-guide-predictive-maintenance/icon-query-settings.png) další příliš**'Source'** pod **"použít postup** na pravém hello **'Nastavení dotazu'**panelu. Ignorujte všechny zprávy upozornění, které se zobrazují.
   * V okně pop hello, nahraďte **"Server"** a **'Databázi'** s vlastními názvy serveru a databáze a pak klikněte na **"OK"**. Pro název serveru, je nutné zadat hello port 1433 (**YourSoutionName.database.windows.net, 1433**). Ponechejte pole databáze hello jako **pmaintenancedb**. Ignorujte upozornění hello, které se zobrazí na úvodní obrazovka.
   * V další pop hello okně, uvidíte dvě možnosti, v levém podokně hello (**Windows** a **databáze**). Klikněte na tlačítko **'Databázi'**, vyplňte vaše **'Uživatelského jména'** a **'Password'** (Toto je hello uživatelské jméno a heslo, které jste zadali při prvním nasazení hello řešení a vytvoření Azure SQL database). V ***vyberte, které úrovně tooapply těchto nastavení se***, zkontrolujte úrovně možnost databáze. Pak klikněte na tlačítko **"Připojit"**.
   * Klikněte na druhé tabulky hello **PMResult** klikněte ![navigační ikonu](media/cortana-analytics-technical-guide-predictive-maintenance/icon-navigation.png) další příliš**'Source'** pod **"použít postup** na pravém hello **'Nastavení dotazu'** panelu a aktualizovat hello server a názvy databází jako hello výše kroky a klikněte na tlačítko OK.
   * Jakmile jste na předchozí stránku s průvodcem back toohello, zavřete okno hello. Objeví se zpráva out – klikněte na tlačítko **použít**. Nakonec klikněte na tlačítko hello **Uložit** tlačítko toosave hello změny. Váš soubor Power BI teď bylo navázáno připojení toohello serveru. Pokud vaše vizualizace jsou prázdné, ujistěte se, všechna data hello vymazat výběry hello na hello vizualizace toovisualize kliknutím na ikonu mazání hello na hello pravého horního rohu hello legendy. Použijte hello aktualizace tlačítko tooreflect nová data na hello vizualizace. Na začátku pouze uvidíte hello počáteční hodnoty dat na vaše vizualizace hello data factory je naplánované toorefresh každé 3 hodiny. Po 3 hodiny zobrazí se nové předpovědi promítnuta vaší vizualizace při aktualizaci dat hello.
3. (Volitelné) Publikovat řídicí panel neaktivní trase hello příliš[Power BI online](http://www.powerbi.com/). Všimněte si, že tento krok vyžaduje účet Power BI (nebo účtu Office 365).
   
   * Klikněte na tlačítko **"Publikovat"** a později několik sekund zobrazí se okno zobrazování "Publikování tooPower BI úspěchu!" s zelená značka zaškrtnutí. Klikněte na odkaz hello níže "Otevřete PredictiveMaintenanceAerospace.pbix v Power BI". toofind podrobné pokyny najdete v tématu [publikování z Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * toocreate novým řídicím panelem: klikněte na tlačítko hello  **+**  přihlášení další toothe **řídicí panely** část v levém podokně hello. Zadejte název hello "Prediktivní údržby ukázku" pro tento nový řídicí panel.
   * Jakmile otevřete hello sestavy, klikněte na tlačítko ![ikonu PŘIPNUTÍ](media/cortana-analytics-technical-guide-predictive-maintenance/icon-pin.png) toopin všechny vizualizace tooyour řídicího panelu. toofind podrobné pokyny najdete v tématu [připnout na řídicí panel dlaždice tooa Power BI ze sestavy](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Přejděte na stránku řídicího panelu toohello a upravte hello velikost a umístění vaší vizualizací a upravit jejich názvy. podrobné pokyny, jak tooedit dlaždice, najdete v části toofind [Upravit vedle sebe - změny velikosti, přesunout, přejmenovat, kódu pin, odstranit, přidání hypertextového odkazu](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Zde je příklad řídicí panel se některé tooit vizualizace připnutý neaktivní trase.  V závislosti na tom, jak dlouho spustíte generátor vaše data může být vaše čísla na hello vizualizace jiný.
     <br/>
     ![Poslední zobrazení](media/cortana-analytics-technical-guide-predictive-maintenance/final-view.png)
     <br/>
   * aktualizace tooschedule hello dat, umístěte ukazatel myši nad hello **PredictiveMaintenanceAerospace** datovou sadu, klikněte na tlačítko ![tří teček ikonu](media/cortana-analytics-technical-guide-predictive-maintenance/icon-elipsis.png) a potom zvolte **naplánovat aktualizaci**.
     <br/>
     **Poznámka:** Pokud se zobrazí upozornění masáž, klikněte na tlačítko **upravit pověření** a zajistěte, aby vaše přihlašovací údaje databáze jsou hello stejné jako ty popsané v kroku 1.
     <br/>
     ![Plán aktualizace](media/cortana-analytics-technical-guide-predictive-maintenance/schedule-refresh.png)
     <br/>
   * Rozbalte hello **naplánovat aktualizaci** části. Zapněte "zachovávat aktuálnost dat".
     <br/>
   * Naplánovat aktualizaci hello na základě potřeb. toofind Další informace najdete v tématu [aktualizace dat v Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Instalační program aktivní trase řídicí panel
Hello následující kroky vás jak dat v reálném čase toovisualize výstup ze služby Stream Analytics úlohy, které byly vygenerovány během hello nasazení řešení. A [Power BI online](http://www.powerbi.com/) účtu je požadovaná tooperform hello následující kroky. Pokud účet nemáte, můžete [vytvořit](https://powerbi.microsoft.com/pricing).

1. Přidáte výstup Power BI v Azure Stream Analytics (ASA).
   
   * Budete potřebovat pokyny hello toofollow v [Azure Stream Analytics & Power BI: řídicí panel analýzy v reálném čase pro streamování dat v reálném čase viditelnost](../stream-analytics/stream-analytics-power-bi-dashboard.md) tooset si výstup hello úlohy Azure Stream Analytics jako vaše napájení BI řídicího panelu.
   * Hello ASA dotaz má tři výstupů, které jsou **aircraftmonitor**, **aircraftalert**, a **flightsbyhour**. Dotaz hello můžete zobrazit kliknutím na kartu dotaz. Odpovídající tooeach těchto tabulek, budete potřebovat tooadd tooASA výstupu. Když přidáte první výstup hello (*například* **aircraftmonitor**) ujistěte se, zda text hello **Alias pro výstup**, **název datové sady** a  **Název tabulky** jsou stejné hello (**aircraftmonitor**). Opakování hello kroky tooadd vstupů pro **aircraftalert**, a **flightsbyhour**. Po přidání všech tří výstupní tabulky a spustit úlohy ASA hello, měli byste obdržet zprávu s potvrzením (*například*, "Úlohy analýzy datového proudu počáteční maintenancesa02asapbi bylo úspěšné").
2. Přihlaste se příliš[Power BI online](http://www.powerbi.com)
   
   * Na levé části datové sady panelů v pracovním prostoru hello ***datovou sadu*** názvy **aircraftmonitor**, **aircraftalert**, a **flightsbyhour**by se měla objevit. Toto je hello streamování data odesílána z Azure Stream Analytics v datové sadě předchozí pro step.hello hello **flightsbyhour** pravděpodobně nezobrazí v hello stejný čas hello kvůli toohello povaha dotazu SQL hello za další dvě datové sady ho. Ale by měl zobrazit za hodinu.
   * Ujistěte se, zda text hello ***vizualizace*** podokně je otevřený a je zobrazena na pravé straně úvodní obrazovka.
3. Jakmile máte hello dat odesílaných do Power BI, můžete začít vizualizace hello streamovaných dat užitečné. Níže je, že tooit připnuli příklad řídicí panel s vizualizacemi některé aktivní trase. Můžete vytvořit další dlaždice řídicího panelu podle příslušné datové sady. V závislosti na tom, jak dlouho spustíte generátor vaše data může být vaše čísla na hello vizualizace jiný.

    ![Zobrazení řídicího panelu](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

1. Tady jsou některé kroky toocreate jednu z výše uvedených – hello dlaždice hello "loďstva zobrazení senzor 11 vs. Prahová hodnota šířka 48,26" dlaždice:
   
   * Klikněte na datovou sadu **aircraftmonitor** na hello levý panel oddíl datové sady.
   * Klikněte na tlačítko hello **spojnicový graf** ikonu.
   * Klikněte na tlačítko **zpracovaných** v hello **pole** podokně tak, aby se zobrazí v části "Osou" v hello **vizualizace** podokně.
   * Klikněte na tlačítko "s.11" a "s.11\_výstraha" tak, aby obě vyskytovat v části "Hodnoty". Klikněte na šipku malé hello další příliš**s.11** a **s.11\_výstraha**, změňte "Sum" příliš "průměrné".
   * Klikněte na tlačítko **Uložit** hello horní a název sestavy hello "aircraftmonitor". Sestavy s názvem "aircraftmonitor" bude zobrazen v hello **sestavy** část v hello **Navigátor** podokna na levé straně hello.
   * Klikněte na tlačítko hello **Pin Visual** ikonu na hello pravém horním rohu tohoto spojnicového grafu. Okno "Pin tooDashboard" může zobrazovat můžete vybrat řídicí panel. Vyberte "Prediktivní údržby ukázku" a potom klikněte na tlačítko "Pin".
   * Najeďte myší hello tuto dlaždici na řídicím panelu hello, klikněte na tlačítko hello "upravit" ikonu v pravém horním rohu toochange hello její název příliš "firemního vozového zobrazení Sensor 11 vs. Prahová hodnota šířka 48,26" a subtitle příliš"Průměrná napříč firemního vozového v čase".

## <a name="how-toodelete-your-solution"></a>**Jak toodelete řešení**
Ověřte, zda zastavit hello generátor dat, při použití nejsou aktivně hello řešení jako spuštění hello generátoru dat bude způsobit vyšší náklady. Pokud nepoužíváte ji odstraňte hello řešení. Odstranění řešení odstraní všechny součásti hello zřízené v rámci vašeho předplatného, při nasazení řešení hello. řešení hello toodelete klikněte na název vašeho řešení v levém panelu hello hello řešení šablony a klikněte na odstranit.

## <a name="cost-estimation-tools"></a>**Odhad nákladů nástroje**
Hello následující dva nástroje jsou k dispozici toohelp lépe pochopit celkové náklady spojené s hello prediktivní Údržba leteckých řešení šablony ve vašem předplatném:

* [Nástroj odhadu náklady na Microsoft Azure (online)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure náklady odhadu Tool (desktop)](http://www.microsoft.com/download/details.aspx?id=43376)

