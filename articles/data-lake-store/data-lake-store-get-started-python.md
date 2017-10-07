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
# <a name="get-started-with-azure-data-lake-store-using-python"></a>Začínáme s Azure Data Lake Store pomocí jazyka Python

> [!div class="op_single_selector"]
> * [Azure Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Zjistěte, jak toouse hello Python SDK pro Azure a Azure Data Lake Store tooperform základních operací, jako vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Požadavky

* **Python**. Python si můžete stáhnout [tady](https://www.python.org/downloads/). Tento článek používá Python verze 3.5.2.

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Vytvoření aplikace Azure Active Directory**. Hello Azure AD aplikace tooauthenticate hello Data Lake Store aplikaci můžete používat s Azure AD. Existují různé přístupy tooauthenticate s Azure AD, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**. Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).

## <a name="install-hello-modules"></a>Instalace modulů hello

toowork s Data Lake Store pomocí jazyka Python, musíte tooinstall tři moduly.

* Hello `azure-mgmt-resource` modulu. Zahrnuje moduly Azure pro Active Directory atd.
* Hello `azure-mgmt-datalake-store` modulu. To zahrnuje operace správy účtů Azure Data Lake Store hello. Další informace o tomto modulu najdete v [referenčních informacích k modulu Azure Data Lake Store Management](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).
* Hello `azure-datalake-store` modulu. To zahrnuje operace systému souborů hello Azure Data Lake Store. Další informace o tomto modulu najdete v [referenčních informacích k modulu Azure Data Lake Store Filesystem](http://azure-datalake-store.readthedocs.io/en/latest/).

Použijte následující příkazy tooinstall hello moduly hello.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Vytvoření nové aplikace v Pythonu

1. V hello IDE zvoleného vytvořte novou aplikaci Python, například **mysample.py**.

2. Přidejte následující řádky tooimport hello požadované moduly hello

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

3. Uložte změny toomysample.py.

## <a name="authentication"></a>Authentication

V této části se věnuje hello různé způsoby tooauthenticate s Azure AD. Hello možnosti, které jsou k dispozici jsou následující:

* Ověřování koncových uživatelů
* Ověřování služba-služba
* Ověřování pomocí služby Multi-Factor Authentication

Tyto možnosti ověřování je nutné použít jak pro správu účtů, tak i pro moduly správy systému souborů.

### <a name="end-user-authentication-for-account-management"></a>Ověřování koncového uživatele pro správu účtu

Pomocí této tooauthenticate s Azure AD pro operace správy účtů (Vytvoření nebo odstranění účtu Data Lake Store, atd.). Je třeba zadat uživatelské jméno a heslo uživatele Azure AD. Všimněte si, že tento uživatel hello by se neměla konfigurovat službu Multi-Factor Authentication.

    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="end-user-authentication-for-filesystem-operations"></a>Ověřování koncového uživatele pro operace se systémem souborů

Pomocí této tooauthenticate s Azure AD pro operace systému souborů (vytvořit složku, nahrávání souborů atd.). Tento kód použijte se stávající aplikací **nativního klienta** Azure AD. Zadejte pověření pro uživatele Hello Azure AD by se neměla konfigurovat službu Multi-Factor Authentication.

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Ověřování služba-služba s tajným klíčem klienta pro správu účtu

Pomocí této tooauthenticate s Azure AD pro operace správy účtů (Vytvoření nebo odstranění účtu Data Lake Store, atd.). Následující fragment kódu Hello lze použít tooauthenticate aplikace interaktivně, pomocí hello tajný klíč klienta pro hlavní aplikaci / service. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

### <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a>Ověřování služba-služba s tajným klíčem klienta pro operace se systémem souborů

Pomocí této tooauthenticate s Azure AD pro operace systému souborů (vytvořit složku, nahrávání souborů atd.). Následující fragment kódu Hello lze použít tooauthenticate aplikace interaktivně, pomocí hello tajný klíč klienta pro hlavní aplikaci / service. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

### <a name="multi-factor-authentication-for-account-management"></a>Vícefaktorové ověřování pro správu účtu

Pomocí této tooauthenticate s Azure AD pro operace správy účtů (Vytvoření nebo odstranění účtu Data Lake Store, atd.). Hello následující fragment kódu může být použit tooauthenticate aplikace pomocí služby Multi-Factor authentication. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.

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

### <a name="multi-factor-authentication-for-filesystem-management"></a>Vícefaktorové ověřování pro správu systému souborů

Pomocí této tooauthenticate s Azure AD pro operace systému souborů (vytvořit složku, nahrávání souborů atd.). Hello následující fragment kódu může být použit tooauthenticate aplikace pomocí služby Multi-Factor authentication. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.

    token = lib.auth(tenant_id='FILL-IN-HERE')

## <a name="create-an-azure-resource-group"></a>Vytvoření skupiny prostředků Azure

Použijte následující toocreate fragmentu kódu skupiny prostředků Azure hello:

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

## <a name="create-clients-and-data-lake-store-account"></a>Vytvoření klientů a účtu Data Lake Store

Následující fragment kódu nejprve Hello vytvoří klienta pro účet Data Lake Store hello. Používá hello klienta objekt toocreate účtu Data Lake Store. Nakonec hello fragment kódu vytvoří objekt systému souborů klienta.

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

## <a name="list-hello-data-lake-store-accounts"></a>Seznam účtů Data Lake Store hello

    ## List hello existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="create-a-directory"></a>Vytvoření adresáře

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a>Nahrání souboru


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a>Stažení souboru

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a>Odstranění adresáře

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="see-also"></a>Viz také

- [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
- [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Referenční dokumentace sady SDK rozhraní .NET služby Data Lake Store](https://msdn.microsoft.com/library/mt581387.aspx)
- [Referenční dokumentace architektury REST služby Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx)
