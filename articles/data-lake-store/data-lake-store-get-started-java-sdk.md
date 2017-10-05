---
title: "Vývoj aplikací pro Azure Data Lake Store pomocí Java SDK | Dokumentace Microsoftu"
description: "Přečtěte si, jak pomocí Azure Data Lake Store Java SDK vytvořit účet Data Lake Store a provádět další základní operace."
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
ms.openlocfilehash: 91128b53a2f1cd3ddcbee5b07da0d67668944fb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-java"></a><span data-ttu-id="a5548-103">Začínáme s Azure Data Lake Store pomocí jazyka Java</span><span class="sxs-lookup"><span data-stu-id="a5548-103">Get started with Azure Data Lake Store using Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a5548-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a5548-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="a5548-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a5548-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="a5548-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="a5548-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="a5548-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="a5548-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="a5548-108">REST API</span><span class="sxs-lookup"><span data-stu-id="a5548-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="a5548-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a5548-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="a5548-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="a5548-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="a5548-111">Python</span><span class="sxs-lookup"><span data-stu-id="a5548-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

<span data-ttu-id="a5548-112">Naučte se používat sadu Java SDK pro Azure Data Lake Store k provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a5548-112">Learn how to use the Azure Data Lake Store Java SDK to perform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

<span data-ttu-id="a5548-113">Dokumentaci rozhraní API sady Java SDK pro Azure Data Lake Store najdete v tématu [Dokumentace rozhraní API sady Java SDK pro Azure Data Lake Store](https://azure.github.io/azure-data-lake-store-java/javadoc/).</span><span class="sxs-lookup"><span data-stu-id="a5548-113">You can access the Java SDK API docs for Azure Data Lake Store at [Azure Data Lake Store Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5548-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a5548-114">Prerequisites</span></span>
* <span data-ttu-id="a5548-115">Java Development Kit (JDK 7 nebo vyšší s využitím Java verze 1.7 nebo vyšší)</span><span class="sxs-lookup"><span data-stu-id="a5548-115">Java Development Kit (JDK 7 or higher, using Java version 1.7 or higher)</span></span>
* <span data-ttu-id="a5548-116">Účet Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a5548-116">Azure Data Lake Store account.</span></span> <span data-ttu-id="a5548-117">Postupujte podle pokynů v tématu [Začínáme s Azure Data Lake Store s použitím webu Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a5548-117">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span>
* <span data-ttu-id="a5548-118">[Maven](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="a5548-118">[Maven](https://maven.apache.org/install.html).</span></span> <span data-ttu-id="a5548-119">V tomto kurzu se používá Maven pro závislosti sestavení a projektu.</span><span class="sxs-lookup"><span data-stu-id="a5548-119">This tutorial uses Maven for build and project dependencies.</span></span> <span data-ttu-id="a5548-120">I když je možné sestavení vytvářet bez použití systému pro sestavení, jako je Maven a Gradle, tyto systémy podstatně usnadňují správu závislostí.</span><span class="sxs-lookup"><span data-stu-id="a5548-120">Although it is possible to build without using a build system like Maven or Gradle, these systems make is much easier to manage dependencies.</span></span>
* <span data-ttu-id="a5548-121">(Volitelné) Rozhraní IDE, jako je například [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) nebo [Eclipse](https://www.eclipse.org/downloads/) nebo podobné.</span><span class="sxs-lookup"><span data-stu-id="a5548-121">(Optional) And IDE like [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) or [Eclipse](https://www.eclipse.org/downloads/) or similar.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="a5548-122">Jak můžu ověřovat pomocí služby Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a5548-122">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="a5548-123">V tomto kurzu používáme tajný kód klienta aplikace Azure AD k načtení tokenu Azure Active Directory (ověřování mezi službami).</span><span class="sxs-lookup"><span data-stu-id="a5548-123">In this tutorial we use a Azure AD application client secret to retrieve an Azure Active Directory token (service-to-service authentication).</span></span> <span data-ttu-id="a5548-124">Tento token používáme k vytvoření objektu klienta služby Data Lake Store k provádění operací operačních souborů a adresářů.</span><span class="sxs-lookup"><span data-stu-id="a5548-124">We use this token to create an Data Lake Store client object to perform operations file and directory operations.</span></span> <span data-ttu-id="a5548-125">Abychom mohli ukázat postup ověřování pomocí služby Azure Data Lake Store za použití sdíleného tajného kódu klienta, provedeme následující základní kroky:</span><span class="sxs-lookup"><span data-stu-id="a5548-125">For instructions on how to authenticate with Azure Data Lake Store using the client secret, we perform the following high-level steps:</span></span>

1. <span data-ttu-id="a5548-126">Vytvoření webové aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="a5548-126">Create an Azure AD web application</span></span>
2. <span data-ttu-id="a5548-127">Načtení ID klienta, tajného kódu klienta a koncového bodu tokenu pro webovou aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="a5548-127">Retrieve the client ID, client secret, and token endpoint for the Azure AD web application.</span></span>
3. <span data-ttu-id="a5548-128">Konfigurace přístupu pro webovou aplikaci Azure AD pro soubor nebo složku služby Data Lake Store, ke kterým chcete získat přístup z aplikace Java, kterou vytváříte</span><span class="sxs-lookup"><span data-stu-id="a5548-128">Configure access for the Azure AD web application on the Data Lake Store file/folder that you want to access from the Java application you are creating.</span></span>

<span data-ttu-id="a5548-129">Pokyny k provedení těchto kroků najdete v tématu [Vytvoření aplikace služby Active Directory](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="a5548-129">For instructions on how to perform these steps, see [Create an Active Directory application](data-lake-store-authenticate-using-active-directory.md).</span></span>

<span data-ttu-id="a5548-130">V Azure Active Directory jsou také další možnosti, jak načíst token.</span><span class="sxs-lookup"><span data-stu-id="a5548-130">Azure Active Directory provides other options as well to retrieve a token.</span></span> <span data-ttu-id="a5548-131">Můžete vybírat z mnoha různých ověřovacích mechanismů vhodných pro váš scénář, jako je například spuštění aplikace v prohlížeči, distribuce aplikace jako desktopové aplikace nebo spuštění serverové aplikace místně nebo na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="a5548-131">You can pick from a number of different authentication mechanisms to suit your scenario, for example, an application running in a browser, an application distributed as a desktop application, or a server application running on-premises or in an Azure virtual machine.</span></span> <span data-ttu-id="a5548-132">Vybírat můžete také z různých typů přihlašovacích údajů, jako jsou hesla, certifikáty, dvojúrovňové ověřování atd. Kromě toho umožňuje Azure Active Directory synchronizaci uživatelů místní služby Active Directory s cloudem.</span><span class="sxs-lookup"><span data-stu-id="a5548-132">You can also pick from different types of credentials like passwords, certificates, 2-factor authentication, etc. In addition, Azure Active Directory allows you to synchronize your on-premises Active Directory users with the cloud.</span></span> <span data-ttu-id="a5548-133">Podrobnosti najdete v tématu [Scénáře ověřování pro Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="a5548-133">For details, see [Authentication Scenarios for Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md).</span></span> 

## <a name="create-a-java-application"></a><span data-ttu-id="a5548-134">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="a5548-134">Create a Java application</span></span>
<span data-ttu-id="a5548-135">Ukázka kódu, která je k dispozici [na GitHubu](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/), vás provede procesem vytvoření souborů v úložišti, zřetězení souborů, stažení souboru a nakonec odstranění některých souborů v úložišti.</span><span class="sxs-lookup"><span data-stu-id="a5548-135">The code sample available [on GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) walks you through the process of creating files in the store, concatenating files, downloading a file, and deleting some files in the store.</span></span> <span data-ttu-id="a5548-136">V této části článku se dozvíte o hlavních částech kódu.</span><span class="sxs-lookup"><span data-stu-id="a5548-136">This section of the article walk you through the main parts of the code.</span></span>

1. <span data-ttu-id="a5548-137">Vytvořte projekt Maven pomocí příkazu [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) z příkazového řádku nebo pomocí rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="a5548-137">Create a Maven project using [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) from the command-line or using an IDE.</span></span> <span data-ttu-id="a5548-138">Pokyny k vytvoření projektu jazyka Java s použitím IntelliJ najdete [zde](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html).</span><span class="sxs-lookup"><span data-stu-id="a5548-138">For instructions on how to create a Java project using IntelliJ, see [here](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html).</span></span> <span data-ttu-id="a5548-139">Pokyny k vytvoření projektu s použitím Eclipse najdete [zde](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).</span><span class="sxs-lookup"><span data-stu-id="a5548-139">For instructions on how to create a project using Eclipse, see [here](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).</span></span> 
2. <span data-ttu-id="a5548-140">Přidejte k souboru Maven **pom.xml** následující závislosti.</span><span class="sxs-lookup"><span data-stu-id="a5548-140">Add the following dependencies to your Maven **pom.xml** file.</span></span> <span data-ttu-id="a5548-141">Přidejte mezi značky **\</version>** a **\</project>** následující fragment textu:</span><span class="sxs-lookup"><span data-stu-id="a5548-141">Add the following snippet of text between the **\</version>** tag and the **\</project>** tag:</span></span>
   
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
   
    <span data-ttu-id="a5548-142">První závislostí je použití sady SDK pro Data Lake Store (`azure-data-lake-store-sdk`) z úložiště maven.</span><span class="sxs-lookup"><span data-stu-id="a5548-142">The first dependency is to use the Data Lake Store SDK (`azure-data-lake-store-sdk`) from the maven repository.</span></span> <span data-ttu-id="a5548-143">Druhou závislostí (`slf4j-nop`) je zadání protokolovacího rozhraní, které se pro tuto aplikaci použije.</span><span class="sxs-lookup"><span data-stu-id="a5548-143">The second dependency (`slf4j-nop`) is to specify which logging framework to use for this application.</span></span> <span data-ttu-id="a5548-144">Sada SDK pro službu Data Lake Store používá při protokolování [slf4j](http://www.slf4j.org/), takže máte možnost si vybrat z řady oblíbených protokolovacích rozhraní, jako je log4j, Java, logback atd., nebo nemusíte použít žádné protokolování.</span><span class="sxs-lookup"><span data-stu-id="a5548-144">The Data Lake Store SDK uses [slf4j](http://www.slf4j.org/) logging façade, which lets you choose from a number of popular logging frameworks, like log4j, Java logging, logback, etc., or no logging.</span></span> <span data-ttu-id="a5548-145">Pro tento příklad zakážeme protokolování a použijeme tedy vazbu **slf4j-nop**.</span><span class="sxs-lookup"><span data-stu-id="a5548-145">For this example, we will disable logging, hence we use the **slf4j-nop** binding.</span></span> <span data-ttu-id="a5548-146">Pokud chcete ve své aplikaci použít jiné možnosti protokolování, přečtěte si informace [zde](http://www.slf4j.org/manual.html#projectDep).</span><span class="sxs-lookup"><span data-stu-id="a5548-146">To use other logging options in your app, see [here](http://www.slf4j.org/manual.html#projectDep).</span></span>

### <a name="add-the-application-code"></a><span data-ttu-id="a5548-147">Přidání kódu aplikace</span><span class="sxs-lookup"><span data-stu-id="a5548-147">Add the application code</span></span>
<span data-ttu-id="a5548-148">Kód má tři hlavní části.</span><span class="sxs-lookup"><span data-stu-id="a5548-148">There are three main parts to the code.</span></span>

1. <span data-ttu-id="a5548-149">Získání tokenu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a5548-149">Obtain the Azure Active Directory token</span></span>
2. <span data-ttu-id="a5548-150">Použití tokenu k vytvoření klienta služby Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a5548-150">Use the token to create a Data Lake Store client.</span></span>
3. <span data-ttu-id="a5548-151">Použití klienta služby Data Lake Store k provedení operací</span><span class="sxs-lookup"><span data-stu-id="a5548-151">Use the Data Lake Store client to perform operations.</span></span>

#### <a name="step-1-obtain-an-azure-active-directory-token"></a><span data-ttu-id="a5548-152">Krok 1: Získání tokenu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a5548-152">Step 1: Obtain an Azure Active Directory token.</span></span>
<span data-ttu-id="a5548-153">V sadě SDK pro Data Lake Store jsou vhodné metody, které umožňují spravovat tokeny zabezpečení potřebné ke komunikaci s účtem služby Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a5548-153">The Data Lake Store SDK provides convenient methods that let you manage the security tokens needed to talk to the Data Lake Store account.</span></span> <span data-ttu-id="a5548-154">Není ale povinné použít tuto sadu SDK a tyto metody.</span><span class="sxs-lookup"><span data-stu-id="a5548-154">However, the SDK does not mandate that only these methods be used.</span></span> <span data-ttu-id="a5548-155">Můžete použít také jakýkoliv jiný způsob získání tokenu, například sadu [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java) nebo vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="a5548-155">You can use any other means of obtaining token as well, like using the [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), or your own custom code.</span></span>

<span data-ttu-id="a5548-156">Pokud chcete k získání tokenu pro webovou aplikaci služby Active Directory, kterou jste vytvořili dříve, použít sadu SDK pro Data Lake Store, použijte jednu z podtříd třídy `AccessTokenProvider` (dále uvedený příklad používá `ClientCredsTokenProvider`).</span><span class="sxs-lookup"><span data-stu-id="a5548-156">To use the Data Lake Store SDK to obtain token for the Active Directory Web application you created earlier, use one of the subclasses of `AccessTokenProvider` (the example below uses `ClientCredsTokenProvider`).</span></span> <span data-ttu-id="a5548-157">Poskytovatel tokenu má uložené přihlašovací údaje v mezipaměti a automaticky token obnovuje, pokud má vypršet jeho platnost.</span><span class="sxs-lookup"><span data-stu-id="a5548-157">The token provider caches the creds used to obtain the token in memory, and automatically renews the token if it is about to expire.</span></span> <span data-ttu-id="a5548-158">Je možné vytvořit vlastní podtřídy třídy `AccessTokenProvider`, takže tokeny se získávají pomocí zákaznického kódu, ale pro tentokrát použijeme tu, která je součástí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="a5548-158">It is possible to create your own subclasses of `AccessTokenProvider` so tokens are obtained by your customer code, but for now let's just use the one provided in the SDK.</span></span>

<span data-ttu-id="a5548-159">Místo **FILL-IN-HERE** zadejte skutečné hodnoty pro webovou aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a5548-159">Replace **FILL-IN-HERE** with the actual values for the Azure Active Directory Web application.</span></span>

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a><span data-ttu-id="a5548-160">Krok 2: Vytvoření objektu klienta služby Azure Data Lake Store (ADLStoreClient)</span><span class="sxs-lookup"><span data-stu-id="a5548-160">Step 2: Create an Azure Data Lake Store client (ADLStoreClient) object</span></span>
<span data-ttu-id="a5548-161">Pokud chcete vytvořit objekt [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/), musíte zadat název účtu služby Data Lake Store a poskytovatele tokenu, kterého jste vygenerovali v minulém kroku.</span><span class="sxs-lookup"><span data-stu-id="a5548-161">Creating an [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) object requires you to specify the Data Lake Store account name and the token provider you generated in the last step.</span></span> <span data-ttu-id="a5548-162">Název účtu služby Data Lake Store musí být plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="a5548-162">Note that the Data Lake Store account name needs to be a fully qualified domain name.</span></span> <span data-ttu-id="a5548-163">Například **FILL-IN-HERE** nahraďte něčím jako **mydatalakestore.azuredatalakestore.net**.</span><span class="sxs-lookup"><span data-stu-id="a5548-163">For example, replace **FILL-IN-HERE** with something like **mydatalakestore.azuredatalakestore.net**.</span></span>

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a><span data-ttu-id="a5548-164">Krok 3: Použití objektu ADLStoreClient k provedení operací souborů a adresářů</span><span class="sxs-lookup"><span data-stu-id="a5548-164">Step 3: Use the ADLStoreClient to perform file and directory operations</span></span>
<span data-ttu-id="a5548-165">Kód níže obsahuje příklady fragmentů některých běžných operací.</span><span class="sxs-lookup"><span data-stu-id="a5548-165">The code below contains example snippets of some common operations.</span></span> <span data-ttu-id="a5548-166">Na další operace se můžete podívat v kompletní [Dokumentaci rozhraní API sady Java SDK pro Azure Data Lake Store](https://azure.github.io/azure-data-lake-store-java/javadoc/) k objektu **ADLStoreClient**.</span><span class="sxs-lookup"><span data-stu-id="a5548-166">You can look at the full [Data Lake Store Java SDK API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/) of the **ADLStoreClient** object to see other operations.</span></span>

<span data-ttu-id="a5548-167">Poznámka: Soubory se čtou a do souborů se zapisuje pomocí standardních streamů Java.</span><span class="sxs-lookup"><span data-stu-id="a5548-167">Note that files are read from and written into using standard Java streams.</span></span> <span data-ttu-id="a5548-168">Znamená to, že kterýkoliv stream Java můžete umístit do vrstvy nad streamy služby Data Lake Store. Díky tomu budete moct využít možností standardních funkcí Java (například streamů tisku pro formátovaný výstup nebo kteréhokoliv ze streamů komprese nebo šifrování pro získání dalších funkcí atd.).</span><span class="sxs-lookup"><span data-stu-id="a5548-168">This means that you can layer any of the Java streams on top of the Data Lake Store streams to benefit from standard Java functionality (e.g., Print streams for formatted output, or any of the compression or encryption streams for additional functionality on top, etc.).</span></span>

     // create file and write some content
     String filename = "/a/b/c.txt";
     OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
     PrintStream out = new PrintStream(stream);
     for (int i = 1; i <= 10; i++) {
         out.println("This is line #" + i);
         out.format("This is the same line (%d), but using formatted output. %n", i);
     }
     out.close();
    
    // set file permission
    client.setPermission(filename, "744");

    // append to file
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

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a><span data-ttu-id="a5548-169">Krok 4: Sestavení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="a5548-169">Step 4: Build and run the application</span></span>
1. <span data-ttu-id="a5548-170">Pokud chcete aplikaci spustit z rozhraní IDE, stiskněte tlačítko **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="a5548-170">To run from within an IDE, locate and press the **Run** button.</span></span> <span data-ttu-id="a5548-171">Pokud ji chcete spustit z Mavenu, použijte příkaz [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).</span><span class="sxs-lookup"><span data-stu-id="a5548-171">To run from Maven, use [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).</span></span>
2. <span data-ttu-id="a5548-172">Jestliže chcete vytvořit samostatný soubor jar, který budete moct spustit z příkazového řádku, vytvořte ho se všemi závislostmi s použitím [modulu plug-in sestavení Maven](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html).</span><span class="sxs-lookup"><span data-stu-id="a5548-172">To produce a standalone jar that you can run from command-line build the jar with all dependencies included, using the [Maven assembly plugin](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html).</span></span> <span data-ttu-id="a5548-173">U pom.xml v [ukázkovém zdrojovém kódu na githubu](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) najdete příklad, jak to provést.</span><span class="sxs-lookup"><span data-stu-id="a5548-173">The pom.xml in the [example source code on github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) has an example of how to do this.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5548-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5548-174">Next steps</span></span>
* [<span data-ttu-id="a5548-175">Prozkoumání JavaDoc k sadě Java SDK</span><span class="sxs-lookup"><span data-stu-id="a5548-175">Explore JavaDoc for the Java SDK</span></span>](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [<span data-ttu-id="a5548-176">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a5548-176">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="a5548-177">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a5548-177">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="a5548-178">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a5548-178">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

