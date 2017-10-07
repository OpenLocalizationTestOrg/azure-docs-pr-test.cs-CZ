---
title: "aaaAzure objekt pro vytváření dat – nejčastější dotazy"
description: "Časté otázky k Azure Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 78289fb4b6e15d74772af6c71ec25c7d2ca1a0bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---frequently-asked-questions"></a>Pro vytváření dat Azure – nejčastější dotazy
## <a name="general-questions"></a>Obecné otázky
### <a name="what-is-azure-data-factory"></a>Co je služba Azure Data Factory?
Data Factory je cloudových dat integrační služby, který **automatizuje hello přesouvání a transformaci dat**. Stejně jako objekt factory, který spouští zařízení tootake suroviny a transformují je na hotové výroby Data Factory orchestruje stávající služby, které sbírají nezpracovaná data a transformují je na informace připravené k použití.

Objekt pro vytváření dat umožňuje toocreate řízené daty pracovních toomove dat mezi místní a cloudové úložiště dat jak procesu nebo transformace dat pomocí výpočetních služeb například Azure HDInsight a Azure Data Lake Analytics. Po vytvoření kanálu, který provádí hello akce, které potřebujete, můžete ji naplánovat toorun pravidelně (každou hodinu, denně, týdně atd.).   

Další informace najdete v tématu [přehled & klíčové koncepty](data-factory-introduction.md).

### <a name="where-can-i-find-pricing-details-for-azure-data-factory"></a>Kde můžete najít podrobnosti o cenách pro vytváření dat Azure?
V tématu [stránku Podrobnosti o cenách na Data Factory] [ adf-pricing-details] pro hello ceny pro hello Azure Data Factory.  

### <a name="how-do-i-get-started-with-azure-data-factory"></a>Jak můžu začít pracovat s Azure Data Factory?
* Přehled Azure Data Factory najdete v tématu [Úvod tooAzure Data Factory](data-factory-introduction.md).
* Kurz týkající se jak příliš**zkopírovat nebo přesunout data** pomocí aktivity kopírování, najdete v tématu [kopírování dat z Azure Blob Storage tooAzure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Kurz týkající se jak příliš**transformovat data** pomocí aktivity HDInsight Hive. V tématu [zpracování dat pomocí skriptu Hive v clusteru Hadoop](data-factory-build-your-first-pipeline.md)

### <a name="what-is-hello-data-factorys-region-availability"></a>Co je dostupnost hello Data Factory v oblastech?
Objekt pro vytváření dat je k dispozici v **USA – západ** a **Severní Evropa**. Hello výpočetní a služby úložiště používá objekty pro vytváření dat může být v jiných oblastech. V tématu [podporované oblasti](data-factory-introduction.md#supported-regions).

### <a name="what-are-hello-limits-on-number-of-data-factoriespipelinesactivitiesdatasets"></a>Jaká jsou omezení hello na několik data Factory/kanálů nebo aktivity nebo datových sad?
V tématu **Azure Data Factory omezení** části hello [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md#data-factory-limits) článku.

### <a name="what-is-hello-authoringdeveloper-experience-with-azure-data-factory-service"></a>Co je rozhraní pro vytváření/vývojáře hello službou Azure Data Factory?
Můžete vytvořit nebo vytvořit datové továrny pomocí jedné z následujících nástrojů nebo sady SDK hello:

* **Portál Azure** hello okna objekt pro vytváření dat v hello portál Azure poskytují bohaté uživatelské rozhraní pro vás toocreate datové továrny ad propojené služby. Hello **editoru služby Data Factory**, což je také součástí hello portál, můžete tooeasily vytvoření propojené služby, tabulky, datových sad a kanálů zadáním definice JSON pro tyto artefakty. V tématu [sestavit svůj první kanál dat pomocí portálu Azure](data-factory-build-your-first-pipeline-using-editor.md) pro příklad použití hello toocreate portal/editoru a nasazení služby data factory.
* **Visual Studio** můžete použít Visual Studio toocreate služby Azure data factory. V tématu [sestavit svůj první kanál dat pomocí sady Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) podrobnosti.
* **Prostředí Azure PowerShell** najdete v části [vytvořit a monitorování Azure Data Factory pomocí Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md) kurzu nebo návod pro vytvoření objekt pro vytváření dat pomocí prostředí PowerShell. V tématu [referenční Data Factory] [ adf-powershell-reference] obsahu v knihovně MSDN úplnou dokumentaci o rutinách služby Data Factory.
* **Knihovna tříd rozhraní .NET** prostřednictvím kódu programu můžete vytvořit datové továrny pomocí .NET SDK služby Data Factory. V tématu [vytvořit, sledovat a spravovat data Factory pomocí sady .NET SDK](data-factory-create-data-factories-programmatically.md) pro návod, jak vytvořit objekt pro vytváření dat pomocí sady .NET SDK. V tématu [Reference knihovny tříd pro objekt pro vytváření dat] [ msdn-class-library-reference] úplnou dokumentaci o .NET SDK služby Data Factory.
* **Rozhraní REST API** můžete také použít hello vystavené toocreate služby Azure Data Factory hello rozhraní REST API a nasadit datové továrny. V tématu [referenci rozhraní API REST pro vytváření dat] [ msdn-rest-api-reference] úplnou dokumentaci o REST API služby Data Factory.
* **Šablona Azure Resource Manageru** najdete v části [kurz: vytvoření vaší první pro vytváření dat Azure pomocí šablony Azure Resource Manager](data-factory-build-your-first-pipeline-using-arm.md) fo podrobnosti.

### <a name="can-i-rename-a-data-factory"></a>Můžete přejmenovat objekt pro vytváření dat?
Ne. Jako další prostředky Azure nelze změnit název hello služby Azure data factory.

### <a name="can-i-move-a-data-factory-from-one-azure-subscription-tooanother"></a>Můžete přesunout objekt pro vytváření dat z jedné tooanother předplatné Azure?
Ano. Použití hello **přesunout** tlačítko na vaše okno objekt pro vytváření dat, jak je znázorněno v následujícím diagramu hello:

![Přesunout objekt pro vytváření dat](media/data-factory-faq/move-data-factory.png)

### <a name="what-are-hello-compute-environments-supported-by-data-factory"></a>Jaké jsou hello výpočetních prostředích podporovaných službou Data Factory?
Hello následující tabulka obsahuje seznam výpočetních prostředích nepodporuje objekt pro vytváření dat a hello aktivity, které můžou běžet na ně.

| Výpočetní prostředí | activities |
| --- | --- |
| [Cluster HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) nebo [vlastní cluster HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [streamování Hadoop](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](data-factory-compute-linked-services.md#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](data-factory-compute-linked-services.md#azure-machine-learning-linked-service) |[Aktivity Machine Learning: Dávkové spouštění a Aktualizace prostředku](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](data-factory-compute-linked-services.md#azure-data-lake-analytics-linked-service) |[U-SQL Data Lake Analytics](data-factory-usql-activity.md) |
| [Azure SQL](data-factory-compute-linked-services.md#azure-sql-linked-service), [Azure SQL Data Warehouse](data-factory-compute-linked-services.md#azure-sql-data-warehouse-linked-service), [systému SQL Server](data-factory-compute-linked-services.md#sql-server-linked-service) |[Uložená procedura](data-factory-stored-proc-activity.md) |

### <a name="how-does-azure-data-factory-compare-with-sql-server-integration-services-ssis"></a>Jak Azure Data Factory porovnat s integrační služby SSIS (SQL Server)? 
V tématu hello [vs Azure Data Factory. SSIS](http://www.sqlbits.com/Sessions/Event15/Azure_Data_Factory_vs_SSIS) prezentace z jednoho z našich MVP (většina cenná Odborníci v oblasti): Reza Rad. Některé hello poslední změny v objektu pro vytváření dat nemusí být uvedena v balíček snímků hello. Nepřetržitě přidáváme další možnosti tooAzure Data Factory. Nepřetržitě přidáváme další možnosti tooAzure Data Factory. Jsme se začlenit tyto aktualizace do hello porovnání technologiemi společnosti Microsoft pro integraci dat zopakovat později tohoto roku.   

## <a name="activities---faq"></a>Aktivity – nejčastější dotazy
### <a name="what-are-hello-different-types-of-activities-you-can-use-in-a-data-factory-pipeline"></a>Jaké jsou hello různé typy aktivit, které můžete použít v kanálu pro vytváření dat?
* [Aktivity přesunu dat](data-factory-data-movement-activities.md) toomove data.
* [Aktivity transformace dat](data-factory-data-transformation-activities.md) tooprocess nebo transformace dat.

### <a name="when-does-an-activity-run"></a>Spuštění aktivity
Hello **dostupnosti** nastavení konfigurace v hello výstupní datová tabulka určuje spuštění aktivity hello. Pokud jsou zadané vstupní datové sady, hello aktivita zkontroluje, zda jsou splněny všechny závislosti vstupní data hello (tedy **připraven** stavu) předtím, než je spuštěn.

## <a name="copy-activity---faq"></a>Kopie aktivity – nejčastější dotazy
### <a name="is-it-better-toohave-a-pipeline-with-multiple-activities-or-a-separate-pipeline-for-each-activity"></a>Je lepší toohave kanál s více aktivit nebo samostatné kanálu pro každou aktivitu?
Kanály mají toobundle aktivit souvisejících s. Pokud hello datové sady, které je propojují nejsou spotřebovávají každá další aktivita mimo hello kanálu, můžete ponechat hello aktivity v jeden kanál. Tímto způsobem aktivní období kanálu toochain nebude potřeba, tak, aby se navzájem zarovnat. Navíc je lépe zajištěná integrita dat hello v kanálu interní toohello tabulky hello při aktualizaci kanálu hello. Kanálu aktualizace v podstatě zastaví všechny hello aktivity v rámci kanálu hello, je odstraní a vytvoří je znovu. Z vytváření perspektivy, je také možné snadněji toosee hello toku dat v rámci hello související aktivity ve formátu JSON jeden soubor pro kanál hello.

### <a name="what-are-hello-supported-data-stores"></a>Jaké jsou hello podporovanými úložišti dat?
Aktivita kopírování v datové továrně zkopíruje data z úložiště zdroje dat úložiště tooa podřízený data. Objekt pro vytváření dat podporuje hello následující datová úložiště. Data z jakéhokoli zdroje může být napsán tooany jímky. Klikněte na tlačítko data store toolearn jak toocopy tooand data z tohoto úložiště.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Úložiště dat, s * může být místní nebo v Azure IaaS a vyžadují tooinstall [Brána pro správu dat](data-factory-data-management-gateway.md) na počítači v místní nebo Azure IaaS.

### <a name="what-are-hello-supported-file-formats"></a>Jaké jsou hello podporované formáty souborů?
[!INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]

### <a name="where-is-hello-copy-operation-performed"></a>Kde se provádí operace kopírování hello?
V tématu [přesun dat globálně dostupnou](data-factory-data-movement-activities.md#global) podrobnosti. Stručně řečeno když je potřebný místnímu úložišti dat, operace kopírování hello provádí hello Brána pro správu dat ve vašem místním prostředí. A pokud hello přesun dat mezi dvěma cloudové úložiště, operace kopírování hello se provádí v hello oblast nejbližší toohello umístění jímka v hello stejné geography.

## <a name="hdinsight-activity---faq"></a>Aktivita HDInsight – nejčastější dotazy
### <a name="what-regions-are-supported-by-hdinsight"></a>Které oblasti jsou podporovány v prostředí HDInsight?
Najdete v části geografickou dostupností v hello následujícího článku hello: nebo [podrobnosti o cenách prostředí HDInsight][hdinsight-supported-regions].

### <a name="what-region-is-used-by-an-on-demand-hdinsight-cluster"></a>Jaké oblasti je používán clusteru HDInsight na vyžádání?
Hello clusteru HDInsight na vyžádání je vytvořen v hello stejné oblasti, kde existuje hello úložiště, které jste zadali toobe používat s clusterem hello.    

### <a name="how-tooassociate-additional-storage-accounts-tooyour-hdinsight-cluster"></a>Jak účtů tooassociate další úložiště clusteru HDInsight tooyour?
Pokud používáte vlastní HDInsight Cluster (BYOC – přineste si vlastní Cluster), přečtěte si téma hello následující témata:

* [Použití clusteru služby HDInsight s účty alternativní úložiště a Metaúložišti][hdinsight-alternate-storage]
* [Použití dalších účtů úložiště s HDInsight Hive][hdinsight-alternate-storage-2]

Pokud používáte cluster služby na vyžádání, který byl vytvořený hello služba Data Factory, zadejte další účty úložiště pro hello HDInsight propojená služba tak, aby služba Data Factory hello můžete zaregistrovat vaším jménem. V hello definici JSON pro propojenou službu hello na vyžádání, použijte **additionalLinkedServiceNames** účtů vlastnost toospecify alternativní úložiště, jak je znázorněno v následujícím fragmentu kódu JSON hello:

```JSON
{
    "name": "MyHDInsightOnDemandLinkedService",
    "properties":
    {
        "type": "HDInsightOnDemandLinkedService",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "LinkedService-SampleData",
            "additionalLinkedServiceNames": [ "otherLinkedServiceName1", "otherLinkedServiceName2" ]
        }
    }
}
```
V předchozím příkladu hello otherLinkedServiceName1 a otherLinkedServiceName2 představují propojené služby, jejichž definice obsahovat přihlašovací údaje, které hello účty alternativní úložiště tooaccess potřebám clusteru HDInsight.

## <a name="slices---faq"></a>Řezy – nejčastější dotazy
### <a name="why-are-my-input-slices-not-in-ready-state"></a>Proč je můj vstupní řezy není ve stavu Připraveno?
Obvyklou chybou není nastavení **externí** vlastnost příliš**true** na hello vstupní datové sady při hello vstupní data je externí toohello objekt pro vytváření dat (není vyprodukované hello data factory).

V následujícím příkladu hello, potřebujete jenom tooset **externí** tootrue na **dataset1**.  

**DataFactory1** kanálu 1: dataset1 -> aktivity "activity1" -> dataset2 -> "activity2" -> dataset3 kanálu 2: dataset3 -> aktivita "activity3" -> dataset4

Pokud máte jiný objekt pro vytváření dat s kanál, který přebírá dataset4 (vyprodukované kanálem 2 v datové továrně 1), protože datovou sadu hello je produkovaný jiný objekt pro vytváření dat (DataFactory1, není DataFactory2) označte dataset4 jako externí datové sady.  

**DataFactory2**    
Kanál 1: dataset4 -> aktivita "activity4" -> dataset5

Pokud je vlastnost external hello správně nastavená, ověřte, zda vstupní data hello existuje v umístění hello zadaný v definici hello vstupní datové sady.

### <a name="how-toorun-a-slice-at-another-time-than-midnight-when-hello-slice-is-being-produced-daily"></a>Jak toorun řez v jiné době než půlnoc, kdy hello řez vytvářen denně?
Použití hello **posun** vytvořil vlastnost toospecify hello čas, kdy chcete toobe řez hello. V tématu [datovou sadu dostupnosti](data-factory-create-datasets.md#dataset-availability) část Podrobnosti o této vlastnosti. Tady je zběžný příklad:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
Denní řezy začínají **6: 00** místo hello výchozí půlnoci.     

### <a name="how-can-i-rerun-a-slice"></a>Jak můžete znovu spustit řez?
Můžete znovu spustit řez v jednom z následujících způsobů hello:

* Pomocí sledování a správě aplikací toorerun okno s aktivity nebo řez. V tématu [znovu spustit vybranou aktivity windows](data-factory-monitor-manage-app.md#perform-batch-actions) pokyny.   
* Klikněte na tlačítko **spustit** panelu příkazů hello hello **datový ŘEZ** okně hello řez ve hello portálu Azure.
* Spustit **Set-AzureRmDataFactorySliceStatus** rutiny se stavem nastavit příliš**čekání** pro řez hello.   

    ```PowerShell
    Set-AzureRmDataFactorySliceStatus -Status Waiting -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00"
    ```
V tématu [Set-AzureRmDataFactorySliceStatus] [ set-azure-datafactory-slice-status] podrobnosti o hello rutiny.

### <a name="how-long-did-it-take-tooprocess-a-slice"></a>Jak dlouho trvalo tooprocess řez?
Pomocí Průzkumníka okno aktivity v spravovat aplikace pro monitorování a tooknow, jak dlouho trvalo tooprocess datový řez. V tématu [aktivity okno Průzkumníka](data-factory-monitor-manage-app.md#activity-window-explorer) podrobnosti.

Můžete také provést hello následující v hello portálu Azure:  

1. Klikněte na tlačítko **datové sady** na hello dlaždici **DATA FACTORY** okno objektu pro vytváření dat.
2. Klikněte na konkrétní datové sady hello na hello **datové sady** okno.
3. Vyberte hello řez, který se zajímáte z hello **poslední řezy** seznamu na hello **tabulky** okno.
4. Klikněte na tlačítko hello aktivity při spuštění z hello **běh aktivit** seznamu na hello **datový ŘEZ** okno.
5. Klikněte na tlačítko **vlastnosti** na hello dlaždici **podrobnosti o spuštění aktivit** okno.
6. Měli byste vidět hello **doba trvání** pole s hodnotou. Tato hodnota je doba hello tooprocess hello řez.   

### <a name="how-toostop-a-running-slice"></a>Jak toostop spuštěné řez?
Pokud potřebujete toostop hello kanálu spuštění, můžete použít [Suspend-AzureRmDataFactoryPipeline](/powershell/module/azurerm.datafactories/suspend-azurermdatafactorypipeline) rutiny. V současné době pozastavení hello kanálu nezastaví spuštěních hello řez, které jsou v průběhu. Po spuštění v průběhu hello dokončit, je převzata žádné další řez.

Pokud skutečně chcete toostop všechny spuštěních hello okamžitě, hello pouze způsobem by být toodelete hello kanálu a vytvořit znovu. Pokud si zvolíte toodelete hello kanálu, není nutné toodelete tabulky a propojené služby, které kanálu hello používá.

[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: /dotnet/api/microsoft.azure.management.datafactories.models
[msdn-rest-api-reference]: /rest/api/datafactory/

[adf-powershell-reference]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/azurerm.datafactories
[azure-portal]: http://portal.azure.com
[set-azure-datafactory-slice-status]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/set-azurermdatafactoryslicestatus

[adf-pricing-details]: http://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: http://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: http://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
