---
title: "aaaCreate clusterů systému Hadoop na vyžádání pomocí služby Data Factory - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate na vyžádání clusterů systému Hadoop v HDInsight pomocí Azure Data Factory."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: spelluru
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: spelluru
ms.openlocfilehash: c869776ac270e37dec710b5fc8d2a792d9263129
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Vytvářet na vyžádání clusterů systému Hadoop v HDInsight pomocí Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

[Azure Data Factory](../data-factory/data-factory-introduction.md) je služba integrace cloudových dat, která orchestruje a automatizuje hello přesouvání a transformaci dat. Může vytvořit tooprocess za běhu clusteru HDInsight Hadoop řez vstupní data a odstranění clusteru hello po dokončení zpracování hello. Některé z výhod hello pomocí cluster systému HDInsight Hadoop na vyžádání jsou:

- Můžete pouze platím pro hello čase úloha běží na clusteru HDInsight Hadoop (plus stručný konfigurovat doby nečinnosti) hello. Hello fakturace pro clustery služby HDInsight se fakturují za minutu, zda jsou jejich používání, nebo ne. Pokud používáte na vyžádání propojené služby HDInsight v objektu pro vytváření dat, hello clustery jsou vytvořeny na vyžádání. A po dokončení úlohy hello jsou automaticky odstraněny hello clustery. Proto platíte jenom pro úlohu hello systémem čas a hello krátké doby nečinnosti (nastavení time to live).
- Můžete vytvořit pracovní postup pomocí objektu pro vytváření dat kanál. Například můžete mít hello kanálu toocopy data z tooan systému SQL Server místní úložiště objektů blob v Azure, zpracování dat hello spuštěním skriptu Hive a Pig skript na cluster systému HDInsight Hadoop na vyžádání. Zkopírujte hello výsledek data tooan Azure SQL Data Warehouse pro tooconsume BI aplikace.
- Můžete naplánovat hello pracovního postupu toorun pravidelně (HODINOVĚ, denně, týdně, měsíčně, atd.).

V Azure Data Factory objekt pro vytváření dat může mít jeden nebo více datových kanálů. Datový kanál obsahuje jeden nebo více aktivit. Existují dva typy aktivit: [aktivity přesunu dat](../data-factory/data-factory-data-movement-activities.md) a [aktivit transformace dat](../data-factory/data-factory-data-transformation-activities.md). Používáte data přesun aktivity (v současné době pouze aktivita kopírování) toomove data ze zdroje dat úložiště tooa cílového úložiště dat. Data transformace aktivity tootransform nebo zpracování dat použijete. Aktivita HDInsight Hive je jedním z podporovaných službou Data Factory aktivit transformace hello. V tomto kurzu použijete aktivitu transformace Hive hello.

Aktivita toouse hive můžete nakonfigurovat vlastní cluster HDInsight Hadoop nebo cluster systému HDInsight Hadoop na vyžádání. V tomto kurzu hello Hive aktivitu v kanálu objekt pro vytváření dat hello je nakonfigurované toouse clusteru HDInsight na vyžádání. Proto při hello aktivita spuštěna tooprocess datový řez, stane se toto:

1. Pro můžete za běhu tooprocess hello řez se automaticky vytvoří cluster HDInsight Hadoop.  
2. spuštění skriptu HiveQL v clusteru hello zpracovává vstupní data Hello.
3. Po dokončení zpracování hello a hello clusteru hello nakonfigurované množství času (nastavení timeToLive) nečinný, se odstraní Hello clusteru HDInsight Hadoop. Pokud hello další datový řez je k dispozici pro zpracování s timeToLive dobu nečinnosti, hello stejného clusteru je použité tooprocess hello řez.  

V tomto kurzu provádí hello skript HiveQL přidružené k aktivitě hive hello hello následující akce:

1. Vytvoří externí tabulku, která odkazuje na hello nezpracovaná webového protokolu data uložená v Azure Blob storage.
2. Hello nezpracovaná data oddíly podle roku a měsíce.
3. Úložiště hello oddílů dat v hello úložiště objektů blob Azure.

V tomto kurzu hello skript HiveQL přidružené k aktivitě hive hello vytvoří externí tabulku, která odkazuje na hello nezpracovaná webového protokolu data uložená v hello Azure Blob Storage. Tady jsou hello ukázka řádků pro každý měsíc ve vstupním souboru hello.

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

oddíly skript HiveQL Hello hello nezpracovaná data podle roku a měsíce. Vytvoří tři výstupní složky podle předchozích vstup hello. Daná složka obsahuje soubor s položky z každý měsíc.

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

Seznam aktivit transformace dat služby Data Factory v aktivitě tooHive přidání najdete v tématu [transformovat a analyzovat pomocí Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).

> [!NOTE]
> V současné době můžete vytvořit pouze verze clusteru HDInsight 3.2 z Azure Data Factory.

## <a name="prerequisites"></a>Požadavky
Než začnete hello pokyny v tomto článku, musíte mít hello následující položky:

* [Předplatné Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure Powershell

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a>Příprava účtu úložiště
Můžete použít toothree účtů úložiště v tomto scénáři:

- Výchozí účet úložiště pro hello HDInsight cluster
- účet úložiště pro vstupní data hello
- účet úložiště hello výstupních dat

toosimplify hello kurzu použijete jeden úložiště účet tooserve hello tři účely. najít v této části Hello prostředí Azure PowerShell ukázkový skript provede hello následující úlohy:

1. Přihlaste se tooAzure.
2. Vytvořte skupinu prostředků Azure.
3. Vytvoření účtu úložiště Azure.
4. Vytvořte kontejner objektů Blob v účtu úložiště hello
5. Zkopírujte následující dva kontejner objektů Blob toohello soubory hello:

   * Vstupní datový soubor: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)
   * Skript HiveQL: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)

     Oba soubory jsou uloženy ve veřejném kontejneru Blob.


**tooprepare hello úložiště a kopírovat hello soubory pomocí Azure PowerShell:**
> [!IMPORTANT]
> Zadejte názvy pro skupinu prostředků Azure hello a hello účtu úložiště Azure, který bude vytvořen skriptem hello.
> Zapište **název skupiny prostředků**, **název účtu úložiště**, a **klíč účtu úložiště** výstupem hello skriptu. Je nutné v další části hello.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect tooAzure
####################################
#region - Connect tooAzure subscription
Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use hello following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

Pokud potřebujete pomoc s skript prostředí PowerShell text hello, přečtěte si téma [hello pomocí prostředí Azure PowerShell s Azure Storage](../storage/common/storage-powershell-guide-full.md). Pokud je jako toouse rozhraní příkazového řádku Azure místo toho zobrazí hello [příloha](#appendix) části hello skript příkazového řádku Azure CLI.

**obsah tooexamine hello úložiště účet a hello**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **skupiny prostředků** v levém podokně hello.
3. Dvakrát klikněte na název skupiny prostředků hello, kterou jste vytvořili ve vašem skriptu prostředí PowerShell. Pomocí filtru hello, pokud máte příliš mnoho skupin prostředků, které jsou uvedené.
4. Na hello **prostředky** dlaždice, musí mít jeden prostředek uvedené Pokud sdílíte s další projekty skupiny prostředků hello. Tento prostředek je hello účet úložiště s hello název, který jste zadali dříve. Klikněte na název účtu úložiště hello.
5. Klikněte na tlačítko hello **objekty BLOB** dlaždice.
6. Klikněte na tlačítko hello **adfgetstarted** kontejneru. Zobrazí dvě složky: **inputdata** a **skriptu**.
7. Otevřete složku hello a zkontrolujte hello soubory ve složkách hello. Hello inputdata soubor input.log hello se vstupní data a složky hello skript obsahuje soubor skriptu HiveQL hello.

## <a name="create-a-data-factory-using-resource-manager-template"></a>Vytvořte objekt pro vytváření dat pomocí šablony Resource Manageru
Účet úložiště hello, hello vstupní data a hello skript HiveQL připravený jsou připravené toocreate služby Azure data factory. Existuje několik metod pro vytvoření služby data factory. V tomto kurzu vytvoříte objekt pro vytváření dat nasazením šablonu Azure Resource Manager pomocí hello portálu Azure. Můžete také nasadit šablony Resource Manageru pomocí [rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy-cli.md) a [prostředí Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template). Ostatními metodami vytvoření objektu pro vytváření dat najdete v části [kurz: sestavení prvního objektu pro vytváření dat](../data-factory/data-factory-build-your-first-pipeline.md).

1. Klikněte na tlačítko hello toosign bitové kopie v tooAzure a otevřete hello šablony Resource Manageru v hello portálu Azure. Šablona Hello je umístěna ve https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json. V tématu hello [entit služby Data Factory v šabloně hello](#data-factory-entities-in-the-template) části Podrobné informace o entitách definovaná v šabloně hello. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Vyberte **použít existující** možnost pro hello **skupiny prostředků** nastavení a vyberte hello název skupiny prostředků hello jste vytvořili v předchozím kroku hello (pomocí skriptu prostředí PowerShell).
3. Zadejte název objektu pro vytváření dat hello (**název objektu pro vytváření dat**). Tento název musí být globálně jedinečný.
4. Zadejte hello **název účtu úložiště** a **klíč účtu úložiště** jste si poznamenali v předchozím kroku hello.
5. Vyberte **souhlasím toohello podmínky a ujednání** stanovené výše po přečtení **podmínky a ujednání**.
6. Vyberte **Pin toodashboard** možnost.
6. Klikněte na tlačítko **nákupu nebo vytvořit**. Zobrazí dlaždice na řídicí panel názvem hello **nasazení šablony**. Počkejte, dokud hello **skupiny prostředků** otevře se okno skupiny prostředků. Můžete také kliknout na hello dlaždice s názvem jako vaše skupina název tooopen hello prostředků okně skupiny prostředků.
6. Pokud okno skupiny prostředků hello není otevřený, klikněte na skupinu prostředků hello dlaždice tooopen hello. Nyní se zobrazí jeden prostředek další objekt pro vytváření dat uvedené dále toohello úložiště účet prostředků.
7. Klikněte na tlačítko hello název objektu pro vytváření dat (hodnota zadaná pro hello **název objektu pro vytváření dat** parametr).
8. V okně hello objekt pro vytváření dat, klikněte na tlačítko hello **Diagram** dlaždici. Hello diagram znázorňuje jednu aktivitu s vstupní datové sady a výstupní datové:

    ![Azure Data Factory HDInsight na vyžádání Hive aktivity kanálu diagram](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    názvy Hello jsou definovány v šabloně Resource Manager hello.
9. Klikněte dvakrát na **AzureBlobOutput**.
10. Na hello **poslední aktualizovat řezy**, zobrazí se jeden řez. Pokud je stav hello **v průběhu**, počkejte, dokud se mění příliš**připraven**. Obvykle trvá přibližně **20 minut** toocreate clusteru služby HDInsight.

### <a name="check-hello-data-factory-output"></a>Zkontrolujte výstup objektu pro vytváření dat hello

1. Použití hello stejný postup v hello poslední relace toocheck hello kontejnery hello kontejneru adfgetstarted. Existují dva nové kontejnery kromě příliš**adfgetsarted**:

   * Kontejner s názvem, který se následující hello: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`. Tento kontejner je hello výchozí kontejner pro hello HDInsight cluster.
   * adfjobs: Tento kontejner je hello kontejner pro ADF hello v protokolech úloh.

     Výstup objektu pro vytváření dat Hello je uložen v **afgetstarted** jak jste nakonfigurovali v hello šablony Resource Manageru.
2. Klikněte na tlačítko **adfgetstarted**.
3. Klikněte dvakrát na **partitioneddata**. Zobrazí **rok = 2014** složky protože ze všech webových protokolů hello v roce 2014.

    ![Azure Data Factory HDInsight na vyžádání Hive aktivity kanálu výstup](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    Pokud přejdete k podrobnostem hello seznamu, zobrazí se tří složek pro leden, února a března. A je do protokolu pro každý měsíc.

    ![Azure Data Factory HDInsight na vyžádání Hive aktivity kanálu výstup](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-hello-template"></a>Entity objektu pro vytváření dat v šabloně hello
Zde je, jak vypadá hello nejvyšší úrovně šablony Resource Manageru pro vytváření dat:

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```

### <a name="define-data-factory"></a>Definování datové továrny
Objekt pro vytváření dat v šabloně Resource Manager hello definovat, jak je uvedeno v hello následující ukázka:  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
Hello dataFactoryName je hello název objektu pro vytváření dat hello, které určíte při nasazování šablony hello. Objekt pro vytváření dat se aktuálně podporuje jenom v oblasti Východ USA, Severní Evropa a západní USA hello.

### <a name="defining-entities-within-hello-data-factory"></a>Definování entity v rámci objektu pro vytváření dat hello
Hello následující entity služby Data Factory jsou definovány v šabloně hello JSON:

* [Propojená služba Azure Storage](#azure-storage-linked-service)
* [Propojená služba HDInsightu na vyžádání](#hdinsight-on-demand-linked-service)
* [Vstupní datová sada Azure Blob](#azure-blob-input-dataset)
* [Výstupní datová sada Azure Blob](#azure-blob-output-dataset)
* [Data Pipeline s aktivitou kopírování](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage
Hello Azure Storage propojená služba propojuje účet úložiště Azure toohello datovou továrnu. V tomto kurzu hello stejný účet úložiště se používá jako hello výchozí účet úložiště HDInsight, úložiště vstupní data a výstupní datové úložiště. Proto můžete definovat jen jeden Azure Storage, propojené služby. V definici hello propojené služby je třeba zadat název hello a klíč účtu úložiště Azure. V tématu [propojená služba Azure Storage](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o toodefine vlastnosti používané JSON Azure Storage, propojené služby.

```json
{
    "name": "[variables('storageLinkedServiceName')]",
    "type": "linkedservices",
    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
Hello **connectionString** používá hello storageAccountName a storageAccountKey parametry. Zadejte hodnoty pro tyto parametry při nasazování šablony hello.  

#### <a name="hdinsight-on-demand-linked-service"></a>Propojená služba HDInsightu na vyžádání
V hello HDInsight na vyžádání propojené definice služby, zadejte hodnoty pro parametry konfigurace, které jsou používány toocreate služby Data Factory hello clusteru HDInsight Hadoop za běhu. V tématu [výpočetní propojené služby](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) článek podrobnosti o toodefine vlastnosti používané JSON propojené služby HDInsight na vyžádání.  

```json

{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "sshUserName": "myuser",                            
            "sshPassword": "MyPassword!",
            "linkedServiceName": "[variables('storageLinkedServiceName')]"
        }
    }
}
```
Všimněte si hello následující body:

* Hello Data Factory vytvoří **systémem Linux** HDInsight cluster.
* Hello clusteru HDInsight Hadoop je vytvořen v hello stejné oblasti jako účet úložiště hello.
* Všimněte si hello *timeToLive* nastavení. objekt pro vytváření dat Hello automaticky odstraní hello clusteru po hello clusteru se nečinnosti, po dobu 30 minut.
* Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**). HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello. Toto chování je záměrné. S na vyžádání propojené služby HDInsight, HDInsight cluster vytvoří pokaždé, když řez musí toobe zpracování, pokud neexistuje aktivní cluster (**timeToLive**) a po dokončení zpracování hello, se odstraní.

Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).

> [!IMPORTANT]
> Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady. vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času". Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.

#### <a name="azure-blob-input-dataset"></a>Vstupní datová sada Azure Blob
V definici hello vstupní datové sady zadejte názvy hello kontejner objektů blob, složku a soubor, který obsahuje vstupní data hello. V tématu [vlastnosti datové sady objektu Blob Azure](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob.

```json

{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
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

Všimněte si následujících konkrétní nastavení v definici JSON hello hello:

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a>Výstupní datová sada Azure Blob
V definici datové sady výstup hello zadejte názvy hello kontejner objektů blob a složky, která obsahuje hello výstupní data. V tématu [vlastnosti datové sady objektu Blob Azure](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) podrobnosti o JSON vlastnosti používané toodefine datové sadě služby Azure Blob.  

```json

{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1,
            "style": "EndOfInterval"
        }
    }
}
```

Hello folderPath určuje hello cesta toohello složky, která obsahuje výstupní data hello:

```json
"folderPath": "adfgetstarted/partitioneddata",
```

Hello [datovou sadu dostupnosti](../data-factory/data-factory-create-datasets.md#dataset-availability) nastavení vypadá takto:

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

V Azure Data Factory, výstupní datovou sadu dostupnosti jednotky hello kanálu. V tomto příkladu hello řez vytváří jednou měsíčně hello poslední den v měsíci (EndOfInterval). Další informace najdete v tématu [Data Factory plánování a provádění](../data-factory/data-factory-scheduling-and-execution.md).

#### <a name="data-pipeline"></a>Data Pipeline
Můžete definovat kanál, který transformuje data pomocí skriptu Hive v clusteru Azure HDInsight na vyžádání. V tématu [JSON kanálu](../data-factory/data-factory-create-pipelines.md#pipeline-json) popisy toodefine prvky používané JSON kanálu v tomto příkladu.

```json
{
    "type": "datapipelines",
    "name": "[parameters('dataFactoryName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobInputDatasetName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobOutputDatasetName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "description": "Azure Data Factory pipeline with an Hadoop Hive activity",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                    "defines": {
                        "inputtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                        "partitionedtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
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
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-01-01T00:00:00Z",
        "end": "2016-01-31T00:00:00Z",
        "isPaused": false
    }
}
```

Hello kanál obsahuje jednu aktivitu, aktivita HDInsightHive. Jako počáteční a koncová data jsou v leden 2016, data pro pouze jeden měsíc () zpracování řezu se. Obě *spustit* a *end* hello aktivity mají na datum v minulosti, takže hello objekt pro vytváření dat zpracovává data pro měsíc hello okamžitě. Pokud koncový hello je budoucí datum, hello data factory vytvoří jiné řez, pokud nastane čas hello. Další informace najdete v tématu [Data Factory plánování a provádění](../data-factory/data-factory-scheduling-and-execution.md).

## <a name="clean-up-hello-tutorial"></a>Vyčistěte kurz hello

### <a name="delete-hello-blob-containers-created-by-on-demand-hdinsight-cluster"></a>Odstranění kontejnerů objektů blob hello vytvořené clusteru HDInsight na vyžádání
S na vyžádání propojené služby HDInsight HDInsight cluster vytvoří pokaždé, když řez musí toobe zpracování, pokud neexistuje aktivní cluster (timeToLive); a hello clusteru se odstraní po dokončení zpracování hello. Pro každý cluster Azure Data Factory vytvoří kontejner objektů blob v hello úložiště objektů blob v Azure jako hello výchozí stroage účet pro hello clusteru. To i v případě odstranění clusteru HDInsight, nebudou odstraněny hello výchozího kontejneru blob storage a hello přidruženého účtu úložiště. Toto chování je záměrné. Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady. vzor podle Hello názvy těchto kontejnerů: `adfyourdatafactoryname-linkedservicename-datetimestamp`.

Odstranit hello **adfjobs** a **adfyourdatafactoryname-linkedservicename razítko_data_a_času** složky. Hello adfjobs kontejner obsahuje protokoly úlohy Azure Data Factory.

### <a name="delete-hello-resource-group"></a>Odstraňte skupinu prostředků hello
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) je použité toodeploy, správu a sledování řešení jako se skupinou.  Odstranění skupiny prostředků se odstraní všechny součásti hello uvnitř skupiny hello.  

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **skupiny prostředků** v levém podokně hello.
3. Klikněte na název skupiny prostředků hello, kterou jste vytvořili ve vašem skriptu prostředí PowerShell. Pomocí filtru hello, pokud máte příliš mnoho skupin prostředků, které jsou uvedené. Hello skupiny prostředků se otevře v novém okně.
4. Na hello **prostředky** dlaždice, musí mít hello výchozí účet úložiště a hello data factory uvedené Pokud sdílíte s další projekty skupiny prostředků hello.
5. Klikněte na tlačítko **odstranit** na hello horní části okna hello. Díky tomu odstraní účet úložiště hello a hello data uložená v účtu úložiště hello.
6. Zadejte odstranění tooconfirm hello prostředků skupiny název a pak klikněte na tlačítko **odstranit**.

V případě, že při odstranění skupiny prostředků hello nechcete, aby účet úložiště hello toodelete, vezměte v úvahu hello následující architektura oddělením hello podniková data z hello výchozí účet úložiště. V takovém případě máte jednu skupinu prostředků pro účet úložiště hello s hello obchodních dat a hello jiné skupině prostředků pro hello výchozí účet úložiště pro HDInsight propojené služby a hello služby data factory. Při odstranění skupiny prostředků druhý hello neovlivní účet úložiště dat hello firmy. toodo tak:

* Přidejte hello následující skupiny nejvyšší úrovně prostředků toohello společně s hello Microsoft.DataFactory/datafactories prostředků ve vaší šabloně Resource Manager. Vytvoří účet úložiště:

    ```json
    {
        "name": "[parameters('defaultStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": {

        },
        "properties": {
            "accountType": "Standard_LRS"
        }
    },
    ```
* Přidejte nové propojené služby bodu toohello nový účet úložiště:

    ```json
    {
        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
        "type": "linkedservices",
        "name": "[variables('defaultStorageLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('defaultStorageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccountName')), variables('defaultApiVersion')).key1)]"
            }
        }
    },
    ```
* Nakonfigurujte další dependsOn a additionalLinkedServiceNames hello HDInsight ondemand propojené služby:

    ```json
    {
        "dependsOn": [
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('defaultStorageLinkedServiceName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"

        ],
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "sshUserName": "myuser",                            
                "sshPassword": "MyPassword!",
                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                "additionalLinkedServiceNames": "[variables('defaultStorageLinkedServiceName')]"
            }
        }
    },            
    ```
## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak toouse Azure Data Factory toocreate na vyžádání HDInsight clusteru tooprocess úloh Hive. Další tooread:

* [Kurz Hadoopu: začněte používat systémem Linux Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Vytvořit clustery se systémem Linux Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Dokumentace prostředí HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)
* [Dokumentace k objektu pro vytváření dat](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a>Příloha

### <a name="azure-cli-script"></a>Azure CLI skriptu
Místo použití prostředí Azure PowerShell toodo hello kurzu můžete použít rozhraní příkazového řádku Azure. toouse rozhraní příkazového řádku Azure, nejprve nainstalujte rozhraní příkazového řádku Azure podle pokynů hello:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-tooprepare-hello-storage-and-copy-hello-files"></a>Používat rozhraní příkazového řádku Azure tooprepare hello úložiště a kopírovat soubory hello

```
azure login

azure config mode arm

azure group create --name "<Azure Resource Group Name>" --location "East US 2"

azure storage account create --resource-group "<Azure Resource Group Name>" --location "East US 2" --type "LRS" <Azure Storage Account Name>

azure storage account keys list --resource-group "<Azure Resource Group Name>" "<Azure Storage Account Name>"
azure storage container create "adfgetstarted" --account-name "<Azure Storage AccountName>" --account-key "<Azure Storage Account Key>"

azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
```

název kontejneru Hello je *adfgetstarted*. Protože se jedná, uchovávejte ho. V opačném případě je nutné šablony Resource Manageru tooupdate hello. Pokud potřebujete pomoc s Tento skript rozhraní příkazového řádku, přečtěte si [hello pomocí rozhraní příkazového řádku Azure s Azure Storage](../storage/common/storage-azure-cli.md).
