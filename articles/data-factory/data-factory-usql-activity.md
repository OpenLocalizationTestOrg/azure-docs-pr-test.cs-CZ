---
title: "aaaTransform data pomocí skriptu U-SQL - Azure | Microsoft Docs"
description: "Zjistěte, jak tooprocess nebo transformace dat pomocí spouštění skriptů U-SQL v Azure Data Lake Analytics výpočetní služby."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: 51fdb40334d0c131720f65c3a96b4c5045a98b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Transformace dat pomocí spouštění skriptů U-SQL v Azure Data Lake Analytics 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Aktivita Hive](data-factory-hive-activity.md) 
> * [Pig aktivity](data-factory-pig-activity.md)
> * [Činnost MapReduce](data-factory-map-reduce.md)
> * [Streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Spark aktivity](data-factory-spark.md)
> * [Aktivita Provedení dávky služby Machine Learning](data-factory-azure-ml-batch-execution-activity.md)
> * [Aktivita Aktualizace prostředků služby Machine Learning](data-factory-azure-ml-update-resource-activity.md)
> * [Aktivita Uložená procedura](data-factory-stored-proc-activity.md)
> * [Aktivita U-SQL služby Data Lake Analytics](data-factory-usql-activity.md)
> * [Vlastní aktivity rozhraní .NET](data-factory-use-custom-activities.md)

Kanál v objektu pro vytváření dat Azure zpracovává data v služby propojené úložiště pomocí propojené výpočetní služby. Obsahuje posloupnost aktivit, kde každá aktivita provede konkrétní zpracování operace. Tento článek popisuje hello **Data Lake Analytics U-SQL aktivity** , která se spouští **U-SQL** skript na **Azure Data Lake Analytics** výpočetní propojené služby. 

> [!NOTE]
> Vytvoření účtu Azure Data Lake Analytics před vytvořením kanálu s aktivitou Data Lake Analytics U-SQL. toolearn o Azure Data Lake Analytics najdete v části [Začínáme s Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
> 
> Zkontrolujte hello [vytvoření vaší první kurz kanálu](data-factory-build-your-first-pipeline.md) pro podrobné kroky toocreate objekt pro vytváření dat, propojené služby, datových sad a kanálu. Fragmenty kódu JSON pomocí editoru služby Data Factory nebo entit služby Data Factory toocreate Visual Studio nebo Azure PowerShell.

## <a name="supported-authentication-types"></a>Typy podporované ověřování
Aktivita U-SQL podporuje následující typy ověřování proti Data Lake Analytics:
* Ověřování instančních objektů
* Ověření přihlašovacích údajů (OAuth) uživatele 

Doporučujeme použít objekt zabezpečení ověřování služby, zejména pro naplánovaného spuštění U-SQL. Vypršení platnosti tokenu chování mohou nastat u ověření přihlašovacích údajů uživatele. Podrobnosti konfigurace najdete v tématu hello [propojené vlastnosti služby](#azure-data-lake-analytics-linked-service) části.

## <a name="azure-data-lake-analytics-linked-service"></a>Propojená služba Azure Data Lake Analytics
Vytvoříte **Azure Data Lake Analytics** propojené služby toolink služby Azure Data Lake Analytics výpočetní služby tooan Azure data factory. Hello Data Lake Analytics U-SQL aktivitu v kanálu hello odkazuje toothis propojené služby. 

Hello následující tabulka obsahuje popis hello obecné vlastnosti používané ve hello definici JSON. Dále můžete mezi instanční objekt a ověření přihlašovacích údajů uživatele.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| **Typ** |vlastnost typu Hello by měla být nastavena na: **AzureDataLakeAnalytics**. |Ano |
| **název účtu** |Název účtu Azure Data Lake Analytics. |Ano |
| **dataLakeAnalyticsUri** |Identifikátor URI služby Azure Data Lake Analytics. |Ne |
| **ID předplatného** |Id předplatného Azure |Ne (když není určeno předplatné hello se používá pro vytváření dat). |
| **Název skupiny prostředků** |Název skupiny prostředků Azure. |Ne (když není určeno skupiny prostředků hello se používá pro vytváření dat). |

### <a name="service-principal-authentication-recommended"></a>Objekt zabezpečení ověřování služby (doporučeno)
objekt zabezpečení ověřování služby toouse registrace entity aplikace v Azure Active Directory (Azure AD) a udělte ho hello přístup tooData Lake Store. Podrobné pokyny najdete v tématu [Service-to-service ověřování](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Poznamenejte si hello následující hodnoty, které používáte toodefine hello propojené služby:
* ID aplikace
* Klíč aplikace 
* ID tenanta

Objekt zabezpečení ověřování služby použijte zadáním hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| **servicePrincipalId** | Zadejte ID aplikace hello klienta. | Ano |
| **servicePrincipalKey** | Zadejte klíč aplikace hello. | Ano |
| **klienta** | Zadejte informace klienta hello (název nebo klienta domény ID) v rámci které se nachází aplikace. Můžete jej načíst po výběru ukázáním hello myši v pravém horním rohu hello hello portálu Azure. | Ano |

**Příkladu: Ověření objektu službu**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Ověření pověření uživatele
Alternativně můžete použít ověřování pověření uživatele pro Data Lake Analytics zadáním hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| **autorizace** | Klikněte na tlačítko hello **Autorizovat** tlačítka na hello editoru služby Data Factory a zadejte svoje přihlašovací údaje, který přiřazuje hello automaticky vygenerovanou autorizace URL toothis vlastnost. | Ano |
| **ID relace** | ID relace OAuth z autorizační relace, hello OAuth. Každé ID relace je jedinečné a může být použit pouze jednou. Toto nastavení se automaticky generuje při použití hello editoru služby Data Factory. | Ano |

**Příklad: Ověření pověření uživatele**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Vypršení platnosti tokenu
Hello autorizační kód vygenerovaného s využitím hello **Autorizovat** tlačítko vyprší po určité době. Viz následující tabulka pro hello vypršení platnosti časy pro různé typy uživatelských účtů hello. Zobrazí následující chybová zpráva hello při hello ověřování **platnost tokenu vyprší**: přihlašovací údaje chyby operace: invalid_grant - AADSTS70002: Chyba při ověřování přihlašovacích údajů. AADSTS70008: hello údaje udělení přístupu vypršela platnost nebo odvolat. ID trasování: ID korelace d18629e8-af88-43c5-88e3-d8419eb1fca1: časové razítko fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21:09:31Z

| Typ uživatele | Platnost vyprší po |
|:--- |:--- |
| Uživatelské účty, které nejsou spravované přes Azure Active Directory (@hotmail.com, @live.comatd.) |12 hodin |
| Účty uživatelů spravované pomocí Azure Active Directory (AAD) |14 dnů po poslední řez hello spustit. <br/><br/>90 dnů, pokud řezu založeného na základě OAuth propojené služby používá alespoň jednou za 14 dní. |

tooavoid nebo vyřešit tuto chybu, opětovné pověření pomocí hello **Authorize** když hello **platnost tokenu vyprší** a znovu nasaďte hello propojené služby. Můžete také vygenerovat hodnoty pro **sessionId** a **autorizace** vlastnosti programově pomocí kódu následujícím způsobem:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

V tématu [azuredatalakestorelinkedservice třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), a [AuthorizationSessionGetResponse třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témata podrobnosti o třídách Data Factory hello používá v kódu hello. Přidat odkaz na: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll pro hello WindowsFormsWebAuthenticationDialog třídy. 

## <a name="data-lake-analytics-u-sql-activity"></a>Aktivita U-SQL služby Data Lake Analytics
Definuje Hello následujícím fragmentu kódu JSON kanálu s aktivitou Data Lake Analytics U-SQL. definici aktivity Hello má toohello referenční dokumentace Azure Data Lake Analytics propojené služby, kterou jste vytvořili dříve.   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline toocompute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

Hello následující tabulka popisuje názvy a popisy vlastností, které jsou specifické toothis aktivity. 

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type |musí být nastavena vlastnost typu Hello příliš**DataLakeAnalyticsU SQL**. |Ano |
| scriptPath |Cesta toofolder, který obsahuje skript hello U-SQL. Název souboru hello rozlišuje velká a malá písmena. |Ne (když používáte skript) |
| scriptLinkedService |Propojené služby, který odkazuje hello úložiště, který obsahuje toohello hello skriptu pro vytváření dat |Ne (když používáte skript) |
| Skript |Zadejte místo zadání scriptPath a scriptLinkedService zpracování vloženého skriptu. Například: `"script": "CREATE DATABASE test"`. |Ne (když používáte scriptPath a scriptLinkedService) |
| degreeOfParallelism |maximální počet uzlů Hello současně použít toorun hello úlohy. |Ne |
| Priorita |Určuje, které z uložených ve frontě úloh by měl být vybrané toorun nejdřív. Hello nižší hello číslo, vyšší prioritu hello hello. |Ne |
| parameters |Parametry pro skript hello U-SQL |Ne |
| runtimeVersion | Verze runtime toouse modul hello U-SQL | Ne | 
| compilationMode | <p>Režim kompilace U-SQL. Musí být jedna z těchto hodnot:</p> <ul><li>**Sémantické:** provádět jenom sémantického kontroly a nezbytné správností kontroly.</li><li>**Úplné:** provést úplné kompilace hello, včetně kontrola syntaxe, optimalizace, generování kódu atd.</li><li>**SingleBox:** provést úplné kompilace hello s tooSingleBox TargetType nastavení.</li></ul><p>Pokud nezadáte hodnotu pro tuto vlastnost, hello server určuje režim optimální kompilace hello. </p>| Ne | 

V tématu [definice skriptu SearchLogProcessing.txt](#sample-u-sql-script) pro definici skriptu hello. 

## <a name="sample-input-and-output-datasets"></a>Ukázkové vstupní a výstupní datové sady
### <a name="input-dataset"></a>Vstupní datové sady
V tomto příkladu hello vstupní data nachází v Azure Data Lake Store (soubor SearchLog.tsv soubor ve složce datalake/vstup hello). 

```json
{
    "name": "DataLakeTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

### <a name="output-dataset"></a>Výstupní datové sady
V tomto příkladu hello výstupních dat vytvářených hello skript U-SQL uložený v Azure Data Lake Store (datalake výstupní složka). 

```json
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="sample-data-lake-store-linked-service"></a>Ukázková Data Lake Store propojená služba
Zde je definice hello hello ukázky Azure Data Lake Store propojené služby používané hello vstupní a výstupní datové sady. 

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
        }
    }
}
```

V tématu [přesunout tooand dat z Azure Data Lake Store](data-factory-azure-datalake-connector.md) článku popisy vlastností JSON. 

## <a name="sample-u-sql-script"></a>Ukázkový skript U-SQL

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    too@out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

Hello hodnoty pro  **@in**  a  **@out**  parametry v hello U-SQL skriptů jsou předaná dynamicky ADF pomocí oddílu "parametry" hello. Naleznete hello 'parametry' v definici kanálu hello.

Také můžete zadat další vlastnosti, například degreeOfParallelism a priority v definici vaší kanálu pro hello úlohy, které běží na hello služby Azure Data Lake Analytics.

## <a name="dynamic-parameters"></a>Dynamické parametry
V definici kanálu ukázka hello a odhlašování parametry jsou přiřazeny pevně definovaných hodnot. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Místo toho je možné toouse dynamických parametrů. Například: 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

V takovém případě vstupní soubory jsou stále zachyceny ze složky /datalake/input hello a výstupní soubory se generují ve složce /datalake/output hello. názvy souborů Hello jsou dynamické podle času zahájení řez hello.  

