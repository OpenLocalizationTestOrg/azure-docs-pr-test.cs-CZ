---
title: "aaaUpload vlastní tooAzure webové aplikace Java"
description: "Tento kurz ukazuje, jak tooupload vlastní Java webové aplikace tooAzure App Service Web Apps."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: eb2ccd83-e5c6-444e-a0fc-08fd5cc8326c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 0cb4a682bb25d86ff08bfd03628c89795c58451e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-custom-java-web-app-tooazure"></a><span data-ttu-id="39c85-103">Nahrát vlastní tooAzure webové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="39c85-103">Upload a custom Java web app tooAzure</span></span>
<span data-ttu-id="39c85-104">Toto téma vysvětluje, jak tooupload vlastní Java webová aplikace příliš[Azure App Service] webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c85-104">This topic explains how tooupload a custom Java web app too[Azure App Service] Web Apps.</span></span> <span data-ttu-id="39c85-105">Jsou zde zahrnuty informace, která se použije tooany Java web nebo webovou aplikaci a také některé příklady pro konkrétní aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c85-105">Included is information that applies tooany Java website or web app, and also some examples for specific applications.</span></span>

<span data-ttu-id="39c85-106">Všimněte si, že Azure poskytuje prostředky pro vytvoření webové aplikace Java pomocí portálu Azure hello konfigurace uživatelského rozhraní a hello Azure Marketplace, jak je uvedeno v [vytvoření webové aplikace Java v Azure App Service](web-sites-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="39c85-106">Note that Azure provides a means for creating Java web apps using hello Azure Portal's configuration UI, and hello Azure Marketplace, as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md).</span></span> <span data-ttu-id="39c85-107">Tento kurz je určen pro scénáře, ve kterých není má uživatelské rozhraní konfigurace portálu Azure hello toouse nebo hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="39c85-107">This tutorial is for scenarios in which you do not want toouse hello Azure Portal configuration UI or hello Azure Marketplace.</span></span>  

## <a name="configuration-guidelines"></a><span data-ttu-id="39c85-108">Pokyny pro konfigurace</span><span class="sxs-lookup"><span data-stu-id="39c85-108">Configuration guidelines</span></span>
<span data-ttu-id="39c85-109">Hello následující text popisuje hello nastavení pro vlastní webové aplikace v jazyce Java v Azure.</span><span class="sxs-lookup"><span data-stu-id="39c85-109">hello following describes hello settings expected for custom Java web apps on Azure.</span></span>

* <span data-ttu-id="39c85-110">port HTTP Hello používá hello procesu Java je dynamicky přiřadit.</span><span class="sxs-lookup"><span data-stu-id="39c85-110">hello HTTP port used by hello Java process is dynamically assigned.</span></span>  <span data-ttu-id="39c85-111">proces Hello musí používat port hello z proměnné prostředí hello `HTTP_PLATFORM_PORT`.</span><span class="sxs-lookup"><span data-stu-id="39c85-111">hello process must use hello port from hello environment variable `HTTP_PLATFORM_PORT`.</span></span>
* <span data-ttu-id="39c85-112">Všechny naslouchat porty jiného, než by mělo být zakázáno hello jeden naslouchací proces protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="39c85-112">All listen ports other than hello single HTTP listener should be disabled.</span></span>  <span data-ttu-id="39c85-113">V Tomcat, který obsahuje hello vypnutí, HTTPS a AJP porty.</span><span class="sxs-lookup"><span data-stu-id="39c85-113">In Tomcat, that includes hello shutdown, HTTPS, and AJP ports.</span></span>
* <span data-ttu-id="39c85-114">kontejner Hello musí toobe nakonfigurované pro jenom přenosy protokolu IPv4.</span><span class="sxs-lookup"><span data-stu-id="39c85-114">hello container needs toobe configured for IPv4 traffic only.</span></span>
* <span data-ttu-id="39c85-115">Hello **spuštění** příkaz pro aplikace hello musí toobe nastavit v konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="39c85-115">hello **startup** command for hello application needs toobe set in hello configuration.</span></span>
* <span data-ttu-id="39c85-116">Aplikace, které vyžadují oprávnění zapisovat adresáře s potřebovat toobe umístěný v adresáři obsahu hello Azure webovou aplikaci, která je **D:\home**.</span><span class="sxs-lookup"><span data-stu-id="39c85-116">Applications that require directories with write permission need toobe located in hello Azure web app's content directory,  which is **D:\home**.</span></span>  <span data-ttu-id="39c85-117">proměnné prostředí Hello `HOME` odkazuje tooD:\home.</span><span class="sxs-lookup"><span data-stu-id="39c85-117">hello environmental variable `HOME` refers tooD:\home.</span></span>  

<span data-ttu-id="39c85-118">Podle potřeby můžete nastavit proměnné prostředí v souboru web.config hello.</span><span class="sxs-lookup"><span data-stu-id="39c85-118">You can set environment variables as required in hello web.config file.</span></span>

## <a name="webconfig-httpplatform-configuration"></a><span data-ttu-id="39c85-119">soubor Web.config httpPlatform konfigurace</span><span class="sxs-lookup"><span data-stu-id="39c85-119">web.config httpPlatform configuration</span></span>
<span data-ttu-id="39c85-120">Hello následující informace popisují hello **httpPlatform** formátu v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="39c85-120">hello following information describes hello **httpPlatform** format within web.config.</span></span>

<span data-ttu-id="39c85-121">**argumenty** (výchozí = "").</span><span class="sxs-lookup"><span data-stu-id="39c85-121">**arguments** (Default="").</span></span> <span data-ttu-id="39c85-122">Argumenty toohello spustitelný soubor nebo skript zadaný v hello **processPath** nastavení.</span><span class="sxs-lookup"><span data-stu-id="39c85-122">Arguments toohello executable or script specified in hello **processPath** setting.</span></span>

<span data-ttu-id="39c85-123">Příklady (ukázka s **processPath** zahrnuté):</span><span class="sxs-lookup"><span data-stu-id="39c85-123">Examples (shown with **processPath** included):</span></span>

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


<span data-ttu-id="39c85-124">**processPath** -cesta toohello spustitelný soubor nebo skript, který se spustí proces naslouchání požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="39c85-124">**processPath** - Path toohello executable or script that will launch a process listening for HTTP requests.</span></span>

<span data-ttu-id="39c85-125">Příklady:</span><span class="sxs-lookup"><span data-stu-id="39c85-125">Examples:</span></span>

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

<span data-ttu-id="39c85-126">**rapidFailsPerMinute** (výchozí = 10.) Počet opakování, proces hello zadaný v **processPath** je povoleno toocrash za minutu.</span><span class="sxs-lookup"><span data-stu-id="39c85-126">**rapidFailsPerMinute** (Default=10.) Number of times hello process specified in **processPath** is allowed toocrash per minute.</span></span> <span data-ttu-id="39c85-127">Pokud je tento limit překročen, **HttpPlatformHandler** zastaví spuštění procesu hello hello zbývající hello minuta.</span><span class="sxs-lookup"><span data-stu-id="39c85-127">If this limit is exceeded, **HttpPlatformHandler** will stop launching hello process for hello remainder of hello minute.</span></span>

<span data-ttu-id="39c85-128">**requestTimeout** (výchozí = "00: 02:00".) Doba trvání, pro kterou **HttpPlatformHandler** bude čekat na odpověď od hello proces naslouchání na portu `%HTTP_PLATFORM_PORT%`.</span><span class="sxs-lookup"><span data-stu-id="39c85-128">**requestTimeout** (Default="00:02:00".) Duration for which **HttpPlatformHandler** will wait for a response from hello process listening on `%HTTP_PLATFORM_PORT%`.</span></span>

<span data-ttu-id="39c85-129">**startupRetryCount** (výchozí = 10.) Počet nezdařených pokusů o **HttpPlatformHandler** pokusí toolaunch hello proces zadaný v **processPath**.</span><span class="sxs-lookup"><span data-stu-id="39c85-129">**startupRetryCount** (Default=10.) Number of times **HttpPlatformHandler** will try toolaunch hello process specified in **processPath**.</span></span> <span data-ttu-id="39c85-130">V tématu **startupTimeLimit** další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="39c85-130">See **startupTimeLimit** for more details.</span></span>

<span data-ttu-id="39c85-131">**startupTimeLimit** (výchozí = 10 sekund.) Doba trvání, pro kterou **HttpPlatformHandler** bude čekat hello spustitelný soubor nebo skript toostart proces naslouchání na portu hello.</span><span class="sxs-lookup"><span data-stu-id="39c85-131">**startupTimeLimit** (Default=10 seconds.) Duration for which **HttpPlatformHandler** will wait for hello executable/script toostart a process listening on hello port.</span></span>  <span data-ttu-id="39c85-132">Pokud je tento časový limit překročen, **HttpPlatformHandler** bude ukončit proces hello a zkuste to toolaunch ho znovu **startupRetryCount** časy.</span><span class="sxs-lookup"><span data-stu-id="39c85-132">If this time limit is exceeded, **HttpPlatformHandler** will kill hello process and try toolaunch it again **startupRetryCount** times.</span></span>

<span data-ttu-id="39c85-133">**stdoutLogEnabled** (výchozí = "true".) V případě hodnoty true **stdout** a **stderr** pro proces hello zadaný v hello **processPath** bude nastavení přesměrovaného toohello soubor zadaný v  **stdoutLogFile** (viz **stdoutLogFile** části).</span><span class="sxs-lookup"><span data-stu-id="39c85-133">**stdoutLogEnabled** (Default="true".) If true, **stdout** and **stderr** for hello process specified in hello **processPath** setting will be redirected toohello file specified in **stdoutLogFile** (see **stdoutLogFile** section).</span></span>

<span data-ttu-id="39c85-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Absolutní cesty, pro kterou **stdout** a **stderr** z procesu hello zadaný v **processPath** bude do protokolu.</span><span class="sxs-lookup"><span data-stu-id="39c85-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Absolute file path for which **stdout** and **stderr** from hello process specified in **processPath** will be logged.</span></span>

> [!NOTE]
> <span data-ttu-id="39c85-135">`%HTTP_PLATFORM_PORT%`je speciální zástupný symbol, který se musí buď jako součást toospecified **argumenty** nebo jako součást hello **httpPlatform** **environmentVariables** seznamu.</span><span class="sxs-lookup"><span data-stu-id="39c85-135">`%HTTP_PLATFORM_PORT%` is a special placeholder which needs toospecified either as part of **arguments** or as part of hello **httpPlatform** **environmentVariables** list.</span></span> <span data-ttu-id="39c85-136">To budou nahrazeny ve interně generovaného port **HttpPlatformHandler** tak, aby proces hello určeného **processPath** můžete naslouchat na tento port.</span><span class="sxs-lookup"><span data-stu-id="39c85-136">This will be replaced by an internally generated port by **HttpPlatformHandler** so that hello process specified by **processPath** can listen on this port.</span></span>
> 
> 

## <a name="deployment"></a><span data-ttu-id="39c85-137">Nasazení</span><span class="sxs-lookup"><span data-stu-id="39c85-137">Deployment</span></span>
<span data-ttu-id="39c85-138">Webové aplikace Java na základě můžou být nasazené snadno prostřednictvím většinu hello stejné znamená, které se používají s hello Internetové informační služby (IIS) na základě webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c85-138">Java based web apps can be deployed easily through most of hello same means that are used with hello Internet Information Services (IIS) based web applications.</span></span>  <span data-ttu-id="39c85-139">FTP, Git a Kudu jsou všechny podporované jako nasazení mechanismy, jako je hello integrované schopnosti SCM pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c85-139">FTP, Git and Kudu are all supported as deployment mechanisms, as is hello integrated SCM capability for web apps.</span></span> <span data-ttu-id="39c85-140">Web Deploy funguje jako protokol, ale jako Java není vyvinuté v sadě Visual Studio, Web Deploy nevejde s případy použití pro nasazení Java webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c85-140">WebDeploy works as a protocol, however, as Java is not developed in Visual Studio, WebDeploy does not fit with Java web app deployment use cases.</span></span>

## <a name="application-configuration-examples"></a><span data-ttu-id="39c85-141">Konfigurace aplikace příklady</span><span class="sxs-lookup"><span data-stu-id="39c85-141">Application configuration Examples</span></span>
<span data-ttu-id="39c85-142">Pro následující aplikace, soubor web.config a hello hello konfigurace aplikace je k dispozici jako příklady tooshow jak tooenable aplikace v jazyce Java v App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="39c85-142">For hello following applications, a web.config file and hello application configuration is provided as examples tooshow how tooenable your Java application on App Service Web Apps.</span></span>

### <a name="tomcat"></a><span data-ttu-id="39c85-143">Tomcat</span><span class="sxs-lookup"><span data-stu-id="39c85-143">Tomcat</span></span>
<span data-ttu-id="39c85-144">Existují dvě varianty na Tomcat, které se dodávají s App Service Web Apps, je stále poměrně možné tooupload zákazníka určité instance.</span><span class="sxs-lookup"><span data-stu-id="39c85-144">While there are two variations on Tomcat that are supplied with App Service Web Apps, it is still quite possible tooupload customer specific instances.</span></span> <span data-ttu-id="39c85-145">Dole je příklad instalace Tomcat s různých Java Virtual Machine (JVM).</span><span class="sxs-lookup"><span data-stu-id="39c85-145">Below is an example of an install of Tomcat with a different Java Virtual Machine (JVM).</span></span>

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat" 
            arguments="">
          <environmentVariables>
            <environmentVariable name="CATALINA_OPTS" value="-Dport.http=%HTTP_PLATFORM_PORT%" />
            <environmentVariable name="CATALINA_HOME" value="%HOME%\site\wwwroot\bin\tomcat" />
            <environmentVariable name="JRE_HOME" value="%HOME%\site\wwwroot\bin\java" /> <!-- optional, if not specified, this will default too%programfiles%\Java -->
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

<span data-ttu-id="39c85-146">U hello Tomcat straně se několik změny konfigurace, které je třeba toobe provedeny.</span><span class="sxs-lookup"><span data-stu-id="39c85-146">On hello Tomcat side, there are a few configuration changes that need toobe made.</span></span> <span data-ttu-id="39c85-147">Hello server.xml potřebuje tooset toobe upravit:</span><span class="sxs-lookup"><span data-stu-id="39c85-147">hello server.xml needs toobe edited tooset:</span></span>

* <span data-ttu-id="39c85-148">Vypnutí portu = -1</span><span class="sxs-lookup"><span data-stu-id="39c85-148">Shutdown port = -1</span></span>
* <span data-ttu-id="39c85-149">Konektor port HTTP = ${port.http}</span><span class="sxs-lookup"><span data-stu-id="39c85-149">HTTP connector port = ${port.http}</span></span>
* <span data-ttu-id="39c85-150">Adresa konektor HTTP = "127.0.0.1"</span><span class="sxs-lookup"><span data-stu-id="39c85-150">HTTP connector address = "127.0.0.1"</span></span>
* <span data-ttu-id="39c85-151">Komentář HTTPS a AJP konektory</span><span class="sxs-lookup"><span data-stu-id="39c85-151">Comment out HTTPS and AJP connectors</span></span>
* <span data-ttu-id="39c85-152">nastavení IPv4 Hello lze nastavit i v souboru catalina.properties hello, kde můžete přidat`java.net.preferIPv4Stack=true`</span><span class="sxs-lookup"><span data-stu-id="39c85-152">hello IPv4 setting can also be set in hello catalina.properties file where you can add     `java.net.preferIPv4Stack=true`</span></span>

<span data-ttu-id="39c85-153">Direct3D – hovory nejsou podporovány v App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="39c85-153">Direct3d calls are not supported on App Service Web Apps.</span></span> <span data-ttu-id="39c85-154">toodisable ty, přidejte následující možnost Java měli aplikace těchto volání hello:`-Dsun.java2d.d3d=false`</span><span class="sxs-lookup"><span data-stu-id="39c85-154">toodisable those, add hello following Java option should your application make such calls: `-Dsun.java2d.d3d=false`</span></span>

### <a name="jetty"></a><span data-ttu-id="39c85-155">Jetty</span><span class="sxs-lookup"><span data-stu-id="39c85-155">Jetty</span></span>
<span data-ttu-id="39c85-156">Stejně jako hello případě pro Tomcat, zákazníků můžete nahrát vlastní instance pro Jetty.</span><span class="sxs-lookup"><span data-stu-id="39c85-156">As is hello case for Tomcat, customers can upload their own instances for Jetty.</span></span> <span data-ttu-id="39c85-157">V případě hello spuštěné hello úplné instalace Jetty hello konfigurace bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="39c85-157">In hello case of running hello full install of Jetty, hello configuration would look like this:</span></span>

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httppPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" 
             arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP_PLATFORM_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"
            startupTimeLimit="20"
          startupRetryCount="10"
          stdoutLogEnabled="true">
        </httpPlatform>
      </system.webServer>
    </configuration>

<span data-ttu-id="39c85-158">Hello Jetty je konfiguraci změnit v hello start.ini tooset toobe `java.net.preferIPv4Stack=true`.</span><span class="sxs-lookup"><span data-stu-id="39c85-158">hello Jetty configuration needs toobe changed in hello start.ini tooset `java.net.preferIPv4Stack=true`.</span></span>

### <a name="springboot"></a><span data-ttu-id="39c85-159">Springboot</span><span class="sxs-lookup"><span data-stu-id="39c85-159">Springboot</span></span>
<span data-ttu-id="39c85-160">Aplikace s v pořadí tooget Springboot potřebovat tooupload váš soubor JAR nebo WAR a přidejte hello následující soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="39c85-160">In order tooget a Springboot application running you need tooupload your JAR or WAR file and add hello following web.config file.</span></span> <span data-ttu-id="39c85-161">soubor web.config Hello přejde do složky wwwroot hello.</span><span class="sxs-lookup"><span data-stu-id="39c85-161">hello web.config file goes into hello wwwroot folder.</span></span> <span data-ttu-id="39c85-162">V souboru web.config hello upravit soubor JAR hello argumenty toopoint tooyour, v hello následující soubor JAR hello příklad nachází ve složce aplikace hello wwwroot také.</span><span class="sxs-lookup"><span data-stu-id="39c85-162">In hello web.config adjust hello arguments toopoint tooyour JAR file, in hello following example hello JAR file is located in hello wwwroot folder as well.</span></span>  

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
            arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\my-web-project.jar&quot;">
        </httpPlatform>
      </system.webServer>
    </configuration>


### <a name="hudson"></a><span data-ttu-id="39c85-163">Hudsonem</span><span class="sxs-lookup"><span data-stu-id="39c85-163">Hudson</span></span>
<span data-ttu-id="39c85-164">Naše testovací používá hello Hudsonem 3.1.2 war a hello výchozí Tomcat 7.0.50 instanci, ale bez použití hello uživatelského rozhraní tooset věcí.</span><span class="sxs-lookup"><span data-stu-id="39c85-164">Our test used hello Hudson 3.1.2 war and hello default Tomcat 7.0.50 instance but without using hello UI tooset things up.</span></span>  <span data-ttu-id="39c85-165">Vzhledem k tomu Hudsonem je nástroj sestavení, je doporučené tooinstall ho na vyhrazené instance, kde hello **AlwaysOn** příznak lze nastavit na hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c85-165">Because Hudson is a software build tool, it is advised tooinstall it on dedicated instances where hello **AlwaysOn** flag can be set on hello web app.</span></span>

1. <span data-ttu-id="39c85-166">Ve vaší webové aplikaci kořenový adresář, tedy **d:\home\site\wwwroot**, vytvoření **webapps** adresáře (Pokud již existuje) a umístěte Hudson.war v **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="39c85-166">In your web app’s root directory, i.e., **d:\home\site\wwwroot**, create a **webapps** directory (if not already present), and place Hudson.war in **d:\home\site\wwwroot\webapps**.</span></span>
2. <span data-ttu-id="39c85-167">Stáhnout apache maven 3.0.5 (kompatibilní s Hudsonem) a umístěte jej v **d:\home\site\wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="39c85-167">Download apache maven 3.0.5 (compatible with Hudson) and place it in **d:\home\site\wwwroot**.</span></span>
3. <span data-ttu-id="39c85-168">Vytvoření souboru web.config v **d:\home\site\wwwroot** a hello vložte následující obsah v něm:</span><span class="sxs-lookup"><span data-stu-id="39c85-168">Create web.config in **d:\home\site\wwwroot** and paste hello following contents in it:</span></span>
   
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>
          <system.webServer>
            <handlers>
              <add name="httppPlatformHandler" path="*" verb="*" 
        modules="httpPlatformHandler" resourceType="Unspecified" />
            </handlers>
            <httpPlatform processPath="%AZURE_TOMCAT7_HOME%\bin\startup.bat"
        startupTimeLimit="20"
        startupRetryCount="10">
        <environmentVariables>
          <environmentVariable name="HUDSON_HOME" 
        value="%HOME%\site\wwwroot\hudson_home" />
          <environmentVariable name="JAVA_OPTS" 
        value="-Djava.net.preferIPv4Stack=true -Duser.home=%HOME%/site/wwwroot/user_home -Dhudson.DNSMultiCast.disabled=true" />
        </environmentVariables>            
            </httpPlatform>
          </system.webServer>
        </configuration>
   
    <span data-ttu-id="39c85-169">Hello webové aplikace v tomto okamžiku může být restartovat tootake hello změny.</span><span class="sxs-lookup"><span data-stu-id="39c85-169">At this point hello web app can be restarted tootake hello changes.</span></span>  <span data-ttu-id="39c85-170">Připojte toohttp://yourwebapp/hudson toostart Hudsonem.</span><span class="sxs-lookup"><span data-stu-id="39c85-170">Connect toohttp://yourwebapp/hudson toostart Hudson.</span></span>
4. <span data-ttu-id="39c85-171">Po Hudsonem automaticky nakonfiguruje, měli byste vidět hello následující obrazovka:</span><span class="sxs-lookup"><span data-stu-id="39c85-171">After Hudson configures itself, you should see hello following screen:</span></span>
   
    ![Hudsonem](./media/web-sites-java-custom-upload/hudson1.png)
5. <span data-ttu-id="39c85-173">Přístup hello stránku konfigurace Hudsonem: klikněte na tlačítko **spravovat Hudsonem**a potom klikněte na **nakonfigurujte systém**.</span><span class="sxs-lookup"><span data-stu-id="39c85-173">Access hello Hudson configuration page: Click **Manage Hudson**, and then click **Configure System**.</span></span>
6. <span data-ttu-id="39c85-174">Nakonfigurujte hello JDK, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="39c85-174">Configure hello JDK as shown below:</span></span>
   
    ![Konfigurace Hudsonem](./media/web-sites-java-custom-upload/hudson2.png)
7. <span data-ttu-id="39c85-176">Konfigurace Maven, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="39c85-176">Configure Maven as shown below:</span></span>
   
    ![Konfigurace maven](./media/web-sites-java-custom-upload/maven.png)
8. <span data-ttu-id="39c85-178">Uložte nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="39c85-178">Save hello settings.</span></span> <span data-ttu-id="39c85-179">Hudsonem by teď měly být konfigurována a připravena k použití.</span><span class="sxs-lookup"><span data-stu-id="39c85-179">Hudson should now be configured and ready for use.</span></span>

<span data-ttu-id="39c85-180">Další informace o Hudsonem najdete v tématu [http://hudson-ci.org](http://hudson-ci.org).</span><span class="sxs-lookup"><span data-stu-id="39c85-180">For additional information on Hudson, see [http://hudson-ci.org](http://hudson-ci.org).</span></span>

### <a name="liferay"></a><span data-ttu-id="39c85-181">Liferay</span><span class="sxs-lookup"><span data-stu-id="39c85-181">Liferay</span></span>
<span data-ttu-id="39c85-182">Liferay je podporována v App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="39c85-182">Liferay is supported on App Service Web Apps.</span></span> <span data-ttu-id="39c85-183">Vzhledem k tomu, že Liferay může vyžadovat velké paměti, hello webová aplikace musí toorun na střední a velké vyhrazené pracovní, což může zajistit dostatek paměti.</span><span class="sxs-lookup"><span data-stu-id="39c85-183">Since Liferay can require significant memory, hello web app needs toorun on a medium or large dedicated worker, which can provide enough memory.</span></span> <span data-ttu-id="39c85-184">Liferay také trvá několik minut toostart.</span><span class="sxs-lookup"><span data-stu-id="39c85-184">Liferay also takes several minutes toostart up.</span></span> <span data-ttu-id="39c85-185">Z tohoto důvodu se doporučuje nastavit webové aplikace hello příliš**Always On**.</span><span class="sxs-lookup"><span data-stu-id="39c85-185">For that reason, it is recommended that you set hello web app too**Always On**.</span></span>  

<span data-ttu-id="39c85-186">Pomocí Liferay 6.1.2, které dodávat Community Edition GA3 Tomcat, hello následující soubory nebyly upravit po stažení Liferay:</span><span class="sxs-lookup"><span data-stu-id="39c85-186">Using Liferay 6.1.2 Community Edition GA3 bundled with Tomcat, hello following files were edited after downloading Liferay:</span></span>

<span data-ttu-id="39c85-187">**Server.XML**</span><span class="sxs-lookup"><span data-stu-id="39c85-187">**Server.xml**</span></span>

* <span data-ttu-id="39c85-188">Změňte vypnutí portu příliš-1.</span><span class="sxs-lookup"><span data-stu-id="39c85-188">Change Shutdown port too-1.</span></span>
* <span data-ttu-id="39c85-189">Příliš změnit konektor HTTP`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span><span class="sxs-lookup"><span data-stu-id="39c85-189">Change HTTP connector too       `<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span></span>
* <span data-ttu-id="39c85-190">Komentář hello AJP konektor.</span><span class="sxs-lookup"><span data-stu-id="39c85-190">Comment out hello AJP connector.</span></span>

<span data-ttu-id="39c85-191">V hello **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** složky, vytvořte soubor s názvem **portál ext.properties**.</span><span class="sxs-lookup"><span data-stu-id="39c85-191">In hello **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** folder, create a file named **portal-ext.properties**.</span></span> <span data-ttu-id="39c85-192">Tento soubor vyžaduje toocontain jeden řádek, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="39c85-192">This file needs toocontain one line, as shown here:</span></span>

    liferay.home=%HOME%/site/wwwroot/liferay

<span data-ttu-id="39c85-193">V hello stejnou úroveň directory jako složka hello tomcat 7.0.40, vytvořte soubor s názvem **web.config** s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="39c85-193">At hello same directory level as hello tomcat-7.0.40 folder, create a file named **web.config** with hello following content:</span></span>

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
    <add name="httpPlatformHandler" path="*" verb="*"
         modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%HOME%\site\wwwroot\tomcat-7.0.40\bin\catalina.bat" 
                      arguments="run" 
                      startupTimeLimit="10" 
                      requestTimeout="00:10:00" 
                      stdoutLogEnabled="true">
          <environmentVariables>
      <environmentVariable name="CATALINA_OPTS" value="-Dport.http=%HTTP_PLATFORM_PORT%" />
      <environmentVariable name="CATALINA_HOME" value="%HOME%\site\wwwroot\tomcat-7.0.40" />
            <environmentVariable name="JRE_HOME" value="D:\Program Files\Java\jdk1.7.0_51" /> 
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

<span data-ttu-id="39c85-194">V části hello **httpPlatform** blok hello **requestTimeout** je nastaven příliš "00:10:00".</span><span class="sxs-lookup"><span data-stu-id="39c85-194">Under hello **httpPlatform** block, hello **requestTimeout** is set too“00:10:00”.</span></span>  <span data-ttu-id="39c85-195">Může snížit, ale pak budete pravděpodobně toosee, některé chyby vypršení časového limitu při Liferay je zavedení spouštěcího programu.</span><span class="sxs-lookup"><span data-stu-id="39c85-195">It can be reduced but then you are likely toosee some timeout errors while Liferay is bootstrapping.</span></span>  <span data-ttu-id="39c85-196">Pokud je tato hodnota změněna, pak hello **connectionTimeout** v hello tomcat by měl být změněn server.xml.</span><span class="sxs-lookup"><span data-stu-id="39c85-196">If this value is changed, then hello **connectionTimeout** in hello tomcat server.xml should also be modified.</span></span>  

<span data-ttu-id="39c85-197">Je vhodné poznamenat, že hello JRE_HOME lnstalování varariable je uveden v hello výše web.config toopoint toohello 64-bit JDK.</span><span class="sxs-lookup"><span data-stu-id="39c85-197">It is worth noting that hello JRE_HOME environnment varariable is specified in hello above web.config toopoint toohello 64-bit JDK.</span></span> <span data-ttu-id="39c85-198">Výchozí hodnota Hello je 32-bit, ale vzhledem k tomu, že Liferay může vyžadovat vysoké úrovně paměti, je doporučeno toouse hello 64-bit JDK.</span><span class="sxs-lookup"><span data-stu-id="39c85-198">hello default is 32-bit, but since Liferay may require high levels of memory, it is recommended toouse hello 64-bit JDK.</span></span>

<span data-ttu-id="39c85-199">Po provedení těchto změn, restartujte webovou aplikaci s Liferay pak otevřete http://yourwebapp.</span><span class="sxs-lookup"><span data-stu-id="39c85-199">Once you make these changes, restart your web app running Liferay, Then, open http://yourwebapp.</span></span> <span data-ttu-id="39c85-200">portál Liferay Hello je k dispozici z kořenového adresáře aplikace hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c85-200">hello Liferay portal is available from hello web app root.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="39c85-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39c85-201">Next steps</span></span>
<span data-ttu-id="39c85-202">Další informace o Liferay najdete v tématu [http://www.liferay.com](http://www.liferay.com).</span><span class="sxs-lookup"><span data-stu-id="39c85-202">For more information about Liferay, see [http://www.liferay.com](http://www.liferay.com).</span></span>

<span data-ttu-id="39c85-203">Další informace o Java najdete v článku [Azure pro vývojáře v jazyce Java](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="39c85-203">For more information about Java, visit [Azure for Java developers](/java/azure).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
