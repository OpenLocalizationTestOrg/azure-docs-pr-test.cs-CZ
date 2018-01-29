---
title: "Událost mřížky a Event Hubs integrace se službou Azure"
description: "Popisuje, jak používat Azure událostí mřížky a Event Hubs k migraci dat do SQL Data Warehouse"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/19/2018
ms.author: tomfitz
ms.openlocfilehash: b315bd77a47a6f106c5768da56828a5169de5fe9
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/20/2018
---
# <a name="stream-big-data-into-a-data-warehouse"></a>Datový proud velkých objemů dat do datového skladu

Azure [událostí mřížky](overview.md) je služba Směrování inteligentního událost, která umožňuje reagovat na oznámení z aplikace a služby. [Zaznamenat centra událostí a událostí mřížky ukázku](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) ukazuje, jak pomocí Azure Event Hubs zachycení událostí mřížky Azure bezproblémově migrace dat z centra událostí do SQL Data Warehouse.

![Přehled aplikace](media/event-grid-event-hubs-integration/overview.png)

Data se odešlou do centra událostí, zachycení získává data z datového proudu a generuje úložiště objektů BLOB s daty ve formátu Avro. Pokud zachycení vygeneruje objektu blob, aktivuje událost. Událost mřížky distribuuje data o události odběratelům. V takovém případě data události je odeslána koncovému bodu Azure Functions. Data události obsahuje cestu generovaný objekt blob. Funkce používá tuto adresu URL k načtení souboru a jeho odeslání do datového skladu.

V tomto článku můžete:

* Nasazení infrastruktury následující:
  * Centra událostí s zachycení povoleno
  * Účet úložiště pro soubory z zachycení
  * Plán služby Azure app service pro hostování aplikace – funkce
  * Funkce aplikace pro zpracování události
  * SQL Server pro hostování datového skladu
  * Datový sklad SQL pro ukládání migrovaná data
* Vytvoření tabulky v datovém skladu
* Přidání kódu do aplikaci funkce
* Přihlášení k odběru události
* Spuštění aplikace, který odesílá data do centra událostí
* Zobrazit migrovaná data v datovém skladu

## <a name="about-the-event-data"></a>O data události

Událost mřížky distribuuje data události odběratelům. Následující příklad zobrazuje data událostí pro vytvoření souboru zachycení. Zejména, Všimněte si `fileUrl` vlastnost `data` objektu. Funkce aplikace získá tuto hodnotu a používá je načíst soubor zachycení.

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu potřebujete:

* Předplatné Azure. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.
* [Visual studio 2017 verze 15.3.2 nebo větší](https://www.visualstudio.com/vs/) s pracovních zátěží pro: vývoj aplikací rozhraní .NET, vývoj pro Azure, ASP.NET a webové vývoj, vývoj Node.js a Python vývoj.
* [EventHubsCaptureEventGridDemo ukázkový projekt](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) stažena do počítače.

## <a name="deploy-the-infrastructure"></a>Nasazení infrastruktury

Ke zjednodušení tohoto článku, nasadíte požadované infrastrukturu pomocí šablony Resource Manageru. Pokud chcete zobrazit prostředky, které jsou nasazeny, zobrazit [šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json). Ve verzi preview podporuje událostí mřížky **westus2** a **westcentralus** oblasti. Použijte jednu z těchto oblastí pro umístění skupiny prostředků.

Pokud používáte Azure CLI, použijte:

```azurecli-interactive
az group create -l westcentralus -n rgDataMigrationSample

az group deployment create \
  --resource-group rgDataMigrationSample \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json \
  --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
```

Pokud používáte PowerShell, použijte:

```powershell
New-AzureRmResourceGroup -Name rgDataMigration -Location westcentralus

New-AzureRmResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
```

Zadejte hodnotu heslo po zobrazení výzvy.

## <a name="create-a-table-in-sql-data-warehouse"></a>Vytvoření tabulky v SQL Data Warehouse

Přidání tabulky do datového skladu tak, že spustíte [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) skriptu. Pro spuštění skriptu, použijte Visual Studio nebo editoru dotazů na portálu.

Spuštění skriptu je:

```sql
CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
    [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [MeasureTime] datetime NULL, 
    [GeneratedPower] float NULL, 
    [WindSpeed] float NULL, 
    [TurbineSpeed] float NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
```

## <a name="publish-the-azure-functions-app"></a>Publikování aplikace Azure Functions

1. Otevřete [EventHubsCaptureEventGridDemo ukázkový projekt](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) ve Visual Studio 2017 (15.3.2 nebo vyšší).

2. V Průzkumníku řešení klikněte pravým tlačítkem na **FunctionDWDumper**a vyberte **publikovat**.

   ![Publikování aplikace – funkce](media/event-grid-event-hubs-integration/publish-function-app.png)

3. Vyberte **aplikaci Azure funkce** a **vyberte existující**. Vyberte **OK**.

   ![Cílové aplikace – funkce](media/event-grid-event-hubs-integration/pick-target.png)

4. Vyberte aplikaci funkce, které jste nasadili prostřednictvím šablony. Vyberte **OK**.

   ![Vyberte aplikaci, – funkce](media/event-grid-event-hubs-integration/select-function-app.png)

5. Když Visual Studio nakonfiguroval profil, vyberte **publikovat**.

   ![Vyberte publikování](media/event-grid-event-hubs-integration/select-publish.png)

6. Po publikování funkce, přejděte na [portál Azure](https://portal.azure.com/). Vyberte prostředek skupiny a funkce aplikace.

   ![Zobrazit aplikaci – funkce](media/event-grid-event-hubs-integration/view-function-app.png)

7. Vyberte funkci.

   ![Vyberte funkci](media/event-grid-event-hubs-integration/select-function.png)

8. Získáte adresu URL pro tuto funkci. Tato adresa URL musíte při vytváření odběru událostí.

   ![Získat adresu URL funkce](media/event-grid-event-hubs-integration/get-function-url.png)

9. Zkopírujte hodnotu.

   ![Zkopírujte adresu URL](media/event-grid-event-hubs-integration/copy-url.png)

## <a name="subscribe-to-the-event"></a>Přihlášení k odběru události

Rozhraní příkazového řádku Azure nebo na portálu můžete použít k odběru události. Tento článek ukazuje obou přístupů.

### <a name="portal"></a>Portál

1. Obor názvů služby Event Hubs, vyberte **událostí mřížky** na levé straně.

   ![Vyberte události mřížky](media/event-grid-event-hubs-integration/select-event-grid.png)

2. Přidáte odběr událostí.

   ![Přidání odběru událostí](media/event-grid-event-hubs-integration/add-event-subscription.png)

3. Zadejte hodnoty pro událost odběru. Použijte adresu URL funkce Azure, který jste zkopírovali. Vyberte **Vytvořit**.

   ![Zadejte hodnoty předplatného](media/event-grid-event-hubs-integration/provide-values.png)

### <a name="azure-cli"></a>Azure CLI

K odběru události, spusťte následující příkazy (které vyžadují verzi 2.0.24 nebo novější z příkazového řádku Azure CLI):

```azurecli-interactive
namespaceid=$(az resource show --namespace Microsoft.EventHub --resource-type namespaces --name <your-EventHubs-namespace> --resource-group rgDataMigrationSample --query id --output tsv)
az eventgrid event-subscription create \
  --resource-id $namespaceid \
  --name captureEventSub \
  --endpoint <your-function-endpoint>
```

## <a name="run-the-app-to-generate-data"></a>Spusťte aplikaci pro generování dat

Dokončení nastavení centra událostí, SQL data warehouse, aplikaci Azure funkce a předplatného události. Řešení je připraven k migraci dat z centra událostí do datového skladu. Než spustíte aplikaci, která generuje data pro centra událostí, musíte nakonfigurovat několik hodnot.

1. Na portálu zvolte svůj obor názvů centra událostí. Vyberte **připojovací řetězce**.

   ![Vyberte připojovací řetězce](media/event-grid-event-hubs-integration/event-hub-connection.png)

2. Select **RootManageSharedAccessKey**

   ![Vyberte klíč](media/event-grid-event-hubs-integration/show-root-key.png)

3. Kopírování **připojovací řetězec - primární klíč**

   ![Zkopírovat klíč](media/event-grid-event-hubs-integration/copy-key.png)

4. Přejděte zpět do projektu sady Visual Studio. Otevřete v projektu WindTurbineDataGenerator **program.cs**.

5. Nahraďte dvě konstantní hodnoty. Použijte zkopírovaný hodnotu pro **EventHubConnectionString**. Použijte název centra událostí pro **EventHubName**.

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://tfdatamigratens.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Sestavte řešení. Spusťte aplikaci WindTurbineGenerator.exe. Za několik minut dotaz v tabulce v datového skladu pro migrovaná data.

## <a name="next-steps"></a>Další postup

* Úvod k mřížce událostí, naleznete v části [o mřížky událostí](overview.md).
* Úvod k zachycení centra událostí, naleznete v části [povolit Event Hubs zaznamenat pomocí portálu Azure](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Další informace o nastavení a spuštění ukázku najdete v tématu [zaznamenat centra událostí a událostí mřížky ukázku](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).