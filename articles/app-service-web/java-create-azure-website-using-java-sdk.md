---
title: "aaaCreate webové aplikace ve službě Azure App Service pomocí hello Azure SDK pro jazyk Java"
description: "Zjistěte, jak hello toocreate webové aplikace v Azure App Service programově pomocí sady Azure SDK pro jazyk Java."
tags: azure-classic-portal
services: app-service-web
documentationcenter: Java
author: donntrenton
manager: erikre
editor: jimbe
ms.assetid: 8954c456-1275-4d57-aff4-ca7d6374b71e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/25/2016
ms.author: v-donntr
ms.openlocfilehash: 42ba86b7fbb5668b3675198d0c5bb454525f706b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-hello-azure-sdk-for-java"></a>Vytvoření webové aplikace v Azure App Service pomocí hello Azure SDK pro jazyk Java
<!-- Azure Active Directory workflow is not yet available on hello Azure Portal -->

## <a name="overview"></a>Přehled
Tento návod ukazuje, jak toocreate sadu Azure SDK pro aplikace Java, která vytvoří webovou aplikaci v [Azure App Service][Azure App Service], pak nasazení tooit aplikace. Skládá se ze dvou částí:

* Část 1 ukazuje, jak toobuild aplikaci Java, vytvoří webovou aplikaci.
* Část 2 ukazuje, jak toocreate jednoduché JSP "Hello World" aplikace a pak použijte k serveru FTP klienta toodeploy kód tooApp služby.

## <a name="prerequisites"></a>Požadavky
### <a name="software-installations"></a>Instalace softwaru
Hello AzureWebDemo kódu aplikace v tomto článku byla zapsána pomocí sady Azure Java SDK 0.7.0, které můžete nainstalovat pomocí hello [instalačního programu webové platformy] [ Web Platform Installer] (WebPI). Kromě toho, ujistěte se, že toouse hello nejnovější verzi hello [nástrojů Azure pro Eclipse][Azure Toolkit for Eclipse]. Po instalaci hello SDK, aktualizace hello závislosti ve vašem projektu Eclipse spuštěním **aktualizovat Index** v **Maven úložiště**, pak znovu přidejte hello nejnovější verze jednotlivých balíčků v hello  **Závislosti** okno. Hello verzi nainstalovaného softwaru v prostředí Eclipse můžete ověřit kliknutím **pomoci > podrobné informace o instalaci**; můžete by měl mít aspoň hello následující verze:

* Balíček pro knihovny Microsoft Azure Libraries for Java 0.7.0.20150309
* Integrované vývojové prostředí pro vývojáře v jazyce Java EE 4.4.2.20150219 Eclipse

### <a name="create-and-configure-cloud-resources-in-azure"></a>Vytvoření a konfigurace cloudové prostředky v Azure
Před zahájením tohoto postupu, potřebujete toohave aktivní předplatné Azure a nastavit výchozí Active Directory (AD) v Azure.

### <a name="create-an-active-directory-ad-in-azure"></a>Vytvořte v Azure Active Directory (AD)
Pokud jste již nemají Active Directory (AD) na vaše předplatné Azure, přihlaste se k hello [portál Azure classic] [ Azure classic portal] s vaším účtem Microsoft. Pokud máte více předplatných, klikněte na tlačítko **odběry** a vyberte hello výchozí adresář pro předplatné hello chcete toouse pro tento projekt. Pak klikněte na tlačítko **použít** zobrazení odběru toothat tooswitch.

1. Vyberte **služby Active Directory** hello nabídce na levé straně. **Klikněte na tlačítko Nový > adresář > vytvořit vlastní**.
2. V **přidat adresář**, vyberte **vytvořte nový adresář**.
3. V **název**, zadejte název adresáře.
4. V **domény**, zadejte název domény. Toto je název základní domény, který je zahrnut ve výchozím nastavení s adresářem; má hello formuláře `<domain_name>.onmicrosoft.com`. Můžete pojmenovat podle názvu adresáře hello nebo jiný název domény, který vlastníte. Později můžete přidat jiný název domény, který vaše organizace už používá.
5. V **zemi nebo oblast**, vyberte národní prostředí.

Další informace o AD, najdete v části [co je adresář Azure AD][What is an Azure AD directory]?

### <a name="create-a-management-certificate-for-azure"></a>Vytvoření certifikátu správy pro Azure.
Hello Azure SDK pro jazyk Java používá tooauthenticate certifikáty správy s předplatným Azure. Toto jsou certifikáty X.509 v3, že používáte tooauthenticate klientská aplikace, která používá tooact hello Service Management API jménem prostředky předplatného toomanage vlastníka předplatného hello.

Hello kód v tomto postupu používá certifikát podepsaný svým držitelem tooauthenticate s Azure. Pro tento postup potřebujete toocreate certifikát a nahrajte ho toohello [portál Azure classic] [ Azure classic portal] předem. To zahrnuje hello následující kroky:

* Generování souboru PFX představující klientský certifikát a uložte ho místně.
* Vygenerujte certifikát správy (soubor CER) ze souboru PFX hello.
* Nahrajte tooyour soubor CER hello předplatného Azure.
* Soubor PFX hello převeďte JKS, protože Java používá tento formát tooauthenticate pomocí certifikátů.
* Zápis aplikace hello ověřovací kód, který odkazuje toohello místního JKS souboru.

Po dokončení tohoto postupu hello CER certifikát se bude nacházet ve vašem předplatném Azure a certifikát JKS hello se bude nacházet na místním disku. Další informace o certifikáty pro správu najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure][Create and Upload a Management Certificate for Azure].

#### <a name="create-a-certificate"></a>Vytvoření certifikátu
toocreate vlastní certifikátu podepsaného svým držitelem, otevřete konzolu příkaz v operačním systému a spusťte následující příkazy hello.

> **Poznámka:** hello počítače, na kterém spustíte tento příkaz musí mít hello JDK nainstalována. Hello cesta toohello keytool navíc závisí na hello umístění, ve kterém nainstalujete hello JDK. Další informace najdete v tématu [klíč a nástroj pro správu certifikát (keytool)] [ Key and Certificate Management Tool (keytool)] v online dokumentaci Java hello.
> 
> 

soubor .pfx toocreate hello:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

soubor .cer toocreate hello:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

Kde:

* `<java-install-dir>`je hello cesta toohello adresář, do které jste nainstalovali Java.
* `<keystore-id>`je identifikátor položky hello úložiště klíčů (například `AzureRemoteAccess`).
* `<cert-store-dir>`je hello cesta toohello adresář, ve kterém chcete toostore certifikáty (například `C:/Certificates`).
* `<cert-file-name>`je hello název souboru certifikátu hello (například `AzureWebDemoCert`).
* `<password>`je heslo hello zvolíte certifikát hello tooprotect; musí být alespoň 6 znaků. Žádné heslo, můžete zadat, i když se to nedoporučuje.
* `<dname>`hello toobe X.500 rozlišující název přidružené alias a slouží jako hello vystavitele a pole předmětu v certifikátu podepsaného svým držitelem hello.

Další informace najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure][Create and Upload a Management Certificate for Azure].

#### <a name="upload-hello-certificate"></a>Nahrát na server certifikát hello
tooupload tooAzure certifikát podepsaný svým držitelem přejděte toohello **nastavení** stránky portálu classic hello a pak klikněte na hello **certifikáty pro správu** kartě. Klikněte na tlačítko **nahrát** dole hello hello stránky a přejděte toohello umístění souboru CER hello jste vytvořili.

#### <a name="convert-hello-pfx-file-into-jks"></a>Převést soubor PFX hello JKS
V hello příkazového řádku systému Windows (spuštění jako správce), disk cd toohello adresář obsahující hello certifikáty a spusťte hello následující příkaz, kde `<java-install-dir>` je hello adresář, do které jste nainstalovali Java ve vašem počítači:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Po zobrazení výzvy zadejte heslo úložiště klíčů cílové hello; bude jím hello heslo pro soubor JKS hello.
2. Po zobrazení výzvy zadejte heslo úložiště klíčů zdroj hello; Toto je hello heslo, které jste zadali pro soubor PFX hello.

dvě hesla Hello nemají toobe hello stejné. Žádné heslo, můžete zadat, i když se to nedoporučuje.

## <a name="build-a-web-app-creation-application"></a>Vytvoření aplikace vytvoření webové aplikace
### <a name="create-hello-eclipse-workspace-and-maven-project"></a>Vytvoření hello Eclipse prostoru a projekt Maven
V této části vytvoříte pracovní prostor a projekt Maven pro hello webové aplikace vytváření aplikace, s názvem AzureWebDemo.

1. Vytvořte nový projekt Maven. Klikněte na tlačítko **soubor > Nový > Projekt Maven**. V **nový projekt Maven**, vyberte **vytvoření jednoduché projektu** a **používat výchozí umístění prostoru**.
2. Na druhé stránce hello **nový projekt Maven**, zadejte následující hello:
   
   * ID skupiny:`com.<username>.azure.webdemo`
   * ID artefaktů: AzureWebDemo
   * Verze: 0.0.1-SNAPSHOT
   * Balení: jar
   * Název: AzureWebDemo
     
     Klikněte na **Dokončit**.
3. Otevřete soubor pom.xml hello nový projekt v prohlížeči projektu. Vyberte hello **závislosti** kartě. Toto je nový projekt, jsou uvedeny ještě žádné balíčky.
4. Otevřete hello Maven úložiště zobrazení. **Klikněte na okno > Zobrazit zobrazení > jiné > Maven > Maven úložiště** a klikněte na tlačítko **OK**. Hello **Maven úložiště** zobrazení se objeví v hello dolní části hello IDE.
5. Otevřete **globální úložiště**, klikněte pravým tlačítkem na hello **centrální** úložiště a vyberte **Rebuild Index**.
   
    ![][1]
   
    Tento krok může trvat několik minut v závislosti na rychlosti připojení hello. Když znovu sestaví hello index, byste měli vidět hello balíčků Microsoft Azure v hello **centrální** Maven úložiště.
6. V **závislosti**, klikněte na tlačítko **přidat**. V **zadejte ID skupiny...**  zadejte `azure-management`. Vyberte hello balíčky pro základní správu a správu App Service Web Apps:
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > **Poznámka:** hello závislosti při aktualizaci po nové verze verze, je nutné toore-přidejte všechny závislosti hello v tomto seznamu.
   > Po kliknutí na tlačítko **přidat** a vyberte každá závislost, zobrazí se s hello nové číslo verze v hello **závislosti** seznamu.
   > 
   > 

Klikněte na **OK**. Hello Azure balíčky, pak se zobrazí v hello **závislosti** seznamu.

### <a name="writing-java-code-toocreate-a-web-app-by-calling-hello-azure-sdk"></a>Psaní kódu v jazyce Java tooCreate webovou aplikaci pomocí volání hello Azure SDK
Dále napište hello kód, který volá rozhraní API v hello Azure SDK pro jazyk Java toocreate hello webové aplikace App Service.

1. Vytvořte kód Java třídy toocontain hello hlavní vstupní bod. V prohlížeči projektu klikněte pravým tlačítkem na uzel projektu hello a vyberte **nový > třída**.
2. V **nová třída Java**, pojmenujte třídu hello `WebCreator` a zkontrolujte hello **veřejné statické void main** zaškrtávací políčko. Výběr Hello by měla vypadat takto:
   
    ![][2]
3. Klikněte na **Dokončit**. soubor WebCreator.java Hello se zobrazí v prohlížeči projektu.

### <a name="calling-hello-azure-api-toocreate-an-app-service-web-app"></a>Volání rozhraní API služby Azure tooCreate hello webové aplikace App Service
#### <a name="add-necessary-imports"></a>Přidejte potřebné importy
V WebCreator.java přidejte následující importy; hello Tyto importy poskytují přístup tooclasses v hello knihovny správy pro použití rozhraní API Správce Azure:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;

    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;

    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;

    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;

    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-hello-main-entry-point-class"></a>Definování hello hlavní vstupní bod – třída
Protože hello účelem hello AzureWebDemo aplikace je toocreate webové aplikace App Service, name hello hlavní třídy pro tuto aplikaci `WebAppCreator`. Tato třída poskytuje hello hlavní vstupní bod kód, který volá hello Azure Service Management API toocreate hello webové aplikace.

Přidejte následující definice parametr hello webové aplikace a webový prostor hello. Je nutné tooprovide své vlastní informace ID a certifikát pro předplatné Azure.

    public class WebAppCreator {

        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";

        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

Kde:

* `<subscription-id>`je ID hello předplatného Azure, ve kterém chcete toocreate hello prostředků.
* `<certificate-store-path>`je hello cestu a název souboru toohello JKS soubor v adresáři úložiště místní certifikát. Například `C:/Certificates/CertificateName.jks` pro Linux a `C:\Certificates\CertificateName.jks` pro systém Windows.
* `<certificate-password>`je hello heslo, které jste zadali při vytváření vašeho JKS certifikátu.
* `webAppName`může být jakýkoli název, který zvolíte; Tento postup používá název hello `WebDemoWebApp`. Hello úplný název domény je hello `webAppName` s hello `domainName` připojí, takže v tomto případě hello úplné doména je `webdemowebapp.azurewebsites.net`.
* `domainName`musí být zadán jako v příkladu nahoře.
* `webSpaceName`musí být jedna z hodnot hello definované v hello [WebSpaceNames] [ WebSpaceNames] třídy.
* `appServicePlanName`musí být zadán jako v příkladu nahoře.

> **Poznámka:** pokaždé, když jste tuto aplikaci spustit, musíte hodnotu hello toochange `webAppName` a `appServicePlanName` (nebo odstranit hello webovou aplikaci na portálu Azure hello) před spuštěním hello aplikaci znovu. Jinak se spuštění selže, protože hello stejný prostředek již existuje v Azure.
> 
> 

#### <a name="define-hello-web-creation-method"></a>Zadejte způsob vytvoření webové hello
V dalším kroku definujte metoda toocreate hello webové aplikace. Tato metoda `createWebApp`, určuje parametry hello hello webové aplikace a webový prostor hello. Také vytváří a konfiguruje hello App Service Web Apps správy klienta, který je definovaný hello [WebSiteManagementClient] [ WebSiteManagementClient] objektu. Klient správy Hello je klíče toocreating webové aplikace. Poskytuje RESTful webové služby, které umožňují toomanage aplikací webové aplikace (provádění operací, například jako vytvoření, aktualizace a odstranění) voláním rozhraní API pro správu služby hello.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for hello App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path toohello JKS file
            keyStorePassword,  // Password for hello JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create hello App Service Web Apps management client toocall Azure APIs
        // and pass it hello App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for hello web app with hello specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that hello server farm name takes hello Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define hello web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create hello web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output hello HTTP status code of hello response; 200 indicates hello request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output hello name of hello web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Kód Hello výstup hello stav protokolu HTTP odpovědi hello indikující úspěch nebo selhání a pokud bylo úspěšné, bude výstup hello název hello vytvořili webovou aplikaci.

#### <a name="define-hello-main-method"></a>Definování hello main() – metoda
Zadejte kód metoda main() hello volání createWebApp() toocreate hello webové aplikaci.

Nakonec volání `createWebApp` z `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-hello-application-and-verify-web-app-creation"></a>Spuštění aplikace hello a ověřit vytvoření webové aplikace
Klikněte na tlačítko tooverify, které vaše aplikace běží, **spustit > Spustit**. Po dokončení spuštění aplikace hello byste měli vidět následující výstup v konzole Eclipse hello hello:

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

Přihlaste se k portálu Azure classic hello a klikněte na tlačítko **webové aplikace**. Hello nové webové aplikace by měla zobrazí v seznamu webové aplikace hello během několika minut.

## <a name="deploying-an-application-toohello-web-app"></a>Nasazení aplikace toohello webové aplikace
Po spustili AzureWebDemo a vytvořili hello novou webovou aplikaci, přihlaste se k portálu classic hello, klikněte na **webové aplikace**a vyberte **WebDemoWebApp** v hello **webové aplikace** seznamu. Na stránce řídicího panelu hello webové aplikace, klikněte na tlačítko **Procházet** (nebo klikněte na adresu URL hello, `webdemowebapp.azurewebsites.net`) toonavigate tooit. Uvidíte zástupný symbol prázdné stránky, protože žádný obsah ještě nebyla toohello publikované webové aplikace.

Budou vedle vytvořit aplikaci "Hello World" a nasaďte ho toohello webové aplikace.

### <a name="create-a-jsp-hello-world-application"></a>Vytvoření aplikace JSP Hello World
#### <a name="create-hello-application"></a>Vytvoření aplikace hello
V pořadí toodemonstrate jak hello toodeploy toohello webové aplikace, následující postup ukazuje, jak toocreate jednoduchou aplikaci Java "Hello, World" a nahrajte ho toohello aplikace služby webové aplikace, kterou vaše aplikace vytvořena.

1. Klikněte na tlačítko **soubor > Nový > dynamického webového projektu**. Pojmenujte ji `JSPHello`. Není nutné toochange další nastavení v tomto dialogu. Klikněte na **Dokončit**.
   
    ![][3]
2. V prohlížeči projektu rozbalte hello **JSPHello** projektu, klikněte pravým tlačítkem na **WebContent**, pak klikněte na tlačítko **nový > soubor JSP**. V dialogovém okně Nový soubor JSP hello, název nového souboru hello `index.jsp`. Klikněte na **Další**.
3. V hello **vybrat šablonu JSP** vyberte **nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.
4. V index.jsp, přidejte následující kód v hello hello `<head>` a `<body>` značky částech:
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, hello time is <%= date %> 
        </body>

#### <a name="run-hello-hello-world-application-in-localhost"></a>Spuštění aplikace Hello World hello v localhost
Než spustíte tuto aplikaci, musíte tooconfigure několik vlastností.

1. Klikněte pravým tlačítkem na hello **JSPHello** projektu a vyberte **vlastnosti**.
2. V hello **vlastnosti** dialogové okno: vyberte **cesta sestavení Java**, vyberte hello **pořadí a Export** zkontrolujte **prostředí JRE systémová knihovna**, pak klikněte na **Až** toomove ho toohello horní části seznamu hello.
   
    ![][4]
3. Také v hello **vlastnosti** dialogu: vyberte **Targeted Runtimes** a klikněte na tlačítko **nový**.
4. V hello **nové běhové prostředí serveru** dialogovém okně, vyberte server, jako **Apache Tomcat v7.0** a klikněte na tlačítko **Další**. V hello **Tomcat Server** dialogové okno, sada **název** příliš`Apache Tomcat v7.0`a nastavte **instalační adresář Tomcat** toohello adresář, do které jste nainstalovali verzi hello Chcete-li toouse server tomcat.
   
    ![][5]
   
    Klikněte na **Dokončit**.
5. Pak se vraťte toohello **Targeted Runtimes** stránku hello **vlastnosti** dialogové okno. Vyberte **Apache Tomcat v7.0**, pak klikněte na tlačítko **OK**.
   
    ![][6]
6. V hello Eclipse **spustit** nabídky, klikněte na tlačítko **spustit**. V hello **spustit jako** dialogovém okně, vyberte **spustit na serveru**. V hello **spustit na serveru** dialogovém okně, vyberte **Tomcat v7.0 Server**:
   
    ![][7]
   
    Klikněte na **Dokončit**.
7. Když hello spouštět aplikace, měli byste vidět hello **JSPHello** stránky se zobrazí v okně localhost v prostředí Eclipse (`http://localhost:8080/JSPHello/`), zobrazování hello následující zpráva:
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-hello-application-as-a-war"></a>Exportovat jako WAR aplikace hello
Exportujte souborů projektu webové hello jako webový archiv (WAR) soubor tak, aby ho mohli nasadit toohello webové aplikace. Hello následující souborů projektu webové jsou umístěny ve složce WebContent hello:

    META-INF
    WEB-INF
    index.jsp

1. Klikněte pravým tlačítkem na složku WebContent hello a vyberte **exportovat**.
2. V hello **Exportovat vyberte** dialogové okno, klikněte na tlačítko **webové > WAR** souboru a pak klikněte na **Další**.
3. V hello **WAR Export** dialogovém okně, vyberte adresář src hello v aktuálním projektu hello a obsahovat název hello hello souboru WAR na konci hello. Například:
   
    `<project-path>/JSPHello/src/JSPHello.war`

Další informace o nasazení WAR soubory, najdete v části [přidat tooAzure aplikace Java App Service Web Apps](web-sites-java-add-app.md).

### <a name="deploying-hello-hello-world-application-using-ftp"></a>Nasazení hello Hello World aplikace pomocí FTP
Vyberte aplikaci hello toopublish klienta FTP třetích stran. Tento postup popisuje dvě možnosti: konzoly Kudu hello integrovaný do Azure; a FileZilla oblíbených nástroj s praktické, grafické uživatelské rozhraní.

> **Poznámka:** hello Azure nástrojů Eclipse podporuje účty toostorage nasazení a cloudové služby, ale aktuálně nepodporuje nasazení tooweb aplikace. Můžete nasadit toostorage účty a cloudových služeb pomocí projektu nasazení aplikace Azure, jak je popsáno v [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), ale není tooweb aplikací. Používejte jiné metody, jako je například protokol FTP nebo Githubu tootransfer soubory tooyour webové aplikace.
> 
> **Poznámka:** nedoporučujeme pomocí FTP z příkazového řádku Windows hello (hello příkazového řádku FTP.EXE nástroj, který se dodává s Windows). Klienti FTP, které používají active FTP, jako je například FTP.EXE, často převzetí služeb při selhání toowork brány firewall. Aktivní FTP určuje interní adresa založený na síti LAN, server toowhich k serveru FTP se pravděpodobně nezdaří tooconnect.
> 
> 

Další informace o nasazení tooan webové aplikace App Service pomocí protokolu FTP najdete v části hello následující témata:

* [Nasazení pomocí nástroje FTP](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a>Nastavit přihlašovací údaje nasazení.
Zajistěte, aby spustíte hello **AzureWebDemo** toocreate aplikace webové aplikace. Přenese soubory toothis umístění.

1. Přihlaste se k portálu classic hello a klikněte na tlačítko **webové aplikace**. Zajistěte, aby **WebDemoWebApp** se zobrazí v hello seznam webových aplikací a ujistěte se, zda je spuštěna. Klikněte na tlačítko **WebDemoWebApp** tooopen jeho **řídicí panel** stránky.
2. Na hello **řídicí panel** v části **rychlý přehled**, klikněte na tlačítko **nastavit přihlašovací údaje nasazení** (Pokud již máte přihlašovací údaje nasazení, to čte  **Resetovat přihlašovací údaje nasazení**).
   
    Přihlašovací údaje nasazení jsou přidruženy k účtu Microsoft. Potřebujete toospecify uživatelské jméno a heslo, které můžete použít toodeploy pomocí Git a FTP. Ve všech předplatných Azure spojené s vaším účtem Microsoft můžete použít tyto přihlašovací údaje toodeploy tooany webové aplikace. Git a FTP nasazení pověření zadejte v dialogovém okně hello a záznamů hello uživatelské jméno a heslo pro budoucí použití.

#### <a name="get-ftp-connection-information"></a>Získat informace o připojení FTP
toouse FTP toodeploy aplikace soubory toohello nově vytvořit webovou aplikaci, musíte tooobtain informace o připojení. Existují dva způsoby tooobtain informace o připojení. Jedním ze způsobů je toovisit hello webové aplikace **řídicí panel** stránka; hello druhý způsob je profil publikování se toodownload hello webové aplikace. profil publikování se Hello je soubor XML, který poskytuje informace, jako je název a přihlašovací údaje hostitele FTP pro vaše webové aplikace v Azure App Service. Toto uživatelské jméno a heslo toodeploy tooany webové aplikace můžete použít v Všechna předplatná spojená s hello účet Azure, ne jenom tato jednoho.

informace o připojení tooobtain FTP v okně hello webové aplikace v hello [portálu Azure][Azure Portal]:

1. V části **Essentials**, najít a zkopírovat hello **název hostitele FTP**. Toto je identifikátor URI, které jsou podobné příliš`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.
2. V části **Essentials**, najít a zkopírovat **uživatelské jméno FTP/nasazení**. To bude mít hello formuláře *webappname\deployment-username*; například `WebDemoWebApp\deployer77`.

profil publikování se informace o připojení FTP tooobtain z hello:

1. V okně hello webové aplikace, klikněte na tlačítko **profilu publikování Get**. To bude stahovat místní jednotku tooyour soubor .publishsettings.
2. Otevřete soubor .publishsettings hello v editoru XML nebo textovém editoru a najděte hello `<publishProfile>` element obsahující `publishMethod="FTP"`. By měl vypadat jako následující hello:
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. Všimněte si, že webová aplikace hello `publishProfile` nastavení mapování toohello správce webu FileZilla nastavení následujícím způsobem:

* `publishUrl`je stejný jako hello **název hostitele FTP**, hello hodnotu nastavenou v **hostitele**.
* `publishMethod="FTP"`znamená, že nastavíte **protokol** příliš**FTP - File Transfer Protocol**, a **šifrování** příliš**pomocí protokolu FTP prostý**.
* `userName`a `userPWD` jsou klíče pro hello skutečnými hodnotami uživatelské jméno a heslo jste zadali, když resetujete hello přihlašovací údaje nasazení. `userName`je stejný jako hello **nasazení / FTP uživatele**. Mapují příliš**uživatele** a **heslo** v FileZilla.
* `ftpPassiveMode="True"`znamená, že lokality hello FTP používá pasivní přenos FTP; Vyberte **pasivní** na hello **nastavení přenosu** kartě.

#### <a name="configure-hello-web-app-toohost-a-java-application"></a>Konfigurovat webovou aplikaci toohost hello aplikace Java
Před publikováním aplikace hello musíte toochange několik nastavení konfigurace tak, aby hello webové aplikace může hostovat aplikace Java.

1. V portálu classic hello přejděte toohello webové aplikace **řídicí panel** a klikněte na tlačítko **konfigurace**. Na hello **konfigurace** stránky, zadejte následující nastavení hello.
2. V **verzi Javy** hello výchozí hodnota je **vypnout**; vyberte verzi Javy hello cíle vaší aplikace; například 1.7.0_51. Až to uděláte, také zkontrolujte, zda **webový kontejner** nastavena tooa verzi serveru Tomcat.
3. V **výchozí dokumenty**přidejte index.jsp a přesunout nahoru toohello horní části seznamu hello. (hello výchozí soubor pro webové aplikace je hostingstart.html.)
4. Klikněte na **Uložit**.

#### <a name="publish-your-application-using-kudu"></a>Publikování aplikace pomocí modulu Kudu
Jedním ze způsobů toopublish hello aplikace je toouse hello ladění Kudu konzoly integrovaný do Azure. Kudu se označuje toobe stabilní a v souladu s App Service Web Apps a Tomcat Server. Můžete procházením tooa URL hello následující formulář získali přístup ke konzole hello pro webovou aplikaci hello:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Pro tento postup je umístěn v následující adresu URL; hello hello Kudu konzoly Procházet toothis umístění:
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. Hello horní nabídce vyberte **ladění konzoly > CMD**.
3. V příkazovém řádku hello konzoly, přejděte příliš`/site/wwwroot` (nebo klikněte na tlačítko `site`, pak `wwwroot` v zobrazení adresáře hello hello horní části stránky hello):
   
    `cd /site/wwwroot`
4. Po zadání **verzi Javy**, Tomcat server měli vytvořit adresáře webapps. V příkazovém řádku hello konzoly přejděte adresáře webapps toohello:
   
    `mkdir webapps`
   
    `cd webapps`
5. Přetáhněte JSPHello.war z `<project-path>/JSPHello/src/` a umístěte jej do zobrazení adresáře Kudu hello pod `/site/wwwroot/webapps`. Není ji přetáhněte toohello "Přetáhněte sem tooupload a zip" oblast, protože Tomcat bude rozbalte ho.
   
   ![][8]

V první JSPHello.war se zobrazí v oblasti directory hello samostatně:

  ![][9]

V krátkém čase (pravděpodobně méně než 5 minut) bude Tomcat Server rozbalte soubor WAR hello do adresář rozbalené JSPHello. Klikněte na tlačítko hello kořenový adresář toosee, zda index.jsp byla rozbalené a zkopírovat existuje. Pokud ano, přejděte zpět toohello webapps directory toosee zda hello vybaleno JSPHello adresář byl vytvořen. Pokud se tyto položky nezobrazí, počkejte a opakujte.

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a>Publikování aplikace pomocí FileZilla (volitelné)
Jiný nástroj, můžete použít aplikaci hello toopublish je FileZilla oblíbených klienta FTP třetích stran s praktické, grafické uživatelské rozhraní. Můžete stáhnout a nainstalovat FileZilla z [http://filezilla-project.org/](http://filezilla-project.org/) Pokud již jste ji. Další informace o používání hello klienta najdete v tématu hello [FileZilla dokumentace](https://wiki.filezilla-project.org/Documentation) a tuto položku blogu na [klienti FTP – část 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. V FileZilla, klikněte na **soubor > Správce webu**.
2. V hello **správce webu** dialogové okno, klikněte na tlačítko **nové lokality**. Nový prázdný web FTP se zobrazí v **vyberte položku** výzvou tooprovide název. Tento postup, pojmenujte ji `AzureWebDemo-FTP`.
   
    Na hello **Obecné** zadejte hello následující nastavení:
   
   * **Hostitele:** Enter hello **název hostitele FTP** který jste zkopírovali z řídicího panelu hello.
   * **Port:** (necháte prázdné, to je pasivní přenos a hello server určí hello port toouse.)
   * **Protokol:** protokol pro přenos souborů FTP
   * **Šifrování:** použít prostý FTP
   * **Typ přihlášení:** normální
   * **Uživatel:** Enter hello nasazení / FTP uživatele, který jste zkopírovali z řídicího panelu hello. Toto je hello úplné FTP uživatelského jména, která má hello formuláře *webappname\username*.
   * **Heslo:** zadejte hello heslo, kterou jste zadali, když nastavíte přihlašovací údaje nasazení hello.
     
     Na hello **nastavení přenosu** vyberte **pasivní**.
3. Klikněte na **Připojit**. Pokud úspěšné, je FileZilla konzoly se zobrazí `Status: Connected` zprávu a problém `LIST` příkaz obsah adresáře toolist hello.
4. V hello **místní** lokality panelů, vyberte hello zdrojový adresář ve který hello JSPHello.war soubor nachází; hello cesta bude podobné toohello následující:
   
    `<project-path>/JSPHello/src/`
5. V hello **vzdáleného** lokality panelů, vyberte hello cílovou složku. Budete nasazovat toohello soubor WAR hello `webapps` adresář v kořenovém adresáři hello webové aplikace. Přejděte příliš`/site/wwwroot`, klikněte pravým tlačítkem na `wwwroot`a vyberte **vytvořit adresář**. Adresář se jménem hello `webapps` a zadejte tento adresář.
6. Přenos JSPHello.war příliš`/site/wwwroot/webapps`. Vyberte JSPHello.war v hello **místní** soubor seznamu, klikněte na něj pravým tlačítkem myši a vyberte **nahrát**. Měli byste vidět se zobrazí v `/site/wwwroot/webapps`.
7. Po zkopírování adresáře webapps toohello JSPHello.war bude automaticky rozbalte Tomcat Server (rozbalte) hello souborů v souboru WAR hello. I když Tomcat Server začne rozbalování téměř okamžitě, může trvat dlouho čas (pravděpodobně hodiny) pro soubory tooappear hello v klientovi hello FTP.

#### <a name="run-hello-hello-world-application-on-hello-web-app"></a>Spuštění aplikace Hello World hello na hello webové aplikace
1. Po nahrát soubor WAR hello a ověřit, že Tomcat server vytvořil rozbalené `JSPHello` adresáře, procházet příliš`http://webdemowebapp.azurewebsites.net/JSPHello` toorun hello aplikace.
   
   > **Poznámka:** Pokud kliknete na tlačítko **Procházet** z portálu classic hello, může získat výchozí webovou stránku hello, oznámením "této webové aplikace Java na základě úspěšně vytvořila." Můžete mít webovou stránku hello toorefresh ve výstupu aplikace hello pořadí tooview místo výchozí webovou stránku hello.
   > 
   > 
2. Při spuštění aplikace hello byste měli vidět na webové stránce s hello následující výstup:
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a>Vyčištění prostředků Azure
Tento postup vytvoří webové aplikace služby App Service. Také existuje bude platit pro prostředek hello. Pokud máte v plánu toocontinue použití hello webové aplikace pro testování nebo pro vývoj, měli byste zvážit, zastavení nebo odstranění. Webovou aplikaci, která byla zastavena stále zpoplatněná malé poplatků, ale můžete ji restartovat kdykoli. Odstraněním webové aplikace vymaže veškerá data, které jste odeslali tooit.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

[1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
[2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
[3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
[4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
[5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
[6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
[7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
[8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
[9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
[10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png


[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure classic portal]: https://manage.windowsazure.com
[What is an Azure AD directory]: http://technet.microsoft.com/library/jj573650.aspx
[Create and Upload a Management Certificate for Azure]: ../cloud-services/cloud-services-certs-create.md
[Key and Certificate Management Tool (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com
