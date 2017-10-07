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
# <a name="how-tooconfigure-a-spring-boot-initializer-app-toouse-redis-cache"></a><span data-ttu-id="e7706-104">Jak tooconfigure toouse aplikace pružiny spouštěcí inicializátoru Redis Cache</span><span class="sxs-lookup"><span data-stu-id="e7706-104">How tooconfigure a Spring Boot Initializer app toouse Redis Cache</span></span>

## <a name="overview"></a><span data-ttu-id="e7706-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="e7706-105">Overview</span></span>

<span data-ttu-id="e7706-106">Hello  **[pružiny Framework]**  open-source řešení, která pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="e7706-106">hello **[Spring Framework]** is an open-source solution which helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="e7706-107">Jeden z hello oblíbených další projekty, které je vytvořené v horní části daného platformy je [pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="e7706-107">One of hello more-popular projects which is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="e7706-108">Vývojáři toohelp začít pracovat s pružiny spouštěcí, několik ukázkových pružiny spouštěcí balíčky jsou k dispozici na <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="e7706-108">toohelp developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="e7706-109">Kromě toho toochoosing ze seznamu hello základní spouštěcí pružiny projekty, hello  **[pružiny Initializr]**  pomáhá vývojářům, jak začít s vytvářením vlastních pružiny spouštění aplikací.</span><span class="sxs-lookup"><span data-stu-id="e7706-109">In addition toochoosing from hello list of basic Spring Boot projects, hello **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="e7706-110">Tento článek vás provede procesem vytvoření mezipaměti Redis pomocí hello portál Azure, pak pomocí hello **pružiny Initializr** toocreate vlastní aplikaci a pak vytvořit Java webové aplikace, které ukládají a načte data pomocí vašeho Redis cache.</span><span class="sxs-lookup"><span data-stu-id="e7706-110">This article walks you through creating a Redis cache using hello Azure portal, then using hello **Spring Initializr** toocreate a custom application, and then creating a Java web application which stores and retrieves data using your Redis cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7706-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e7706-111">Prerequisites</span></span>

<span data-ttu-id="e7706-112">v pořadí toofollow hello kroky v tomto článku jsou potřeba Hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="e7706-112">hello following prerequisites are required in order toofollow hello steps in this article:</span></span>

* <span data-ttu-id="e7706-113">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="e7706-113">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="e7706-114">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e7706-114">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="e7706-115">[Apache Maven](http://maven.apache.org/), verze 3.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e7706-115">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="e7706-116">Vytvoření mezipaměti Redis v Azure</span><span class="sxs-lookup"><span data-stu-id="e7706-116">Create a Redis cache on Azure</span></span>

1. <span data-ttu-id="e7706-117">Procházet toohello Azure portálu na <https://portal.azure.com/> a klikněte na položku hello pro **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="e7706-117">Browse toohello Azure portal at <https://portal.azure.com/> and click hello item for **+New**.</span></span>

   ![portál Azure][AZ01]

1. <span data-ttu-id="e7706-119">Klikněte na tlačítko **databáze**a potom klikněte na **Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="e7706-119">Click **Database**, and then click **Redis Cache**.</span></span>

   ![portál Azure][AZ02]

1. <span data-ttu-id="e7706-121">Na hello **nová mezipaměť Redis** zadejte hello **název DNS** pro mezipaměť, pak zadejte vaše **předplatné**, **skupiny prostředků**,  **Umístění**, a **cenová úroveň**.</span><span class="sxs-lookup"><span data-stu-id="e7706-121">On hello **New Redis Cache** page, enter hello **DNS name** for your cache, then specify your **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span> <span data-ttu-id="e7706-122">Pokud jste zadali tyto možnosti, klikněte na tlačítko **vytvořit** toocreate svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e7706-122">When you have specified these options, click **Create** toocreate your cache.</span></span>

   ![portál Azure][AZ03]

1. <span data-ttu-id="e7706-124">Po dokončení mezipaměti uvidíte tu najdete na vaše Azure **řídicí panel**, i jako v části hello **všechny prostředky**, a **mezipaměti Redis cache** stránky.</span><span class="sxs-lookup"><span data-stu-id="e7706-124">Once your cache has been completed, you will see it listed on your Azure **Dashboard**, as well as under hello **All Resources**, and **Redis Caches** pages.</span></span> <span data-ttu-id="e7706-125">Kliknutím na vaší mezipaměti na žádném z těchto umístění tooopen hello vlastnosti stránky ke svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e7706-125">You can click on your cache on any of those locations tooopen hello properties page for your cache.</span></span>

   ![portál Azure][AZ04]

1. <span data-ttu-id="e7706-127">Když se zobrazí stránka hello, který obsahuje hello seznam vlastností pro mezipaměť, klikněte na tlačítko **přístupové klíče** a zkopírování přístupových klíčů pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="e7706-127">When hello page which contains hello list of properties for your cache is displayed, click **Access keys** and copy your access keys for your cache.</span></span>

   ![portál Azure][AZ05]

## <a name="create-a-custom-application-using-hello-spring-initializr"></a><span data-ttu-id="e7706-129">Vytvořit vlastní aplikaci pomocí hello pružiny Initializr</span><span class="sxs-lookup"><span data-stu-id="e7706-129">Create a custom application using hello Spring Initializr</span></span>

1. <span data-ttu-id="e7706-130">Procházet příliš<https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="e7706-130">Browse too<https://start.spring.io/>.</span></span>

1. <span data-ttu-id="e7706-131">Určete, zda má toogenerate **Maven** projektu s **Java**, zadejte hello **skupiny** a **Aritifact** názvy vaší aplikace. a pak klikněte na odkaz hello příliš**přepínač toohello plnou verzi** z hello pružiny Initializr.</span><span class="sxs-lookup"><span data-stu-id="e7706-131">Specify that you want toogenerate a **Maven** project with **Java**, enter hello **Group** and **Aritifact** names for your application, and then click hello link too**Switch toohello full version** of hello Spring Initializr.</span></span>

   ![Možnosti Initializr pružiny Basic][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="e7706-133">Hello pružiny Initializr použije hello **skupiny** a **Aritifact** název balíčku hello toocreate názvy; například: *com.contoso.myazuredemo*.</span><span class="sxs-lookup"><span data-stu-id="e7706-133">hello Spring Initializr will use hello **Group** and **Aritifact** names toocreate hello package name; for example: *com.contoso.myazuredemo*.</span></span>
   >

1. <span data-ttu-id="e7706-134">Projděte dolů toohello **webové** části a zaškrtněte políčko hello pro **webové**, posuňte se dolů toohello **NoSQL** části a zaškrtněte políčko hello pro **Redis**, posuňte se toohello dolní části stránky hello a klikněte na tlačítko hello příliš**generovat projektu**.</span><span class="sxs-lookup"><span data-stu-id="e7706-134">Scroll down toohello **Web** section and check hello box for **Web**, then scroll down toohello **NoSQL** section and check hello box for **Redis**, then scroll toohello bottom of hello page and click hello button too**Generate Project**.</span></span>

   ![Úplné pružiny Initializr možnosti][SI02]

1. <span data-ttu-id="e7706-136">Po zobrazení výzvy, stáhněte si hello projektu tooa cestu v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="e7706-136">When prompted, download hello project tooa path on your local computer.</span></span>

   ![Stáhněte si vlastní spouštěcí pružiny projekt][SI03]

1. <span data-ttu-id="e7706-138">Po extrahování souborů hello v místním systému bude vaše vlastní spouštěcí pružiny aplikace připravené pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="e7706-138">After you have extracted hello files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![Soubory projektu pružiny vlastní spouštěcí][SI04]

## <a name="configure-your-custom-spring-boot-toouse-your-redis-cache"></a><span data-ttu-id="e7706-140">Konfigurace vaší vlastní spouštěcí pružiny toouse vaše Redis Cache</span><span class="sxs-lookup"><span data-stu-id="e7706-140">Configure your custom Spring Boot toouse your Redis Cache</span></span>

1. <span data-ttu-id="e7706-141">Vyhledejte hello *application.properties* souboru v hello *prostředky* adresář vaší aplikace, nebo vytvořit soubor hello, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e7706-141">Locate hello *application.properties* file in hello *resources* directory of your app, or create hello file if it does not already exist.</span></span>

   ![Vyhledejte soubor application.properties hello][RE01]

1. <span data-ttu-id="e7706-143">Otevřete hello *application.properties* v textovém editoru soubor, přidejte následující řádky toohello soubor hello a hello ukázkové hodnoty nahraďte hello příslušné vlastnosti z mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="e7706-143">Open hello *application.properties* file in a text editor, and add hello following lines toohello file, and replace hello sample values with hello appropriate properties from your cache:</span></span>

   ```yaml
   # Specify hello DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify hello port for your Redis cache.
   spring.redis.port=6380

   # Specify hello access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Úpravy souboru application.properties hello][RE02]

1. <span data-ttu-id="e7706-145">Uložte a zavřete hello *application.properties* souboru.</span><span class="sxs-lookup"><span data-stu-id="e7706-145">Save and close hello *application.properties* file.</span></span>

1. <span data-ttu-id="e7706-146">Vytvořte složku s názvem *řadič* pod hello zdrojové složky balíčku; například:</span><span class="sxs-lookup"><span data-stu-id="e7706-146">Create a folder named *controller* under hello source folder for your package; for example:</span></span>

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   <span data-ttu-id="e7706-147">-nebo-</span><span class="sxs-lookup"><span data-stu-id="e7706-147">-or-</span></span>

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. <span data-ttu-id="e7706-148">Vytvořte nový soubor s názvem *HelloController.java* v hello *řadič* složky.</span><span class="sxs-lookup"><span data-stu-id="e7706-148">Create a new file named *HelloController.java* in hello *controller* folder.</span></span> <span data-ttu-id="e7706-149">Otevřete soubor hello v textovém editoru a přidejte následující kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="e7706-149">Open hello file in a text editor and add hello following code tooit:</span></span>

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
   
   <span data-ttu-id="e7706-150">Kde budete potřebovat tooreplace `com.contoso.myazuredemo` s hello název balíčku pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="e7706-150">Where you will need tooreplace `com.contoso.myazuredemo` with hello package name for your project.</span></span>

1. <span data-ttu-id="e7706-151">Uložte a zavřete hello *HelloController.java* souboru.</span><span class="sxs-lookup"><span data-stu-id="e7706-151">Save and close hello *HelloController.java* file.</span></span>

1. <span data-ttu-id="e7706-152">Spouštěcí pružiny aplikace s Maven sestavíte a spustíte. například:</span><span class="sxs-lookup"><span data-stu-id="e7706-152">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="e7706-153">Test webové aplikace hello procházení toohttp://localhost:8080 pomocí webového prohlížeče, nebo použijte syntaxi hello jako následující příklad, pokud máte k dispozici curl hello:</span><span class="sxs-lookup"><span data-stu-id="e7706-153">Test hello web app by browsing toohttp://localhost:8080 using a web browser, or use hello syntax like hello following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="e7706-154">Měli byste vidět hello "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="e7706-154">You should see hello "Hello World!"</span></span> <span data-ttu-id="e7706-155">zpráva z ukázkové řadiče zobrazí, se načte se dynamicky z mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="e7706-155">message from your sample controller displayed, which is being retrieved dynamically from your Redis cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7706-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7706-156">Next steps</span></span>

<span data-ttu-id="e7706-157">Další informace o používání pružiny spuštění aplikace v Azure najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="e7706-157">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="e7706-158">Nasazení aplikace spouštěcí pružiny toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e7706-158">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [<span data-ttu-id="e7706-159">Spuštění aplikace spouštěcí Spring v clusteru s podporou Kubernetes v hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="e7706-159">Running a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="e7706-160">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="e7706-160">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="e7706-161">Další informace o získávání spuštění Redis Cache pomocí Javy v Azure, najdete v tématu [jak toouse Azure Redis mezipaměti s Javou][Redis Cache with Java].</span><span class="sxs-lookup"><span data-stu-id="e7706-161">For more information about getting started using Redis Cache with Java on Azure, see [How toouse Azure Redis Cache with Java][Redis Cache with Java].</span></span>

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
