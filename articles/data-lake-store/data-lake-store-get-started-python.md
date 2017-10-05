---
title: "Začínáme s Azure Data Lake Store pomocí sady Python SDK | Dokumentace Microsoftu"
description: "Přečtěte si, jak pomocí sady SDK pro Python pracovat s účty a systémem souborů Data Lake Store."
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
ms.openlocfilehash: 375a603360ac249fc1b08923a94c85652390a3fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-python"></a><span data-ttu-id="fa727-103">Začínáme s Azure Data Lake Store pomocí jazyka Python</span><span class="sxs-lookup"><span data-stu-id="fa727-103">Get started with Azure Data Lake Store using Python</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="fa727-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fa727-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="fa727-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fa727-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="fa727-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="fa727-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="fa727-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="fa727-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="fa727-108">REST API</span><span class="sxs-lookup"><span data-stu-id="fa727-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="fa727-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fa727-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="fa727-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="fa727-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="fa727-111">Python</span><span class="sxs-lookup"><span data-stu-id="fa727-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="fa727-112">Naučte se používat sadu SDK pro Python pro Azure Data Lake Store k provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fa727-112">Learn how to use the Python SDK for Azure and Azure Data Lake Store to perform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa727-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fa727-113">Prerequisites</span></span>

* <span data-ttu-id="fa727-114">**Python**.</span><span class="sxs-lookup"><span data-stu-id="fa727-114">**Python**.</span></span> <span data-ttu-id="fa727-115">Python si můžete stáhnout [tady](https://www.python.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="fa727-115">You can download Python from [here](https://www.python.org/downloads/).</span></span> <span data-ttu-id="fa727-116">Tento článek používá Python verze 3.5.2.</span><span class="sxs-lookup"><span data-stu-id="fa727-116">This article uses Python 3.5.2.</span></span>

* <span data-ttu-id="fa727-117">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="fa727-117">**An Azure subscription**.</span></span> <span data-ttu-id="fa727-118">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fa727-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="fa727-119">**Vytvoření aplikace Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fa727-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="fa727-120">Aplikaci Azure AD použijete k ověření aplikace Data Lake Store ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fa727-120">You use the Azure AD application to authenticate the Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="fa727-121">Existují různé přístupy k ověřování ve službě Azure AD, jsou to **ověřování koncového uživatele** nebo **ověřování služba-služba**.</span><span class="sxs-lookup"><span data-stu-id="fa727-121">There are different approaches to authenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="fa727-122">Pokyny a další informace o ověřování najdete v tématu [Ověřování koncových uživatelů](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Ověřování služba-služba](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="fa727-122">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="install-the-modules"></a><span data-ttu-id="fa727-123">Instalace modulů</span><span class="sxs-lookup"><span data-stu-id="fa727-123">Install the modules</span></span>

<span data-ttu-id="fa727-124">Abyste mohli pracovat se službou Data Lake Store pomocí Pythonu, je nutné nainstalovat tři moduly.</span><span class="sxs-lookup"><span data-stu-id="fa727-124">To work with Data Lake Store using Python, you need to install three modules.</span></span>

* <span data-ttu-id="fa727-125">Modul `azure-mgmt-resource`.</span><span class="sxs-lookup"><span data-stu-id="fa727-125">The `azure-mgmt-resource` module.</span></span> <span data-ttu-id="fa727-126">Zahrnuje moduly Azure pro Active Directory atd.</span><span class="sxs-lookup"><span data-stu-id="fa727-126">This includes Azure modules for Active Directory, etc..</span></span>
* <span data-ttu-id="fa727-127">Modul `azure-mgmt-datalake-store`.</span><span class="sxs-lookup"><span data-stu-id="fa727-127">The `azure-mgmt-datalake-store` module.</span></span> <span data-ttu-id="fa727-128">Zahrnuje operace správy účtů Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fa727-128">This includes the Azure Data Lake Store account management operations.</span></span> <span data-ttu-id="fa727-129">Další informace o tomto modulu najdete v [referenčních informacích k modulu Azure Data Lake Store Management](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).</span><span class="sxs-lookup"><span data-stu-id="fa727-129">For more information on this module, see [Azure Data Lake Store Management module reference](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).</span></span>
* <span data-ttu-id="fa727-130">Modul `azure-datalake-store`.</span><span class="sxs-lookup"><span data-stu-id="fa727-130">The `azure-datalake-store` module.</span></span> <span data-ttu-id="fa727-131">Zahrnuje operace se systémem souborů Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fa727-131">This includes the Azure Data Lake Store filesystem operations.</span></span> <span data-ttu-id="fa727-132">Další informace o tomto modulu najdete v [referenčních informacích k modulu Azure Data Lake Store Filesystem](http://azure-datalake-store.readthedocs.io/en/latest/).</span><span class="sxs-lookup"><span data-stu-id="fa727-132">For more information on this module, see [Azure Data Lake Store Filesystem module reference](http://azure-datalake-store.readthedocs.io/en/latest/).</span></span>

<span data-ttu-id="fa727-133">Pomocí následujících příkazů tyto moduly nainstalujte.</span><span class="sxs-lookup"><span data-stu-id="fa727-133">Use the following commands to install the modules.</span></span>

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a><span data-ttu-id="fa727-134">Vytvoření nové aplikace v Pythonu</span><span class="sxs-lookup"><span data-stu-id="fa727-134">Create a new Python application</span></span>

1. <span data-ttu-id="fa727-135">Pomocí integrovaného vývojového prostředí (IDE) podle vašeho výběru vytvořte novou aplikaci v Pythonu, například **mysample.py**.</span><span class="sxs-lookup"><span data-stu-id="fa727-135">In the IDE of your choice create a new Python application, for example, **mysample.py**.</span></span>

2. <span data-ttu-id="fa727-136">Přidáním následujících řádků importujte požadované moduly.</span><span class="sxs-lookup"><span data-stu-id="fa727-136">Add the following lines to import the required modules</span></span>

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

3. <span data-ttu-id="fa727-137">Uložte změny v souboru mysample.py.</span><span class="sxs-lookup"><span data-stu-id="fa727-137">Save changes to mysample.py.</span></span>

## <a name="authentication"></a><span data-ttu-id="fa727-138">Ověřování</span><span class="sxs-lookup"><span data-stu-id="fa727-138">Authentication</span></span>

<span data-ttu-id="fa727-139">V této části popíšeme různé způsoby, jak provádět ověření pomocí Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fa727-139">In this section, we talk about the different ways to authenticate with Azure AD.</span></span> <span data-ttu-id="fa727-140">Dostupné jsou následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="fa727-140">The options available are:</span></span>

* <span data-ttu-id="fa727-141">Ověřování koncových uživatelů</span><span class="sxs-lookup"><span data-stu-id="fa727-141">End-user authentication</span></span>
* <span data-ttu-id="fa727-142">Ověřování služba-služba</span><span class="sxs-lookup"><span data-stu-id="fa727-142">Service-to-service authentication</span></span>
* <span data-ttu-id="fa727-143">Ověřování pomocí služby Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="fa727-143">Multi-factor authentication</span></span>

<span data-ttu-id="fa727-144">Tyto možnosti ověřování je nutné použít jak pro správu účtů, tak i pro moduly správy systému souborů.</span><span class="sxs-lookup"><span data-stu-id="fa727-144">You must use these authentication options for both account management and filesystem management modules.</span></span>

### <a name="end-user-authentication-for-account-management"></a><span data-ttu-id="fa727-145">Ověřování koncového uživatele pro správu účtu</span><span class="sxs-lookup"><span data-stu-id="fa727-145">End-user authentication for account management</span></span>

<span data-ttu-id="fa727-146">Tento kód použijte k ověření ve službě Azure AD pro operace správy účtu (vytvoření/odstranění účtu Data Lake Store atd.).</span><span class="sxs-lookup"><span data-stu-id="fa727-146">Use this to authenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="fa727-147">Je třeba zadat uživatelské jméno a heslo uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fa727-147">You must provide username and password for an Azure AD user.</span></span> <span data-ttu-id="fa727-148">Upozorňujeme, že daný uživatel by neměl mít zapnuté vícefaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="fa727-148">Note that the user should not be configured for multi-factor authentication.</span></span>

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="end-user-authentication-for-filesystem-operations"></a><span data-ttu-id="fa727-149">Ověřování koncového uživatele pro operace se systémem souborů</span><span class="sxs-lookup"><span data-stu-id="fa727-149">End-user authentication for filesystem operations</span></span>

<span data-ttu-id="fa727-150">Tento kód použijte k ověření pomocí Azure AD pro operace se systémem souborů (vytvoření složky, nahrání souboru atd.).</span><span class="sxs-lookup"><span data-stu-id="fa727-150">Use this to authenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="fa727-151">Tento kód použijte se stávající aplikací **nativního klienta** Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fa727-151">Use this with an existing Azure AD **native client** application.</span></span> <span data-ttu-id="fa727-152">Uživatel, pro kterého zadáváte přihlašovací údaje Azure AD, by neměl mít zapnuté vícefaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="fa727-152">The Azure AD user you provide credentials for should not be configured for multi-factor authentication.</span></span>

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a><span data-ttu-id="fa727-153">Ověřování služba-služba s tajným klíčem klienta pro správu účtu</span><span class="sxs-lookup"><span data-stu-id="fa727-153">Service-to-service authentication with client secret for account management</span></span>

<span data-ttu-id="fa727-154">Tento kód použijte k ověření ve službě Azure AD pro operace správy účtu (vytvoření/odstranění účtu Data Lake Store atd.).</span><span class="sxs-lookup"><span data-stu-id="fa727-154">Use this to authenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="fa727-155">Následující fragment kódu lze použít k neinteraktivnímu ověřování vaší aplikace pomocí tajného klíče klienta pro aplikaci nebo instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="fa727-155">The following snippet can be used to authenticate your application non-interactively, using the client secret for an application / service principal.</span></span> <span data-ttu-id="fa727-156">Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa727-156">Use this with an existing Azure AD "Web App" application.</span></span>

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

### <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a><span data-ttu-id="fa727-157">Ověřování služba-služba s tajným klíčem klienta pro operace se systémem souborů</span><span class="sxs-lookup"><span data-stu-id="fa727-157">Service-to-service authentication with client secret for filesystem operations</span></span>

<span data-ttu-id="fa727-158">Tento kód použijte k ověření pomocí Azure AD pro operace se systémem souborů (vytvoření složky, nahrání souboru atd.).</span><span class="sxs-lookup"><span data-stu-id="fa727-158">Use this to authenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="fa727-159">Následující fragment kódu lze použít k neinteraktivnímu ověřování vaší aplikace pomocí tajného klíče klienta pro aplikaci nebo instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="fa727-159">The following snippet can be used to authenticate your application non-interactively, using the client secret for an application / service principal.</span></span> <span data-ttu-id="fa727-160">Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa727-160">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

### <a name="multi-factor-authentication-for-account-management"></a><span data-ttu-id="fa727-161">Vícefaktorové ověřování pro správu účtu</span><span class="sxs-lookup"><span data-stu-id="fa727-161">Multi-factor authentication for account management</span></span>

<span data-ttu-id="fa727-162">Tento kód použijte k ověření ve službě Azure AD pro operace správy účtu (vytvoření/odstranění účtu Data Lake Store atd.).</span><span class="sxs-lookup"><span data-stu-id="fa727-162">Use this to authenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="fa727-163">Následující fragment kódu můžete použít k ověřování vaší aplikace pomocí vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="fa727-163">The following snippet can be used to authenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="fa727-164">Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa727-164">Use this with an existing Azure AD "Web App" application.</span></span>

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

### <a name="multi-factor-authentication-for-filesystem-management"></a><span data-ttu-id="fa727-165">Vícefaktorové ověřování pro správu systému souborů</span><span class="sxs-lookup"><span data-stu-id="fa727-165">Multi-factor authentication for filesystem management</span></span>

<span data-ttu-id="fa727-166">Tento kód použijte k ověření pomocí Azure AD pro operace se systémem souborů (vytvoření složky, nahrání souboru atd.).</span><span class="sxs-lookup"><span data-stu-id="fa727-166">Use this to authenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="fa727-167">Následující fragment kódu můžete použít k ověřování vaší aplikace pomocí vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="fa727-167">The following snippet can be used to authenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="fa727-168">Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="fa727-168">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id='FILL-IN-HERE')

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="fa727-169">Vytvoření skupiny prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="fa727-169">Create an Azure Resource Group</span></span>

<span data-ttu-id="fa727-170">Pomocí následujícího fragmentu kódu vytvořte skupinu prostředků Azure:</span><span class="sxs-lookup"><span data-stu-id="fa727-170">Use the following code snippet to create an Azure Resource Group:</span></span>

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

## <a name="create-clients-and-data-lake-store-account"></a><span data-ttu-id="fa727-171">Vytvoření klientů a účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fa727-171">Create clients and Data Lake Store account</span></span>

<span data-ttu-id="fa727-172">Následující fragment kódu nejprve vytvoří klienta účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fa727-172">The following snippet first creates the Data Lake Store account client.</span></span> <span data-ttu-id="fa727-173">Objekt klienta použije k vytvoření účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fa727-173">It uses the client object to create a Data Lake Store account.</span></span> <span data-ttu-id="fa727-174">Nakonec vytvoří objekt klienta systému souborů.</span><span class="sxs-lookup"><span data-stu-id="fa727-174">Finally, the snippet creates a filesystem client object.</span></span>

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

## <a name="list-the-data-lake-store-accounts"></a><span data-ttu-id="fa727-175">Výpis účtů Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fa727-175">List the Data Lake Store accounts</span></span>

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="create-a-directory"></a><span data-ttu-id="fa727-176">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="fa727-176">Create a directory</span></span>

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a><span data-ttu-id="fa727-177">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="fa727-177">Upload a file</span></span>


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a><span data-ttu-id="fa727-178">Stažení souboru</span><span class="sxs-lookup"><span data-stu-id="fa727-178">Download a file</span></span>

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a><span data-ttu-id="fa727-179">Odstranění adresáře</span><span class="sxs-lookup"><span data-stu-id="fa727-179">Delete a directory</span></span>

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="see-also"></a><span data-ttu-id="fa727-180">Viz také</span><span class="sxs-lookup"><span data-stu-id="fa727-180">See also</span></span>

- [<span data-ttu-id="fa727-181">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fa727-181">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
- [<span data-ttu-id="fa727-182">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fa727-182">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [<span data-ttu-id="fa727-183">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fa727-183">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
- [<span data-ttu-id="fa727-184">Referenční dokumentace sady SDK rozhraní .NET služby Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fa727-184">Data Lake Store .NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt581387.aspx)
- [<span data-ttu-id="fa727-185">Referenční dokumentace architektury REST služby Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fa727-185">Data Lake Store REST Reference</span></span>](https://msdn.microsoft.com/library/mt693424.aspx)
