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
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a><span data-ttu-id="42d1f-103">Začínáme s Azure Data Lake Store pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="42d1f-103">Get started with Azure Data Lake Store using .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="42d1f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="42d1f-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="42d1f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="42d1f-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="42d1f-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="42d1f-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="42d1f-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="42d1f-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="42d1f-108">REST API</span><span class="sxs-lookup"><span data-stu-id="42d1f-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="42d1f-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="42d1f-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="42d1f-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="42d1f-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="42d1f-111">Python</span><span class="sxs-lookup"><span data-stu-id="42d1f-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="42d1f-112">Zjistěte, jak toouse hello [.NET SDK služby Azure Data Lake Store](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) tooperform základních operací, jako vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="42d1f-112">Learn how toouse hello [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) tooperform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42d1f-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="42d1f-113">Prerequisites</span></span>
* <span data-ttu-id="42d1f-114">**Visual Studio 2013, 2015 nebo 2017**.</span><span class="sxs-lookup"><span data-stu-id="42d1f-114">**Visual Studio 2013, 2015, or 2017**.</span></span> <span data-ttu-id="42d1f-115">níže uvedené Hello pokyny používají Visual Studio 2015 Update 2.</span><span class="sxs-lookup"><span data-stu-id="42d1f-115">hello instructions below use Visual Studio 2015 Update 2.</span></span>

* <span data-ttu-id="42d1f-116">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="42d1f-116">**An Azure subscription**.</span></span> <span data-ttu-id="42d1f-117">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="42d1f-117">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="42d1f-118">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="42d1f-118">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="42d1f-119">Pokyny najdete v části toocreate na účet, [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="42d1f-119">For instructions on how toocreate an account, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>

* <span data-ttu-id="42d1f-120">**Vytvoření aplikace Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="42d1f-120">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="42d1f-121">Hello Azure AD aplikace tooauthenticate hello Data Lake Store aplikaci můžete používat s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="42d1f-121">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="42d1f-122">Existují různé přístupy tooauthenticate s Azure AD, které jsou **ověřování koncového uživatele** nebo **service-to-service ověřování**.</span><span class="sxs-lookup"><span data-stu-id="42d1f-122">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="42d1f-123">Pokyny a další informace o tooauthenticate, najdete v části [ověřování koncového uživatele](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Service-to-service ověřování](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="42d1f-123">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="create-a-net-application"></a><span data-ttu-id="42d1f-124">Vytvoření aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="42d1f-124">Create a .NET application</span></span>
1. <span data-ttu-id="42d1f-125">Otevřete Visual Studio a vytvořte konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="42d1f-125">Open Visual Studio and create a console application.</span></span>
2. <span data-ttu-id="42d1f-126">Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="42d1f-126">From hello **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="42d1f-127">Z **nový projekt**, zadejte nebo vyberte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="42d1f-127">From **New Project**, type or select hello following values:</span></span>

   | <span data-ttu-id="42d1f-128">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="42d1f-128">Property</span></span> | <span data-ttu-id="42d1f-129">Hodnota</span><span class="sxs-lookup"><span data-stu-id="42d1f-129">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="42d1f-130">Kategorie</span><span class="sxs-lookup"><span data-stu-id="42d1f-130">Category</span></span> |<span data-ttu-id="42d1f-131">Šablony/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="42d1f-131">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="42d1f-132">Šablona</span><span class="sxs-lookup"><span data-stu-id="42d1f-132">Template</span></span> |<span data-ttu-id="42d1f-133">Konzolová aplikace</span><span class="sxs-lookup"><span data-stu-id="42d1f-133">Console Application</span></span> |
   | <span data-ttu-id="42d1f-134">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="42d1f-134">Name</span></span> |<span data-ttu-id="42d1f-135">VytvořeníAplikaceADL</span><span class="sxs-lookup"><span data-stu-id="42d1f-135">CreateADLApplication</span></span> |
4. <span data-ttu-id="42d1f-136">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="42d1f-136">Click **OK** toocreate hello project.</span></span>
5. <span data-ttu-id="42d1f-137">Přidáte projekt tooyour balíčky Nuget hello.</span><span class="sxs-lookup"><span data-stu-id="42d1f-137">Add hello Nuget packages tooyour project.</span></span>

   1. <span data-ttu-id="42d1f-138">Klikněte pravým tlačítkem na název projektu hello v Průzkumníku řešení hello a klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="42d1f-138">Right-click hello project name in hello Solution Explorer and click **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="42d1f-139">V hello **Správce balíčků Nuget** kartě, ujistěte se, že **zdroj balíčku** je nastaven příliš**nuget.org** a že **zahrnout předběžné verze** zaškrtávací políčko je vybrán.</span><span class="sxs-lookup"><span data-stu-id="42d1f-139">In hello **Nuget Package Manager** tab, make sure that **Package source** is set too**nuget.org** and that **Include prerelease** check box is selected.</span></span>
   3. <span data-ttu-id="42d1f-140">Vyhledejte a nainstalujte následující balíčky NuGet hello:</span><span class="sxs-lookup"><span data-stu-id="42d1f-140">Search for and install hello following NuGet packages:</span></span>

      * <span data-ttu-id="42d1f-141">`Microsoft.Azure.Management.DataLake.Store` – Tento kurz používá verzi v2.1.3-preview.</span><span class="sxs-lookup"><span data-stu-id="42d1f-141">`Microsoft.Azure.Management.DataLake.Store` - This tutorial uses v2.1.3-preview.</span></span>
      * <span data-ttu-id="42d1f-142">`Microsoft.Rest.ClientRuntime.Azure.Authentication` – Tento kurz používá verzi v2.2.12.</span><span class="sxs-lookup"><span data-stu-id="42d1f-142">`Microsoft.Rest.ClientRuntime.Azure.Authentication` - This tutorial uses v2.2.12.</span></span>

        <span data-ttu-id="42d1f-143">![Přidání zdroje Nuget](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Vytvoření nového účtu Azure Data Lake")</span><span class="sxs-lookup"><span data-stu-id="42d1f-143">![Add a Nuget source](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Create a new Azure Data Lake account")</span></span>
   4. <span data-ttu-id="42d1f-144">Zavřít hello **Správce balíčků Nuget**.</span><span class="sxs-lookup"><span data-stu-id="42d1f-144">Close hello **Nuget Package Manager**.</span></span>
6. <span data-ttu-id="42d1f-145">Otevřete **Program.cs**, odstraňte stávající kód hello a potom vložte následující příkazy tooadd odkazy toonamespaces hello.</span><span class="sxs-lookup"><span data-stu-id="42d1f-145">Open **Program.cs**, delete hello existing code, and then include hello following statements tooadd references toonamespaces.</span></span>

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. <span data-ttu-id="42d1f-146">Deklarujte proměnné hello, jak je uvedeno níže a zadejte hodnoty hello název Data Lake Store a hello název skupiny prostředků, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="42d1f-146">Declare hello variables as shown below, and provide hello values for Data Lake Store name and hello resource group name that already exist.</span></span> <span data-ttu-id="42d1f-147">Taky se ujistěte, že hello místní cesta a název souboru, které tady zadáte, musí existovat v počítači hello.</span><span class="sxs-lookup"><span data-stu-id="42d1f-147">Also, make sure hello local path and file name you provide here must exist on hello computer.</span></span> <span data-ttu-id="42d1f-148">Přidejte následující fragment kódu po deklarace oboru názvů hello hello.</span><span class="sxs-lookup"><span data-stu-id="42d1f-148">Add hello following code snippet after hello namespace declarations.</span></span>

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

<span data-ttu-id="42d1f-149">Ve zbývající části článku hello hello uvidíte, jak toouse hello dostupné .NET metody tooperform operace jako je například ověřování, nahrávání souborů atd.</span><span class="sxs-lookup"><span data-stu-id="42d1f-149">In hello remaining sections of hello article, you can see how toouse hello available .NET methods tooperform operations such as authentication, file upload, etc.</span></span>

## <a name="authentication"></a><span data-ttu-id="42d1f-150">Authentication</span><span class="sxs-lookup"><span data-stu-id="42d1f-150">Authentication</span></span>

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a><span data-ttu-id="42d1f-151">Pokud používáte ověřování koncového uživatele (doporučeno pro tento kurz)</span><span class="sxs-lookup"><span data-stu-id="42d1f-151">If you are using end-user authentication (recommended for this tutorial)</span></span>

<span data-ttu-id="42d1f-152">Použít s existující tooauthenticate nativní aplikace Azure AD vaší aplikace **interaktivně**, což znamená, bude vyzván tooenter vaše přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="42d1f-152">Use this with an existing Azure AD native application tooauthenticate your application **interactively**, which means you will be prompted tooenter your Azure credentials.</span></span>

<span data-ttu-id="42d1f-153">Následující fragment hello pro snadné použití, používá výchozí hodnoty pro ID klienta a přesměrování identifikátor URI, který bude fungovat s jakéhokoli předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="42d1f-153">For ease of use, hello snippet below uses default values for client ID and redirect URI that will work with any Azure subscription.</span></span> <span data-ttu-id="42d1f-154">toohelp rychlejší dokončení tohoto kurzu, doporučujeme, abyste že tento postup použijte.</span><span class="sxs-lookup"><span data-stu-id="42d1f-154">toohelp you complete this tutorial faster, we recommend you use this approach.</span></span> <span data-ttu-id="42d1f-155">V následující fragment hello stačí zadejte hello hodnotu pro vaše ID klienta.</span><span class="sxs-lookup"><span data-stu-id="42d1f-155">In hello snippet below, just provide hello value for your tenant ID.</span></span> <span data-ttu-id="42d1f-156">Můžete načíst pomocí hello pokynů uvedených v [vytvoření aplikace Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="42d1f-156">You can retrieve it using hello instructions provided at [Create an Active Directory Application](data-lake-store-end-user-authenticate-using-active-directory.md).</span></span>

    // User login via interactive popup
    // Use hello client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with hello user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

<span data-ttu-id="42d1f-157">Pár věcí tooknow o výše tento fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="42d1f-157">A couple of things tooknow about this snippet above:</span></span>

* <span data-ttu-id="42d1f-158">toohelp rychlejší dokončení hello kurzu, používá tento fragment kódu na Azure AD domény a klienta ID, která je k dispozici ve výchozím nastavení pro všechny odběry služby Azure.</span><span class="sxs-lookup"><span data-stu-id="42d1f-158">toohelp you complete hello tutorial faster, this snippet uses an an Azure AD domain and client ID that is available by default for all Azure subscriptions.</span></span> <span data-ttu-id="42d1f-159">Můžete tedy **použít ve své aplikaci tento fragment kódu bez jakýchkoli úprav**.</span><span class="sxs-lookup"><span data-stu-id="42d1f-159">So, you can **use this snippet as-is in your application**.</span></span>
* <span data-ttu-id="42d1f-160">Ale pokud chcete toouse vlastní doménu služby Azure AD a ID klienta aplikace, musíte vytvořit nativní aplikaci Azure AD a poté použijte hello Azure AD ID klienta ID klienta a identifikátor URI přesměrování pro aplikaci hello jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="42d1f-160">However, if you do want toouse your own Azure AD domain and application client ID, you must create an Azure AD native application and then use hello Azure AD tenant ID, client ID, and redirect URI for hello application you created.</span></span> <span data-ttu-id="42d1f-161">Pokyny najdete v tématu [Vytvoření aplikace Active Directory pro ověřování koncového uživatele pomocí služby Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="42d1f-161">See [Create an Active Directory Application for end-user authentication with Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md) for instructions.</span></span>

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a><span data-ttu-id="42d1f-162">Pokud používáte ověřování služba-služba s tajným klíčem klienta</span><span class="sxs-lookup"><span data-stu-id="42d1f-162">If you are using service-to-service authentication with client secret</span></span>
<span data-ttu-id="42d1f-163">Hello následující fragment kódu může být použité tooauthenticate aplikace **neinteraktivně**, pomocí tajný klíč klienta hello / klíč pro hlavní aplikaci / service.</span><span class="sxs-lookup"><span data-stu-id="42d1f-163">hello following snippet can be used tooauthenticate your application **non-interactively**, using hello client secret / key for an application / service principal.</span></span> <span data-ttu-id="42d1f-164">Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="42d1f-164">Use this with an existing Azure AD "Web App" Application.</span></span> <span data-ttu-id="42d1f-165">Pokyny, jak toocreate hello Azure AD webové aplikace a jak tooretrieve hello ID klienta a tajný klíč klienta, které jsou potřeba v hello fragmentu kódu níže, najdete v části [vytvoření aplikace Active Directory pro ověřování service-to-service s daty Lake Store](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="42d1f-165">For instructions on how toocreate hello Azure AD web application and how tooretrieve hello client ID and client secret required in hello snippet below, see [Create an Active Directory Application for service-to-service authentication with Data Lake Store](data-lake-store-authenticate-using-active-directory.md).</span></span>

    // Service principal / appplication authentication with client secret / key
    // Use hello client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a><span data-ttu-id="42d1f-166">Pokud používáte ověřování služba-služba s certifikátem</span><span class="sxs-lookup"><span data-stu-id="42d1f-166">If you are using service-to-service authentication with certificate</span></span>

<span data-ttu-id="42d1f-167">Jako třetí volby, hello následující fragment kódu lze použít tooauthenticate aplikace **neinteraktivně**, pomocí hello certifikátu pro aplikaci Azure Active Directory / service principal.</span><span class="sxs-lookup"><span data-stu-id="42d1f-167">As a third option, hello following snippet can be used tooauthenticate your application **non-interactively**, using hello certificate for an Azure Active Directory application / service principal.</span></span> <span data-ttu-id="42d1f-168">Použijte tento fragment kódu se stávající [aplikací Azure AD s certifikáty](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="42d1f-168">Use this with an existing [Azure AD Application with certificates](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

    // Service principal / application authentication with certificate
    // Use hello client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a><span data-ttu-id="42d1f-169">Vytvoření objektů klienta</span><span class="sxs-lookup"><span data-stu-id="42d1f-169">Create client objects</span></span>
<span data-ttu-id="42d1f-170">Hello následující fragment vytváří hello účtu Data Lake Store a systému souborů klienta objekty, které se používají tooissue požadavky toohello služby.</span><span class="sxs-lookup"><span data-stu-id="42d1f-170">hello following snippet creates hello Data Lake Store account and filesystem client objects, which are used tooissue requests toohello service.</span></span>

    // Create client objects and set hello subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a><span data-ttu-id="42d1f-171">Zobrazení seznamu všech účtů Data Lake Store v rámci předplatného</span><span class="sxs-lookup"><span data-stu-id="42d1f-171">List all Data Lake Store accounts within a subscription</span></span>
<span data-ttu-id="42d1f-172">Hello následující fragment kódu obsahuje seznam všech účtů Data Lake Store v rámci konkrétního předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="42d1f-172">hello following snippet lists all Data Lake Store accounts within a given Azure subscription.</span></span>

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

## <a name="create-a-directory"></a><span data-ttu-id="42d1f-173">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="42d1f-173">Create a directory</span></span>
<span data-ttu-id="42d1f-174">Následující fragment kódu ukazuje Hello `CreateDirectory` metody, které můžete použít toocreate adresáře v rámci účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="42d1f-174">hello following snippet shows a `CreateDirectory` method that you can use toocreate a directory within a Data Lake Store account.</span></span>

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a><span data-ttu-id="42d1f-175">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="42d1f-175">Upload a file</span></span>
<span data-ttu-id="42d1f-176">Následující fragment kódu ukazuje Hello `UploadFile` metody, které můžete použít tooupload soubory tooa účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="42d1f-176">hello following snippet shows an `UploadFile` method that you can use tooupload files tooa Data Lake Store account.</span></span>

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

<span data-ttu-id="42d1f-177">Hello SDK podporuje rekurzivní nahrávání a stahování mezi cestu k místnímu souboru a cesty k souboru Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="42d1f-177">hello SDK supports recursive upload and download between a local file path and a Data Lake Store file path.</span></span>    

## <a name="get-file-or-directory-info"></a><span data-ttu-id="42d1f-178">Získání informací o souboru nebo adresáři</span><span class="sxs-lookup"><span data-stu-id="42d1f-178">Get file or directory info</span></span>
<span data-ttu-id="42d1f-179">Následující fragment kódu ukazuje Hello `GetItemInfo` metoda používané tooretrieve informace o souboru nebo adresáři dostupném v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="42d1f-179">hello following snippet shows a `GetItemInfo` method that you can use tooretrieve information about a file or directory available in Data Lake Store.</span></span>

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a><span data-ttu-id="42d1f-180">Zobrazení seznamu souboru nebo adresářů</span><span class="sxs-lookup"><span data-stu-id="42d1f-180">List file or directories</span></span>
<span data-ttu-id="42d1f-181">Následující fragment kódu ukazuje Hello `ListItem` , můžete použít toolist hello souborů a adresářů v účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="42d1f-181">hello following snippet shows a `ListItem` method that can use toolist hello file and directories in a Data Lake Store account.</span></span>

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a><span data-ttu-id="42d1f-182">Řetězení souborů</span><span class="sxs-lookup"><span data-stu-id="42d1f-182">Concatenate files</span></span>
<span data-ttu-id="42d1f-183">Následující fragment kódu ukazuje Hello `ConcatenateFiles` metoda, že používáte tooconcatenate soubory.</span><span class="sxs-lookup"><span data-stu-id="42d1f-183">hello following snippet shows a `ConcatenateFiles` method that you use tooconcatenate files.</span></span>

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-tooa-file"></a><span data-ttu-id="42d1f-184">Připojení souboru tooa</span><span class="sxs-lookup"><span data-stu-id="42d1f-184">Append tooa file</span></span>
<span data-ttu-id="42d1f-185">Následující fragment kódu ukazuje Hello `AppendToFile` metoda, kterou použijete připojit datový soubor tooa už uložený v účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="42d1f-185">hello following snippet shows a `AppendToFile` method that you use append data tooa file already stored in a Data Lake Store account.</span></span>

    // Append toofile
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a><span data-ttu-id="42d1f-186">Stažení souboru</span><span class="sxs-lookup"><span data-stu-id="42d1f-186">Download a file</span></span>
<span data-ttu-id="42d1f-187">Následující fragment kódu ukazuje Hello `DownloadFile` metoda použít toodownload soubor z účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="42d1f-187">hello following snippet shows a `DownloadFile` method that you use toodownload a file from a Data Lake Store account.</span></span>

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
         _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath);
    }

## <a name="next-steps"></a><span data-ttu-id="42d1f-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42d1f-188">Next steps</span></span>
* [<span data-ttu-id="42d1f-189">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="42d1f-189">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="42d1f-190">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="42d1f-190">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="42d1f-191">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="42d1f-191">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
* [<span data-ttu-id="42d1f-192">Referenční dokumentace sady SDK rozhraní .NET služby Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="42d1f-192">Data Lake Store .NET SDK Reference</span></span>](https://docs.microsoft.com/dotnet/api/?view=azuremgmtdatalakestore-2.1.0-preview&term=DataLake.Store)
* [<span data-ttu-id="42d1f-193">Referenční dokumentace architektury REST služby Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="42d1f-193">Data Lake Store REST Reference</span></span>](https://msdn.microsoft.com/library/mt693424.aspx)
