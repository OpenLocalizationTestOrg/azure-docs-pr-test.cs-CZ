---
title: "Nahrání vlastní webové aplikace v jazyce Java do Azure"
description: "Tento kurz ukazuje, jak nahrát vlastní webové aplikace v jazyce Java do Azure App Service Web Apps."
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
ms.openlocfilehash: 9c8f9ee7780859f7640ac82d6ebce85082170ad7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-a-custom-java-web-app-to-azure"></a><span data-ttu-id="1f12a-103">Nahrání vlastní webové aplikace v jazyce Java do Azure</span><span class="sxs-lookup"><span data-stu-id="1f12a-103">Upload a custom Java web app to Azure</span></span>
<span data-ttu-id="1f12a-104">Toto téma vysvětluje, jak nahrát vlastní webové aplikace Java do [Azure App Service] webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f12a-104">This topic explains how to upload a custom Java web app to [Azure App Service] Web Apps.</span></span> <span data-ttu-id="1f12a-105">Jsou zde zahrnuty informace, která platí pro všechny web nebo webovou aplikaci Java a také některé příklady pro konkrétní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f12a-105">Included is information that applies to any Java website or web app, and also some examples for specific applications.</span></span>

<span data-ttu-id="1f12a-106">Všimněte si, že Azure poskytuje prostředky pro vytvoření webové aplikace v jazyce Java pomocí portálu Azure konfigurace uživatelského rozhraní a Azure Marketplace, jak je uvedeno v [vytvoření webové aplikace Java v Azure App Service](web-sites-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1f12a-106">Note that Azure provides a means for creating Java web apps using the Azure Portal's configuration UI, and the Azure Marketplace, as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md).</span></span> <span data-ttu-id="1f12a-107">Tento kurz je určen pro scénáře, ve kterých nechcete používat portál Azure konfigurace uživatelského rozhraní nebo v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1f12a-107">This tutorial is for scenarios in which you do not want to use the Azure Portal configuration UI or the Azure Marketplace.</span></span>  

## <a name="configuration-guidelines"></a><span data-ttu-id="1f12a-108">Pokyny pro konfigurace</span><span class="sxs-lookup"><span data-stu-id="1f12a-108">Configuration guidelines</span></span>
<span data-ttu-id="1f12a-109">Následující text popisuje nastavení pro vlastní webové aplikace v jazyce Java v Azure.</span><span class="sxs-lookup"><span data-stu-id="1f12a-109">The following describes the settings expected for custom Java web apps on Azure.</span></span>

* <span data-ttu-id="1f12a-110">Port HTTP používaný procesem Java je dynamicky přiřadit.</span><span class="sxs-lookup"><span data-stu-id="1f12a-110">The HTTP port used by the Java process is dynamically assigned.</span></span>  <span data-ttu-id="1f12a-111">Tento proces musí používat port z proměnné prostředí `HTTP_PLATFORM_PORT`.</span><span class="sxs-lookup"><span data-stu-id="1f12a-111">The process must use the port from the environment variable `HTTP_PLATFORM_PORT`.</span></span>
* <span data-ttu-id="1f12a-112">Všechny porty naslouchání než jeden naslouchací proces protokolu HTTP by mělo být zakázáno.</span><span class="sxs-lookup"><span data-stu-id="1f12a-112">All listen ports other than the single HTTP listener should be disabled.</span></span>  <span data-ttu-id="1f12a-113">V Tomcat, který obsahuje vypnutí, HTTPS a AJP porty.</span><span class="sxs-lookup"><span data-stu-id="1f12a-113">In Tomcat, that includes the shutdown, HTTPS, and AJP ports.</span></span>
* <span data-ttu-id="1f12a-114">Kontejner je potřeba nakonfigurovat pro jenom přenosy protokolu IPv4.</span><span class="sxs-lookup"><span data-stu-id="1f12a-114">The container needs to be configured for IPv4 traffic only.</span></span>
* <span data-ttu-id="1f12a-115">**Spuštění** příkaz pro aplikace musí být nastavena v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="1f12a-115">The **startup** command for the application needs to be set in the configuration.</span></span>
* <span data-ttu-id="1f12a-116">Aplikace, které vyžadují oprávnění zapisovat adresáře se musí nacházet ve službě Azure web app adresář s obsahem, který je **D:\home**.</span><span class="sxs-lookup"><span data-stu-id="1f12a-116">Applications that require directories with write permission need to be located in the Azure web app's content directory,  which is **D:\home**.</span></span>  <span data-ttu-id="1f12a-117">Proměnnou prostředí `HOME` odkazuje na D:\home.</span><span class="sxs-lookup"><span data-stu-id="1f12a-117">The environmental variable `HOME` refers to D:\home.</span></span>  

<span data-ttu-id="1f12a-118">Podle potřeby můžete nastavit proměnné prostředí v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="1f12a-118">You can set environment variables as required in the web.config file.</span></span>

## <a name="webconfig-httpplatform-configuration"></a><span data-ttu-id="1f12a-119">soubor Web.config httpPlatform konfigurace</span><span class="sxs-lookup"><span data-stu-id="1f12a-119">web.config httpPlatform configuration</span></span>
<span data-ttu-id="1f12a-120">Následující informace popisují **httpPlatform** formátu v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="1f12a-120">The following information describes the **httpPlatform** format within web.config.</span></span>

<span data-ttu-id="1f12a-121">**argumenty** (výchozí = "").</span><span class="sxs-lookup"><span data-stu-id="1f12a-121">**arguments** (Default="").</span></span> <span data-ttu-id="1f12a-122">Argumenty pro spustitelný soubor nebo skript zadaný v **processPath** nastavení.</span><span class="sxs-lookup"><span data-stu-id="1f12a-122">Arguments to the executable or script specified in the **processPath** setting.</span></span>

<span data-ttu-id="1f12a-123">Příklady (ukázka s **processPath** zahrnuté):</span><span class="sxs-lookup"><span data-stu-id="1f12a-123">Examples (shown with **processPath** included):</span></span>

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


<span data-ttu-id="1f12a-124">**processPath** -cestu k spustitelný soubor nebo skript, který se spustí proces naslouchání požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f12a-124">**processPath** - Path to the executable or script that will launch a process listening for HTTP requests.</span></span>

<span data-ttu-id="1f12a-125">Příklady:</span><span class="sxs-lookup"><span data-stu-id="1f12a-125">Examples:</span></span>

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

<span data-ttu-id="1f12a-126">**rapidFailsPerMinute** (výchozí = 10.) Proces zadaného v **processPath** je dovoleno havárií za minutu.</span><span class="sxs-lookup"><span data-stu-id="1f12a-126">**rapidFailsPerMinute** (Default=10.) Number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1f12a-127">Pokud je tento limit překročen, **HttpPlatformHandler** zastaví spuštění procesu pro zbytek minutu.</span><span class="sxs-lookup"><span data-stu-id="1f12a-127">If this limit is exceeded, **HttpPlatformHandler** will stop launching the process for the remainder of the minute.</span></span>

<span data-ttu-id="1f12a-128">**requestTimeout** (výchozí = "00: 02:00".) Doba trvání, pro kterou **HttpPlatformHandler** bude čekat na odpověď z procesu naslouchání na portu `%HTTP_PLATFORM_PORT%`.</span><span class="sxs-lookup"><span data-stu-id="1f12a-128">**requestTimeout** (Default="00:02:00".) Duration for which **HttpPlatformHandler** will wait for a response from the process listening on `%HTTP_PLATFORM_PORT%`.</span></span>

<span data-ttu-id="1f12a-129">**startupRetryCount** (výchozí = 10.) Počet nezdařených pokusů o **HttpPlatformHandler** se pokusí spustit proces zadaný v **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1f12a-129">**startupRetryCount** (Default=10.) Number of times **HttpPlatformHandler** will try to launch the process specified in **processPath**.</span></span> <span data-ttu-id="1f12a-130">V tématu **startupTimeLimit** další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1f12a-130">See **startupTimeLimit** for more details.</span></span>

<span data-ttu-id="1f12a-131">**startupTimeLimit** (výchozí = 10 sekund.) Doba trvání, pro kterou **HttpPlatformHandler** vyčká na spustitelný soubor nebo skript ke spuštění procesu naslouchání na portu.</span><span class="sxs-lookup"><span data-stu-id="1f12a-131">**startupTimeLimit** (Default=10 seconds.) Duration for which **HttpPlatformHandler** will wait for the executable/script to start a process listening on the port.</span></span>  <span data-ttu-id="1f12a-132">Pokud je tento časový limit překročen, **HttpPlatformHandler** bude ukončit proces a pokuste se znovu spusťte **startupRetryCount** časy.</span><span class="sxs-lookup"><span data-stu-id="1f12a-132">If this time limit is exceeded, **HttpPlatformHandler** will kill the process and try to launch it again **startupRetryCount** times.</span></span>

<span data-ttu-id="1f12a-133">**stdoutLogEnabled** (výchozí = "true".) V případě hodnoty true **stdout** a **stderr** pro proces zadaný v **processPath** nastavení bude přesměrován na soubor zadaný v **stdoutLogFile** (najdete v části **stdoutLogFile** části).</span><span class="sxs-lookup"><span data-stu-id="1f12a-133">**stdoutLogEnabled** (Default="true".) If true, **stdout** and **stderr** for the process specified in the **processPath** setting will be redirected to the file specified in **stdoutLogFile** (see **stdoutLogFile** section).</span></span>

<span data-ttu-id="1f12a-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Absolutní cesty, pro kterou **stdout** a **stderr** z procesu zadaný v **processPath** bude do protokolu.</span><span class="sxs-lookup"><span data-stu-id="1f12a-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Absolute file path for which **stdout** and **stderr** from the process specified in **processPath** will be logged.</span></span>

> [!NOTE]
> <span data-ttu-id="1f12a-135">`%HTTP_PLATFORM_PORT%`je speciální zástupný symbol, který se musí zadat buď jako součást **argumenty** nebo jako součást **httpPlatform** **environmentVariables** seznamu.</span><span class="sxs-lookup"><span data-stu-id="1f12a-135">`%HTTP_PLATFORM_PORT%` is a special placeholder which needs to specified either as part of **arguments** or as part of the **httpPlatform** **environmentVariables** list.</span></span> <span data-ttu-id="1f12a-136">To budou nahrazeny ve interně generovaného port **HttpPlatformHandler** tak, aby proces určeného **processPath** můžete naslouchat na tento port.</span><span class="sxs-lookup"><span data-stu-id="1f12a-136">This will be replaced by an internally generated port by **HttpPlatformHandler** so that the process specified by **processPath** can listen on this port.</span></span>
> 
> 

## <a name="deployment"></a><span data-ttu-id="1f12a-137">Nasazení</span><span class="sxs-lookup"><span data-stu-id="1f12a-137">Deployment</span></span>
<span data-ttu-id="1f12a-138">Java založené na webové aplikace můžou být nasazené snadno prostřednictvím většinu stejné znamená, že se používají s Internetové informační služby (IIS) na základě webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f12a-138">Java based web apps can be deployed easily through most of the same means that are used with the Internet Information Services (IIS) based web applications.</span></span>  <span data-ttu-id="1f12a-139">FTP, Git a Kudu jsou všechny podporované jako nasazení mechanismy, jako je integrované funkce SCM pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f12a-139">FTP, Git and Kudu are all supported as deployment mechanisms, as is the integrated SCM capability for web apps.</span></span> <span data-ttu-id="1f12a-140">Web Deploy funguje jako protokol, ale jako Java není vyvinuté v sadě Visual Studio, Web Deploy nevejde s případy použití pro nasazení Java webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f12a-140">WebDeploy works as a protocol, however, as Java is not developed in Visual Studio, WebDeploy does not fit with Java web app deployment use cases.</span></span>

## <a name="application-configuration-examples"></a><span data-ttu-id="1f12a-141">Konfigurace aplikace příklady</span><span class="sxs-lookup"><span data-stu-id="1f12a-141">Application configuration Examples</span></span>
<span data-ttu-id="1f12a-142">Konfigurace je pro tyto aplikace, soubor web.config a aplikace k dispozici jako příklady ukazují, jak povolit aplikace v jazyce Java v App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="1f12a-142">For the following applications, a web.config file and the application configuration is provided as examples to show how to enable your Java application on App Service Web Apps.</span></span>

### <a name="tomcat"></a><span data-ttu-id="1f12a-143">Tomcat</span><span class="sxs-lookup"><span data-stu-id="1f12a-143">Tomcat</span></span>
<span data-ttu-id="1f12a-144">Když existují dvě varianty na Tomcat, které se dodávají s App Service Web Apps, je stále poměrně možné nahrát určité instance zákazníka.</span><span class="sxs-lookup"><span data-stu-id="1f12a-144">While there are two variations on Tomcat that are supplied with App Service Web Apps, it is still quite possible to upload customer specific instances.</span></span> <span data-ttu-id="1f12a-145">Dole je příklad instalace Tomcat s různých Java Virtual Machine (JVM).</span><span class="sxs-lookup"><span data-stu-id="1f12a-145">Below is an example of an install of Tomcat with a different Java Virtual Machine (JVM).</span></span>

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
            <environmentVariable name="JRE_HOME" value="%HOME%\site\wwwroot\bin\java" /> <!-- optional, if not specified, this will default to %programfiles%\Java -->
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

<span data-ttu-id="1f12a-146">Na straně Tomcat existuje několik změny konfigurace, které je potřeba provést.</span><span class="sxs-lookup"><span data-stu-id="1f12a-146">On the Tomcat side, there are a few configuration changes that need to be made.</span></span> <span data-ttu-id="1f12a-147">Server.xml potřebuje k provádění úprav nastavení:</span><span class="sxs-lookup"><span data-stu-id="1f12a-147">The server.xml needs to be edited to set:</span></span>

* <span data-ttu-id="1f12a-148">Vypnutí portu = -1</span><span class="sxs-lookup"><span data-stu-id="1f12a-148">Shutdown port = -1</span></span>
* <span data-ttu-id="1f12a-149">Konektor port HTTP = ${port.http}</span><span class="sxs-lookup"><span data-stu-id="1f12a-149">HTTP connector port = ${port.http}</span></span>
* <span data-ttu-id="1f12a-150">Adresa konektor HTTP = "127.0.0.1"</span><span class="sxs-lookup"><span data-stu-id="1f12a-150">HTTP connector address = "127.0.0.1"</span></span>
* <span data-ttu-id="1f12a-151">Komentář HTTPS a AJP konektory</span><span class="sxs-lookup"><span data-stu-id="1f12a-151">Comment out HTTPS and AJP connectors</span></span>
* <span data-ttu-id="1f12a-152">Nastavení IPv4 lze nastavit i v catalina.properties souboru, ve kterém můžete přidat`java.net.preferIPv4Stack=true`</span><span class="sxs-lookup"><span data-stu-id="1f12a-152">The IPv4 setting can also be set in the catalina.properties file where you can add     `java.net.preferIPv4Stack=true`</span></span>

<span data-ttu-id="1f12a-153">Direct3D – hovory nejsou podporovány v App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="1f12a-153">Direct3d calls are not supported on App Service Web Apps.</span></span> <span data-ttu-id="1f12a-154">Zakažte ty, přidejte následující možnost Java měli aplikace těchto volání:`-Dsun.java2d.d3d=false`</span><span class="sxs-lookup"><span data-stu-id="1f12a-154">To disable those, add the following Java option should your application make such calls: `-Dsun.java2d.d3d=false`</span></span>

### <a name="jetty"></a><span data-ttu-id="1f12a-155">Jetty</span><span class="sxs-lookup"><span data-stu-id="1f12a-155">Jetty</span></span>
<span data-ttu-id="1f12a-156">Stejně jako v případě pro Tomcat, zákazníků můžete nahrát vlastní instance pro Jetty.</span><span class="sxs-lookup"><span data-stu-id="1f12a-156">As is the case for Tomcat, customers can upload their own instances for Jetty.</span></span> <span data-ttu-id="1f12a-157">V případě spuštěná úplná instalace Jetty, konfigurace bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="1f12a-157">In the case of running the full install of Jetty, the configuration would look like this:</span></span>

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

<span data-ttu-id="1f12a-158">Je potřeba změnit v start.ini nastavit konfiguraci Jetty `java.net.preferIPv4Stack=true`.</span><span class="sxs-lookup"><span data-stu-id="1f12a-158">The Jetty configuration needs to be changed in the start.ini to set `java.net.preferIPv4Stack=true`.</span></span>

### <a name="springboot"></a><span data-ttu-id="1f12a-159">Springboot</span><span class="sxs-lookup"><span data-stu-id="1f12a-159">Springboot</span></span>
<span data-ttu-id="1f12a-160">Chcete-li získat Springboot aplikace s muset nahrát soubor JAR nebo WAR a přidejte následující soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="1f12a-160">In order to get a Springboot application running you need to upload your JAR or WAR file and add the following web.config file.</span></span> <span data-ttu-id="1f12a-161">V souboru web.config přejde do složky wwwroot.</span><span class="sxs-lookup"><span data-stu-id="1f12a-161">The web.config file goes into the wwwroot folder.</span></span> <span data-ttu-id="1f12a-162">V souboru web.config upravte argumenty tak, aby odkazovaly na váš soubor JAR v následujícím příkladu, které na soubor JAR je umístěný ve složce wwwroot.</span><span class="sxs-lookup"><span data-stu-id="1f12a-162">In the web.config adjust the arguments to point to your JAR file, in the following example the JAR file is located in the wwwroot folder as well.</span></span>  

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


### <a name="hudson"></a><span data-ttu-id="1f12a-163">Hudsonem</span><span class="sxs-lookup"><span data-stu-id="1f12a-163">Hudson</span></span>
<span data-ttu-id="1f12a-164">Naše testovací používá Hudsonem 3.1.2 war a Tomcat 7.0.50 výchozí instanci, ale bez věcí nastavit pomocí uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1f12a-164">Our test used the Hudson 3.1.2 war and the default Tomcat 7.0.50 instance but without using the UI to set things up.</span></span>  <span data-ttu-id="1f12a-165">Hudsonem je nástroj pro sestavení softwaru, a proto se doporučuje nainstalovat na vyhrazené instance kde **AlwaysOn** příznak lze nastavit ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1f12a-165">Because Hudson is a software build tool, it is advised to install it on dedicated instances where the **AlwaysOn** flag can be set on the web app.</span></span>

1. <span data-ttu-id="1f12a-166">Ve vaší webové aplikaci kořenový adresář, tedy **d:\home\site\wwwroot**, vytvoření **webapps** adresáře (Pokud již existuje) a umístěte Hudson.war v **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="1f12a-166">In your web app’s root directory, i.e., **d:\home\site\wwwroot**, create a **webapps** directory (if not already present), and place Hudson.war in **d:\home\site\wwwroot\webapps**.</span></span>
2. <span data-ttu-id="1f12a-167">Stáhnout apache maven 3.0.5 (kompatibilní s Hudsonem) a umístěte jej v **d:\home\site\wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="1f12a-167">Download apache maven 3.0.5 (compatible with Hudson) and place it in **d:\home\site\wwwroot**.</span></span>
3. <span data-ttu-id="1f12a-168">Vytvoření souboru web.config v **d:\home\site\wwwroot** a vložte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="1f12a-168">Create web.config in **d:\home\site\wwwroot** and paste the following contents in it:</span></span>
   
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
   
    <span data-ttu-id="1f12a-169">Webové aplikace v tomto okamžiku může být restartován provést změny.</span><span class="sxs-lookup"><span data-stu-id="1f12a-169">At this point the web app can be restarted to take the changes.</span></span>  <span data-ttu-id="1f12a-170">Připojte k http://yourwebapp/hudson zahájíte Hudsonem.</span><span class="sxs-lookup"><span data-stu-id="1f12a-170">Connect to http://yourwebapp/hudson to start Hudson.</span></span>
4. <span data-ttu-id="1f12a-171">Po Hudsonem automaticky nakonfiguruje, byste měli vidět následující obrazovka:</span><span class="sxs-lookup"><span data-stu-id="1f12a-171">After Hudson configures itself, you should see the following screen:</span></span>
   
    ![Hudsonem](./media/web-sites-java-custom-upload/hudson1.png)
5. <span data-ttu-id="1f12a-173">Přístup ke stránce Hudsonem konfigurace: klikněte na tlačítko **spravovat Hudsonem**a potom klikněte na **nakonfigurujte systém**.</span><span class="sxs-lookup"><span data-stu-id="1f12a-173">Access the Hudson configuration page: Click **Manage Hudson**, and then click **Configure System**.</span></span>
6. <span data-ttu-id="1f12a-174">Nakonfigurujte sadu JDK, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="1f12a-174">Configure the JDK as shown below:</span></span>
   
    ![Konfigurace Hudsonem](./media/web-sites-java-custom-upload/hudson2.png)
7. <span data-ttu-id="1f12a-176">Konfigurace Maven, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="1f12a-176">Configure Maven as shown below:</span></span>
   
    ![Konfigurace maven](./media/web-sites-java-custom-upload/maven.png)
8. <span data-ttu-id="1f12a-178">Uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="1f12a-178">Save the settings.</span></span> <span data-ttu-id="1f12a-179">Hudsonem by teď měly být konfigurována a připravena k použití.</span><span class="sxs-lookup"><span data-stu-id="1f12a-179">Hudson should now be configured and ready for use.</span></span>

<span data-ttu-id="1f12a-180">Další informace o Hudsonem najdete v tématu [http://hudson-ci.org](http://hudson-ci.org).</span><span class="sxs-lookup"><span data-stu-id="1f12a-180">For additional information on Hudson, see [http://hudson-ci.org](http://hudson-ci.org).</span></span>

### <a name="liferay"></a><span data-ttu-id="1f12a-181">Liferay</span><span class="sxs-lookup"><span data-stu-id="1f12a-181">Liferay</span></span>
<span data-ttu-id="1f12a-182">Liferay je podporována v App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="1f12a-182">Liferay is supported on App Service Web Apps.</span></span> <span data-ttu-id="1f12a-183">Vzhledem k tomu, že Liferay může vyžadovat velké paměti, webové aplikace je potřeba spustit na střední a velké vyhrazené pracovní, což může zajistit dostatek paměti.</span><span class="sxs-lookup"><span data-stu-id="1f12a-183">Since Liferay can require significant memory, the web app needs to run on a medium or large dedicated worker, which can provide enough memory.</span></span> <span data-ttu-id="1f12a-184">Liferay také trvá několik minut, aby bylo možné spustit.</span><span class="sxs-lookup"><span data-stu-id="1f12a-184">Liferay also takes several minutes to start up.</span></span> <span data-ttu-id="1f12a-185">Z tohoto důvodu se doporučuje nastavit webové aplikace na **Always On**.</span><span class="sxs-lookup"><span data-stu-id="1f12a-185">For that reason, it is recommended that you set the web app to **Always On**.</span></span>  

<span data-ttu-id="1f12a-186">Pomocí Liferay 6.1.2, které dodávat Community Edition GA3 Tomcat, se po stažení Liferay upravit následující soubory:</span><span class="sxs-lookup"><span data-stu-id="1f12a-186">Using Liferay 6.1.2 Community Edition GA3 bundled with Tomcat, the following files were edited after downloading Liferay:</span></span>

<span data-ttu-id="1f12a-187">**Server.XML**</span><span class="sxs-lookup"><span data-stu-id="1f12a-187">**Server.xml**</span></span>

* <span data-ttu-id="1f12a-188">Změňte port ukončení na hodnotu -1.</span><span class="sxs-lookup"><span data-stu-id="1f12a-188">Change Shutdown port to -1.</span></span>
* <span data-ttu-id="1f12a-189">Konektor změnu HTTP`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span><span class="sxs-lookup"><span data-stu-id="1f12a-189">Change HTTP connector to       `<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span></span>
* <span data-ttu-id="1f12a-190">Komentář AJP konektor.</span><span class="sxs-lookup"><span data-stu-id="1f12a-190">Comment out the AJP connector.</span></span>

<span data-ttu-id="1f12a-191">V **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** složky, vytvořte soubor s názvem **portál ext.properties**.</span><span class="sxs-lookup"><span data-stu-id="1f12a-191">In the **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** folder, create a file named **portal-ext.properties**.</span></span> <span data-ttu-id="1f12a-192">Tento soubor musí obsahovat jeden řádek, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="1f12a-192">This file needs to contain one line, as shown here:</span></span>

    liferay.home=%HOME%/site/wwwroot/liferay

<span data-ttu-id="1f12a-193">Na stejné úrovni directory jako složka tomcat 7.0.40, vytvořte soubor s názvem **web.config** s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="1f12a-193">At the same directory level as the tomcat-7.0.40 folder, create a file named **web.config** with the following content:</span></span>

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

<span data-ttu-id="1f12a-194">V části **httpPlatform** bloku **requestTimeout** je nastaven na "00:10:00".</span><span class="sxs-lookup"><span data-stu-id="1f12a-194">Under the **httpPlatform** block, the **requestTimeout** is set to “00:10:00”.</span></span>  <span data-ttu-id="1f12a-195">Může snížit, ale pak budete chtít nejspíš chcete zobrazit některé chyby vypršení časového limitu při Liferay je zavedení spouštěcího programu.</span><span class="sxs-lookup"><span data-stu-id="1f12a-195">It can be reduced but then you are likely to see some timeout errors while Liferay is bootstrapping.</span></span>  <span data-ttu-id="1f12a-196">Pokud je tato hodnota změněna, pak se **connectionTimeout** v tomcat je také třeba upravit server.xml.</span><span class="sxs-lookup"><span data-stu-id="1f12a-196">If this value is changed, then the **connectionTimeout** in the tomcat server.xml should also be modified.</span></span>  

<span data-ttu-id="1f12a-197">Je vhodné poznamenat, že varariable lnstalování JRE_HOME je zadána v souboru web.config výše tak, aby odkazoval na 64-bit JDK.</span><span class="sxs-lookup"><span data-stu-id="1f12a-197">It is worth noting that the JRE_HOME environnment varariable is specified in the above web.config to point to the 64-bit JDK.</span></span> <span data-ttu-id="1f12a-198">Výchozí hodnota je 32-bit, ale vzhledem k tomu, že Liferay může vyžadovat vysoké úrovně paměti, doporučuje se používat sadu JDK 64-bit.</span><span class="sxs-lookup"><span data-stu-id="1f12a-198">The default is 32-bit, but since Liferay may require high levels of memory, it is recommended to use the 64-bit JDK.</span></span>

<span data-ttu-id="1f12a-199">Po provedení těchto změn, restartujte webovou aplikaci s Liferay pak otevřete http://yourwebapp.</span><span class="sxs-lookup"><span data-stu-id="1f12a-199">Once you make these changes, restart your web app running Liferay, Then, open http://yourwebapp.</span></span> <span data-ttu-id="1f12a-200">Portál Liferay je k dispozici z kořenového adresáře webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f12a-200">The Liferay portal is available from the web app root.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1f12a-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f12a-201">Next steps</span></span>
<span data-ttu-id="1f12a-202">Další informace o Liferay najdete v tématu [http://www.liferay.com](http://www.liferay.com).</span><span class="sxs-lookup"><span data-stu-id="1f12a-202">For more information about Liferay, see [http://www.liferay.com](http://www.liferay.com).</span></span>

<span data-ttu-id="1f12a-203">Další informace o Java najdete v článku [Azure pro vývojáře v jazyce Java](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="1f12a-203">For more information about Java, visit [Azure for Java developers](/java/azure).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
