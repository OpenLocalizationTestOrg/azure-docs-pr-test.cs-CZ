---
title: "aaaUse hello REST API tooget začít s Data Lake Store | Microsoft Docs"
description: "Použití rozhraní REST API WebHDFS tooperform operací v Data Lake Store"
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
ms.openlocfilehash: 62fce8293dfee730a61f2a3d37fc138ce7c3afdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a><span data-ttu-id="11b9b-103">Začínáme s Azure Data Lake Store pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="11b9b-103">Get started with Azure Data Lake Store using REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="11b9b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="11b9b-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="11b9b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="11b9b-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="11b9b-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="11b9b-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="11b9b-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="11b9b-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="11b9b-108">REST API</span><span class="sxs-lookup"><span data-stu-id="11b9b-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="11b9b-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="11b9b-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="11b9b-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="11b9b-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="11b9b-111">Python</span><span class="sxs-lookup"><span data-stu-id="11b9b-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

<span data-ttu-id="11b9b-112">V tomto článku se dozvíte, jak toouse rozhraní REST API WebHDFS a rozhraní API REST Data Lake Store tooperform účtu správu, jakož i operací systému souborů v Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-112">In this article, you will learn how toouse WebHDFS REST APIs and Data Lake Store REST APIs tooperform account management as well as filesystem operations on Azure Data Lake Store.</span></span> <span data-ttu-id="11b9b-113">Azure Data Lake Store zpřístupňuje vlastní rozhraní REST API pro operace správy účtů.</span><span class="sxs-lookup"><span data-stu-id="11b9b-113">Azure Data Lake Store exposes its own REST APIs for account management operations.</span></span> <span data-ttu-id="11b9b-114">Data Lake Store je nicméně kompatibilní s ekosystémy HDFS a Hadoop, a proto podporuje použití rozhraní REST API WebHDFS pro operace systému souborů.</span><span class="sxs-lookup"><span data-stu-id="11b9b-114">However, because Data Lake Store is compatible with HDFS and Hadoop ecosystem, it supports using WebHDFS REST APIs for filesystem operations.</span></span>

> [!NOTE]
> <span data-ttu-id="11b9b-115">Podrobné informace o podporovaných hello REST API pro Data Lake Store najdete v tématu [Azure Data Lake Store REST referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/mt693424.aspx).</span><span class="sxs-lookup"><span data-stu-id="11b9b-115">For detailed information on hello REST API support for Data Lake Store, see [Azure Data Lake Store REST API Reference](https://msdn.microsoft.com/library/mt693424.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="11b9b-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="11b9b-116">Prerequisites</span></span>
* <span data-ttu-id="11b9b-117">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="11b9b-117">**An Azure subscription**.</span></span> <span data-ttu-id="11b9b-118">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="11b9b-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="11b9b-119">**Vytvoření aplikace Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="11b9b-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="11b9b-120">Hello Azure AD aplikace tooauthenticate hello Data Lake Store aplikaci můžete používat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11b9b-120">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="11b9b-121">Existují různé přístupy tooauthenticate s Azure AD, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**.</span><span class="sxs-lookup"><span data-stu-id="11b9b-121">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="11b9b-122">Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="11b9b-122">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>
* <span data-ttu-id="11b9b-123">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="11b9b-123">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="11b9b-124">Tento článek používá cURL toodemonstrate, jakým způsobem volá toomake REST API vůči účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-124">This article uses cURL toodemonstrate how toomake REST API calls against a Data Lake Store account.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="11b9b-125">Jak můžu ověřovat pomocí služby Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="11b9b-125">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="11b9b-126">Můžete použít dva přístupy tooauthenticate pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="11b9b-126">You can use two approaches tooauthenticate using Azure Active Directory.</span></span>

### <a name="end-user-authentication-interactive"></a><span data-ttu-id="11b9b-127">Ověření koncového uživatele (interaktivní)</span><span class="sxs-lookup"><span data-stu-id="11b9b-127">End-user authentication (interactive)</span></span>
<span data-ttu-id="11b9b-128">V tomto scénáři aplikace hello vyzve uživatele toolog hello v a všechny operace hello se provádějí v kontextu hello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="11b9b-128">In this scenario, hello application prompts hello user toolog in and all hello operations are performed in hello context of hello user.</span></span> <span data-ttu-id="11b9b-129">Proveďte následující kroky pro interaktivní ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="11b9b-129">Perform hello following steps for interactive authentication.</span></span>

1. <span data-ttu-id="11b9b-130">Prostřednictvím aplikace přesměrování toohello uživatele hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="11b9b-130">Through your application, redirect hello user toohello following URL:</span></span>
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > <span data-ttu-id="11b9b-131">\<REDIRECT-URI > musí toobe kódováním pro použití v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="11b9b-131">\<REDIRECT-URI> needs toobe encoded for use in a URL.</span></span> <span data-ttu-id="11b9b-132">Pro adresu https://localhost proto použijte zápis `https%3A%2F%2Flocalhost`).</span><span class="sxs-lookup"><span data-stu-id="11b9b-132">So, for https://localhost, use `https%3A%2F%2Flocalhost`)</span></span>
   > 
   > 
   
    <span data-ttu-id="11b9b-133">Za účelem hello tohoto kurzu můžete nahradit zástupné hodnoty hello v adrese URL hello výše a vložte jej do adresního řádku webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="11b9b-133">For hello purpose of this tutorial, you can replace hello placeholder values in hello URL above and paste it in a web browser's address bar.</span></span> <span data-ttu-id="11b9b-134">Bude přesměrované tooauthenticate pomocí přihlášení Azure.</span><span class="sxs-lookup"><span data-stu-id="11b9b-134">You will be redirected tooauthenticate using your Azure login.</span></span> <span data-ttu-id="11b9b-135">Jakmile úspěšně přihlásíte, hello odpovědi se zobrazí v adresním řádku prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="11b9b-135">Once you successfully log in, hello response is displayed in hello browser's address bar.</span></span> <span data-ttu-id="11b9b-136">odpověď Hello bude v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="11b9b-136">hello response will be in hello following format:</span></span>
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. <span data-ttu-id="11b9b-137">Zaznamenejte hello autorizační kód z odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="11b9b-137">Capture hello authorization code from hello response.</span></span> <span data-ttu-id="11b9b-138">V tomto kurzu můžete zkopírovat hello autorizační kód z panelu Adresa hello hello webového prohlížeče a předat jej v hello POST požadavek toohello koncovému bodu tokenu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="11b9b-138">For this tutorial, you can copy hello authorization code from hello address bar of hello web browser and pass it in hello POST request toohello token endpoint, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > <span data-ttu-id="11b9b-139">V takovém případě hello \<REDIRECT-URI > nemusí být zakódován.</span><span class="sxs-lookup"><span data-stu-id="11b9b-139">In this case, hello \<REDIRECT-URI> need not be encoded.</span></span>
   > 
   > 
3. <span data-ttu-id="11b9b-140">odpověď Hello je objekt JSON, který obsahuje přístupový token (například `"access_token": "<ACCESS_TOKEN>"`) a obnovovací token (například `"refresh_token": "<REFRESH_TOKEN>"`).</span><span class="sxs-lookup"><span data-stu-id="11b9b-140">hello response is a JSON object that contains an access token (e.g., `"access_token": "<ACCESS_TOKEN>"`) and a refresh token (e.g., `"refresh_token": "<REFRESH_TOKEN>"`).</span></span> <span data-ttu-id="11b9b-141">Vaše aplikace používá hello přístupový token při přístupu k Azure Data Lake Store a hello aktualizace tokenu tooget dalšího přístupového tokenu když vyprší platnost přístupového tokenu.</span><span class="sxs-lookup"><span data-stu-id="11b9b-141">Your application uses hello access token when accessing Azure Data Lake Store and hello refresh token tooget another access token when an access token expires.</span></span>
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. <span data-ttu-id="11b9b-142">Když vyprší platnost hello přístupový token, můžete požádat o nový přístupový token pomocí hello aktualizace tokenu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="11b9b-142">When hello access token expires, you can request a new access token using hello refresh token, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

<span data-ttu-id="11b9b-143">Další informace o interaktivním ověřování uživatelů najdete v tématu [Tok poskytování autorizačních kódů](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span><span class="sxs-lookup"><span data-stu-id="11b9b-143">For more information on interactive user authentication, see [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span></span>

### <a name="service-to-service-authentication-non-interactive"></a><span data-ttu-id="11b9b-144">Ověřování služba-služba (neinteraktivní)</span><span class="sxs-lookup"><span data-stu-id="11b9b-144">Service-to-service authentication (non-interactive)</span></span>
<span data-ttu-id="11b9b-145">V tomto scénáři hello hello aplikace poskytuje svoje vlastní přihlašovací údaje tooperform hello operace.</span><span class="sxs-lookup"><span data-stu-id="11b9b-145">In this scenario, hello hello application provides its own credentials tooperform hello operations.</span></span> <span data-ttu-id="11b9b-146">V takovém případě musíte vydat požadavek POST jako hello níže.</span><span class="sxs-lookup"><span data-stu-id="11b9b-146">For this, you must issue a POST request like hello one shown below.</span></span> 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

<span data-ttu-id="11b9b-147">Hello výstup tohoto požadavku bude obsahovat autorizační token (odlišené `access-token` ve výstupu hello níže), potom budete předávat s voláními rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="11b9b-147">hello output of this request will include an authorization token (denoted by `access-token` in hello output below) that you will subsequently pass with your REST API calls.</span></span> <span data-ttu-id="11b9b-148">Tento ověřovací token si uložte do textového souboru, protože ho budete později v tomto článku potřebovat.</span><span class="sxs-lookup"><span data-stu-id="11b9b-148">Save this authentication token in a text file; you will need this later in this article.</span></span>

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

<span data-ttu-id="11b9b-149">Tento článek používá hello **neinteraktivní** přístup.</span><span class="sxs-lookup"><span data-stu-id="11b9b-149">This article uses hello **non-interactive** approach.</span></span> <span data-ttu-id="11b9b-150">Další informace o neinteraktivním (volání služba služba) najdete v tématu [služba pomocí přihlašovacích údajů volání tooservice](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span><span class="sxs-lookup"><span data-stu-id="11b9b-150">For more information on non-interactive (service-to-service calls), see [Service tooservice calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span></span>

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="11b9b-151">Vytvoření účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11b9b-151">Create a Data Lake Store account</span></span>
<span data-ttu-id="11b9b-152">Tato operace je založená na volání rozhraní REST API hello definované [zde](https://msdn.microsoft.com/library/mt694078.aspx).</span><span class="sxs-lookup"><span data-stu-id="11b9b-152">This operation is based on hello REST API call defined [here](https://msdn.microsoft.com/library/mt694078.aspx).</span></span>

<span data-ttu-id="11b9b-153">Použijte následující příkaz cURL hello.</span><span class="sxs-lookup"><span data-stu-id="11b9b-153">Use hello following cURL command.</span></span> <span data-ttu-id="11b9b-154">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-154">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

<span data-ttu-id="11b9b-155">V hello výše příkazu nahraďte položku \< `REDACTED` \> s tokenem autorizace hello jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="11b9b-155">In hello above command, replace \<`REDACTED`\> with hello authorization token you retrieved earlier.</span></span> <span data-ttu-id="11b9b-156">Hello datová část požadavku tohoto příkazu se nachází v hello **input.json** soubor, který je k dispozici pro hello `-d` parametr výše.</span><span class="sxs-lookup"><span data-stu-id="11b9b-156">hello request payload for this command is contained in hello **input.json** file that is provided for hello `-d` parameter above.</span></span> <span data-ttu-id="11b9b-157">Hello obsah souboru Input.JSON vypadá hello vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="11b9b-157">hello contents of hello input.json file resemble hello following:</span></span>

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a><span data-ttu-id="11b9b-158">Vytváření složek v účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11b9b-158">Create folders in a Data Lake Store account</span></span>
<span data-ttu-id="11b9b-159">Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="11b9b-159">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span></span>

<span data-ttu-id="11b9b-160">Použijte následující příkaz cURL hello.</span><span class="sxs-lookup"><span data-stu-id="11b9b-160">Use hello following cURL command.</span></span> <span data-ttu-id="11b9b-161">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-161">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

<span data-ttu-id="11b9b-162">V hello výše příkazu nahraďte položku \< `REDACTED` \> s tokenem autorizace hello jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="11b9b-162">In hello above command, replace \<`REDACTED`\> with hello authorization token you retrieved earlier.</span></span> <span data-ttu-id="11b9b-163">Tento příkaz vytvoří adresář s názvem **mytempdir** hello kořenové složce účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-163">This command creates a directory called **mytempdir** under hello root folder of your Data Lake Store account.</span></span>

<span data-ttu-id="11b9b-164">Pokud po úspěšném dokončení operace hello se zobrazí odpověď takto:</span><span class="sxs-lookup"><span data-stu-id="11b9b-164">You should see a response like this if hello operation completes successfully:</span></span>

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a><span data-ttu-id="11b9b-165">Zobrazení seznamu složek v účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11b9b-165">List folders in a Data Lake Store account</span></span>
<span data-ttu-id="11b9b-166">Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="11b9b-166">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span></span>

<span data-ttu-id="11b9b-167">Použijte následující příkaz cURL hello.</span><span class="sxs-lookup"><span data-stu-id="11b9b-167">Use hello following cURL command.</span></span> <span data-ttu-id="11b9b-168">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-168">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

<span data-ttu-id="11b9b-169">V hello výše příkazu nahraďte položku \< `REDACTED` \> s tokenem autorizace hello jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="11b9b-169">In hello above command, replace \<`REDACTED`\> with hello authorization token you retrieved earlier.</span></span>

<span data-ttu-id="11b9b-170">Pokud po úspěšném dokončení operace hello se zobrazí odpověď takto:</span><span class="sxs-lookup"><span data-stu-id="11b9b-170">You should see a response like this if hello operation completes successfully:</span></span>

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

## <a name="upload-data-into-a-data-lake-store-account"></a><span data-ttu-id="11b9b-171">Nahrání dat do účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11b9b-171">Upload data into a Data Lake Store account</span></span>
<span data-ttu-id="11b9b-172">Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span><span class="sxs-lookup"><span data-stu-id="11b9b-172">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span></span>

<span data-ttu-id="11b9b-173">Použijte následující příkaz cURL hello.</span><span class="sxs-lookup"><span data-stu-id="11b9b-173">Use hello following cURL command.</span></span> <span data-ttu-id="11b9b-174">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-174">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

<span data-ttu-id="11b9b-175">V hello výše syntaxe **-T** parametr je hello umístění souboru hello nahrávání.</span><span class="sxs-lookup"><span data-stu-id="11b9b-175">In hello above syntax **-T** parameter is hello location of hello file you are uploading.</span></span>

<span data-ttu-id="11b9b-176">výstup Hello je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="11b9b-176">hello output is similar toohello following:</span></span>
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a><span data-ttu-id="11b9b-177">Čtení dat z účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11b9b-177">Read data from a Data Lake Store account</span></span>
<span data-ttu-id="11b9b-178">Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span><span class="sxs-lookup"><span data-stu-id="11b9b-178">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span></span>

<span data-ttu-id="11b9b-179">Čtení dat z účtu Data Lake Store je proces, který obsahuje dva kroky.</span><span class="sxs-lookup"><span data-stu-id="11b9b-179">Reading data from a Data Lake Store account is a two-step process.</span></span>

* <span data-ttu-id="11b9b-180">Nejdřív odešlete požadavek GET na koncový bod hello `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span><span class="sxs-lookup"><span data-stu-id="11b9b-180">You first submit a GET request against hello endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span></span> <span data-ttu-id="11b9b-181">Tato možnost vrátí umístění toosubmit hello další požadavek GET na.</span><span class="sxs-lookup"><span data-stu-id="11b9b-181">This will return a location toosubmit hello next GET request to.</span></span>
* <span data-ttu-id="11b9b-182">Potom odešlete požadavek GET hello koncový bod hello `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span><span class="sxs-lookup"><span data-stu-id="11b9b-182">You then submit hello GET request against hello endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span></span> <span data-ttu-id="11b9b-183">Tato akce zobrazí hello obsah souboru hello.</span><span class="sxs-lookup"><span data-stu-id="11b9b-183">This will display hello contents of hello file.</span></span>

<span data-ttu-id="11b9b-184">Ale vzhledem k tomu, že není žádný rozdíl v hello vstupní parametry mezi hello nejprve a hello druhé krok, můžete použít hello `-L` parametr toosubmit hello prvního požadavku.</span><span class="sxs-lookup"><span data-stu-id="11b9b-184">However, because there is no difference in hello input parameters between hello first and hello second step, you can use hello `-L` parameter toosubmit hello first request.</span></span> <span data-ttu-id="11b9b-185">`-L`možnost v podstatě kombinuje dva požadavky do jednoho a bude cURL zopakuje požadavek hello na nové umístění hello.</span><span class="sxs-lookup"><span data-stu-id="11b9b-185">`-L` option essentially combines two requests into one and will make cURL redo hello request on hello new location.</span></span> <span data-ttu-id="11b9b-186">Nakonec hello výstup ze všech volání požadavků hello se zobrazuje, jako vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="11b9b-186">Finally, hello output from all hello request calls is displayed, like shown below.</span></span> <span data-ttu-id="11b9b-187">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-187">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

<span data-ttu-id="11b9b-188">Měli byste vidět výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="11b9b-188">You should see an output similar toohello following:</span></span>

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a><span data-ttu-id="11b9b-189">Přejmenování souboru v účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11b9b-189">Rename a file in a Data Lake Store account</span></span>
<span data-ttu-id="11b9b-190">Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="11b9b-190">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span></span>

<span data-ttu-id="11b9b-191">Použijte následující hello cURL toorename příkaz do souboru.</span><span class="sxs-lookup"><span data-stu-id="11b9b-191">Use hello following cURL command toorename a file.</span></span> <span data-ttu-id="11b9b-192">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-192">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

<span data-ttu-id="11b9b-193">Měli byste vidět výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="11b9b-193">You should see an output similar toohello following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a><span data-ttu-id="11b9b-194">Odstranění souboru z účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11b9b-194">Delete a file from a Data Lake Store account</span></span>
<span data-ttu-id="11b9b-195">Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="11b9b-195">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span></span>

<span data-ttu-id="11b9b-196">Použijte následující hello cURL toodelete příkaz do souboru.</span><span class="sxs-lookup"><span data-stu-id="11b9b-196">Use hello following cURL command toodelete a file.</span></span> <span data-ttu-id="11b9b-197">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-197">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

<span data-ttu-id="11b9b-198">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="11b9b-198">You should see an output like hello following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a><span data-ttu-id="11b9b-199">Odstranění účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11b9b-199">Delete a Data Lake Store account</span></span>
<span data-ttu-id="11b9b-200">Tato operace je založená na volání rozhraní REST API hello definované [zde](https://msdn.microsoft.com/library/mt694075.aspx).</span><span class="sxs-lookup"><span data-stu-id="11b9b-200">This operation is based on hello REST API call defined [here](https://msdn.microsoft.com/library/mt694075.aspx).</span></span>

<span data-ttu-id="11b9b-201">Použijte následující hello cURL příkaz toodelete účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-201">Use hello following cURL command toodelete a Data Lake Store account.</span></span> <span data-ttu-id="11b9b-202">Položku **\<yourstorename>** nahraďte názvem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11b9b-202">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

<span data-ttu-id="11b9b-203">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="11b9b-203">You should see an output like hello following:</span></span>

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a><span data-ttu-id="11b9b-204">Viz také</span><span class="sxs-lookup"><span data-stu-id="11b9b-204">See also</span></span>
* [<span data-ttu-id="11b9b-205">Aplikace typu Open Source pro velké objemy dat kompatibilní s Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="11b9b-205">Open Source Big Data applications compatible with Azure Data Lake Store</span></span>](data-lake-store-compatible-oss-other-applications.md)

