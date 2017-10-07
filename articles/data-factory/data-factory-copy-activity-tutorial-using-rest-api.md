---
title: "Kurz: Použití rozhraní REST API toocreate kanál služby Azure Data Factory | Microsoft Docs"
description: "V tomto kurzu použijete rozhraní API REST toocreate kanál služby Azure Data Factory s aktivitou kopírování toocopy dat z Azure blob storage Azure SQL database."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1704cdf8-30ad-49bc-a71c-4057e26e7350
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: aa6c9b035101c4ff9acff90117ca6e3e7067f418
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-rest-api-toocreate-an-azure-data-factory-pipeline-toocopy-data"></a>Kurz: Toocreate pomocí REST API Azure Data Factory kanálu toocopy dat 
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

V tomto článku se dozvíte, jak toouse REST API toocreate objekt pro vytváření dat s kanál, který kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure. Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článek před provedením tohoto kurzu.   

V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování. Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště. Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem. Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).

Kanál může obsahovat víc než jednu aktivitu. A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE]
> Tento článek nepopisuje všechny hello REST API služby Data Factory. Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Rozhraní REST API služby Data Factory - referenční informace](/rest/api/datafactory/).
>  
> Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Požadavky
* Projděte [přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a dokončení hello **požadavek** kroky.
* Nainstalujte na svůj počítač nástroj [Curl](https://curl.haxx.se/dlwiz/). Použijete nástroj Curl hello s toocreate příkazů REST služby data factory. 
* Postupujte podle pokynů v [tomto článku](../azure-resource-manager/resource-group-create-service-principal-portal.md) a proveďte následující: 
  1. V Azure Active Directory vytvořte webovou aplikaci s názvem **ADFCopyTutorialApp**.
  2. Získejte **ID klienta** a **tajný klíč**. 
  3. Získejte **ID tenanta**. 
  4. Přiřadit hello **ADFCopyTutorialApp** aplikace toohello **Data Factory Přispěvatel** role.  
* Nainstalujte [Azure PowerShell](/powershell/azure/overview).  
* Spusťte **prostředí PowerShell** a hello následující kroky. Nechte prostředí Azure PowerShell otevřené až hello konce tohoto kurzu. Pokud zavřete a znovu otevřete, toorun hello příkazů je třeba znovu.
  
  1. Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure:
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet:

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello. Nahraďte  **&lt;NameOfAzureSubscription** &gt; s názvem hello předplatného Azure. 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** tak, že spustíte následující příkaz v hello prostředí PowerShell hello:  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      Pokud skupina prostředků hello již existuje, zadejte zda tooupdate ho (Y) nebo jej zachovat jako (ne). 
     
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
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.json
> [!IMPORTANT]
> Položky **accountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem. toolearn tooget úložiště přístupu klíče najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).

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

Podrobné informace o vlastnostech JSON najdete v tématu [Propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service).

### <a name="azuersqllinkedservicejson"></a>azuersqllinkedservice.json
> [!IMPORTANT]
> Nahraďte **servername**, **databasename**, **uživatelské jméno**, a **heslo** s názvem serveru Azure SQL, název databáze SQL uživatele účet a heslo pro účet hello.  
> 
>

```JSON
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

Podrobné informace o vlastnostech JSON najdete v tématu [Propojená služba Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties).

### <a name="inputdatasetjson"></a>inputdataset.json

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
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
    "type": "AzureBlob",
    "linkedServiceName": "AzureStorageLinkedService",
    "typeProperties": {
      "folderPath": "adftutorial/",
      "fileName": "emp.txt",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ","
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

| Vlastnost | Popis |
|:--- |:--- |
| type | Hello vlastnost Typ nastavena příliš**AzureBlob** protože data se nachází v Azure blob storage. |
| linkedServiceName | Odkazuje toohello **AzureStorageLinkedService** kterou jste vytvořili dříve. |
| folderPath | Určuje objekt blob hello **kontejneru** a hello **složky** , který obsahuje vstupní objekty BLOB. V tomto kurzu adftutorial hello kontejner objektů blob, složka hello kořenové složky. | 
| fileName | Tato vlastnost je nepovinná. Pokud ji vynecháte, jsou vybrány všechny soubory z hello folderPath. V tomto kurzu **emp.txt** zadaný pro hello název souboru, takže pouze tento soubor je převzata pro zpracování. |
| format -> type |vstupní soubor Hello je ve formátu textu hello, takže použijeme **TextFormat**. |
| columnDelimiter | Hello sloupců ve vstupním souboru hello oddělené **čárku (`,`)**. |
| frequency/interval | je příliš nastavena frekvence Hello**hodinu** a interval je nastaven příliš**1**, což znamená, že hello vstupní řezy jsou dostupné **každou hodinu**. Jinými slovy, hello služba Data Factory hledá vstupní data každou hodinu v kořenové složce hello kontejner objektů blob (**adftutorial**) jste zadali. Hledá hello dat v rámci hello kanálu počáteční a koncové dobu, není před nebo po této doby.  |
| external | Tato vlastnost nastavena příliš**true** Pokud hello data nevygenerovala pomocí tohoto kanálu. Hello vstupní data v tomto kurzu je v souboru emp.txt hello, který není generované tímto kanálem, abyste nám nastavit tuto vlastnost tootrue. |

Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).

### <a name="outputdatasetjson"></a>outputdataset.json

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
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
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "emp"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:

| Vlastnost | Popis |
|:--- |:--- |
| type | Hello vlastnost Typ nastavena příliš**AzureSqlTable** protože data jsou zkopírované tooa tabulky v databázi Azure SQL. |
| linkedServiceName | Odkazuje toohello **AzureSqlLinkedService** kterou jste vytvořili dříve. |
| tableName | Zadaný hello **tabulky** toowhich hello data budou zkopírována. | 
| frequency/interval | Hello je nastavena frekvence příliš**hodinu** a interval je **1**, což znamená, že vytváří řezy výstup hello **každou hodinu** mezi hello kanálu počáteční a koncové krát, ne dřív nebo Po této doby.  |

Existují tři sloupce – **ID**, **FirstName**, a **LastName** – v tabulce emp hello v databázi hello. ID je sloupec identity, takže je nutné pouze toospecify **FirstName** a **LastName** sem.

Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure SQL](data-factory-azure-sql-connector.md#dataset-properties).

### <a name="pipelinejson"></a>pipeline.json

```JSON
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "description": "Push Regional Effectiveness Campaign data tooAzure SQL database",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2017-05-11T00:00:00Z",
    "end": "2017-05-12T00:00:00Z"
  }
}
```

Všimněte si hello následující body:

- V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**. Další informace o aktivitě kopírování hello najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md). V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).
- Vstup aktivity hello nastaven příliš**AzureBlobInput** a výstup hello aktivity je nastavený příliš**AzureSqlOutput**. 
- V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello. Úplný seznam úložišť dat nepodporuje aktivity kopírování hello jako zdroje a jímky, najdete v části [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats). toolearn jak toouse konkrétní podporované datové úložiště jako zdroj/jímka, klikněte na odkaz hello v tabulce hello.  
 
Nahraďte hodnotu hello hello **spustit** vlastnost s hello aktuální den a **end** hodnotu s hello další den. Můžete zadat jenom část data hello a přeskočit část času hello hello datum a čas. Například "2017-02-03", který je ekvivalentní příliš "2017-02-03T00:00:00Z"
 
Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2016-10-14T16:32:41Z. Hello **end** čas je nepovinný, ale používáme v tomto kurzu. 
 
Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin**". bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost.
 
V předchozím příkladu hello jsou 24 datových řezů jako se vytvářejí každou hodinu.

Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md). Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md). Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md). Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).

## <a name="set-global-variables"></a>Nastavení globálních proměnných
V prostředí Azure PowerShell spusťte následující příkazy po nahrazení hello hodnoty vlastními hello:

> [!IMPORTANT]
> Postup získání ID klienta, tajného klíče klienta, ID tenanta a ID předplatného naleznete v sekci [Požadavky](#prerequisites).   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
```

Spusťte následující příkaz po aktualizaci hello název objektu pro vytváření dat hello, který používáte hello: 

```
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a>Ověření pomocí ADD
Spusťte následující příkaz tooauthenticate s Azure Active Directory (AAD) hello: 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
V tomto kroku vytvoříte objekt služby Azure Data Factory s názvem **ADFCopyTutorialDF**. Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Například úložiště dat toocopy aktivity kopírování ze zdroje dat cílový tooa. Toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data tooproduct výstupní data. Spusťte následující příkazy toocreate hello objekt pro vytváření dat hello: 

1. Přiřadit toovariable hello příkaz s názvem **cmd**. 
   
    > [!IMPORTANT]
    > Potvrďte, že hello název objektu pro vytváření dat hello zde určíte název zadaný v hello hello (ADFCopyTutorialDF) odpovídá **datafactory.json**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF0411?api-version=2015-10-01};
    ```
2. Spusťte příkaz hello pomocí **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zobrazení výsledků hello. Pokud objekt pro vytváření dat hello byl úspěšně vytvořen, zobrazí hello JSON pro vytváření dat hello v hello **výsledky**, jinak se zobrazí chybová zpráva.  
   
    ```
    Write-Host $results
    ```

Všimněte si hello následující body:

* Název Hello hello objekt pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí chyba hello ve výsledcích: **název objektu pro vytváření dat "ADFCopyTutorialDF" není k dispozici**, hello následující kroky:  
  
  1. Změna hello název (například yournameADFCopyTutorialDF) v hello **datafactory.json** souboru.
  2. V první příkaz hello kde hello **$cmd** přiřazena hodnota proměnné, nahraďte ADFCopyTutorialDF hello nový název a spusťte příkaz hello. 
  3. Spusťte hello následující dva příkazy tooinvoke hello REST API toocreate hello data factory a tisk hello výsledky operace hello. 
     
     V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
* instance služby Data Factory toocreate, je nutné toobe přispěvatelem/správcem předplatného Azure hello
* Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.
* Pokud se zobrazí chyba hello: "**toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory**", proveďte jednu z následujících hello a zkuste to znovu publikovat: 
  
  * V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello: 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Můžete spustit následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Přihlášení pomocí hello předplatného Azure hello [portál Azure](https://portal.azure.com) a přejděte tooa okno objekt pro vytváření dat (nebo) vytvořte objekt pro vytváření dat v hello portálu Azure. Tato akce automaticky registruje zprostředkovatele hello za vás.

Před vytvořením kanálu, je nutné toocreate několik entit služby Data Factory nejdřív. Nejprve vytvoříte propojené služby toolink zdrojové a cílové data ukládá tooyour data uložit. Potom zadejte vstupní a výstupní datové sady toorepresent data v propojených úložištích dat. Nakonec vytvořte hello kanálu s aktivitou, která používá tyto datové sady.

## <a name="create-linked-services"></a>Vytvoření propojených služeb
Vytvoření propojených služeb v objektu pro vytváření toolink dat, data se ukládá a výpočetní služby toohello data factory. V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics. Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl). Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.  

Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu. Tento účet úložiště se hello, jeden ve které jste vytvořili kontejner a objemu odeslaných dat hello jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu. Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi. Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).  

### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure. Zadejte název hello a klíč účtu úložiště Azure v této části. V tématu [propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o toodefine vlastnosti používané JSON Azure Storage, propojené služby.  

1. Přiřadit toovariable hello příkaz s názvem **cmd**. 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. Spusťte příkaz hello pomocí **Invoke-Command**.

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zobrazení výsledků hello. Pokud hello propojené služby byla úspěšně vytvořena, najdete v části hello JSON hello propojené služby hello **výsledky**, jinak se zobrazí chybová zpráva.

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a>Vytvoření propojené služby Azure SQL
V tomto kroku propojíte datovou továrnu tooyour databáze Azure SQL. Zadejte název serveru Azure SQL hello, název databáze, uživatelské jméno a heslo uživatele v této části. V tématu [propojená služba Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) podrobnosti o toodefine vlastnosti používané JSON Azure SQL propojené služby.

1. Přiřadit toovariable hello příkaz s názvem **cmd**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
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
V předchozím kroku hello jste vytvořili propojené služby toolink, účet úložiště Azure a Azure SQL database tooyour objekt pro vytváření dat. V tomto kroku definujete dvě datové sady s názvem AzureBlobInput a AzureSqlOutput, které představují vstupní a výstupní data, která jsou uložená v úložištích dat hello odkazuje AzureStorageLinkedService a AzureSqlLinkedService.

Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure. A datovou sadu hello vstupního objektu blob (AzureBlobInput) určuje hello kontejneru a složce hello, který obsahuje vstupní data hello.  

Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. A hello výstupní datovou sadu tabulky SQL (OututDataset) určuje hello tabulky v hello databáze hello toowhich zkopírování dat z úložiště objektů blob hello. 

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
V tomto kroku vytvoříte datové sady s názvem AzureBlobInput, který ukazuje soubor blob tooa (emp.txt) v kořenové složce hello kontejner objektů blob (adftutorial) v hello Azure Storage reprezentované hello AzureStorageLinkedService propojené služby. Pokud nechcete zadat hodnotu pro název souboru hello (nebo ji přeskočit), jsou data ze všech objektů BLOB ve složce vstupní hello zkopírovaný toohello cílový. V tomto kurzu zadejte hodnotu pro název souboru hello. 

1. Přiřadit toovariable hello příkaz s názvem **cmd**. 

    ```PowerSHell   
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
Hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database. Hello výstupní SQL tabulky datové sady (OututDataset) v tomto kroku vytvoříte Určuje, že se zkopíruje hello tabulky v hello databáze toowhich hello dat z úložiště objektů blob hello.

1. Přiřadit toovariable hello příkaz s názvem **cmd**.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
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
V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **AzureBlobInput** jako vstup a **AzureSqlOutput** jako výstup.

Výstupní datové sady v současné době je, jaké jednotky hello plán. V tomto kurzu výstupní datové sady je nakonfigurované tooproduce řez jednou za hodinu. Hello kanálu má čas spuštění a čas ukončení, které jsou od sebe, jeden den, což je 24 hodin. Proto 24 řezů výstupní datové sady vyprodukované hello kanálu. 

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

**Blahopřejeme!** Úspěšně jste vytvořili objekt pro vytváření dat Azure, s kanál, který kopíruje data z databáze SQL Azure Blob Storage tooAzure.

## <a name="monitor-pipeline"></a>Monitorování kanálu
V tomto kroku použijete řezy toomonitor REST API služby Data Factory vytvářen hello kanálu.

```PowerShell
$ds ="AzureSqlOutput"
```

> [!IMPORTANT] 
> Ujistěte se, že hello počáteční a koncové časy zadané v hello následující příkaz shodu hello spuštění a ukončení hello kanálu. 

```PowerShell
$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=2017-05-11T00%3a00%3a00.0000000Z"&"end=2017-05-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};
```

```PowerShell
$results2 = Invoke-Command -scriptblock $cmd;
```

```PowerShell
IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

Spustit hello Invoke-Command a hello dalšímu, dokud neuvidíte řez v **připraven** stavu nebo **se nezdařilo** stavu. Když hello řez ve stavu Připraveno, zkontrolujte hello **emp** tabulky v databázi Azure SQL pro hello výstupní data. 

Pro každý řez jsou dva řádky dat ze zdrojového souboru hello zkopírovaný toohello tabulce emp v databázi Azure SQL hello. Proto byste vidět 24 nové záznamy v tabulce emp hello při všech řezech hello úspěšně zpracování (ve stavu Připraveno). 

## <a name="summary"></a>Souhrn
V tomto kurzu jste použili toocreate REST API služby Azure data factory toocopy data z Azure SQL database tooan objektů blob v Azure. Zde jsou hello kroků, které jste provedli v tomto kurzu:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste **propojené služby**:
   1. Azure Storage propojené služby toolink účtu úložiště Azure, který obsahuje vstupní data.     
   2. Azure SQL propojené toolink služby Azure SQL database, který obsahuje hello výstupní data. 
3. Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4. Vytvořili jste **kanál** s aktivitou kopírování, která má jako zdroj BlobSource a jako jímku SqlSink. 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat. Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.
