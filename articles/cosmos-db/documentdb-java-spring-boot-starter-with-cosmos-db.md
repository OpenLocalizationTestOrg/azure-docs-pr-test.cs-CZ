---
title: "aaaHow toouse hello pružiny spouštěcí Starter rozhraním Azure Cosmos DB DocumentDB API"
description: "Zjistěte, jak tooconfigure aplikace vytvořena s hello pružiny spouštěcí inicializátoru s hello rozhraní API služby Azure Cosmos databáze DocumentDB."
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
ms.openlocfilehash: a2c6de678f850676cb2887e224e5c12950db0e53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="b5eb2-104">Jak toouse hello pružiny spouštěcí Starter s rozhraním API pro Azure Cosmos databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="b5eb2-104">How toouse hello Spring Boot Starter with Azure Cosmos DB DocumentDB API</span></span>

## <a name="overview"></a><span data-ttu-id="b5eb2-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="b5eb2-105">Overview</span></span>

<span data-ttu-id="b5eb2-106">Hello  **[pružiny Framework]**  open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-106">hello **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="b5eb2-107">Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-107">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="b5eb2-108">Vývojáři toohelp začít pracovat s pružiny spouštěcí, několik ukázkových pružiny spouštěcí balíčky jsou k dispozici na <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-108">toohelp developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="b5eb2-109">Kromě toho toochoosing ze seznamu hello základní spouštěcí pružiny projekty, hello  **[pružiny Initializr]**  pomáhá vývojářům, jak začít s vytvářením vlastních pružiny spouštění aplikací.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-109">In addition toochoosing from hello list of basic Spring Boot projects, hello **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="b5eb2-110">Azure Cosmos DB je globálně distribuované databáze služba, která umožňuje vývojářům toowork s daty pomocí různých standardní rozhraní API, například DocumentDB, MongoDB, graf a tabulka rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-110">Azure Cosmos DB is a globally-distributed database service that allows developers toowork with data using a variety of standard APIs, such as DocumentDB, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="b5eb2-111">Společnosti Microsoft pružiny spouštěcí Starter umožňuje vývojáři toouse pružiny spouštěcí aplikace, které se snadnou integraci s Azure Cosmos DB pomocí rozhraní API DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-111">Microsoft's Spring Boot Starter enables developers toouse Spring Boot applications that easily integrate with Azure Cosmos DB by using DocumentDB APIs.</span></span>

<span data-ttu-id="b5eb2-112">Tento článek ukazuje, vytváření Azure DB Cosmos pomocí hello portál Azure a pak pomocí hello **pružiny Initializr** toocreate aplikace vlastní java a poté přidejte vlastní tooyour funkce pružiny spouštěcí Starter hello data aplikací toostore v a načtení dat z vaší databáze Cosmos Azure pomocí hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-112">This article demonstrates creating an Azure Cosmos DB using hello Azure portal, then using hello **Spring Initializr** toocreate a custom java application, and then add hello Spring Boot Starter functionality tooyour custom application toostore data in and retrieve data from your Azure Cosmos DB by using hello DocumentDB API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5eb2-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5eb2-113">Prerequisites</span></span>

<span data-ttu-id="b5eb2-114">v pořadí toofollow hello kroky v tomto článku jsou potřeba Hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-114">hello following prerequisites are required in order toofollow hello steps in this article:</span></span>

* <span data-ttu-id="b5eb2-115">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="b5eb2-115">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="b5eb2-116">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-116">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="b5eb2-117">[Apache Maven](http://maven.apache.org/), verze 3.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-117">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-hello-azure-portal"></a><span data-ttu-id="b5eb2-118">Vytvoření Azure DB Cosmos pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b5eb2-118">Create an Azure Cosmos DB by using hello Azure portal</span></span>

1. <span data-ttu-id="b5eb2-119">Procházet toohello Azure portálu na <https://portal.azure.com/> a klikněte na tlačítko **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-119">Browse toohello Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![portál Azure][AZ01]

1. <span data-ttu-id="b5eb2-121">Klikněte na tlačítko **databáze**a potom klikněte na **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-121">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![portál Azure][AZ02]

1. <span data-ttu-id="b5eb2-123">Na hello **Azure Cosmos DB** zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-123">On hello **Azure Cosmos DB** page, enter hello following information:</span></span>

   * <span data-ttu-id="b5eb2-124">Zadejte jedinečný **ID**, které budete používat jako hello identifikátor URI pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-124">Enter a unique **ID**, which you will use as hello URI for your database.</span></span> <span data-ttu-id="b5eb2-125">Příklad: *wingtiptoysdata.documents.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-125">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="b5eb2-126">Zvolte **SQL (DB dokumentu)** pro hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-126">Choose **SQL (Document DB)** for hello API.</span></span>
   * <span data-ttu-id="b5eb2-127">Zvolte hello **předplatné** chcete toouse pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-127">Choose hello **Subscription** you want toouse for your database.</span></span>
   * <span data-ttu-id="b5eb2-128">Určit, zda toocreate a nové **skupiny prostředků** pro vaši databázi nebo vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-128">Specify whether toocreate a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="b5eb2-129">Zadejte hello **umístění** pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-129">Specify hello **Location** for your database.</span></span>
   
   <span data-ttu-id="b5eb2-130">Pokud jste zadali tyto možnosti, klikněte na tlačítko **vytvořit** toocreate vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-130">When you have specified these options, click **Create** toocreate your database.</span></span>

   ![portál Azure][AZ03]

1. <span data-ttu-id="b5eb2-132">Po vytvoření databáze, je uvedena ve vaší službě Azure **řídicí panel**, i jako v části hello **všechny prostředky** a **Azure Cosmos DB** stránky.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-132">When your database has been created, it is listed on your Azure **Dashboard**, as well as under hello **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="b5eb2-133">Kliknutím na vaši databázi na žádném z těchto umístění tooopen hello vlastnosti stránky ke svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-133">You can click on your database on any of those locations tooopen hello properties page for your cache.</span></span>

   ![portál Azure][AZ04]

1. <span data-ttu-id="b5eb2-135">Až se zobrazí stránku hello vlastností pro vaši databázi, klikněte na tlačítko **přístupové klíče** a kopírování klíče URI a přístup pro vaši databázi, budete používat tyto hodnoty v aplikaci pružiny spouštěcí.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-135">When hello properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![portál Azure][AZ05]

## <a name="create-a-simple-spring-boot-application-with-hello-spring-initializr"></a><span data-ttu-id="b5eb2-137">Vytvořit jednoduchou aplikaci pružiny spouštěcí s hello pružiny Initializr</span><span class="sxs-lookup"><span data-stu-id="b5eb2-137">Create a simple Spring Boot application with hello Spring Initializr</span></span>

1. <span data-ttu-id="b5eb2-138">Procházet příliš<https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-138">Browse too<https://start.spring.io/>.</span></span>

1. <span data-ttu-id="b5eb2-139">Určete, zda má toogenerate **Maven** projektu s **Java**, zadejte hello **skupiny** a **artefaktů** názvy pro vaši aplikaci a Klepněte na tlačítko hello příliš**generovat projektu**.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-139">Specify that you want toogenerate a **Maven** project with **Java**, enter hello **Group** and **Artifact** names for your application, and then click hello button too**Generate Project**.</span></span>

   ![Možnosti Initializr pružiny Basic][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="b5eb2-141">Hello pružiny Initializr používá hello **skupiny** a **artefaktů** název balíčku hello toocreate názvy; například: *com.example.wintiptoys*.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-141">hello Spring Initializr uses hello **Group** and **Artifact** names toocreate hello package name; for example: *com.example.wintiptoys*.</span></span>
   >

1. <span data-ttu-id="b5eb2-142">Po zobrazení výzvy, stáhněte si hello projektu tooa cestu v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-142">When prompted, download hello project tooa path on your local computer.</span></span>

   ![Stáhněte si vlastní spouštěcí pružiny projekt][SI02]

1. <span data-ttu-id="b5eb2-144">Po extrahování souborů hello v místním systému bude jednoduché pružiny spouštěcí aplikace připravené pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-144">After you have extracted hello files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![Soubory projektu pružiny vlastní spouštěcí][SI03]

## <a name="configure-your-spring-boot-app-toouse-hello-azure-spring-boot-starter"></a><span data-ttu-id="b5eb2-146">Konfigurace vaší pružiny spouštěcí aplikace toouse hello Azure Spring spouštěcí Starter</span><span class="sxs-lookup"><span data-stu-id="b5eb2-146">Configure your Spring Boot app toouse hello Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="b5eb2-147">Vyhledejte hello *pom.xml* soubor v adresáři hello aplikace; například:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-147">Locate hello *pom.xml* file in hello directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\pom.xml`

   <span data-ttu-id="b5eb2-148">-nebo-</span><span class="sxs-lookup"><span data-stu-id="b5eb2-148">-or-</span></span>

   `/users/example/home/wingtiptoys/pom.xml`

   ![Vyhledejte soubor pom.xml hello][PM01]

1. <span data-ttu-id="b5eb2-150">Otevřete hello *pom.xml* soubor v textovém editoru a přidejte následující řádky toolist z hello `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-150">Open hello *pom.xml* file in a text editor, and add hello following lines toolist of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Úpravy soubor pom.xml hello][PM02]

1. <span data-ttu-id="b5eb2-152">Uložte a zavřete hello *pom.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-152">Save and close hello *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-toouse-your-azure-cosmos-db"></a><span data-ttu-id="b5eb2-153">Konfigurace vaší aplikace toouse pružiny spouštěcí vaší Azure DB Cosmos</span><span class="sxs-lookup"><span data-stu-id="b5eb2-153">Configure your Spring Boot app toouse your Azure Cosmos DB</span></span>

1. <span data-ttu-id="b5eb2-154">Vyhledejte hello *application.properties* souboru v hello *prostředky* adresáře aplikace; například:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-154">Locate hello *application.properties* file in hello *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   <span data-ttu-id="b5eb2-155">-nebo-</span><span class="sxs-lookup"><span data-stu-id="b5eb2-155">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Vyhledejte soubor application.properties hello][RE01]

1. <span data-ttu-id="b5eb2-157">Otevřete hello *application.properties* v textovém editoru soubor, přidejte následující řádky toohello soubor hello a nahraďte ukázkové hodnoty hello hello příslušné vlastnosti pro vaši databázi:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-157">Open hello *application.properties* file in a text editor, and add hello following lines toohello file, and replace hello sample values with hello appropriate properties for your database:</span></span>

   ```yaml
   # Specify hello DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify hello access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify hello name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Úpravy souboru application.properties hello][RE02]

1. <span data-ttu-id="b5eb2-159">Uložte a zavřete hello *application.properties* souboru.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-159">Save and close hello *application.properties* file.</span></span>

## <a name="add-sample-code-tooimplement-basic-database-functionality"></a><span data-ttu-id="b5eb2-160">Přidání funkce základní databáze tooimplement ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="b5eb2-160">Add sample code tooimplement basic database functionality</span></span>

<span data-ttu-id="b5eb2-161">V této části vytvoříte dvě třídy Java pro ukládání uživatelských dat a pak upravit vaší hlavní aplikace třída toocreate instance třídy hello uživatele a uložte ho tooyour databáze.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-161">In this section you create two Java classes for storing user data, and then you modify your main application class toocreate an instance of hello user class and save it tooyour database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="b5eb2-162">Zadejte základní třídu pro uložení uživatelských dat.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-162">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="b5eb2-163">Vytvořte nový soubor s názvem *User.java* v hello stejného adresáře jako souboru hlavní aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-163">Create a new file named *User.java* in hello same directory as your main application Java file.</span></span>

1. <span data-ttu-id="b5eb2-164">Otevřete hello *User.java* soubor v textovém editoru a přidejte následující hello řádků toohello souboru toodefine generického uživatelského třídu, která ukládá a k získávání hodnot v databázi:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-164">Open hello *User.java* file in a text editor, and add hello following lines toohello file toodefine a generic user class that stores and retrieve values in your database:</span></span>

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

1. <span data-ttu-id="b5eb2-165">Uložte a zavřete hello *User.java* souboru.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-165">Save and close hello *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="b5eb2-166">Definujte rozhraní úložiště dat</span><span class="sxs-lookup"><span data-stu-id="b5eb2-166">Define a data repository interface</span></span>

1. <span data-ttu-id="b5eb2-167">Vytvořte nový soubor s názvem *UserRepository.java* v hello stejného adresáře jako souboru hlavní aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-167">Create a new file named *UserRepository.java* in hello same directory as your main application Java file.</span></span>

1. <span data-ttu-id="b5eb2-168">Otevřete hello *UserRepository.java* soubor v textovém editoru a přidejte následující hello řádků toohello souboru toodefine uživatelské rozhraní úložiště, které rozšiřuje rozhraní úložiště DocumentDB výchozí hello:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-168">Open hello *UserRepository.java* file in a text editor, and add hello following lines toohello file toodefine a user repository interface that extends hello default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. <span data-ttu-id="b5eb2-169">Uložte a zavřete hello *UserRepository.java* souboru.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-169">Save and close hello *UserRepository.java* file.</span></span>

### <a name="modify-hello-main-application-class"></a><span data-ttu-id="b5eb2-170">Upravit třídy hlavní aplikace hello</span><span class="sxs-lookup"><span data-stu-id="b5eb2-170">Modify hello main application class</span></span>

1. <span data-ttu-id="b5eb2-171">Vyhledejte soubor Java hello hlavní aplikace v adresáři balíčku hello aplikace; například:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-171">Locate hello main application Java file in hello package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   <span data-ttu-id="b5eb2-172">-nebo-</span><span class="sxs-lookup"><span data-stu-id="b5eb2-172">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Vyhledejte soubor hello aplikace Java][JV01]

1. <span data-ttu-id="b5eb2-174">Otevřete soubor Java hello hlavní aplikace v textovém editoru a přidejte následující řádky toohello soubor hello:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-174">Open hello main application Java file in a text editor, and add hello following lines toohello file:</span></span>

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

1. <span data-ttu-id="b5eb2-175">Uložte a zavřete soubor hello hlavní aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-175">Save and close hello main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="b5eb2-176">Vytvoření a testování vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="b5eb2-176">Build and test your app</span></span>

1. <span data-ttu-id="b5eb2-177">Otevřete příkazový řádek a změňte složku toohello kde vaše *pom.xml* se nachází soubor; například:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-177">Open a command prompt and change directory toohello folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoys`

   <span data-ttu-id="b5eb2-178">-nebo-</span><span class="sxs-lookup"><span data-stu-id="b5eb2-178">-or-</span></span>

   `cd /users/example/home/wingtiptoys`

1. <span data-ttu-id="b5eb2-179">Spouštěcí pružiny aplikace s Maven sestavíte a spustíte. například:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-179">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. <span data-ttu-id="b5eb2-180">Aplikace se zobrazí několik zpráv runtime a měli byste vidět zprávu hello `User: testFirstName testLastName` zobrazí tooindicate, hodnoty byly úspěšně uloženy a načíst z databáze.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-180">Your application will display several runtime messages, and you should see hello message `User: testFirstName testLastName` displayed tooindicate that values have been successfully stored and retrieved from your database.</span></span>

   ![Úspěšný výstup z aplikace hello][JV02]

1. <span data-ttu-id="b5eb2-182">Volitelné: Můžete použít hello Azure portálu tooview hello obsah vaší databázi Cosmos Azure ze stránky hello vlastnosti pro vaši databázi kliknutím **Průzkumníka dokumentů**a pak vyberete a položku z hello zobrazí seznam tooview hello obsah.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-182">OPTIONAL: You can use hello Azure portal tooview hello contents of your Azure Cosmos DB from hello properties page for your database by clicking  **Document Explorer**, and then selecting and item from hello displayed list tooview hello contents.</span></span>

   ![Pomocí Průzkumníka dokumentů tooview hello vaše data][JV03]

## <a name="next-steps"></a><span data-ttu-id="b5eb2-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5eb2-184">Next steps</span></span>

<span data-ttu-id="b5eb2-185">Další informace o používání Azure Cosmos DB a Java najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-185">For more information about using Azure Cosmos DB and Java, see hello following articles:</span></span>

* <span data-ttu-id="b5eb2-186">[Dokumentace Azure Cosmos DB].</span><span class="sxs-lookup"><span data-stu-id="b5eb2-186">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="b5eb2-187">[Azure Cosmos DB: Sestavení aplikace DocumentDB API v jazyce Java a hello portálu Azure][Build a DocumentDB API app with Java]</span><span class="sxs-lookup"><span data-stu-id="b5eb2-187">[Azure Cosmos DB: Build a DocumentDB API app with Java and hello Azure portal][Build a DocumentDB API app with Java]</span></span>

<span data-ttu-id="b5eb2-188">Další informace o používání pružiny spuštění aplikace v Azure najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="b5eb2-188">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="b5eb2-189">Spring spouštěcí DocumenDB Starter pro Azure.</span><span class="sxs-lookup"><span data-stu-id="b5eb2-189">Spring Boot DocumenDB Starter for Azure</span></span>](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [<span data-ttu-id="b5eb2-190">Nasazení aplikace spouštěcí pružiny toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b5eb2-190">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [<span data-ttu-id="b5eb2-191">Spuštění aplikace spouštěcí Spring v clusteru s podporou Kubernetes v hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="b5eb2-191">Running a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="b5eb2-192">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="b5eb2-192">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Dokumentace Azure Cosmos DB]: /azure/cosmos-db/
[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny Initializr]: https://start.spring.io/
[pružiny Framework]: https://spring.io/

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
