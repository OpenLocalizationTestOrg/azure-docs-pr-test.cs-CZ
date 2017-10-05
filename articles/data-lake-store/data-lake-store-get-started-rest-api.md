---
title: "Začínáme s Data Lake Storem pomocí REST API | Dokumentace Microsoftu"
description: "Použití rozhraní REST API WebHDFS k provádění operací v Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: dc2c8f58e0a2faf1b00f4903148328a5141a8637
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a><span data-ttu-id="f8399-103">Začínáme s Azure Data Lake Store pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="f8399-103">Get started with Azure Data Lake Store using REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f8399-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f8399-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="f8399-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f8399-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="f8399-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="f8399-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="f8399-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="f8399-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="f8399-108">REST API</span><span class="sxs-lookup"><span data-stu-id="f8399-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="f8399-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f8399-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="f8399-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="f8399-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="f8399-111">Python</span><span class="sxs-lookup"><span data-stu-id="f8399-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

<span data-ttu-id="f8399-112">V tomto článku se naučíte používat rozhraní REST API WebHDFS a rozhraní REST API Data Lake Store k provádění správy účtů a operací systému souborů v Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-112">In this article, you will learn how to use WebHDFS REST APIs and Data Lake Store REST APIs to perform account management as well as filesystem operations on Azure Data Lake Store.</span></span> <span data-ttu-id="f8399-113">Azure Data Lake Store zpřístupňuje vlastní rozhraní REST API pro operace správy účtů.</span><span class="sxs-lookup"><span data-stu-id="f8399-113">Azure Data Lake Store exposes its own REST APIs for account management operations.</span></span> <span data-ttu-id="f8399-114">Data Lake Store je nicméně kompatibilní s ekosystémy HDFS a Hadoop, a proto podporuje použití rozhraní REST API WebHDFS pro operace systému souborů.</span><span class="sxs-lookup"><span data-stu-id="f8399-114">However, because Data Lake Store is compatible with HDFS and Hadoop ecosystem, it supports using WebHDFS REST APIs for filesystem operations.</span></span>

> [!NOTE]
> <span data-ttu-id="f8399-115">Podrobné informace týkající se podpory rozhraní REST API pro Data Lake Store najdete v tématu [Referenční informace týkající se rozhraní REST API Azure Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8399-115">For detailed information on the REST API support for Data Lake Store, see [Azure Data Lake Store REST API Reference](https://msdn.microsoft.com/library/mt693424.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="f8399-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f8399-116">Prerequisites</span></span>
* <span data-ttu-id="f8399-117">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="f8399-117">**An Azure subscription**.</span></span> <span data-ttu-id="f8399-118">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f8399-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="f8399-119">**Vytvoření aplikace Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f8399-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="f8399-120">Aplikaci Azure AD použijete k ověření aplikace Data Lake Store ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8399-120">You use the Azure AD application to authenticate the Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="f8399-121">Existují různé přístupy k ověřování ve službě Azure AD, jsou to **ověřování koncového uživatele** nebo **ověřování služba-služba**.</span><span class="sxs-lookup"><span data-stu-id="f8399-121">There are different approaches to authenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="f8399-122">Pokyny a další informace o ověřování najdete v tématu [Ověřování koncových uživatelů](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Ověřování služba-služba](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="f8399-122">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>
* <span data-ttu-id="f8399-123">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="f8399-123">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="f8399-124">Tento článek používá cURL k předvedení toho, jak provádět volání rozhraní REST API vůči účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-124">This article uses cURL to demonstrate how to make REST API calls against a Data Lake Store account.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="f8399-125">Jak můžu ověřovat pomocí služby Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f8399-125">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="f8399-126">Ověřování pomocí služby Azure Active Directory můžete provádět dvěma přístupy.</span><span class="sxs-lookup"><span data-stu-id="f8399-126">You can use two approaches to authenticate using Azure Active Directory.</span></span>

### <a name="end-user-authentication-interactive"></a><span data-ttu-id="f8399-127">Ověření koncového uživatele (interaktivní)</span><span class="sxs-lookup"><span data-stu-id="f8399-127">End-user authentication (interactive)</span></span>
<span data-ttu-id="f8399-128">V tomto scénáři aplikace vyzve uživatele k přihlášení a všechny operace se provádějí v kontextu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f8399-128">In this scenario, the application prompts the user to log in and all the operations are performed in the context of the user.</span></span> <span data-ttu-id="f8399-129">V případě interaktivního ověřování proveďte následující postup.</span><span class="sxs-lookup"><span data-stu-id="f8399-129">Perform the following steps for interactive authentication.</span></span>

1. <span data-ttu-id="f8399-130">Prostřednictvím aplikace přesměrujte uživatele na tuto adresu URL:</span><span class="sxs-lookup"><span data-stu-id="f8399-130">Through your application, redirect the user to the following URL:</span></span>
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > <span data-ttu-id="f8399-131">\<REDIRECT-URI> musí být zakódovaný, aby se dal použít jako adresa URL.</span><span class="sxs-lookup"><span data-stu-id="f8399-131">\<REDIRECT-URI> needs to be encoded for use in a URL.</span></span> <span data-ttu-id="f8399-132">Pro adresu https://localhost proto použijte zápis `https%3A%2F%2Flocalhost`).</span><span class="sxs-lookup"><span data-stu-id="f8399-132">So, for https://localhost, use `https%3A%2F%2Flocalhost`)</span></span>
   > 
   > 
   
    <span data-ttu-id="f8399-133">Pro účely tohoto kurzu můžete ve výše zobrazené adrese URL nahradit zástupné hodnoty a vložit ji do adresního řádku webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f8399-133">For the purpose of this tutorial, you can replace the placeholder values in the URL above and paste it in a web browser's address bar.</span></span> <span data-ttu-id="f8399-134">Budete přesměrováni na ověření pomocí přihlášení Azure.</span><span class="sxs-lookup"><span data-stu-id="f8399-134">You will be redirected to authenticate using your Azure login.</span></span> <span data-ttu-id="f8399-135">Po úspěšném přihlášení se zobrazí v adresním řádku prohlížeče odpověď.</span><span class="sxs-lookup"><span data-stu-id="f8399-135">Once you successfully log in, the response is displayed in the browser's address bar.</span></span> <span data-ttu-id="f8399-136">Odpověď bude mít tento formát:</span><span class="sxs-lookup"><span data-stu-id="f8399-136">The response will be in the following format:</span></span>
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. <span data-ttu-id="f8399-137">Zaznamenejte autorizační kód z odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f8399-137">Capture the authorization code from the response.</span></span> <span data-ttu-id="f8399-138">Pro účely tohoto kurzu můžete zkopírovat autorizační kód z adresního řádku webového prohlížeče a předat jej v požadavku POST koncovému bodu tokenu, jak vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="f8399-138">For this tutorial, you can copy the authorization code from the address bar of the web browser and pass it in the POST request to the token endpoint, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > <span data-ttu-id="f8399-139">V takovém případě nemusí být identifikátor \<REDIRECT-URI> zakódovaný.</span><span class="sxs-lookup"><span data-stu-id="f8399-139">In this case, the \<REDIRECT-URI> need not be encoded.</span></span>
   > 
   > 
3. <span data-ttu-id="f8399-140">Odpovědí je objekt JSON, který obsahuje přístupový token (např. `"access_token": "<ACCESS_TOKEN>"`) a obnovovací token (např. `"refresh_token": "<REFRESH_TOKEN>"`).</span><span class="sxs-lookup"><span data-stu-id="f8399-140">The response is a JSON object that contains an access token (e.g., `"access_token": "<ACCESS_TOKEN>"`) and a refresh token (e.g., `"refresh_token": "<REFRESH_TOKEN>"`).</span></span> <span data-ttu-id="f8399-141">Aplikace používá přístupový token při přístupu k Azure Data Lake Store, zatímco obnovovací token používá k získání dalšího přístupového tokenu, když přístupovému tokenu vyprší platnost.</span><span class="sxs-lookup"><span data-stu-id="f8399-141">Your application uses the access token when accessing Azure Data Lake Store and the refresh token to get another access token when an access token expires.</span></span>
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. <span data-ttu-id="f8399-142">Když vyprší platnost přístupového tokenu, můžete pomocí obnovovacího tokenu požádat o nový přístupový token, jak vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="f8399-142">When the access token expires, you can request a new access token using the refresh token, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

<span data-ttu-id="f8399-143">Další informace o interaktivním ověřování uživatelů najdete v tématu [Tok poskytování autorizačních kódů](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8399-143">For more information on interactive user authentication, see [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span></span>

### <a name="service-to-service-authentication-non-interactive"></a><span data-ttu-id="f8399-144">Ověřování služba-služba (neinteraktivní)</span><span class="sxs-lookup"><span data-stu-id="f8399-144">Service-to-service authentication (non-interactive)</span></span>
<span data-ttu-id="f8399-145">V tomto scénáři aplikace poskytuje svoje vlastní přihlašovací údaje k provedení operací.</span><span class="sxs-lookup"><span data-stu-id="f8399-145">In this scenario, the the application provides its own credentials to perform the operations.</span></span> <span data-ttu-id="f8399-146">V tomto případě musíte vydat požadavek POST podobný tomu, který vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="f8399-146">For this, you must issue a POST request like the one shown below.</span></span> 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

<span data-ttu-id="f8399-147">Výstup tohoto požadavku bude obsahovat autorizační token (níže ve výstupu označený jako `access-token`), který potom budete předávat s voláními rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="f8399-147">The output of this request will include an authorization token (denoted by `access-token` in the output below) that you will subsequently pass with your REST API calls.</span></span> <span data-ttu-id="f8399-148">Tento ověřovací token si uložte do textového souboru, protože ho budete později v tomto článku potřebovat.</span><span class="sxs-lookup"><span data-stu-id="f8399-148">Save this authentication token in a text file; you will need this later in this article.</span></span>

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

<span data-ttu-id="f8399-149">Tento článek používá **neinteraktivní** přístup.</span><span class="sxs-lookup"><span data-stu-id="f8399-149">This article uses the **non-interactive** approach.</span></span> <span data-ttu-id="f8399-150">Další informace o neinteraktivním přístupu (volání služba-služba) najdete v tématu [Volání služba-služba pomocí přihlašovacích údajů](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8399-150">For more information on non-interactive (service-to-service calls), see [Service to service calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span></span>

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="f8399-151">Vytvoření účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8399-151">Create a Data Lake Store account</span></span>
<span data-ttu-id="f8399-152">Tato operace je založená na volání rozhraní REST API, které je definované [tady](https://msdn.microsoft.com/library/mt694078.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8399-152">This operation is based on the REST API call defined [here](https://msdn.microsoft.com/library/mt694078.aspx).</span></span>

<span data-ttu-id="f8399-153">Použijte následující příkaz cURL.</span><span class="sxs-lookup"><span data-stu-id="f8399-153">Use the following cURL command.</span></span> <span data-ttu-id="f8399-154">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-154">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

<span data-ttu-id="f8399-155">Ve výše uvedeném příkazu nahraďte položku \<`REDACTED`\> autorizačním tokenem, který jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="f8399-155">In the above command, replace \<`REDACTED`\> with the authorization token you retrieved earlier.</span></span> <span data-ttu-id="f8399-156">Datová část požadavku tohoto příkazu se nachází v souboru **input.json** poskytnutém pro parametr `-d` výše.</span><span class="sxs-lookup"><span data-stu-id="f8399-156">The request payload for this command is contained in the **input.json** file that is provided for the `-d` parameter above.</span></span> <span data-ttu-id="f8399-157">Obsah souboru input.json vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="f8399-157">The contents of the input.json file resemble the following:</span></span>

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a><span data-ttu-id="f8399-158">Vytváření složek v účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8399-158">Create folders in a Data Lake Store account</span></span>
<span data-ttu-id="f8399-159">Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="f8399-159">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span></span>

<span data-ttu-id="f8399-160">Použijte následující příkaz cURL.</span><span class="sxs-lookup"><span data-stu-id="f8399-160">Use the following cURL command.</span></span> <span data-ttu-id="f8399-161">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-161">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

<span data-ttu-id="f8399-162">Ve výše uvedeném příkazu nahraďte položku \<`REDACTED`\> autorizačním tokenem, který jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="f8399-162">In the above command, replace \<`REDACTED`\> with the authorization token you retrieved earlier.</span></span> <span data-ttu-id="f8399-163">Tento příkaz vytvoří adresář s názvem **mytempdir** v kořenové složce účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-163">This command creates a directory called **mytempdir** under the root folder of your Data Lake Store account.</span></span>

<span data-ttu-id="f8399-164">Po úspěšném dokončení operace se zobrazí takováto odpověď:</span><span class="sxs-lookup"><span data-stu-id="f8399-164">You should see a response like this if the operation completes successfully:</span></span>

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a><span data-ttu-id="f8399-165">Zobrazení seznamu složek v účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8399-165">List folders in a Data Lake Store account</span></span>
<span data-ttu-id="f8399-166">Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="f8399-166">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span></span>

<span data-ttu-id="f8399-167">Použijte následující příkaz cURL.</span><span class="sxs-lookup"><span data-stu-id="f8399-167">Use the following cURL command.</span></span> <span data-ttu-id="f8399-168">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-168">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

<span data-ttu-id="f8399-169">Ve výše uvedeném příkazu nahraďte položku \<`REDACTED`\> autorizačním tokenem, který jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="f8399-169">In the above command, replace \<`REDACTED`\> with the authorization token you retrieved earlier.</span></span>

<span data-ttu-id="f8399-170">Po úspěšném dokončení operace se zobrazí takováto odpověď:</span><span class="sxs-lookup"><span data-stu-id="f8399-170">You should see a response like this if the operation completes successfully:</span></span>

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a><span data-ttu-id="f8399-171">Nahrání dat do účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8399-171">Upload data into a Data Lake Store account</span></span>
<span data-ttu-id="f8399-172">Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span><span class="sxs-lookup"><span data-stu-id="f8399-172">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span></span>

<span data-ttu-id="f8399-173">Použijte následující příkaz cURL.</span><span class="sxs-lookup"><span data-stu-id="f8399-173">Use the following cURL command.</span></span> <span data-ttu-id="f8399-174">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-174">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

<span data-ttu-id="f8399-175">Ve výše uvedené syntaxi je parametr **-T** umístění nahrávaného souboru.</span><span class="sxs-lookup"><span data-stu-id="f8399-175">In the above syntax **-T** parameter is the location of the file you are uploading.</span></span>

<span data-ttu-id="f8399-176">Výstup je podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="f8399-176">The output is similar to the following:</span></span>
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a><span data-ttu-id="f8399-177">Čtení dat z účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8399-177">Read data from a Data Lake Store account</span></span>
<span data-ttu-id="f8399-178">Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span><span class="sxs-lookup"><span data-stu-id="f8399-178">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span></span>

<span data-ttu-id="f8399-179">Čtení dat z účtu Data Lake Store je proces, který obsahuje dva kroky.</span><span class="sxs-lookup"><span data-stu-id="f8399-179">Reading data from a Data Lake Store account is a two-step process.</span></span>

* <span data-ttu-id="f8399-180">Nejdřív odešlete požadavek GET na koncový bod `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span><span class="sxs-lookup"><span data-stu-id="f8399-180">You first submit a GET request against the endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span></span> <span data-ttu-id="f8399-181">Vrátí se umístění, na které je nutné odeslat další požadavek GET.</span><span class="sxs-lookup"><span data-stu-id="f8399-181">This will return a location to submit the next GET request to.</span></span>
* <span data-ttu-id="f8399-182">Potom odešlete požadavek GET na koncový bod `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span><span class="sxs-lookup"><span data-stu-id="f8399-182">You then submit the GET request against the endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span></span> <span data-ttu-id="f8399-183">Tím se zobrazí obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="f8399-183">This will display the contents of the file.</span></span>

<span data-ttu-id="f8399-184">Vstupní parametry v prvním a druhém kroku se ale nijak neliší, a proto můžete parametr `-L` použít k odeslání prvního požadavku.</span><span class="sxs-lookup"><span data-stu-id="f8399-184">However, because there is no difference in the input parameters between the first and the second step, you can use the `-L` parameter to submit the first request.</span></span> <span data-ttu-id="f8399-185">Možnost `-L` v podstatě kombinuje dva požadavky do jednoho. Výsledkem je, že cURL zopakuje požadavek na novém umístění.</span><span class="sxs-lookup"><span data-stu-id="f8399-185">`-L` option essentially combines two requests into one and will make cURL redo the request on the new location.</span></span> <span data-ttu-id="f8399-186">Nakonec se zobrazí výstup ze všech volání požadavků, který bude vypadat přibližně tak, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="f8399-186">Finally, the output from all the request calls is displayed, like shown below.</span></span> <span data-ttu-id="f8399-187">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-187">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

<span data-ttu-id="f8399-188">Zobrazený výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="f8399-188">You should see an output similar to the following:</span></span>

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a><span data-ttu-id="f8399-189">Přejmenování souboru v účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8399-189">Rename a file in a Data Lake Store account</span></span>
<span data-ttu-id="f8399-190">Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="f8399-190">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span></span>

<span data-ttu-id="f8399-191">Pokud chcete přejmenovat soubor, použijte následující příkaz cURL.</span><span class="sxs-lookup"><span data-stu-id="f8399-191">Use the following cURL command to rename a file.</span></span> <span data-ttu-id="f8399-192">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-192">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

<span data-ttu-id="f8399-193">Zobrazený výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="f8399-193">You should see an output similar to the following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a><span data-ttu-id="f8399-194">Odstranění souboru z účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8399-194">Delete a file from a Data Lake Store account</span></span>
<span data-ttu-id="f8399-195">Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="f8399-195">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span></span>

<span data-ttu-id="f8399-196">Pokud chcete odstranit soubor, použijte následující příkaz cURL.</span><span class="sxs-lookup"><span data-stu-id="f8399-196">Use the following cURL command to delete a file.</span></span> <span data-ttu-id="f8399-197">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-197">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

<span data-ttu-id="f8399-198">Zobrazený výstup by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="f8399-198">You should see an output like the following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a><span data-ttu-id="f8399-199">Odstranění účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8399-199">Delete a Data Lake Store account</span></span>
<span data-ttu-id="f8399-200">Tato operace je založená na volání rozhraní REST API, které je definované [tady](https://msdn.microsoft.com/library/mt694075.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8399-200">This operation is based on the REST API call defined [here](https://msdn.microsoft.com/library/mt694075.aspx).</span></span>

<span data-ttu-id="f8399-201">Pokud chcete odstranit účet Data Lake Store, použijte následující příkaz cURL.</span><span class="sxs-lookup"><span data-stu-id="f8399-201">Use the following cURL command to delete a Data Lake Store account.</span></span> <span data-ttu-id="f8399-202">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f8399-202">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

<span data-ttu-id="f8399-203">Zobrazený výstup by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="f8399-203">You should see an output like the following:</span></span>

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a><span data-ttu-id="f8399-204">Viz také</span><span class="sxs-lookup"><span data-stu-id="f8399-204">See also</span></span>
* [<span data-ttu-id="f8399-205">Aplikace typu Open Source pro velké objemy dat kompatibilní s Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8399-205">Open Source Big Data applications compatible with Azure Data Lake Store</span></span>](data-lake-store-compatible-oss-other-applications.md)

