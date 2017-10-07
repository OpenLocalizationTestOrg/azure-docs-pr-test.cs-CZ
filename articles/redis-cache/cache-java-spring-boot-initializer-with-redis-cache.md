---
title: "aaaHow tooconfigure toouse aplikace pružiny spouštěcí inicializátoru Redis Cache"
description: "Zjistěte, jak tooconfigure pružiny spouštěcí aplikace vytvořena s hello pružiny Initializr toouse Azure Redis Cache."
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Mezipaměť Redis pružiny pružiny spouštěcí Starter,"
ms.assetid: 
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: java
ms.topic: article
ms.date: 7/21/2017
ms.author: robmcm;zhijzhao;yidon
ms.openlocfilehash: ad532c88d2d67b97079eeb0e0e392add29ac365b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-a-spring-boot-initializer-app-toouse-redis-cache"></a>Jak tooconfigure toouse aplikace pružiny spouštěcí inicializátoru Redis Cache

## <a name="overview"></a>Přehled

Hello  **[pružiny Framework]**  open-source řešení, která pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java. Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java. Vývojáři toohelp začít pracovat s pružiny spouštěcí, několik ukázkových pružiny spouštěcí balíčky jsou k dispozici na <https://github.com/spring-guides/>. Kromě toho toochoosing ze seznamu hello základní spouštěcí pružiny projekty, hello  **[pružiny Initializr]**  pomáhá vývojářům, jak začít s vytvářením vlastních pružiny spouštění aplikací.

Tento článek vás provede procesem vytvoření mezipaměti Redis pomocí hello portál Azure, pak pomocí hello **pružiny Initializr** toocreate vlastní aplikaci a pak vytvořit Java webové aplikace, které ukládají a načte data pomocí vašeho Redis cache.

## <a name="prerequisites"></a>Požadavky

v pořadí toofollow hello kroky v tomto článku jsou potřeba Hello následující požadavky:

* Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].

* A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), verze 1.7 nebo novější.

* [Apache Maven](http://maven.apache.org/), verze 3.0 nebo novější.

## <a name="create-a-redis-cache-on-azure"></a>Vytvoření mezipaměti Redis v Azure

1. Procházet toohello Azure portálu na <https://portal.azure.com/> a klikněte na položku hello pro **+ nový**.

   ![portál Azure][AZ01]

1. Klikněte na tlačítko **databáze**a potom klikněte na **Redis Cache**.

   ![portál Azure][AZ02]

1. Na hello **nová mezipaměť Redis** zadejte hello **název DNS** pro mezipaměť, pak zadejte vaše **předplatné**, **skupiny prostředků**,  **Umístění**, a **cenová úroveň**. Pokud jste zadali tyto možnosti, klikněte na tlačítko **vytvořit** toocreate svojí mezipaměti.

   ![portál Azure][AZ03]

1. Po dokončení mezipaměti uvidíte tu najdete na vaše Azure **řídicí panel**, i jako v části hello **všechny prostředky**, a **mezipaměti Redis cache** stránky. Kliknutím na vaší mezipaměti na žádném z těchto umístění tooopen hello vlastnosti stránky ke svojí mezipaměti.

   ![portál Azure][AZ04]

1. Když se zobrazí stránka hello, který obsahuje hello seznam vlastností pro mezipaměť, klikněte na tlačítko **přístupové klíče** a zkopírování přístupových klíčů pro mezipaměť.

   ![portál Azure][AZ05]

## <a name="create-a-custom-application-using-hello-spring-initializr"></a>Vytvořit vlastní aplikaci pomocí hello pružiny Initializr

1. Procházet příliš<https://start.spring.io/>.

1. Určete, zda má toogenerate **Maven** projektu s **Java**, zadejte hello **skupiny** a **Aritifact** názvy vaší aplikace. a pak klikněte na odkaz hello příliš**přepínač toohello plnou verzi** z hello pružiny Initializr.

   ![Možnosti Initializr pružiny Basic][SI01]

   > [!NOTE]
   >
   > Hello pružiny Initializr použije hello **skupiny** a **Aritifact** název balíčku hello toocreate názvy; například: *com.contoso.myazuredemo*.
   >

1. Projděte dolů toohello **webové** části a zaškrtněte políčko hello pro **webové**, posuňte se dolů toohello **NoSQL** části a zaškrtněte políčko hello pro **Redis**, posuňte se toohello dolní části stránky hello a klikněte na tlačítko hello příliš**generovat projektu**.

   ![Úplné pružiny Initializr možnosti][SI02]

1. Po zobrazení výzvy, stáhněte si hello projektu tooa cestu v místním počítači.

   ![Stáhněte si vlastní spouštěcí pružiny projekt][SI03]

1. Po extrahování souborů hello v místním systému bude vaše vlastní spouštěcí pružiny aplikace připravené pro úpravy.

   ![Soubory projektu pružiny vlastní spouštěcí][SI04]

## <a name="configure-your-custom-spring-boot-toouse-your-redis-cache"></a>Konfigurace vaší vlastní spouštěcí pružiny toouse vaše Redis Cache

1. Vyhledejte hello *application.properties* souboru v hello *prostředky* adresář vaší aplikace, nebo vytvořit soubor hello, pokud ještě neexistuje.

   ![Vyhledejte soubor application.properties hello][RE01]

1. Otevřete hello *application.properties* v textovém editoru soubor, přidejte následující řádky toohello soubor hello a hello ukázkové hodnoty nahraďte hello příslušné vlastnosti z mezipaměti:

   ```yaml
   # Specify hello DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify hello port for your Redis cache.
   spring.redis.port=6380

   # Specify hello access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Úpravy souboru application.properties hello][RE02]

1. Uložte a zavřete hello *application.properties* souboru.

1. Vytvořte složku s názvem *řadič* pod hello zdrojové složky balíčku; například:

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   -nebo-

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. Vytvořte nový soubor s názvem *HelloController.java* v hello *řadič* složky. Otevřete soubor hello v textovém editoru a přidejte následující kód tooit hello:

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Value;
   import redis.clients.jedis.Jedis;
   import redis.clients.jedis.JedisShardInfo;

   @RestController
   public class HelloController {
   
      // Retrieve hello DNS name for your cache.
      @Value("${spring.redis.host}")
      private String redisHost;

      // Retrieve hello port for your cache.
      @Value("${spring.redis.port}")
      private int redisPort;

      // Retrieve hello access key for your cache.
      @Value("${spring.redis.password}")
      private String redisPassword;

      @RequestMapping("/")
      // Define hello Hello World controller.
      public String hello() {
      
         // Create a JedisShardInfo object tooconnect tooyour Redis cache.
         JedisShardInfo jedisShardInfo = new JedisShardInfo(redisHost, redisPort, true);
         // Specify your access key.
         jedisShardInfo.setPassword(redisPassword);
         // Create a Jedis object toostore/retrieve information from your cache.
         Jedis jedis = new Jedis(jedisShardInfo);

         // Add a Hello World string tooyour cache.
         jedis.set("greeting", "Hello World!");

         // Return hello string from your cache.
         return jedis.get("greeting");
      }
   }
   ```
   
   Kde budete potřebovat tooreplace `com.contoso.myazuredemo` s hello název balíčku pro váš projekt.

1. Uložte a zavřete hello *HelloController.java* souboru.

1. Spouštěcí pružiny aplikace s Maven sestavíte a spustíte. například:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Test webové aplikace hello procházení toohttp://localhost:8080 pomocí webového prohlížeče, nebo použijte syntaxi hello jako následující příklad, pokud máte k dispozici curl hello:

   ```shell
   curl http://localhost:8080
   ```

   Měli byste vidět hello "Hello, World!" zpráva z ukázkové řadiče zobrazí, se načte se dynamicky z mezipaměti Redis.

## <a name="next-steps"></a>Další kroky

Další informace o používání pružiny spuštění aplikace v Azure najdete v části hello následující články:

* [Nasazení aplikace spouštěcí pružiny toohello Azure App Service](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Spuštění aplikace spouštěcí Spring v clusteru s podporou Kubernetes v hello Azure Container Service](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].

Další informace o získávání spuštění Redis Cache pomocí Javy v Azure, najdete v tématu [jak toouse Azure Redis mezipaměti s Javou][Redis Cache with Java].

<!-- URL List -->

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny Initializr]: https://start.spring.io/
[pružiny Framework]: https://spring.io/
[Redis Cache with Java]: cache-java-get-started.md

<!-- IMG List -->

[AZ01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ01.png
[AZ02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ02.png
[AZ03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ03.png
[AZ04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ04.png
[AZ05]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ05.png

[SI01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI01.png
[SI02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI02.png
[SI03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI03.png
[SI04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI04.png

[RE01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE01.png
[RE02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE02.png
