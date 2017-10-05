---
title: "Nasazení aplikace spouštěcí pružiny do Azure App Service | Microsoft Docs"
description: "V tomto kurzu Průvodce vývojářům provede kroky nasazení spouštěcí pružiny Začínáme webové aplikace do služby Azure App Service."
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
ms.openlocfilehash: 0c388862d927a1492745832225c686670c071f86
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a><span data-ttu-id="422d9-103">Nasazení aplikace Spring Boot do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="422d9-103">Deploy a Spring Boot Application to the Azure App Service</span></span>

<span data-ttu-id="422d9-104"> **[Pružiny Framework]**  open-source řešení, která pomáhá vytvářet aplikace na podnikové úrovni vývojáře v jazyce Java a jedním z dalších oblíbených projekty, které je postavená na této platformě je [ Pramenité spouštěcí], který poskytuje zjednodušenou metodu pro vytvoření samostatné aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="422d9-104">The **[Spring Framework]** is an open-source solution which helps Java developers create enterprise-level applications, and one of the more-popular projects which is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="422d9-105">Tento kurz vás provede, když vytvoření ukázkové pružiny spouštěcí Začínáme webové aplikace a nasazení jeho [Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="422d9-105">This tutorial will walk you though creating the sample Spring Boot Getting Started web app and deploying it to [Azure App Service].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="422d9-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="422d9-106">Prerequisites</span></span>

<span data-ttu-id="422d9-107">Aby bylo možné provést kroky v tomto kurzu, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="422d9-107">In order to complete the steps in this tutorial, you need to have the following:</span></span>

* <span data-ttu-id="422d9-108">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="422d9-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="422d9-109">Aktuální [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="422d9-109">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="422d9-110">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="422d9-110">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="422d9-111">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="422d9-111">A [Git] client.</span></span>

## <a name="create-the-spring-boot-getting-started-web-app"></a><span data-ttu-id="422d9-112">Vytvoření webové aplikace pružiny spouštěcí Začínáme</span><span class="sxs-lookup"><span data-stu-id="422d9-112">Create the Spring Boot Getting Started web app</span></span>

<span data-ttu-id="422d9-113">Následující postup vás provede kroky, které jsou potřebné k vytvoření jednoduché webové aplikace pružiny spouštěcí a otestovat ji místně.</span><span class="sxs-lookup"><span data-stu-id="422d9-113">The following steps will walk you through the steps that are required to create a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="422d9-114">Otevřete příkazový řádek a vytvořte místní adresář pro uložení aplikace, změnit do tohoto adresáře; například:</span><span class="sxs-lookup"><span data-stu-id="422d9-114">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="422d9-115">--nebo--</span><span class="sxs-lookup"><span data-stu-id="422d9-115">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="422d9-116">Klon [pružiny spouštěcí Začínáme] ukázkový projekt do adresáře, kterou jste právě vytvořili; například:</span><span class="sxs-lookup"><span data-stu-id="422d9-116">Clone the [Spring Boot Getting Started] sample project into the directory you just created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. <span data-ttu-id="422d9-117">Změňte adresář na dokončené projektu. například:</span><span class="sxs-lookup"><span data-stu-id="422d9-117">Change directory to the completed project; for example:</span></span>
   ```
   cd gs-spring-boot
   cd complete
   ```

1. <span data-ttu-id="422d9-118">Sestavení na soubor JAR pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="422d9-118">Build the JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="422d9-119">Po vytvoření webové aplikace, změňte adresář na soubor JAR a spustí webovou aplikaci; například:</span><span class="sxs-lookup"><span data-stu-id="422d9-119">Once the web app has been created, change directory to the JAR file and start the web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. <span data-ttu-id="422d9-120">Test webové aplikace tak, že přejde na adrese http://localhost: 8080 pomocí webového prohlížeče, nebo použijte syntaxi jako v následujícím příkladu, pokud máte curl k dispozici:</span><span class="sxs-lookup"><span data-stu-id="422d9-120">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="422d9-121">Měli byste vidět zobrazenou následující zprávu: **pozdrav z jara spouštěcí!**</span><span class="sxs-lookup"><span data-stu-id="422d9-121">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Procházet ukázkové aplikace][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a><span data-ttu-id="422d9-123">Vytvoření webové aplikace Azure pro použití s Javou</span><span class="sxs-lookup"><span data-stu-id="422d9-123">Create an Azure web app for use with Java</span></span>

<span data-ttu-id="422d9-124">Následující postup vás provede kroky k vytvoření webové aplikace Azure, nakonfigurujte požadovaná nastavení pro jazyk Java a konfigurovat přihlašovací údaje serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="422d9-124">The following steps will walk you through the steps to create an Azure Web App, configure the required settings for Java, and configure your FTP credentials.</span></span>

1. <span data-ttu-id="422d9-125">Vyhledejte [portál Azure] a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="422d9-125">Browse to the [Azure portal] and log in.</span></span>

1. <span data-ttu-id="422d9-126">Po přihlášení ke svému účtu na portálu Azure klikněte na ikonu nabídky **App Services**:</span><span class="sxs-lookup"><span data-stu-id="422d9-126">Once you have logged into your account on the Azure portal, click the menu icon for **App Services**:</span></span>
   
   ![portál Azure][AZ01]

1. <span data-ttu-id="422d9-128">Když **App Services** stránky se zobrazí, klikněte na tlačítko **+ přidat** k vytvoření nové aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="422d9-128">When the **App Services** page is displayed, click **+ Add** to create a new App Service.</span></span>

   ![Vytvoření služby App Service][AZ02]

1. <span data-ttu-id="422d9-130">Když se zobrazí v seznamu šablon webové aplikace, klikněte na odkaz pro základní webové aplikace Microsoft.</span><span class="sxs-lookup"><span data-stu-id="422d9-130">When the list of web app templates is displayed, click the link for the basic Microsoft Web App.</span></span>

   ![Šablony webové aplikace][AZ03]

1. <span data-ttu-id="422d9-132">Pokud stránka informace o šabloně webové aplikace se zobrazí, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="422d9-132">When the information page for the Web App template is displayed, click **Create**.</span></span>

   ![Vytvoření webové aplikace][AZ04]

1. <span data-ttu-id="422d9-134">Zadejte jedinečný název pro vaši webovou aplikaci a zadejte jakákoli další nastavení a potom **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="422d9-134">Provide a unique name for your web app and specify any additional settings, and then **Create**.</span></span>

   ![Vytvoření nastavení webové aplikace][AZ05]

1. <span data-ttu-id="422d9-136">Po vytvoření webové aplikace, klikněte na ikonu nabídky **App Services**a potom klikněte na nově vytvořené webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="422d9-136">Once your web app has been created, click the menu icon for **App Services**, and then click your newly-created web app:</span></span>

   ![Seznam webových aplikací][AZ06]

1. <span data-ttu-id="422d9-138">Když webové aplikace se zobrazí, zadejte verzi Javy pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="422d9-138">When your web app is displayed, specify the Java version by using the following steps:</span></span>

   <span data-ttu-id="422d9-139">a.</span><span class="sxs-lookup"><span data-stu-id="422d9-139">a.</span></span> <span data-ttu-id="422d9-140">Klikněte **nastavení aplikace** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="422d9-140">Click the **Application Settings** menu item.</span></span>

   <span data-ttu-id="422d9-141">b.</span><span class="sxs-lookup"><span data-stu-id="422d9-141">b.</span></span> <span data-ttu-id="422d9-142">Zvolte **Java 8** pro verzi Javy.</span><span class="sxs-lookup"><span data-stu-id="422d9-142">Choose **Java 8** for the Java version.</span></span>

   <span data-ttu-id="422d9-143">c.</span><span class="sxs-lookup"><span data-stu-id="422d9-143">c.</span></span> <span data-ttu-id="422d9-144">Zvolte **nejnovější** pro podverzi Javy.</span><span class="sxs-lookup"><span data-stu-id="422d9-144">Choose **Newest** for the minor Java version.</span></span>

   <span data-ttu-id="422d9-145">d.</span><span class="sxs-lookup"><span data-stu-id="422d9-145">d.</span></span> <span data-ttu-id="422d9-146">Zvolte **nejnovější 8.5 Tomcat** pro webový kontejner.</span><span class="sxs-lookup"><span data-stu-id="422d9-146">Choose **Newest Tomcat 8.5** for the web container.</span></span> <span data-ttu-id="422d9-147">(Tento kontejner ve skutečnosti se nepoužijí; Azure pomocí kontejneru z vaší aplikace pružiny spouštěcí.)</span><span class="sxs-lookup"><span data-stu-id="422d9-147">(This container will not actually be used; Azure will use the container from your Spring Boot application.)</span></span>

   <span data-ttu-id="422d9-148">e.</span><span class="sxs-lookup"><span data-stu-id="422d9-148">e.</span></span> <span data-ttu-id="422d9-149">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="422d9-149">Click **Save**.</span></span>

   ![Nastavení aplikace][AZ07]

1. <span data-ttu-id="422d9-151">Zadejte přihlašovací údaje nasazení FTP pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="422d9-151">Specify your FTP deployment credentials by using the following steps:</span></span>

   <span data-ttu-id="422d9-152">a.</span><span class="sxs-lookup"><span data-stu-id="422d9-152">a.</span></span> <span data-ttu-id="422d9-153">Klikněte **přihlašovací údaje nasazení** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="422d9-153">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="422d9-154">b.</span><span class="sxs-lookup"><span data-stu-id="422d9-154">b.</span></span> <span data-ttu-id="422d9-155">Zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="422d9-155">Specify your username and password.</span></span>

   <span data-ttu-id="422d9-156">c.</span><span class="sxs-lookup"><span data-stu-id="422d9-156">c.</span></span> <span data-ttu-id="422d9-157">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="422d9-157">Click **Save**.</span></span>

   ![Zadejte přihlašovací údaje nasazení.][AZ08]

1. <span data-ttu-id="422d9-159">Načíst informace o připojení FTP pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="422d9-159">Retrieve your FTP connection information by using the following steps:</span></span>

   <span data-ttu-id="422d9-160">a.</span><span class="sxs-lookup"><span data-stu-id="422d9-160">a.</span></span> <span data-ttu-id="422d9-161">Klikněte **přihlašovací údaje nasazení** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="422d9-161">Click the **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="422d9-162">b.</span><span class="sxs-lookup"><span data-stu-id="422d9-162">b.</span></span> <span data-ttu-id="422d9-163">Úplné uživatelské jméno FTP a adresu URL zkopírovat a uložit pro další části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="422d9-163">Copy your full FTP username and URL and save them for the next section of this tutorial.</span></span>

   ![Adresy URL FTP a přihlašovací údaje][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a><span data-ttu-id="422d9-165">Nasazení webové aplikace pružiny spouštěcí do Azure</span><span class="sxs-lookup"><span data-stu-id="422d9-165">Deploy your Spring Boot web app to Azure</span></span>

<span data-ttu-id="422d9-166">Následující postup vás provede kroky nasazení spouštěcí pružiny webové aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="422d9-166">The following steps will walk you through the steps to deploy your Spring Boot web app to Azure.</span></span>

1. <span data-ttu-id="422d9-167">Otevřít textového editoru, například Windows Poznámkový blok, vložte následující text do nového dokumentu a pak soubor uložte jako *web.config*:</span><span class="sxs-lookup"><span data-stu-id="422d9-167">Open a text editor such as Windows Notepad and paste the following text into a new document, then save the file as *web.config*:</span></span>
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

1. <span data-ttu-id="422d9-168">Po uložení *web.config* souboru k vašemu systému, připojí se k vaší webové aplikace přes FTP pomocí adresy URL, uživatelské jméno a heslo z předchozí části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="422d9-168">After you have saved the *web.config* file to your system, connect to your web app via FTP using the URL, username, and password from the preceding section of this tutorial.</span></span> <span data-ttu-id="422d9-169">Například:</span><span class="sxs-lookup"><span data-stu-id="422d9-169">For example:</span></span>
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. <span data-ttu-id="422d9-170">Změnit vzdáleného adresáře do kořenové složky vaší webové aplikace (což je v */web/wwwroot*), pak zkopírujte soubor JAR z vaší aplikace pružiny spouštěcí a *web.config* z dříve.</span><span class="sxs-lookup"><span data-stu-id="422d9-170">Change the remote directory to the root folder of your web app, (which is at */site/wwwroot*), then copy the JAR file from your Spring Boot application and the *web.config* from earlier.</span></span> <span data-ttu-id="422d9-171">Například:</span><span class="sxs-lookup"><span data-stu-id="422d9-171">For example:</span></span>
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. <span data-ttu-id="422d9-172">Po nasazení vaší JAR a *web.config* soubory do vaší webové aplikace, budete muset restartovat webovou aplikaci pomocí portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="422d9-172">After you have deployed your JAR and *web.config* files to your web app, you need to restart your web app using the Azure portal:</span></span>

   ![][AZ10]

1. <span data-ttu-id="422d9-173">Test webové aplikace tak, že přejde k adrese URL webové aplikace pomocí webového prohlížeče, nebo použijte syntaxi jako v následujícím příkladu, pokud máte curl k dispozici:</span><span class="sxs-lookup"><span data-stu-id="422d9-173">Test the web app by browsing to your web app's URL using a web browser, or use the syntax like the following example if you have curl available:</span></span>
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. <span data-ttu-id="422d9-174">Měli byste vidět zobrazenou následující zprávu: **pozdrav z jara spouštěcí!**</span><span class="sxs-lookup"><span data-stu-id="422d9-174">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Procházet ukázkové aplikace][SB02]

## <a name="next-steps"></a><span data-ttu-id="422d9-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="422d9-176">Next steps</span></span>

<span data-ttu-id="422d9-177">Další informace o používání pružiny spuštění aplikace v Azure najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="422d9-177">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="422d9-178">Nasazení aplikace spouštěcí Spring v systému Linux v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="422d9-178">Deploy a Spring Boot Application on Linux in the Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [<span data-ttu-id="422d9-179">Nasazení aplikace spouštěcí pružiny na Cluster Kubernetes v Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="422d9-179">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="422d9-180">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).</span><span class="sxs-lookup"><span data-stu-id="422d9-180">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="422d9-181">Další informace o depoying webové aplikace do Azure pomocí FTP, najdete v tématu [nasazení vaší aplikace do Azure App Service pomocí FTP nebo S].</span><span class="sxs-lookup"><span data-stu-id="422d9-181">For additional information about depoying web apps to Azure using FTP, see [Deploy your app to Azure App Service using FTP/S].</span></span>

<span data-ttu-id="422d9-182">Další podrobnosti o ukázkový projekt pružiny spouštěcí najdete v tématu [pružiny spouštěcí Začínáme].</span><span class="sxs-lookup"><span data-stu-id="422d9-182">For further details about the Spring Boot sample project, see [Spring Boot Getting Started].</span></span>

<span data-ttu-id="422d9-183">Pomoc s Začínáme s aplikací pružiny spouštěcí najdete v tématu **pružiny Initializr** v https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="422d9-183">For help with getting started with your own Spring Boot applications, see the **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="422d9-184">Další informace o konfigurování dalších nastavení pro webové aplikace najdete v tématu [konfigurace webových aplikací ve službě Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="422d9-184">For more information about configuring additional settings for your web app, see [Configure web apps in Azure App Service].</span></span>

<!-- URL List -->

<span data-ttu-id="422d9-185">[Azure App Service]: https://azure.microsoft.com/services/app-service/</span><span class="sxs-lookup"><span data-stu-id="422d9-185">[Azure App Service]: https://azure.microsoft.com/services/app-service/</span></span>
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
<span data-ttu-id="422d9-186">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="422d9-186">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="422d9-187">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="422d9-187">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="422d9-188">[konfigurace webových aplikací ve službě Azure App Service]: /azure/app-service-web/web-sites-configure</span><span class="sxs-lookup"><span data-stu-id="422d9-188">[Configure web apps in Azure App Service]: /azure/app-service-web/web-sites-configure</span></span>
<span data-ttu-id="422d9-189">[nasazení vaší aplikace do Azure App Service pomocí FTP nebo S]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp</span><span class="sxs-lookup"><span data-stu-id="422d9-189">[Deploy your app to Azure App Service using FTP/S]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp</span></span>
<span data-ttu-id="422d9-190">[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="422d9-190">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="422d9-191">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="422d9-191">[Git]: https://github.com/</span></span>
<span data-ttu-id="422d9-192">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span><span class="sxs-lookup"><span data-stu-id="422d9-192">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span></span>
<span data-ttu-id="422d9-193">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="422d9-193">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>
<span data-ttu-id="422d9-194">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="422d9-194">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="422d9-195">[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="422d9-195">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="422d9-196">[ Pramenité spouštěcí]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="422d9-196">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="422d9-197">[pružiny spouštěcí Začínáme]: https://github.com/spring-guides/gs-spring-boot</span><span class="sxs-lookup"><span data-stu-id="422d9-197">[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot</span></span>
<span data-ttu-id="422d9-198">[Pružiny Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="422d9-198">[Spring Framework]: https://spring.io/</span></span>

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
