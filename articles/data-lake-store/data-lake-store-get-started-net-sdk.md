---
title: aaaUse hello .NET SDK toodevelop aplikace v Azure Data Lake Store | Microsoft Docs
description: "Použít toocreate .NET SDK služby Azure Data Lake Store účtu Data Lake Store a provádění základních operací v Data Lake Store hello"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: nitinme
ms.openlocfilehash: cb3a1dfb2f6379f728069d66b0ee77ce0f838fe7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Začínáme s Azure Data Lake Store pomocí sady .NET SDK
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

Zjistěte, jak toouse hello [.NET SDK služby Azure Data Lake Store](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) tooperform základních operací, jako vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Požadavky
* **Visual Studio 2013, 2015 nebo 2017**. níže uvedené Hello pokyny používají Visual Studio 2015 Update 2.

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Účet Azure Data Lake Store**. Pokyny najdete v části toocreate na účet, [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)

* **Vytvoření aplikace Azure Active Directory**. Hello Azure AD aplikace tooauthenticate hello Data Lake Store aplikaci můžete používat s Azure AD. Existují různé přístupy tooauthenticate s Azure AD, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**. Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Vytvoření aplikace .NET
1. Otevřete Visual Studio a vytvořte konzolovou aplikaci.
2. Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.
3. Z **nový projekt**, zadejte nebo vyberte hello následující hodnoty:

   | Vlastnost | Hodnota |
   | --- | --- |
   | Kategorie |Šablony/Visual C#/Windows |
   | Šablona |Konzolová aplikace |
   | Name (Název) |VytvořeníAplikaceADL |
4. Klikněte na tlačítko **OK** toocreate hello projektu.
5. Přidáte projekt tooyour balíčky Nuget hello.

   1. Klikněte pravým tlačítkem na název projektu hello v Průzkumníku řešení hello a klikněte na tlačítko **spravovat balíčky NuGet**.
   2. V hello **Správce balíčků Nuget** kartě, ujistěte se, že **zdroj balíčku** je nastaven příliš**nuget.org** a že **zahrnout předběžné verze** zaškrtávací políčko je vybrán.
   3. Vyhledejte a nainstalujte následující balíčky NuGet hello:

      * `Microsoft.Azure.Management.DataLake.Store` – Tento kurz používá verzi v2.1.3-preview.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` – Tento kurz používá verzi v2.2.12.

        ![Přidání zdroje Nuget](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Vytvoření nového účtu Azure Data Lake")
   4. Zavřít hello **Správce balíčků Nuget**.
6. Otevřete **Program.cs**, odstraňte stávající kód hello a potom vložte následující příkazy tooadd odkazy toonamespaces hello.

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. Deklarujte proměnné hello, jak je uvedeno níže a zadejte hodnoty hello název Data Lake Store a hello název skupiny prostředků, který již existuje. Taky se ujistěte, že hello místní cesta a název souboru, které tady zadáte, musí existovat v počítači hello. Přidejte následující fragment kódu po deklarace oboru názvů hello hello.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with hello name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with hello name of hello resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

Ve zbývající části článku hello hello uvidíte, jak toouse hello dostupné .NET metody tooperform operace jako je například ověřování, nahrávání souborů atd.

## <a name="authentication"></a>Authentication

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Pokud používáte ověřování koncového uživatele (doporučeno pro tento kurz)

Použít s existující tooauthenticate nativní aplikace Azure AD vaší aplikace **interaktivně**, což znamená, bude vyzván tooenter vaše přihlašovací údaje Azure.

Následující fragment hello pro snadné použití, používá výchozí hodnoty pro ID klienta a přesměrování identifikátor URI, který bude fungovat s jakéhokoli předplatného Azure. toohelp rychlejší dokončení tohoto kurzu, doporučujeme, abyste že tento postup použijte. V následující fragment hello stačí zadejte hello hodnotu pro vaše ID klienta. Můžete načíst pomocí hello pokynů uvedených v [vytvoření aplikace Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

    // User login via interactive popup
    // Use hello client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with hello user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

Pár věcí tooknow o výše tento fragment kódu:

* toohelp rychlejší dokončení hello kurzu, používá tento fragment kódu na Azure AD domény a klienta ID, která je k dispozici ve výchozím nastavení pro všechny odběry služby Azure. Můžete tedy **použít ve své aplikaci tento fragment kódu bez jakýchkoli úprav**.
* Ale pokud chcete toouse vlastní doménu služby Azure AD a ID klienta aplikace, musíte vytvořit nativní aplikaci Azure AD a poté použijte hello Azure AD ID klienta ID klienta a identifikátor URI přesměrování pro aplikaci hello jste vytvořili. Pokyny najdete v tématu [Vytvoření aplikace Active Directory pro ověřování koncového uživatele pomocí služby Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md).

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Pokud používáte ověřování služba-služba s tajným klíčem klienta
Hello následující fragment kódu může být použité tooauthenticate aplikace **neinteraktivně**, pomocí tajný klíč klienta hello / klíč pro hlavní aplikaci / service. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace. Pokyny, jak toocreate hello Azure AD webové aplikace a jak tooretrieve hello ID klienta a tajný klíč klienta, které jsou potřeba v hello fragmentu kódu níže, najdete v části [vytvoření aplikace Active Directory pro ověřování service-to-service s daty Lake Store](data-lake-store-authenticate-using-active-directory.md).

    // Service principal / appplication authentication with client secret / key
    // Use hello client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Pokud používáte ověřování služba-služba s certifikátem

Jako třetí volby, hello následující fragment kódu lze použít tooauthenticate aplikace **neinteraktivně**, pomocí hello certifikátu pro aplikaci Azure Active Directory / service principal. Použijte tento fragment kódu se stávající [aplikací Azure AD s certifikáty](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    // Service principal / application authentication with certificate
    // Use hello client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a>Vytvoření objektů klienta
Hello následující fragment vytváří hello účtu Data Lake Store a systému souborů klienta objekty, které se používají tooissue požadavky toohello služby.

    // Create client objects and set hello subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Zobrazení seznamu všech účtů Data Lake Store v rámci předplatného
Hello následující fragment kódu obsahuje seznam všech účtů Data Lake Store v rámci konkrétního předplatného Azure.

    // List all ADLS accounts within hello subscription
    public static async Task<List<DataLakeStoreAccount>> ListAdlStoreAccounts()
    {
        var response = await _adlsClient.Account.ListAsync();
        var accounts = new List<DataLakeStoreAccount>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="create-a-directory"></a>Vytvoření adresáře
Následující fragment kódu ukazuje Hello `CreateDirectory` metody, které můžete použít toocreate adresáře v rámci účtu Data Lake Store.

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Nahrání souboru
Následující fragment kódu ukazuje Hello `UploadFile` metody, které můžete použít tooupload soubory tooa účtu Data Lake Store.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

Hello SDK podporuje rekurzivní nahrávání a stahování mezi cestu k místnímu souboru a cesty k souboru Data Lake Store.    

## <a name="get-file-or-directory-info"></a>Získání informací o souboru nebo adresáři
Následující fragment kódu ukazuje Hello `GetItemInfo` metoda používané tooretrieve informace o souboru nebo adresáři dostupném v Data Lake Store.

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Zobrazení seznamu souboru nebo adresářů
Následující fragment kódu ukazuje Hello `ListItem` , můžete použít toolist hello souborů a adresářů v účtu Data Lake Store.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Řetězení souborů
Následující fragment kódu ukazuje Hello `ConcatenateFiles` metoda, že používáte tooconcatenate soubory.

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-tooa-file"></a>Připojení souboru tooa
Následující fragment kódu ukazuje Hello `AppendToFile` metoda, kterou použijete připojit datový soubor tooa už uložený v účtu Data Lake Store.

    // Append toofile
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a>Stažení souboru
Následující fragment kódu ukazuje Hello `DownloadFile` metoda použít toodownload soubor z účtu Data Lake Store.

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
         _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath);
    }

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Referenční dokumentace sady SDK rozhraní .NET služby Data Lake Store](https://docs.microsoft.com/dotnet/api/?view=azuremgmtdatalakestore-2.1.0-preview&term=DataLake.Store)
* [Referenční dokumentace architektury REST služby Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx)
