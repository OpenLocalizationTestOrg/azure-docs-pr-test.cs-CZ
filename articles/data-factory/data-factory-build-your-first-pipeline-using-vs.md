---
title: "aaaBuild vaše první objekt pro vytváření dat (Visual Studio) | Microsoft Docs"
description: "V tomto kurzu vytvoříte pomocí sady Visual Studio ukázkový kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7398c0c9-7a03-4628-94b3-f2aaef4a72c5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 0c5eb01b685d978d80916da0293cc2d3701b2d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-by-using-visual-studio"></a>Kurz: Vytvoření datové továrny pomocí sady Visual Studio
> [!div class="op_single_selector" title="Tools/SDKs"]
> * [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Šablona Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Tento kurz ukazuje, jak toocreate objekt pro vytváření dat Azure pomocí sady Visual Studio. Vytvořit projekt sady Visual Studio pomocí šablony projektu pro vytváření dat hello, zadejte ve formátu JSON entit služby Data Factory (propojené služby, datové sady a kanál) a pak publikování/nasazení cloudu toohello tyto entity. 

Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**. Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data. Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas. 

> [!NOTE]
> Tento kurz neukazuje, jak kopírovat data pomocí Azure Data Factory. Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).


## <a name="walkthrough-create-and-publish-data-factory-entities"></a>Názorný postup: Vytvoření a publikování entit Data Factory
Zde jsou hello kroky, které můžete provádět v rámci tohoto návodu:

1. Vytvořte dvě propojené služby: **AzureStorageLinkedService1** a **HDInsightOnDemandLinkedService1**. 
   
    V tomto kurzu vstupní a výstupní data pro aktivitu hive hello jsou v hello stejné úložiště objektů Blob Azure. Použijete na vyžádání HDInsight clusteru tooprocess existující vstupní data tooproduce výstupní data. cluster HDInsight na vyžádání Hello se automaticky vytvoří za vás službou Azure Data Factory v době běhu, pokud vstupní data hello je připraven toobe zpracovat. Je nutné toolink ukládá data nebo vypočítá tooyour pro vytváření dat, aby se služba Data Factory hello připojovat toothem za běhu. Proto propojení účtu úložiště Azure služby data factory toohello pomocí hello AzureStorageLinkedService1 a propojení clusteru HDInsight na vyžádání pomocí hello HDInsightOnDemandLinkedService1. Při publikování, je třeba zadat název hello hello data factory toobe vytvořen nebo existující datovou továrnu.  
2. Vytvořte dvě datové sady: **InputDataset** a **OutputDataset**, které představují vstupní a výstupní data hello uložených v hello úložiště objektů blob Azure. 
   
    Tyto definice datové sady odkazují toohello propojenou službu úložiště Azure, kterou jste vytvořili v předchozím kroku hello. Pro hello InputDataset zadejte hello blob kontejneru (adfgetstarted) a hello složky (inptutdata), která obsahuje objekt blob se vstupní data hello. Pro hello OutputDataset zadejte hello blob kontejneru (adfgetstarted) a složku hello (partitioneddata), který obsahuje hello výstupní data. Zadáte také další vlastnosti, jako jsou například struktura, dostupnost a zásady.
3. Vytvořte kanál s názvem **MyFirstPipeline**. 
  
    V tomto návodu hello kanálu má jenom jedna aktivita: **aktivitu HDInsight Hive**. Tato aktivita transformace vstupní data tooproduce výstupní data pomocí skriptu hive v clusteru HDInsight na vyžádání. toolearn Další informace o hive aktivity, najdete v části [Hive aktivity](data-factory-hive-activity.md) 
4. Vytvořte datovou továrnu s názvem **DataFactoryUsingVS**. Nasaďte hello data factory a všechny entity služby Data Factory (propojené služby, tabulky a kanál hello).
5. Po publikování, pomocí oken webu Azure portal a sledování aplikace a správu toomonitor hello kanálu. 
  
### <a name="prerequisites"></a>Požadavky
1. Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky. Můžete také vybrat hello **přehled a požadavky** možnost v rozevíracím seznamu hello v článku toohello nejvyšší tooswitch hello. Po dokončení hello požadavky přepnout zpět toothis článku výběrem **Visual Studio** možnost v rozevíracím seznamu hello.
2. instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.  
3. Musíte mít nainstalované ve vašem počítači hello tyto položky:
   * Visual Studio 2013 nebo Visual Studio 2015.
   * Stáhněte si sadu Azure SDK pro Visual Studio 2013 nebo Visual Studio 2015. Přejděte příliš[stránku položek ke stažení Azure](https://azure.microsoft.com/downloads/) a klikněte na tlačítko **VS 2013** nebo **VS 2015** v hello **.NET** části.
   * Stáhnout hello nejnovější modul plug-in Azure Data Factory pro Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Můžete také aktualizovat modul plug-in hello provedením následujících kroků hello: hello nabídce klikněte na **nástroje** -> **rozšíření a aktualizace** -> **Online**  ->  **Galerie sady visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **aktualizace**.

Teď umožňuje pomocí sady Visual Studio toocreate služby Azure data factory.

### <a name="create-visual-studio-project"></a>Vytvoření projektu v sadě Visual Studio
1. Spusťte **Visual Studio 2013** nebo **Visual Studio 2015**. Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**. Měli byste vidět hello **nový projekt** dialogové okno.  
2. V hello **nový projekt** dialogové okno, vyberte hello **DataFactory** šablonu a klikněte na **prázdný projekt Data Factory**.   

    ![Dialogové okno Nový projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)
3. Zadejte **název** pro projekt hello **umístění**a název hello **řešení**a klikněte na tlačítko **OK**.

    ![Průzkumník řešení](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

### <a name="create-linked-services"></a>Vytvoření propojených služeb
V tomto kroku vytvoříte dvě propojené služby, **Azure Storage** a **HDInsight na vyžádání**. 

Hello Azure Storage propojená služba propojuje účet úložiště Azure toohello datovou továrnu tím, že poskytuje informace o připojení hello. Služba data Factory používá hello připojovacího řetězce z toohello tooconnect nastavení hello propojené služby úložiště Azure za běhu. Toto úložiště obsahuje vstupní a výstupní data pro kanál hello a hello hive soubor skriptu hive aktivitou hello používá. 

S na vyžádání propojené služby HDInsight se hello clusteru HDInsight se automaticky vytvoří za běhu při hello vstupních dat je připraven tooprocessed. Odstranění clusteru Hello až dokončí zpracování a nečinnosti pro hello zadanou dobu. 

> [!NOTE]
> Vytvoříte objekt pro vytváření dat zadáním jeho název a nastavení v době hello publikování řešení Data Factory.

#### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
1. Klikněte pravým tlačítkem na **propojené služby** v Průzkumníku řešení hello bod příliš**přidat**a klikněte na tlačítko **novou položku**.      
2. V hello **přidat novou položku** dialogové okno, vyberte **propojená služba úložiště Azure** hello seznamu a klikněte na **přidat**.
    ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)
3. Nahraďte `<accountname>` a `<accountkey>` s hello název účtu úložiště Azure a jeho klíčem. toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
    ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)
4. Uložit hello **AzureStorageLinkedService1.json** souboru.

#### <a name="create-azure-hdinsight-linked-service"></a>Vytvoření propojené služby Azure HDInsight
1. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **propojené služby**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.
2. Vyberte **HDInsight On Demand Linked Service** a klikněte na **Přidat**.
3. Nahraďte hello **JSON** s hello následující JSON:

     ```json
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
        "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService1"
            }
        }
    }
    ```

    Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

    Vlastnost | Popis
    -------- | ----------- 
    ClusterSize | Určuje velikost hello hello clusteru HDInsight Hadoop.
    TimeToLive | Nastavení této hello nečinnosti pro hello HDInsight cluster, než se odstraní.
    linkedServiceName | Určuje účet úložiště hello, který je použité toostore hello protokoly, které jsou generovány nástrojem clusteru HDInsight Hadoop. 

    > [!IMPORTANT]
    > Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (linkedServiceName). HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello. Toto chování je záměrné. Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když se zpracuje určitý řez, pokud neexistuje aktivní cluster (timeToLive). Hello clusteru je automaticky odstraněna po dokončení zpracování hello.
    > 
    > Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady. vzor podle Hello názvy těchto kontejnerů: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`. Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.

    Další informace o vlastnostech JSON najdete v tématu [Propojené služby Compute](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
4. Uložit hello **HDInsightOnDemandLinkedService1.json** souboru.

### <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoříte datové sady toorepresent hello vstupní a výstupní data pro zpracování Hive. Tyto datové sady odkazují toohello **AzureStorageLinkedService1** jste vytvořili dříve v tomto kurzu. Hello propojené služby body tooan účet služby Azure Storage a datové sady zadejte kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní a výstupní data.   

#### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
1. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **tabulky**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.
2. Vyberte **objektů Blob v Azure** hello seznamu, změňte název hello hello souboru příliš**InputDataSet.json**a klikněte na tlačítko **přidat**.
3. Nahraďte hello **JSON** v editoru hello se hello následujícím fragmentu kódu JSON:

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    Tento fragment kódu JSON Určuje datovou sadu s názvem **AzureBlobInput** , která představuje vstupní data aktivity hello hive v kanálu hello. Určíte, zda text hello vstupní data nachází v kontejneru objektů blob hello s názvem `adfgetstarted` a hello složku s názvem `inputdata`.

    Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

    Vlastnost | Popis |
    -------- | ----------- |
    type |Hello vlastnost Typ nastavena příliš**AzureBlob** protože data se nachází v Azure Blob Storage.
    linkedServiceName | Odkazuje toohello AzureStorageLinkedService1, které jste vytvořili dříve.
    fileName |Tato vlastnost je nepovinná. Pokud ji vynecháte, jsou vybrány všechny soubory hello z hello folderPath. V takovém případě pouze hello soubor Input.log.
    type | soubory protokolu Hello jsou v textovém formátu, takže používáme TextFormat. |
    columnDelimiter | sloupce v souborech protokolů hello oddělené čárkou hello (`,`)
    frequency/interval | frekvence nastavená tooMonth a interval je 1, což znamená, že hello vstupní řezy jsou k dispozici každý měsíc.
    external | Tato vlastnost nastavena tootrue, pokud hello vstupní data pro aktivitu hello nevygenerovala hello kanálu. Tato vlastnost se určuje jenom pro vstupní datové sady. Pro hello vstupní datové sady hello první aktivitu vždy nastavte tootrue.
4. Uložit hello **InputDataset.json** souboru.

#### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
Teď vytvořte hello výstupní datovou sadu toorepresent výstupní data ve hello úložiště objektů Blob v Azure.

1. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **tabulky**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.
2. Vyberte **objektů Blob v Azure** hello seznamu, změňte název hello hello souboru příliš**OutputDataset.json**a klikněte na tlačítko **přidat**.
3. Nahraďte hello **JSON** v editoru hello se hello následující JSON:
    
    ```json
    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }
    ```
    Hello fragmentu kódu JSON Určuje datovou sadu s názvem **AzureBlobOutput** , představuje výstupní data vytvořená hello hive aktivitu v kanálu hello. Určíte, že se v kontejneru objektů blob hello názvem umístí výstup hello data je vytvořená pomocí aktivity hive hello `adfgetstarted` a hello složku s názvem `partitioneddata`. 
    
    Hello **dostupnosti** část určuje, že hello výstupní sada vytváří jednou měsíčně. plán hello Hello výstupní datovou sadu jednotky hello kanálu. kanál Hello spustí každý měsíc mezi jeho počáteční a koncový čas. 

    V tématu **vytvoření vstupní datové sady hello** části popisy těchto vlastností. Sady nenajdete vlastnost external hello u výstupní datové jako hello datovou sadu vytváří hello kanálu.
4. Uložit hello **OutputDataset.json** souboru.

### <a name="create-pipeline"></a>Vytvoření kanálu
Vytvořili jste hello propojenou službu úložiště Azure a vstupní a výstupní datové sady, pokud. Teď vytvoříte kanál s aktivitou **HDInsightHive**. Hello **vstupní** pro hello hive aktivity je nastavený příliš**AzureBlobInput** a **výstup** je nastaven příliš**AzureBlobOutput**. Řezy vstupní datové sady je dostupný jednou měsíčně (frekvence: Month, interval: 1), a hello výstupní řez se vytváří jednou měsíčně příliš. 

1. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **kanály**, bod příliš**přidat**a klikněte na tlačítko **novou položku.**
2. Vyberte **kanál transformace Hive** hello seznamu a klikněte na **přidat**.
3. Nahraďte hello **JSON** s hello následující fragment kódu:

    > [!IMPORTANT]
    > Nahraďte `<storageaccountname>` hello název účtu úložiště.

    ```json
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService1",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    > [!IMPORTANT]
    > Nahraďte `<storageaccountname>` hello název účtu úložiště.

    fragmentu kódu JSON Hello definuje kanál, který se skládá z jedné aktivity (aktivita Hive). Tato aktivita běží vstupní data Hive skriptu tooprocess na na vyžádání HDInsight clusteru tooproduce výstupní data. V části aktivity hello hello kódu JSON kanálu, uvidíte jenom jedna aktivita v hello pole s typem nastavit příliš**HDInsightHive**. 

    Ve vlastnostech typu hello, které jsou specifické tooHDInsight Hive aktivity určete, jaké propojenou službu úložiště Azure má soubor skriptu hive hello, soubor skriptu toohello hello cesty a souboru skriptu toohello parametry. 

    soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného vlastností hello scriptLinkedService) a v hello `script` složky v kontejneru hello `adfgetstarted`.

    Hello `defines` část se nastavení používané toospecify hello běhového prostředí, které se předávají toohello skriptu hive jako konfigurační hodnoty Hive (např `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.

    Hello **spustit** a **end** vlastnosti kanálu hello určuje hello aktivní období kanálu hello. Jste nakonfigurovali toobe hello datovou sadu vytváří také jednou měsíčně, tedy pouze po řez je produkovaný hello kanálu (protože měsíc hello je stejný v počátečním a koncovým datem).

    V kódu JSON aktivity hello určujete, že hello skript Hive běžet ve výpočetní hello určeného hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.
4. Uložit hello **HiveActivity1.json** souboru.

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>Přidání souborů partitionweblogs.hql a input.log jako závislosti
1. Klikněte pravým tlačítkem na **závislosti** v hello **Průzkumníku řešení** okně bodu příliš**přidat**a klikněte na tlačítko **existující položka**.  
2. Přejděte toohello **C:\ADFGettingStarted** a vyberte **partitionweblogs.hql**, **input.log** soubory a klikněte na tlačítko **přidat**. Tyto dva soubory jste vytvořili v rámci požadavků z hello [přehled kurzu](data-factory-build-your-first-pipeline.md).

Když publikujete hello řešení v dalším kroku hello, hello **partitionweblogs.hql** soubor je nahrané toohello **skriptu** složky v hello `adfgetstarted` kontejner objektů blob.   

### <a name="publishdeploy-data-factory-entities"></a>Publikování/nasazení entit služby Data Factory
V tomto kroku publikujete hello entit služby Data Factory (propojené služby, datové sady a kanál) ve vašem projektu toohello služby Azure Data Factory. V hello proces publikování zadejte hello název objektu pro vytváření dat. 

1. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení hello a klikněte na tlačítko **publikovat**.
2. Pokud se zobrazí **přihlášení tooyour účtu Microsoft** dialogové okno, zadejte svoje přihlašovací údaje pro hello účet, který má předplatné Azure a klikněte na tlačítko **přihlášení**.
3. Měli byste vidět hello následující dialogové okno:

   ![Dialogové okno publikování](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
4. V hello **objekt pro vytváření dat konfigurace** stránky, hello následující kroky:

    ![Publikování – nová nastavení datové továrny](media/data-factory-build-your-first-pipeline-using-vs/publish-new-data-factory.png)

   1. Vyberte možnost **Create New Data Factory** (Vytvořit nový objekt pro vytváření dat).
   2. Zadejte jedinečný **název** hello data Factory. Příklad: **DataFactoryUsingVS09152016**. Název Hello musí být globálně jedinečný.
   3. Vyberte správné předplatné hello hello **předplatné** pole. 
        > [!IMPORTANT]
        > Pokud se nezobrazí žádné předplatné. Ujistěte se, že jste se přihlásili pomocí účtu, který je správce nebo spolusprávce předplatného hello.
   4. Vyberte hello **skupiny prostředků** pro hello data factory toobe vytvořili.
   5. Vyberte hello **oblast** hello data Factory.
   6. Klikněte na tlačítko **Další** tooswitch toohello **publikování položky** stránky. (Stisknutím klávesy **KARTĚ** toomove mimo hello název pole tooif hello **Další** tlačítko je zakázané.)

    > [!IMPORTANT]
    > Pokud se zobrazí chyba hello **název objektu pro vytváření dat "DataFactoryUsingVS" není k dispozici** při publikování, změňte název hello (například yournameDataFactoryUsingVS). V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.   
1. V hello **publikování položky** stránky, ujistěte se, že všechny hello datové továrny entity jsou vybrané a klikněte na tlačítko **Další** tooswitch toohello **Souhrn** stránky.

    ![Stránka publikování položek](media/data-factory-build-your-first-pipeline-using-vs/publish-items-page.png)     
2. Zkontrolujte hello shrnutí a klikněte na **Další** toostart hello nasazení proces a zobrazení hello **stav nasazení**.

    ![Stránka souhrnu](media/data-factory-build-your-first-pipeline-using-vs/summary-page.png)
3. V hello **stav nasazení** stránky, měli byste vidět hello stav procesu nasazení hello. Po dokončení hello nasazení, klikněte na tlačítko Dokončit.

Toonote důležité body:

- Pokud se zobrazí chyba hello: **toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory**, proveďte jednu z následujících hello a zkuste to znovu publikovat:
    - V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello.
        ```PowerShell   
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
        ```
        Můžete spustit následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory.

        ```PowerShell
        Get-AzureRmResourceProvider
        ```
    - Přihlášení pomocí hello předplatného Azure v toohello [portál Azure](https://portal.azure.com) a přejděte tooa okno objekt pro vytváření dat (nebo) vytvořte objekt pro vytváření dat v hello portálu Azure. Tato akce automaticky registruje zprostředkovatele hello za vás.
- Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.
- instance služby Data Factory toocreate, potřebujete toobe správce nebo spolusprávce hello předplatného Azure

### <a name="monitor-pipeline"></a>Monitorování kanálu
V tomto kroku můžete monitorovat pomocí zobrazení diagramu objektu pro vytváření dat hello kanál hello. 

#### <a name="monitor-pipeline-using-diagram-view"></a>Monitorování kanálu pomocí Zobrazení diagramu
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/), hello následující kroky:
   1. Klikněte na **Další služby** a poté na **Objekty pro vytváření dat**.
       
        ![Procházet -> Datové továrny](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png)
   2. Vyberte hello název objektu pro vytváření dat (například: **DataFactoryUsingVS09152016**) ze seznamu hello datové továrny.
   
       ![Výběr objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
2. Hello domovské stránce objektu pro vytváření dat, klikněte na tlačítko **Diagram**.

    ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
3. V zobrazení diagramu hello zobrazí se přehled hello kanálů a datové sady použité v tomto kurzu.

    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png)
4. tooview všechny aktivity v kanálu hello, klikněte pravým tlačítkem na kanál v hello diagram a klikněte na Otevřít kanál.

    ![Nabídka Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
5. Zkontrolujte, jestli aktivita HDInsightHive hello v kanálu hello.

    ![Zobrazení Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    toonavigate zpět toohello předchozího zobrazení, klikněte na tlačítko **objekt pro vytváření dat** v nabídce hello s popisem cesty v horní části hello.
6. V hello **zobrazení diagramu**, dvakrát klikněte na datovou sadu hello **AzureBlobInput**. Potvrďte, že hello řez je v **připraven** stavu. Může trvat několik minut, než se řez tooshow hello ve stavu Připraveno. Pokud není k tomu dojít po určité době čekat na, zjistěte, zda obsahuje hello vstupní soubor (input.log umístěny ve správném kontejneru hello) (`adfgetstarted`) a složku (`inputdata`). A ujistěte se, že hello **externí** vlastnost v hello vstupní datové sady je nastavená příliš**true**. 

   ![Vstupní řez ve stavu Připraveno](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
7. Klikněte na tlačítko **X** tooclose **AzureBlobInput** okno.
8. V hello **zobrazení diagramu**, dvakrát klikněte na datovou sadu hello **AzureBlobOutput**. Uvidíte, že hello řez, který se právě zpracovává.

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. Po dokončení zpracování uvidíte hello řez ve **připraven** stavu.

   > [!IMPORTANT]
   > Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut). Proto očekávat hello kanálu tootake **přibližně 30 minut** tooprocess hello řez.  
   
    ![Datová sada](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png)    
10. Pokud je řez hello v **připraven** stavu, zkontrolujte hello `partitioneddata` složky v hello `adfgetstarted` kontejneru ve službě blob storage hello výstupní data.  

    ![Výstupní data](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Klikněte na tlačítko hello řez toosee podrobnosti o jeho **datový řez** okno.

    ![Podrobnosti o datovém řezu](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Klikněte na aktivitu spustit v hello **aktivita spuštěna seznamu** toosee podrobnosti o aktivitě spustit (Hive aktivitu v tomto scénáři) **podrobnosti o spuštění aktivit** okno. 
  
    ![Podrobnosti o spuštění aktivit](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)    

    Ze souborů protokolů hello se zobrazí informace o stavu a hello dotaz Hive, která byla spuštěna. Tyto protokoly jsou užitečné při řešení potíží.  

V tématu [monitorování datových sad a kanálu](data-factory-monitor-manage-pipelines.md) pokyny, jak toouse hello Azure portálu toomonitor hello kanálu a datových sad jste vytvořili v tomto kurzu.

#### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitorování kanálu pomocí aplikace pro monitorování a správu
Také můžete použít monitorování a Správa aplikací toomonitor vašeho kanálů. Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).

1. Klikněte na dlaždici Monitorování a správa.

    ![Dlaždice Monitorování a správa](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png)
2. Měla by se zobrazit aplikace pro monitorování a správu. Změna hello **počáteční čas** a **čas ukončení** toomatch spuštění (04-01-2016 12:00:00) a ukončení (04-02-2016 12:00:00) kanálu a klikněte na tlačítko **použít**.

    ![Aplikace pro monitorování a správu](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png)
3. toosee podrobnosti o okno s aktivity, vyberte ho v hello **seznamu aktivity Windows** toosee podrobnosti o něm.
    ![Podrobnosti o okně aktivity](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)

> [!IMPORTANT]
> vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní. Proto, chcete-li řez hello toorerun nebo znovu hello kurzu, nahrajte hello vstupní soubor (input.log) toohello `inputdata` složky hello `adfgetstarted` kontejneru.

### <a name="additional-notes"></a>Další poznámky
- Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data. V tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pro všechny hello zdroje a jímky nepodporuje hello aktivity kopírování. V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) hello seznam výpočetní služby podporovaných službou Data Factory.
- Propojené služby propojují úložiště dat nebo výpočetní služby tooan pro vytváření dat Azure. V tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pro všechny hello zdroje a jímky nepodporuje hello aktivity kopírování. V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) hello seznam podporovaných službou Data Factory výpočetní služby a [aktivit transformace](data-factory-data-transformation-activities.md) , můžete spustit na ně.
- V tématu [přesun dat z / tooAzure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o vlastnostech JSON použitých ve hello propojená definice služby Azure Storage.
- Místo clusteru HDInsight na vyžádání můžete použít také vlastní cluster HDInsight. Podrobnosti najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).
-  Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello předcházející JSON. Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
- Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (linkedServiceName). HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello. Toto chování je záměrné. Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když se zpracuje určitý řez, pokud neexistuje aktivní cluster (timeToLive). Hello clusteru je automaticky odstraněna po dokončení zpracování hello.
    
    Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady. vzor podle Hello názvy těchto kontejnerů: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.
- Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup. Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello. 
- Tento kurz neukazuje, jak kopírovat data pomocí Azure Data Factory. Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="use-server-explorer-tooview-data-factories"></a>Pomocí Průzkumníka serveru tooview datové továrny
1. V **Visual Studio**, klikněte na tlačítko **zobrazení** hello nabídce a klikněte na tlačítko **Průzkumníka serveru**.
2. V okně hello Průzkumníka serveru rozbalte **Azure** a rozbalte **Data Factory**. Pokud se zobrazí **přihlášení tooVisual Studio**, zadejte hello **účet** přidružené k vašemu předplatnému Azure a klikněte na tlačítko **pokračovat**. Zadejte **heslo** a klikněte na **Přihlásit**. Visual Studio se pokusí tooget informace o všechny objekty pro vytváření dat Azure v rámci vašeho předplatného. Zobrazí hello stav této operace v hello **seznam úkolů objekt pro vytváření dat** okno.

    ![Průzkumník serveru](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. Klikněte pravým tlačítkem na objekt pro vytváření dat a vyberte **tooNew Export Data Factory Project** toocreate projekt sady Visual Studio podle existující datovou továrnu.

    ![Export objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Aktualizace nástrojů služby Data Factory pro Visual Studio
tooupdate Azure Data Factory tools pro Visual Studio, hello následující kroky:

1. Klikněte na tlačítko **nástroje** na hello nabídku a vyberte **rozšíření a aktualizace**.
2. Vyberte **aktualizace** v levém podokně hello a pak vyberte **Galerie sady Visual Studio**.
3. Vyberte možnost **Azure Data Factory tools for Visual Studio** (Nástroje služby Azure Data Factory pro Visual Studio) a klikněte na **Aktualizovat**. Pokud se tato položka nezobrazí, už máte nejnovější verzi nástroje hello hello.

## <a name="use-configuration-files"></a>Použití konfiguračních souborů
Konfigurační soubory v sadě Visual Studio tooconfigure vlastnosti můžete použít pro propojených služeb/tabulek/kanálů pro různá prostředí.

Vezměte v úvahu následující definici JSON pro služby Azure Storage, propojené hello. toospecify **connectionString** s různými hodnotami pro accountname a accountkey pro různá prostředí (Dev/testovací/produkční) toowhich hello nasazujete entity služby Data Factory. Provedete to tak, že pro každé prostředí použijete samostatný konfigurační soubor.

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="add-a-configuration-file"></a>Přidání konfiguračního souboru
Přidejte konfigurační soubor pro každé prostředí provedením následujících kroků hello:   

1. Klikněte pravým tlačítkem na projekt Data Factory hello v řešení sady Visual Studio, přejděte příliš**přidat**a klikněte na tlačítko **novou položku**.
2. Vyberte **konfigurace** hello seznamu nainstalovaných šablon vlevo hello vyberte **konfigurační soubor**, zadejte **název** pro konfiguraci hello souboru a klikněte na tlačítko **Přidat**.

    ![Přidání konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Přidejte konfigurační parametry a jejich hodnoty v hello následující formát:

    ```json
    {
        "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
        "AzureStorageLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        ],
        "AzureSqlLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        ]
    }
    ```

    Tento příklad umožňuje nakonfigurovat vlastnost connectionString propojené služby Azure Storage a propojené služby Azure SQL. Všimněte si, že je hello syntaxe pro určení názvu [JsonPath](http://goessner.net/articles/JsonPath/).   

    Pokud má kód JSON vlastnost, která má pole hodnot, jak je znázorněno v následujícím kódu hello:  

    ```json
    "structure": [
          {
              "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
        }
    ],
    ```

    Nakonfigurujte vlastnosti, jak je znázorněno v následující soubor konfigurace (použijte indexování od nuly) hello:

    ```json
    {
        "name": "$.properties.structure[0].name",
        "value": "FirstName"
    }
    {
        "name": "$.properties.structure[0].type",
        "value": "String"
    }
    {
        "name": "$.properties.structure[1].name",
        "value": "LastName"
    }
    {
        "name": "$.properties.structure[1].type",
        "value": "String"
    }
    ```

### <a name="property-names-with-spaces"></a>Názvy vlastností s mezerami
Pokud název vlastnosti obsahuje mezery, použijte hranaté závorky, jak ukazuje následující příklad (název databázového serveru) hello:

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a>Nasazení řešení pomocí konfigurace
Když v sadě VS publikujete entity služby Azure Data Factory, abyste mohli zadat konfiguraci hello chcete toouse pro danou operaci publikování.

toopublish entity v projektu Azure Data Factory pomocí konfiguračního souboru:   

1. Klikněte pravým tlačítkem na projekt Data Factory a klikněte na tlačítko **publikovat** toosee hello **publikování položky** dialogové okno.
2. Vyberte existující datovou továrnu nebo zadejte hodnoty pro vytvoření služby data factory na hello **objekt pro vytváření dat konfigurace** a klikněte na tlačítko **Další**.   
3. Na hello **publikování položky** stránky: zobrazí rozevírací seznam s dostupnými konfiguracemi pro hello **výběr konfigurace nasazení** pole.

    ![Výběr konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. Vyberte hello **konfigurační soubor** , že chcete vytvořit toouse a klikněte na tlačítko **Další**.
5. Zkontrolujte, jestli hello název souboru JSON v hello **Souhrn** a klikněte na tlačítko **Další**.
6. Klikněte na tlačítko **Dokončit** po dokončení operace nasazení hello.

Když nasadíte, hello hodnoty z konfiguračního souboru hello jsou použité tooset hodnoty vlastností v souborech JSON hello před hello entity jsou nasazené tooAzure služba Data Factory.   

## <a name="use-azure-key-vault"></a>Použití Azure Key Vault
Není vhodné a často proti zabezpečení zásady toocommit citlivých dat jako úložiště kódu toohello řetězce připojení. V tématu [zabezpečeného publikování ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) ukázku na Githubu toolearn o ukládání citlivých informací v Azure Key Vault a jeho použití při publikování entit služby Data Factory. Hello zabezpečeného publikování rozšíření pro Visual Studio umožňuje toobe hello tajné klíče uložené v Key Vault a pouze odkazy toothem jsou určené v propojené služby nebo konfigurace nasazení. Tyto odkazy se překládají při publikování tooAzure entity služby Data Factory. Tyto soubory pak lze potvrdit toosource úložiště bez vystavení všech tajných klíčů.

## <a name="summary"></a>Souhrn
V tomto kurzu jste vytvořili Azure data factory tooprocess data pomocí skriptu Hive v clusteru HDInsight hadoop. Jste použili hello editoru služby Data Factory v hello Azure portálu toodo hello následující kroky:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste dvě **propojené služby**:
   1. **Úložiště Azure** propojené služby toolink služby Azure blob storage, který obsahuje vstupní/výstupní soubory toohello data factory.
   2. **Azure HDInsight** na vyžádání propojené služby toolink služby na vyžádání HDInsight Hadoop clusteru toohello data factory. Azure Data Factory vytvoří HDInsight Hadoop clusteru za běhu tooprocess vstupní data a výstupní data produktu.
3. Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu hello.
4. Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.  

## <a name="next-steps"></a>Další kroky
V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru HDInsight na vyžádání spouští skript Hive. jak zjistit, toouse toocopy aktivity kopírování dat z tooAzure objektů Blob v Azure SQL, toosee [kurz: kopírování dat ze objektů blob v Azure tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Dvě aktivity (spustit aktivitu po jiné) můžete zřetězené nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md). 


## <a name="see-also"></a>Viz také
| Téma | Popis |
|:--- |:--- |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct řízené daty pracovních pro vaší situaci nebo podniku. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Aktivity transformace dat](data-factory-data-transformation-activities.md) |Tento článek obsahuje seznam aktivit transformace dat (třeba transformaci HDInsight Hive, kterou jste použili v tomto kurzu) podporovaných službou Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) |Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací. |
