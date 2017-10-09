---
title: "aaaData Factory - protokol změn rozhraní API .NET | Microsoft Docs"
description: "Popisuje nejnovější změny, přidání funkce a opravy chyb atd... v konkrétní verzi rozhraní API .NET pro hello Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8208271b-7f4c-4214-b665-d2ff503c4470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: spelluru
ms.openlocfilehash: 1d44b45c3dc8f9d483d1f1cef7068edacc610932
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---net-api-change-log"></a>Azure Data Factory – protokol změn rozhraní API .NET
Tento článek obsahuje informace o změny tooAzure Data Factory SDK v konkrétní verzi. Můžete najít hello nejnovější balíček NuGet pro Azure Data Factory [sem](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories)

## <a name="version-4110"></a>Verze 4.11.0
Přidání funkce:

* Hello následující typy propojené služby byly přidány:
  * [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
  * [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
  * [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
* byly přidány následující typy datovou sadu Hello:
  * [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
  * [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
* Zkopírujte následující Hello zdroje, které byly přidány typy:
  * [MongoDbSource](https://msdn.microsoft.com/library/mt765123.aspx)

## <a name="version-4100"></a>Verze 4.10.0
* byly přidány následující volitelné vlastnosti Hello tooTextFormat:
  * [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
  * [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
  * [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
* Hello následující typy propojené služby byly přidány:
  * [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
  * [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
* byly přidány následující typy datovou sadu Hello:
  * [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
* Zkopírujte následující Hello zdroje, které byly přidány typy:
  * [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
* Přidat [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) tooAzureMLBatchExecutionActivity vlastnost
  * Povolit předání více vstupů webové služby Azure Machine Learning experimentu tooan

## <a name="version-491"></a>Verze 4.9.1
### <a name="bug-fix"></a>Oprava chyby
* Přestat používat ověřování na základě WebApi pro [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Verze 4.9.0
### <a name="feature-additions"></a>Přidání funkce
* Přidat [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) a [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) tooCopyActivity vlastnosti. V tématu [připravený kopie](data-factory-copy-activity-performance.md#staged-copy) podrobnosti o funkci hello.

### <a name="bug-fix"></a>Oprava chyby
* Zavést přetížení [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) metoda, která přebírá [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) instance.
* Označit [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) a [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) jako volitelný v CopySink.

## <a name="version-480"></a>Verze 4.8.0
### <a name="feature-additions"></a>Přidání funkce
* Hello následující volitelné vlastnosti přidané aktivity tooCopy zadejte tooenable ladění výkonu kopie:
  * [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
  * [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Verze 4.7.0
### <a name="feature-additions"></a>Přidání funkce
* Přidat nový typ StorageFormat [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) zadejte toocopy soubory ve sloupcovém formátu (ORC) optimalizované řádek.
* Přidat [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) a tooSqlDWSink PolyBaseSettings vlastnosti.
  * Umožňuje použití hello PolyBase toocopy dat do SQL Data Warehouse.

## <a name="version-461"></a>Verze 4.6.1
### <a name="bug-fixes"></a>Opravy chyb
* Opravy požadavku HTTP pro výpis okna aktivity.
  * Název skupiny prostředků hello a název objektu pro vytváření dat hello odebere hello datová část požadavku.

## <a name="version-460"></a>Verze 4.6.0
### <a name="feature-additions"></a>Přidání funkce
* Hello následující vlastnosti jsou přidané příliš[PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
  * [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
  * [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
  * [Datové sady](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
* Hello následující vlastnosti jsou přidané příliš[PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
  * [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
* Přidat nové [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) typ [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) zadejte toodefine datové sady, jejichž data jsou ve formátu JSON.

## <a name="version-450"></a>Verze 4.5.0
### <a name="feature-additions"></a>Přidání funkce
* Přidat [seznam operací pro okna aktivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
  * Přidání metody tooretrieve aktivity windows s filtry na základě typů entit hello (tedy datové továrny, datové sady, kanály a aktivity).
* Hello následující typy propojené služby byly přidány:
  * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* byly přidány následující typy datovou sadu Hello:
  * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Zkopírujte následující Hello zdroje, které byly přidány typy:     
  * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Verze 4.4.0
### <a name="feature-additions"></a>Přidání funkce
* Hello následující typu propojené služby byl přidán jako zdroje dat a jímky kopie aktivity:
  * [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). V tématu [propojená služba Azure úložiště SAS](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) koncepční informace a příklady.

## <a name="version-430"></a>Verze 4.3.0
### <a name="feature-additions"></a>Přidání funkce
* Hello následující typy propojené služby nebyla některá byl přidán jako zdroje dat pro kopírování aktivity:
  * [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). V tématu [přesun dat z HDFS pomocí služby Data Factory](data-factory-hdfs-connector.md) koncepční informace a příklady.
  * [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). V tématu [přesunout data z ODBC datová úložiště pomocí Azure Data Factory](data-factory-odbc-connector.md) koncepční informace a příklady.

## <a name="version-420"></a>Verze 4.2.0
### <a name="feature-additions"></a>Přidání funkce
* byla přidána následující nového typu aktivity Hello: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Podrobnosti o aktivitě hello najdete v tématu [aktualizace Azure ML modelů pomocí hello aktivita prostředku aktualizace](data-factory-azure-ml-batch-execution-activity.md).
* Nové volitelná vlastnost [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) přidala toohello [AzureMLLinkedService třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx).
* [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) a [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) vlastnosti byly přidány toohello [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) třídy.
* Rutiny umožňují konfiguraci hello časové limity pro toohello volání klienta služby Data Factory.

## <a name="version-410"></a>Verze 4.1.0
### <a name="feature-additions"></a>Přidání funkce
* Hello následující typy propojené služby byly přidány:
  * [Azuredatalakestorelinkedservice.](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
  * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* byly přidány Hello následující typy aktivit:
  * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* byly přidány následující typy datovou sadu Hello:
  * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Hello následující typy zdroj a jímka pro aktivitu kopírování byly přidány:
  * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
  * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)

## <a name="version-401"></a>Verze 4.0.1
### <a name="breaking-changes"></a>Nejnovější změny
byly přejmenovány Hello následující třídy. Hello nové názvy byly hello původní názvy tříd před 4.0.0 verzi.

| Název v 4.0.0 | Název v 4.0.1 |
|:--- |:--- |
| AzureSqlDataWarehouseDataset |[AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx) |
| AzureSqlDataset |[AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx) |
| AzureDataset |[AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx) |
| OracleDataset |[OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx) |
| RelationalDataset |[RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx) |
| SqlServerDataset |[SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx) |

## <a name="version-400"></a>Verze 4.0.0
### <a name="breaking-changes"></a>Nejnovější změny
* byly přejmenovány Hello následující třídy nebo rozhraní.

| Starý název | Nový název |
|:--- |:--- |
| ITableOperations |[IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |
| Table |[Datové sady](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) |
| TableProperties |[DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) |
| TableTypeProprerties |[DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters |[DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) |
| TableCreateOrUpdateResponse |[DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) |
| TableGetResponse |[DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) |
| TableListResponse |[DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters |[DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) |

* Hello **seznamu** metody teď vrací stránkových výsledků. Pokud hello odpovědi obsahuje neprázdný **NextLink** vlastnost hello klientská aplikace musí toocontinue načítání hello další stránku dokud jsou vráceny všechny stránky.  Zde naleznete příklad:

    ```csharp
    PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
    var pipelines = new List<Pipeline>(response.Pipelines);

    string nextLink = response.NextLink;
    while (!string.IsNullOrEmpty(nextLink))
    {
        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
        pipelines.AddRange(nextResponse.Pipelines);

        nextLink = nextResponse.NextLink;
    }
    ```
* **Seznam** kanálu rozhraní API vrátí pouze hello kanálu místo úplné podrobnosti souhrnu. Například aktivity v kanálu souhrn obsahovat jenom název a typ.

### <a name="feature-additions"></a>Přidání funkce
* Hello [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) třída podporuje dva nové vlastnosti **SliceIdentifierColumnName** a **SqlWriterCleanupScript**, toosupport idempotent kopie tooAzure dat SQL Skladu. V tématu hello [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) podrobnosti o těchto vlastnostech najdete v článku.
* Teď podporují spuštěným uložené procedury pro Azure SQL Database a Azure SQL Data Warehouse zdroje v rámci hello aktivity kopírování. Hello [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) a [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) třídy mají hello následující vlastnosti: **SqlReaderStoredProcedureName** a **StoredProcedureParameters** . V tématu hello [Azure SQL Database](data-factory-azure-sql-connector.md#sqlsource) a [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) články na Azure.com. Podrobnosti o těchto vlastnostech.  
