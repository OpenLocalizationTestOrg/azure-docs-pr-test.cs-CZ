---
title: "kurz vývoje aplikace aaaJava pomocí Azure Cosmos DB | Microsoft Docs"
description: "Tento kurz webové aplikace Java ukazuje, jak toouse hello Azure Cosmos DB a hello toostore DocumentDB rozhraní API a přístup k datům z aplikace Java hostované na webech Azure."
keywords: "Vývoj aplikací, databázový kurz, aplikace v jazyce java, kurz vývoje webových aplikací v jazyce java, documentdb, azure, Microsoft azure"
services: cosmos-db
documentationcenter: java
author: dennyglee
manager: jhubbard
editor: mimig
ms.assetid: 0867a4a2-4bf5-4898-a1f4-44e3868f8725
ms.service: cosmos-db
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 08/22/2017
ms.author: denlee
ms.openlocfilehash: e073de23beb0037ee1e37b48a69e8fe7cdc3fc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-web-application-using-azure-cosmos-db-and-hello-documentdb-api"></a><span data-ttu-id="f1807-104">Vytvoření webové aplikace Java pomocí Azure Cosmos DB a hello DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f1807-104">Build a Java web application using Azure Cosmos DB and hello DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f1807-105">.NET</span><span class="sxs-lookup"><span data-stu-id="f1807-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="f1807-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="f1807-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="f1807-107">Java</span><span class="sxs-lookup"><span data-stu-id="f1807-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="f1807-108">Python</span><span class="sxs-lookup"><span data-stu-id="f1807-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="f1807-109">Tento kurz webové aplikace Java ukazuje, jak toouse hello [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) služby toostore a přístupová data z aplikace Java hostované v Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="f1807-109">This Java web application tutorial shows you how toouse hello [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service toostore and access data from a Java application hosted on Azure App Service Web Apps.</span></span> <span data-ttu-id="f1807-110">V tomto tématu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="f1807-110">In this topic, you will learn:</span></span>

* <span data-ttu-id="f1807-111">Jak toobuild základní aplikaci JavaServer stránky (JSP) v prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="f1807-111">How toobuild a basic JavaServer Pages (JSP) application in Eclipse.</span></span>
* <span data-ttu-id="f1807-112">Jak toowork s hello Azure Cosmos DB služby pomocí hello [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="f1807-112">How toowork with hello Azure Cosmos DB service using hello [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

<span data-ttu-id="f1807-113">Tento kurz aplikace Java ukazuje, jak úloh toocreate webovou aplikaci pro správu úkolů umožňující toocreate můžete načíst a označit jako dokončené, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="f1807-113">This Java application tutorial shows you how toocreate a web-based task-management application that enables you toocreate, retrieve, and mark tasks as complete, as shown in hello following image.</span></span> <span data-ttu-id="f1807-114">Jednotlivé úlohy hello v seznamu úkolů hello jsou ukládány jako dokumenty JSON v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f1807-114">Each of hello tasks in hello ToDo list are stored as JSON documents in Azure Cosmos DB.</span></span>

![Aplikace pro seznam úkolů v jazyce Java](./media/documentdb-java-application/image1.png)

> [!TIP]
> <span data-ttu-id="f1807-116">V tomto kurzu vývoje aplikace se předpokládá, že již máte zkušenosti s jazykem Java.</span><span class="sxs-lookup"><span data-stu-id="f1807-116">This application development tutorial assumes that you have prior experience using Java.</span></span> <span data-ttu-id="f1807-117">Pokud jste nový tooJava nebo hello [požadovaných nástrojů](#Prerequisites), doporučujeme stáhnout úplný hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektu z Githubu a postupovat podle [hello pokyny na konci hello článek](#GetProject).</span><span class="sxs-lookup"><span data-stu-id="f1807-117">If you are new tooJava or hello [prerequisite tools](#Prerequisites), we recommend downloading hello complete [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project from GitHub and building it using [hello instructions at hello end of this article](#GetProject).</span></span> <span data-ttu-id="f1807-118">Po jejím vytvořené, můžete zkontrolovat hello článku toogain porozuměli hello kódu v kontextu hello hello projektu.</span><span class="sxs-lookup"><span data-stu-id="f1807-118">Once you have it built, you can review hello article toogain insight on hello code in hello context of hello project.</span></span>  
> 
> 

## <span data-ttu-id="f1807-119"><a id="Prerequisites"></a>Předpoklady pro tento kurz webové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="f1807-119"><a id="Prerequisites"></a>Prerequisites for this Java web application tutorial</span></span>
<span data-ttu-id="f1807-120">Než začnete tento kurz vývoje aplikace, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="f1807-120">Before you begin this application development tutorial, you must have hello following:</span></span>

* <span data-ttu-id="f1807-121">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f1807-121">An active Azure account.</span></span> <span data-ttu-id="f1807-122">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="f1807-122">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f1807-123">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f1807-123">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/)</span></span>

    <span data-ttu-id="f1807-124">NEBO</span><span class="sxs-lookup"><span data-stu-id="f1807-124">OR</span></span>

    <span data-ttu-id="f1807-125">Místní instalace hello [emulátoru DB Cosmos Azure](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="f1807-125">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="f1807-126">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)</span><span class="sxs-lookup"><span data-stu-id="f1807-126">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* [<span data-ttu-id="f1807-127">Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE</span><span class="sxs-lookup"><span data-stu-id="f1807-127">Eclipse IDE for Java EE Developers.</span></span>](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
* [<span data-ttu-id="f1807-128">Web s Azure s Java runtime environment (např. Tomcat nebo Jetty) povolen.</span><span class="sxs-lookup"><span data-stu-id="f1807-128">An Azure Web Site with a Java runtime environment (e.g. Tomcat or Jetty) enabled.</span></span>](../app-service-web/web-sites-java-get-started.md)

<span data-ttu-id="f1807-129">Pokud tyto nástroje pro hello instalujete poprvé, coreservlets.com poskytuje návod hello procesu instalace v části rychlý Start hello z jejich [kurz: instalace TomCat7 a jeho použití s Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) článku.</span><span class="sxs-lookup"><span data-stu-id="f1807-129">If you're installing these tools for hello first time, coreservlets.com provides a walk-through of hello installation process in hello Quick Start section of their [Tutorial: Installing TomCat7 and Using it with Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) article.</span></span>

## <span data-ttu-id="f1807-130"><a id="CreateDB"></a>Krok 1: Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f1807-130"><a id="CreateDB"></a>Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="f1807-131">Začněme vytvořením účtu služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f1807-131">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="f1807-132">Pokud již máte účet, nebo pokud používáte hello emulátoru DB Cosmos Azure pro účely tohoto kurzu, můžete přeskočit příliš[krok 2: vytvoření aplikace Java JSP hello](#CreateJSP).</span><span class="sxs-lookup"><span data-stu-id="f1807-132">If you already have an account or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Step 2: Create hello Java JSP application](#CreateJSP).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="f1807-133"><a id="CreateJSP"></a>Krok 2: Vytvoření aplikace Java JSP hello</span><span class="sxs-lookup"><span data-stu-id="f1807-133"><a id="CreateJSP"></a>Step 2: Create hello Java JSP application</span></span>
<span data-ttu-id="f1807-134">toocreate hello aplikace JSP:</span><span class="sxs-lookup"><span data-stu-id="f1807-134">toocreate hello JSP application:</span></span>

1. <span data-ttu-id="f1807-135">Nejdříve začneme vytvořením projektu Java.</span><span class="sxs-lookup"><span data-stu-id="f1807-135">First, we’ll start off by creating a Java project.</span></span> <span data-ttu-id="f1807-136">Spusťte Eclipse, klikněte na **File** (Soubor), pak na **New** (Nový) a nakonec na **Dynamic Web Project** (Dynamický webový projekt).</span><span class="sxs-lookup"><span data-stu-id="f1807-136">Start Eclipse, then click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="f1807-137">Pokud nevidíte **Dynamic Web Project** uvedené jako dostupné projekt, hello následující: klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**, klikněte na tlačítko **projektu**..., rozbalte položku **webové**, klikněte na tlačítko **Dynamic Web Project**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f1807-137">If you don’t see **Dynamic Web Project** listed as an available project, do hello following: click **File**, click **New**, click **Project**…, expand **Web**, click **Dynamic Web Project**, and click **Next**.</span></span>
   
    ![Vývoj aplikace Java JSP](./media/documentdb-java-application/image10.png)
2. <span data-ttu-id="f1807-139">Zadejte název projektu v hello **název projektu** pole a v hello **cílový modul Runtime** rozevírací nabídky, volitelně vyberte hodnotu (např. Apache Tomcat v7.0) a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f1807-139">Enter a project name in hello **Project name** box, and in hello **Target Runtime** drop-down menu, optionally select a value (e.g. Apache Tomcat v7.0), and then click **Finish**.</span></span> <span data-ttu-id="f1807-140">Výběr cílový modul runtime umožňuje vám toorun projektu místně prostřednictvím Eclipse.</span><span class="sxs-lookup"><span data-stu-id="f1807-140">Selecting a target runtime enables you toorun your project locally through Eclipse.</span></span>
3. <span data-ttu-id="f1807-141">V prostředí Eclipse v zobrazení Project Exploreru hello rozbalte projekt.</span><span class="sxs-lookup"><span data-stu-id="f1807-141">In Eclipse, in hello Project Explorer view, expand your project.</span></span> <span data-ttu-id="f1807-142">Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).</span><span class="sxs-lookup"><span data-stu-id="f1807-142">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
4. <span data-ttu-id="f1807-143">V hello **nový soubor JSP** dialogové okno, název souboru hello **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="f1807-143">In hello **New JSP File** dialog box, name hello file **index.jsp**.</span></span> <span data-ttu-id="f1807-144">Zachovat hello nadřazené složky jako **WebContent**, jak ukazuje následující obrázek hello a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f1807-144">Keep hello parent folder as **WebContent**, as shown in hello following illustration, and then click **Next**.</span></span>
   
    ![Vytvoření nového souboru JSP – kurz vývoje aplikace Java](./media/documentdb-java-application/image11.png)
5. <span data-ttu-id="f1807-146">V hello **vybrat šablonu JSP** dialogové okno, hello pro účely tohoto kurzu možnost **nový soubor JSP (html)**a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f1807-146">In hello **Select JSP Template** dialog box, for hello purpose of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
6. <span data-ttu-id="f1807-147">Když se soubor index.jsp hello se otevře v prostředí Eclipse, přidejte text toodisplay **Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="f1807-147">When hello index.jsp file opens in Eclipse, add text toodisplay **Hello World!**</span></span> <span data-ttu-id="f1807-148">v rámci existující hello <body> elementu.</span><span class="sxs-lookup"><span data-stu-id="f1807-148">within hello existing <body> element.</span></span> <span data-ttu-id="f1807-149">Aktualizovaný <body> obsah by měl vypadat jako hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="f1807-149">Your updated <body> content should look like hello following code:</span></span>
   
        <body>
            <% out.println("Hello World!"); %>
        </body>
7. <span data-ttu-id="f1807-150">Uložte soubor index.jsp hello.</span><span class="sxs-lookup"><span data-stu-id="f1807-150">Save hello index.jsp file.</span></span>
8. <span data-ttu-id="f1807-151">Pokud v kroku 2 nastavíte cílový modul runtime, můžete kliknout na **projektu** a potom **spustit** toorun aplikaci JSP místně:</span><span class="sxs-lookup"><span data-stu-id="f1807-151">If you set a target runtime in step 2, you can click **Project** and then **Run** toorun your JSP application locally:</span></span>
   
    ![Hello World – kurz aplikace Java](./media/documentdb-java-application/image12.png)

## <span data-ttu-id="f1807-153"><a id="InstallSDK"></a>Krok 3: Instalace hello DocumentDB Java SDK</span><span class="sxs-lookup"><span data-stu-id="f1807-153"><a id="InstallSDK"></a>Step 3: Install hello DocumentDB Java SDK</span></span>
<span data-ttu-id="f1807-154">Hello nejjednodušší způsob, jak toopull v hello DocumentDB Java SDK a jeho závislé součásti je prostřednictvím [Apache Maven](http://maven.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="f1807-154">hello easiest way toopull in hello DocumentDB Java SDK and its dependencies is through [Apache Maven](http://maven.apache.org/).</span></span>

<span data-ttu-id="f1807-155">toodo, budete potřebovat tooconvert projekt maven tooa projektu provedením hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f1807-155">toodo this, you will need tooconvert your project tooa maven project by completing hello following steps:</span></span>

1. <span data-ttu-id="f1807-156">Klikněte pravým tlačítkem na projekt v Project Exploreru hello, klikněte na tlačítko **konfigurace**, klikněte na tlačítko **převést tooMaven projektu**.</span><span class="sxs-lookup"><span data-stu-id="f1807-156">Right-click your project in hello Project Explorer, click **Configure**, click **Convert tooMaven Project**.</span></span>
2. <span data-ttu-id="f1807-157">V hello **vytvořit nový POM** okno, přijměte výchozí hodnoty hello a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f1807-157">In hello **Create new POM** window, accept hello defaults and click **Finish**.</span></span>
3. <span data-ttu-id="f1807-158">V **Project Exploreru**, otevřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="f1807-158">In **Project Explorer**, open hello pom.xml file.</span></span>
4. <span data-ttu-id="f1807-159">Na hello **závislosti** na kartě hello **závislosti** podokně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1807-159">On hello **Dependencies** tab, in hello **Dependencies** pane, click **Add**.</span></span>
5. <span data-ttu-id="f1807-160">V hello **vyberte závislost** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="f1807-160">In hello **Select Dependency** window, do hello following:</span></span>
   
   * <span data-ttu-id="f1807-161">V hello **Id skupiny** zadejte com.microsoft.azure.</span><span class="sxs-lookup"><span data-stu-id="f1807-161">In hello **Group Id** box, enter com.microsoft.azure.</span></span>
   * <span data-ttu-id="f1807-162">V hello **Id artefaktů** zadejte azure-documentdb.</span><span class="sxs-lookup"><span data-stu-id="f1807-162">In hello **Artifact Id** box, enter azure-documentdb.</span></span>
   * <span data-ttu-id="f1807-163">V hello **verze** zadejte 1.5.1.</span><span class="sxs-lookup"><span data-stu-id="f1807-163">In hello **Version** box, enter 1.5.1.</span></span>
     
   ![Instalace DocumentDB Java Application SDK](./media/documentdb-java-application/image13.png)
     
   * <span data-ttu-id="f1807-165">Nebo přidejte XML závislosti hello Id skupiny a Id artefaktů přímo toohello pom.xml pomocí textového editoru:</span><span class="sxs-lookup"><span data-stu-id="f1807-165">Or add hello dependency XML for Group Id and Artifact Id directly toohello pom.xml via a text editor:</span></span>
     
        <span data-ttu-id="f1807-166"><dependency><groupId>com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version></dependency></span><span class="sxs-lookup"><span data-stu-id="f1807-166"><dependency> <groupId>com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version> </dependency></span></span>
6. <span data-ttu-id="f1807-167">Klikněte na tlačítko **OK** a Maven nainstaluje DocumentDB Java SDK hello.</span><span class="sxs-lookup"><span data-stu-id="f1807-167">Click **OK** and Maven will install hello DocumentDB Java SDK.</span></span>
7. <span data-ttu-id="f1807-168">Uložte soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="f1807-168">Save hello pom.xml file.</span></span>

## <span data-ttu-id="f1807-169"><a id="UseService"></a>Krok 4: Využití služby Azure Cosmos DB hello v aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="f1807-169"><a id="UseService"></a>Step 4: Using hello Azure Cosmos DB service in a Java application</span></span>
1. <span data-ttu-id="f1807-170">Nejdříve definujme objekt TodoItem hello v TodoItem.java:</span><span class="sxs-lookup"><span data-stu-id="f1807-170">First, let's define hello TodoItem object in TodoItem.java:</span></span>
   
        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }
   
    <span data-ttu-id="f1807-171">V tomto projektu používáme [Project Lombok](http://projectlombok.org/) toogenerate hello konstruktor, metody getter, setter a tvůrce.</span><span class="sxs-lookup"><span data-stu-id="f1807-171">In this project, we are using [Project Lombok](http://projectlombok.org/) toogenerate hello constructor, getters, setters, and a builder.</span></span> <span data-ttu-id="f1807-172">Alternativně můžete tento kód napsat ručně nebo jej vygenerovat hello IDE.</span><span class="sxs-lookup"><span data-stu-id="f1807-172">Alternatively, you can write this code manually or have hello IDE generate it.</span></span>
2. <span data-ttu-id="f1807-173">Služba Azure Cosmos DB hello tooinvoke, musíte vytvořit instanci novou **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="f1807-173">tooinvoke hello Azure Cosmos DB service, you must instantiate a new **DocumentClient**.</span></span> <span data-ttu-id="f1807-174">Obecně platí, je nejlepší hello tooreuse **DocumentClient** – spíš než vytvořit nového klienta pro každý další požadavek.</span><span class="sxs-lookup"><span data-stu-id="f1807-174">In general, it is best tooreuse hello **DocumentClient** - rather than construct a new client for each subsequent request.</span></span> <span data-ttu-id="f1807-175">Hello klienta můžeme opakovaně nástrojem pro zabalení hello klienta v **DocumentClientFactory**.</span><span class="sxs-lookup"><span data-stu-id="f1807-175">We can reuse hello client by wrapping hello client in a **DocumentClientFactory**.</span></span> <span data-ttu-id="f1807-176">DocumentClientFactory.java, je nutné URI a PRIMARY KEY hodnotu serveru toopaste hello jste uložili tooyour schránky v [krok 1](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="f1807-176">In DocumentClientFactory.java, you need toopaste hello URI and PRIMARY KEY value you saved tooyour clipboard in [step 1](#CreateDB).</span></span> <span data-ttu-id="f1807-177">Nahraďte [YOUR\_ENDPOINT\_HERE] hodnotou URI a [YOUR\_KEY\_HERE] hodnotou PRIMARY KEY.</span><span class="sxs-lookup"><span data-stu-id="f1807-177">Replace [YOUR\_ENDPOINT\_HERE] with your URI and replace [YOUR\_KEY\_HERE] with your PRIMARY KEY.</span></span>
   
        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";
   
        private static DocumentClient documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
   
        public static DocumentClient getDocumentClient() {
            return documentClient;
        }
3. <span data-ttu-id="f1807-178">Nyní Vytvořme objekt DAO (Data Access) tooabstract, uložením naše tooAzure položky ToDo Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f1807-178">Now let's create a Data Access Object (DAO) tooabstract persisting our ToDo items tooAzure Cosmos DB.</span></span>
   
    <span data-ttu-id="f1807-179">Pořadí položek ToDo toosave tooa kolekce, hello klient potřebuje tooknow které databáze a kolekce toopersist příliš (jako odkazovaná odkazů na sebe sama).</span><span class="sxs-lookup"><span data-stu-id="f1807-179">In order toosave ToDo items tooa collection, hello client needs tooknow which database and collection toopersist too(as referenced by self-links).</span></span> <span data-ttu-id="f1807-180">Obecně platí, je nejlepší toocache hello databázi a kolekci, pokud je to možné tooavoid nadbytečným přístupům toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="f1807-180">In general, it is best toocache hello database and collection when possible tooavoid additional round-trips toohello database.</span></span>
   
    <span data-ttu-id="f1807-181">Hello následující kódu ukazuje, jak tooretrieve databázi a kolekci, pokud existuje, nebo vytvořte novou, pokud neexistuje:</span><span class="sxs-lookup"><span data-stu-id="f1807-181">hello following code illustrates how tooretrieve our database and collection, if it exists, or create a new one if it doesn't exist:</span></span>
   
        public class DocDbDao implements TodoDao {
            // hello name of our database.
            private static final String DATABASE_ID = "TodoDB";
   
            // hello name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";
   
            // hello Azure Cosmos DB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();
   
            // Cache for hello database object, so we don't have tooquery for it to
            // retrieve self links.
            private static Database databaseCache;
   
            // Cache for hello collection object, so we don't have tooquery for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;
   
            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get hello database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (databaseList.size() > 0) {
                        // Cache hello database object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create hello database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);
   
                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return databaseCache;
            }
   
            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get hello collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (collectionList.size() > 0) {
                        // Cache hello collection object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create hello collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);
   
                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return collectionCache;
            }
        }
4. <span data-ttu-id="f1807-182">dalším krokem Hello je toowrite některé hello toopersist kód TodoItems v kolekci toohello.</span><span class="sxs-lookup"><span data-stu-id="f1807-182">hello next step is toowrite some code toopersist hello TodoItems in toohello collection.</span></span> <span data-ttu-id="f1807-183">V tomto příkladu použijeme [Gson](https://code.google.com/p/google-gson/) tooserialize a deserializovat dokumenty tooJSON TodoItem Plain staré objekty Java (Pojo).</span><span class="sxs-lookup"><span data-stu-id="f1807-183">In this example, we will use [Gson](https://code.google.com/p/google-gson/) tooserialize and de-serialize TodoItem Plain Old Java Objects (POJOs) tooJSON documents.</span></span>
   
        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();
   
        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize hello TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));
   
            // Annotate hello document as a TodoItem for retrieval (so that we can
            // store multiple entity types in hello collection).
            todoItemDocument.set("entityType", "todoItem");
   
            try {
                // Persist hello document using hello DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
5. <span data-ttu-id="f1807-184">Podobně jako databáze a kolekce Azure Cosmos DB se i dokumenty odkazují pomocí odkazů na sebe sama.</span><span class="sxs-lookup"><span data-stu-id="f1807-184">Like Azure Cosmos DB databases and collections, documents are also referenced by self-links.</span></span> <span data-ttu-id="f1807-185">Hello následující pomocné funkce umožňuje nám načíst dokumenty jiný atribut (například "id"), spíš než vlastního odkazu:</span><span class="sxs-lookup"><span data-stu-id="f1807-185">hello following helper function lets us retrieve documents by another attribute (e.g. "id") rather than self-link:</span></span>
   
        private Document getDocumentById(String id) {
            // Retrieve hello document using hello DocumentClient.
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.id='" + id + "'", null)
                    .getQueryIterable().toList();
   
            if (documentList.size() > 0) {
                return documentList.get(0);
            } else {
                return null;
            }
        }
6. <span data-ttu-id="f1807-186">Jsme můžete použít metodu helper hello v kroku 5 tooretrieve dokumentu JSON objektu TodoItem podle id a jeho následné deserializaci tooa POJO:</span><span class="sxs-lookup"><span data-stu-id="f1807-186">We can use hello helper method in step 5 tooretrieve a TodoItem JSON document by id and then deserialize it tooa POJO:</span></span>
   
        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve hello document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);
   
            if (todoItemDocument != null) {
                // De-serialize hello document in tooa TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }
7. <span data-ttu-id="f1807-187">Také můžeme použít hello DocumentClient tooget kolekce nebo seznamu objektů Todoitem pomocí DocumentDB SQL:</span><span class="sxs-lookup"><span data-stu-id="f1807-187">We can also use hello DocumentClient tooget a collection or list of TodoItems using DocumentDB SQL:</span></span>
   
        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();
   
            // Retrieve hello TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();
   
            // De-serialize hello documents in tooTodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }
   
            return todoItems;
        }
8. <span data-ttu-id="f1807-188">Existuje mnoho způsobů tooupdate dokument s hello DocumentClient.</span><span class="sxs-lookup"><span data-stu-id="f1807-188">There are many ways tooupdate a document with hello DocumentClient.</span></span> <span data-ttu-id="f1807-189">V naší aplikaci seznamu úkolů Chceme mít tootoggle toobe informaci, jestli dokončení úkolu.</span><span class="sxs-lookup"><span data-stu-id="f1807-189">In our Todo list application, we want toobe able tootoggle whether a TodoItem is complete.</span></span> <span data-ttu-id="f1807-190">Toho lze dosáhnout aktualizací atributu "úplná" hello v rámci hello dokumentu:</span><span class="sxs-lookup"><span data-stu-id="f1807-190">This can be achieved by updating hello "complete" attribute within hello document:</span></span>
   
        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve hello document from hello database
            Document todoItemDocument = getDocumentById(id);
   
            // You can update hello document as a JSON document directly.
            // For more complex operations - you could de-serialize hello document in
            // tooa POJO, update hello POJO, and then re-serialize hello POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);
   
            try {
                // Persist/replace hello updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
9. <span data-ttu-id="f1807-191">Nakonec chcete mít možnost toodelete hello úkolu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="f1807-191">Finally, we want hello ability toodelete a TodoItem from our list.</span></span> <span data-ttu-id="f1807-192">toodo, můžeme použít metodu helper hello jsme napsali dříve tooretrieve hello vlastního odkazu a potom sdělte hello klienta toodelete ho:</span><span class="sxs-lookup"><span data-stu-id="f1807-192">toodo this, we can use hello helper method we wrote earlier tooretrieve hello self-link and then tell hello client toodelete it:</span></span>
   
        @Override
        public boolean deleteTodoItem(String id) {
            // Azure Cosmos DB refers toodocuments by self link rather than id.
   
            // Query for hello document tooretrieve hello self link.
            Document todoItemDocument = getDocumentById(id);
   
            try {
                // Delete hello document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }
   
            return true;
        }

## <span data-ttu-id="f1807-193"><a id="Wire"></a>Krok 5: Vzájemné propojení zbytku hello hello projektu vývoje aplikace Java společně</span><span class="sxs-lookup"><span data-stu-id="f1807-193"><a id="Wire"></a>Step 5: Wiring hello rest of hello of Java application development project together</span></span>
<span data-ttu-id="f1807-194">Teď, když jsme dokončili hello fun bits - již zbývá je toobuild rychlé uživatelské rozhraní a propojit je s až tooour DAO.</span><span class="sxs-lookup"><span data-stu-id="f1807-194">Now that we've finished hello fun bits - all that's left is toobuild a quick user interface and wire it up tooour DAO.</span></span>

1. <span data-ttu-id="f1807-195">Nejdříve začneme vytvořením kontroleru toocall náš objekt DAO:</span><span class="sxs-lookup"><span data-stu-id="f1807-195">First, let's start with building a controller toocall our DAO:</span></span>
   
        public class TodoItemController {
            public static TodoItemController getInstance() {
                if (todoItemController == null) {
                    todoItemController = new TodoItemController(TodoDaoFactory.getDao());
                }
                return todoItemController;
            }
   
            private static TodoItemController todoItemController;
   
            private final TodoDao todoDao;
   
            TodoItemController(TodoDao todoDao) {
                this.todoDao = todoDao;
            }
   
            public TodoItem createTodoItem(@NonNull String name,
                    @NonNull String category, boolean isComplete) {
                TodoItem todoItem = TodoItem.builder().name(name).category(category)
                        .complete(isComplete).build();
                return todoDao.createTodoItem(todoItem);
            }
   
            public boolean deleteTodoItem(@NonNull String id) {
                return todoDao.deleteTodoItem(id);
            }
   
            public TodoItem getTodoItemById(@NonNull String id) {
                return todoDao.readTodoItem(id);
            }
   
            public List<TodoItem> getTodoItems() {
                return todoDao.readTodoItems();
            }
   
            public TodoItem updateTodoItem(@NonNull String id, boolean isComplete) {
                return todoDao.updateTodoItem(id, isComplete);
            }
        }
   
    <span data-ttu-id="f1807-196">Ve složitější aplikaci může hello kontroler obsahovat komplikovanou obchodní logiku nad hello DAO.</span><span class="sxs-lookup"><span data-stu-id="f1807-196">In a more complex application, hello controller may house complicated business logic on top of hello DAO.</span></span>
2. <span data-ttu-id="f1807-197">V dalším kroku vytvoříme řadič servlet požadavky HTTP tooroute toohello:</span><span class="sxs-lookup"><span data-stu-id="f1807-197">Next, we'll create a servlet tooroute HTTP requests toohello controller:</span></span>
   
        public class TodoServlet extends HttpServlet {
            // API Keys
            public static final String API_METHOD = "method";
   
            // API Methods
            public static final String CREATE_TODO_ITEM = "createTodoItem";
            public static final String GET_TODO_ITEMS = "getTodoItems";
            public static final String UPDATE_TODO_ITEM = "updateTodoItem";
   
            // API Parameters
            public static final String TODO_ITEM_ID = "todoItemId";
            public static final String TODO_ITEM_NAME = "todoItemName";
            public static final String TODO_ITEM_CATEGORY = "todoItemCategory";
            public static final String TODO_ITEM_COMPLETE = "todoItemComplete";
   
            public static final String MESSAGE_ERROR_INVALID_METHOD = "{'error': 'Invalid method'}";
   
            private static final long serialVersionUID = 1L;
            private static final Gson gson = new Gson();
   
            @Override
            protected void doGet(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
   
                String apiResponse = MESSAGE_ERROR_INVALID_METHOD;
   
                TodoItemController todoItemController = TodoItemController
                        .getInstance();
   
                String id = request.getParameter(TODO_ITEM_ID);
                String name = request.getParameter(TODO_ITEM_NAME);
                String category = request.getParameter(TODO_ITEM_CATEGORY);
                boolean isComplete = StringUtils.equalsIgnoreCase("true",
                        request.getParameter(TODO_ITEM_COMPLETE)) ? true : false;
   
                switch (request.getParameter(API_METHOD)) {
                case CREATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.createTodoItem(name,
                            category, isComplete));
                    break;
                case GET_TODO_ITEMS:
                    apiResponse = gson.toJson(todoItemController.getTodoItems());
                    break;
                case UPDATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.updateTodoItem(id,
                            isComplete));
                    break;
                default:
                    break;
                }
   
                response.getWriter().println(apiResponse);
            }
   
            @Override
            protected void doPost(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
                doGet(request, response);
            }
        }
3. <span data-ttu-id="f1807-198">Budeme potřebovat webové uživatelské rozhraní toodisplay toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1807-198">We'll need a web user interface toodisplay toohello user.</span></span> <span data-ttu-id="f1807-199">Znovu napište hello index.jsp, že jsme vytvořili výše:</span><span class="sxs-lookup"><span data-stu-id="f1807-199">Let's re-write hello index.jsp we created earlier:</span></span>
    ```html
        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure Cosmos DB Java Sample</title>
   
          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
   
          <style>
            /* Add padding toobody for fixed nav bar */
            body {
              padding-top: 50px;
            }
          </style>
        </head>
        <body>
          <!-- Nav Bar -->
          <div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
            <div class="container">
              <div class="navbar-header">
                <a class="navbar-brand" href="#">My Tasks</a>
              </div>
            </div>
          </div>
   
          <!-- Body -->
          <div class="container">
            <h1>My ToDo List</h1>
   
            <hr/>
   
            <!-- hello ToDo List -->
            <div class = "todoList">
              <table class="table table-bordered table-striped" id="todoItems">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Category</th>
                    <th>Complete</th>
                  </tr>
                </thead>
                <tbody>
                </tbody>
              </table>
   
              <!-- Update Button -->
              <div class="todoUpdatePanel">
                <form class="form-horizontal" role="form">
                  <button type="button" class="btn btn-primary">Update Tasks</button>
                </form>
              </div>
   
            </div>
   
            <hr/>
   
            <!-- Item Input Form -->
            <div class="todoForm">
              <form class="form-horizontal" role="form">
                <div class="form-group">
                  <label for="inputItemName" class="col-sm-2">Task Name</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemName" placeholder="Enter name">
                  </div>
                </div>
   
                <div class="form-group">
                  <label for="inputItemCategory" class="col-sm-2">Task Category</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemCategory" placeholder="Enter category">
                  </div>
                </div>
   
                <button type="button" class="btn btn-primary">Add Task</button>
              </form>
            </div>
   
          </div>
   
          <!-- Placed at hello end of hello document so hello pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>
    ```
4. <span data-ttu-id="f1807-200">A nakonec zápis některých klienta JavaScript tootie hello webové uživatelské rozhraní a společně hello servletem:</span><span class="sxs-lookup"><span data-stu-id="f1807-200">And finally, write some client-side JavaScript tootie hello web user interface and hello servlet together:</span></span>
   
        var todoApp = {
          /*
           * API methods toocall Java backend.
           */
          apiEndpoint: "api",
   
          createTodoItem: function(name, category, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "createTodoItem",
                "todoItemName": name,
                "todoItemCategory": category,
                "todoItemComplete": isComplete
              },
              function(data) {
                var todoItem = data;
                todoApp.addTodoItemToTable(todoItem.id, todoItem.name, todoItem.category, todoItem.complete);
              },
              "json");
          },
   
          getTodoItems: function() {
            $.post(todoApp.apiEndpoint, {
                "method": "getTodoItems"
              },
              function(data) {
                var todoItemArr = data;
                $.each(todoItemArr, function(index, value) {
                  todoApp.addTodoItemToTable(value.id, value.name, value.category, value.complete);
                });
              },
              "json");
          },
   
          updateTodoItem: function(id, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "updateTodoItem",
                "todoItemId": id,
                "todoItemComplete": isComplete
              },
              function(data) {},
              "json");
          },
   
          /*
           * UI Methods
           */
          addTodoItemToTable: function(id, name, category, isComplete) {
            var rowColor = isComplete ? "active" : "warning";
   
            todoApp.ui_table().append($("<tr>")
              .append($("<td>").text(name))
              .append($("<td>").text(category))
              .append($("<td>")
                .append($("<input>")
                  .attr("type", "checkbox")
                  .attr("id", id)
                  .attr("checked", isComplete)
                  .attr("class", "isComplete")
                ))
              .addClass(rowColor)
            );
          },
   
          /*
           * UI Bindings
           */
          bindCreateButton: function() {
            todoApp.ui_createButton().click(function() {
              todoApp.createTodoItem(todoApp.ui_createNameInput().val(), todoApp.ui_createCategoryInput().val(), false);
              todoApp.ui_createNameInput().val("");
              todoApp.ui_createCategoryInput().val("");
            });
          },
   
          bindUpdateButton: function() {
            todoApp.ui_updateButton().click(function() {
              // Disable button temporarily.
              var myButton = $(this);
              var originalText = myButton.text();
              $(this).text("Updating...");
              $(this).prop("disabled", true);
   
              // Call api tooupdate todo items.
              $.each(todoApp.ui_updateId(), function(index, value) {
                todoApp.updateTodoItem(value.name, value.value);
                $(value).remove();
              });
   
              // Re-enable button.
              setTimeout(function() {
                myButton.prop("disabled", false);
                myButton.text(originalText);
              }, 500);
            });
          },
   
          bindUpdateCheckboxes: function() {
            todoApp.ui_table().on("click", ".isComplete", function(event) {
              var checkboxElement = $(event.currentTarget);
              var rowElement = $(event.currentTarget).parents('tr');
              var id = checkboxElement.attr('id');
              var isComplete = checkboxElement.is(':checked');
   
              // Toggle table row color
              if (isComplete) {
                rowElement.addClass("active");
                rowElement.removeClass("warning");
              } else {
                rowElement.removeClass("active");
                rowElement.addClass("warning");
              }
   
              // Update hidden inputs for update panel.
              todoApp.ui_updateForm().children("input[name='" + id + "']").remove();
   
              todoApp.ui_updateForm().append($("<input>")
                .attr("type", "hidden")
                .attr("class", "updateComplete")
                .attr("name", id)
                .attr("value", isComplete));
   
            });
          },
   
          /*
           * UI Elements
           */
          ui_createNameInput: function() {
            return $(".todoForm #inputItemName");
          },
   
          ui_createCategoryInput: function() {
            return $(".todoForm #inputItemCategory");
          },
   
          ui_createButton: function() {
            return $(".todoForm button");
          },
   
          ui_table: function() {
            return $(".todoList table tbody");
          },
   
          ui_updateButton: function() {
            return $(".todoUpdatePanel button");
          },
   
          ui_updateForm: function() {
            return $(".todoUpdatePanel form");
          },
   
          ui_updateId: function() {
            return $(".todoUpdatePanel .updateComplete");
          },
   
          /*
           * Install hello TodoApp
           */
          install: function() {
            todoApp.bindCreateButton();
            todoApp.bindUpdateButton();
            todoApp.bindUpdateCheckboxes();
   
            todoApp.getTodoItems();
          }
        };
   
        $(document).ready(function() {
          todoApp.install();
        });
5. <span data-ttu-id="f1807-201">Skvělé!</span><span class="sxs-lookup"><span data-stu-id="f1807-201">Awesome!</span></span> <span data-ttu-id="f1807-202">Nyní již zbývá je tootest hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1807-202">Now all that's left is tootest hello application.</span></span> <span data-ttu-id="f1807-203">Místní spuštění aplikace hello a přidejte několik položek Todo vyplněním hello názvů a kategorie položek a kliknutím na tlačítko **přidat úloha**.</span><span class="sxs-lookup"><span data-stu-id="f1807-203">Run hello application locally, and add some Todo items by filling in hello item name and category and clicking **Add Task**.</span></span>
6. <span data-ttu-id="f1807-204">Jakmile se zobrazí položka hello, můžete aktualizovat, zda je dokončená, přepínáním přepnutím hello políčka a kliknutím na **aktualizace úlohy**.</span><span class="sxs-lookup"><span data-stu-id="f1807-204">Once hello item appears, you can update whether it's complete by toggling hello checkbox and clicking **Update Tasks**.</span></span>

## <span data-ttu-id="f1807-205"><a id="Deploy"></a>Krok 6: Nasazení vaší tooAzure aplikace Java webů</span><span class="sxs-lookup"><span data-stu-id="f1807-205"><a id="Deploy"></a>Step 6: Deploy your Java application tooAzure Web Sites</span></span>
<span data-ttu-id="f1807-206">Díky weby Azure je nasazování aplikací Java stejně snadné jako Export aplikace jako souboru WAR a jeho nahrání buď přes řízení zdrojů (např. Git) nebo FTP.</span><span class="sxs-lookup"><span data-stu-id="f1807-206">Azure Web Sites makes deploying Java applications as simple as exporting your application as a WAR file and either uploading it via source control (e.g. Git) or FTP.</span></span>

1. <span data-ttu-id="f1807-207">tooexport aplikace jako souboru WAR, klikněte pravým tlačítkem na projekt v **Project Exploreru**, klikněte na tlačítko **exportovat**a potom klikněte na **soubor WAR**.</span><span class="sxs-lookup"><span data-stu-id="f1807-207">tooexport your application as a WAR file, right-click on your project in **Project Explorer**, click **Export**, and then click **WAR File**.</span></span>
2. <span data-ttu-id="f1807-208">V hello **WAR Export** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="f1807-208">In hello **WAR Export** window, do hello following:</span></span>
   
   * <span data-ttu-id="f1807-209">Hello webového projektu pole zadejte azure-documentdb-java-sample.</span><span class="sxs-lookup"><span data-stu-id="f1807-209">In hello Web project box, enter azure-documentdb-java-sample.</span></span>
   * <span data-ttu-id="f1807-210">Hello cílového pole zvolte cílový soubor WAR hello toosave.</span><span class="sxs-lookup"><span data-stu-id="f1807-210">In hello Destination box, choose a destination toosave hello WAR file.</span></span>
   * <span data-ttu-id="f1807-211">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f1807-211">Click **Finish**.</span></span>
3. <span data-ttu-id="f1807-212">Teď, když máte soubor WAR v ručně, můžete tento soubor jednoduše nahrát tooyour webu Azure na **webapps** adresáře.</span><span class="sxs-lookup"><span data-stu-id="f1807-212">Now that you have a WAR file in hand, you can simply upload it tooyour Azure Web Site's **webapps** directory.</span></span> <span data-ttu-id="f1807-213">Pokyny pro nahrání souboru hello, v tématu [přidat tooAzure aplikace Java App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="f1807-213">For instructions on uploading hello file, see [Add a Java application tooAzure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span></span>
   
    <span data-ttu-id="f1807-214">Jakmile soubor WAR hello nahrané toohello adresáře webapps, hello běhové prostředí zjistí, že jste jej přidali a automaticky ho načte.</span><span class="sxs-lookup"><span data-stu-id="f1807-214">Once hello WAR file is uploaded toohello webapps directory, hello runtime environment will detect that you've added it and will automatically load it.</span></span>
4. <span data-ttu-id="f1807-215">tooview hotový produkt, přejděte toohttp://YOUR\_lokality\_NAME.azurewebsites.net/azure-java-sample/ a začněte přidávat úkoly!</span><span class="sxs-lookup"><span data-stu-id="f1807-215">tooview your finished product, navigate toohttp://YOUR\_SITE\_NAME.azurewebsites.net/azure-java-sample/ and start adding your tasks!</span></span>

## <span data-ttu-id="f1807-216"><a id="GetProject"></a>Získat hello projektu z Githubu</span><span class="sxs-lookup"><span data-stu-id="f1807-216"><a id="GetProject"></a>Get hello project from GitHub</span></span>
<span data-ttu-id="f1807-217">Všechny ukázky hello v tomto kurzu jsou součástí hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="f1807-217">All hello samples in this tutorial are included in hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project on GitHub.</span></span> <span data-ttu-id="f1807-218">tooimport hello projekt todo do prostředí Eclipse, ujistěte se, máte hello software a prostředky uvedené v hello [požadavky](#Prerequisites) části a pak hello následující:</span><span class="sxs-lookup"><span data-stu-id="f1807-218">tooimport hello todo project into Eclipse, ensure you have hello software and resources listed in hello [Prerequisites](#Prerequisites) section, then do hello following:</span></span>

1. <span data-ttu-id="f1807-219">Nainstalujte [Project Lombok](http://projectlombok.org/).</span><span class="sxs-lookup"><span data-stu-id="f1807-219">Install [Project Lombok](http://projectlombok.org/).</span></span> <span data-ttu-id="f1807-220">Lombok je použité toogenerate konstruktory, metod getter a setter v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="f1807-220">Lombok is used toogenerate constructors, getters, setters in hello project.</span></span> <span data-ttu-id="f1807-221">Jakmile budete mít stažen soubor lombok.jar hello, klikněte dvakrát na jeho tooinstall ji nebo ji nainstalovat z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="f1807-221">Once you have downloaded hello lombok.jar file, double-click it tooinstall it or install it from hello command line.</span></span>
2. <span data-ttu-id="f1807-222">Pokud je prostředí Eclipse otevřené, zavřete ji a restartujte ji tooload Lombok.</span><span class="sxs-lookup"><span data-stu-id="f1807-222">If Eclipse is open, close it and restart it tooload Lombok.</span></span>
3. <span data-ttu-id="f1807-223">V prostředí Eclipse v hello **soubor** nabídky, klikněte na tlačítko **Import**.</span><span class="sxs-lookup"><span data-stu-id="f1807-223">In Eclipse, on hello **File** menu, click **Import**.</span></span>
4. <span data-ttu-id="f1807-224">V hello **Import** okně klikněte na tlačítko **Git**, klikněte na tlačítko **projekty z Gitu**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f1807-224">In hello **Import** window, click **Git**, click **Projects from Git**, and then click **Next**.</span></span>
5. <span data-ttu-id="f1807-225">Na hello **vybrat zdroj úložiště** obrazovky, klikněte na tlačítko **klon URI**.</span><span class="sxs-lookup"><span data-stu-id="f1807-225">On hello **Select Repository Source** screen, click **Clone URI**.</span></span>
6. <span data-ttu-id="f1807-226">Na hello **zdrojové úložiště Git** obrazovky v hello **URI** zadejte https://github.com/Azure-Samples/java-todo-app.git a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f1807-226">On hello **Source Git Repository** screen, in hello **URI** box, enter https://github.com/Azure-Samples/java-todo-app.git, and then click **Next**.</span></span>
7. <span data-ttu-id="f1807-227">Na hello **výběr větve** obrazovky, ujistěte se, že **hlavní** je vybrána a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f1807-227">On hello **Branch Selection** screen, ensure that **master** is selected, and then click **Next**.</span></span>
8. <span data-ttu-id="f1807-228">Na hello **místní cílovou** obrazovky, klikněte na tlačítko **Procházet** tooselect do složky, kam lze zkopírovat hello úložiště a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f1807-228">On hello **Local Destination** screen, click **Browse** tooselect a folder where hello repository can be copied, and then click **Next**.</span></span>
9. <span data-ttu-id="f1807-229">Na hello **vyberte toouse Průvodce pro import projektů** obrazovky, ujistěte se, že **importovat existující projekty** je vybrána a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f1807-229">On hello **Select a wizard toouse for importing projects** screen, ensure that **Import existing projects** is selected, and then click **Next**.</span></span>
10. <span data-ttu-id="f1807-230">Na hello **Import projektů** obrazovky, zrušit výběr hello **DocumentDB** projektu a pak klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f1807-230">On hello **Import Projects** screen, unselect hello **DocumentDB** project, and then click **Finish**.</span></span> <span data-ttu-id="f1807-231">Projekt DocumentDB Hello obsahuje hello Azure Cosmos DB Java SDK, kterou přidáme jako závislost místo.</span><span class="sxs-lookup"><span data-stu-id="f1807-231">hello DocumentDB project contains hello Azure Cosmos DB Java SDK, which we will add as a dependency instead.</span></span>
11. <span data-ttu-id="f1807-232">V **Project Exploreru**, přejděte tooazure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java a nahraďte hodnoty HOST a MASTER_KEY hello hello URI a primární klíč pro vaše Azure Cosmos DB účet a potom soubor uložit hello.</span><span class="sxs-lookup"><span data-stu-id="f1807-232">In **Project Explorer**, navigate tooazure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java and replace hello HOST and MASTER_KEY values with hello URI and PRIMARY KEY for your Azure Cosmos DB account, and then save hello file.</span></span> <span data-ttu-id="f1807-233">Další informace najdete v části [Krok 1. Vytvoření účtu Azure Cosmos DB databáze](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="f1807-233">For more information, see [Step 1. Create an Azure Cosmos DB database account](#CreateDB).</span></span>
12. <span data-ttu-id="f1807-234">V **Project Exploreru**, klikněte pravým tlačítkem na hello **azure-documentdb-java-sample**, klikněte na tlačítko **cesta sestavení**a pak klikněte na tlačítko **konfigurace cesta sestavení**.</span><span class="sxs-lookup"><span data-stu-id="f1807-234">In **Project Explorer**, right click hello **azure-documentdb-java-sample**, click **Build Path**, and then click **Configure Build Path**.</span></span>
13. <span data-ttu-id="f1807-235">Na hello **cesta sestavení Java** obrazovky v pravém podokně hello vyberte hello **knihovny** a pak klikněte **přidat externí JARs**.</span><span class="sxs-lookup"><span data-stu-id="f1807-235">On hello **Java Build Path** screen, in hello right pane, select hello **Libraries** tab, and then click **Add External JARs**.</span></span> <span data-ttu-id="f1807-236">Přejděte toohello umístění souboru lombok.jar hello a klikněte na tlačítko **otevřete**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1807-236">Navigate toohello location of hello lombok.jar file, and click **Open**, and then click **OK**.</span></span>
14. <span data-ttu-id="f1807-237">Pomocí kroku 12 tooopen hello **vlastnosti** znovu okno a v levém podokně hello klikněte na **Targeted Runtimes**.</span><span class="sxs-lookup"><span data-stu-id="f1807-237">Use step 12 tooopen hello **Properties** window again, and then in hello left pane click **Targeted Runtimes**.</span></span>
15. <span data-ttu-id="f1807-238">Na hello **Targeted Runtimes** obrazovky, klikněte na tlačítko **nový**, vyberte **Apache Tomcat v7.0**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1807-238">On hello **Targeted Runtimes** screen, click **New**, select **Apache Tomcat v7.0**, and then click **OK**.</span></span>
16. <span data-ttu-id="f1807-239">Pomocí kroku 12 tooopen hello **vlastnosti** znovu okno a v levém podokně hello klikněte na **omezující vlastnosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="f1807-239">Use step 12 tooopen hello **Properties** window again, and then in hello left pane click **Project Facets**.</span></span>
17. <span data-ttu-id="f1807-240">Na hello **omezující vlastnosti projektu** obrazovku, vyberte **dynamický webový modul** a **Java**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1807-240">On hello **Project Facets** screen, select **Dynamic Web Module** and **Java**, and then click **OK**.</span></span>
18. <span data-ttu-id="f1807-241">Na hello **servery** kartě dole hello úvodní obrazovka, klikněte pravým tlačítkem na **Tomcat v7.0 Server at localhost** a pak klikněte na **přidávat a odebírat**.</span><span class="sxs-lookup"><span data-stu-id="f1807-241">On hello **Servers** tab at hello bottom of hello screen, right-click **Tomcat v7.0 Server at localhost** and then click **Add and Remove**.</span></span>
19. <span data-ttu-id="f1807-242">Na hello **přidávat a odebírat** okně přesunout **azure-documentdb-java-sample** toohello **konfigurovaná** pole a pak klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f1807-242">On hello **Add and Remove** window, move **azure-documentdb-java-sample** toohello **Configured** box, and then click **Finish**.</span></span>
20. <span data-ttu-id="f1807-243">V hello **servery** kartě, klikněte pravým tlačítkem na **Tomcat v7.0 Server at localhost**a potom klikněte na **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="f1807-243">In hello **Servers** tab, right-click **Tomcat v7.0 Server at localhost**, and then click **Restart**.</span></span>
21. <span data-ttu-id="f1807-244">V prohlížeči přejděte toohttp://localhost:8080 / azure-documentdb-java-sample / a začněte přidávat položky seznamu úkolů tooyour.</span><span class="sxs-lookup"><span data-stu-id="f1807-244">In a browser, navigate toohttp://localhost:8080/azure-documentdb-java-sample/ and start adding tooyour task list.</span></span> <span data-ttu-id="f1807-245">Všimněte si, že pokud jste změnili výchozí port hodnoty, změňte hodnotu toohello 8080, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="f1807-245">Note that if you changed your default port values, change 8080 toohello value you selected.</span></span>
22. <span data-ttu-id="f1807-246">toodeploy tooan vašeho projektu webu Azure, najdete v části [kroku 6. Nasazení vaší aplikace tooAzure weby](#Deploy).</span><span class="sxs-lookup"><span data-stu-id="f1807-246">toodeploy your project tooan Azure web site, see [Step 6. Deploy your application tooAzure Web Sites](#Deploy).</span></span>

[1]: media/documentdb-java-application/keys.png
