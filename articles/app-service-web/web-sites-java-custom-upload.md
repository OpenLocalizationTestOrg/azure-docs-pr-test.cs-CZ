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
# <a name="upload-a-custom-java-web-app-to-azure"></a>Nahrání vlastní webové aplikace v jazyce Java do Azure
Toto téma vysvětluje, jak nahrát vlastní webové aplikace Java do [Azure App Service] webové aplikace. Jsou zde zahrnuty informace, která platí pro všechny web nebo webovou aplikaci Java a také některé příklady pro konkrétní aplikace.

Všimněte si, že Azure poskytuje prostředky pro vytvoření webové aplikace v jazyce Java pomocí portálu Azure konfigurace uživatelského rozhraní a Azure Marketplace, jak je uvedeno v [vytvoření webové aplikace Java v Azure App Service](web-sites-java-get-started.md). Tento kurz je určen pro scénáře, ve kterých nechcete používat portál Azure konfigurace uživatelského rozhraní nebo v Azure Marketplace.  

## <a name="configuration-guidelines"></a>Pokyny pro konfigurace
Následující text popisuje nastavení pro vlastní webové aplikace v jazyce Java v Azure.

* Port HTTP používaný procesem Java je dynamicky přiřadit.  Tento proces musí používat port z proměnné prostředí `HTTP_PLATFORM_PORT`.
* Všechny porty naslouchání než jeden naslouchací proces protokolu HTTP by mělo být zakázáno.  V Tomcat, který obsahuje vypnutí, HTTPS a AJP porty.
* Kontejner je potřeba nakonfigurovat pro jenom přenosy protokolu IPv4.
* **Spuštění** příkaz pro aplikace musí být nastavena v konfiguraci.
* Aplikace, které vyžadují oprávnění zapisovat adresáře se musí nacházet ve službě Azure web app adresář s obsahem, který je **D:\home**.  Proměnnou prostředí `HOME` odkazuje na D:\home.  

Podle potřeby můžete nastavit proměnné prostředí v souboru web.config.

## <a name="webconfig-httpplatform-configuration"></a>soubor Web.config httpPlatform konfigurace
Následující informace popisují **httpPlatform** formátu v souboru web.config.

**argumenty** (výchozí = ""). Argumenty pro spustitelný soubor nebo skript zadaný v **processPath** nastavení.

Příklady (ukázka s **processPath** zahrnuté):

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


**processPath** -cestu k spustitelný soubor nebo skript, který se spustí proces naslouchání požadavkům HTTP.

Příklady:

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

**rapidFailsPerMinute** (výchozí = 10.) Proces zadaného v **processPath** je dovoleno havárií za minutu. Pokud je tento limit překročen, **HttpPlatformHandler** zastaví spuštění procesu pro zbytek minutu.

**requestTimeout** (výchozí = "00: 02:00".) Doba trvání, pro kterou **HttpPlatformHandler** bude čekat na odpověď z procesu naslouchání na portu `%HTTP_PLATFORM_PORT%`.

**startupRetryCount** (výchozí = 10.) Počet nezdařených pokusů o **HttpPlatformHandler** se pokusí spustit proces zadaný v **processPath**. V tématu **startupTimeLimit** další podrobnosti.

**startupTimeLimit** (výchozí = 10 sekund.) Doba trvání, pro kterou **HttpPlatformHandler** vyčká na spustitelný soubor nebo skript ke spuštění procesu naslouchání na portu.  Pokud je tento časový limit překročen, **HttpPlatformHandler** bude ukončit proces a pokuste se znovu spusťte **startupRetryCount** časy.

**stdoutLogEnabled** (výchozí = "true".) V případě hodnoty true **stdout** a **stderr** pro proces zadaný v **processPath** nastavení bude přesměrován na soubor zadaný v **stdoutLogFile** (najdete v části **stdoutLogFile** části).

**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Absolutní cesty, pro kterou **stdout** a **stderr** z procesu zadaný v **processPath** bude do protokolu.

> [!NOTE]
> `%HTTP_PLATFORM_PORT%`je speciální zástupný symbol, který se musí zadat buď jako součást **argumenty** nebo jako součást **httpPlatform** **environmentVariables** seznamu. To budou nahrazeny ve interně generovaného port **HttpPlatformHandler** tak, aby proces určeného **processPath** můžete naslouchat na tento port.
> 
> 

## <a name="deployment"></a>Nasazení
Java založené na webové aplikace můžou být nasazené snadno prostřednictvím většinu stejné znamená, že se používají s Internetové informační služby (IIS) na základě webové aplikace.  FTP, Git a Kudu jsou všechny podporované jako nasazení mechanismy, jako je integrované funkce SCM pro webové aplikace. Web Deploy funguje jako protokol, ale jako Java není vyvinuté v sadě Visual Studio, Web Deploy nevejde s případy použití pro nasazení Java webové aplikace.

## <a name="application-configuration-examples"></a>Konfigurace aplikace příklady
Konfigurace je pro tyto aplikace, soubor web.config a aplikace k dispozici jako příklady ukazují, jak povolit aplikace v jazyce Java v App Service Web Apps.

### <a name="tomcat"></a>Tomcat
Když existují dvě varianty na Tomcat, které se dodávají s App Service Web Apps, je stále poměrně možné nahrát určité instance zákazníka. Dole je příklad instalace Tomcat s různých Java Virtual Machine (JVM).

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

Na straně Tomcat existuje několik změny konfigurace, které je potřeba provést. Server.xml potřebuje k provádění úprav nastavení:

* Vypnutí portu = -1
* Konektor port HTTP = ${port.http}
* Adresa konektor HTTP = "127.0.0.1"
* Komentář HTTPS a AJP konektory
* Nastavení IPv4 lze nastavit i v catalina.properties souboru, ve kterém můžete přidat`java.net.preferIPv4Stack=true`

Direct3D – hovory nejsou podporovány v App Service Web Apps. Zakažte ty, přidejte následující možnost Java měli aplikace těchto volání:`-Dsun.java2d.d3d=false`

### <a name="jetty"></a>Jetty
Stejně jako v případě pro Tomcat, zákazníků můžete nahrát vlastní instance pro Jetty. V případě spuštěná úplná instalace Jetty, konfigurace bude vypadat takto:

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

Je potřeba změnit v start.ini nastavit konfiguraci Jetty `java.net.preferIPv4Stack=true`.

### <a name="springboot"></a>Springboot
Chcete-li získat Springboot aplikace s muset nahrát soubor JAR nebo WAR a přidejte následující soubor web.config. V souboru web.config přejde do složky wwwroot. V souboru web.config upravte argumenty tak, aby odkazovaly na váš soubor JAR v následujícím příkladu, které na soubor JAR je umístěný ve složce wwwroot.  

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
Naše testovací používá Hudsonem 3.1.2 war a Tomcat 7.0.50 výchozí instanci, ale bez věcí nastavit pomocí uživatelského rozhraní.  Hudsonem je nástroj pro sestavení softwaru, a proto se doporučuje nainstalovat na vyhrazené instance kde **AlwaysOn** příznak lze nastavit ve webové aplikaci.

1. Ve vaší webové aplikaci kořenový adresář, tedy **d:\home\site\wwwroot**, vytvoření **webapps** adresáře (Pokud již existuje) a umístěte Hudson.war v **d:\home\site\wwwroot\webapps**.
2. Stáhnout apache maven 3.0.5 (kompatibilní s Hudsonem) a umístěte jej v **d:\home\site\wwwroot**.
3. Vytvoření souboru web.config v **d:\home\site\wwwroot** a vložte následující obsah:
   
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
   
    Webové aplikace v tomto okamžiku může být restartován provést změny.  Připojte k http://yourwebapp/hudson zahájíte Hudsonem.
4. Po Hudsonem automaticky nakonfiguruje, byste měli vidět následující obrazovka:
   
    ![Hudsonem](./media/web-sites-java-custom-upload/hudson1.png)
5. Přístup ke stránce Hudsonem konfigurace: klikněte na tlačítko **spravovat Hudsonem**a potom klikněte na **nakonfigurujte systém**.
6. Nakonfigurujte sadu JDK, jak je uvedeno níže:
   
    ![Konfigurace Hudsonem](./media/web-sites-java-custom-upload/hudson2.png)
7. Konfigurace Maven, jak je uvedeno níže:
   
    ![Konfigurace maven](./media/web-sites-java-custom-upload/maven.png)
8. Uložte nastavení. Hudsonem by teď měly být konfigurována a připravena k použití.

Další informace o Hudsonem najdete v tématu [http://hudson-ci.org](http://hudson-ci.org).

### <a name="liferay"></a>Liferay
Liferay je podporována v App Service Web Apps. Vzhledem k tomu, že Liferay může vyžadovat velké paměti, webové aplikace je potřeba spustit na střední a velké vyhrazené pracovní, což může zajistit dostatek paměti. Liferay také trvá několik minut, aby bylo možné spustit. Z tohoto důvodu se doporučuje nastavit webové aplikace na **Always On**.  

Pomocí Liferay 6.1.2, které dodávat Community Edition GA3 Tomcat, se po stažení Liferay upravit následující soubory:

**Server.XML**

* Změňte port ukončení na hodnotu -1.
* Konektor změnu HTTP`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`
* Komentář AJP konektor.

V **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** složky, vytvořte soubor s názvem **portál ext.properties**. Tento soubor musí obsahovat jeden řádek, jak je vidět tady:

    liferay.home=%HOME%/site/wwwroot/liferay

Na stejné úrovni directory jako složka tomcat 7.0.40, vytvořte soubor s názvem **web.config** s následujícím obsahem:

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

V části **httpPlatform** bloku **requestTimeout** je nastaven na "00:10:00".  Může snížit, ale pak budete chtít nejspíš chcete zobrazit některé chyby vypršení časového limitu při Liferay je zavedení spouštěcího programu.  Pokud je tato hodnota změněna, pak se **connectionTimeout** v tomcat je také třeba upravit server.xml.  

Je vhodné poznamenat, že varariable lnstalování JRE_HOME je zadána v souboru web.config výše tak, aby odkazoval na 64-bit JDK. Výchozí hodnota je 32-bit, ale vzhledem k tomu, že Liferay může vyžadovat vysoké úrovně paměti, doporučuje se používat sadu JDK 64-bit.

Po provedení těchto změn, restartujte webovou aplikaci s Liferay pak otevřete http://yourwebapp. Portál Liferay je k dispozici z kořenového adresáře webové aplikace. 

## <a name="next-steps"></a>Další kroky
Další informace o Liferay najdete v tématu [http://www.liferay.com](http://www.liferay.com).

Další informace o Java najdete v článku [Azure pro vývojáře v jazyce Java](/java/azure).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
