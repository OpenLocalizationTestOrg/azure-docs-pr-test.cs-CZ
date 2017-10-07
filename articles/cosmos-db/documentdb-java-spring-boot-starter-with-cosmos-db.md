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
# <a name="how-toouse-hello-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a>Jak toouse hello pružiny spouštěcí Starter s rozhraním API pro Azure Cosmos databáze DocumentDB

## <a name="overview"></a>Přehled

Hello  **[pružiny Framework]**  open-source řešení, které pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java. Vývojáři toohelp začít pracovat s pružiny spouštěcí, několik ukázkových pružiny spouštěcí balíčky jsou k dispozici na <https://github.com/spring-guides/>. Kromě toho toochoosing ze seznamu hello základní spouštěcí pružiny projekty, hello  **[pružiny Initializr]**  pomáhá vývojářům, jak začít s vytvářením vlastních pružiny spouštění aplikací.

Azure Cosmos DB je globálně distribuované databáze služba, která umožňuje vývojářům toowork s daty pomocí různých standardní rozhraní API, například DocumentDB, MongoDB, graf a tabulka rozhraní API. Společnosti Microsoft pružiny spouštěcí Starter umožňuje vývojáři toouse pružiny spouštěcí aplikace, které se snadnou integraci s Azure Cosmos DB pomocí rozhraní API DocumentDB.

Tento článek ukazuje, vytváření Azure DB Cosmos pomocí hello portál Azure a pak pomocí hello **pružiny Initializr** toocreate aplikace vlastní java a poté přidejte vlastní tooyour funkce pružiny spouštěcí Starter hello data aplikací toostore v a načtení dat z vaší databáze Cosmos Azure pomocí hello DocumentDB rozhraní API.

## <a name="prerequisites"></a>Požadavky

v pořadí toofollow hello kroky v tomto článku jsou potřeba Hello následující požadavky:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].

* A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), verze 1.7 nebo novější.

* [Apache Maven](http://maven.apache.org/), verze 3.0 nebo novější.

## <a name="create-an-azure-cosmos-db-by-using-hello-azure-portal"></a>Vytvoření Azure DB Cosmos pomocí hello portálu Azure

1. Procházet toohello Azure portálu na <https://portal.azure.com/> a klikněte na tlačítko **+ nový**.

   ![portál Azure][AZ01]

1. Klikněte na tlačítko **databáze**a potom klikněte na **Azure Cosmos DB**.

   ![portál Azure][AZ02]

1. Na hello **Azure Cosmos DB** zadejte hello následující informace:

   * Zadejte jedinečný **ID**, které budete používat jako hello identifikátor URI pro vaši databázi. Příklad: *wingtiptoysdata.documents.azure.com*.
   * Zvolte **SQL (DB dokumentu)** pro hello rozhraní API.
   * Zvolte hello **předplatné** chcete toouse pro vaši databázi.
   * Určit, zda toocreate a nové **skupiny prostředků** pro vaši databázi nebo vyberte existující skupinu prostředků.
   * Zadejte hello **umístění** pro vaši databázi.
   
   Pokud jste zadali tyto možnosti, klikněte na tlačítko **vytvořit** toocreate vaší databáze.

   ![portál Azure][AZ03]

1. Po vytvoření databáze, je uvedena ve vaší službě Azure **řídicí panel**, i jako v části hello **všechny prostředky** a **Azure Cosmos DB** stránky. Kliknutím na vaši databázi na žádném z těchto umístění tooopen hello vlastnosti stránky ke svojí mezipaměti.

   ![portál Azure][AZ04]

1. Až se zobrazí stránku hello vlastností pro vaši databázi, klikněte na tlačítko **přístupové klíče** a kopírování klíče URI a přístup pro vaši databázi, budete používat tyto hodnoty v aplikaci pružiny spouštěcí.

   ![portál Azure][AZ05]

## <a name="create-a-simple-spring-boot-application-with-hello-spring-initializr"></a>Vytvořit jednoduchou aplikaci pružiny spouštěcí s hello pružiny Initializr

1. Procházet příliš<https://start.spring.io/>.

1. Určete, zda má toogenerate **Maven** projektu s **Java**, zadejte hello **skupiny** a **artefaktů** názvy pro vaši aplikaci a Klepněte na tlačítko hello příliš**generovat projektu**.

   ![Možnosti Initializr pružiny Basic][SI01]

   > [!NOTE]
   >
   > Hello pružiny Initializr používá hello **skupiny** a **artefaktů** název balíčku hello toocreate názvy; například: *com.example.wintiptoys*.
   >

1. Po zobrazení výzvy, stáhněte si hello projektu tooa cestu v místním počítači.

   ![Stáhněte si vlastní spouštěcí pružiny projekt][SI02]

1. Po extrahování souborů hello v místním systému bude jednoduché pružiny spouštěcí aplikace připravené pro úpravy.

   ![Soubory projektu pružiny vlastní spouštěcí][SI03]

## <a name="configure-your-spring-boot-app-toouse-hello-azure-spring-boot-starter"></a>Konfigurace vaší pružiny spouštěcí aplikace toouse hello Azure Spring spouštěcí Starter

1. Vyhledejte hello *pom.xml* soubor v adresáři hello aplikace; například:

   `C:\SpringBoot\wingtiptoys\pom.xml`

   -nebo-

   `/users/example/home/wingtiptoys/pom.xml`

   ![Vyhledejte soubor pom.xml hello][PM01]

1. Otevřete hello *pom.xml* soubor v textovém editoru a přidejte následující řádky toolist z hello `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Úpravy soubor pom.xml hello][PM02]

1. Uložte a zavřete hello *pom.xml* souboru.

## <a name="configure-your-spring-boot-app-toouse-your-azure-cosmos-db"></a>Konfigurace vaší aplikace toouse pružiny spouštěcí vaší Azure DB Cosmos

1. Vyhledejte hello *application.properties* souboru v hello *prostředky* adresáře aplikace; například:

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   -nebo-

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Vyhledejte soubor application.properties hello][RE01]

1. Otevřete hello *application.properties* v textovém editoru soubor, přidejte následující řádky toohello soubor hello a nahraďte ukázkové hodnoty hello hello příslušné vlastnosti pro vaši databázi:

   ```yaml
   # Specify hello DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify hello access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify hello name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Úpravy souboru application.properties hello][RE02]

1. Uložte a zavřete hello *application.properties* souboru.

## <a name="add-sample-code-tooimplement-basic-database-functionality"></a>Přidání funkce základní databáze tooimplement ukázka kódu

V této části vytvoříte dvě třídy Java pro ukládání uživatelských dat a pak upravit vaší hlavní aplikace třída toocreate instance třídy hello uživatele a uložte ho tooyour databáze.

### <a name="define-a-basic-class-for-storing-user-data"></a>Zadejte základní třídu pro uložení uživatelských dat.

1. Vytvořte nový soubor s názvem *User.java* v hello stejného adresáře jako souboru hlavní aplikace Java.

1. Otevřete hello *User.java* soubor v textovém editoru a přidejte následující hello řádků toohello souboru toodefine generického uživatelského třídu, která ukládá a k získávání hodnot v databázi:

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

1. Uložte a zavřete hello *User.java* souboru.

### <a name="define-a-data-repository-interface"></a>Definujte rozhraní úložiště dat

1. Vytvořte nový soubor s názvem *UserRepository.java* v hello stejného adresáře jako souboru hlavní aplikace Java.

1. Otevřete hello *UserRepository.java* soubor v textovém editoru a přidejte následující hello řádků toohello souboru toodefine uživatelské rozhraní úložiště, které rozšiřuje rozhraní úložiště DocumentDB výchozí hello:

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. Uložte a zavřete hello *UserRepository.java* souboru.

### <a name="modify-hello-main-application-class"></a>Upravit třídy hlavní aplikace hello

1. Vyhledejte soubor Java hello hlavní aplikace v adresáři balíčku hello aplikace; například:

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   -nebo-

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Vyhledejte soubor hello aplikace Java][JV01]

1. Otevřete soubor Java hello hlavní aplikace v textovém editoru a přidejte následující řádky toohello soubor hello:

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

1. Uložte a zavřete soubor hello hlavní aplikace Java.

## <a name="build-and-test-your-app"></a>Vytvoření a testování vaší aplikace

1. Otevřete příkazový řádek a změňte složku toohello kde vaše *pom.xml* se nachází soubor; například:

   `cd C:\SpringBoot\wingtiptoys`

   -nebo-

   `cd /users/example/home/wingtiptoys`

1. Spouštěcí pružiny aplikace s Maven sestavíte a spustíte. například:

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. Aplikace se zobrazí několik zpráv runtime a měli byste vidět zprávu hello `User: testFirstName testLastName` zobrazí tooindicate, hodnoty byly úspěšně uloženy a načíst z databáze.

   ![Úspěšný výstup z aplikace hello][JV02]

1. Volitelné: Můžete použít hello Azure portálu tooview hello obsah vaší databázi Cosmos Azure ze stránky hello vlastnosti pro vaši databázi kliknutím **Průzkumníka dokumentů**a pak vyberete a položku z hello zobrazí seznam tooview hello obsah.

   ![Pomocí Průzkumníka dokumentů tooview hello vaše data][JV03]

## <a name="next-steps"></a>Další kroky

Další informace o používání Azure Cosmos DB a Java najdete v tématu hello následující články:

* [Dokumentace Azure Cosmos DB].

* [Azure Cosmos DB: Sestavení aplikace DocumentDB API v jazyce Java a hello portálu Azure][Build a DocumentDB API app with Java]

Další informace o používání pružiny spuštění aplikace v Azure najdete v části hello následující články:

* [Spring spouštěcí DocumenDB Starter pro Azure.](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [Nasazení aplikace spouštěcí pružiny toohello Azure App Service](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Spuštění aplikace spouštěcí Spring v clusteru s podporou Kubernetes v hello Azure Container Service](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].

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
