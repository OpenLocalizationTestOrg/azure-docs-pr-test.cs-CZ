---
title: aaaUse hello sady Java SDK toodevelop aplikace v Azure Data Lake Store | Microsoft Docs
description: "Pomocí Azure Data Lake Store Java SDK toocreate účtu Data Lake Store a provádění základních operací v Data Lake Store hello"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/28/2017
ms.author: nitinme
ms.openlocfilehash: d3bcee449c2a2a4bd2f7b241af46ecc010b6b62e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-java"></a><span data-ttu-id="6bd50-103">Začínáme s Azure Data Lake Store pomocí jazyka Java</span><span class="sxs-lookup"><span data-stu-id="6bd50-103">Get started with Azure Data Lake Store using Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6bd50-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6bd50-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="6bd50-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6bd50-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="6bd50-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="6bd50-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="6bd50-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="6bd50-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="6bd50-108">REST API</span><span class="sxs-lookup"><span data-stu-id="6bd50-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="6bd50-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="6bd50-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="6bd50-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="6bd50-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="6bd50-111">Python</span><span class="sxs-lookup"><span data-stu-id="6bd50-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

<span data-ttu-id="6bd50-112">Zjistěte, jak toouse hello Azure Data Lake Store Java SDK tooperform základních operací, jako vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6bd50-112">Learn how toouse hello Azure Data Lake Store Java SDK tooperform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

<span data-ttu-id="6bd50-113">Dostanete hello dokumentace rozhraní API Java SDK pro Azure Data Lake Store v [dokumentace rozhraní API služby Azure Data Lake Store Java](https://azure.github.io/azure-data-lake-store-java/javadoc/).</span><span class="sxs-lookup"><span data-stu-id="6bd50-113">You can access hello Java SDK API docs for Azure Data Lake Store at [Azure Data Lake Store Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6bd50-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6bd50-114">Prerequisites</span></span>
* <span data-ttu-id="6bd50-115">Java Development Kit (JDK 7 nebo vyšší s využitím Java verze 1.7 nebo vyšší)</span><span class="sxs-lookup"><span data-stu-id="6bd50-115">Java Development Kit (JDK 7 or higher, using Java version 1.7 or higher)</span></span>
* <span data-ttu-id="6bd50-116">Účet Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6bd50-116">Azure Data Lake Store account.</span></span> <span data-ttu-id="6bd50-117">Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6bd50-117">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span>
* <span data-ttu-id="6bd50-118">[Maven](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="6bd50-118">[Maven](https://maven.apache.org/install.html).</span></span> <span data-ttu-id="6bd50-119">V tomto kurzu se používá Maven pro závislosti sestavení a projektu.</span><span class="sxs-lookup"><span data-stu-id="6bd50-119">This tutorial uses Maven for build and project dependencies.</span></span> <span data-ttu-id="6bd50-120">Přestože je možné toobuild bez použití systém sestavení jako Maven nebo Gradle, zkontrolujte tyto systémy je mnohem snazší toomanage závislosti.</span><span class="sxs-lookup"><span data-stu-id="6bd50-120">Although it is possible toobuild without using a build system like Maven or Gradle, these systems make is much easier toomanage dependencies.</span></span>
* <span data-ttu-id="6bd50-121">(Volitelné) Rozhraní IDE, jako je například [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) nebo [Eclipse](https://www.eclipse.org/downloads/) nebo podobné.</span><span class="sxs-lookup"><span data-stu-id="6bd50-121">(Optional) And IDE like [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) or [Eclipse](https://www.eclipse.org/downloads/) or similar.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="6bd50-122">Jak můžu ověřovat pomocí služby Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6bd50-122">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="6bd50-123">V tomto kurzu budeme používat Azure AD aplikace klienta tajný tooretrieve tokenu Azure Active Directory (service-to-service ověřování).</span><span class="sxs-lookup"><span data-stu-id="6bd50-123">In this tutorial we use a Azure AD application client secret tooretrieve an Azure Active Directory token (service-to-service authentication).</span></span> <span data-ttu-id="6bd50-124">Používáme tento token toocreate soubor Data Lake Store klienta objektu tooperform operací a operací directory.</span><span class="sxs-lookup"><span data-stu-id="6bd50-124">We use this token toocreate an Data Lake Store client object tooperform operations file and directory operations.</span></span> <span data-ttu-id="6bd50-125">Návod, jak tooauthenticate s Azure Data Lake Store pomocí hello tajný klíč klienta jsme proveďte následující postup vysoké úrovně hello:</span><span class="sxs-lookup"><span data-stu-id="6bd50-125">For instructions on how tooauthenticate with Azure Data Lake Store using hello client secret, we perform hello following high-level steps:</span></span>

1. <span data-ttu-id="6bd50-126">Vytvoření webové aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="6bd50-126">Create an Azure AD web application</span></span>
2. <span data-ttu-id="6bd50-127">Načtení ID klienta hello, sdílený tajný klíč klienta a koncový bod tokenu pro hello webové aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6bd50-127">Retrieve hello client ID, client secret, and token endpoint for hello Azure AD web application.</span></span>
3. <span data-ttu-id="6bd50-128">Nakonfigurujte přístup pro webovou aplikaci Azure AD hello na hello Data Lake Store soubor nebo složku, která chcete tooaccess z hello aplikaci Java, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="6bd50-128">Configure access for hello Azure AD web application on hello Data Lake Store file/folder that you want tooaccess from hello Java application you are creating.</span></span>

<span data-ttu-id="6bd50-129">Pokyny, jak tooperform tyto kroky v tématu [vytvoření aplikace Active Directory](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="6bd50-129">For instructions on how tooperform these steps, see [Create an Active Directory application](data-lake-store-authenticate-using-active-directory.md).</span></span>

<span data-ttu-id="6bd50-130">Azure Active Directory poskytuje že další možnosti také tooretrieve token.</span><span class="sxs-lookup"><span data-stu-id="6bd50-130">Azure Active Directory provides other options as well tooretrieve a token.</span></span> <span data-ttu-id="6bd50-131">Můžete si vybrat z mnoha různých ověřovacích mechanismů toosuit váš scénář, například aplikace běží v prohlížeči, aplikace distribuovaných jako desktopová aplikace nebo serverová aplikace spuštěna místně nebo v Azure virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6bd50-131">You can pick from a number of different authentication mechanisms toosuit your scenario, for example, an application running in a browser, an application distributed as a desktop application, or a server application running on-premises or in an Azure virtual machine.</span></span> <span data-ttu-id="6bd50-132">Vybírat můžete také z různých typů přihlašovacích údajů, jako jsou hesla, certifikáty, dvojúrovňové ověřování atd. Kromě toho Azure Active Directory umožňuje toosynchronize vaší místní služby Active Directory uživatele s cloudem hello.</span><span class="sxs-lookup"><span data-stu-id="6bd50-132">You can also pick from different types of credentials like passwords, certificates, 2-factor authentication, etc. In addition, Azure Active Directory allows you toosynchronize your on-premises Active Directory users with hello cloud.</span></span> <span data-ttu-id="6bd50-133">Podrobnosti najdete v tématu [Scénáře ověřování pro Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="6bd50-133">For details, see [Authentication Scenarios for Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md).</span></span> 

## <a name="create-a-java-application"></a><span data-ttu-id="6bd50-134">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="6bd50-134">Create a Java application</span></span>
<span data-ttu-id="6bd50-135">Ukázka kódu Hello k dispozici [na Githubu](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) vás provede procesem vytváření souborů v hello úložišti, zřetězení souborů, stažení souboru a odstranit některé soubory v úložišti hello hello.</span><span class="sxs-lookup"><span data-stu-id="6bd50-135">hello code sample available [on GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) walks you through hello process of creating files in hello store, concatenating files, downloading a file, and deleting some files in hello store.</span></span> <span data-ttu-id="6bd50-136">Tato část hello článek vás provede procesem hello hlavní části kódu, hello.</span><span class="sxs-lookup"><span data-stu-id="6bd50-136">This section of hello article walk you through hello main parts of hello code.</span></span>

1. <span data-ttu-id="6bd50-137">Vytvořte projekt Maven pomocí [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) hello z příkazového řádku nebo pomocí rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="6bd50-137">Create a Maven project using [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) from hello command-line or using an IDE.</span></span> <span data-ttu-id="6bd50-138">Pokyny jak toocreate Java projektu pomocí IntelliJ, najdete v části [zde](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html).</span><span class="sxs-lookup"><span data-stu-id="6bd50-138">For instructions on how toocreate a Java project using IntelliJ, see [here](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html).</span></span> <span data-ttu-id="6bd50-139">Pokyny najdete v části toocreate a projekt pomocí prostředí Eclipse [zde](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).</span><span class="sxs-lookup"><span data-stu-id="6bd50-139">For instructions on how toocreate a project using Eclipse, see [here](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).</span></span> 
2. <span data-ttu-id="6bd50-140">Přidejte následující závislosti tooyour Maven hello **pom.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="6bd50-140">Add hello following dependencies tooyour Maven **pom.xml** file.</span></span> <span data-ttu-id="6bd50-141">Přidejte následující fragment textu mezi hello hello  **\</version >** značky a hello  **\</project >** značky:</span><span class="sxs-lookup"><span data-stu-id="6bd50-141">Add hello following snippet of text between hello **\</version>** tag and hello **\</project>** tag:</span></span>
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.5</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    <span data-ttu-id="6bd50-142">první závislost Hello je toouse hello Data Lake Store SDK (`azure-data-lake-store-sdk`) z úložiště maven hello.</span><span class="sxs-lookup"><span data-stu-id="6bd50-142">hello first dependency is toouse hello Data Lake Store SDK (`azure-data-lake-store-sdk`) from hello maven repository.</span></span> <span data-ttu-id="6bd50-143">Hello druhý závislosti (`slf4j-nop`) je toospecify které toouse framework protokolování pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6bd50-143">hello second dependency (`slf4j-nop`) is toospecify which logging framework toouse for this application.</span></span> <span data-ttu-id="6bd50-144">Hello Data Lake Store SDK používá [slf4j](http://www.slf4j.org/) průčelí za protokolování, který vám umožňuje vybrat z řady oblíbených protokolování rozhraní, jako je log4j, Java protokolování, logback atd., nebo žádné protokolování.</span><span class="sxs-lookup"><span data-stu-id="6bd50-144">hello Data Lake Store SDK uses [slf4j](http://www.slf4j.org/) logging façade, which lets you choose from a number of popular logging frameworks, like log4j, Java logging, logback, etc., or no logging.</span></span> <span data-ttu-id="6bd50-145">V tomto příkladu jsme zakáže protokolování, proto použijeme hello **slf4j nop** vazby.</span><span class="sxs-lookup"><span data-stu-id="6bd50-145">For this example, we will disable logging, hence we use hello **slf4j-nop** binding.</span></span> <span data-ttu-id="6bd50-146">toouse další možnosti protokolování v aplikaci, najdete v části [zde](http://www.slf4j.org/manual.html#projectDep).</span><span class="sxs-lookup"><span data-stu-id="6bd50-146">toouse other logging options in your app, see [here](http://www.slf4j.org/manual.html#projectDep).</span></span>

### <a name="add-hello-application-code"></a><span data-ttu-id="6bd50-147">Přidejte kód aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6bd50-147">Add hello application code</span></span>
<span data-ttu-id="6bd50-148">Existují tři hlavní části toohello kódu.</span><span class="sxs-lookup"><span data-stu-id="6bd50-148">There are three main parts toohello code.</span></span>

1. <span data-ttu-id="6bd50-149">Získat token služby Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="6bd50-149">Obtain hello Azure Active Directory token</span></span>
2. <span data-ttu-id="6bd50-150">Pomocí tokenu toocreate hello klienta Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6bd50-150">Use hello token toocreate a Data Lake Store client.</span></span>
3. <span data-ttu-id="6bd50-151">Pomocí operace tooperform klienta Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="6bd50-151">Use hello Data Lake Store client tooperform operations.</span></span>

#### <a name="step-1-obtain-an-azure-active-directory-token"></a><span data-ttu-id="6bd50-152">Krok 1: Získání tokenu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6bd50-152">Step 1: Obtain an Azure Active Directory token.</span></span>
<span data-ttu-id="6bd50-153">Hello Data Lake Store SDK poskytuje pohodlné metody, které vám umožní spravovat tokeny zabezpečení hello potřeby tootalk toohello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6bd50-153">hello Data Lake Store SDK provides convenient methods that let you manage hello security tokens needed tootalk toohello Data Lake Store account.</span></span> <span data-ttu-id="6bd50-154">Ale hello SDK nenutí použít pouze tyto metody.</span><span class="sxs-lookup"><span data-stu-id="6bd50-154">However, hello SDK does not mandate that only these methods be used.</span></span> <span data-ttu-id="6bd50-155">Můžete použít jakýkoli jiný získání tokenu taky jako hello [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), nebo vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="6bd50-155">You can use any other means of obtaining token as well, like using hello [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), or your own custom code.</span></span>

<span data-ttu-id="6bd50-156">toouse hello Data Lake Store SDK tooobtain token pro hello Active Directory webovou aplikaci jste vytvořili dříve, použijte jednu z měly podtřídy hello `AccessTokenProvider` (hello dole uvedený příklad používá `ClientCredsTokenProvider`).</span><span class="sxs-lookup"><span data-stu-id="6bd50-156">toouse hello Data Lake Store SDK tooobtain token for hello Active Directory Web application you created earlier, use one of hello subclasses of `AccessTokenProvider` (hello example below uses `ClientCredsTokenProvider`).</span></span> <span data-ttu-id="6bd50-157">Hello zprostředkovatele tokenu mezipamětí hello přihlašovací údaje pro používá tooobtain hello token v paměti a automaticky obnovuje hello token, pokud se jedná o tooexpire.</span><span class="sxs-lookup"><span data-stu-id="6bd50-157">hello token provider caches hello creds used tooobtain hello token in memory, and automatically renews hello token if it is about tooexpire.</span></span> <span data-ttu-id="6bd50-158">Je možné toocreate vlastní měly podtřídy `AccessTokenProvider` tokeny jsou získány podle kódu zákazníků, takže teď umožňuje právě používá hello jeden součástí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="6bd50-158">It is possible toocreate your own subclasses of `AccessTokenProvider` so tokens are obtained by your customer code, but for now let's just use hello one provided in hello SDK.</span></span>

<span data-ttu-id="6bd50-159">Nahraďte **doplňovat zde** hello skutečnými hodnotami pro hello Azure Active Directory webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6bd50-159">Replace **FILL-IN-HERE** with hello actual values for hello Azure Active Directory Web application.</span></span>

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a><span data-ttu-id="6bd50-160">Krok 2: Vytvoření objektu klienta služby Azure Data Lake Store (ADLStoreClient)</span><span class="sxs-lookup"><span data-stu-id="6bd50-160">Step 2: Create an Azure Data Lake Store client (ADLStoreClient) object</span></span>
<span data-ttu-id="6bd50-161">Vytvoření [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) objekt vyžaduje, abyste toospecify hello Data Lake Store účtu název a hello zprostředkovatele tokenu jste vygenerovali v posledním kroku hello.</span><span class="sxs-lookup"><span data-stu-id="6bd50-161">Creating an [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) object requires you toospecify hello Data Lake Store account name and hello token provider you generated in hello last step.</span></span> <span data-ttu-id="6bd50-162">Všimněte si, že hello účtu Data Lake Store, že název je toobe platný plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="6bd50-162">Note that hello Data Lake Store account name needs toobe a fully qualified domain name.</span></span> <span data-ttu-id="6bd50-163">Například **FILL-IN-HERE** nahraďte něčím jako **mydatalakestore.azuredatalakestore.net**.</span><span class="sxs-lookup"><span data-stu-id="6bd50-163">For example, replace **FILL-IN-HERE** with something like **mydatalakestore.azuredatalakestore.net**.</span></span>

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just hello account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-hello-adlstoreclient-tooperform-file-and-directory-operations"></a><span data-ttu-id="6bd50-164">Krok 3: Použití operace hello ADLStoreClient tooperform souborů a adresářů</span><span class="sxs-lookup"><span data-stu-id="6bd50-164">Step 3: Use hello ADLStoreClient tooperform file and directory operations</span></span>
<span data-ttu-id="6bd50-165">Následující kód Hello obsahuje fragmenty příklad některých běžných operací.</span><span class="sxs-lookup"><span data-stu-id="6bd50-165">hello code below contains example snippets of some common operations.</span></span> <span data-ttu-id="6bd50-166">Můžete se podívat na úplné hello [dokumentace rozhraní API Data Lake Store Java SDK](https://azure.github.io/azure-data-lake-store-java/javadoc/) z hello **ADLStoreClient** objektu toosee jiné operace.</span><span class="sxs-lookup"><span data-stu-id="6bd50-166">You can look at hello full [Data Lake Store Java SDK API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/) of hello **ADLStoreClient** object toosee other operations.</span></span>

<span data-ttu-id="6bd50-167">Poznámka: Soubory se čtou a do souborů se zapisuje pomocí standardních streamů Java.</span><span class="sxs-lookup"><span data-stu-id="6bd50-167">Note that files are read from and written into using standard Java streams.</span></span> <span data-ttu-id="6bd50-168">To znamená, že jste žádné hello Java datové proudy nad hello Data Lake Store datové proudy toobenefit z standardní funkce Java (například tisk datové proudy formátovaný výstup, nebo libovolná z hello kompresi nebo šifrování datové proudy další funkce na vrstvy "nahoře, atd.).</span><span class="sxs-lookup"><span data-stu-id="6bd50-168">This means that you can layer any of hello Java streams on top of hello Data Lake Store streams toobenefit from standard Java functionality (e.g., Print streams for formatted output, or any of hello compression or encryption streams for additional functionality on top, etc.).</span></span>

     // create file and write some content
     String filename = "/a/b/c.txt";
     OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
     PrintStream out = new PrintStream(stream);
     for (int i = 1; i <= 10; i++) {
         out.println("This is line #" + i);
         out.format("This is hello same line (%d), but using formatted output. %n", i);
     }
     out.close();
    
    // set file permission
    client.setPermission(filename, "744");

    // append toofile
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate hello two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename hello file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all hello subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-hello-application"></a><span data-ttu-id="6bd50-169">Krok 4: Sestavení a spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6bd50-169">Step 4: Build and run hello application</span></span>
1. <span data-ttu-id="6bd50-170">toorun z v rámci IDE, vyhledejte a stiskněte klávesu hello **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6bd50-170">toorun from within an IDE, locate and press hello **Run** button.</span></span> <span data-ttu-id="6bd50-171">toorun z Maven, použijte [exec: exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).</span><span class="sxs-lookup"><span data-stu-id="6bd50-171">toorun from Maven, use [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).</span></span>
2. <span data-ttu-id="6bd50-172">tooproduce samostatné jar, který můžete spustit z příkazového řádku sestavení hello jar s všechny závislosti, které jsou zahrnuty, pomocí hello [Maven sestavení modulu plug-in](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html).</span><span class="sxs-lookup"><span data-stu-id="6bd50-172">tooproduce a standalone jar that you can run from command-line build hello jar with all dependencies included, using hello [Maven assembly plugin](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html).</span></span> <span data-ttu-id="6bd50-173">Hello pom.xml v hello [Příklad zdrojového kódu na githubu](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) má příklad toodo to.</span><span class="sxs-lookup"><span data-stu-id="6bd50-173">hello pom.xml in hello [example source code on github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) has an example of how toodo this.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6bd50-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6bd50-174">Next steps</span></span>
* [<span data-ttu-id="6bd50-175">Prozkoumat JavaDoc pro hello sady Java SDK</span><span class="sxs-lookup"><span data-stu-id="6bd50-175">Explore JavaDoc for hello Java SDK</span></span>](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [<span data-ttu-id="6bd50-176">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6bd50-176">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="6bd50-177">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6bd50-177">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="6bd50-178">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6bd50-178">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

