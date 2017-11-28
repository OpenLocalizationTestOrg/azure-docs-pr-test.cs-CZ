---
title: "aaaUse hello Python SDK tooget začít s Azure Data Lake Store | Microsoft Docs"
description: "Zjistěte, jak toouse Python SDK toowork s účty Data Lake Store a hello systému souborů."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 7061fdf25ef607608bab618a20ddd3d6fc7af01d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-python"></a><span data-ttu-id="180d8-103">Začínáme s Azure Data Lake Store pomocí jazyka Python</span><span class="sxs-lookup"><span data-stu-id="180d8-103">Get started with Azure Data Lake Store using Python</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="180d8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="180d8-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="180d8-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="180d8-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="180d8-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="180d8-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="180d8-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="180d8-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="180d8-108">REST API</span><span class="sxs-lookup"><span data-stu-id="180d8-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="180d8-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="180d8-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="180d8-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="180d8-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="180d8-111">Python</span><span class="sxs-lookup"><span data-stu-id="180d8-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="180d8-112">Zjistěte, jak toouse hello Python SDK pro Azure a Azure Data Lake Store tooperform základních operací, jako vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="180d8-112">Learn how toouse hello Python SDK for Azure and Azure Data Lake Store tooperform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="180d8-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="180d8-113">Prerequisites</span></span>

* <span data-ttu-id="180d8-114">**Python**.</span><span class="sxs-lookup"><span data-stu-id="180d8-114">**Python**.</span></span> <span data-ttu-id="180d8-115">Python si můžete stáhnout [tady](https://www.python.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="180d8-115">You can download Python from [here](https://www.python.org/downloads/).</span></span> <span data-ttu-id="180d8-116">Tento článek používá Python verze 3.5.2.</span><span class="sxs-lookup"><span data-stu-id="180d8-116">This article uses Python 3.5.2.</span></span>

* <span data-ttu-id="180d8-117">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="180d8-117">**An Azure subscription**.</span></span> <span data-ttu-id="180d8-118">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="180d8-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="180d8-119">**Vytvoření aplikace Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="180d8-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="180d8-120">Hello Azure AD aplikace tooauthenticate hello Data Lake Store aplikaci můžete používat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="180d8-120">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="180d8-121">Existují různé přístupy tooauthenticate s Azure AD, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**.</span><span class="sxs-lookup"><span data-stu-id="180d8-121">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="180d8-122">Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="180d8-122">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="install-hello-modules"></a><span data-ttu-id="180d8-123">Instalace modulů hello</span><span class="sxs-lookup"><span data-stu-id="180d8-123">Install hello modules</span></span>

<span data-ttu-id="180d8-124">toowork s Data Lake Store pomocí jazyka Python, musíte tooinstall tři moduly.</span><span class="sxs-lookup"><span data-stu-id="180d8-124">toowork with Data Lake Store using Python, you need tooinstall three modules.</span></span>

* <span data-ttu-id="180d8-125">Hello `azure-mgmt-resource` modulu.</span><span class="sxs-lookup"><span data-stu-id="180d8-125">hello `azure-mgmt-resource` module.</span></span> <span data-ttu-id="180d8-126">Zahrnuje moduly Azure pro Active Directory atd.</span><span class="sxs-lookup"><span data-stu-id="180d8-126">This includes Azure modules for Active Directory, etc..</span></span>
* <span data-ttu-id="180d8-127">Hello `azure-mgmt-datalake-store` modulu.</span><span class="sxs-lookup"><span data-stu-id="180d8-127">hello `azure-mgmt-datalake-store` module.</span></span> <span data-ttu-id="180d8-128">To zahrnuje operace správy účtů Azure Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="180d8-128">This includes hello Azure Data Lake Store account management operations.</span></span> <span data-ttu-id="180d8-129">Další informace o tomto modulu najdete v [referenčních informacích k modulu Azure Data Lake Store Management](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).</span><span class="sxs-lookup"><span data-stu-id="180d8-129">For more information on this module, see [Azure Data Lake Store Management module reference](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).</span></span>
* <span data-ttu-id="180d8-130">Hello `azure-datalake-store` modulu.</span><span class="sxs-lookup"><span data-stu-id="180d8-130">hello `azure-datalake-store` module.</span></span> <span data-ttu-id="180d8-131">To zahrnuje operace systému souborů hello Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="180d8-131">This includes hello Azure Data Lake Store filesystem operations.</span></span> <span data-ttu-id="180d8-132">Další informace o tomto modulu najdete v [referenčních informacích k modulu Azure Data Lake Store Filesystem](http://azure-datalake-store.readthedocs.io/en/latest/).</span><span class="sxs-lookup"><span data-stu-id="180d8-132">For more information on this module, see [Azure Data Lake Store Filesystem module reference](http://azure-datalake-store.readthedocs.io/en/latest/).</span></span>

<span data-ttu-id="180d8-133">Použijte následující příkazy tooinstall hello moduly hello.</span><span class="sxs-lookup"><span data-stu-id="180d8-133">Use hello following commands tooinstall hello modules.</span></span>

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a><span data-ttu-id="180d8-134">Vytvoření nové aplikace v Pythonu</span><span class="sxs-lookup"><span data-stu-id="180d8-134">Create a new Python application</span></span>

1. <span data-ttu-id="180d8-135">V hello IDE zvoleného vytvořte novou aplikaci Python, například **mysample.py**.</span><span class="sxs-lookup"><span data-stu-id="180d8-135">In hello IDE of your choice create a new Python application, for example, **mysample.py**.</span></span>

2. <span data-ttu-id="180d8-136">Přidejte následující řádky tooimport hello požadované moduly hello</span><span class="sxs-lookup"><span data-stu-id="180d8-136">Add hello following lines tooimport hello required modules</span></span>

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. <span data-ttu-id="180d8-137">Uložte změny toomysample.py.</span><span class="sxs-lookup"><span data-stu-id="180d8-137">Save changes toomysample.py.</span></span>

## <a name="authentication"></a><span data-ttu-id="180d8-138">Authentication</span><span class="sxs-lookup"><span data-stu-id="180d8-138">Authentication</span></span>

<span data-ttu-id="180d8-139">V této části se věnuje hello různé způsoby tooauthenticate s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="180d8-139">In this section, we talk about hello different ways tooauthenticate with Azure AD.</span></span> <span data-ttu-id="180d8-140">Hello možnosti, které jsou k dispozici jsou následující:</span><span class="sxs-lookup"><span data-stu-id="180d8-140">hello options available are:</span></span>

* <span data-ttu-id="180d8-141">Ověřování koncových uživatelů</span><span class="sxs-lookup"><span data-stu-id="180d8-141">End-user authentication</span></span>
* <span data-ttu-id="180d8-142">Ověřování služba-služba</span><span class="sxs-lookup"><span data-stu-id="180d8-142">Service-to-service authentication</span></span>
* <span data-ttu-id="180d8-143">Ověřování pomocí služby Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="180d8-143">Multi-factor authentication</span></span>

<span data-ttu-id="180d8-144">Tyto možnosti ověřování je nutné použít jak pro správu účtů, tak i pro moduly správy systému souborů.</span><span class="sxs-lookup"><span data-stu-id="180d8-144">You must use these authentication options for both account management and filesystem management modules.</span></span>

### <a name="end-user-authentication-for-account-management"></a><span data-ttu-id="180d8-145">Ověřování koncového uživatele pro správu účtu</span><span class="sxs-lookup"><span data-stu-id="180d8-145">End-user authentication for account management</span></span>

<span data-ttu-id="180d8-146">Pomocí této tooauthenticate s Azure AD pro operace správy účtů (Vytvoření nebo odstranění účtu Data Lake Store, atd.).</span><span class="sxs-lookup"><span data-stu-id="180d8-146">Use this tooauthenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="180d8-147">Je třeba zadat uživatelské jméno a heslo uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="180d8-147">You must provide username and password for an Azure AD user.</span></span> <span data-ttu-id="180d8-148">Všimněte si, že tento uživatel hello by se neměla konfigurovat službu Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="180d8-148">Note that hello user should not be configured for multi-factor authentication.</span></span>

    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="end-user-authentication-for-filesystem-operations"></a><span data-ttu-id="180d8-149">Ověřování koncového uživatele pro operace se systémem souborů</span><span class="sxs-lookup"><span data-stu-id="180d8-149">End-user authentication for filesystem operations</span></span>

<span data-ttu-id="180d8-150">Pomocí této tooauthenticate s Azure AD pro operace systému souborů (vytvořit složku, nahrávání souborů atd.).</span><span class="sxs-lookup"><span data-stu-id="180d8-150">Use this tooauthenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="180d8-151">Tento kód použijte se stávající aplikací **nativního klienta** Azure AD.</span><span class="sxs-lookup"><span data-stu-id="180d8-151">Use this with an existing Azure AD **native client** application.</span></span> <span data-ttu-id="180d8-152">Zadejte pověření pro uživatele Hello Azure AD by se neměla konfigurovat službu Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="180d8-152">hello Azure AD user you provide credentials for should not be configured for multi-factor authentication.</span></span>

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a><span data-ttu-id="180d8-153">Ověřování služba-služba s tajným klíčem klienta pro správu účtu</span><span class="sxs-lookup"><span data-stu-id="180d8-153">Service-to-service authentication with client secret for account management</span></span>

<span data-ttu-id="180d8-154">Pomocí této tooauthenticate s Azure AD pro operace správy účtů (Vytvoření nebo odstranění účtu Data Lake Store, atd.).</span><span class="sxs-lookup"><span data-stu-id="180d8-154">Use this tooauthenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="180d8-155">Následující fragment kódu Hello lze použít tooauthenticate aplikace interaktivně, pomocí hello tajný klíč klienta pro hlavní aplikaci / service.</span><span class="sxs-lookup"><span data-stu-id="180d8-155">hello following snippet can be used tooauthenticate your application non-interactively, using hello client secret for an application / service principal.</span></span> <span data-ttu-id="180d8-156">Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="180d8-156">Use this with an existing Azure AD "Web App" application.</span></span>

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

### <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a><span data-ttu-id="180d8-157">Ověřování služba-služba s tajným klíčem klienta pro operace se systémem souborů</span><span class="sxs-lookup"><span data-stu-id="180d8-157">Service-to-service authentication with client secret for filesystem operations</span></span>

<span data-ttu-id="180d8-158">Pomocí této tooauthenticate s Azure AD pro operace systému souborů (vytvořit složku, nahrávání souborů atd.).</span><span class="sxs-lookup"><span data-stu-id="180d8-158">Use this tooauthenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="180d8-159">Následující fragment kódu Hello lze použít tooauthenticate aplikace interaktivně, pomocí hello tajný klíč klienta pro hlavní aplikaci / service.</span><span class="sxs-lookup"><span data-stu-id="180d8-159">hello following snippet can be used tooauthenticate your application non-interactively, using hello client secret for an application / service principal.</span></span> <span data-ttu-id="180d8-160">Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="180d8-160">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

### <a name="multi-factor-authentication-for-account-management"></a><span data-ttu-id="180d8-161">Vícefaktorové ověřování pro správu účtu</span><span class="sxs-lookup"><span data-stu-id="180d8-161">Multi-factor authentication for account management</span></span>

<span data-ttu-id="180d8-162">Pomocí této tooauthenticate s Azure AD pro operace správy účtů (Vytvoření nebo odstranění účtu Data Lake Store, atd.).</span><span class="sxs-lookup"><span data-stu-id="180d8-162">Use this tooauthenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="180d8-163">Hello následující fragment kódu může být použit tooauthenticate aplikace pomocí služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="180d8-163">hello following snippet can be used tooauthenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="180d8-164">Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="180d8-164">Use this with an existing Azure AD "Web App" application.</span></span>

    authority_host_url = "https://login.microsoftonline.com"
    tenant = "FILL-IN-HERE"
    authority_url = authority_host_url + '/' + tenant
    client_id = 'FILL-IN-HERE'
    redirect = 'urn:ietf:wg:oauth:2.0:oob'
    RESOURCE = 'https://management.core.windows.net/'
    
    context = adal.AuthenticationContext(authority_url)
    code = context.acquire_user_code(RESOURCE, client_id)
    print(code['message'])
    mgmt_token = context.acquire_token_with_device_code(RESOURCE, code, client_id)
    credentials = AADTokenCredentials(mgmt_token, client_id)

### <a name="multi-factor-authentication-for-filesystem-management"></a><span data-ttu-id="180d8-165">Vícefaktorové ověřování pro správu systému souborů</span><span class="sxs-lookup"><span data-stu-id="180d8-165">Multi-factor authentication for filesystem management</span></span>

<span data-ttu-id="180d8-166">Pomocí této tooauthenticate s Azure AD pro operace systému souborů (vytvořit složku, nahrávání souborů atd.).</span><span class="sxs-lookup"><span data-stu-id="180d8-166">Use this tooauthenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="180d8-167">Hello následující fragment kódu může být použit tooauthenticate aplikace pomocí služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="180d8-167">hello following snippet can be used tooauthenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="180d8-168">Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="180d8-168">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id='FILL-IN-HERE')

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="180d8-169">Vytvoření skupiny prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="180d8-169">Create an Azure Resource Group</span></span>

<span data-ttu-id="180d8-170">Použijte následující toocreate fragmentu kódu skupiny prostředků Azure hello:</span><span class="sxs-lookup"><span data-stu-id="180d8-170">Use hello following code snippet toocreate an Azure Resource Group:</span></span>

    ## Declare variables
    subscriptionId= 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'
    
    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )
    
    ## Create an Azure Resource Group
    resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )

## <a name="create-clients-and-data-lake-store-account"></a><span data-ttu-id="180d8-171">Vytvoření klientů a účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="180d8-171">Create clients and Data Lake Store account</span></span>

<span data-ttu-id="180d8-172">Následující fragment kódu nejprve Hello vytvoří klienta pro účet Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="180d8-172">hello following snippet first creates hello Data Lake Store account client.</span></span> <span data-ttu-id="180d8-173">Používá hello klienta objekt toocreate účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="180d8-173">It uses hello client object toocreate a Data Lake Store account.</span></span> <span data-ttu-id="180d8-174">Nakonec hello fragment kódu vytvoří objekt systému souborů klienta.</span><span class="sxs-lookup"><span data-stu-id="180d8-174">Finally, hello snippet creates a filesystem client object.</span></span>

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(credentials, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(token, store_name=adlsAccountName)

## <a name="list-hello-data-lake-store-accounts"></a><span data-ttu-id="180d8-175">Seznam účtů Data Lake Store hello</span><span class="sxs-lookup"><span data-stu-id="180d8-175">List hello Data Lake Store accounts</span></span>

    ## List hello existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="create-a-directory"></a><span data-ttu-id="180d8-176">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="180d8-176">Create a directory</span></span>

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a><span data-ttu-id="180d8-177">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="180d8-177">Upload a file</span></span>


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a><span data-ttu-id="180d8-178">Stažení souboru</span><span class="sxs-lookup"><span data-stu-id="180d8-178">Download a file</span></span>

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a><span data-ttu-id="180d8-179">Odstranění adresáře</span><span class="sxs-lookup"><span data-stu-id="180d8-179">Delete a directory</span></span>

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="see-also"></a><span data-ttu-id="180d8-180">Viz také</span><span class="sxs-lookup"><span data-stu-id="180d8-180">See also</span></span>

- [<span data-ttu-id="180d8-181">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="180d8-181">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
- [<span data-ttu-id="180d8-182">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="180d8-182">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [<span data-ttu-id="180d8-183">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="180d8-183">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
- [<span data-ttu-id="180d8-184">Referenční dokumentace sady SDK rozhraní .NET služby Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="180d8-184">Data Lake Store .NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt581387.aspx)
- [<span data-ttu-id="180d8-185">Referenční dokumentace architektury REST služby Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="180d8-185">Data Lake Store REST Reference</span></span>](https://msdn.microsoft.com/library/mt693424.aspx)
