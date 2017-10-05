---
title: "Jak používat Starter spouštěcí pružiny rozhraním Azure Cosmos DB DocumentDB API"
description: "Informace o konfiguraci aplikace vytvořené pomocí inicializátoru spouštěcí pružiny s rozhraním API pro Azure Cosmos databáze DocumentDB."
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Spring pružiny spouštěcí Starter, Cosmos DB"
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/08/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: 273cc750857c5e466882060a38ac0f3475811e98
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="f38bc-104">Jak používat Starter spouštěcí pružiny s rozhraním API pro Azure Cosmos databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="f38bc-104">How to use the Spring Boot Starter with Azure Cosmos DB DocumentDB API</span></span>

## <a name="overview"></a><span data-ttu-id="f38bc-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="f38bc-105">Overview</span></span>

<span data-ttu-id="f38bc-106"> **[Pružiny Framework]**  open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="f38bc-106">The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="f38bc-107">Jedním z dalších oblíbených projektů, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="f38bc-107">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="f38bc-108">Chcete-li začít pracovat s pružiny spouštěcí vývojáři, jsou k dispozici na několik balíčků spouštěcí pružiny ukázkové <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="f38bc-108">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="f38bc-109">Kromě výběr ze seznamu základní pružiny spouštěcí projekty,  **[pružiny Initializr]**  pomáhá vývojářům, jak začít s vytvářením vlastních pružiny spouštění aplikací.</span><span class="sxs-lookup"><span data-stu-id="f38bc-109">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="f38bc-110">Azure Cosmos DB je globálně distribuované databáze služba, která umožňuje vývojářům pracovat s daty pomocí různých standardní rozhraní API, například DocumentDB, MongoDB, graf a tabulka rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f38bc-110">Azure Cosmos DB is a globally-distributed database service that allows developers to work with data using a variety of standard APIs, such as DocumentDB, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="f38bc-111">Společnosti Microsoft pružiny spouštěcí Starter umožňuje vývojářům používat spouštěcí pružiny aplikace, které se snadnou integraci s Azure Cosmos DB pomocí rozhraní API DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="f38bc-111">Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using DocumentDB APIs.</span></span>

<span data-ttu-id="f38bc-112">Tento článek ukazuje, vytváření Azure DB Cosmos pomocí portálu Azure a pak pomocí **Initializr pružiny** vytvořit aplikaci java vlastní, a pak přidat funkci pružiny spouštěcí Starter do vlastní aplikace ukládání dat v a načtení dat z vaší databáze Cosmos Azure pomocí rozhraní API DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="f38bc-112">This article demonstrates creating an Azure Cosmos DB using the Azure portal, then using the **Spring Initializr** to create a custom java application, and then add the Spring Boot Starter functionality to your custom application to store data in and retrieve data from your Azure Cosmos DB by using the DocumentDB API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f38bc-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f38bc-113">Prerequisites</span></span>

<span data-ttu-id="f38bc-114">Chcete-li postupujte podle kroků v tomto článku jsou požadovány následující součásti:</span><span class="sxs-lookup"><span data-stu-id="f38bc-114">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="f38bc-115">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="f38bc-115">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="f38bc-116">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f38bc-116">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="f38bc-117">[Apache Maven](http://maven.apache.org/), verze 3.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f38bc-117">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a><span data-ttu-id="f38bc-118">Vytvořit databázi Cosmos Azure pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f38bc-118">Create an Azure Cosmos DB by using the Azure portal</span></span>

1. <span data-ttu-id="f38bc-119">Přejděte na portál Azure na adrese <https://portal.azure.com/> a klikněte na tlačítko **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="f38bc-119">Browse to the Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![portál Azure][AZ01]

1. <span data-ttu-id="f38bc-121">Klikněte na tlačítko **databáze**a potom klikněte na **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="f38bc-121">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![portál Azure][AZ02]

1. <span data-ttu-id="f38bc-123">Na **Azure Cosmos DB** stránky, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f38bc-123">On the **Azure Cosmos DB** page, enter the following information:</span></span>

   * <span data-ttu-id="f38bc-124">Zadejte jedinečný **ID**, které budete používat jako identifikátor URI pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="f38bc-124">Enter a unique **ID**, which you will use as the URI for your database.</span></span> <span data-ttu-id="f38bc-125">Příklad: *wingtiptoysdata.documents.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="f38bc-125">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="f38bc-126">Zvolte **SQL (dokument databáze)** pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f38bc-126">Choose **SQL (Document DB)** for the API.</span></span>
   * <span data-ttu-id="f38bc-127">Vyberte **předplatné** chcete použít pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="f38bc-127">Choose the **Subscription** you want to use for your database.</span></span>
   * <span data-ttu-id="f38bc-128">Určete, zda chcete vytvořit nový **skupiny prostředků** pro vaši databázi nebo vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f38bc-128">Specify whether to create a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="f38bc-129">Zadejte **umístění** pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="f38bc-129">Specify the **Location** for your database.</span></span>
   
   <span data-ttu-id="f38bc-130">Pokud jste zadali tyto možnosti, klikněte na tlačítko **vytvořit** k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="f38bc-130">When you have specified these options, click **Create** to create your database.</span></span>

   ![portál Azure][AZ03]

1. <span data-ttu-id="f38bc-132">Po vytvoření databáze, je uvedena ve vaší službě Azure **řídicí panel**, stejně jako v části **všechny prostředky** a **Azure Cosmos DB** stránky.</span><span class="sxs-lookup"><span data-stu-id="f38bc-132">When your database has been created, it is listed on your Azure **Dashboard**, as well as under the **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="f38bc-133">Kliknutím na vaši databázi na žádném z těchto umístění, které chcete otevřít stránku vlastnosti ke svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f38bc-133">You can click on your database on any of those locations to open the properties page for your cache.</span></span>

   ![portál Azure][AZ04]

1. <span data-ttu-id="f38bc-135">Když stránku vlastností pro vaši databázi se zobrazí, klikněte na tlačítko **přístupové klíče** a kopírování klíče URI a přístup pro vaši databázi, budete používat tyto hodnoty v aplikaci pružiny spouštěcí.</span><span class="sxs-lookup"><span data-stu-id="f38bc-135">When the properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![portál Azure][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a><span data-ttu-id="f38bc-137">Vytvořit jednoduchou aplikaci pružiny spouštěcí s Initializr pružiny</span><span class="sxs-lookup"><span data-stu-id="f38bc-137">Create a simple Spring Boot application with the Spring Initializr</span></span>

1. <span data-ttu-id="f38bc-138">Přejděte do <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="f38bc-138">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="f38bc-139">Zadejte, že chcete vygenerovat **Maven** projektu s **Java**, zadejte **skupiny** a **artefaktů** názvy pro vaši aplikaci a Klikněte na tlačítko **generovat projektu**.</span><span class="sxs-lookup"><span data-stu-id="f38bc-139">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Artifact** names for your application, and then click the button to **Generate Project**.</span></span>

   ![Možnosti Initializr pružiny Basic][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="f38bc-141">Používá Initializr pružiny **skupiny** a **artefaktů** názvy vytvořit název balíčku; například: *com.example.wintiptoys*.</span><span class="sxs-lookup"><span data-stu-id="f38bc-141">The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wintiptoys*.</span></span>
   >

1. <span data-ttu-id="f38bc-142">Po zobrazení výzvy, stáhněte si projekt na cestu v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="f38bc-142">When prompted, download the project to a path on your local computer.</span></span>

   ![Stáhněte si vlastní spouštěcí pružiny projekt][SI02]

1. <span data-ttu-id="f38bc-144">Po extrahování souborů v místním systému bude jednoduché pružiny spouštěcí aplikace připravené pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="f38bc-144">After you have extracted the files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![Soubory projektu pružiny vlastní spouštěcí][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a><span data-ttu-id="f38bc-146">Konfigurace aplikace pružiny spouštěcí používat Starter spouštěcí pružiny Azure</span><span class="sxs-lookup"><span data-stu-id="f38bc-146">Configure your Spring Boot app to use the Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="f38bc-147">Vyhledejte *pom.xml* soubor v adresáři vaší aplikace; například:</span><span class="sxs-lookup"><span data-stu-id="f38bc-147">Locate the *pom.xml* file in the directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\pom.xml`

   <span data-ttu-id="f38bc-148">-nebo-</span><span class="sxs-lookup"><span data-stu-id="f38bc-148">-or-</span></span>

   `/users/example/home/wingtiptoys/pom.xml`

   ![Vyhledejte soubor pom.xml][PM01]

1. <span data-ttu-id="f38bc-150">Otevřete *pom.xml* soubor v textovém editoru a přidejte následující řádky do seznamu `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="f38bc-150">Open the *pom.xml* file in a text editor, and add the following lines to list of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Úpravy soubor pom.xml][PM02]

1. <span data-ttu-id="f38bc-152">Uložte a zavřete *pom.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="f38bc-152">Save and close the *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a><span data-ttu-id="f38bc-153">Konfigurace aplikace pružiny spouštěcí používat vaše Azure DB Cosmos</span><span class="sxs-lookup"><span data-stu-id="f38bc-153">Configure your Spring Boot app to use your Azure Cosmos DB</span></span>

1. <span data-ttu-id="f38bc-154">Vyhledejte *application.properties* v soubor *prostředky* adresář vaší aplikace; například:</span><span class="sxs-lookup"><span data-stu-id="f38bc-154">Locate the *application.properties* file in the *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   <span data-ttu-id="f38bc-155">-nebo-</span><span class="sxs-lookup"><span data-stu-id="f38bc-155">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Vyhledejte soubor application.properties][RE01]

1. <span data-ttu-id="f38bc-157">Otevřete *application.properties* soubor v textovém editoru a přidejte následující řádky do souboru a nahraďte příslušné vlastnosti pro vaši databázi ukázkové hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f38bc-157">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:</span></span>

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Úpravy souboru application.properties][RE02]

1. <span data-ttu-id="f38bc-159">Uložte a zavřete *application.properties* souboru.</span><span class="sxs-lookup"><span data-stu-id="f38bc-159">Save and close the *application.properties* file.</span></span>

## <a name="add-sample-code-to-implement-basic-database-functionality"></a><span data-ttu-id="f38bc-160">Přidejte ukázkový kód pro implementaci funkce základní databáze</span><span class="sxs-lookup"><span data-stu-id="f38bc-160">Add sample code to implement basic database functionality</span></span>

<span data-ttu-id="f38bc-161">V této části vytvoříte dvě třídy Java pro ukládání uživatelských dat a potom upravte třídě hlavní aplikace k vytvoření instance třídy uživatelů a uložte je do vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="f38bc-161">In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the user class and save it to your database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="f38bc-162">Zadejte základní třídu pro uložení uživatelských dat.</span><span class="sxs-lookup"><span data-stu-id="f38bc-162">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="f38bc-163">Vytvořte nový soubor s názvem *User.java* ve stejném adresáři jako souboru hlavní aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="f38bc-163">Create a new file named *User.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="f38bc-164">Otevřete *User.java* soubor v textovém editoru a přidejte následující řádky do souboru definujte obecné uživatelské třídu, která ukládá a k získávání hodnot v databázi:</span><span class="sxs-lookup"><span data-stu-id="f38bc-164">Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:</span></span>

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. <span data-ttu-id="f38bc-165">Uložte a zavřete *User.java* souboru.</span><span class="sxs-lookup"><span data-stu-id="f38bc-165">Save and close the *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="f38bc-166">Definujte rozhraní úložiště dat</span><span class="sxs-lookup"><span data-stu-id="f38bc-166">Define a data repository interface</span></span>

1. <span data-ttu-id="f38bc-167">Vytvořte nový soubor s názvem *UserRepository.java* ve stejném adresáři jako souboru hlavní aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="f38bc-167">Create a new file named *UserRepository.java* in the same directory as your main application Java file.</span></span>

1. <span data-ttu-id="f38bc-168">Otevřete *UserRepository.java* soubor v textovém editoru a přidejte následující řádky do souboru Definujte uživatelské rozhraní úložiště, který rozšiřuje výchozí rozhraní úložiště DocumentDB:</span><span class="sxs-lookup"><span data-stu-id="f38bc-168">Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. <span data-ttu-id="f38bc-169">Uložte a zavřete *UserRepository.java* souboru.</span><span class="sxs-lookup"><span data-stu-id="f38bc-169">Save and close the *UserRepository.java* file.</span></span>

### <a name="modify-the-main-application-class"></a><span data-ttu-id="f38bc-170">Upravit třída hlavní aplikace</span><span class="sxs-lookup"><span data-stu-id="f38bc-170">Modify the main application class</span></span>

1. <span data-ttu-id="f38bc-171">Vyhledejte soubor Java hlavní aplikace v adresáři balíčku aplikace; například:</span><span class="sxs-lookup"><span data-stu-id="f38bc-171">Locate the main application Java file in the package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   <span data-ttu-id="f38bc-172">-nebo-</span><span class="sxs-lookup"><span data-stu-id="f38bc-172">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Vyhledejte soubor aplikace Java][JV01]

1. <span data-ttu-id="f38bc-174">V textovém editoru otevřete soubor hlavní aplikace Java a přidejte následující řádky do souboru:</span><span class="sxs-lookup"><span data-stu-id="f38bc-174">Open the main application Java file in a text editor, and add the following lines to the file:</span></span>

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. <span data-ttu-id="f38bc-175">Uložte a zavřete soubor hlavní aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="f38bc-175">Save and close the main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="f38bc-176">Vytvoření a testování vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f38bc-176">Build and test your app</span></span>

1. <span data-ttu-id="f38bc-177">Otevřete příkazový řádek a změnit adresář, do složky, kde vaše *pom.xml* se nachází soubor; například:</span><span class="sxs-lookup"><span data-stu-id="f38bc-177">Open a command prompt and change directory to the folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoys`

   <span data-ttu-id="f38bc-178">-nebo-</span><span class="sxs-lookup"><span data-stu-id="f38bc-178">-or-</span></span>

   `cd /users/example/home/wingtiptoys`

1. <span data-ttu-id="f38bc-179">Spouštěcí pružiny aplikace s Maven sestavíte a spustíte. například:</span><span class="sxs-lookup"><span data-stu-id="f38bc-179">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. <span data-ttu-id="f38bc-180">Aplikace se zobrazí několik zpráv runtime a měli byste vidět zprávu `User: testFirstName testLastName` které oznamuje, že hodnoty byly úspěšně uložené a načíst z databáze.</span><span class="sxs-lookup"><span data-stu-id="f38bc-180">Your application will display several runtime messages, and you should see the message `User: testFirstName testLastName` displayed to indicate that values have been successfully stored and retrieved from your database.</span></span>

   ![Úspěšný výstup z aplikace][JV02]

1. <span data-ttu-id="f38bc-182">Volitelné: Můžete portál Azure pro vaši databázi kliknutím zobrazit obsah vaší Azure DB Cosmos na stránce vlastnosti **Průzkumníka dokumentů**a potom vyberete a položku ze seznamu zobrazených zobrazit obsah.</span><span class="sxs-lookup"><span data-stu-id="f38bc-182">OPTIONAL: You can use the Azure portal to view the contents of your Azure Cosmos DB from the properties page for your database by clicking  **Document Explorer**, and then selecting and item from the displayed list to view the contents.</span></span>

   ![Chcete-li zobrazit data pomocí Průzkumníka dokumentů][JV03]

## <a name="next-steps"></a><span data-ttu-id="f38bc-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f38bc-184">Next steps</span></span>

<span data-ttu-id="f38bc-185">Další informace o používání Azure Cosmos DB a Java najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="f38bc-185">For more information about using Azure Cosmos DB and Java, see the following articles:</span></span>

* <span data-ttu-id="f38bc-186">[Dokumentace Azure Cosmos DB].</span><span class="sxs-lookup"><span data-stu-id="f38bc-186">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="f38bc-187">[Azure Cosmos DB: Vytvoření aplikace DocumentDB API Java a portálu Azure][Build a DocumentDB API app with Java]</span><span class="sxs-lookup"><span data-stu-id="f38bc-187">[Azure Cosmos DB: Build a DocumentDB API app with Java and the Azure portal][Build a DocumentDB API app with Java]</span></span>

<span data-ttu-id="f38bc-188">Další informace o používání pružiny spuštění aplikace v Azure najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="f38bc-188">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="f38bc-189">Spring spouštěcí DocumenDB Starter pro Azure.</span><span class="sxs-lookup"><span data-stu-id="f38bc-189">Spring Boot DocumenDB Starter for Azure</span></span>](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [<span data-ttu-id="f38bc-190">Nasazení aplikace spouštěcí pružiny do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f38bc-190">Deploy a Spring Boot Application to the Azure App Service</span></span>](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [<span data-ttu-id="f38bc-191">Spuštění aplikace spouštěcí Spring v clusteru s podporou Kubernetes v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="f38bc-191">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="f38bc-192">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).</span><span class="sxs-lookup"><span data-stu-id="f38bc-192">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="f38bc-193">[Dokumentace Azure Cosmos DB]: /azure/cosmos-db/</span><span class="sxs-lookup"><span data-stu-id="f38bc-193">[Azure Cosmos DB Documentation]: /azure/cosmos-db/</span></span>
<span data-ttu-id="f38bc-194">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="f38bc-194">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
<span data-ttu-id="f38bc-195">[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="f38bc-195">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="f38bc-196">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="f38bc-196">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>
<span data-ttu-id="f38bc-197">[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="f38bc-197">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="f38bc-198">[pružiny spouštěcí]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="f38bc-198">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="f38bc-199">[pružiny Initializr]: https://start.spring.io/</span><span class="sxs-lookup"><span data-stu-id="f38bc-199">[Spring Initializr]: https://start.spring.io/</span></span>
<span data-ttu-id="f38bc-200">[Pružiny Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="f38bc-200">[Spring Framework]: https://spring.io/</span></span>

<!-- IMG List -->

[AZ01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ01.png
[AZ02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ02.png
[AZ03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ03.png
[AZ04]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ04.png
[AZ05]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ05.png

[SI01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI01.png
[SI02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI02.png
[SI03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI03.png

[RE01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE01.png
[RE02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE02.png

[PM01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM01.png
[PM02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM02.png

[JV01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV01.png
[JV02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV02.png
[JV03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV03.png
