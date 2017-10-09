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
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Začínáme s Azure Data Lake Store pomocí rozhraní REST API
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

V tomto článku se dozvíte, jak toouse rozhraní REST API WebHDFS a rozhraní API REST Data Lake Store tooperform účtu správu, jakož i operací systému souborů v Azure Data Lake Store. Azure Data Lake Store zpřístupňuje vlastní rozhraní REST API pro operace správy účtů. Data Lake Store je nicméně kompatibilní s ekosystémy HDFS a Hadoop, a proto podporuje použití rozhraní REST API WebHDFS pro operace systému souborů.

> [!NOTE]
> Podrobné informace o podporovaných hello REST API pro Data Lake Store najdete v tématu [Azure Data Lake Store REST referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/mt693424.aspx).
> 
> 

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Vytvoření aplikace Azure Active Directory**. Hello Azure AD aplikace tooauthenticate hello Data Lake Store aplikaci můžete používat s Azure AD. Existují různé přístupy tooauthenticate s Azure AD, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**. Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).
* [cURL](http://curl.haxx.se/). Tento článek používá cURL toodemonstrate, jakým způsobem volá toomake REST API vůči účtu Data Lake Store.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak můžu ověřovat pomocí služby Azure Active Directory?
Můžete použít dva přístupy tooauthenticate pomocí služby Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Ověření koncového uživatele (interaktivní)
V tomto scénáři aplikace hello vyzve uživatele toolog hello v a všechny operace hello se provádějí v kontextu hello hello uživatele. Proveďte následující kroky pro interaktivní ověřování hello.

1. Prostřednictvím aplikace přesměrování toohello uživatele hello následující adresu URL:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI > musí toobe kódováním pro použití v adrese URL. Pro adresu https://localhost proto použijte zápis `https%3A%2F%2Flocalhost`).
   > 
   > 
   
    Za účelem hello tohoto kurzu můžete nahradit zástupné hodnoty hello v adrese URL hello výše a vložte jej do adresního řádku webového prohlížeče. Bude přesměrované tooauthenticate pomocí přihlášení Azure. Jakmile úspěšně přihlásíte, hello odpovědi se zobrazí v adresním řádku prohlížeče hello. odpověď Hello bude v hello následující formát:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Zaznamenejte hello autorizační kód z odpovědi hello. V tomto kurzu můžete zkopírovat hello autorizační kód z panelu Adresa hello hello webového prohlížeče a předat jej v hello POST požadavek toohello koncovému bodu tokenu, jak je uvedeno níže:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
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
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Další informace o interaktivním ověřování uživatelů najdete v tématu [Tok poskytování autorizačních kódů](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Ověřování služba-služba (neinteraktivní)
V tomto scénáři hello hello aplikace poskytuje svoje vlastní přihlašovací údaje tooperform hello operace. V takovém případě musíte vydat požadavek POST jako hello níže. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Hello výstup tohoto požadavku bude obsahovat autorizační token (odlišené `access-token` ve výstupu hello níže), potom budete předávat s voláními rozhraní REST API. Tento ověřovací token si uložte do textového souboru, protože ho budete později v tomto článku potřebovat.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Tento článek používá hello **neinteraktivní** přístup. Další informace o neinteraktivním (volání služba služba) najdete v tématu [služba pomocí přihlašovacích údajů volání tooservice](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Vytvoření účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API hello definované [zde](https://msdn.microsoft.com/library/mt694078.aspx).

Použijte následující příkaz cURL hello. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

V hello výše příkazu nahraďte položku \< `REDACTED` \> s tokenem autorizace hello jste získali dříve. Hello datová část požadavku tohoto příkazu se nachází v hello **input.json** soubor, který je k dispozici pro hello `-d` parametr výše. Hello obsah souboru Input.JSON vypadá hello vypadat hello následující:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a>Vytváření složek v účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Použijte následující příkaz cURL hello. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

V hello výše příkazu nahraďte položku \< `REDACTED` \> s tokenem autorizace hello jste získali dříve. Tento příkaz vytvoří adresář s názvem **mytempdir** hello kořenové složce účtu Data Lake Store.

Pokud po úspěšném dokončení operace hello se zobrazí odpověď takto:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Zobrazení seznamu složek v účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Použijte následující příkaz cURL hello. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

V hello výše příkazu nahraďte položku \< `REDACTED` \> s tokenem autorizace hello jste získali dříve.

Pokud po úspěšném dokončení operace hello se zobrazí odpověď takto:

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

## <a name="upload-data-into-a-data-lake-store-account"></a>Nahrání dat do účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Použijte následující příkaz cURL hello. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

V hello výše syntaxe **-T** parametr je hello umístění souboru hello nahrávání.

výstup Hello je podobné toohello následující:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a>Čtení dat z účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Čtení dat z účtu Data Lake Store je proces, který obsahuje dva kroky.

* Nejdřív odešlete požadavek GET na koncový bod hello `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Tato možnost vrátí umístění toosubmit hello další požadavek GET na.
* Potom odešlete požadavek GET hello koncový bod hello `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Tato akce zobrazí hello obsah souboru hello.

Ale vzhledem k tomu, že není žádný rozdíl v hello vstupní parametry mezi hello nejprve a hello druhé krok, můžete použít hello `-L` parametr toosubmit hello prvního požadavku. `-L`možnost v podstatě kombinuje dva požadavky do jednoho a bude cURL zopakuje požadavek hello na nové umístění hello. Nakonec hello výstup ze všech volání požadavků hello se zobrazuje, jako vidíte níže. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Měli byste vidět výstup podobný toohello následující:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Přejmenování souboru v účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Použijte následující hello cURL toorename příkaz do souboru. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Měli byste vidět výstup podobný toohello následující:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Odstranění souboru z účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API WebHDFS hello definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Použijte následující hello cURL toodelete příkaz do souboru. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Měli byste vidět výstup jako hello následující:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Odstranění účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API hello definované [zde](https://msdn.microsoft.com/library/mt694075.aspx).

Použijte následující hello cURL příkaz toodelete účtu Data Lake Store. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Měli byste vidět výstup jako hello následující:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Viz také
* [Aplikace typu Open Source pro velké objemy dat kompatibilní s Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md)

