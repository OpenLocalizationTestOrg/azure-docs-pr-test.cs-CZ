---
title: "Správa Azure Data Lake Analytics pomocí sady Azure .NET SDK | Microsoft Docs"
description: "Naučte se spravovat úloh Data Lake Analytics, zdroje dat, uživatelé. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 811d172d-9873-4ce9-a6d5-c1a26b374c79
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: saveenr
ms.openlocfilehash: 0f8a95f96ce4c816dfb9132923faa9a9bf20c205
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a><span data-ttu-id="f3257-103">Správa Azure Data Lake Analytics pomocí sady Azure .NET SDK</span><span class="sxs-lookup"><span data-stu-id="f3257-103">Manage Azure Data Lake Analytics using Azure .NET SDK</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="f3257-104">Zjistěte, jak pro správu účtů Azure Data Lake Analytics, zdroje dat, uživatelů a úloh pomocí .NET SDK služby Azure.</span><span class="sxs-lookup"><span data-stu-id="f3257-104">Learn how to manage Azure Data Lake Analytics accounts, data sources, users, and jobs using the Azure .NET SDK.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="f3257-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f3257-105">Prerequisites</span></span>

* <span data-ttu-id="f3257-106">**Visual Studio 2015, Visual Studio 2013 Update 4 nebo Visual Studio 2012 s nainstalovaným Visual C++**.</span><span class="sxs-lookup"><span data-stu-id="f3257-106">**Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012 with Visual C++ Installed**.</span></span>
* <span data-ttu-id="f3257-107">**Sada Microsoft Azure SDK pro .NET verze 2.5 nebo vyšší**.</span><span class="sxs-lookup"><span data-stu-id="f3257-107">**Microsoft Azure SDK for .NET version 2.5 or above**.</span></span>  <span data-ttu-id="f3257-108">Nainstalujte ji pomocí [Instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="f3257-108">Install it using the [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="f3257-109">**Balíčky požadované NuGet**</span><span class="sxs-lookup"><span data-stu-id="f3257-109">**Required NuGet Packages**</span></span>

### <a name="install-nuget-packages"></a><span data-ttu-id="f3257-110">Instalace balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="f3257-110">Install NuGet packages</span></span>

|<span data-ttu-id="f3257-111">Balíček</span><span class="sxs-lookup"><span data-stu-id="f3257-111">Package</span></span>|<span data-ttu-id="f3257-112">Verze</span><span class="sxs-lookup"><span data-stu-id="f3257-112">Version</span></span>|
|-------|-------|
|[<span data-ttu-id="f3257-113">Microsoft.Rest.ClientRuntime.Azure.Authentication</span><span class="sxs-lookup"><span data-stu-id="f3257-113">Microsoft.Rest.ClientRuntime.Azure.Authentication</span></span>](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication)| <span data-ttu-id="f3257-114">2.3.1</span><span class="sxs-lookup"><span data-stu-id="f3257-114">2.3.1</span></span>|
|[<span data-ttu-id="f3257-115">Microsoft.Azure.Management.DataLake.Analytics</span><span class="sxs-lookup"><span data-stu-id="f3257-115">Microsoft.Azure.Management.DataLake.Analytics</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics)|<span data-ttu-id="f3257-116">3.0.0</span><span class="sxs-lookup"><span data-stu-id="f3257-116">3.0.0</span></span>|
|[<span data-ttu-id="f3257-117">Microsoft.Azure.Management.DataLake.Store</span><span class="sxs-lookup"><span data-stu-id="f3257-117">Microsoft.Azure.Management.DataLake.Store</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store)|<span data-ttu-id="f3257-118">2.2.0</span><span class="sxs-lookup"><span data-stu-id="f3257-118">2.2.0</span></span>|
|[<span data-ttu-id="f3257-119">Microsoft.Azure.Management.ResourceManager</span><span class="sxs-lookup"><span data-stu-id="f3257-119">Microsoft.Azure.Management.ResourceManager</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|<span data-ttu-id="f3257-120">1.6.0-Preview</span><span class="sxs-lookup"><span data-stu-id="f3257-120">1.6.0-preview</span></span>|
|[<span data-ttu-id="f3257-121">Microsoft.Azure.Graph.RBAC</span><span class="sxs-lookup"><span data-stu-id="f3257-121">Microsoft.Azure.Graph.RBAC</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|<span data-ttu-id="f3257-122">3.4.0-Preview</span><span class="sxs-lookup"><span data-stu-id="f3257-122">3.4.0-preview</span></span>|

<span data-ttu-id="f3257-123">Tyto balíčky NuGet příkazového řádku můžete nainstalovat pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="f3257-123">You can install these packages via the NuGet command line with the following commands:</span></span>

```
Install-Package -Id Microsoft.Rest.ClientRuntime.Azure.Authentication  -Version 2.3.1
Install-Package -Id Microsoft.Azure.Management.DataLake.Analytics  -Version 3.0.0
Install-Package -Id Microsoft.Azure.Management.DataLake.Store  -Version 2.2.0
Install-Package -Id Microsoft.Azure.Management.ResourceManager  -Version 1.6.0-preview
Install-Package -Id Microsoft.Azure.Graph.RBAC -Version 3.4.0-preview
```

## <a name="common-variables"></a><span data-ttu-id="f3257-124">Běžné proměnné</span><span class="sxs-lookup"><span data-stu-id="f3257-124">Common variables</span></span>

``` csharp
string subid = "<Subscription ID>"; // Subscription ID (a GUID)
string tenantid = "<Tenant ID>"; // AAD tenant ID or domain. For example, "contoso.onmicrosoft.com"
string rg == "<value>"; // Resource  group name
string clientid = "1950a258-227b-4e31-a9cf-717495945fc2"; // Sample client ID (this will work, but you should pick your own)
```

## <a name="authentication"></a><span data-ttu-id="f3257-125">Authentication</span><span class="sxs-lookup"><span data-stu-id="f3257-125">Authentication</span></span>

<span data-ttu-id="f3257-126">Máte několik možností pro přihlášení k Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="f3257-126">You have multiple options for logging on to Azure Data Lake Analytics.</span></span> <span data-ttu-id="f3257-127">Následující fragment kódu ukazuje příklad ověření s interaktivním ověřování uživatelů s automaticky.</span><span class="sxs-lookup"><span data-stu-id="f3257-127">The following snippet shows an example of authentication with interactive user authentication with a pop-up.</span></span>

``` csharp
using System;
using System.IO;
using System.Threading;
using System.Security.Cryptography.X509Certificates;

using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.DataLake.Analytics;
using Microsoft.Azure.Management.DataLake.Analytics.Models;
using Microsoft.Azure.Management.DataLake.Store;
using Microsoft.Azure.Management.DataLake.Store.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Azure.Graph.RBAC;

public static Program
{
   public static string TENANT = "microsoft.onmicrosoft.com";
   public static string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
   public static System.Uri ARM_TOKEN_AUDIENCE = new System.Uri( @"https://management.core.windows.net/");
   public static System.Uri ADL_TOKEN_AUDIENCE = new System.Uri( @"https://datalake.azure.net/" );
   public static System.Uri GRAPH_TOKEN_AUDIENCE = new System.Uri( @"https://graph.windows.net/" );

   static void Main(string[] args)
   {
      string MY_DOCUMENTS= System.Environment.GetFolderPath( System.Environment.SpecialFolder.MyDocuments);
      string TOKEN_CACHE_PATH = System.IO.Path.Combine(MY_DOCUMENTS, "my.tokencache");

      var tokenCache = GetTokenCache(TOKEN_CACHE_PATH);
      var armCreds = GetCreds_User_Popup(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var adlCreds = GetCreds_User_Popup(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var graphCreds = GetCreds_User_Popup(TENANT, GRAPH_TOKEN_AUDIENCE, CLIENTID, tokenCache);
   }
}
```

<span data-ttu-id="f3257-128">Zdrojový kód pro **GetCreds_User_Popup** a kód pro další možnosti pro ověřování jsou popsané v [možnosti ověřování Data Lake Analytics .NET](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)</span><span class="sxs-lookup"><span data-stu-id="f3257-128">The source code for **GetCreds_User_Popup** and the code for other options for authentication are covered in [Data Lake Analytics .NET authentication options](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)</span></span>


## <a name="create-the-client-management-objects"></a><span data-ttu-id="f3257-129">Vytvoření klienta objekty pro správu</span><span class="sxs-lookup"><span data-stu-id="f3257-129">Create the client management objects</span></span>

``` csharp
var resourceManagementClient = new ResourceManagementClient(armCreds) { SubscriptionId = subid };

var adlaAccountClient = new DataLakeAnalyticsAccountManagementClient(armCreds);
adlaAccountClient.SubscriptionId = subid;

var adlsAccountClient = new DataLakeStoreAccountManagementClient(armCreds);
adlsAccountClient.SubscriptionId = subid;

var adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClient(adlCreds);
var adlaJobClient = new DataLakeAnalyticsJobManagementClient(adlCreds);

var adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(adlCreds);

var  graphClient = new GraphRbacManagementClient(graphCreds);
graphClient.TenantID = domain;

```

## <a name="manage-accounts"></a><span data-ttu-id="f3257-130">Správa účtů</span><span class="sxs-lookup"><span data-stu-id="f3257-130">Manage accounts</span></span>

### <a name="create-an-azure-resource-group"></a><span data-ttu-id="f3257-131">Vytvoření skupiny prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="f3257-131">Create an Azure Resource Group</span></span>

<span data-ttu-id="f3257-132">Pokud jste již žádný nevytvořili, musí mít skupiny prostředků Azure k vytvoření komponentů vaše Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="f3257-132">If you haven't already created one, you must have an Azure Resource Group to create your Data Lake Analytics components.</span></span> <span data-ttu-id="f3257-133">Budete potřebovat pověření pro ověření, ID předplatného a umístění.</span><span class="sxs-lookup"><span data-stu-id="f3257-133">You  need your authentication credentials, subscription ID, and a location.</span></span> <span data-ttu-id="f3257-134">Následující kód ukazuje, jak vytvořit skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="f3257-134">The following code shows how to create a resource group:</span></span>

``` csharp
var resourceGroup = new ResourceGroup { Location = location };
resourceManagementClient.ResourceGroups.CreateOrUpdate(groupName, rg);
```
<span data-ttu-id="f3257-135">Další informace najdete v tématu [skupiny prostředků Azure a Data Lake Analytics](#Azure-Resource-Groups-and-Data-Lake-Analytics).</span><span class="sxs-lookup"><span data-stu-id="f3257-135">For more information, see [Azure Resource Groups and Data Lake Analytics](#Azure-Resource-Groups-and-Data-Lake-Analytics).</span></span>

### <a name="create-a-data-lake-store-account"></a><span data-ttu-id="f3257-136">Vytvoření účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f3257-136">Create a Data Lake Store account</span></span>

<span data-ttu-id="f3257-137">Někdy ADLA účet vyžaduje účtu ADLS.</span><span class="sxs-lookup"><span data-stu-id="f3257-137">Ever ADLA account requires an ADLS account.</span></span> <span data-ttu-id="f3257-138">Pokud chcete používat nemáte, můžete vytvořit jeden následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f3257-138">If you don't already have one to use, you can create one with the following code:</span></span>

``` csharp
var new_adls_params = new DataLakeStoreAccount(location: _location);
adlsAccountClient.Account.Create(rg, adls, new_adls_params);
```

### <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="f3257-139">Vytvoření účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="f3257-139">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="f3257-140">Následující kód vytvoří účtu ADLS</span><span class="sxs-lookup"><span data-stu-id="f3257-140">The following code creates an ADLS account</span></span>

``` csharp
var new_adla_params = new DataLakeAnalyticsAccount()
{
   DefaultDataLakeStoreAccount = adls,
   Location = location
};

adlaClient.Account.Create(rg, adla, new_adla_params);
```

### <a name="list-data-lake-store-accounts"></a><span data-ttu-id="f3257-141">Účty seznamu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f3257-141">List Data Lake Store accounts</span></span>

``` csharp
var adlsAccounts = adlsAccountClient.Account.List().ToList();
foreach (var adls in adlsAccounts)
{
   Console.WriteLine($"ADLS: {0}", adls.Name);
}
```

### <a name="list-data-lake-analytics-accounts"></a><span data-ttu-id="f3257-142">Účty seznamu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="f3257-142">List Data Lake Analytics accounts</span></span>

``` csharp
var adlaAccounts = adlaClient.Account.List().ToList();

for (var adla in AdlaAccounts)
{
   Console.WriteLine($"ADLA: {0}, adla.Name");
}
```

### <a name="checking-if-an-account-exists"></a><span data-ttu-id="f3257-143">Kontrola, jestli účet existuje</span><span class="sxs-lookup"><span data-stu-id="f3257-143">Checking if an account exists</span></span>

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
```

### <a name="get-information-about-an-account"></a><span data-ttu-id="f3257-144">Získání informací o účtu</span><span class="sxs-lookup"><span data-stu-id="f3257-144">Get information about an account</span></span>

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
if (exists)
{
   var adla_accnt = adlaClient.Account.Get(rg, adla);
}
```

### <a name="delete-an-account"></a><span data-ttu-id="f3257-145">Odstranit účet</span><span class="sxs-lookup"><span data-stu-id="f3257-145">Delete an account</span></span>

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
   adlaClient.Account.Delete(rg, adla);
}
```

### <a name="get-the-default-data-lake-store-account"></a><span data-ttu-id="f3257-146">Získání výchozího účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f3257-146">Get the default Data Lake Store account</span></span>

<span data-ttu-id="f3257-147">Každý účet Data Lake Analytics vyžaduje výchozího účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f3257-147">Every Data Lake Analytics account requires a default Data Lake Store account.</span></span> <span data-ttu-id="f3257-148">Pomocí tohoto kódu k určení výchozí účet úložiště pro účet Analytics.</span><span class="sxs-lookup"><span data-stu-id="f3257-148">Use this code to determine the default Store account for an Analytics account.</span></span>

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
  var adla_accnt = adlaClient.Account.Get(rg, adla);
  string def_adls_account = adla_accnt.DefaultDataLakeStoreAccount;
}
```

## <a name="manage-data-sources"></a><span data-ttu-id="f3257-149">Správa zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="f3257-149">Manage data sources</span></span>

<span data-ttu-id="f3257-150">Data Lake Analytics teď podporuje následující zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="f3257-150">Data Lake Analytics currently supports the following data sources:</span></span>

* [<span data-ttu-id="f3257-151">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f3257-151">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="f3257-152">Účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="f3257-152">Azure Storage Account</span></span>](../storage/common/storage-introduction.md)

### <a name="link-to-an-azure-storage-account"></a><span data-ttu-id="f3257-153">Odkaz na účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="f3257-153">Link to an Azure Storage account</span></span>

<span data-ttu-id="f3257-154">Můžete vytvořit odkazy na účty Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f3257-154">You can create links to Azure Storage accounts.</span></span>

``` csharp
string storage_key = "xxxxxxxxxxxxxxxxxxxx";
string storage_account = "mystorageaccount";
var addParams = new AddStorageAccountParameters(storage_key);            
adlaClient.StorageAccounts.Add(rg, adla, storage_account, addParams);
```

### <a name="list-azure-storage-data-sources"></a><span data-ttu-id="f3257-155">Seznam zdrojů dat úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="f3257-155">List Azure Storage data sources</span></span>

``` csharp
var stg_accounts = adlaAccountClient.StorageAccounts.ListByAccount(rg, adla);

if (stg_accounts != null)
{
  foreach (var stg_account in stg_accounts)
  {
      Console.WriteLine($"Storage account: {0}", stg_account.Name);
  }
}
```

### <a name="list-data-lake-store-data-sources"></a><span data-ttu-id="f3257-156">Zdroje dat seznamu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f3257-156">List Data Lake Store data sources</span></span>

``` csharp
var adls_accounts = adlsClient.Account.List();

if (adls_accounts != null)
{
  foreach (var adls_accnt in adls_accounts)
  {
      Console.WriteLine($"ADLS account: {0}", adls_accnt.Name);
  }
}
```

### <a name="upload-and-download-folders-and-files"></a><span data-ttu-id="f3257-157">Nahrávání a stahování složek a souborů</span><span class="sxs-lookup"><span data-stu-id="f3257-157">Upload and download folders and files</span></span>
<span data-ttu-id="f3257-158">Objekt správy systému klienta služby Data Lake Store souborů můžete použít k odesílání a stahování jednotlivé soubory nebo složky z Azure do místního počítače, pomocí následujících metod:</span><span class="sxs-lookup"><span data-stu-id="f3257-158">You can use the Data Lake Store file system client management object to upload and download individual files or folders from Azure to your local computer, using the following methods:</span></span>

- <span data-ttu-id="f3257-159">UploadFolder</span><span class="sxs-lookup"><span data-stu-id="f3257-159">UploadFolder</span></span>
- <span data-ttu-id="f3257-160">UploadFile</span><span class="sxs-lookup"><span data-stu-id="f3257-160">UploadFile</span></span>
- <span data-ttu-id="f3257-161">DownloadFolder</span><span class="sxs-lookup"><span data-stu-id="f3257-161">DownloadFolder</span></span>
- <span data-ttu-id="f3257-162">DownloadFile</span><span class="sxs-lookup"><span data-stu-id="f3257-162">DownloadFile</span></span>

<span data-ttu-id="f3257-163">První parametr pro tyto metody je název účtu Data Lake Store, ve následuje parametry pro zdrojovou cestu a cílovou cestu.</span><span class="sxs-lookup"><span data-stu-id="f3257-163">The first parameter for these methods is the name of the Data Lake Store Account, followed by parameters for the source path and the destination path.</span></span>

<span data-ttu-id="f3257-164">Následující příklad ukazuje, jak stáhnout složku v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f3257-164">The following example shows how to download a folder in the Data Lake Store.</span></span>

``` csharp
adlsFileSystemClient.FileSystem.DownloadFolder(adls, sourcePath, destinationPath);
```

### <a name="create-a-file-in-a-data-lake-store-account"></a><span data-ttu-id="f3257-165">Vytvoření souboru v účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f3257-165">Create a file in a Data Lake Store account</span></span>

``` csharp
using (var memstream = new MemoryStream())
{
   using (var sw = new StreamWriter(memstream, UTF8Encoding.UTF8))
   {
      sw.WriteLine("Hello World");
      sw.Flush();

      adlsFileSystemClient.FileSystem.Create(adls, "/Samples/Output/randombytes.csv", memstream);
   }
}
```

### <a name="verify-azure-storage-account-paths"></a><span data-ttu-id="f3257-166">Ověření cesty účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="f3257-166">Verify Azure Storage account paths</span></span>
<span data-ttu-id="f3257-167">Následující kód ověří, jestli účet služby Azure Storage (storageAccntName) existuje v účtu Data Lake Analytics (analyticsAccountName), a Pokud kontejner (containerName) existuje v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f3257-167">The following code checks if an Azure Storage account (storageAccntName) exists in a Data Lake Analytics account (analyticsAccountName), and if a container (containerName) exists in the Azure Storage account.</span></span>

``` csharp
string storage_account = "mystorageaccount";
string storage_container = "mycontainer";
bool accountExists = adlaClient.Account.StorageAccountExists(rg, adla, storage_account));
bool containerExists = adlaClient.Account.StorageContainerExists(rg, adla, storage_account, storage_container));
```

## <a name="manage-catalog-and-jobs"></a><span data-ttu-id="f3257-168">Správa katalogu a úlohy</span><span class="sxs-lookup"><span data-stu-id="f3257-168">Manage catalog and jobs</span></span>
<span data-ttu-id="f3257-169">Objekt DataLakeAnalyticsCatalogManagementClient poskytuje metody pro správu databáze SQL, zadaná pro každý účet Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="f3257-169">The DataLakeAnalyticsCatalogManagementClient object provides methods for managing the SQL database provided for each Azure Data Lake Analytics account.</span></span> <span data-ttu-id="f3257-170">DataLakeAnalyticsJobManagementClient poskytuje metody k odeslání a spravovat úlohy spustit na databázi s skriptů U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f3257-170">The DataLakeAnalyticsJobManagementClient provides methods to submit and manage jobs run on the database with U-SQL scripts.</span></span>

### <a name="list-databases-and-schemas"></a><span data-ttu-id="f3257-171">Seznam databází a schémat.</span><span class="sxs-lookup"><span data-stu-id="f3257-171">List databases and schemas</span></span>
<span data-ttu-id="f3257-172">Mezi několik věcí, které můžete vytvořit seznam nejobvyklejší jsou databáze a jejich schématu.</span><span class="sxs-lookup"><span data-stu-id="f3257-172">Among the several things you can list, the most common are databases and their schema.</span></span> <span data-ttu-id="f3257-173">Následující kód získá kolekci databází a poté zobrazí schéma pro každou databázi.</span><span class="sxs-lookup"><span data-stu-id="f3257-173">The following code obtains a collection of databases, and then enumerates the schema for each database.</span></span>

``` csharp
var databases = adlaCatalogClient.Catalog.ListDatabases(adla);
foreach (var db in databases)
{
  Console.WriteLine($"Database: {db.Name}");
  Console.WriteLine(" - Schemas:");
  var schemas = adlaCatalogClient.Catalog.ListSchemas(adla, db.Name);
  foreach (var schm in schemas)
  {
      Console.WriteLine($"\t{schm.Name}");
  }
}
```

### <a name="list-table-columns"></a><span data-ttu-id="f3257-174">Seznam sloupců tabulky</span><span class="sxs-lookup"><span data-stu-id="f3257-174">List table columns</span></span>
<span data-ttu-id="f3257-175">Následující kód ukazuje, jak získat přístup k databázi pomocí katalogu Data Lake Analytics správy klienta seznam sloupců zadané tabulky.</span><span class="sxs-lookup"><span data-stu-id="f3257-175">The following code shows how to access the database with a Data Lake Analytics Catalog management client to list the columns in a specified table.</span></span>

``` csharp
var tbl = adlaCatalogClient.Catalog.GetTable(adla, "master", "dbo", "MyTableName");
IEnumerable<USqlTableColumn> columns = tbl.ColumnList;

foreach (USqlTableColumn utc in columns)
{
  string scriptPath = "/Samples/Scripts/SearchResults_Wikipedia_Script.txt";
  Stream scriptStrm = adlsFileSystemClient.FileSystem.Open(_adlsAccountName, scriptPath);
  string scriptTxt = string.Empty;
  using (StreamReader sr = new StreamReader(scriptStrm))
  {
      scriptTxt = sr.ReadToEnd();
  }

  var jobName = "SR_Wikipedia";
  var jobId = Guid.NewGuid();
  var properties = new USqlJobProperties(scriptTxt);
  var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
  var jobInfo = adlaJobClient.Job.Create(adla, jobId, parameters);
  Console.WriteLine($"Job {jobName} submitted.");

}
```

### <a name="list-failed-jobs"></a><span data-ttu-id="f3257-176">Seznam neúspěšných úloh</span><span class="sxs-lookup"><span data-stu-id="f3257-176">List failed jobs</span></span>
<span data-ttu-id="f3257-177">Následující kód uvádí informace o úlohách, které se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="f3257-177">The following code lists information about jobs that failed.</span></span>

``` csharp
var odq = new ODataQuery<JobInformation> { Filter = "result eq 'Failed'" };
var jobs = adlaJobClient.Job.List(adla, odq);
foreach (var j in jobs)
{
   Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
}
```

### <a name="list-pipelines"></a><span data-ttu-id="f3257-178">Seznam kanálů</span><span class="sxs-lookup"><span data-stu-id="f3257-178">List pipelines</span></span>
<span data-ttu-id="f3257-179">Následující kód uvádí informace o jednotlivých kanálu úlohy, odeslané do účtu.</span><span class="sxs-lookup"><span data-stu-id="f3257-179">The following code lists information about each pipeline of jobs submitted to the account.</span></span>

``` csharp
var pipelines = adlaJobClient.Pipeline.List(adla);
foreach (var p in pipelines)
{
   Console.WriteLine($"Pipeline: {p.Name}\t{p.PipelineId}\t{p.LastSubmitTime}");
}
```

### <a name="list-recurrences"></a><span data-ttu-id="f3257-180">Opakování seznamu</span><span class="sxs-lookup"><span data-stu-id="f3257-180">List recurrences</span></span>
<span data-ttu-id="f3257-181">Následující kód uvádí informace o každém opakování úlohy, odeslané do účtu.</span><span class="sxs-lookup"><span data-stu-id="f3257-181">The following code lists information about each recurrence of jobs submitted to the account.</span></span>

``` csharp
var recurrences = adlaJobClient.Recurrence.List(adla);
foreach (var r in recurrences)
{
   Console.WriteLine($"Recurrence: {r.Name}\t{r.RecurrenceId}\t{r.LastSubmitTime}");
}
```

## <a name="common-graph-scenarios"></a><span data-ttu-id="f3257-182">Běžné scénáře grafu</span><span class="sxs-lookup"><span data-stu-id="f3257-182">Common graph scenarios</span></span>

### <a name="look-up-user-in-the-aad-directory"></a><span data-ttu-id="f3257-183">Vyhledání uživatele v adresáři AAD</span><span class="sxs-lookup"><span data-stu-id="f3257-183">Look up user in the AAD directory</span></span>

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
```

### <a name="get-the-objectid-of-a-user-in-the-aad-directory"></a><span data-ttu-id="f3257-184">Získejte ObjectId uživatele v adresáři AAD</span><span class="sxs-lookup"><span data-stu-id="f3257-184">Get the ObjectId of a user in the AAD directory</span></span>

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
Console.WriteLine( userinfo.ObjectId )
```

## <a name="manage-compute-policies"></a><span data-ttu-id="f3257-185">Správa zásad výpočetní</span><span class="sxs-lookup"><span data-stu-id="f3257-185">Manage compute policies</span></span>
<span data-ttu-id="f3257-186">Objekt DataLakeAnalyticsAccountManagementClient poskytuje metody pro správu výpočetních zásady pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="f3257-186">The DataLakeAnalyticsAccountManagementClient object provides methods for managing the compute policies for a Data Lake Analytics account.</span></span>

### <a name="list-compute-policies"></a><span data-ttu-id="f3257-187">Seznam výpočetní zásad</span><span class="sxs-lookup"><span data-stu-id="f3257-187">List compute policies</span></span>
<span data-ttu-id="f3257-188">Následující kód načte seznam výpočetní zásad pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="f3257-188">The following code retrieves a list of compute policies for a Data Lake Analytics account.</span></span>

``` csharp
var policies = adlaAccountClient.ComputePolicies.ListByAccount(rg, adla);
foreach (var p in policies)
{
   Console.WriteLine($"Name: {p.Name}\tType: {p.ObjectType}\tMax AUs / job: {p.MaxDegreeOfParallelismPerJob}\tMin priority / job: {p.MinPriorityPerJob}");
}
```

### <a name="create-a-new-compute-policy"></a><span data-ttu-id="f3257-189">Vytvořit novou zásadu výpočetní</span><span class="sxs-lookup"><span data-stu-id="f3257-189">Create a new compute policy</span></span>
<span data-ttu-id="f3257-190">Následující kód vytvoří novou zásadu výpočetní pro účet Data Lake Analytics, nastavení maximální Austrálie dostupná pro zadaného uživatele na 50 a priority minimální úloh na 250.</span><span class="sxs-lookup"><span data-stu-id="f3257-190">The following code creates a new compute policy for a Data Lake Analytics account, setting the maximum AUs available to the specified user to 50, and the minimum job priority to 250.</span></span>

``` csharp
var userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde";
var newPolicyParams = new ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250);
adlaAccountClient.ComputePolicies.CreateOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams);
```

## <a name="next-steps"></a><span data-ttu-id="f3257-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3257-191">Next steps</span></span>
* [<span data-ttu-id="f3257-192">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="f3257-192">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="f3257-193">Správa Azure Data Lake Analytics pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f3257-193">Manage Azure Data Lake Analytics using Azure portal</span></span>](data-lake-analytics-manage-use-portal.md)
* [<span data-ttu-id="f3257-194">Monitorování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f3257-194">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
