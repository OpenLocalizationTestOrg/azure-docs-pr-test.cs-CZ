---
title: "aaaBuild vaše první objekt pro vytváření dat (PowerShell) | Microsoft Docs"
description: "V tomto kurzu vytvoříte pomocí prostředí Azure PowerShell ukázkový kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 22ec1236-ea86-4eb7-b903-0e79a58b90c7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 626260798b56d590577b3c4b24f7cf52873c9f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-powershell"></a>Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí prostředí Azure PowerShell
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Šablona Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>

V tomto článku používáte prostředí Azure PowerShell toocreate první objekt pro vytváření dat Azure. kurz hello toodo pomocí jiných nástrojů nebo sady SDK, vyberte jednu z možností hello hello rozevíracího seznamu.

Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**. Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data. Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas. 

> [!NOTE]
> Hello datovém kanálu v tomto kurzu transformuje vstupní data tooproduce výstupní data. Nekopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

## <a name="prerequisites"></a>Požadavky
* Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky.
* Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku tooinstall nejnovější verzi prostředí Azure PowerShell ve vašem počítači.
* (volitelné) Tento článek nepopisuje všechny rutiny služby Data Factory hello. Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Referenční informace o rutinách služby Data Factory](/powershell/module/azurerm.datafactories).

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
V tomto kroku budete pomocí prostředí Azure PowerShell toocreate Azure Data Factory s názvem **FirstDataFactoryPSH**. Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data. Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.

1. Otevřete prostředí Azure PowerShell a spusťte následující příkaz hello. Nechte prostředí Azure PowerShell otevřené až hello konce tohoto kurzu. Pokud zavřete a znovu, musíte toorun tyto příkazy znovu.
   * Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.
    ```PowerShell
    Login-AzureRmAccount
    ```    
   * Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet.
    ```PowerShell
    Get-AzureRmSubscription 
    ```
   * Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello. Toto předplatné by měl být hello stejné jako hello jeden, které jste použili v hello portálu Azure.
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```     
2. Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** spuštěním hello následující příkaz:
    
    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    Některé hello kroky v tomto kurzu vychází z předpokladu, že používáte hello skupinu prostředků s názvem ADFTutorialResourceGroup. Pokud používáte jiné skupině prostředků, je nutné toouse ho místo skupiny ADFTutorialResourceGroup v tomto kurzu.
3. Spustit hello **New-AzureRmDataFactory** rutinu, která vytvoří objekt pro vytváření dat s názvem **FirstDataFactoryPSH**.

    ```PowerShell
    New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"
    ```
Všimněte si hello následující body:

* Název Hello hello objekt pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí chyba hello **název objektu pro vytváření dat "FirstDataFactoryPSH" není k dispozici**, změňte název hello (například na název Váš_název_firstdatafactorypsh). Při provádění postupů v tomto kurzu potom tento název používejte místo názvu ADFTutorialFactoryPSH. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
* instance služby Data Factory toocreate, je nutné toobe přispěvatelem/správcem předplatného Azure hello
* Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.
* Pokud se zobrazí chyba hello: "**toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory**", proveďte jednu z následujících hello a zkuste to znovu publikovat:

  * V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello:

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
      Můžete spustit následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory:

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Přihlášení pomocí hello předplatného Azure hello [portál Azure](https://portal.azure.com) a přejděte tooa okno objekt pro vytváření dat (nebo) vytvořte objekt pro vytváření dat v hello portálu Azure. Tato akce automaticky registruje zprostředkovatele hello za vás.

Před vytvořením kanálu, je nutné toocreate několik entit služby Data Factory nejdřív. Nejdřív vytvoříte propojené služby toolink dat úložiště nebo výpočtů tooyour data ukládat, definujete vstupní a výstupní datové sady toorepresent vstupní a výstupní data v propojených úložištích dat a poté vytvořit hello kanálu s aktivitou, která používá tyto datové sady.

## <a name="create-linked-services"></a>Vytvoření propojených služeb
V tomto kroku propojíte účtu úložiště Azure a služby na vyžádání Azure HDInsight clusteru tooyour data factory. blokování účtu Azure Storage Hello hello vstupní a výstupní data pro kanál hello v této ukázce. Hello propojené služby HDInsight je použité toorun skriptu Hive určeného v hello aktivitu hello kanál v této ukázce. Určit data, která úložiště/výpočetní služby se používají ve vašem scénáři a vytvořením propojených služeb spojit tyto služby toohello objekt pro vytváření dat.

### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure. Hello použijete stejný účet úložiště Azure toostore vstupní a výstupní data a hello HQL soubor skriptu.

1. Vytvořte soubor JSON s názvem StorageLinkedService.json ve složce C:\ADFGetStarted hello s hello následující obsah. Pokud ještě neexistuje, vytvořte hello složky ADFGetStarted.

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
    Nahraďte **název účtu** hello název účtu úložiště Azure a **klíč účtu** s přístupovým klíčem hello hello účtu úložiště Azure. toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
2. V prostředí Azure PowerShell přepínače toohello složky ADFGetStarted.
3. Můžete použít hello **New-AzureRmDataFactoryLinkedService** rutinu, která vytvoří propojené služby. Tato rutina a jiné rutiny služby Data Factory používané v tomto kurzu vyžaduje, aby vám toopass hodnoty pro hello *ResourceGroupName* a *DataFactoryName* parametry. Alternativně můžete použít **Get-AzureRmDataFactory** tooget **DataFactory** objektu a hello objekt předat, aniž by museli zadávat *ResourceGroupName* a * DataFactoryName* pokaždé, když spustíte rutinu. Hello spusťte následující příkaz tooassign hello výstup hello **Get-AzureRmDataFactory** rutiny tooa **$df** proměnné.

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
4. Nyní, spustit hello **New-AzureRmDataFactoryLinkedService** rutinu, která vytvoří hello propojené **StorageLinkedService** služby.

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json
    ```
    Pokud jste nespustili hello **Get-AzureRmDataFactory** rutiny a přiřazené hello výstupní toohello **$df** proměnné, byste měli toospecify hodnoty pro hello *ResourceGroupName*a *DataFactoryName* parametry následujícím způsobem.

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json
    ```
    Pokud uprostřed hello hello kurzu zavřete prostředí Azure PowerShell, máte toorun hello **Get-AzureRmDataFactory** rutiny při příštím spuštění prostředí Azure PowerShell toocomplete hello kurzu.

### <a name="create-azure-hdinsight-linked-service"></a>Vytvoření propojené služby Azure HDInsight
V tomto kroku propojíte služby na vyžádání HDInsight clusteru tooyour data factory. Hello HDInsight cluster je automaticky vytvoří za běhu a až dokončí zpracování a nečinnosti hello zadaného množství času se odstraní. Místo clusteru HDInsight na vyžádání můžete použít také vlastní cluster HDInsight. Podrobnosti najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).

1. Vytvořte soubor JSON s názvem **HDInsightOnDemandLinkedService**.json v hello **C:\ADFGetStarted** složku s hello následující obsah.

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
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }
    ```
    Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

   | Vlastnost | Popis |
   |:--- |:--- |
   | ClusterSize |Určuje velikost hello hello clusteru HDInsight. |
   | TimeToLive |Nastavení této hello nečinnosti pro hello HDInsight cluster, než se odstraní. |
   | linkedServiceName |Určuje účet úložiště hello, který je použité toostore hello protokoly, které jsou generovány nástrojem HDInsight |

    Všimněte si hello následující body:

   * Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello JSON. Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
   * Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**. Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
   * Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**). HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello. Toto chování je záměrné. Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je zpracován určitý řez, pokud neexistuje aktivní cluster (**timeToLive**). Hello clusteru je automaticky odstraněna po dokončení zpracování hello.

       Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady. vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času". Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.

     Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
2. Spustit hello **New-AzureRmDataFactoryLinkedService** rutinu, která vytvoří hello propojená služba s názvem HDInsightOnDemandLinkedService.
    
    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json
    ```

## <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoříte datové sady toorepresent hello vstupní a výstupní data pro zpracování Hive. Tyto datové sady odkazují toohello **StorageLinkedService** jste vytvořili dříve v tomto kurzu. Hello propojené služby body tooan účet služby Azure Storage a datové sady zadejte kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní a výstupní data.

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
1. Vytvořte soubor JSON s názvem **C:\adfgetstarted** v hello **C:\ADFGetStarted** složku s hello následující obsah:

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
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
    Hello JSON Určuje datovou sadu s názvem **AzureBlobInput**, která představuje vstupní data pro aktivitu v kanálu hello. Kromě toho určuje, zda text hello vstupní data nachází v kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **inputdata**.

    Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

   | Vlastnost | Popis |
   |:--- |:--- |
   | type |Hello vlastnost Typ nastavena tooAzureBlob, protože data se nachází v úložišti objektů blob Azure. |
   | linkedServiceName |odkazuje toohello StorageLinkedService, které jste vytvořili dříve. |
   | fileName |Tato vlastnost je nepovinná. Pokud ji vynecháte, jsou vybrány všechny soubory hello z hello folderPath. V takovém případě pouze hello soubor Input.log. |
   | type |soubory protokolu Hello jsou v textovém formátu, takže používáme TextFormat. |
   | columnDelimiter |sloupce v souborech protokolů hello jsou odděleny hello čárku (,). |
   | frequency/interval |frekvence nastavená tooMonth a interval je 1, což znamená, že hello vstupní řezy jsou k dispozici každý měsíc. |
   | external |Tato vlastnost nastavena tootrue, pokud hello vstupní data nevygenerovala služba Data Factory hello. |
2. Spusťte následující příkaz v prostředí Azure PowerShell datovou sadu služby Data Factory hello toocreate hello:

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\InputTable.json
    ```

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
Teď vytvořte hello výstupní datovou sadu toorepresent hello výstupní data ve hello úložiště objektů Blob v Azure.

1. Vytvořte soubor JSON s názvem **C:\adfgetstarted** v hello **C:\ADFGetStarted** složku s hello následující obsah:

    ```json
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
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
    Hello JSON Určuje datovou sadu s názvem **AzureBlobOutput**, která představuje výstupní data pro aktivitu v kanálu hello. Kromě toho určuje, že hello výsledky ukládat do kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **partitioneddata**. Hello **dostupnosti** část určuje, že hello výstupní sada vytváří jednou měsíčně.
2. Spusťte následující příkaz v prostředí Azure PowerShell datovou sadu služby Data Factory hello toocreate hello:

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\OutputTable.json
    ```

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte svůj první kanál s aktivitou **HDInsightHive**. Vstupní řez je dostupný jednou měsíčně (frekvence: Month, interval: 1), výstupní řez se vytváří jednou měsíčně a vlastnost scheduler hello aktivity hello nastavena také toomonthly. nastavení Hello hello výstupní datovou sadu a hello aktivitu Plánovač se musí shodovat. Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup. Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello. na konci hello v této části jsou vysvětleny Hello vlastnosti používané ve hello následující JSON.

1. Vytvořte soubor JSON s názvem MyFirstPipelinePSH.json ve složce C:\ADFGetStarted hello s hello následující obsah:

   > [!IMPORTANT]
   > Nahraďte **storageaccountname** hello název účtu úložiště v hello JSON.
   >
   >

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
                        "scriptLinkedService": "StorageLinkedService",
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
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```
    V hello fragmentu kódu JSON vytváříte kanál sestávající z jediné aktivity, který používá Hive tooprocess Data v clusteru HDInsight.

    soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného hello scriptLinkedService, nazývá **StorageLinkedService**) a v **skriptu ** složky v kontejneru hello **adfgetstarted**.

    Hello **definuje** část nastavení použité toospecify hello běhového prostředí, které se předá skriptu hive toohello jako konfigurační hodnoty Hive (např. ${hiveconf: inputtable}, ${určuje}).

    Hello **spustit** a **end** vlastnosti kanálu hello určuje hello aktivní období kanálu hello.

    V kódu JSON aktivity hello určujete, že hello skript Hive běžet ve výpočetní hello určeného hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > V části "JSON kanálu" v [kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md) podrobné informace o vlastnostech JSON, které se používají v příkladu hello.

2. Zkontrolujte, jestli hello **input.log** souboru v hello **adfgetstarted/inputdata** složky v hello úložiště objektů blob v Azure a spusťte hello následující příkaz toodeploy hello kanálu. Od hello **spustit** a **end** jsou nastavené v posledních hello a **isPaused** je sada toofalse, hello kanál (aktivita v kanálu hello) spustí hned po nasazení.

    ```PowerShell
    New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
    ```
3. Úspěšně jste vytvořili první kanál pomocí prostředí Azure PowerShell, blahopřejeme!

## <a name="monitor-pipeline"></a>Monitorování kanálu
V tomto kroku budete pomocí prostředí Azure PowerShell toomonitor co se děje v objektu pro vytváření dat Azure.

1. Spustit **Get-AzureRmDataFactory** a přiřaďte výstup tooa hello **$df** proměnné.

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
2. Spustit **Get-AzureRmDataFactorySlice** tooget podrobnosti o všech řezech tabulky hello **EmpSQLTable**, což je výstupní tabulkou kanálu hello hello.

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```
    Všimněte si, že hello StartDateTime zde určíte je stejná jako počáteční čas uvedený v kódu JSON kanálu hello hello. Tady je ukázkový výstup hello:

    ```PowerShell
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : FirstDataFactoryPSH
    DatasetName       : AzureBlobOutput
    Start             : 7/1/2017 12:00:00 AM
    End               : 7/2/2017 12:00:00 AM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. Spustit **Get-AzureRmDataFactoryRun** tooget hello podrobnosti aktivity spouští pro určitý řez.

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```

    Tady je ukázkový výstup hello: 

    ```PowerShell
    Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : FirstDataFactoryPSH
    DatasetName         : AzureBlobOutput
    ProcessingStartTime : 12/18/2015 4:50:33 AM
    ProcessingEndTime   : 12/31/9999 11:59:59 PM
    PercentComplete     : 0
    DataSliceStart      : 7/1/2017 12:00:00 AM
    DataSliceEnd        : 7/2/2017 12:00:00 AM
    Status              : AllocatingResources
    Timestamp           : 12/18/2015 4:50:33 AM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : RunSampleHiveActivity
    PipelineName        : MyFirstPipeline
    Type                : Script
    ```
    Můžete spouštět tuto rutinu dokud neuvidíte hello řez ve **připraven** stavu nebo **se nezdařilo** stavu. Když hello řez ve stavu Připraveno, zkontrolujte hello **partitioneddata** složky v hello **adfgetstarted** kontejneru ve službě blob storage hello výstupní data.  Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá.

    ![Výstupní data](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)

> [!IMPORTANT]
> Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut). Proto očekávat hello kanálu tootake **přibližně 30 minut** tooprocess hello řez.
>
> vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní. Chcete-li řez hello toorerun nebo znovu hello kurzu, tedy nahrajte hello vstupní soubor (input.log) toohello složky inputdata hello kontejneru adfgetstarted.
>
>

## <a name="summary"></a>Souhrn
V tomto kurzu jste vytvořili Azure data factory tooprocess data pomocí skriptu Hive v clusteru HDInsight hadoop. Jste použili hello editoru služby Data Factory v hello Azure portálu toodo hello následující kroky:

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste dvě **propojené služby**:
   1. **Úložiště Azure** propojené služby toolink služby Azure blob storage, který obsahuje vstupní/výstupní soubory toohello data factory.
   2. **Azure HDInsight** na vyžádání propojené služby toolink služby na vyžádání HDInsight Hadoop clusteru toohello data factory. Azure Data Factory vytvoří HDInsight Hadoop clusteru za běhu tooprocess vstupní data a výstupní data produktu.
3. Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu hello.
4. Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.

## <a name="next-steps"></a>Další kroky
V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru Azure HDInsight na vyžádání spouští skript Hive. jak zjistit, toouse toocopy aktivity kopírování dat z tooAzure objektů Blob v Azure SQL, toosee [kurz: kopírování dat ze objektů Blob v Azure tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Viz také
| Téma | Popis |
|:--- |:--- |
| [Referenční informace o rutinách služby Data Factory](/powershell/module/azurerm.datafactories) |Tady najdete rozsáhlou dokumentaci o rutinách služby Data Factory. |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct začátku do konce řízené daty pracovní postupy pro vaší situaci nebo podniku. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) |Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací. |
