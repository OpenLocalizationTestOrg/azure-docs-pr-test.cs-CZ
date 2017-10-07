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
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Začínáme se službou Azure Data Lake Analytics pomocí rozhraní REST API
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Zjistěte, jak toouse rozhraní REST API WebHDFS a rozhraní API REST Data Lake Analytics toomanage Data Lake Analytics účty, úlohy a katalogu. 

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Vytvoření aplikace Azure Active Directory**. Hello Azure AD aplikace tooauthenticate hello Data Lake Analytics aplikaci můžete používat s Azure AD. Existují různé přístupy tooauthenticate s Azure AD, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**. Pokyny a další informace o tooauthenticate, najdete v části [ověřit s Data Lake Analytics pomocí Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).
* [cURL](http://curl.haxx.se/). Tento článek používá cURL toodemonstrate, jakým způsobem volá toomake REST API vůči účtu Data Lake Analytics.

## <a name="authenticate-with-azure-active-directory"></a>Ověřování pomocí Azure Active Directory
Existují dvě metody ověřování pomocí Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Ověření koncového uživatele (interaktivní)
Pomocí této metody aplikace vyzve uživatele toolog hello v a všechny operace hello se provádějí v kontextu hello hello uživatele. 

Pokud chcete zavést interaktivní ověřování, postupujte následovně:

1. Prostřednictvím aplikace přesměrování toohello uživatele hello následující adresu URL:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI > musí toobe kódováním pro použití v adrese URL. Pro adresu https://localhost proto použijte zápis `https%3A%2F%2Flocalhost`).
   > 
   > 
   
    Za účelem hello tohoto kurzu můžete nahradit zástupné hodnoty hello v adrese URL hello výše a vložte jej do adresního řádku webového prohlížeče. Bude přesměrované tooauthenticate pomocí přihlášení Azure. Po úspěšném přihlášení, hello odpovědi se zobrazí v adresním řádku prohlížeče hello. odpověď Hello bude v hello následující formát:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Zaznamenejte hello autorizační kód z odpovědi hello. V tomto kurzu můžete zkopírovat hello autorizační kód z panelu Adresa hello hello webového prohlížeče a předat jej v hello POST požadavek toohello koncovému bodu tokenu, jak je uvedeno níže:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > V takovém případě hello \<REDIRECT-URI > nemusí být zakódován.
   > 
   > 
3. odpověď Hello je objekt JSON, který obsahuje přístupový token (například `"access_token": "<ACCESS_TOKEN>"`) a obnovovací token (například `"refresh_token": "<REFRESH_TOKEN>"`). Vaše aplikace používá hello přístupový token při přístupu k Azure Data Lake Store a hello aktualizace tokenu tooget dalšího přístupového tokenu když vyprší platnost přístupového tokenu.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. Když vyprší platnost hello přístupový token, můžete požádat o nový přístupový token pomocí hello aktualizace tokenu, jak je uvedeno níže:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Další informace o interaktivním ověřování uživatelů najdete v tématu [Tok poskytování autorizačních kódů](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Ověřování služba-služba (neinteraktivní)
Pomocí této metody aplikace poskytuje svoje vlastní přihlašovací údaje při tooperform hello operace. V takovém případě musíte vydat požadavek POST jako hello níže: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Hello výstup tohoto požadavku bude obsahovat autorizační token (odlišené `access-token` ve výstupu hello níže), potom budete předávat s voláními rozhraní REST API. Tento ověřovací token si uložte do textového souboru, protože ho budete později v tomto článku potřebovat.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Tento článek používá hello **neinteraktivní** přístup. Další informace o neinteraktivním (volání služba služba) najdete v tématu [služba pomocí přihlašovacích údajů volání tooservice](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics
Před vytvořením účtu Data Lake Analytics je nutné vytvořit skupinu prostředků Azure a účet Data Lake Store.  Viz [Vytvoření účtu Data Lake Store](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Dobrý den, jak následující příkaz ukazuje Curl toocreate účet:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `AzureSubscriptionID` \> s svoje ID předplatného \< `AzureResourceGroupName` \> s existující prostředek Azure Název skupiny a \< `NewAzureDataLakeAnalyticsAccountName` \> s novým názvem účtu Data Lake Analytics. Hello datová část požadavku tohoto příkazu se nachází v hello **CreateDatalakeAnalyticsAccountRequest.json** soubor, který je k dispozici pro hello `-d` parametr výše. Hello obsah souboru Input.JSON vypadá hello vypadat hello následující:

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


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Zobrazení seznamu účtů Data Lake Analytics v předplatném
Následující příkaz Curl Hello ukazuje, jak toolist účty v předplatném:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `AzureSubscriptionID` \> s vaším ID předplatného. výstup Hello je podobná:

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

## <a name="get-information-about-a-data-lake-analytics-account"></a>Získání informací o účtu Data Lake Analytics
Dobrý den, jak následující příkaz ukazuje Curl tooget informace účtu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `AzureSubscriptionID` \> s svoje ID předplatného \< `AzureResourceGroupName` \> s existující prostředek Azure Název skupiny a \< `DataLakeAnalyticsAccountName` \> s názvem hello stávajícího účtu Data Lake Analytics. výstup Hello je podobná:

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

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Zobrazení seznamu služeb Data Lake Store pro účet Data Lake Analytics
Následující příkaz Curl Hello ukazuje, jak ukládá toolist Data Lake účtu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `AzureSubscriptionID` \> s svoje ID předplatného \< `AzureResourceGroupName` \> s existující prostředek Azure Název skupiny a \< `DataLakeAnalyticsAccountName` \> s názvem hello stávajícího účtu Data Lake Analytics. výstup Hello je podobná:

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

## <a name="submit-u-sql-jobs"></a>Odesílání úloh U-SQL
Dobrý den, jak následující příkaz ukazuje Curl úlohy toosubmit U-SQL:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Nahraďte \< `REDACTED` \> s tokenem autorizace hello, \< `DataLakeAnalyticsAccountName` \> s názvem hello stávajícího účtu Data Lake Analytics. Hello datová část požadavku tohoto příkazu se nachází v hello **SubmitADLAJob.json** soubor, který je k dispozici pro hello `-d` parametr výše. Hello obsah souboru Input.JSON vypadá hello vypadat hello následující:

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

výstup Hello je podobná:

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


## <a name="list-u-sql-jobs"></a>Zobrazení seznamu úloh U-SQL
Dobrý den, jak následující příkaz ukazuje Curl úlohy toolist U-SQL:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Nahraďte \< `REDACTED` \> s tokenem autorizace hello, a \< `DataLakeAnalyticsAccountName` \> s názvem hello stávajícího účtu Data Lake Analytics. 

výstup Hello je podobná:

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


## <a name="get-catalog-items"></a>Získání položek katalogu
Následující příkaz Curl Hello ukazuje, jak hello tooget hello databáze z katalogu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

výstup Hello je podobná:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Viz také
* toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* tooget práce s vývojem aplikací U-SQL najdete v části [skriptů vyvíjet U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).
* Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).
* tooget uvádí přehled Data Lake Analytics najdete v části [přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).
* toosee hello stejný kurz pomocí jiných nástrojů, klikněte na selektory karet hello na hello horní části stránky hello.

