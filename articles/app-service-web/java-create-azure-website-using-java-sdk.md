---
title: "Vytvoření webové aplikace v Azure App Service pomocí sady Azure SDK pro jazyk Java"
description: "Naučte se vytvořit webovou aplikaci v Azure App Service programově pomocí sady Azure SDK pro jazyk Java."
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
ms.openlocfilehash: 08bb53de8cf437a5a2b1c3b38bce9f81b8349493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Vytvoření webové aplikace v Azure App Service pomocí sady Azure SDK pro jazyk Java
<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Přehled
Tento postup vám ukáže, jak vytvořit sadu Azure SDK pro aplikace Java, která vytvoří webovou aplikaci v [Azure App Service][Azure App Service], pak nasazení aplikace do ní. Skládá se ze dvou částí:

* Část 1 ukazuje, jak sestavit aplikaci Java, která vytvoří webovou aplikaci.
* Část 2 ukazuje, jak vytvořit jednoduché JSP "Hello World" aplikace a pak použijte k serveru FTP klienta nasadíte kód do služby App Service.

## <a name="prerequisites"></a>Požadavky
### <a name="software-installations"></a>Instalace softwaru
AzureWebDemo kódu aplikace v tomto článku byla zapsána pomocí sady Azure Java SDK 0.7.0, které můžete nainstalovat pomocí [instalačního programu webové platformy] [ Web Platform Installer] (WebPI). Kromě toho, nezapomeňte použít nejnovější verzi [nástrojů Azure pro Eclipse][Azure Toolkit for Eclipse]. Po instalaci sady SDK, aktualizace závislosti ve vašem projektu Eclipse spuštěním **aktualizovat Index** v **Maven úložiště**, nejnovější verze jednotlivých balíčků v znovu přidat **závislosti** okno. Kliknutím můžete ověřit verzi nainstalovaného softwaru v prostředí Eclipse **pomoci > podrobné informace o instalaci**; byste měli mít alespoň následující verze:

* Balíček pro knihovny Microsoft Azure Libraries for Java 0.7.0.20150309
* Integrované vývojové prostředí pro vývojáře v jazyce Java EE 4.4.2.20150219 Eclipse

### <a name="create-and-configure-cloud-resources-in-azure"></a>Vytvoření a konfigurace cloudové prostředky v Azure
Před zahájením tohoto postupu, potřebujete mít aktivní předplatné Azure a nastavit výchozí Active Directory (AD) v Azure.

### <a name="create-an-active-directory-ad-in-azure"></a>Vytvořte v Azure Active Directory (AD)
Pokud jste již nemají Active Directory (AD) na vaše předplatné Azure, přihlaste se k [portál Azure classic] [ Azure classic portal] s vaším účtem Microsoft. Pokud máte více předplatných, klikněte na tlačítko **odběry** a vyberte výchozí adresář pro předplatné, které chcete použít pro tento projekt. Pak klikněte na tlačítko **použít** přepnout do zobrazení tohoto odběru.

1. Vyberte **služby Active Directory** z nabídky na levé straně. **Klikněte na tlačítko Nový > adresář > vytvořit vlastní**.
2. V **přidat adresář**, vyberte **vytvořte nový adresář**.
3. V **název**, zadejte název adresáře.
4. V **domény**, zadejte název domény. Toto je název základní domény, který je zahrnut ve výchozím nastavení s adresářem; má formuláře `<domain_name>.onmicrosoft.com`. Můžete pojmenovat podle názvu adresáře nebo jiný název domény, který vlastníte. Později můžete přidat jiný název domény, který vaše organizace už používá.
5. V **zemi nebo oblast**, vyberte národní prostředí.

Další informace o AD, najdete v části [co je adresář Azure AD][What is an Azure AD directory]?

### <a name="create-a-management-certificate-for-azure"></a>Vytvoření certifikátu správy pro Azure.
Azure SDK pro jazyk Java používá certifikáty pro správu k ověřování pomocí předplatných Azure. Toto jsou certifikáty X.509 v3 sloužící k ověření pravosti aplikace klienta, která používá Service Management API zastupovat vlastník předplatného ke správě prostředků předplatného.

Kód v tomto postupu používá certifikát podepsaný svým držitelem k ověření pomocí Azure. Tento postup, musíte vytvořit certifikát a nahrajte ho do [portál Azure classic] [ Azure classic portal] předem. To zahrnuje následující kroky:

* Generování souboru PFX představující klientský certifikát a uložte ho místně.
* Vygenerujte certifikát správy (soubor CER) ze souboru PFX.
* Nahrání souboru CER do vašeho předplatného Azure.
* Soubor PFX převeďte JKS, protože Java použije tento formát ověřování pomocí certifikátů.
* Zápis kódu ověřování aplikace, která odkazuje na místní soubor JKS.

Po dokončení tohoto postupu CER certifikát se bude nacházet ve vašem předplatném Azure a certifikát JKS se bude nacházet na místním disku. Další informace o certifikáty pro správu najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure][Create and Upload a Management Certificate for Azure].

#### <a name="create-a-certificate"></a>Vytvoření certifikátu
Pokud chcete vytvořit vlastní certifikát podepsaný svým držitelem, otevřete konzolu příkaz v operačním systému a spusťte následující příkazy.

> **Poznámka:** počítač, na kterém je spuštěn tento příkaz musí mít JDK nainstalována. Cesta ke keytool navíc závisí na umístění, ve kterém nainstalujete sadu JDK. Další informace najdete v tématu [klíč a nástroj pro správu certifikát (keytool)] [ Key and Certificate Management Tool (keytool)] v online dokumentaci Java.
> 
> 

Chcete-li vytvořit soubor .pfx:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

Chcete-li vytvořit soubor .cer:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

Kde:

* `<java-install-dir>`je cesta k adresáři, do které jste nainstalovali Java.
* `<keystore-id>`je identifikátor záznamu úložiště klíčů (například `AzureRemoteAccess`).
* `<cert-store-dir>`je cesta k adresáři, ve kterém chcete uložit certifikáty (například `C:/Certificates`).
* `<cert-file-name>`je název souboru certifikátu (například `AzureWebDemoCert`).
* `<password>`je heslo, které jste se rozhodli chránit certifikátu; musí být alespoň 6 znaků. Žádné heslo, můžete zadat, i když se to nedoporučuje.
* `<dname>`je X.500 rozlišující název, který se má přidružit alias a slouží jako pole Vystavitel a předmětu v certifikátu podepsaného svým držitelem.

Další informace najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure][Create and Upload a Management Certificate for Azure].

#### <a name="upload-the-certificate"></a>Nahrát na server certifikát
Nahrajte certifikát podepsaný svým držitelem do Azure, přejděte na **nastavení** na portálu classic a pak klikněte na **certifikáty pro správu** kartě. Klikněte na tlačítko **nahrát** v dolní části stránky a přejděte do umístění souboru CER jste vytvořili.

#### <a name="convert-the-pfx-file-into-jks"></a>Převeďte soubor PFX do JKS
V na příkazovém řádku Windows (spuštěna jako správce), disk cd adresář obsahující certifikáty a spusťte následující příkaz, kde `<java-install-dir>` je adresář, ve kterém je nainstalovaná Java ve vašem počítači:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Po zobrazení výzvy zadejte heslo cílové úložiště klíčů; bude heslo pro soubor JKS.
2. Po zobrazení výzvy zadejte heslo zdrojové úložiště klíčů; Toto je heslo, které jste zadali pro soubor PFX.

Zadaná dvě hesla se nemusíte být stejné. Žádné heslo, můžete zadat, i když se to nedoporučuje.

## <a name="build-a-web-app-creation-application"></a>Vytvoření aplikace vytvoření webové aplikace
### <a name="create-the-eclipse-workspace-and-maven-project"></a>Vytvořit pracovní prostor Eclipse a projekt Maven
V této části vytvoříte pracovní prostor a pro webovou aplikaci vytvoření aplikaci s názvem AzureWebDemo projekt Maven.

1. Vytvořte nový projekt Maven. Klikněte na tlačítko **soubor > Nový > Projekt Maven**. V **nový projekt Maven**, vyberte **vytvoření jednoduché projektu** a **používat výchozí umístění prostoru**.
2. Na druhé stránce **nový projekt Maven**, zadejte následující:
   
   * ID skupiny:`com.<username>.azure.webdemo`
   * ID artefaktů: AzureWebDemo
   * Verze: 0.0.1-SNAPSHOT
   * Balení: jar
   * Název: AzureWebDemo
     
     Klikněte na **Dokončit**.
3. Otevřete soubor pom.xml nový projekt v prohlížeči projektu. Vyberte **závislosti** kartě. Toto je nový projekt, jsou uvedeny ještě žádné balíčky.
4. Otevření zobrazení Maven úložiště. **Klikněte na okno > Zobrazit zobrazení > jiné > Maven > Maven úložiště** a klikněte na tlačítko **OK**. **Maven úložiště** zobrazení se zobrazí v dolní části rozhraní IDE.
5. Otevřete **globální úložiště**, klikněte pravým tlačítkem myši **centrální** úložiště a vyberte **Rebuild Index**.
   
    ![][1]
   
    Tento krok může trvat několik minut v závislosti na rychlosti připojení. Když znovu sestaví index, byste měli vidět balíčků Microsoft Azure v **centrální** Maven úložiště.
6. V **závislosti**, klikněte na tlačítko **přidat**. V **zadejte ID skupiny...**  zadejte `azure-management`. Vyberte balíčky, pro základní správu a správu App Service Web Apps:
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > **Poznámka:** po novou verzi verzí při aktualizaci závislosti, budete muset znovu přidejte všechny závislosti v tomto seznamu.
   > Po kliknutí na tlačítko **přidat** a vyberte každá závislost, zobrazí se nové číslo verze v **závislosti** seznamu.
   > 
   > 

Klikněte na **OK**. Azure balíčky se potom zobrazí v **závislosti** seznamu.

### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Psaní kódu Java k vytvoření webové aplikace při volání sady Azure SDK
Dále napište kód, který zavolá rozhraní API v Azure SDK pro jazyk Java k vytvoření webové aplikace služby App Service.

1. Vytvořte třídu Java, obsahuje kód hlavní vstupní bod. V prohlížeči projektu klikněte pravým tlačítkem na uzel projektu a vyberte **nový > třída**.
2. V **nová třída Java**, název třídy `WebCreator` a zkontrolujte **veřejné statické void main** zaškrtávací políčko. Výběr by měla vypadat takto:
   
    ![][2]
3. Klikněte na **Dokončit**. Soubor WebCreator.java se zobrazí v prohlížeči projektu.

### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Volání rozhraní API Azure k vytvoření webové aplikace služby App Service
#### <a name="add-necessary-imports"></a>Přidejte potřebné importy
V WebCreator.java přidejte následující importy; Tyto importy poskytnout přístup k třídy v knihovnách správy pro použití rozhraní API Správce Azure:

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


#### <a name="define-the-main-entry-point-class"></a>Zadejte třídu hlavní vstupní bod
Protože účelem AzureWebDemo aplikace je vytvoření webové aplikace App Service, název hlavní třídy pro tuto aplikaci `WebAppCreator`. Tato třída poskytuje kód hlavní vstupní bod, který volá Azure Service Management API k vytvoření webové aplikace.

Přidejte následující definice parametr pro webovou aplikaci a webový prostor. Musíte zadat své vlastní informace ID a certifikát pro předplatné Azure.

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

* `<subscription-id>`je ID předplatného Azure, ve kterém chcete vytvořit prostředek.
* `<certificate-store-path>`je cesta a název souboru k souboru JKS ve vašem adresáři úložiště místní certifikát. Například `C:/Certificates/CertificateName.jks` pro Linux a `C:\Certificates\CertificateName.jks` pro systém Windows.
* `<certificate-password>`je heslo, které jste zadali při vytváření vašeho JKS certifikátu.
* `webAppName`může být jakýkoli název, který zvolíte; Tento postup používá název `WebDemoWebApp`. Je plně názvu domény `webAppName` s `domainName` připojí, takže v tomto případě úplné doména je `webdemowebapp.azurewebsites.net`.
* `domainName`musí být zadán jako v příkladu nahoře.
* `webSpaceName`musí být jedna z hodnot fronty definovaných v [WebSpaceNames] [ WebSpaceNames] třídy.
* `appServicePlanName`musí být zadán jako v příkladu nahoře.

> **Poznámka:** pokaždé, když jste tuto aplikaci spustit, budete muset změnit hodnotu `webAppName` a `appServicePlanName` (nebo odstranit webovou aplikaci na portálu Azure) před spuštěním aplikaci znovu. Jinak se spuštění selže, protože stejný prostředek již existuje v Azure.
> 
> 

#### <a name="define-the-web-creation-method"></a>Definování webové metody vytvoření
V dalším kroku definujte metodu pro vytvoření webové aplikace. Tato metoda `createWebApp`, určuje parametry webové aplikace a webový prostor. Také vytváří a konfiguruje App Service Web Apps správy klienta, který je definovaný [WebSiteManagementClient] [ WebSiteManagementClient] objektu. Klient pro správu je klíčem k vytvoření webové aplikace. Poskytuje RESTful webové služby, která umožňují aplikacím ke správě webových aplikací (provádění operací, například jako vytvoření, aktualizace a odstranění) voláním rozhraní API správy služby.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
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
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Kód bude výstup stav protokolu HTTP odpovědi, která udává úspěch nebo selhání a pokud bylo úspěšné, bude výstup název vytvořenou webovou aplikaci.

#### <a name="define-the-main-method"></a>Zadejte metodu main()
Zadejte kód main() metoda, která volá createWebApp() k vytvoření webové aplikace.

Nakonec volání `createWebApp` z `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Spusťte aplikaci a ověřte vytvoření webové aplikace
Chcete-li ověřit, že je aplikace spuštěna, klikněte na tlačítko **spustit > Spustit**. Po dokončení spuštění aplikace byste měli vidět následující výstup v prostředí Eclipse konzole:

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

Přihlaste se k portálu Azure classic a klikněte na tlačítko **webové aplikace**. Nové webové aplikace mají objevit v seznamu webové aplikace během několika minut.

## <a name="deploying-an-application-to-the-web-app"></a>Nasazení aplikace do webové aplikace
Poté, co jste spustili AzureWebDemo a vytvořit novou webovou aplikaci, přihlaste se k portálu classic klikněte na **webové aplikace**a vyberte **WebDemoWebApp** v **webové aplikace** seznamu. Na stránce řídicího panelu webové aplikace, klikněte na tlačítko **Procházet** (nebo klikněte na adresu URL, `webdemowebapp.azurewebsites.net`) a přejděte k němu. Uvidíte zástupný symbol prázdné stránky, protože žádný obsah má zatím nebyly publikovány do webové aplikace.

Budou vedle vytvořit aplikaci "Hello World" a nasazení do webové aplikace.

### <a name="create-a-jsp-hello-world-application"></a>Vytvoření aplikace JSP Hello World
#### <a name="create-the-application"></a>Vytvoření aplikace
Chcete-li ukazují, jak nasadit aplikaci na webu, následující postup ukazuje, jak vytvořit jednoduchou aplikaci Java "Hello World" a nahrajte ho do webové aplikace App Service, vytvořené aplikace.

1. Klikněte na tlačítko **soubor > Nový > dynamického webového projektu**. Pojmenujte ji `JSPHello`. Nepotřebujete žádné další nastavení v tomto dialogu můžete změnit. Klikněte na **Dokončit**.
   
    ![][3]
2. V prohlížeči projektu, rozbalte **JSPHello** projektu, klikněte pravým tlačítkem na **WebContent**, pak klikněte na tlačítko **nový > soubor JSP**. V dialogovém okně Nový soubor JSP název nového souboru `index.jsp`. Klikněte na **Další**.
3. V **vybrat šablonu JSP** vyberte **nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.
4. V index.jsp, přidejte následující kód do `<head>` a `<body>` značky částech:
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, the time is <%= date %> 
        </body>

#### <a name="run-the-hello-world-application-in-localhost"></a>Spuštění aplikace Hello World v localhost
Před spuštěním této aplikace, budete muset nakonfigurovat několik vlastností.

1. Klikněte pravým tlačítkem myši **JSPHello** projektu a vyberte **vlastnosti**.
2. V **vlastnosti** dialogové okno: vyberte **cesta sestavení Java**, vyberte **pořadí a Export** zkontrolujte **prostředí JRE systémová knihovna**, pak klikněte na tlačítko **až** ho přesunout na začátek seznamu.
   
    ![][4]
3. Také v **vlastnosti** dialogu: vyberte **Targeted Runtimes** a klikněte na tlačítko **nový**.
4. V **nové běhové prostředí serveru** dialogovém okně, vyberte server, jako **Apache Tomcat v7.0** a klikněte na tlačítko **Další**. V **Tomcat Server** dialogové okno, sada **název** k `Apache Tomcat v7.0`a nastavte **instalační adresář Tomcat** k adresáři, do které jste nainstalovali verzi serveru Tomcat, kterou chcete použít.
   
    ![][5]
   
    Klikněte na **Dokončit**.
5. Budete pak se vraťte do **Targeted Runtimes** stránky **vlastnosti** dialogové okno. Vyberte **Apache Tomcat v7.0**, pak klikněte na tlačítko **OK**.
   
    ![][6]
6. V prostředí Eclipse **spustit** nabídky, klikněte na tlačítko **spustit**. V **spustit jako** dialogovém okně, vyberte **spustit na serveru**. V **spustit na serveru** dialogovém okně, vyberte **Tomcat v7.0 Server**:
   
    ![][7]
   
    Klikněte na **Dokončit**.
7. Když je aplikace spuštěná, měli byste vidět **JSPHello** stránky se zobrazí v okně localhost v prostředí Eclipse (`http://localhost:8080/JSPHello/`), zobrazí následující zprávu:
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-the-application-as-a-war"></a>Export aplikace jako soubor WAR
Exportujte souborů projektu webové jako webový archiv (WAR) soubor tak, aby ji můžete nasadit do webové aplikace. Následující souborů projektu webové jsou umístěny ve složce WebContent:

    META-INF
    WEB-INF
    index.jsp

1. Klikněte pravým tlačítkem na složku WebContent a vyberte **exportovat**.
2. V **Exportovat vyberte** dialogové okno, klikněte na tlačítko **webové > WAR** souboru a pak klikněte na **Další**.
3. V **WAR Export** dialogovém okně, vyberte adresář, src v aktuálním projektu a obsahovat název souboru WAR na konci. Například:
   
    `<project-path>/JSPHello/src/JSPHello.war`

Další informace o nasazení WAR soubory, najdete v části [přidat aplikace v jazyce Java do Azure App Service Web Apps](web-sites-java-add-app.md).

### <a name="deploying-the-hello-world-application-using-ftp"></a>Nasazení aplikace Hello World pomocí protokolu FTP
Vyberte klienta FTP třetích stran k publikování aplikace. Tento postup popisuje dvě možnosti: konzole Kudu integrovaný do Azure; a FileZilla oblíbených nástroj s praktické, grafické uživatelské rozhraní.

> **Poznámka:** sady nástrojů Azure pro prostředí Eclipse podporuje nasazení na účty úložiště a cloudové služby, ale aktuálně nepodporuje nasazení do webové aplikace. Můžete nasadit na účty úložiště a cloudových služeb pomocí projektu nasazení aplikace Azure, jak je popsáno v [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), ale ne na webové aplikace. Používejte jiné metody, jako je například protokol FTP nebo GitHub pro přenos souborů do vaší webové aplikace.
> 
> **Poznámka:** nedoporučujeme pomocí protokolu FTP z příkazového řádku Windows (nástroj příkazového řádku FTP.EXE dodávané se systémem Windows). Klienti FTP, které používají active FTP, jako je například FTP.EXE, často přestat fungovat přes brány firewall. Aktivní FTP určuje interní adresa založený na síti LAN, do které FTP server se pravděpodobně nepodaří připojit.
> 
> 

Další informace o nasazení do webové aplikace služby App Service pomocí protokolu FTP najdete v následujících tématech:

* [Nasazení pomocí nástroje FTP](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a>Nastavit přihlašovací údaje nasazení.
Zajistěte, aby jste spustili **AzureWebDemo** aplikace k vytvoření webové aplikace. Bude přenos souborů do tohoto umístění.

1. Přihlaste se k portálu classic a klikněte na tlačítko **webové aplikace**. Zajistěte, aby **WebDemoWebApp** se zobrazí v seznamu webové aplikace a ujistěte se, zda je spuštěna. Klikněte na tlačítko **WebDemoWebApp** otevřete jeho **řídicí panel** stránky.
2. Na **řídicí panel** v části **rychlý přehled**, klikněte na tlačítko **nastavit přihlašovací údaje nasazení** (Pokud již máte přihlašovací údaje nasazení, to čte **resetovat přihlašovací údaje nasazení**).
   
    Přihlašovací údaje nasazení jsou přidruženy k účtu Microsoft. Je třeba zadat uživatelské jméno a heslo, které můžete nasadit pomocí Git a FTP. Tyto přihlašovací údaje můžete použít k nasazení na jakoukoli webovou aplikaci ve všech předplatných Azure spojené s vaším účtem Microsoft. Zadejte Git a FTP přihlašovací údaje nasazení v dialogovém okně a zaznamenejte uživatelské jméno a heslo pro budoucí použití.

#### <a name="get-ftp-connection-information"></a>Získat informace o připojení FTP
K nasazení souborů aplikace do nově vytvořené webové aplikace pomocí protokolu FTP, potřebujete získat informace o připojení. Existují dva způsoby, jak získat informace o připojení. Jedním ze způsobů je navštívit webovou aplikaci **řídicí panel** stránka; druhý způsob je ke stažení webové aplikace profilu publikování. Profil publikování je soubor XML, který poskytuje informace, jako je název a přihlašovací údaje hostitele FTP pro vaše webové aplikace v Azure App Service. Toto uživatelské jméno a heslo slouží k nasazení na jakoukoli webovou aplikaci v Všechna předplatná spojená s účtem Azure, ne jenom následujícímu.

Získat informace o připojení FTP v okně webové aplikace v [portálu Azure][Azure Portal]:

1. V části **Essentials**, najít a zkopírovat **název hostitele FTP**. Toto je identifikátor URI podobná `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.
2. V části **Essentials**, najít a zkopírovat **uživatelské jméno FTP/nasazení**. To bude mít formát *webappname\deployment-username*; například `WebDemoWebApp\deployer77`.

Získat informace o připojení FTP z profilu publikování:

1. V okně webové aplikace, klikněte na tlačítko **profilu publikování Get**. To se stáhnout soubor .publishsettings na místní jednotku.
2. Otevřete soubor .publishsettings v editoru XML nebo textovém editoru a najděte `<publishProfile>` element obsahující `publishMethod="FTP"`. By měl vypadat jako následující:
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. Všimněte si, že webová aplikace `publishProfile` nastavení mapu, aby správce webu FileZilla nastavení následujícím způsobem:

* `publishUrl`je stejný jako **název hostitele FTP**, hodnotu nastavenou v **hostitele**.
* `publishMethod="FTP"`znamená, že nastavíte **protokol** k **FTP - File Transfer Protocol**, a **šifrování** k **pomocí protokolu FTP prostý**.
* `userName`a `userPWD` jsou klíče pro skutečné uživatelské jméno a heslo hodnoty, které jste zadali, když resetujete přihlašovací údaje nasazení. `userName`je stejný jako **nasazení / FTP uživatele**. Mapují na **uživatele** a **heslo** v FileZilla.
* `ftpPassiveMode="True"`znamená, že tento server FTP použije pasivní přenos FTP; Vyberte **pasivní** na **nastavení přenosu** kartě.

#### <a name="configure-the-web-app-to-host-a-java-application"></a>Konfigurovat webovou aplikaci k hostování aplikace Java
Před publikováním aplikace, budete muset změnit některá nastavení konfigurace tak, aby webové aplikace může hostovat aplikace Java.

1. Na portálu classic přejděte na webovou aplikaci **řídicí panel** a klikněte na tlačítko **konfigurace**. Na **konfigurace** stránky, zadejte následující nastavení.
2. V **verzi Javy** výchozí hodnota je **vypnout**; vyberte verzi jazyka Java cíle vaší aplikace; například 1.7.0_51. Až to uděláte, také zkontrolujte, zda **webový kontejner** je nastaven na verzi serveru Tomcat.
3. V **výchozí dokumenty**index.jsp a přidejte ho přesunout na začátek seznamu. (Výchozí soubor pro webové aplikace je hostingstart.html.)
4. Klikněte na **Uložit**.

#### <a name="publish-your-application-using-kudu"></a>Publikování aplikace pomocí modulu Kudu
Jeden způsob, jak publikovat aplikaci je pomocí konzoly pro ladění Kudu integrovaný do Azure. Kudu se označuje jako stabilní a v souladu s App Service Web Apps a Tomcat Server. Můžete přístup do konzoly pro webovou aplikaci tak, že přejde na adresu URL v následujícím formátu:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Pro tento postup je konzola Kudu umístěna na následující adrese URL; Přejděte do tohoto umístění:
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. V hlavní nabídce vyberte **ladění konzoly > CMD**.
3. V konzole příkazového řádku, přejděte na `/site/wwwroot` (nebo klikněte na tlačítko `site`, pak `wwwroot` v zobrazení adresáře v horní části stránky):
   
    `cd /site/wwwroot`
4. Po zadání **verzi Javy**, Tomcat server měli vytvořit adresáře webapps. V příkazovém řádku konzoly přejděte do adresáře webapps:
   
    `mkdir webapps`
   
    `cd webapps`
5. Přetáhněte JSPHello.war z `<project-path>/JSPHello/src/` a umístěte jej do zobrazení adresáře Kudu pod `/site/wwwroot/webapps`. Není přetáhněte jej do oblasti "Přetáhněte sem odeslání a zip", protože Tomcat bude rozbalte ho.
   
   ![][8]

V první JSPHello.war se zobrazí v oblasti directory samostatně:

  ![][9]

V krátkém čase (pravděpodobně méně než 5 minut) bude Tomcat Server rozbalte soubor WAR do adresář rozbalené JSPHello. Klikněte na kořenový adresář zda index.jsp má byla rozbalené a zkopírovat existuje. Pokud ano, přejděte zpět do adresáře webapps zobrazíte, zda byl vytvořen rozbalené JSPHello adresáře. Pokud se tyto položky nezobrazí, počkejte a opakujte.

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a>Publikování aplikace pomocí FileZilla (volitelné)
Jiný nástroj, který slouží k publikování aplikace je FileZilla oblíbených klienta FTP třetích stran s praktické, grafické uživatelské rozhraní. Můžete stáhnout a nainstalovat FileZilla z [http://filezilla-project.org/](http://filezilla-project.org/) Pokud již jste ji. Další informace o používání klienta najdete v tématu [FileZilla dokumentace](https://wiki.filezilla-project.org/Documentation) a tuto položku blogu na [klienti FTP – část 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. V FileZilla, klikněte na **soubor > Správce webu**.
2. V **správce webu** dialogové okno, klikněte na tlačítko **nové lokality**. Nový prázdný web FTP se zobrazí v **vyberte položku** zobrazení výzvy k zadání názvu. Tento postup, pojmenujte ji `AzureWebDemo-FTP`.
   
    Na **Obecné** určete následující nastavení:
   
   * **Hostitele:** Enter **název hostitele FTP** který jste zkopírovali z řídicího panelu.
   * **Port:** (necháte prázdné, jak toto je pasivní přenos a server určí port, který se použít.)
   * **Protokol:** protokol pro přenos souborů FTP
   * **Šifrování:** použít prostý FTP
   * **Typ přihlášení:** normální
   * **Uživatel:** zadejte nasazení / FTP uživatele, který jste zkopírovali z řídicího panelu. Toto je úplná username FTP, který má formulář *webappname\username*.
   * **Heslo:** zadejte heslo, kterou jste zadali, když nastavíte přihlašovací údaje nasazení.
     
     Na **nastavení přenosu** vyberte **pasivní**.
3. Klikněte na **Připojit**. Pokud úspěšné, je FileZilla konzoly se zobrazí `Status: Connected` zprávu a problém `LIST` příkaz Zobrazit obsah adresáře.
4. V **místní** lokality panelů, vyberte zdrojový adresář, ve kterém se nachází soubor JSPHello.war; cesta bude vypadat podobně jako následující:
   
    `<project-path>/JSPHello/src/`
5. V **vzdáleného** lokality panelů, vyberte cílovou složku. Budete nasazovat soubor WAR `webapps` adresář v kořenovém adresáři webové aplikace. Přejděte na `/site/wwwroot`, klikněte pravým tlačítkem na `wwwroot`a vyberte **vytvořit adresář**. Název adresáře `webapps` a zadejte tento adresář.
6. Přenos JSPHello.war k `/site/wwwroot/webapps`. Vyberte JSPHello.war v **místní** soubor seznamu, klikněte na něj pravým tlačítkem myši a vyberte **nahrát**. Měli byste vidět se zobrazí v `/site/wwwroot/webapps`.
7. Po zkopírování JSPHello.war do adresáře webapps, bude automaticky rozbalte Tomcat Server (rozbalte) soubory v souboru WAR. I když Tomcat Server začne rozbalování téměř okamžitě, může trvat dlouho čas (pravděpodobně hodiny) pro soubory se objeví v klientovi FTP.

#### <a name="run-the-hello-world-application-on-the-web-app"></a>Spuštění aplikace Hello World na webové aplikace
1. Po nahrát soubor WAR a ověřit, že Tomcat server vytvořil rozbalené `JSPHello` adresáře, procházet a `http://webdemowebapp.azurewebsites.net/JSPHello` ke spuštění aplikace.
   
   > **Poznámka:** Pokud kliknete na tlačítko **Procházet** z klasického portálu, může získat výchozí webovou stránku, oznámením "této webové aplikace Java na základě úspěšně vytvořila." Možná budete muset aktualizovat webovou stránku, chcete-li zobrazit výstup aplikace místo výchozí webovou stránku.
   > 
   > 
2. Když je aplikace spuštěná, byste měli vidět na webové stránce s následující výstup:
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a>Vyčištění prostředků Azure
Tento postup vytvoří webové aplikace služby App Service. Také existuje bude platit pro prostředek. Pokud budete chtít pokračovat v používání webové aplikace pro testování nebo pro vývoj, měli byste zvážit, zastavení nebo odstranění. Webovou aplikaci, která byla zastavena stále zpoplatněná malé poplatků, ale můžete ji restartovat kdykoli. Odstranění webové aplikace vymaže všechna data, které jste odeslali do ní.

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
