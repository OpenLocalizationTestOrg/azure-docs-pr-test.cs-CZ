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
# <a name="upload-a-custom-java-web-app-tooazure"></a>Nahrát vlastní tooAzure webové aplikace Java
Toto téma vysvětluje, jak tooupload vlastní Java webová aplikace příliš[Azure App Service] webové aplikace. Jsou zde zahrnuty informace, která se použije tooany Java web nebo webovou aplikaci a také některé příklady pro konkrétní aplikace.

Všimněte si, že Azure poskytuje prostředky pro vytvoření webové aplikace Java pomocí portálu Azure hello konfigurace uživatelského rozhraní a hello Azure Marketplace, jak je uvedeno v [vytvoření webové aplikace Java v Azure App Service](web-sites-java-get-started.md). Tento kurz je určen pro scénáře, ve kterých není má uživatelské rozhraní konfigurace portálu Azure hello toouse nebo hello Azure Marketplace.  

## <a name="configuration-guidelines"></a>Pokyny pro konfigurace
Hello následující text popisuje hello nastavení pro vlastní webové aplikace v jazyce Java v Azure.

* port HTTP Hello používá hello procesu Java je dynamicky přiřadit.  proces Hello musí používat port hello z proměnné prostředí hello `HTTP_PLATFORM_PORT`.
* Všechny naslouchat porty jiného, než by mělo být zakázáno hello jeden naslouchací proces protokolu HTTP.  V Tomcat, který obsahuje hello vypnutí, HTTPS a AJP porty.
* kontejner Hello musí toobe nakonfigurované pro jenom přenosy protokolu IPv4.
* Hello **spuštění** příkaz pro aplikace hello musí toobe nastavit v konfiguraci hello.
* Aplikace, které vyžadují oprávnění zapisovat adresáře s potřebovat toobe umístěný v adresáři obsahu hello Azure webovou aplikaci, která je **D:\home**.  proměnné prostředí Hello `HOME` odkazuje tooD:\home.  

Podle potřeby můžete nastavit proměnné prostředí v souboru web.config hello.

## <a name="webconfig-httpplatform-configuration"></a>soubor Web.config httpPlatform konfigurace
Hello následující informace popisují hello **httpPlatform** formátu v souboru web.config.

**argumenty** (výchozí = ""). Argumenty toohello spustitelný soubor nebo skript zadaný v hello **processPath** nastavení.

Příklady (ukázka s **processPath** zahrnuté):

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


**processPath** -cesta toohello spustitelný soubor nebo skript, který se spustí proces naslouchání požadavkům HTTP.

Příklady:

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

**rapidFailsPerMinute** (výchozí = 10.) Počet opakování, proces hello zadaný v **processPath** je povoleno toocrash za minutu. Pokud je tento limit překročen, **HttpPlatformHandler** zastaví spuštění procesu hello hello zbývající hello minuta.

**requestTimeout** (výchozí = "00: 02:00".) Doba trvání, pro kterou **HttpPlatformHandler** bude čekat na odpověď od hello proces naslouchání na portu `%HTTP_PLATFORM_PORT%`.

**startupRetryCount** (výchozí = 10.) Počet nezdařených pokusů o **HttpPlatformHandler** pokusí toolaunch hello proces zadaný v **processPath**. V tématu **startupTimeLimit** další podrobnosti.

**startupTimeLimit** (výchozí = 10 sekund.) Doba trvání, pro kterou **HttpPlatformHandler** bude čekat hello spustitelný soubor nebo skript toostart proces naslouchání na portu hello.  Pokud je tento časový limit překročen, **HttpPlatformHandler** bude ukončit proces hello a zkuste to toolaunch ho znovu **startupRetryCount** časy.

**stdoutLogEnabled** (výchozí = "true".) V případě hodnoty true **stdout** a **stderr** pro proces hello zadaný v hello **processPath** bude nastavení přesměrovaného toohello soubor zadaný v  **stdoutLogFile** (viz **stdoutLogFile** části).

**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Absolutní cesty, pro kterou **stdout** a **stderr** z procesu hello zadaný v **processPath** bude do protokolu.

> [!NOTE]
> `%HTTP_PLATFORM_PORT%`je speciální zástupný symbol, který se musí buď jako součást toospecified **argumenty** nebo jako součást hello **httpPlatform** **environmentVariables** seznamu. To budou nahrazeny ve interně generovaného port **HttpPlatformHandler** tak, aby proces hello určeného **processPath** můžete naslouchat na tento port.
> 
> 

## <a name="deployment"></a>Nasazení
Webové aplikace Java na základě můžou být nasazené snadno prostřednictvím většinu hello stejné znamená, které se používají s hello Internetové informační služby (IIS) na základě webové aplikace.  FTP, Git a Kudu jsou všechny podporované jako nasazení mechanismy, jako je hello integrované schopnosti SCM pro webové aplikace. Web Deploy funguje jako protokol, ale jako Java není vyvinuté v sadě Visual Studio, Web Deploy nevejde s případy použití pro nasazení Java webové aplikace.

## <a name="application-configuration-examples"></a>Konfigurace aplikace příklady
Pro následující aplikace, soubor web.config a hello hello konfigurace aplikace je k dispozici jako příklady tooshow jak tooenable aplikace v jazyce Java v App Service Web Apps.

### <a name="tomcat"></a>Tomcat
Existují dvě varianty na Tomcat, které se dodávají s App Service Web Apps, je stále poměrně možné tooupload zákazníka určité instance. Dole je příklad instalace Tomcat s různých Java Virtual Machine (JVM).

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

U hello Tomcat straně se několik změny konfigurace, které je třeba toobe provedeny. Hello server.xml potřebuje tooset toobe upravit:

* Vypnutí portu = -1
* Konektor port HTTP = ${port.http}
* Adresa konektor HTTP = "127.0.0.1"
* Komentář HTTPS a AJP konektory
* nastavení IPv4 Hello lze nastavit i v souboru catalina.properties hello, kde můžete přidat`java.net.preferIPv4Stack=true`

Direct3D – hovory nejsou podporovány v App Service Web Apps. toodisable ty, přidejte následující možnost Java měli aplikace těchto volání hello:`-Dsun.java2d.d3d=false`

### <a name="jetty"></a>Jetty
Stejně jako hello případě pro Tomcat, zákazníků můžete nahrát vlastní instance pro Jetty. V případě hello spuštěné hello úplné instalace Jetty hello konfigurace bude vypadat takto:

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

Hello Jetty je konfiguraci změnit v hello start.ini tooset toobe `java.net.preferIPv4Stack=true`.

### <a name="springboot"></a>Springboot
Aplikace s v pořadí tooget Springboot potřebovat tooupload váš soubor JAR nebo WAR a přidejte hello následující soubor web.config. soubor web.config Hello přejde do složky wwwroot hello. V souboru web.config hello upravit soubor JAR hello argumenty toopoint tooyour, v hello následující soubor JAR hello příklad nachází ve složce aplikace hello wwwroot také.  

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


### <a name="hudson"></a>Hudsonem
Naše testovací používá hello Hudsonem 3.1.2 war a hello výchozí Tomcat 7.0.50 instanci, ale bez použití hello uživatelského rozhraní tooset věcí.  Vzhledem k tomu Hudsonem je nástroj sestavení, je doporučené tooinstall ho na vyhrazené instance, kde hello **AlwaysOn** příznak lze nastavit na hello webové aplikace.

1. Ve vaší webové aplikaci kořenový adresář, tedy **d:\home\site\wwwroot**, vytvoření **webapps** adresáře (Pokud již existuje) a umístěte Hudson.war v **d:\home\site\wwwroot\webapps**.
2. Stáhnout apache maven 3.0.5 (kompatibilní s Hudsonem) a umístěte jej v **d:\home\site\wwwroot**.
3. Vytvoření souboru web.config v **d:\home\site\wwwroot** a hello vložte následující obsah v něm:
   
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
   
    Hello webové aplikace v tomto okamžiku může být restartovat tootake hello změny.  Připojte toohttp://yourwebapp/hudson toostart Hudsonem.
4. Po Hudsonem automaticky nakonfiguruje, měli byste vidět hello následující obrazovka:
   
    ![Hudsonem](./media/web-sites-java-custom-upload/hudson1.png)
5. Přístup hello stránku konfigurace Hudsonem: klikněte na tlačítko **spravovat Hudsonem**a potom klikněte na **nakonfigurujte systém**.
6. Nakonfigurujte hello JDK, jak je uvedeno níže:
   
    ![Konfigurace Hudsonem](./media/web-sites-java-custom-upload/hudson2.png)
7. Konfigurace Maven, jak je uvedeno níže:
   
    ![Konfigurace maven](./media/web-sites-java-custom-upload/maven.png)
8. Uložte nastavení hello. Hudsonem by teď měly být konfigurována a připravena k použití.

Další informace o Hudsonem najdete v tématu [http://hudson-ci.org](http://hudson-ci.org).

### <a name="liferay"></a>Liferay
Liferay je podporována v App Service Web Apps. Vzhledem k tomu, že Liferay může vyžadovat velké paměti, hello webová aplikace musí toorun na střední a velké vyhrazené pracovní, což může zajistit dostatek paměti. Liferay také trvá několik minut toostart. Z tohoto důvodu se doporučuje nastavit webové aplikace hello příliš**Always On**.  

Pomocí Liferay 6.1.2, které dodávat Community Edition GA3 Tomcat, hello následující soubory nebyly upravit po stažení Liferay:

**Server.XML**

* Změňte vypnutí portu příliš-1.
* Příliš změnit konektor HTTP`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`
* Komentář hello AJP konektor.

V hello **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** složky, vytvořte soubor s názvem **portál ext.properties**. Tento soubor vyžaduje toocontain jeden řádek, jak je vidět tady:

    liferay.home=%HOME%/site/wwwroot/liferay

V hello stejnou úroveň directory jako složka hello tomcat 7.0.40, vytvořte soubor s názvem **web.config** s hello následující obsah:

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

V části hello **httpPlatform** blok hello **requestTimeout** je nastaven příliš "00:10:00".  Může snížit, ale pak budete pravděpodobně toosee, některé chyby vypršení časového limitu při Liferay je zavedení spouštěcího programu.  Pokud je tato hodnota změněna, pak hello **connectionTimeout** v hello tomcat by měl být změněn server.xml.  

Je vhodné poznamenat, že hello JRE_HOME lnstalování varariable je uveden v hello výše web.config toopoint toohello 64-bit JDK. Výchozí hodnota Hello je 32-bit, ale vzhledem k tomu, že Liferay může vyžadovat vysoké úrovně paměti, je doporučeno toouse hello 64-bit JDK.

Po provedení těchto změn, restartujte webovou aplikaci s Liferay pak otevřete http://yourwebapp. portál Liferay Hello je k dispozici z kořenového adresáře aplikace hello webové aplikace. 

## <a name="next-steps"></a>Další kroky
Další informace o Liferay najdete v tématu [http://www.liferay.com](http://www.liferay.com).

Další informace o Java najdete v článku [Azure pro vývojáře v jazyce Java](/java/azure).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
