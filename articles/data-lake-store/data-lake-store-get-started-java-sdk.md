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
# <a name="get-started-with-azure-data-lake-store-using-java"></a>Začínáme s Azure Data Lake Store pomocí jazyka Java
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

Zjistěte, jak toouse hello Azure Data Lake Store Java SDK tooperform základních operací, jako vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).

Dostanete hello dokumentace rozhraní API Java SDK pro Azure Data Lake Store v [dokumentace rozhraní API služby Azure Data Lake Store Java](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Požadavky
* Java Development Kit (JDK 7 nebo vyšší s využitím Java verze 1.7 nebo vyšší)
* Účet Azure Data Lake Store. Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). V tomto kurzu se používá Maven pro závislosti sestavení a projektu. Přestože je možné toobuild bez použití systém sestavení jako Maven nebo Gradle, zkontrolujte tyto systémy je mnohem snazší toomanage závislosti.
* (Volitelné) Rozhraní IDE, jako je například [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) nebo [Eclipse](https://www.eclipse.org/downloads/) nebo podobné.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak můžu ověřovat pomocí služby Azure Active Directory?
V tomto kurzu budeme používat Azure AD aplikace klienta tajný tooretrieve tokenu Azure Active Directory (service-to-service ověřování). Používáme tento token toocreate soubor Data Lake Store klienta objektu tooperform operací a operací directory. Návod, jak tooauthenticate s Azure Data Lake Store pomocí hello tajný klíč klienta jsme proveďte následující postup vysoké úrovně hello:

1. Vytvoření webové aplikace Azure AD
2. Načtení ID klienta hello, sdílený tajný klíč klienta a koncový bod tokenu pro hello webové aplikace Azure AD.
3. Nakonfigurujte přístup pro webovou aplikaci Azure AD hello na hello Data Lake Store soubor nebo složku, která chcete tooaccess z hello aplikaci Java, kterou vytváříte.

Pokyny, jak tooperform tyto kroky v tématu [vytvoření aplikace Active Directory](data-lake-store-authenticate-using-active-directory.md).

Azure Active Directory poskytuje že další možnosti také tooretrieve token. Můžete si vybrat z mnoha různých ověřovacích mechanismů toosuit váš scénář, například aplikace běží v prohlížeči, aplikace distribuovaných jako desktopová aplikace nebo serverová aplikace spuštěna místně nebo v Azure virtuální počítač. Vybírat můžete také z různých typů přihlašovacích údajů, jako jsou hesla, certifikáty, dvojúrovňové ověřování atd. Kromě toho Azure Active Directory umožňuje toosynchronize vaší místní služby Active Directory uživatele s cloudem hello. Podrobnosti najdete v tématu [Scénáře ověřování pro Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Vytvoření aplikace Java
Ukázka kódu Hello k dispozici [na Githubu](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) vás provede procesem vytváření souborů v hello úložišti, zřetězení souborů, stažení souboru a odstranit některé soubory v úložišti hello hello. Tato část hello článek vás provede procesem hello hlavní části kódu, hello.

1. Vytvořte projekt Maven pomocí [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) hello z příkazového řádku nebo pomocí rozhraní IDE. Pokyny jak toocreate Java projektu pomocí IntelliJ, najdete v části [zde](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Pokyny najdete v části toocreate a projekt pomocí prostředí Eclipse [zde](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 
2. Přidejte následující závislosti tooyour Maven hello **pom.xml** souboru. Přidejte následující fragment textu mezi hello hello  **\</version >** značky a hello  **\</project >** značky:
   
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
   
    první závislost Hello je toouse hello Data Lake Store SDK (`azure-data-lake-store-sdk`) z úložiště maven hello. Hello druhý závislosti (`slf4j-nop`) je toospecify které toouse framework protokolování pro tuto aplikaci. Hello Data Lake Store SDK používá [slf4j](http://www.slf4j.org/) průčelí za protokolování, který vám umožňuje vybrat z řady oblíbených protokolování rozhraní, jako je log4j, Java protokolování, logback atd., nebo žádné protokolování. V tomto příkladu jsme zakáže protokolování, proto použijeme hello **slf4j nop** vazby. toouse další možnosti protokolování v aplikaci, najdete v části [zde](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-hello-application-code"></a>Přidejte kód aplikace hello
Existují tři hlavní části toohello kódu.

1. Získat token služby Azure Active Directory hello
2. Pomocí tokenu toocreate hello klienta Data Lake Store.
3. Pomocí operace tooperform klienta Data Lake Store hello.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Krok 1: Získání tokenu Azure Active Directory
Hello Data Lake Store SDK poskytuje pohodlné metody, které vám umožní spravovat tokeny zabezpečení hello potřeby tootalk toohello účtu Data Lake Store. Ale hello SDK nenutí použít pouze tyto metody. Můžete použít jakýkoli jiný získání tokenu taky jako hello [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), nebo vlastní kód.

toouse hello Data Lake Store SDK tooobtain token pro hello Active Directory webovou aplikaci jste vytvořili dříve, použijte jednu z měly podtřídy hello `AccessTokenProvider` (hello dole uvedený příklad používá `ClientCredsTokenProvider`). Hello zprostředkovatele tokenu mezipamětí hello přihlašovací údaje pro používá tooobtain hello token v paměti a automaticky obnovuje hello token, pokud se jedná o tooexpire. Je možné toocreate vlastní měly podtřídy `AccessTokenProvider` tokeny jsou získány podle kódu zákazníků, takže teď umožňuje právě používá hello jeden součástí hello SDK.

Nahraďte **doplňovat zde** hello skutečnými hodnotami pro hello Azure Active Directory webové aplikace.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Krok 2: Vytvoření objektu klienta služby Azure Data Lake Store (ADLStoreClient)
Vytvoření [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) objekt vyžaduje, abyste toospecify hello Data Lake Store účtu název a hello zprostředkovatele tokenu jste vygenerovali v posledním kroku hello. Všimněte si, že hello účtu Data Lake Store, že název je toobe platný plně kvalifikovaný název domény. Například **FILL-IN-HERE** nahraďte něčím jako **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just hello account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-hello-adlstoreclient-tooperform-file-and-directory-operations"></a>Krok 3: Použití operace hello ADLStoreClient tooperform souborů a adresářů
Následující kód Hello obsahuje fragmenty příklad některých běžných operací. Můžete se podívat na úplné hello [dokumentace rozhraní API Data Lake Store Java SDK](https://azure.github.io/azure-data-lake-store-java/javadoc/) z hello **ADLStoreClient** objektu toosee jiné operace.

Poznámka: Soubory se čtou a do souborů se zapisuje pomocí standardních streamů Java. To znamená, že jste žádné hello Java datové proudy nad hello Data Lake Store datové proudy toobenefit z standardní funkce Java (například tisk datové proudy formátovaný výstup, nebo libovolná z hello kompresi nebo šifrování datové proudy další funkce na vrstvy "nahoře, atd.).

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

#### <a name="step-4-build-and-run-hello-application"></a>Krok 4: Sestavení a spuštění aplikace hello
1. toorun z v rámci IDE, vyhledejte a stiskněte klávesu hello **spustit** tlačítko. toorun z Maven, použijte [exec: exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).
2. tooproduce samostatné jar, který můžete spustit z příkazového řádku sestavení hello jar s všechny závislosti, které jsou zahrnuty, pomocí hello [Maven sestavení modulu plug-in](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). Hello pom.xml v hello [Příklad zdrojového kódu na githubu](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) má příklad toodo to.

## <a name="next-steps"></a>Další kroky
* [Prozkoumat JavaDoc pro hello sady Java SDK](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

