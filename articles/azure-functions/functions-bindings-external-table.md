---
title: "Vazba funkce externí tabulky aaaAzure (Preview) | Microsoft Docs"
description: "Používání vazeb externí tabulky v Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: bf19d7d377232edc91087d5f4110602bb82c67ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-external-table-binding-preview"></a><span data-ttu-id="a1b2d-103">Externí tabulky funkcí vazby Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="a1b2d-103">Azure Functions External Table binding (Preview)</span></span>
<span data-ttu-id="a1b2d-104">Tento článek ukazuje, jak toomanipulate tabulková data v poskytovatelů SaaS (například služby Sharepoint, Dynamics) v rámci funkce s integrovanou vazby.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-104">This article shows how toomanipulate tabular data on SaaS providers (e.g. Sharepoint, Dynamics) within your function with built-in bindings.</span></span> <span data-ttu-id="a1b2d-105">Azure Functions podporuje vstupní a výstupní vazby pro externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-105">Azure Functions supports input, and output bindings for external tables.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a><span data-ttu-id="a1b2d-106">Připojení rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a1b2d-106">API Connections</span></span>

<span data-ttu-id="a1b2d-107">Tabulka vazby využívat externí rozhraní API připojení tooauthenticate zprostředkovatelům SaaS 3. stran.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-107">Table bindings leverage external API connections tooauthenticate with 3rd party SaaS providers.</span></span> 

<span data-ttu-id="a1b2d-108">Při přiřazování vazbu můžete buď vytvořit nové připojení rozhraní API, nebo použít stávající připojení k rozhraní API v rámci hello stejnou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="a1b2d-108">When assigning a binding you can either create a new API connection or use an existing API connection within hello same resource group</span></span>

### <a name="supported-api-connections-tables"></a><span data-ttu-id="a1b2d-109">Připojení podporované rozhraní API (tabulky) s</span><span class="sxs-lookup"><span data-stu-id="a1b2d-109">Supported API Connections (Table)s</span></span>

|<span data-ttu-id="a1b2d-110">konektor</span><span class="sxs-lookup"><span data-stu-id="a1b2d-110">Connector</span></span>|<span data-ttu-id="a1b2d-111">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="a1b2d-111">Trigger</span></span>|<span data-ttu-id="a1b2d-112">Vstup</span><span class="sxs-lookup"><span data-stu-id="a1b2d-112">Input</span></span>|<span data-ttu-id="a1b2d-113">Výstup</span><span class="sxs-lookup"><span data-stu-id="a1b2d-113">Output</span></span>|
|:-----|:---:|:---:|:---:|
|[<span data-ttu-id="a1b2d-114">DB2</span><span class="sxs-lookup"><span data-stu-id="a1b2d-114">DB2</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||<span data-ttu-id="a1b2d-115">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-115">x</span></span>|<span data-ttu-id="a1b2d-116">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-116">x</span></span>
|[<span data-ttu-id="a1b2d-117">Dynamics 365 pro operace</span><span class="sxs-lookup"><span data-stu-id="a1b2d-117">Dynamics 365 for Operations</span></span>](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||<span data-ttu-id="a1b2d-118">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-118">x</span></span>|<span data-ttu-id="a1b2d-119">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-119">x</span></span>
|[<span data-ttu-id="a1b2d-120">Dynamics 365</span><span class="sxs-lookup"><span data-stu-id="a1b2d-120">Dynamics 365</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="a1b2d-121">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-121">x</span></span>|<span data-ttu-id="a1b2d-122">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-122">x</span></span>
|[<span data-ttu-id="a1b2d-123">Dynamics NAV</span><span class="sxs-lookup"><span data-stu-id="a1b2d-123">Dynamics NAV</span></span>](https://msdn.microsoft.com/library/gg481835.aspx)||<span data-ttu-id="a1b2d-124">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-124">x</span></span>|<span data-ttu-id="a1b2d-125">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-125">x</span></span>
|[<span data-ttu-id="a1b2d-126">Tabulky Google</span><span class="sxs-lookup"><span data-stu-id="a1b2d-126">Google Sheets</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||<span data-ttu-id="a1b2d-127">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-127">x</span></span>|<span data-ttu-id="a1b2d-128">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-128">x</span></span>
|[<span data-ttu-id="a1b2d-129">Informix</span><span class="sxs-lookup"><span data-stu-id="a1b2d-129">Informix</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||<span data-ttu-id="a1b2d-130">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-130">x</span></span>|<span data-ttu-id="a1b2d-131">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-131">x</span></span>
|[<span data-ttu-id="a1b2d-132">Dynamics 365 pro Finance</span><span class="sxs-lookup"><span data-stu-id="a1b2d-132">Dynamics 365 for Financials</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="a1b2d-133">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-133">x</span></span>|<span data-ttu-id="a1b2d-134">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-134">x</span></span>
|[<span data-ttu-id="a1b2d-135">MySQL</span><span class="sxs-lookup"><span data-stu-id="a1b2d-135">MySQL</span></span>](https://docs.microsoft.com/azure/store-php-create-mysql-database)||<span data-ttu-id="a1b2d-136">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-136">x</span></span>|<span data-ttu-id="a1b2d-137">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-137">x</span></span>
|[<span data-ttu-id="a1b2d-138">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="a1b2d-138">Oracle Database</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||<span data-ttu-id="a1b2d-139">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-139">x</span></span>|<span data-ttu-id="a1b2d-140">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-140">x</span></span>
|[<span data-ttu-id="a1b2d-141">Běžné datové služby</span><span class="sxs-lookup"><span data-stu-id="a1b2d-141">Common Data Service</span></span>](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||<span data-ttu-id="a1b2d-142">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-142">x</span></span>|<span data-ttu-id="a1b2d-143">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-143">x</span></span>
|[<span data-ttu-id="a1b2d-144">Salesforce</span><span class="sxs-lookup"><span data-stu-id="a1b2d-144">Salesforce</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||<span data-ttu-id="a1b2d-145">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-145">x</span></span>|<span data-ttu-id="a1b2d-146">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-146">x</span></span>
|[<span data-ttu-id="a1b2d-147">SharePoint</span><span class="sxs-lookup"><span data-stu-id="a1b2d-147">SharePoint</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||<span data-ttu-id="a1b2d-148">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-148">x</span></span>|<span data-ttu-id="a1b2d-149">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-149">x</span></span>
|[<span data-ttu-id="a1b2d-150">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a1b2d-150">SQL Server</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||<span data-ttu-id="a1b2d-151">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-151">x</span></span>|<span data-ttu-id="a1b2d-152">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-152">x</span></span>
|[<span data-ttu-id="a1b2d-153">Teradata</span><span class="sxs-lookup"><span data-stu-id="a1b2d-153">Teradata</span></span>](http://www.teradata.com/products-and-services/azure/products/)||<span data-ttu-id="a1b2d-154">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-154">x</span></span>|<span data-ttu-id="a1b2d-155">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-155">x</span></span>
|<span data-ttu-id="a1b2d-156">UserVoice</span><span class="sxs-lookup"><span data-stu-id="a1b2d-156">UserVoice</span></span>||<span data-ttu-id="a1b2d-157">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-157">x</span></span>|<span data-ttu-id="a1b2d-158">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-158">x</span></span>
|<span data-ttu-id="a1b2d-159">Zendesk</span><span class="sxs-lookup"><span data-stu-id="a1b2d-159">Zendesk</span></span>||<span data-ttu-id="a1b2d-160">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-160">x</span></span>|<span data-ttu-id="a1b2d-161">x</span><span class="sxs-lookup"><span data-stu-id="a1b2d-161">x</span></span>


> [!NOTE]
> <span data-ttu-id="a1b2d-162">Externí připojení tabulky lze použít také v [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="a1b2d-162">External Table connections can also be used in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>

### <a name="creating-an-api-connection-step-by-step"></a><span data-ttu-id="a1b2d-163">Vytvoření připojení k rozhraní API: krok za krokem</span><span class="sxs-lookup"><span data-stu-id="a1b2d-163">Creating an API connection: step by step</span></span>

1. <span data-ttu-id="a1b2d-164">Vytvoření funkce > vlastní funkce ![vytvoření vlastní funkce](./media/functions-bindings-storage-table/create-custom-function.jpg)</span><span class="sxs-lookup"><span data-stu-id="a1b2d-164">Create a function > custom function ![Create a custom function](./media/functions-bindings-storage-table/create-custom-function.jpg)</span></span>
1. <span data-ttu-id="a1b2d-165">Scénář `Experimental`  >  `ExternalTable-CSharp` šablony > vytvořit novou `External Table connection` 
 ![vstupní šablonu vybrat tabulky](./media/functions-bindings-storage-table/create-template-table.jpg)</span><span class="sxs-lookup"><span data-stu-id="a1b2d-165">Scenario `Experimental` > `ExternalTable-CSharp` template > Create a new `External Table connection`
![Choose table input template](./media/functions-bindings-storage-table/create-template-table.jpg)</span></span>
1. <span data-ttu-id="a1b2d-166">Vyberte poskytovatele SaaS > vyberte nebo vytvořte připojení ![připojení SaaS konfigurace](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="a1b2d-166">Choose your SaaS provider > choose/create a connection ![Configure SaaS connection](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span></span>
1. <span data-ttu-id="a1b2d-167">Vyberte připojení k rozhraní API > vytvořit funkci hello ![vytvořit tabulku – funkce](./media/functions-bindings-storage-table/table-template-options.jpg)</span><span class="sxs-lookup"><span data-stu-id="a1b2d-167">Select your API connection > create hello function ![Create table function](./media/functions-bindings-storage-table/table-template-options.jpg)</span></span>
1. <span data-ttu-id="a1b2d-168">Vyberte`Integrate` > `External Table`</span><span class="sxs-lookup"><span data-stu-id="a1b2d-168">Select `Integrate` > `External Table`</span></span>
    1. <span data-ttu-id="a1b2d-169">Nakonfigurujte připojení toouse hello cílová tabulka.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-169">Configure hello connection toouse your target table.</span></span> <span data-ttu-id="a1b2d-170">Tato nastavení budou velmi mezi poskytovatelů SaaS.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-170">These settings will very between SaaS providers.</span></span> <span data-ttu-id="a1b2d-171">Jsou outline níže v [nastavení zdroje dat](#datasourcesettings)
![konfigurace tabulky](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="a1b2d-171">They are outline below in [data source settings](#datasourcesettings)
![Configure table](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span></span>

## <a name="usage"></a><span data-ttu-id="a1b2d-172">Využití</span><span class="sxs-lookup"><span data-stu-id="a1b2d-172">Usage</span></span>

<span data-ttu-id="a1b2d-173">Tento příklad se připojí tooa tabulku s názvem "Kontakt" s Id, FirstName a LastName sloupce.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-173">This example connects tooa table named "Contact" with Id, LastName, and FirstName columns.</span></span> <span data-ttu-id="a1b2d-174">Kód Hello uvádí hello entity kontaktů v tabulce hello a protokoly hello jména a příjmení.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-174">hello code lists hello Contact entities in hello table and logs hello first and last names.</span></span>

### <a name="bindings"></a><span data-ttu-id="a1b2d-175">Vazby</span><span class="sxs-lookup"><span data-stu-id="a1b2d-175">Bindings</span></span>
```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```
<span data-ttu-id="a1b2d-176">`entityId`musí být u vazeb tabulky prázdný.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-176">`entityId` must be empty for table bindings.</span></span>

<span data-ttu-id="a1b2d-177">`ConnectionAppSettingsKey`identifikuje hello nastavení aplikace, která ukládá hello API připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-177">`ConnectionAppSettingsKey` identifies hello app setting that stores hello API connection string.</span></span> <span data-ttu-id="a1b2d-178">Hello nastavení aplikace je vytvořena automaticky při přidání rozhraní API připojení v hello integrovat uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-178">hello app setting is created automatically when you add an API connection in hello integrate UI.</span></span>

<span data-ttu-id="a1b2d-179">Tabulkové konektor poskytuje datových sad a každé datové sady obsahuje tabulky.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-179">A tabular connector provides data sets, and each data set contains tables.</span></span> <span data-ttu-id="a1b2d-180">Název Hello hello výchozí datové sady je "default".</span><span class="sxs-lookup"><span data-stu-id="a1b2d-180">hello name of hello default data set is “default.”</span></span> <span data-ttu-id="a1b2d-181">Hello názvy pro datovou sadu a tabulky v různých zprostředkovatelů SaaS jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="a1b2d-181">hello titles for a dataset and a table in various SaaS providers are listed below:</span></span>

|<span data-ttu-id="a1b2d-182">konektor</span><span class="sxs-lookup"><span data-stu-id="a1b2d-182">Connector</span></span>|<span data-ttu-id="a1b2d-183">Datová sada</span><span class="sxs-lookup"><span data-stu-id="a1b2d-183">Dataset</span></span>|<span data-ttu-id="a1b2d-184">Table</span><span class="sxs-lookup"><span data-stu-id="a1b2d-184">Table</span></span>|
|:-----|:---|:---| 
|<span data-ttu-id="a1b2d-185">**SharePoint**</span><span class="sxs-lookup"><span data-stu-id="a1b2d-185">**SharePoint**</span></span>|<span data-ttu-id="a1b2d-186">Lokality</span><span class="sxs-lookup"><span data-stu-id="a1b2d-186">Site</span></span>|<span data-ttu-id="a1b2d-187">Seznam serveru SharePoint</span><span class="sxs-lookup"><span data-stu-id="a1b2d-187">SharePoint List</span></span>
|<span data-ttu-id="a1b2d-188">**SQL**</span><span class="sxs-lookup"><span data-stu-id="a1b2d-188">**SQL**</span></span>|<span data-ttu-id="a1b2d-189">Databáze</span><span class="sxs-lookup"><span data-stu-id="a1b2d-189">Database</span></span>|<span data-ttu-id="a1b2d-190">Table</span><span class="sxs-lookup"><span data-stu-id="a1b2d-190">Table</span></span> 
|<span data-ttu-id="a1b2d-191">**List Google**</span><span class="sxs-lookup"><span data-stu-id="a1b2d-191">**Google Sheet**</span></span>|<span data-ttu-id="a1b2d-192">Tabulky</span><span class="sxs-lookup"><span data-stu-id="a1b2d-192">Spreadsheet</span></span>|<span data-ttu-id="a1b2d-193">Listu</span><span class="sxs-lookup"><span data-stu-id="a1b2d-193">Worksheet</span></span> 
|<span data-ttu-id="a1b2d-194">**Excel**</span><span class="sxs-lookup"><span data-stu-id="a1b2d-194">**Excel**</span></span>|<span data-ttu-id="a1b2d-195">Soubor aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="a1b2d-195">Excel file</span></span>|<span data-ttu-id="a1b2d-196">List</span><span class="sxs-lookup"><span data-stu-id="a1b2d-196">Sheet</span></span> 

<!--
See hello language-specific sample that copies hello input file toohello output file.

* [C#](#incsharp)
* [Node.js](#innodejs)

-->
<a name="incsharp"></a>

### <a name="usage-in-c"></a><span data-ttu-id="a1b2d-197">Použití v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="a1b2d-197">Usage in C#</span></span> #

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound toohello incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in hello source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retreive table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

<span data-ttu-id="a1b2d-198"><!--
<a name="innodejs"></a>

### Usage in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```
-->
<a name="datasourcesettings"></a>
##Nastavení zdroje dat</span><span class="sxs-lookup"><span data-stu-id="a1b2d-198"><!--
<a name="innodejs"></a>

### Usage in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```
-->
<a name="datasourcesettings"></a>
## Data Source Settings</span></span>

### <a name="sql-server"></a><span data-ttu-id="a1b2d-199">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a1b2d-199">SQL Server</span></span>

<span data-ttu-id="a1b2d-200">Hello toocreate skriptu a naplnění tabulky Kontakt hello je nižší než.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-200">hello script toocreate and populate hello Contact table is below.</span></span> <span data-ttu-id="a1b2d-201">dataSetName je "default".</span><span class="sxs-lookup"><span data-stu-id="a1b2d-201">dataSetName is “default.”</span></span>

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets"></a><span data-ttu-id="a1b2d-202">Tabulky Google</span><span class="sxs-lookup"><span data-stu-id="a1b2d-202">Google Sheets</span></span>
<span data-ttu-id="a1b2d-203">V Google dokumentace, vytvořit tabulku s názvem list `Contact`.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-203">In Google Docs, create a spreadsheet with a worksheet named `Contact`.</span></span> <span data-ttu-id="a1b2d-204">konektor Hello nelze použít hello tabulky zobrazovaný název.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-204">hello connector cannot use hello spreadsheet display name.</span></span> <span data-ttu-id="a1b2d-205">interní název Hello (tučným písmem) musí toobe použít jako dataSetName, například: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  přidat názvy sloupců hello `Id`, `LastName`, `FirstName` toohello první řádek a potom na naplnění dat Další řádky.</span><span class="sxs-lookup"><span data-stu-id="a1b2d-205">hello internal name (in bold) needs toobe used as dataSetName, for example: `docs.google.com/spreadsheets/d/`**`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** Add hello column names `Id`, `LastName`, `FirstName` toohello first row, then populate data on subsequent rows.</span></span>

### <a name="salesforce"></a><span data-ttu-id="a1b2d-206">Salesforce</span><span class="sxs-lookup"><span data-stu-id="a1b2d-206">Salesforce</span></span>
<span data-ttu-id="a1b2d-207">dataSetName je "default".</span><span class="sxs-lookup"><span data-stu-id="a1b2d-207">dataSetName is “default.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1b2d-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a1b2d-208">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
