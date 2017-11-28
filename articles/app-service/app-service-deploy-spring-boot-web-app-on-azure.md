---
title: "aaaDeploy toohello pružiny spouštění aplikací Azure App Service | Microsoft Docs"
description: "V tomto kurzu Průvodce vývojáře prostřednictvím hello kroky toodeploy hello pružiny spouštěcí Začínáme webové aplikace tooAzure služby App Service."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 69f9c4903fd740125194402cdb4b4db46a1f2773
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-toohello-azure-app-service"></a><span data-ttu-id="ec75a-103">Nasazení aplikace spouštěcí pružiny toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ec75a-103">Deploy a Spring Boot Application toohello Azure App Service</span></span>

<span data-ttu-id="ec75a-104">Hello  **[pružiny Framework]**  open-source řešení, která pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java a jeden z hello oblíbených další projekty, které je postavená na této platformě je [Pružiny spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="ec75a-104">hello **[Spring Framework]** is an open-source solution which helps Java developers create enterprise-level applications, and one of hello more-popular projects which is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="ec75a-105">Tento kurz vás provede, když vytvoření hello ukázkovou pružiny spouštěcí Začínáme webovou aplikaci a jeho nasazení příliš[Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="ec75a-105">This tutorial will walk you though creating hello sample Spring Boot Getting Started web app and deploying it too[Azure App Service].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ec75a-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ec75a-106">Prerequisites</span></span>

<span data-ttu-id="ec75a-107">Pořadí toocomplete hello kroky v tomto kurzu je třeba toohave hello následující:</span><span class="sxs-lookup"><span data-stu-id="ec75a-107">In order toocomplete hello steps in this tutorial, you need toohave hello following:</span></span>

* <span data-ttu-id="ec75a-108">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="ec75a-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="ec75a-109">Aktuální [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="ec75a-109">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="ec75a-110">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="ec75a-110">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="ec75a-111">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="ec75a-111">A [Git] client.</span></span>

## <a name="create-hello-spring-boot-getting-started-web-app"></a><span data-ttu-id="ec75a-112">Vytvoření hello pružiny spouštěcí Začínáme webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ec75a-112">Create hello Spring Boot Getting Started web app</span></span>

<span data-ttu-id="ec75a-113">Hello následující postup vás provede kroky hello, které jsou požadované toocreate jednoduchou webovou aplikaci pružiny spouštěcí a otestovat ji místně.</span><span class="sxs-lookup"><span data-stu-id="ec75a-113">hello following steps will walk you through hello steps that are required toocreate a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="ec75a-114">Otevřete příkazový řádek a vytvářet toohold místního adresáře aplikace a změnit adresář toothat; například:</span><span class="sxs-lookup"><span data-stu-id="ec75a-114">Open a command-prompt and create a local directory toohold your application, and change toothat directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="ec75a-115">--nebo--</span><span class="sxs-lookup"><span data-stu-id="ec75a-115">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="ec75a-116">Klon hello [pružiny spouštěcí Začínáme] ukázkový projekt do adresáře hello jste právě vytvořili; například:</span><span class="sxs-lookup"><span data-stu-id="ec75a-116">Clone hello [Spring Boot Getting Started] sample project into hello directory you just created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. <span data-ttu-id="ec75a-117">Změnit adresář toohello dokončit projekt; například:</span><span class="sxs-lookup"><span data-stu-id="ec75a-117">Change directory toohello completed project; for example:</span></span>
   ```
   cd gs-spring-boot
   cd complete
   ```

1. <span data-ttu-id="ec75a-118">Vytvořit soubor JAR hello pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="ec75a-118">Build hello JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="ec75a-119">Po vytvoření webové aplikace hello změnit soubor JAR toohello adresáře a spusťte hello webové aplikace; například:</span><span class="sxs-lookup"><span data-stu-id="ec75a-119">Once hello web app has been created, change directory toohello JAR file and start hello web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. <span data-ttu-id="ec75a-120">Test webové aplikace hello procházení toohttp://localhost:8080 pomocí webového prohlížeče, nebo použijte syntaxi hello jako následující příklad, pokud máte k dispozici curl hello:</span><span class="sxs-lookup"><span data-stu-id="ec75a-120">Test hello web app by browsing toohttp://localhost:8080 using a web browser, or use hello syntax like hello following example if you have curl available:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="ec75a-121">Měli byste vidět hello následující zpráva: **pozdrav z jara spouštěcí!**</span><span class="sxs-lookup"><span data-stu-id="ec75a-121">You should see hello following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Procházet ukázkové aplikace][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a><span data-ttu-id="ec75a-123">Vytvoření webové aplikace Azure pro použití s Javou</span><span class="sxs-lookup"><span data-stu-id="ec75a-123">Create an Azure web app for use with Java</span></span>

<span data-ttu-id="ec75a-124">Hello následující kroky vás provedou kroky toocreate hello webové aplikace Azure, nakonfigurujete hello požadované nastavení pro jazyk Java a konfigurovat přihlašovací údaje serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="ec75a-124">hello following steps will walk you through hello steps toocreate an Azure Web App, configure hello required settings for Java, and configure your FTP credentials.</span></span>

1. <span data-ttu-id="ec75a-125">Procházet toohello [portál Azure] a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="ec75a-125">Browse toohello [Azure portal] and log in.</span></span>

1. <span data-ttu-id="ec75a-126">Po přihlášení ke svému účtu na hello portálu Azure klikněte na ikonu hello nabídky pro **App Services**:</span><span class="sxs-lookup"><span data-stu-id="ec75a-126">Once you have logged into your account on hello Azure portal, click hello menu icon for **App Services**:</span></span>
   
   ![portál Azure][AZ01]

1. <span data-ttu-id="ec75a-128">Když hello **App Services** stránky se zobrazí, klikněte na tlačítko **+ přidat** toocreate nové služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ec75a-128">When hello **App Services** page is displayed, click **+ Add** toocreate a new App Service.</span></span>

   ![Vytvoření služby App Service][AZ02]

1. <span data-ttu-id="ec75a-130">Když se zobrazí seznam hello šablony webové aplikace, klikněte na odkaz hello hello základní webové aplikace Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ec75a-130">When hello list of web app templates is displayed, click hello link for hello basic Microsoft Web App.</span></span>

   ![Šablony webové aplikace][AZ03]

1. <span data-ttu-id="ec75a-132">Když se zobrazí stránka informace o hello hello šablony webové aplikace, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ec75a-132">When hello information page for hello Web App template is displayed, click **Create**.</span></span>

   ![Vytvoření webové aplikace][AZ04]

1. <span data-ttu-id="ec75a-134">Zadejte jedinečný název pro vaši webovou aplikaci a zadejte jakákoli další nastavení a potom **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ec75a-134">Provide a unique name for your web app and specify any additional settings, and then **Create**.</span></span>

   ![Vytvoření nastavení webové aplikace][AZ05]

1. <span data-ttu-id="ec75a-136">Po vytvoření webové aplikace, klikněte na ikonu nabídky hello **App Services**a potom klikněte na nově vytvořené webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="ec75a-136">Once your web app has been created, click hello menu icon for **App Services**, and then click your newly-created web app:</span></span>

   ![Seznam webových aplikací][AZ06]

1. <span data-ttu-id="ec75a-138">Když webové aplikace se zobrazí, zadejte verzi Javy hello pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec75a-138">When your web app is displayed, specify hello Java version by using hello following steps:</span></span>

   <span data-ttu-id="ec75a-139">a.</span><span class="sxs-lookup"><span data-stu-id="ec75a-139">a.</span></span> <span data-ttu-id="ec75a-140">Klikněte na tlačítko hello **nastavení aplikace** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="ec75a-140">Click hello **Application Settings** menu item.</span></span>

   <span data-ttu-id="ec75a-141">b.</span><span class="sxs-lookup"><span data-stu-id="ec75a-141">b.</span></span> <span data-ttu-id="ec75a-142">Zvolte **Java 8** pro verzi Javy hello.</span><span class="sxs-lookup"><span data-stu-id="ec75a-142">Choose **Java 8** for hello Java version.</span></span>

   <span data-ttu-id="ec75a-143">c.</span><span class="sxs-lookup"><span data-stu-id="ec75a-143">c.</span></span> <span data-ttu-id="ec75a-144">Zvolte **nejnovější** pro hello podverzi Javy.</span><span class="sxs-lookup"><span data-stu-id="ec75a-144">Choose **Newest** for hello minor Java version.</span></span>

   <span data-ttu-id="ec75a-145">d.</span><span class="sxs-lookup"><span data-stu-id="ec75a-145">d.</span></span> <span data-ttu-id="ec75a-146">Zvolte **nejnovější 8.5 Tomcat** pro hello webový kontejner.</span><span class="sxs-lookup"><span data-stu-id="ec75a-146">Choose **Newest Tomcat 8.5** for hello web container.</span></span> <span data-ttu-id="ec75a-147">(Tento kontejner ve skutečnosti se nepoužijí; Azure bude používat hello kontejneru z vaší aplikace pružiny spouštěcí.)</span><span class="sxs-lookup"><span data-stu-id="ec75a-147">(This container will not actually be used; Azure will use hello container from your Spring Boot application.)</span></span>

   <span data-ttu-id="ec75a-148">e.</span><span class="sxs-lookup"><span data-stu-id="ec75a-148">e.</span></span> <span data-ttu-id="ec75a-149">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ec75a-149">Click **Save**.</span></span>

   ![Nastavení aplikace][AZ07]

1. <span data-ttu-id="ec75a-151">Zadejte přihlašovací údaje nasazení FTP pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec75a-151">Specify your FTP deployment credentials by using hello following steps:</span></span>

   <span data-ttu-id="ec75a-152">a.</span><span class="sxs-lookup"><span data-stu-id="ec75a-152">a.</span></span> <span data-ttu-id="ec75a-153">Klikněte na tlačítko hello **přihlašovací údaje nasazení** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="ec75a-153">Click hello **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="ec75a-154">b.</span><span class="sxs-lookup"><span data-stu-id="ec75a-154">b.</span></span> <span data-ttu-id="ec75a-155">Zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="ec75a-155">Specify your username and password.</span></span>

   <span data-ttu-id="ec75a-156">c.</span><span class="sxs-lookup"><span data-stu-id="ec75a-156">c.</span></span> <span data-ttu-id="ec75a-157">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ec75a-157">Click **Save**.</span></span>

   ![Zadejte přihlašovací údaje nasazení.][AZ08]

1. <span data-ttu-id="ec75a-159">Načíst informace o připojení FTP pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec75a-159">Retrieve your FTP connection information by using hello following steps:</span></span>

   <span data-ttu-id="ec75a-160">a.</span><span class="sxs-lookup"><span data-stu-id="ec75a-160">a.</span></span> <span data-ttu-id="ec75a-161">Klikněte na tlačítko hello **přihlašovací údaje nasazení** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="ec75a-161">Click hello **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="ec75a-162">b.</span><span class="sxs-lookup"><span data-stu-id="ec75a-162">b.</span></span> <span data-ttu-id="ec75a-163">Úplné uživatelské jméno FTP a adresu URL zkopírovat a uložit pro hello další části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ec75a-163">Copy your full FTP username and URL and save them for hello next section of this tutorial.</span></span>

   ![Adresy URL FTP a přihlašovací údaje][AZ09]

## <a name="deploy-your-spring-boot-web-app-tooazure"></a><span data-ttu-id="ec75a-165">Nasazení vaší tooAzure pružiny spouštěcí webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ec75a-165">Deploy your Spring Boot web app tooAzure</span></span>

<span data-ttu-id="ec75a-166">Hello následující kroky vás provede kroky toodeploy hello vaší tooAzure pružiny spouštěcí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec75a-166">hello following steps will walk you through hello steps toodeploy your Spring Boot web app tooAzure.</span></span>

1. <span data-ttu-id="ec75a-167">Otevřete textový editor, například Poznámkový blok systému Windows a vložte následující text do nového dokumentu hello a potom uložte soubor hello jako *web.config*:</span><span class="sxs-lookup"><span data-stu-id="ec75a-167">Open a text editor such as Windows Notepad and paste hello following text into a new document, then save hello file as *web.config*:</span></span>
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. <span data-ttu-id="ec75a-168">Po uložení hello *web.config* tooyour systému souborů, připojit tooyour webové aplikace pomocí FTP pomocí adresy URL hello, uživatelského jména a hesla z hello předcházející části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ec75a-168">After you have saved hello *web.config* file tooyour system, connect tooyour web app via FTP using hello URL, username, and password from hello preceding section of this tutorial.</span></span> <span data-ttu-id="ec75a-169">Například:</span><span class="sxs-lookup"><span data-stu-id="ec75a-169">For example:</span></span>
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. <span data-ttu-id="ec75a-170">Změna hello vzdáleného adresáře toohello kořenové složky vaší webové aplikace (což je v */web/wwwroot*), pak zkopírujte soubor JAR hello z vaší aplikace pružiny spouštěcí a hello *web.config* z dříve.</span><span class="sxs-lookup"><span data-stu-id="ec75a-170">Change hello remote directory toohello root folder of your web app, (which is at */site/wwwroot*), then copy hello JAR file from your Spring Boot application and hello *web.config* from earlier.</span></span> <span data-ttu-id="ec75a-171">Například:</span><span class="sxs-lookup"><span data-stu-id="ec75a-171">For example:</span></span>
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. <span data-ttu-id="ec75a-172">Po nasazení vaší JAR a *web.config* soubory tooyour webové aplikace, budete potřebovat toorestart vaší webové aplikace pomocí hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="ec75a-172">After you have deployed your JAR and *web.config* files tooyour web app, you need toorestart your web app using hello Azure portal:</span></span>

   ![][AZ10]

1. <span data-ttu-id="ec75a-173">Test webové aplikace hello procházení URL tooyour webovou aplikaci pomocí webového prohlížeče, nebo použijte syntaxi hello jako následující příklad, pokud máte k dispozici curl hello:</span><span class="sxs-lookup"><span data-stu-id="ec75a-173">Test hello web app by browsing tooyour web app's URL using a web browser, or use hello syntax like hello following example if you have curl available:</span></span>
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. <span data-ttu-id="ec75a-174">Měli byste vidět hello následující zpráva: **pozdrav z jara spouštěcí!**</span><span class="sxs-lookup"><span data-stu-id="ec75a-174">You should see hello following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Procházet ukázkové aplikace][SB02]

## <a name="next-steps"></a><span data-ttu-id="ec75a-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec75a-176">Next steps</span></span>

<span data-ttu-id="ec75a-177">Další informace o používání pružiny spuštění aplikace v Azure najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="ec75a-177">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="ec75a-178">Nasazení aplikace spouštěcí Spring v systému Linux v hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="ec75a-178">Deploy a Spring Boot Application on Linux in hello Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [<span data-ttu-id="ec75a-179">Nasazení aplikace spouštěcí pružiny na Kubernetes Cluster hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="ec75a-179">Deploy a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="ec75a-180">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="ec75a-180">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="ec75a-181">Další informace o depoying webové aplikace tooAzure pomocí protokolu FTP najdete v tématu [nasazení vaší aplikace tooAzure služby App Service pomocí FTP nebo S].</span><span class="sxs-lookup"><span data-stu-id="ec75a-181">For additional information about depoying web apps tooAzure using FTP, see [Deploy your app tooAzure App Service using FTP/S].</span></span>

<span data-ttu-id="ec75a-182">Další podrobnosti o hello pružiny spouštěcí ukázkový projekt najdete v tématu [pružiny spouštěcí Začínáme].</span><span class="sxs-lookup"><span data-stu-id="ec75a-182">For further details about hello Spring Boot sample project, see [Spring Boot Getting Started].</span></span>

<span data-ttu-id="ec75a-183">Pomoc s Začínáme s aplikací pružiny spouštěcí najdete v tématu hello **pružiny Initializr** v https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="ec75a-183">For help with getting started with your own Spring Boot applications, see hello **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="ec75a-184">Další informace o konfigurování dalších nastavení pro webové aplikace najdete v tématu [konfigurace webových aplikací ve službě Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="ec75a-184">For more information about configuring additional settings for your web app, see [Configure web apps in Azure App Service].</span></span>

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[konfigurace webových aplikací ve službě Azure App Service]: /azure/app-service-web/web-sites-configure
[nasazení vaší aplikace tooAzure služby App Service pomocí FTP nebo S]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí Začínáme]: https://github.com/spring-guides/gs-spring-boot
[pružiny Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
