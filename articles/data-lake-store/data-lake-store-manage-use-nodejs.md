---
title: "aaaGet začít s Azure Data Lake Store pomocí sady Azure SDK pro Node.js | Microsoft Docs"
description: "Zjistěte, jak toouse toowork Node.js s účty Data Lake Store a hello systému souborů."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 2fee173c-69ae-4e1d-8773-48618cda9e16
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/06/2017
ms.author: nitinme
ms.openlocfilehash: ce36a2e0de4e091a4e85ed784a3381415ef6f9e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a><span data-ttu-id="9cdbe-103">Začínáme s Azure Data Lake Store pomocí sady Azure SDK pro Node.js</span><span class="sxs-lookup"><span data-stu-id="9cdbe-103">Get started with Azure Data Lake Store using Azure SDK for Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9cdbe-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9cdbe-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="9cdbe-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9cdbe-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="9cdbe-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="9cdbe-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="9cdbe-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="9cdbe-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="9cdbe-108">REST API</span><span class="sxs-lookup"><span data-stu-id="9cdbe-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="9cdbe-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9cdbe-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="9cdbe-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="9cdbe-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="9cdbe-111">Python</span><span class="sxs-lookup"><span data-stu-id="9cdbe-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

> [!NOTE]
> <span data-ttu-id="9cdbe-112">Pro odesílání a stahování velké množství dat (velkých souborů, velký počet souborů nebo obě), doporučujeme použít hello [Python SDK](data-lake-store-get-started-python.md), hello [.NET SDK](data-lake-store-get-started-net-sdk.md), [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md), nebo [prostředí Azure PowerShell](data-lake-store-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9cdbe-112">For uploading and downloading large amount of data (large files, a large number of files, or both), we recommend that you use hello [Python SDK](data-lake-store-get-started-python.md), hello [.NET SDK](data-lake-store-get-started-net-sdk.md), [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md), or [Azure PowerShell](data-lake-store-get-started-powershell.md).</span></span> <span data-ttu-id="9cdbe-113">Tyto možnosti mají lepší výkon, které používají více přesun dat hello tooparallelize vláken.</span><span class="sxs-lookup"><span data-stu-id="9cdbe-113">These options have better performance as they use multiple threads tooparallelize hello data movement.</span></span>
> 
> 

<span data-ttu-id="9cdbe-114">Zjistěte, jak toouse hello Azure SDK pro Node.js toocreate účtu Azure Data Lake Store a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake Store najdete v tématu [Přehled Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9cdbe-114">Learn how toouse hello Azure SDK for Node.js toocreate an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span> <span data-ttu-id="9cdbe-115">V současné době podporuje SDK hello</span><span class="sxs-lookup"><span data-stu-id="9cdbe-115">Currently, hello SDK supports</span></span>

* <span data-ttu-id="9cdbe-116">**Verze Node.js: 0.10.0 nebo vyšší**</span><span class="sxs-lookup"><span data-stu-id="9cdbe-116">**Node.js version: 0.10.0 or higher**</span></span>
* <span data-ttu-id="9cdbe-117">**Verze rozhraní REST API pro Účet: 2015-10-01-preview**</span><span class="sxs-lookup"><span data-stu-id="9cdbe-117">**REST API version for Account: 2015-10-01-preview**</span></span>
* <span data-ttu-id="9cdbe-118">**Verze rozhraní REST API pro systém souborů: 2015-10-01-preview**</span><span class="sxs-lookup"><span data-stu-id="9cdbe-118">**REST API version for FileSystem: 2015-10-01-preview**</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9cdbe-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9cdbe-119">Prerequisites</span></span>
<span data-ttu-id="9cdbe-120">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="9cdbe-120">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="9cdbe-121">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="9cdbe-121">**An Azure subscription**.</span></span> <span data-ttu-id="9cdbe-122">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9cdbe-122">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="9cdbe-123">**Vytvoření aplikace Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9cdbe-123">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="9cdbe-124">Hello Azure AD aplikace tooauthenticate hello Data Lake Store aplikaci můžete používat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9cdbe-124">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="9cdbe-125">Existují různé přístupy tooauthenticate s Azure AD, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**.</span><span class="sxs-lookup"><span data-stu-id="9cdbe-125">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="9cdbe-126">Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="9cdbe-126">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="how-tooinstall"></a><span data-ttu-id="9cdbe-127">Jak tooInstall</span><span class="sxs-lookup"><span data-stu-id="9cdbe-127">How tooInstall</span></span>
```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a><span data-ttu-id="9cdbe-128">Ověření pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9cdbe-128">Authenticate using Azure Active Directory</span></span>
<span data-ttu-id="9cdbe-129">Hello níže zobrazené fragmenty kódu ukazují dva samostatné způsoby ověřování s Data Lake Store pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9cdbe-129">hello snippets below show two separate ways of authenticating with Data Lake Store using Azure AD.</span></span> <span data-ttu-id="9cdbe-130">Podrobné informace o různých toouse metody pro ověřování s Data Lake Store, naleznete v části [ověřit s Data Lake Store pomocí Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="9cdbe-130">For a detailed discussion on various methods toouse for authentication with Data Lake Store, see [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

<span data-ttu-id="9cdbe-131">Následující fragment Hello také vyžaduje zadání jako název domény služby Azure AD, ID klienta pro aplikací Azure AD, atd. Tyto podrobnosti můžete načíst z aplikace Azure AD, která je nutné vytvořit, hello podrobnosti, které jsou součástí výše uvedený odkaz.</span><span class="sxs-lookup"><span data-stu-id="9cdbe-131">hello snippet below also requires inputs like Azure AD domain name, client ID for an Azure AD app, etc. All these details can be retrieved from an Azure AD application that you must created, hello details of which are also included in link above.</span></span>

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-hello-data-lake-store-clients"></a><span data-ttu-id="9cdbe-132">Vytvořit hello Data Lake Store klienti</span><span class="sxs-lookup"><span data-stu-id="9cdbe-132">Create hello Data Lake Store Clients</span></span>
```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="9cdbe-133">Vytvoření účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="9cdbe-133">Create a Data Lake Store Account</span></span>
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object toocreate
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference toohello actual request and response, so you can see what was sent and received on hello wire.
      hello structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'hello response body if any',
        request: reference tooa stripped version of http request
        response: reference tooa stripped version of hello response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a><span data-ttu-id="9cdbe-134">Vytvořte soubor s obsahem</span><span class="sxs-lookup"><span data-stu-id="9cdbe-134">Create a file with content</span></span>
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a><span data-ttu-id="9cdbe-135">Získat seznam souborů a složek</span><span class="sxs-lookup"><span data-stu-id="9cdbe-135">Get a list of files and folders</span></span>
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a><span data-ttu-id="9cdbe-136">Viz také</span><span class="sxs-lookup"><span data-stu-id="9cdbe-136">See also</span></span>
* [<span data-ttu-id="9cdbe-137">Microsoft Azure SDK pro Node.js</span><span class="sxs-lookup"><span data-stu-id="9cdbe-137">Microsoft Azure SDK for Node.js</span></span>](https://github.com/azure/azure-sdk-for-node)
* [<span data-ttu-id="9cdbe-138">Microsoft Azure SDK pro Node.js – Správa Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="9cdbe-138">Microsoft Azure SDK for Node.js - Data Lake Analytics Management</span></span>](https://www.npmjs.com/package/azure-arm-datalake-analytics)

