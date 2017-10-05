---
title: "Kurz vývoje aplikace Java využívající službu Azure Cosmos DB | Dokumentace Microsoftu"
description: "Tento kurz webové aplikace Java popisuje, jak pomocí Azure Cosmos DB a rozhraní API DocumentDB ukládat a přistupovat k datům z aplikace Java hostované na webech Azure."
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
ms.openlocfilehash: 292115b5603c6f05a5eab3492d4b3e2096b58ed2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="build-a-java-web-application-using-azure-cosmos-db-and-the-documentdb-api"></a><span data-ttu-id="ca62c-104">Vytvoření webové aplikace Java pomocí Azure Cosmos DB a rozhraní API DocumentDB</span><span class="sxs-lookup"><span data-stu-id="ca62c-104">Build a Java web application using Azure Cosmos DB and the DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ca62c-105">.NET</span><span class="sxs-lookup"><span data-stu-id="ca62c-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="ca62c-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="ca62c-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="ca62c-107">Java</span><span class="sxs-lookup"><span data-stu-id="ca62c-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="ca62c-108">Python</span><span class="sxs-lookup"><span data-stu-id="ca62c-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="ca62c-109">Tento kurz webové aplikace Java se dozvíte, jak používat [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) služby k ukládání a přístup k datům z aplikace Java hostované v Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="ca62c-109">This Java web application tutorial shows you how to use the [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service to store and access data from a Java application hosted on Azure App Service Web Apps.</span></span> <span data-ttu-id="ca62c-110">V tomto tématu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="ca62c-110">In this topic, you will learn:</span></span>

* <span data-ttu-id="ca62c-111">Jak vytvořit základní aplikaci JavaServer stránky (JSP) v prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ca62c-111">How to build a basic JavaServer Pages (JSP) application in Eclipse.</span></span>
* <span data-ttu-id="ca62c-112">Jak pracovat se službou Azure Cosmos DB pomocí sady [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="ca62c-112">How to work with the Azure Cosmos DB service using the [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

<span data-ttu-id="ca62c-113">Tento kurz o aplikaci Java vám ukáže, jak vytvořit webovou aplikaci pro správu úkolů, která umožňuje vytvářet a získávat úkoly a označovat je jako dokončené, jak ilustruje následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="ca62c-113">This Java application tutorial shows you how to create a web-based task-management application that enables you to create, retrieve, and mark tasks as complete, as shown in the following image.</span></span> <span data-ttu-id="ca62c-114">Každý z úkolů v seznamu se ve službě Azure Cosmos DB ukládá jako dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="ca62c-114">Each of the tasks in the ToDo list are stored as JSON documents in Azure Cosmos DB.</span></span>

![Aplikace pro seznam úkolů v jazyce Java](./media/documentdb-java-application/image1.png)

> [!TIP]
> <span data-ttu-id="ca62c-116">V tomto kurzu vývoje aplikace se předpokládá, že již máte zkušenosti s jazykem Java.</span><span class="sxs-lookup"><span data-stu-id="ca62c-116">This application development tutorial assumes that you have prior experience using Java.</span></span> <span data-ttu-id="ca62c-117">Pokud je pro vás Java nebo některý z [požadovaných nástrojů](#Prerequisites) nový, doporučujeme stáhnout úplný ukázkový projekt [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) z GitHubu a postupovat podle [pokynů na konci tohoto článku](#GetProject).</span><span class="sxs-lookup"><span data-stu-id="ca62c-117">If you are new to Java or the [prerequisite tools](#Prerequisites), we recommend downloading the complete [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project from GitHub and building it using [the instructions at the end of this article](#GetProject).</span></span> <span data-ttu-id="ca62c-118">Až jej budete mít sestavený, můžete se k tomuto článku vrátit, abyste kódu lépe porozuměli v kontextu projektu.</span><span class="sxs-lookup"><span data-stu-id="ca62c-118">Once you have it built, you can review the article to gain insight on the code in the context of the project.</span></span>  
> 
> 

## <span data-ttu-id="ca62c-119"><a id="Prerequisites"></a>Předpoklady pro tento kurz webové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="ca62c-119"><a id="Prerequisites"></a>Prerequisites for this Java web application tutorial</span></span>
<span data-ttu-id="ca62c-120">Než zahájíte tento kurz vývoje aplikace, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="ca62c-120">Before you begin this application development tutorial, you must have the following:</span></span>

* <span data-ttu-id="ca62c-121">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ca62c-121">An active Azure account.</span></span> <span data-ttu-id="ca62c-122">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="ca62c-122">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ca62c-123">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ca62c-123">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/)</span></span>

    <span data-ttu-id="ca62c-124">NEBO</span><span class="sxs-lookup"><span data-stu-id="ca62c-124">OR</span></span>

    <span data-ttu-id="ca62c-125">Místní instalaci [emulátoru služby Azure Cosmos DB](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="ca62c-125">A local installation of the [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="ca62c-126">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)</span><span class="sxs-lookup"><span data-stu-id="ca62c-126">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* [<span data-ttu-id="ca62c-127">Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE</span><span class="sxs-lookup"><span data-stu-id="ca62c-127">Eclipse IDE for Java EE Developers.</span></span>](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
* [<span data-ttu-id="ca62c-128">Web s Azure s Java runtime environment (např. Tomcat nebo Jetty) povolen.</span><span class="sxs-lookup"><span data-stu-id="ca62c-128">An Azure Web Site with a Java runtime environment (e.g. Tomcat or Jetty) enabled.</span></span>](../app-service-web/web-sites-java-get-started.md)

<span data-ttu-id="ca62c-129">Pokud tyto nástroje instalujete poprvé, coreservlets.com poskytuje k procesu instalace návod v části Quick Start článku [Tutorial: Installing TomCat7 and Using it with Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) (Kurz: Instalace TomCat7 a jeho použití s Eclipse).</span><span class="sxs-lookup"><span data-stu-id="ca62c-129">If you're installing these tools for the first time, coreservlets.com provides a walk-through of the installation process in the Quick Start section of their [Tutorial: Installing TomCat7 and Using it with Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) article.</span></span>

## <span data-ttu-id="ca62c-130"><a id="CreateDB"></a>Krok 1: Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ca62c-130"><a id="CreateDB"></a>Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="ca62c-131">Začněme vytvořením účtu služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ca62c-131">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="ca62c-132">Pokud již účet máte nebo pokud používáte pro účely tohoto kurzu emulátor služby Azure Cosmos DB, můžete přeskočit na [Krok 2: Vytvoření aplikace Java JSP](#CreateJSP).</span><span class="sxs-lookup"><span data-stu-id="ca62c-132">If you already have an account or if you are using the Azure Cosmos DB Emulator for this tutorial, you can skip to [Step 2: Create the Java JSP application](#CreateJSP).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="ca62c-133"><a id="CreateJSP"></a>Krok 2: Vytvoření aplikace Java JSP</span><span class="sxs-lookup"><span data-stu-id="ca62c-133"><a id="CreateJSP"></a>Step 2: Create the Java JSP application</span></span>
<span data-ttu-id="ca62c-134">Vytvoření aplikace JSP:</span><span class="sxs-lookup"><span data-stu-id="ca62c-134">To create the JSP application:</span></span>

1. <span data-ttu-id="ca62c-135">Nejdříve začneme vytvořením projektu Java.</span><span class="sxs-lookup"><span data-stu-id="ca62c-135">First, we’ll start off by creating a Java project.</span></span> <span data-ttu-id="ca62c-136">Spusťte Eclipse, klikněte na **File** (Soubor), pak na **New** (Nový) a nakonec na **Dynamic Web Project** (Dynamický webový projekt).</span><span class="sxs-lookup"><span data-stu-id="ca62c-136">Start Eclipse, then click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="ca62c-137">Pokud se **Dynamic Web Project** v seznamu dostupných projektů nenachází, udělejte následující: klikněte na **File**, pak na **New**, dále na **Project** (Projekt), rozbalte **Web**, klikněte na **Dynamic Web Project** a nakonec na **Next** (Další). </span><span class="sxs-lookup"><span data-stu-id="ca62c-137">If you don’t see **Dynamic Web Project** listed as an available project, do the following: click **File**, click **New**, click **Project**…, expand **Web**, click **Dynamic Web Project**, and click **Next**.</span></span>
   
    ![Vývoj aplikace Java JSP](./media/documentdb-java-application/image10.png)
2. <span data-ttu-id="ca62c-139">Zadejte název projektu do pole **Project name** (Název projektu), volitelně v rozevírací nabídce **Target Runtime** (Cílový modul runtime) vyberte hodnotu (např. Apache Tomcat v7.0) a klikněte na **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="ca62c-139">Enter a project name in the **Project name** box, and in the **Target Runtime** drop-down menu, optionally select a value (e.g. Apache Tomcat v7.0), and then click **Finish**.</span></span> <span data-ttu-id="ca62c-140">Pokud vyberete cílový modul runtime, budete moci spouštět projekt místně přes Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ca62c-140">Selecting a target runtime enables you to run your project locally through Eclipse.</span></span>
3. <span data-ttu-id="ca62c-141">V prostředí Eclipse v zobrazení Project Explorer (Průzkumník projektů) rozbalte projekt.</span><span class="sxs-lookup"><span data-stu-id="ca62c-141">In Eclipse, in the Project Explorer view, expand your project.</span></span> <span data-ttu-id="ca62c-142">Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).</span><span class="sxs-lookup"><span data-stu-id="ca62c-142">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
4. <span data-ttu-id="ca62c-143">V dialogovém okně **New JSP File** (Nový soubor JSP) pojmenujte soubor **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-143">In the **New JSP File** dialog box, name the file **index.jsp**.</span></span> <span data-ttu-id="ca62c-144">Nadřazený adresář ponechte na **WebContent**, jak ukazuje následující ilustrace, a klikněte na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="ca62c-144">Keep the parent folder as **WebContent**, as shown in the following illustration, and then click **Next**.</span></span>
   
    ![Vytvoření nového souboru JSP – kurz vývoje aplikace Java](./media/documentdb-java-application/image11.png)
5. <span data-ttu-id="ca62c-146">V dialogovém okně **Select JSP Template** (Výběr šablony JSP) vyberte pro účely tohoto kurzu možnost **New JSP File (html)** (Nový soubor JSP (HTML)) a klikněte na **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="ca62c-146">In the **Select JSP Template** dialog box, for the purpose of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
6. <span data-ttu-id="ca62c-147">Až se soubor index.jsp otevře v prostředí Eclipse, přidejte text pro zobrazení **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="ca62c-147">When the index.jsp file opens in Eclipse, add text to display **Hello World!**</span></span> <span data-ttu-id="ca62c-148">do existujícího elementu <body>.</span><span class="sxs-lookup"><span data-stu-id="ca62c-148">within the existing <body> element.</span></span> <span data-ttu-id="ca62c-149">Aktualizovaný obsah <body> by se měl podobat následujícímu kódu:</span><span class="sxs-lookup"><span data-stu-id="ca62c-149">Your updated <body> content should look like the following code:</span></span>
   
        <body>
            <% out.println("Hello World!"); %>
        </body>
7. <span data-ttu-id="ca62c-150">Uložte soubor index.jsp.</span><span class="sxs-lookup"><span data-stu-id="ca62c-150">Save the index.jsp file.</span></span>
8. <span data-ttu-id="ca62c-151">Pokud v kroku 2 nastavíte cílový modul runtime, můžete kliknout na **Project** a pomocí příkazu **Run** (Spustit) aplikaci JSP místně spustit:</span><span class="sxs-lookup"><span data-stu-id="ca62c-151">If you set a target runtime in step 2, you can click **Project** and then **Run** to run your JSP application locally:</span></span>
   
    ![Hello World – kurz aplikace Java](./media/documentdb-java-application/image12.png)

## <span data-ttu-id="ca62c-153"><a id="InstallSDK"></a>Krok 3: Instalace sady DocumentDB Java SDK</span><span class="sxs-lookup"><span data-stu-id="ca62c-153"><a id="InstallSDK"></a>Step 3: Install the DocumentDB Java SDK</span></span>
<span data-ttu-id="ca62c-154">Nejjednodušším způsobem, jak stáhnout sadu DocumentDB Java SDK a její závislosti, je použít [Apache Maven](http://maven.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ca62c-154">The easiest way to pull in the DocumentDB Java SDK and its dependencies is through [Apache Maven](http://maven.apache.org/).</span></span>

<span data-ttu-id="ca62c-155">K tomu bude nutné převést projekt na projekt Maven. K tomu slouží následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ca62c-155">To do this, you will need to convert your project to a maven project by completing the following steps:</span></span>

1. <span data-ttu-id="ca62c-156">Klikněte pravým tlačítkem na projekt v Project Exploreru, pak klikněte na **Configure** (Konfigurovat) a následně na **Convert to Maven Project** (Převést na projekt Maven).</span><span class="sxs-lookup"><span data-stu-id="ca62c-156">Right-click your project in the Project Explorer, click **Configure**, click **Convert to Maven Project**.</span></span>
2. <span data-ttu-id="ca62c-157">V okně **Create new POM** (Vytvořit nový POM) přijměte výchozí hodnoty a klikněte na **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="ca62c-157">In the **Create new POM** window, accept the defaults and click **Finish**.</span></span>
3. <span data-ttu-id="ca62c-158">V **Project Exploreru** otevřete soubor pom.xml.</span><span class="sxs-lookup"><span data-stu-id="ca62c-158">In **Project Explorer**, open the pom.xml file.</span></span>
4. <span data-ttu-id="ca62c-159">Na kartě **Dependencies** (Závislosti) v podokně **Dependencies** klikněte na **Add** (Přidat).</span><span class="sxs-lookup"><span data-stu-id="ca62c-159">On the **Dependencies** tab, in the **Dependencies** pane, click **Add**.</span></span>
5. <span data-ttu-id="ca62c-160">V okně **Select Dependency** (Vybrat závislost) udělejte následující:</span><span class="sxs-lookup"><span data-stu-id="ca62c-160">In the **Select Dependency** window, do the following:</span></span>
   
   * <span data-ttu-id="ca62c-161">V **Id skupiny** zadejte com.microsoft.azure.</span><span class="sxs-lookup"><span data-stu-id="ca62c-161">In the **Group Id** box, enter com.microsoft.azure.</span></span>
   * <span data-ttu-id="ca62c-162">V **Id artefaktů** zadejte azure-documentdb.</span><span class="sxs-lookup"><span data-stu-id="ca62c-162">In the **Artifact Id** box, enter azure-documentdb.</span></span>
   * <span data-ttu-id="ca62c-163">V **verze** zadejte 1.5.1.</span><span class="sxs-lookup"><span data-stu-id="ca62c-163">In the **Version** box, enter 1.5.1.</span></span>
     
   ![Instalace DocumentDB Java Application SDK](./media/documentdb-java-application/image13.png)
     
   * <span data-ttu-id="ca62c-165">Nebo přidejte XML závislosti pro Id skupiny a Id artefaktů přímo do souboru pom.xml pomocí textového editoru:</span><span class="sxs-lookup"><span data-stu-id="ca62c-165">Or add the dependency XML for Group Id and Artifact Id directly to the pom.xml via a text editor:</span></span>
     
        <span data-ttu-id="ca62c-166"><dependency><groupId>com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version></dependency></span><span class="sxs-lookup"><span data-stu-id="ca62c-166"><dependency> <groupId>com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version> </dependency></span></span>
6. <span data-ttu-id="ca62c-167">Klikněte na tlačítko **OK** a Maven nainstaluje DocumentDB Java SDK.</span><span class="sxs-lookup"><span data-stu-id="ca62c-167">Click **OK** and Maven will install the DocumentDB Java SDK.</span></span>
7. <span data-ttu-id="ca62c-168">Uložte soubor pom.xml.</span><span class="sxs-lookup"><span data-stu-id="ca62c-168">Save the pom.xml file.</span></span>

## <span data-ttu-id="ca62c-169"><a id="UseService"></a>Krok 4: Využití služby Azure Cosmos DB v aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="ca62c-169"><a id="UseService"></a>Step 4: Using the Azure Cosmos DB service in a Java application</span></span>
1. <span data-ttu-id="ca62c-170">Nejdříve definujme objekt TodoItem v TodoItem.java:</span><span class="sxs-lookup"><span data-stu-id="ca62c-170">First, let's define the TodoItem object in TodoItem.java:</span></span>
   
        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }
   
    <span data-ttu-id="ca62c-171">V tomto projektu používáme [Project Lombok](http://projectlombok.org/), pomocí kterého generujeme konstruktor, metody getter a setter a tvůrce (builder).</span><span class="sxs-lookup"><span data-stu-id="ca62c-171">In this project, we are using [Project Lombok](http://projectlombok.org/) to generate the constructor, getters, setters, and a builder.</span></span> <span data-ttu-id="ca62c-172">Alternativně můžete tento kód napsat ručně nebo jej vygenerovat pomocí rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="ca62c-172">Alternatively, you can write this code manually or have the IDE generate it.</span></span>
2. <span data-ttu-id="ca62c-173">Abyste mohli vyvolat službu Azure Cosmos DB, musíte vytvořit novou instanci **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-173">To invoke the Azure Cosmos DB service, you must instantiate a new **DocumentClient**.</span></span> <span data-ttu-id="ca62c-174">Obecně je lépe opakovaně používat **DocumentClient** než pro každý další požadavek vytvářet nového klienta.</span><span class="sxs-lookup"><span data-stu-id="ca62c-174">In general, it is best to reuse the **DocumentClient** - rather than construct a new client for each subsequent request.</span></span> <span data-ttu-id="ca62c-175">Klienta můžeme opakovaně používat tak, že jej zabalíme do **DocumentClientFactory**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-175">We can reuse the client by wrapping the client in a **DocumentClientFactory**.</span></span> <span data-ttu-id="ca62c-176">V DocumentClientFactory.java, je třeba vložit hodnotu URI a PRIMARY KEY jste uložili do schránky v [krok 1](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="ca62c-176">In DocumentClientFactory.java, you need to paste the URI and PRIMARY KEY value you saved to your clipboard in [step 1](#CreateDB).</span></span> <span data-ttu-id="ca62c-177">Nahraďte [YOUR\_ENDPOINT\_HERE] hodnotou URI a [YOUR\_KEY\_HERE] hodnotou PRIMARY KEY.</span><span class="sxs-lookup"><span data-stu-id="ca62c-177">Replace [YOUR\_ENDPOINT\_HERE] with your URI and replace [YOUR\_KEY\_HERE] with your PRIMARY KEY.</span></span>
   
        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";
   
        private static DocumentClient documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
   
        public static DocumentClient getDocumentClient() {
            return documentClient;
        }
3. <span data-ttu-id="ca62c-178">Nyní vytvořme objekt pro přístup k datům (DAO), pomocí kterého zajistíme abstrakci uchovávání položek ToDo ve službě Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ca62c-178">Now let's create a Data Access Object (DAO) to abstract persisting our ToDo items to Azure Cosmos DB.</span></span>
   
    <span data-ttu-id="ca62c-179">Abychom mohli položky ToDo ukládat do kolekce, klient musí vědět, která databáze nebo kolekce se má k uchovávání použít (podle odkazů na sebe sama).</span><span class="sxs-lookup"><span data-stu-id="ca62c-179">In order to save ToDo items to a collection, the client needs to know which database and collection to persist to (as referenced by self-links).</span></span> <span data-ttu-id="ca62c-180">Obecně je nejlépe uložit databázi a kolekci do mezipaměti, kdykoli je to možné, aby se zamezilo nadbytečným přístupům do databáze.</span><span class="sxs-lookup"><span data-stu-id="ca62c-180">In general, it is best to cache the database and collection when possible to avoid additional round-trips to the database.</span></span>
   
    <span data-ttu-id="ca62c-181">Následující kód ukazuje, jak získat databázi a kolekci, pokud existuje, nebo vytvořit novou, pokud neexistuje:</span><span class="sxs-lookup"><span data-stu-id="ca62c-181">The following code illustrates how to retrieve our database and collection, if it exists, or create a new one if it doesn't exist:</span></span>
   
        public class DocDbDao implements TodoDao {
            // The name of our database.
            private static final String DATABASE_ID = "TodoDB";
   
            // The name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";
   
            // The Azure Cosmos DB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();
   
            // Cache for the database object, so we don't have to query for it to
            // retrieve self links.
            private static Database databaseCache;
   
            // Cache for the collection object, so we don't have to query for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;
   
            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get the database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (databaseList.size() > 0) {
                        // Cache the database object so we won't have to query for it
                        // later to retrieve the selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create the database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);
   
                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - the app wasn't
                            // able to query or create the collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return databaseCache;
            }
   
            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get the collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (collectionList.size() > 0) {
                        // Cache the collection object so we won't have to query for it
                        // later to retrieve the selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create the collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);
   
                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - the app wasn't
                            // able to query or create the collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return collectionCache;
            }
        }
4. <span data-ttu-id="ca62c-182">Dalším krokem je napsat kód, který uchová TodoItems v kolekci.</span><span class="sxs-lookup"><span data-stu-id="ca62c-182">The next step is to write some code to persist the TodoItems in to the collection.</span></span> <span data-ttu-id="ca62c-183">V tomto příkladu použijeme [Gson](https://code.google.com/p/google-gson/), pomocí kterého serializujeme a deserializujeme objekty TodoItem Plain Old Java Object (POJO) do dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="ca62c-183">In this example, we will use [Gson](https://code.google.com/p/google-gson/) to serialize and de-serialize TodoItem Plain Old Java Objects (POJOs) to JSON documents.</span></span>
   
        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();
   
        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize the TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));
   
            // Annotate the document as a TodoItem for retrieval (so that we can
            // store multiple entity types in the collection).
            todoItemDocument.set("entityType", "todoItem");
   
            try {
                // Persist the document using the DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
5. <span data-ttu-id="ca62c-184">Podobně jako databáze a kolekce Azure Cosmos DB se i dokumenty odkazují pomocí odkazů na sebe sama.</span><span class="sxs-lookup"><span data-stu-id="ca62c-184">Like Azure Cosmos DB databases and collections, documents are also referenced by self-links.</span></span> <span data-ttu-id="ca62c-185">Následující pomocná funkce nám umožní získat dokumenty podle jiného atributu (např. id) namísto odkazu na sebe sama:</span><span class="sxs-lookup"><span data-stu-id="ca62c-185">The following helper function lets us retrieve documents by another attribute (e.g. "id") rather than self-link:</span></span>
   
        private Document getDocumentById(String id) {
            // Retrieve the document using the DocumentClient.
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
6. <span data-ttu-id="ca62c-186">Pomocnou metodu z kroku 5 můžete využít k získání dokumentu JSON objektu TodoItem podle id a jeho následné deserializaci na objekt POJO:</span><span class="sxs-lookup"><span data-stu-id="ca62c-186">We can use the helper method in step 5 to retrieve a TodoItem JSON document by id and then deserialize it to a POJO:</span></span>
   
        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve the document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);
   
            if (todoItemDocument != null) {
                // De-serialize the document in to a TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }
7. <span data-ttu-id="ca62c-187">Také můžeme použít DocumentClient k získání kolekce nebo seznamu objektů TodoItem pomocí DocumentDB SQL:</span><span class="sxs-lookup"><span data-stu-id="ca62c-187">We can also use the DocumentClient to get a collection or list of TodoItems using DocumentDB SQL:</span></span>
   
        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();
   
            // Retrieve the TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();
   
            // De-serialize the documents in to TodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }
   
            return todoItems;
        }
8. <span data-ttu-id="ca62c-188">Existuje mnoho způsobů, jak pomocí DocumentClient aktualizovat dokument.</span><span class="sxs-lookup"><span data-stu-id="ca62c-188">There are many ways to update a document with the DocumentClient.</span></span> <span data-ttu-id="ca62c-189">V naší aplikaci seznamu úkolů chceme mít možnost určovat, zda je položka TodoItem dokončená.</span><span class="sxs-lookup"><span data-stu-id="ca62c-189">In our Todo list application, we want to be able to toggle whether a TodoItem is complete.</span></span> <span data-ttu-id="ca62c-190">Toho lze dosáhnout aktualizací atributu complete v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ca62c-190">This can be achieved by updating the "complete" attribute within the document:</span></span>
   
        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve the document from the database
            Document todoItemDocument = getDocumentById(id);
   
            // You can update the document as a JSON document directly.
            // For more complex operations - you could de-serialize the document in
            // to a POJO, update the POJO, and then re-serialize the POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);
   
            try {
                // Persist/replace the updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
9. <span data-ttu-id="ca62c-191">Nakonec chcete mít možnost odstranit TodoItem ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="ca62c-191">Finally, we want the ability to delete a TodoItem from our list.</span></span> <span data-ttu-id="ca62c-192">K tomu můžeme využít pomocnou metodu, kterou jsme napsali dříve, abychom získali odkaz na sebe sama a pak dali pokyn klientovi, aby jej odstranil:</span><span class="sxs-lookup"><span data-stu-id="ca62c-192">To do this, we can use the helper method we wrote earlier to retrieve the self-link and then tell the client to delete it:</span></span>
   
        @Override
        public boolean deleteTodoItem(String id) {
            // Azure Cosmos DB refers to documents by self link rather than id.
   
            // Query for the document to retrieve the self link.
            Document todoItemDocument = getDocumentById(id);
   
            try {
                // Delete the document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }
   
            return true;
        }

## <span data-ttu-id="ca62c-193"><a id="Wire"></a>Krok 5: Vzájemné propojení zbytku projektu vývoje aplikace Java</span><span class="sxs-lookup"><span data-stu-id="ca62c-193"><a id="Wire"></a>Step 5: Wiring the rest of the of Java application development project together</span></span>
<span data-ttu-id="ca62c-194">Teď, když jsme dokončili fun bits - již zbývá je sestavení rychlé uživatelské rozhraní a propojit je s objektem DAO.</span><span class="sxs-lookup"><span data-stu-id="ca62c-194">Now that we've finished the fun bits - all that's left is to build a quick user interface and wire it up to our DAO.</span></span>

1. <span data-ttu-id="ca62c-195">Nejdříve začneme vytvořením kontroleru, který bude náš objekt DAO volat:</span><span class="sxs-lookup"><span data-stu-id="ca62c-195">First, let's start with building a controller to call our DAO:</span></span>
   
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
   
    <span data-ttu-id="ca62c-196">Ve složitější aplikaci může kontroler obsahovat komplikovanou obchodní logiku pro práci s objektem DAO.</span><span class="sxs-lookup"><span data-stu-id="ca62c-196">In a more complex application, the controller may house complicated business logic on top of the DAO.</span></span>
2. <span data-ttu-id="ca62c-197">Dále vytvoříme servlet pro směrování požadavků HTTP na kontroler:</span><span class="sxs-lookup"><span data-stu-id="ca62c-197">Next, we'll create a servlet to route HTTP requests to the controller:</span></span>
   
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
3. <span data-ttu-id="ca62c-198">Budeme potřebovat webové uživatelské rozhraní zobrazit uživateli.</span><span class="sxs-lookup"><span data-stu-id="ca62c-198">We'll need a web user interface to display to the user.</span></span> <span data-ttu-id="ca62c-199">Přepišme soubor index.jsp, který jsme vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="ca62c-199">Let's re-write the index.jsp we created earlier:</span></span>
    ```html
        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure Cosmos DB Java Sample</title>
   
          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
   
          <style>
            /* Add padding to body for fixed nav bar */
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
   
            <!-- The ToDo List -->
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
   
          <!-- Placed at the end of the document so the pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>
    ```
4. <span data-ttu-id="ca62c-200">A nakonec napišme určitého kódu JavaScript na straně klienta ke svázání společně webové uživatelské rozhraní a se servletem:</span><span class="sxs-lookup"><span data-stu-id="ca62c-200">And finally, write some client-side JavaScript to tie the web user interface and the servlet together:</span></span>
   
        var todoApp = {
          /*
           * API methods to call Java backend.
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
   
              // Call api to update todo items.
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
           * Install the TodoApp
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
5. <span data-ttu-id="ca62c-201">Skvělé!</span><span class="sxs-lookup"><span data-stu-id="ca62c-201">Awesome!</span></span> <span data-ttu-id="ca62c-202">Nyní již zbývá aplikaci jen otestovat.</span><span class="sxs-lookup"><span data-stu-id="ca62c-202">Now all that's left is to test the application.</span></span> <span data-ttu-id="ca62c-203">Spusťte aplikaci místně a zadáním názvů a kategorie položek a kliknutím na **Add Task** (Přidat úkol) přidejte několik položek Todo.</span><span class="sxs-lookup"><span data-stu-id="ca62c-203">Run the application locally, and add some Todo items by filling in the item name and category and clicking **Add Task**.</span></span>
6. <span data-ttu-id="ca62c-204">Až se položka zobrazí, můžete aktualizovat, zda je dokončená, přepínáním zaškrtávacího políčka a kliknutím na **Update Tasks** (Aktualizovat úkoly).</span><span class="sxs-lookup"><span data-stu-id="ca62c-204">Once the item appears, you can update whether it's complete by toggling the checkbox and clicking **Update Tasks**.</span></span>

## <span data-ttu-id="ca62c-205"><a id="Deploy"></a>Krok 6: Nasazení aplikace v jazyce Java na weby Azure</span><span class="sxs-lookup"><span data-stu-id="ca62c-205"><a id="Deploy"></a>Step 6: Deploy your Java application to Azure Web Sites</span></span>
<span data-ttu-id="ca62c-206">Díky weby Azure je nasazování aplikací Java stejně snadné jako Export aplikace jako souboru WAR a jeho nahrání buď přes řízení zdrojů (např. Git) nebo FTP.</span><span class="sxs-lookup"><span data-stu-id="ca62c-206">Azure Web Sites makes deploying Java applications as simple as exporting your application as a WAR file and either uploading it via source control (e.g. Git) or FTP.</span></span>

1. <span data-ttu-id="ca62c-207">Aplikaci exportovat jako soubor WAR, klikněte pravým tlačítkem na projekt v **Project Exploreru**, klikněte na tlačítko **exportovat**a potom klikněte na **soubor WAR**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-207">To export your application as a WAR file, right-click on your project in **Project Explorer**, click **Export**, and then click **WAR File**.</span></span>
2. <span data-ttu-id="ca62c-208">V okně **WAR Export** udělejte následující:</span><span class="sxs-lookup"><span data-stu-id="ca62c-208">In the **WAR Export** window, do the following:</span></span>
   
   * <span data-ttu-id="ca62c-209">Do pole Web project (Webový projekt) zadejte azure-documentdb-java-sample.</span><span class="sxs-lookup"><span data-stu-id="ca62c-209">In the Web project box, enter azure-documentdb-java-sample.</span></span>
   * <span data-ttu-id="ca62c-210">V poli Destination (Cíl) vyberte cíl, do kterého se uloží soubor WAR.</span><span class="sxs-lookup"><span data-stu-id="ca62c-210">In the Destination box, choose a destination to save the WAR file.</span></span>
   * <span data-ttu-id="ca62c-211">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-211">Click **Finish**.</span></span>
3. <span data-ttu-id="ca62c-212">Teď, když máte soubor WAR v ručně, můžete tento soubor jednoduše nahrát do Azure webu **webapps** adresáře.</span><span class="sxs-lookup"><span data-stu-id="ca62c-212">Now that you have a WAR file in hand, you can simply upload it to your Azure Web Site's **webapps** directory.</span></span> <span data-ttu-id="ca62c-213">Pokyny pro nahrání souboru, v tématu [přidat aplikace v jazyce Java do Azure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="ca62c-213">For instructions on uploading the file, see [Add a Java application to Azure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span></span>
   
    <span data-ttu-id="ca62c-214">Až bude soubor WAR nahrán do adresáře webapps, běhové prostředí zjistí, že jste jej přidali, a automaticky ho načte.</span><span class="sxs-lookup"><span data-stu-id="ca62c-214">Once the WAR file is uploaded to the webapps directory, the runtime environment will detect that you've added it and will automatically load it.</span></span>
4. <span data-ttu-id="ca62c-215">Pokud chcete zobrazit hotový produkt, přejděte na http://YOUR\_lokality\_NAME.azurewebsites.net/azure-java-sample/ a začněte přidávat úkoly!</span><span class="sxs-lookup"><span data-stu-id="ca62c-215">To view your finished product, navigate to http://YOUR\_SITE\_NAME.azurewebsites.net/azure-java-sample/ and start adding your tasks!</span></span>

## <span data-ttu-id="ca62c-216"><a id="GetProject"></a>Získání projektu z Githubu</span><span class="sxs-lookup"><span data-stu-id="ca62c-216"><a id="GetProject"></a>Get the project from GitHub</span></span>
<span data-ttu-id="ca62c-217">Všechny ukázky v tomto kurzu jsou součástí projektu [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="ca62c-217">All the samples in this tutorial are included in the [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project on GitHub.</span></span> <span data-ttu-id="ca62c-218">Pokud chcete importovat projekt todo do prostředí Eclipse, ujistěte se, že máte software a prostředky uvedené v části [Předpoklady](#Prerequisites), a udělejte následující:</span><span class="sxs-lookup"><span data-stu-id="ca62c-218">To import the todo project into Eclipse, ensure you have the software and resources listed in the [Prerequisites](#Prerequisites) section, then do the following:</span></span>

1. <span data-ttu-id="ca62c-219">Nainstalujte [Project Lombok](http://projectlombok.org/).</span><span class="sxs-lookup"><span data-stu-id="ca62c-219">Install [Project Lombok](http://projectlombok.org/).</span></span> <span data-ttu-id="ca62c-220">Lombok slouží ke generování konstruktorů a metod getter a setter v projektu.</span><span class="sxs-lookup"><span data-stu-id="ca62c-220">Lombok is used to generate constructors, getters, setters in the project.</span></span> <span data-ttu-id="ca62c-221">Jakmile budete mít stažen soubor lombok.jar, dvakrát na něj klikněte, aby se nainstaloval, nebo jej nainstalujte z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ca62c-221">Once you have downloaded the lombok.jar file, double-click it to install it or install it from the command line.</span></span>
2. <span data-ttu-id="ca62c-222">Pokud je prostředí Eclipse otevřené, zavřete ho a znovu ho spusťte, aby se načetl Lombok.</span><span class="sxs-lookup"><span data-stu-id="ca62c-222">If Eclipse is open, close it and restart it to load Lombok.</span></span>
3. <span data-ttu-id="ca62c-223">V prostředí Eclipse v nabídce **File** (Soubor) klikněte na **Import**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-223">In Eclipse, on the **File** menu, click **Import**.</span></span>
4. <span data-ttu-id="ca62c-224">V okně **Import** klikněte na **Git**, pak na **Projects from Git** (Projekty z Gitu) a nakonec na **Next** (Další).</span><span class="sxs-lookup"><span data-stu-id="ca62c-224">In the **Import** window, click **Git**, click **Projects from Git**, and then click **Next**.</span></span>
5. <span data-ttu-id="ca62c-225">Na obrazovce **Select Repository Source** (Výběr zdroje úložiště) klikněte na **Clone URI** (Klonovat URI).</span><span class="sxs-lookup"><span data-stu-id="ca62c-225">On the **Select Repository Source** screen, click **Clone URI**.</span></span>
6. <span data-ttu-id="ca62c-226">Na **zdrojové úložiště Git** obrazovce **URI** zadejte https://github.com/Azure-Samples/java-todo-app.git a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-226">On the **Source Git Repository** screen, in the **URI** box, enter https://github.com/Azure-Samples/java-todo-app.git, and then click **Next**.</span></span>
7. <span data-ttu-id="ca62c-227">Na obrazovce **Branch Selection** (Výběr větve) se ujistěte, že je zvolena možnost **master** (hlavní), a klikněte na **Next**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-227">On the **Branch Selection** screen, ensure that **master** is selected, and then click **Next**.</span></span>
8. <span data-ttu-id="ca62c-228">Na obrazovce **Local Destination** (Místní cíl) klikněte na **Browse** (Procházet), vyberte složku, do které lze úložiště zkopírovat, a pak klikněte na **Next**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-228">On the **Local Destination** screen, click **Browse** to select a folder where the repository can be copied, and then click **Next**.</span></span>
9. <span data-ttu-id="ca62c-229">Na obrazovce **Select a wizard to use for importing projects** (Výběr průvodce, který se použije k importování projektů) se ujistěte, že je vybrána možnost **Import existing projects** (Import existujících projektů) a klikněte na **Next**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-229">On the **Select a wizard to use for importing projects** screen, ensure that **Import existing projects** is selected, and then click **Next**.</span></span>
10. <span data-ttu-id="ca62c-230">Na obrazovce **Import Projects** (Import projektů) zrušte výběr projektu **DocumentDB** a klikněte na **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="ca62c-230">On the **Import Projects** screen, unselect the **DocumentDB** project, and then click **Finish**.</span></span> <span data-ttu-id="ca62c-231">Projekt DocumentDB obsahuje sadu Azure Cosmos DB Java SDK, kterou přidáme jako závislost místo.</span><span class="sxs-lookup"><span data-stu-id="ca62c-231">The DocumentDB project contains the Azure Cosmos DB Java SDK, which we will add as a dependency instead.</span></span>
11. <span data-ttu-id="ca62c-232">V **Project Exploreru**, přejděte na Azure-documentdb-Java-sample\src\com.microsoft.Azure.documentdb.Sample.dao\DocumentClientFactory.Java a nahraďte hodnoty HOST a MASTER_KEY URI a primární klíč pro vaše Azure Cosmos DB účtu a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="ca62c-232">In **Project Explorer**, navigate to azure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java and replace the HOST and MASTER_KEY values with the URI and PRIMARY KEY for your Azure Cosmos DB account, and then save the file.</span></span> <span data-ttu-id="ca62c-233">Další informace najdete v části [Krok 1. Vytvoření účtu Azure Cosmos DB databáze](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="ca62c-233">For more information, see [Step 1. Create an Azure Cosmos DB database account](#CreateDB).</span></span>
12. <span data-ttu-id="ca62c-234">V **Project Exploreru** klikněte pravým tlačítkem na **azure-documentdb-java-sample**, pak levým na **Build Path** (Cesta sestavení) a nakonec na **Configure Build Path** (Konfigurovat cestu sestavení).</span><span class="sxs-lookup"><span data-stu-id="ca62c-234">In **Project Explorer**, right click the **azure-documentdb-java-sample**, click **Build Path**, and then click **Configure Build Path**.</span></span>
13. <span data-ttu-id="ca62c-235">Na obrazovce **Java Build Path** (Cesta sestavení Java) v pravém podokně vyberte kartu **Libraries** (Knihovny) a klikněte na **Add External JARs** (Přidat externí balíčky JAR).</span><span class="sxs-lookup"><span data-stu-id="ca62c-235">On the **Java Build Path** screen, in the right pane, select the **Libraries** tab, and then click **Add External JARs**.</span></span> <span data-ttu-id="ca62c-236">Přejděte na umístění souboru lombok.jar, klikněte na **Open** (Otevřít) a pak na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-236">Navigate to the location of the lombok.jar file, and click **Open**, and then click **OK**.</span></span>
14. <span data-ttu-id="ca62c-237">Pomocí kroku 12 otevřete znovu okno **Properties** (Vlastnosti) a v levém podokně klikněte na **Targeted Runtimes** (Cílené moduly runtime).</span><span class="sxs-lookup"><span data-stu-id="ca62c-237">Use step 12 to open the **Properties** window again, and then in the left pane click **Targeted Runtimes**.</span></span>
15. <span data-ttu-id="ca62c-238">Na obrazovce **Targeted Runtimes** klikněte na **New** (Nový), vyberte **Apache Tomcat v7.0** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-238">On the **Targeted Runtimes** screen, click **New**, select **Apache Tomcat v7.0**, and then click **OK**.</span></span>
16. <span data-ttu-id="ca62c-239">Pomocí kroku 12 otevřete znovu okno **Properties** a v levém podokně klikněte na **Project Facets** (Omezující vlastnosti projektu).</span><span class="sxs-lookup"><span data-stu-id="ca62c-239">Use step 12 to open the **Properties** window again, and then in the left pane click **Project Facets**.</span></span>
17. <span data-ttu-id="ca62c-240">Na obrazovce **Project Facets** vyberte **Dynamic Web Module** (Dynamický webový modul) a **Java** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-240">On the **Project Facets** screen, select **Dynamic Web Module** and **Java**, and then click **OK**.</span></span>
18. <span data-ttu-id="ca62c-241">Na kartě **Servers** (Servery) v dolní části obrazovky klikněte pravým tlačítkem na **Tomcat v7.0 Server at localhost** a pak levým na **Add and Remove** (Přidat a odstranit).</span><span class="sxs-lookup"><span data-stu-id="ca62c-241">On the **Servers** tab at the bottom of the screen, right-click **Tomcat v7.0 Server at localhost** and then click **Add and Remove**.</span></span>
19. <span data-ttu-id="ca62c-242">V okně **Add and Remove** přesuňte **azure-documentdb-java-sample** do pole **Configured** (Nakonfigurováno) a klikněte na **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="ca62c-242">On the **Add and Remove** window, move **azure-documentdb-java-sample** to the **Configured** box, and then click **Finish**.</span></span>
20. <span data-ttu-id="ca62c-243">V **servery** kartě, klikněte pravým tlačítkem na **Tomcat v7.0 Server at localhost**a potom klikněte na **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="ca62c-243">In the **Servers** tab, right-click **Tomcat v7.0 Server at localhost**, and then click **Restart**.</span></span>
21. <span data-ttu-id="ca62c-244">Přejděte v prohlížeči na http://localhost:8080/azure-documentdb-java-sample/ a začněte přidávat položky do seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="ca62c-244">In a browser, navigate to http://localhost:8080/azure-documentdb-java-sample/ and start adding to your task list.</span></span> <span data-ttu-id="ca62c-245">Poznámka: Pokud jste změnili výchozí hodnoty portů, změňte 8080 na hodnotu, kterou jste si vybrali.</span><span class="sxs-lookup"><span data-stu-id="ca62c-245">Note that if you changed your default port values, change 8080 to the value you selected.</span></span>
22. <span data-ttu-id="ca62c-246">Postup nasazení projektu na web Azure najdete v části [Krok 6. Nasazení aplikace na weby Azure](#Deploy).</span><span class="sxs-lookup"><span data-stu-id="ca62c-246">To deploy your project to an Azure web site, see [Step 6. Deploy your application to Azure Web Sites](#Deploy).</span></span>

[1]: media/documentdb-java-application/keys.png
