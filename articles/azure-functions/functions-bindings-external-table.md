---
title: "Externí tabulky funkcí vazby Azure (Preview) | Microsoft Docs"
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
ms.openlocfilehash: 716438e5ea490f6716999813112305499dbe61a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-external-table-binding-preview"></a><span data-ttu-id="a4c1e-103">Externí tabulky funkcí vazby Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="a4c1e-103">Azure Functions External Table binding (Preview)</span></span>
<span data-ttu-id="a4c1e-104">Tento článek ukazuje, jak k manipulaci s tabulková data v poskytovatelů SaaS (například služby Sharepoint, Dynamics) v rámci funkce s integrovanou vazby.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-104">This article shows how to manipulate tabular data on SaaS providers (e.g. Sharepoint, Dynamics) within your function with built-in bindings.</span></span> <span data-ttu-id="a4c1e-105">Azure Functions podporuje vstupní a výstupní vazby pro externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-105">Azure Functions supports input, and output bindings for external tables.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a><span data-ttu-id="a4c1e-106">Připojení rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a4c1e-106">API Connections</span></span>

<span data-ttu-id="a4c1e-107">Tabulka vazby využívat externí rozhraní API připojení k ověření pomocí poskytovatelů SaaS 3. stran.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-107">Table bindings leverage external API connections to authenticate with 3rd party SaaS providers.</span></span> 

<span data-ttu-id="a4c1e-108">Při přiřazování vazbu můžete buď vytvořit nové připojení rozhraní API, nebo použít stávající připojení k rozhraní API v rámci stejné skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a4c1e-108">When assigning a binding you can either create a new API connection or use an existing API connection within the same resource group</span></span>

### <a name="supported-api-connections-tables"></a><span data-ttu-id="a4c1e-109">Připojení podporované rozhraní API (tabulky) s</span><span class="sxs-lookup"><span data-stu-id="a4c1e-109">Supported API Connections (Table)s</span></span>

|<span data-ttu-id="a4c1e-110">konektor</span><span class="sxs-lookup"><span data-stu-id="a4c1e-110">Connector</span></span>|<span data-ttu-id="a4c1e-111">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="a4c1e-111">Trigger</span></span>|<span data-ttu-id="a4c1e-112">Vstup</span><span class="sxs-lookup"><span data-stu-id="a4c1e-112">Input</span></span>|<span data-ttu-id="a4c1e-113">Výstup</span><span class="sxs-lookup"><span data-stu-id="a4c1e-113">Output</span></span>|
|:-----|:---:|:---:|:---:|
|[<span data-ttu-id="a4c1e-114">DB2</span><span class="sxs-lookup"><span data-stu-id="a4c1e-114">DB2</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||<span data-ttu-id="a4c1e-115">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-115">x</span></span>|<span data-ttu-id="a4c1e-116">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-116">x</span></span>
|[<span data-ttu-id="a4c1e-117">Dynamics 365 pro operace</span><span class="sxs-lookup"><span data-stu-id="a4c1e-117">Dynamics 365 for Operations</span></span>](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||<span data-ttu-id="a4c1e-118">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-118">x</span></span>|<span data-ttu-id="a4c1e-119">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-119">x</span></span>
|[<span data-ttu-id="a4c1e-120">Dynamics 365</span><span class="sxs-lookup"><span data-stu-id="a4c1e-120">Dynamics 365</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="a4c1e-121">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-121">x</span></span>|<span data-ttu-id="a4c1e-122">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-122">x</span></span>
|[<span data-ttu-id="a4c1e-123">Dynamics NAV</span><span class="sxs-lookup"><span data-stu-id="a4c1e-123">Dynamics NAV</span></span>](https://msdn.microsoft.com/library/gg481835.aspx)||<span data-ttu-id="a4c1e-124">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-124">x</span></span>|<span data-ttu-id="a4c1e-125">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-125">x</span></span>
|[<span data-ttu-id="a4c1e-126">Tabulky Google</span><span class="sxs-lookup"><span data-stu-id="a4c1e-126">Google Sheets</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||<span data-ttu-id="a4c1e-127">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-127">x</span></span>|<span data-ttu-id="a4c1e-128">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-128">x</span></span>
|[<span data-ttu-id="a4c1e-129">Informix</span><span class="sxs-lookup"><span data-stu-id="a4c1e-129">Informix</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||<span data-ttu-id="a4c1e-130">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-130">x</span></span>|<span data-ttu-id="a4c1e-131">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-131">x</span></span>
|[<span data-ttu-id="a4c1e-132">Dynamics 365 pro Finance</span><span class="sxs-lookup"><span data-stu-id="a4c1e-132">Dynamics 365 for Financials</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="a4c1e-133">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-133">x</span></span>|<span data-ttu-id="a4c1e-134">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-134">x</span></span>
|[<span data-ttu-id="a4c1e-135">MySQL</span><span class="sxs-lookup"><span data-stu-id="a4c1e-135">MySQL</span></span>](https://docs.microsoft.com/azure/store-php-create-mysql-database)||<span data-ttu-id="a4c1e-136">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-136">x</span></span>|<span data-ttu-id="a4c1e-137">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-137">x</span></span>
|[<span data-ttu-id="a4c1e-138">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="a4c1e-138">Oracle Database</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||<span data-ttu-id="a4c1e-139">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-139">x</span></span>|<span data-ttu-id="a4c1e-140">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-140">x</span></span>
|[<span data-ttu-id="a4c1e-141">Běžné datové služby</span><span class="sxs-lookup"><span data-stu-id="a4c1e-141">Common Data Service</span></span>](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||<span data-ttu-id="a4c1e-142">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-142">x</span></span>|<span data-ttu-id="a4c1e-143">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-143">x</span></span>
|[<span data-ttu-id="a4c1e-144">Salesforce</span><span class="sxs-lookup"><span data-stu-id="a4c1e-144">Salesforce</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||<span data-ttu-id="a4c1e-145">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-145">x</span></span>|<span data-ttu-id="a4c1e-146">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-146">x</span></span>
|[<span data-ttu-id="a4c1e-147">SharePoint</span><span class="sxs-lookup"><span data-stu-id="a4c1e-147">SharePoint</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||<span data-ttu-id="a4c1e-148">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-148">x</span></span>|<span data-ttu-id="a4c1e-149">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-149">x</span></span>
|[<span data-ttu-id="a4c1e-150">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a4c1e-150">SQL Server</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||<span data-ttu-id="a4c1e-151">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-151">x</span></span>|<span data-ttu-id="a4c1e-152">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-152">x</span></span>
|[<span data-ttu-id="a4c1e-153">Teradata</span><span class="sxs-lookup"><span data-stu-id="a4c1e-153">Teradata</span></span>](http://www.teradata.com/products-and-services/azure/products/)||<span data-ttu-id="a4c1e-154">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-154">x</span></span>|<span data-ttu-id="a4c1e-155">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-155">x</span></span>
|<span data-ttu-id="a4c1e-156">UserVoice</span><span class="sxs-lookup"><span data-stu-id="a4c1e-156">UserVoice</span></span>||<span data-ttu-id="a4c1e-157">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-157">x</span></span>|<span data-ttu-id="a4c1e-158">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-158">x</span></span>
|<span data-ttu-id="a4c1e-159">Zendesk</span><span class="sxs-lookup"><span data-stu-id="a4c1e-159">Zendesk</span></span>||<span data-ttu-id="a4c1e-160">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-160">x</span></span>|<span data-ttu-id="a4c1e-161">x</span><span class="sxs-lookup"><span data-stu-id="a4c1e-161">x</span></span>


> [!NOTE]
> <span data-ttu-id="a4c1e-162">Externí připojení tabulky lze použít také v [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="a4c1e-162">External Table connections can also be used in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>

### <a name="creating-an-api-connection-step-by-step"></a><span data-ttu-id="a4c1e-163">Vytvoření připojení k rozhraní API: krok za krokem</span><span class="sxs-lookup"><span data-stu-id="a4c1e-163">Creating an API connection: step by step</span></span>

1. <span data-ttu-id="a4c1e-164">Vytvoření funkce > vlastní funkce ![vytvoření vlastní funkce](./media/functions-bindings-storage-table/create-custom-function.jpg)</span><span class="sxs-lookup"><span data-stu-id="a4c1e-164">Create a function > custom function ![Create a custom function](./media/functions-bindings-storage-table/create-custom-function.jpg)</span></span>
1. <span data-ttu-id="a4c1e-165">Scénář `Experimental`  >  `ExternalTable-CSharp` šablony > vytvořit novou `External Table connection` 
 ![vstupní šablonu vybrat tabulky](./media/functions-bindings-storage-table/create-template-table.jpg)</span><span class="sxs-lookup"><span data-stu-id="a4c1e-165">Scenario `Experimental` > `ExternalTable-CSharp` template > Create a new `External Table connection`
![Choose table input template](./media/functions-bindings-storage-table/create-template-table.jpg)</span></span>
1. <span data-ttu-id="a4c1e-166">Vyberte poskytovatele SaaS > vyberte nebo vytvořte připojení ![připojení SaaS konfigurace](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="a4c1e-166">Choose your SaaS provider > choose/create a connection ![Configure SaaS connection](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span></span>
1. <span data-ttu-id="a4c1e-167">Vyberte připojení k rozhraní API > vytvořit funkci ![vytvořit tabulku – funkce](./media/functions-bindings-storage-table/table-template-options.jpg)</span><span class="sxs-lookup"><span data-stu-id="a4c1e-167">Select your API connection > create the function ![Create table function](./media/functions-bindings-storage-table/table-template-options.jpg)</span></span>
1. <span data-ttu-id="a4c1e-168">Vyberte`Integrate` > `External Table`</span><span class="sxs-lookup"><span data-stu-id="a4c1e-168">Select `Integrate` > `External Table`</span></span>
    1. <span data-ttu-id="a4c1e-169">Nakonfigurujte připojení používat, cílová tabulka.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-169">Configure the connection to use your target table.</span></span> <span data-ttu-id="a4c1e-170">Tato nastavení budou velmi mezi poskytovatelů SaaS.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-170">These settings will very between SaaS providers.</span></span> <span data-ttu-id="a4c1e-171">Jsou outline níže v [nastavení zdroje dat](#datasourcesettings)
![konfigurace tabulky](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="a4c1e-171">They are outline below in [data source settings](#datasourcesettings)
![Configure table](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span></span>

## <a name="usage"></a><span data-ttu-id="a4c1e-172">Využití</span><span class="sxs-lookup"><span data-stu-id="a4c1e-172">Usage</span></span>

<span data-ttu-id="a4c1e-173">Tento příklad se připojí k tabulku s názvem "Kontakt" s Id, FirstName a LastName sloupce.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-173">This example connects to a table named "Contact" with Id, LastName, and FirstName columns.</span></span> <span data-ttu-id="a4c1e-174">Kód zobrazí entity kontaktů v tabulce a protokoly první a poslední názvy.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-174">The code lists the Contact entities in the table and logs the first and last names.</span></span>

### <a name="bindings"></a><span data-ttu-id="a4c1e-175">Vazby</span><span class="sxs-lookup"><span data-stu-id="a4c1e-175">Bindings</span></span>
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
<span data-ttu-id="a4c1e-176">`entityId`musí být u vazeb tabulky prázdný.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-176">`entityId` must be empty for table bindings.</span></span>

<span data-ttu-id="a4c1e-177">`ConnectionAppSettingsKey`Určuje nastavení aplikace, která ukládá připojovací řetězec rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-177">`ConnectionAppSettingsKey` identifies the app setting that stores the API connection string.</span></span> <span data-ttu-id="a4c1e-178">Nastavení aplikace je vytvořena automaticky při přidání připojení k rozhraní API v integrací uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-178">The app setting is created automatically when you add an API connection in the integrate UI.</span></span>

<span data-ttu-id="a4c1e-179">Tabulkové konektor poskytuje datových sad a každé datové sady obsahuje tabulky.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-179">A tabular connector provides data sets, and each data set contains tables.</span></span> <span data-ttu-id="a4c1e-180">Název výchozí sadu dat je "default".</span><span class="sxs-lookup"><span data-stu-id="a4c1e-180">The name of the default data set is “default.”</span></span> <span data-ttu-id="a4c1e-181">Názvy pro datovou sadu a tabulky v různých zprostředkovatelů SaaS jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="a4c1e-181">The titles for a dataset and a table in various SaaS providers are listed below:</span></span>

|<span data-ttu-id="a4c1e-182">konektor</span><span class="sxs-lookup"><span data-stu-id="a4c1e-182">Connector</span></span>|<span data-ttu-id="a4c1e-183">Datová sada</span><span class="sxs-lookup"><span data-stu-id="a4c1e-183">Dataset</span></span>|<span data-ttu-id="a4c1e-184">Table</span><span class="sxs-lookup"><span data-stu-id="a4c1e-184">Table</span></span>|
|:-----|:---|:---| 
|<span data-ttu-id="a4c1e-185">**SharePoint**</span><span class="sxs-lookup"><span data-stu-id="a4c1e-185">**SharePoint**</span></span>|<span data-ttu-id="a4c1e-186">Lokality</span><span class="sxs-lookup"><span data-stu-id="a4c1e-186">Site</span></span>|<span data-ttu-id="a4c1e-187">Seznam serveru SharePoint</span><span class="sxs-lookup"><span data-stu-id="a4c1e-187">SharePoint List</span></span>
|<span data-ttu-id="a4c1e-188">**SQL**</span><span class="sxs-lookup"><span data-stu-id="a4c1e-188">**SQL**</span></span>|<span data-ttu-id="a4c1e-189">Databáze</span><span class="sxs-lookup"><span data-stu-id="a4c1e-189">Database</span></span>|<span data-ttu-id="a4c1e-190">Table</span><span class="sxs-lookup"><span data-stu-id="a4c1e-190">Table</span></span> 
|<span data-ttu-id="a4c1e-191">**List Google**</span><span class="sxs-lookup"><span data-stu-id="a4c1e-191">**Google Sheet**</span></span>|<span data-ttu-id="a4c1e-192">Tabulky</span><span class="sxs-lookup"><span data-stu-id="a4c1e-192">Spreadsheet</span></span>|<span data-ttu-id="a4c1e-193">Listu</span><span class="sxs-lookup"><span data-stu-id="a4c1e-193">Worksheet</span></span> 
|<span data-ttu-id="a4c1e-194">**Excel**</span><span class="sxs-lookup"><span data-stu-id="a4c1e-194">**Excel**</span></span>|<span data-ttu-id="a4c1e-195">Soubor aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="a4c1e-195">Excel file</span></span>|<span data-ttu-id="a4c1e-196">List</span><span class="sxs-lookup"><span data-stu-id="a4c1e-196">Sheet</span></span> 

<!--
See the language-specific sample that copies the input file to the output file.

* [C#](#incsharp)
* [Node.js](#innodejs)

-->
<a name="incsharp"></a>

### <a name="usage-in-c"></a><span data-ttu-id="a4c1e-197">Použití v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="a4c1e-197">Usage in C#</span></span> #

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
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

<span data-ttu-id="a4c1e-198"><!--
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
##Nastavení zdroje dat</span><span class="sxs-lookup"><span data-stu-id="a4c1e-198"><!--
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

### <a name="sql-server"></a><span data-ttu-id="a4c1e-199">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a4c1e-199">SQL Server</span></span>

<span data-ttu-id="a4c1e-200">Skript, který chcete vytvořit a naplnit tabulky Kontakt je níže.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-200">The script to create and populate the Contact table is below.</span></span> <span data-ttu-id="a4c1e-201">dataSetName je "default".</span><span class="sxs-lookup"><span data-stu-id="a4c1e-201">dataSetName is “default.”</span></span>

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

### <a name="google-sheets"></a><span data-ttu-id="a4c1e-202">Tabulky Google</span><span class="sxs-lookup"><span data-stu-id="a4c1e-202">Google Sheets</span></span>
<span data-ttu-id="a4c1e-203">V Google dokumentace, vytvořit tabulku s názvem list `Contact`.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-203">In Google Docs, create a spreadsheet with a worksheet named `Contact`.</span></span> <span data-ttu-id="a4c1e-204">Zobrazovaný název tabulky nelze použít konektor.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-204">The connector cannot use the spreadsheet display name.</span></span> <span data-ttu-id="a4c1e-205">Interní název (tučným písmem) potřebám má být použit jako dataSetName, například: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  přidat názvy sloupců `Id`, `LastName`, `FirstName` na první řádek, pak naplnění dat na Další řádky.</span><span class="sxs-lookup"><span data-stu-id="a4c1e-205">The internal name (in bold) needs to be used as dataSetName, for example: `docs.google.com/spreadsheets/d/`**`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** Add the column names `Id`, `LastName`, `FirstName` to the first row, then populate data on subsequent rows.</span></span>

### <a name="salesforce"></a><span data-ttu-id="a4c1e-206">Salesforce</span><span class="sxs-lookup"><span data-stu-id="a4c1e-206">Salesforce</span></span>
<span data-ttu-id="a4c1e-207">dataSetName je "default".</span><span class="sxs-lookup"><span data-stu-id="a4c1e-207">dataSetName is “default.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4c1e-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a4c1e-208">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
