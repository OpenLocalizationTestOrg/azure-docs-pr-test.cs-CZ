---
title: "aaaGet začít s Data Lake Analytics pomocí rozhraní REST API | Microsoft Docs"
description: "Použití rozhraní REST API WebHDFS tooperform operací v Data Lake Analytics"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 5e133d92-baaa-44c9-890c-ab2d85c91122
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: jgao
ms.openlocfilehash: a0b13d521821fd2d74716cc52485585feb7c51b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a><span data-ttu-id="0ca03-103">Začínáme se službou Azure Data Lake Analytics pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="0ca03-103">Get started with Azure Data Lake Analytics using REST APIs</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="0ca03-104">Zjistěte, jak toouse rozhraní REST API WebHDFS a rozhraní API REST Data Lake Analytics toomanage Data Lake Analytics účty, úlohy a katalogu.</span><span class="sxs-lookup"><span data-stu-id="0ca03-104">Learn how toouse WebHDFS REST APIs and Data Lake Analytics REST APIs toomanage Data Lake Analytics accounts, jobs, and catalog.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="0ca03-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0ca03-105">Prerequisites</span></span>
* <span data-ttu-id="0ca03-106">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="0ca03-106">**An Azure subscription**.</span></span> <span data-ttu-id="0ca03-107">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0ca03-107">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="0ca03-108">**Vytvoření aplikace Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0ca03-108">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="0ca03-109">Hello Azure AD aplikace tooauthenticate hello Data Lake Analytics aplikaci můžete používat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ca03-109">You use hello Azure AD application tooauthenticate hello Data Lake Analytics application with Azure AD.</span></span> <span data-ttu-id="0ca03-110">Existují různé přístupy tooauthenticate s Azure AD, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**.</span><span class="sxs-lookup"><span data-stu-id="0ca03-110">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="0ca03-111">Pokyny a další informace o tooauthenticate, najdete v části [ověřit s Data Lake Analytics pomocí Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="0ca03-111">For instructions and more information on how tooauthenticate, see [Authenticate with Data Lake Analytics using Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span>
* <span data-ttu-id="0ca03-112">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="0ca03-112">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="0ca03-113">Tento článek používá cURL toodemonstrate, jakým způsobem volá toomake REST API vůči účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0ca03-113">This article uses cURL toodemonstrate how toomake REST API calls against a Data Lake Analytics account.</span></span>

## <a name="authenticate-with-azure-active-directory"></a><span data-ttu-id="0ca03-114">Ověřování pomocí Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0ca03-114">Authenticate with Azure Active Directory</span></span>
<span data-ttu-id="0ca03-115">Existují dvě metody ověřování pomocí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0ca03-115">There are two methods for authenticating with Azure Active Directory.</span></span>

### <a name="end-user-authentication-interactive"></a><span data-ttu-id="0ca03-116">Ověření koncového uživatele (interaktivní)</span><span class="sxs-lookup"><span data-stu-id="0ca03-116">End-user authentication (interactive)</span></span>
<span data-ttu-id="0ca03-117">Pomocí této metody aplikace vyzve uživatele toolog hello v a všechny operace hello se provádějí v kontextu hello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="0ca03-117">Using this method, application prompts hello user toolog in and all hello operations are performed in hello context of hello user.</span></span> 

<span data-ttu-id="0ca03-118">Pokud chcete zavést interaktivní ověřování, postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="0ca03-118">Follow these steps for interactive authentication:</span></span>

1. <span data-ttu-id="0ca03-119">Prostřednictvím aplikace přesměrování toohello uživatele hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="0ca03-119">Through your application, redirect hello user toohello following URL:</span></span>
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > <span data-ttu-id="0ca03-120">\<REDIRECT-URI > musí toobe kódováním pro použití v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="0ca03-120">\<REDIRECT-URI> needs toobe encoded for use in a URL.</span></span> <span data-ttu-id="0ca03-121">Pro adresu https://localhost proto použijte zápis `https%3A%2F%2Flocalhost`).</span><span class="sxs-lookup"><span data-stu-id="0ca03-121">So, for https://localhost, use `https%3A%2F%2Flocalhost`)</span></span>
   > 
   > 
   
    <span data-ttu-id="0ca03-122">Za účelem hello tohoto kurzu můžete nahradit zástupné hodnoty hello v adrese URL hello výše a vložte jej do adresního řádku webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0ca03-122">For hello purpose of this tutorial, you can replace hello placeholder values in hello URL above and paste it in a web browser's address bar.</span></span> <span data-ttu-id="0ca03-123">Bude přesměrované tooauthenticate pomocí přihlášení Azure.</span><span class="sxs-lookup"><span data-stu-id="0ca03-123">You will be redirected tooauthenticate using your Azure login.</span></span> <span data-ttu-id="0ca03-124">Po úspěšném přihlášení, hello odpovědi se zobrazí v adresním řádku prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="0ca03-124">Once you succesfully log in, hello response is displayed in hello browser's address bar.</span></span> <span data-ttu-id="0ca03-125">odpověď Hello bude v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="0ca03-125">hello response will be in hello following format:</span></span>
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. <span data-ttu-id="0ca03-126">Zaznamenejte hello autorizační kód z odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="0ca03-126">Capture hello authorization code from hello response.</span></span> <span data-ttu-id="0ca03-127">V tomto kurzu můžete zkopírovat hello autorizační kód z panelu Adresa hello hello webového prohlížeče a předat jej v hello POST požadavek toohello koncovému bodu tokenu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="0ca03-127">For this tutorial, you can copy hello authorization code from hello address bar of hello web browser and pass it in hello POST request toohello token endpoint, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > <span data-ttu-id="0ca03-128">V takovém případě hello \<REDIRECT-URI > nemusí být zakódován.</span><span class="sxs-lookup"><span data-stu-id="0ca03-128">In this case, hello \<REDIRECT-URI> need not be encoded.</span></span>
   > 
   > 
3. <span data-ttu-id="0ca03-129">odpověď Hello je objekt JSON, který obsahuje přístupový token (například `"access_token": "<ACCESS_TOKEN>"`) a obnovovací token (například `"refresh_token": "<REFRESH_TOKEN>"`).</span><span class="sxs-lookup"><span data-stu-id="0ca03-129">hello response is a JSON object that contains an access token (e.g., `"access_token": "<ACCESS_TOKEN>"`) and a refresh token (e.g., `"refresh_token": "<REFRESH_TOKEN>"`).</span></span> <span data-ttu-id="0ca03-130">Vaše aplikace používá hello přístupový token při přístupu k Azure Data Lake Store a hello aktualizace tokenu tooget dalšího přístupového tokenu když vyprší platnost přístupového tokenu.</span><span class="sxs-lookup"><span data-stu-id="0ca03-130">Your application uses hello access token when accessing Azure Data Lake Store and hello refresh token tooget another access token when an access token expires.</span></span>
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. <span data-ttu-id="0ca03-131">Když vyprší platnost hello přístupový token, můžete požádat o nový přístupový token pomocí hello aktualizace tokenu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="0ca03-131">When hello access token expires, you can request a new access token using hello refresh token, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

<span data-ttu-id="0ca03-132">Další informace o interaktivním ověřování uživatelů najdete v tématu [Tok poskytování autorizačních kódů](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ca03-132">For more information on interactive user authentication, see [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span></span>

### <a name="service-to-service-authentication-non-interactive"></a><span data-ttu-id="0ca03-133">Ověřování služba-služba (neinteraktivní)</span><span class="sxs-lookup"><span data-stu-id="0ca03-133">Service-to-service authentication (non-interactive)</span></span>
<span data-ttu-id="0ca03-134">Pomocí této metody aplikace poskytuje svoje vlastní přihlašovací údaje při tooperform hello operace.</span><span class="sxs-lookup"><span data-stu-id="0ca03-134">Using this method, application provides its own credentials tooperform hello operations.</span></span> <span data-ttu-id="0ca03-135">V takovém případě musíte vydat požadavek POST jako hello níže:</span><span class="sxs-lookup"><span data-stu-id="0ca03-135">For this, you must issue a POST request like hello one shown below:</span></span> 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

<span data-ttu-id="0ca03-136">Hello výstup tohoto požadavku bude obsahovat autorizační token (odlišené `access-token` ve výstupu hello níže), potom budete předávat s voláními rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="0ca03-136">hello output of this request will include an authorization token (denoted by `access-token` in hello output below) that you will subsequently pass with your REST API calls.</span></span> <span data-ttu-id="0ca03-137">Tento ověřovací token si uložte do textového souboru, protože ho budete později v tomto článku potřebovat.</span><span class="sxs-lookup"><span data-stu-id="0ca03-137">Save this authentication token in a text file; you will need this later in this article.</span></span>

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

<span data-ttu-id="0ca03-138">Tento článek používá hello **neinteraktivní** přístup.</span><span class="sxs-lookup"><span data-stu-id="0ca03-138">This article uses hello **non-interactive** approach.</span></span> <span data-ttu-id="0ca03-139">Další informace o neinteraktivním (volání služba služba) najdete v tématu [služba pomocí přihlašovacích údajů volání tooservice](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ca03-139">For more information on non-interactive (service-to-service calls), see [Service tooservice calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="0ca03-140">Vytvoření účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="0ca03-140">Create a Data Lake Analytics account</span></span>
<span data-ttu-id="0ca03-141">Před vytvořením účtu Data Lake Analytics je nutné vytvořit skupinu prostředků Azure a účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="0ca03-141">You must create an Azure Resource group, and a Data Lake Store account before you can create a Data Lake Analytics account.</span></span>  <span data-ttu-id="0ca03-142">Viz [Vytvoření účtu Data Lake Store](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).</span><span class="sxs-lookup"><span data-stu-id="0ca03-142">See [Create a Data Lake Store account](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).</span></span>

<span data-ttu-id="0ca03-143">Dobrý den, jak následující příkaz ukazuje Curl toocreate účet:</span><span class="sxs-lookup"><span data-stu-id="0ca03-143">hello following Curl command shows how toocreate an account:</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

<span data-ttu-id="0ca03-144">Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `AzureSubscriptionID` \> s svoje ID předplatného \< `AzureResourceGroupName` \> s existující prostředek Azure Název skupiny a \< `NewAzureDataLakeAnalyticsAccountName` \> s novým názvem účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0ca03-144">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`NewAzureDataLakeAnalyticsAccountName`\> with a new Data Lake Analytics Account name.</span></span> <span data-ttu-id="0ca03-145">Hello datová část požadavku tohoto příkazu se nachází v hello **CreateDatalakeAnalyticsAccountRequest.json** soubor, který je k dispozici pro hello `-d` parametr výše.</span><span class="sxs-lookup"><span data-stu-id="0ca03-145">hello request payload for this command is contained in hello **CreateDatalakeAnalyticsAccountRequest.json** file that is provided for hello `-d` parameter above.</span></span> <span data-ttu-id="0ca03-146">Hello obsah souboru Input.JSON vypadá hello vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="0ca03-146">hello contents of hello input.json file resemble hello following:</span></span>

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a><span data-ttu-id="0ca03-147">Zobrazení seznamu účtů Data Lake Analytics v předplatném</span><span class="sxs-lookup"><span data-stu-id="0ca03-147">List Data Lake Analytics accounts in a subscription</span></span>
<span data-ttu-id="0ca03-148">Následující příkaz Curl Hello ukazuje, jak toolist účty v předplatném:</span><span class="sxs-lookup"><span data-stu-id="0ca03-148">hello following Curl command shows how toolist accounts in a subscription:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

<span data-ttu-id="0ca03-149">Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `AzureSubscriptionID` \> s vaším ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="0ca03-149">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID.</span></span> <span data-ttu-id="0ca03-150">výstup Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="0ca03-150">hello output is similar to:</span></span>

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="0ca03-151">Získání informací o účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="0ca03-151">Get information about a Data Lake Analytics account</span></span>
<span data-ttu-id="0ca03-152">Dobrý den, jak následující příkaz ukazuje Curl tooget informace účtu:</span><span class="sxs-lookup"><span data-stu-id="0ca03-152">hello following Curl command shows how tooget an account information:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

<span data-ttu-id="0ca03-153">Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `AzureSubscriptionID` \> s svoje ID předplatného \< `AzureResourceGroupName` \> s existující prostředek Azure Název skupiny a \< `DataLakeAnalyticsAccountName` \> s názvem hello stávajícího účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0ca03-153">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="0ca03-154">výstup Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="0ca03-154">hello output is similar to:</span></span>

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a><span data-ttu-id="0ca03-155">Zobrazení seznamu služeb Data Lake Store pro účet Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="0ca03-155">List Data Lake Stores of a Data Lake Analytics account</span></span>
<span data-ttu-id="0ca03-156">Následující příkaz Curl Hello ukazuje, jak ukládá toolist Data Lake účtu:</span><span class="sxs-lookup"><span data-stu-id="0ca03-156">hello following Curl command shows how toolist Data Lake Stores of an account:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

<span data-ttu-id="0ca03-157">Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `AzureSubscriptionID` \> s svoje ID předplatného \< `AzureResourceGroupName` \> s existující prostředek Azure Název skupiny a \< `DataLakeAnalyticsAccountName` \> s názvem hello stávajícího účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0ca03-157">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="0ca03-158">výstup Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="0ca03-158">hello output is similar to:</span></span>

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a><span data-ttu-id="0ca03-159">Odesílání úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="0ca03-159">Submit U-SQL jobs</span></span>
<span data-ttu-id="0ca03-160">Dobrý den, jak následující příkaz ukazuje Curl úlohy toosubmit U-SQL:</span><span class="sxs-lookup"><span data-stu-id="0ca03-160">hello following Curl command shows how toosubmit a U-SQL job:</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

<span data-ttu-id="0ca03-161">Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `DataLakeAnalyticsAccountName` \> s názvem hello stávajícího účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0ca03-161">Replace \<`REDACTED`\> with hello authorization token, \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="0ca03-162">Hello datová část požadavku tohoto příkazu se nachází v hello **SubmitADLAJob.json** soubor, který je k dispozici pro hello `-d` parametr výše.</span><span class="sxs-lookup"><span data-stu-id="0ca03-162">hello request payload for this command is contained in hello **SubmitADLAJob.json** file that is provided for hello `-d` parameter above.</span></span> <span data-ttu-id="0ca03-163">Hello obsah souboru Input.JSON vypadá hello vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="0ca03-163">hello contents of hello input.json file resemble hello following:</span></span>

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    too\"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

<span data-ttu-id="0ca03-164">výstup Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="0ca03-164">hello output is similar to:</span></span>

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a><span data-ttu-id="0ca03-165">Zobrazení seznamu úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="0ca03-165">List U-SQL jobs</span></span>
<span data-ttu-id="0ca03-166">Dobrý den, jak následující příkaz ukazuje Curl úlohy toolist U-SQL:</span><span class="sxs-lookup"><span data-stu-id="0ca03-166">hello following Curl command shows how toolist U-SQL jobs:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

<span data-ttu-id="0ca03-167">Nahraďte \< `REDACTED` \> s tokenem autorizace hello, a \< `DataLakeAnalyticsAccountName` \> s názvem hello stávajícího účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0ca03-167">Replace \<`REDACTED`\> with hello authorization token, and \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> 

<span data-ttu-id="0ca03-168">výstup Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="0ca03-168">hello output is similar to:</span></span>

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a><span data-ttu-id="0ca03-169">Získání položek katalogu</span><span class="sxs-lookup"><span data-stu-id="0ca03-169">Get catalog items</span></span>
<span data-ttu-id="0ca03-170">Následující příkaz Curl Hello ukazuje, jak hello tooget hello databáze z katalogu:</span><span class="sxs-lookup"><span data-stu-id="0ca03-170">hello following Curl command shows how tooget hello databases from hello catalog:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

<span data-ttu-id="0ca03-171">výstup Hello je podobná:</span><span class="sxs-lookup"><span data-stu-id="0ca03-171">hello output is similar to:</span></span>

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a><span data-ttu-id="0ca03-172">Viz také</span><span class="sxs-lookup"><span data-stu-id="0ca03-172">See also</span></span>
* <span data-ttu-id="0ca03-173">toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="0ca03-173">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="0ca03-174">tooget práce s vývojem aplikací U-SQL najdete v části [skriptů vyvíjet U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0ca03-174">tooget started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="0ca03-175">toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0ca03-175">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="0ca03-176">Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0ca03-176">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
* <span data-ttu-id="0ca03-177">tooget uvádí přehled Data Lake Analytics najdete v části [přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0ca03-177">tooget an overview of Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>
* <span data-ttu-id="0ca03-178">toosee hello stejný kurz pomocí jiných nástrojů, klikněte na selektory karet hello na hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="0ca03-178">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>

