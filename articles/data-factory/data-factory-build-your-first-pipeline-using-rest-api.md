---
title: "aaaBuild vaše první objekt pro vytváření dat (REST) | Microsoft Docs"
description: "V tomto kurzu vytvoříte pomocí rozhraní REST API služby Data Factory ukázkový kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7e0a2465-2d85-4143-a4bb-42e03c273097
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 5d8e39bd598cca35f91d501bad74e8a8436f8f89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí rozhraní REST API služby Data Factory
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Šablona Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>


V tomto článku můžete pomocí REST API služby Data Factory toocreate první objekt pro vytváření dat Azure. kurz hello toodo pomocí jiných nástrojů nebo sady SDK, vyberte jednu z možností hello hello rozevíracího seznamu.

Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**. Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data. Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas.

> [!NOTE]
> Tento článek nepopisuje všechny hello REST API. Úplnou dokumentaci o rozhraní REST API najdete v článku [Rozhraní REST API služby Data Factory – referenční informace](/rest/api/datafactory/).
> 
> Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).


## <a name="prerequisites"></a>Požadavky
* Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky.
* Nainstalujte na svůj počítač nástroj [Curl](https://curl.haxx.se/dlwiz/). Použijete nástroj CURL hello s toocreate příkazů REST služby data factory.
* Postupujte podle pokynů v [tomto článku](../azure-resource-manager/resource-group-create-service-principal-portal.md) a proveďte následující:
  1. V Azure Active Directory vytvořte webovou aplikaci s názvem **ADFGetStartedApp**.
  2. Získejte **ID klienta** a **tajný klíč**.
  3. Získejte **ID tenanta**.
  4. Přiřadit hello **ADFGetStartedApp** aplikace toohello **Data Factory Přispěvatel** role.
* Nainstalujte [Azure PowerShell](/powershell/azure/overview).
* Spusťte **prostředí PowerShell** a hello spusťte následující příkaz. Nechte prostředí Azure PowerShell otevřené až hello konce tohoto kurzu. Pokud zavřete a znovu otevřete, toorun hello příkazů je třeba znovu.
  1. Spustit **Login-AzureRmAccount** a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.
  2. Spustit **Get-AzureRmSubscription** tooview všechny hello předplatná pro tento účet.
  3. Spustit **Get-AzureRmSubscription - Název_předplatného NameOfAzureSubscription | Set-AzureRmContext** tooselect hello předplatné, které chcete toowork s. Nahraďte **NameOfAzureSubscription** s názvem hello předplatného Azure.
* Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** tak, že spustíte následující příkaz v hello prostředí PowerShell hello:

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

   Některé hello kroky v tomto kurzu vychází z předpokladu, že používáte hello skupinu prostředků s názvem ADFTutorialResourceGroup. Pokud používáte jiné skupině prostředků, musíte v tomto kurzu toouse hello název vaší skupiny prostředků místo skupiny ADFTutorialResourceGroup.

## <a name="create-json-definitions"></a>Vytvoření definic JSON
Vytvořte následující soubory JSON ve složce hello, kde se nachází curl.exe.

### <a name="datafactoryjson"></a>datafactory.json
> [!IMPORTANT]
> Název musí být globálně jedinečné, proto je vhodné toomake ADFCopyTutorialDF tooprefix či přípon je jedinečný název.
>
>

```JSON
{
    "name": "FirstDataFactoryREST",
    "location": "WestUS"
}
```

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.json
> [!IMPORTANT]
> Položky **accountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem. toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
>
>

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.json

```JSON
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

| Vlastnost | Popis |
|:--- |:--- |
| ClusterSize |Velikost clusteru HDInsight hello. |
| TimeToLive |Nastavení této hello nečinnosti pro hello HDInsight cluster, než se odstraní. |
| linkedServiceName |Určuje účet úložiště hello, který je použité toostore hello protokoly, které jsou generovány nástrojem HDInsight |

Všimněte si hello následující body:

* Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello výše JSON. Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
* Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**. Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
* Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**). HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello. Toto chování je záměrné. S na vyžádání propojené služby HDInsight, HDInsight cluster vytvoří pokaždé, když řez je zpracovat, pokud neexistuje aktivní cluster (**timeToLive**) a po dokončení zpracování hello, se odstraní.

    Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady. vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času". Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.

Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).

### <a name="inputdatasetjson"></a>inputdataset.json

```JSON
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
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
| columnDelimiter |sloupce v souborech protokolů hello oddělené čárkou () |
| frequency/interval |frekvence nastavená tooMonth a interval je 1, což znamená, že hello vstupní řezy jsou k dispozici každý měsíc. |
| external |Tato vlastnost nastavena tootrue, pokud hello vstupní data nevygenerovala služba Data Factory hello. |

### <a name="outputdatasetjson"></a>outputdataset.json

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
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

### <a name="pipelinejson"></a>pipeline.json
> [!IMPORTANT]
> Nahraďte **storageaccountname** názvem vašeho účtu služby Azure Storage.
>
>

```JSON
{
    "name": "MyFirstPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                "scriptLinkedService": "AzureStorageLinkedService",
                "defines": {
                    "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                    "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                }
            },
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
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
        }],
        "start": "2017-07-10T00:00:00Z",
        "end": "2017-07-11T00:00:00Z",
        "isPaused": false
    }
}
```

V hello fragmentu kódu JSON vytváříte kanál sestávající z jediné aktivity, která používá Hive tooprocess data v clusteru HDInsight.

soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného hello scriptLinkedService, nazývá **StorageLinkedService**) a v **skriptu**  složky v kontejneru hello **adfgetstarted**.

Hello **definuje** část určuje nastavení běhového prostředí, které se předávají toohello skriptu hive jako konfigurační hodnoty Hive (např. ${hiveconf: inputtable}, ${určuje}).

Hello **spustit** a **end** vlastnosti kanálu hello určuje hello aktivní období kanálu hello.

V kódu JSON aktivity hello určujete, že hello skript Hive běžet ve výpočetní hello určeného hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.

> [!NOTE]
> V části "JSON kanálu" v [kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md) podrobnosti o vlastnostech JSON použitých v předchozím příkladu hello.
>
>

## <a name="set-global-variables"></a>Nastavení globálních proměnných
V prostředí Azure PowerShell spusťte následující příkazy po nahrazení hello hodnoty vlastními hello:

> [!IMPORTANT]
> Postup získání ID klienta, tajného klíče klienta, ID tenanta a ID předplatného naleznete v sekci [Požadavky](#prerequisites).
>
>

```PowerShell
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
$adf = "FirstDataFactoryREST"
```


## <a name="authenticate-with-aad"></a>Ověření pomocí ADD

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken)
```


## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
V tomto kroku vytvoříte službu Azure Data Factory s názvem **FirstDataFactoryREST**. Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Například aktivitu kopírování toocopy dat z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive dat tootransform skriptu Hive. Spusťte následující příkazy toocreate hello objekt pro vytváření dat hello:

1. Přiřadit toovariable hello příkaz s názvem **cmd**.

    Potvrďte, že hello název objektu pro vytváření dat hello zde určíte název zadaný v hello hello (ADFCopyTutorialDF) odpovídá **datafactory.json**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
    ```
2. Spusťte příkaz hello pomocí **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zobrazení výsledků hello. Pokud objekt pro vytváření dat hello byl úspěšně vytvořen, zobrazí hello JSON pro vytváření dat hello v hello **výsledky**, jinak se zobrazí chybová zpráva.

    ```PowerShell
    Write-Host $results
    ```

Všimněte si hello následující body:

* Název Hello hello objekt pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí chyba hello ve výsledcích: **název objektu pro vytváření dat "FirstDataFactoryREST" není k dispozici**, hello následující kroky:
  1. Změna hello název (například yournameFirstDataFactoryREST) v hello **datafactory.json** souboru. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
  2. V první příkaz hello kde hello **$cmd** přiřazena hodnota proměnné, nahraďte FirstDataFactoryREST hello nový název a spusťte příkaz hello.
  3. Spusťte hello následující dva příkazy tooinvoke hello REST API toocreate hello data factory a tisk hello výsledky operace hello.
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

Před vytvořením kanálu, je nutné toocreate několik entit služby Data Factory nejdřív. Nejdřív vytvoříte propojené služby toolink dat úložiště nebo výpočtů tooyour data ukládat, definujete vstupní a výstupní datové sady toorepresent data v propojených úložištích dat.

## <a name="create-linked-services"></a>Vytvoření propojených služeb
V tomto kroku propojíte účtu úložiště Azure a služby na vyžádání Azure HDInsight clusteru tooyour data factory. blokování účtu Azure Storage Hello hello vstupní a výstupní data pro kanál hello v této ukázce. Hello propojené služby HDInsight je použité toorun skriptu Hive určeného v hello aktivitu hello kanál v této ukázce.

### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure. V tomto kurzu použijete hello stejný účet úložiště Azure toostore vstupní a výstupní data a hello HQL soubor skriptu.

1. Přiřadit toovariable hello příkaz s názvem **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. Spusťte příkaz hello pomocí **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zobrazení výsledků hello. Pokud hello propojené služby byla úspěšně vytvořena, najdete v části hello JSON hello propojené služby hello **výsledky**, jinak se zobrazí chybová zpráva.

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-azure-hdinsight-linked-service"></a>Vytvoření propojené služby Azure HDInsight
V tomto kroku propojíte služby na vyžádání HDInsight clusteru tooyour data factory. Hello HDInsight cluster je automaticky vytvoří za běhu a až dokončí zpracování a nečinnosti hello zadaného množství času se odstraní. Místo clusteru HDInsight na vyžádání můžete použít také vlastní cluster HDInsight. Podrobnosti najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).

1. Přiřadit toovariable hello příkaz s názvem **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
    ```
2. Spusťte příkaz hello pomocí **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zobrazení výsledků hello. Pokud hello propojené služby byla úspěšně vytvořena, najdete v části hello JSON hello propojené služby hello **výsledky**, jinak se zobrazí chybová zpráva.

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoříte datové sady toorepresent hello vstupní a výstupní data pro zpracování Hive. Tyto datové sady odkazují toohello **StorageLinkedService** jste vytvořili dříve v tomto kurzu. Hello propojené služby body tooan účet služby Azure Storage a datové sady zadejte kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní a výstupní data.

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
V tomto kroku vytvoříte hello vstupní datové sady toorepresent vstupní data uložená v úložišti objektů Blob v Azure hello.

1. Přiřadit toovariable hello příkaz s názvem **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. Spusťte příkaz hello pomocí **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zobrazení výsledků hello. Pokud hello datová sada úspěšně vytvořila, zobrazí hello JSON pro datovou sadu hello v hello **výsledky**, jinak se zobrazí chybová zpráva.

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
V tomto kroku vytvoříte hello výstupní datovou sadu toorepresent výstupní data ve hello úložiště objektů Blob v Azure.

1. Přiřadit toovariable hello příkaz s názvem **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
    ```
2. Spusťte příkaz hello pomocí **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zobrazení výsledků hello. Pokud hello datová sada úspěšně vytvořila, zobrazí hello JSON pro datovou sadu hello v hello **výsledky**, jinak se zobrazí chybová zpráva.

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte svůj první kanál s aktivitou **HDInsightHive**. Vstupní řez je dostupný jednou měsíčně (frekvence: Month, interval: 1), výstupní řez se vytváří jednou měsíčně a vlastnost scheduler hello aktivity hello nastavena také toomonthly. nastavení Hello hello výstupní datovou sadu a hello aktivitu Plánovač se musí shodovat. Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup. Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello.

Zkontrolujte, jestli hello **input.log** souboru v hello **adfgetstarted/inputdata** složky v hello úložiště objektů blob v Azure a spusťte hello následující příkaz toodeploy hello kanálu. Od hello **spustit** a **end** jsou nastavené v posledních hello a **isPaused** je sada toofalse, hello kanál (aktivita v kanálu hello) spustí hned po nasazení.

1. Přiřadit toovariable hello příkaz s názvem **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. Spusťte příkaz hello pomocí **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zobrazení výsledků hello. Pokud hello datová sada úspěšně vytvořila, zobrazí hello JSON pro datovou sadu hello v hello **výsledky**, jinak se zobrazí chybová zpráva.

    ```PowerShell
    Write-Host $results
    ```
4. Úspěšně jste vytvořili první kanál pomocí prostředí Azure PowerShell, blahopřejeme!

## <a name="monitor-pipeline"></a>Monitorování kanálu
V tomto kroku použijete řezy toomonitor REST API služby Data Factory vytvářen hello kanálu.

```PowerShell
$ds ="AzureBlobOutput"

$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

$results2 = Invoke-Command -scriptblock $cmd;

IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

> [!IMPORTANT]
> Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut). Proto očekávat hello kanálu tootake **přibližně 30 minut** tooprocess hello řez.
>
>

Spustit hello Invoke-Command a hello dalšímu, dokud neuvidíte hello řez ve **připraven** stavu nebo **se nezdařilo** stavu. Když hello řez ve stavu Připraveno, zkontrolujte hello **partitioneddata** složky v hello **adfgetstarted** kontejneru ve službě blob storage hello výstupní data.  Hello vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá.

![Výstupní data](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [!IMPORTANT]
> vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní. Chcete-li řez hello toorerun nebo znovu hello kurzu, tedy nahrajte hello vstupní soubor (input.log) toohello složky inputdata hello kontejneru adfgetstarted.
>
>

Můžete také použít Azure portálu toomonitor řezy a vyřešte všechny problémy. Přečtěte si podrobnosti o [monitorování kanálů pomocí webu Azure Portal](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline).

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
| [Rozhraní REST API služby Data Factory – referenční informace](/rest/api/datafactory/) |Tady najdete rozsáhlou dokumentaci o rutinách služby Data Factory. |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct začátku do konce řízené daty pracovní postupy pro vaší situaci nebo podniku. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) |Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací. |
