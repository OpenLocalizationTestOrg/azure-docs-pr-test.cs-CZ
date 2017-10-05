---
title: "Pokročilá konfigurace a rozšíření Azure App Service webové aplikace"
description: "Pomocí deklarace Transformation(XDT) dokumentu XML k transformaci soubor ApplicationHost.config ve vaší webové aplikace služby Azure App Service a přidejte pro soukromá rozšíření povolí správu vlastní akce."
author: cephalin
writer: cephalin
editor: mollybos
manager: erikre
services: app-service
documentationcenter: 
ms.assetid: b441a286-ef38-4abc-b102-cdb249baf5bc
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.openlocfilehash: 314d3a954e712b829e7cf5eb37b23b31670f976b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a><span data-ttu-id="f3458-103">Pokročilá konfigurace a rozšíření Azure App Service webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f3458-103">Azure App Service web app advanced config and extensions</span></span>
<span data-ttu-id="f3458-104">Pomocí [transformace dokumentů XML](http://msdn.microsoft.com/library/dd465326.aspx) deklarace (XDT), můžete převést [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) souborů ve vaší webové aplikaci ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f3458-104">By using [XML Document Transformation](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) declarations, you can transform the [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) file in your web app in Azure App Service.</span></span> <span data-ttu-id="f3458-105">Deklarace XDT můžete taky přidat pro soukromá rozšíření povolit akce správy vlastní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3458-105">You can also use XDT declarations to add private extensions to enable custom web app administration actions.</span></span> <span data-ttu-id="f3458-106">Tento článek obsahuje rozšíření aplikace webového správce PHP ukázkové, který umožňuje správu nastavení PHP prostřednictvím webového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f3458-106">This article includes a sample PHP Manager web app extension that enables management of PHP settings through a web interface.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="f3458-107"><a id="transform"></a>Pokročilá konfigurace prostřednictvím ApplicationHost.config</span><span class="sxs-lookup"><span data-stu-id="f3458-107"><a id="transform"></a>Advanced configuration through ApplicationHost.config</span></span>
<span data-ttu-id="f3458-108">Platforma služby App Service poskytuje flexibilitu a řízení pro konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3458-108">The App Service platform provides flexibility and control for web app configuration.</span></span> <span data-ttu-id="f3458-109">I když není k dispozici pro přímé úpravy ve službě App Service standardní konfigurační soubor ApplicationHost.config služby IIS, platformu podporuje deklarativní model transformace ApplicationHost.config založené na XML dokumentu transformaci (XDT).</span><span class="sxs-lookup"><span data-stu-id="f3458-109">Although the standard IIS ApplicationHost.config configuration file is not available for direct editing in App Service, the platform supports a declarative ApplicationHost.config transform model based on XML Document Transformation (XDT).</span></span>

<span data-ttu-id="f3458-110">Tato funkce transformace využít, vytvořte soubor ApplicationHost.xdt s XDT obsahu a umístěte do kořenové lokality (d:\home\site) v [Kudu konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="f3458-110">To leverage this transform functionality, you create an ApplicationHost.xdt file with XDT content and place under the site root (d:\home\site) in the [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span></span> <span data-ttu-id="f3458-111">Musíte restartovat webovou aplikaci pro změny vstoupily v platnost.</span><span class="sxs-lookup"><span data-stu-id="f3458-111">You may need to restart the Web App for changes to take effect.</span></span>

<span data-ttu-id="f3458-112">Následující ukázka applicationHost.xdt ukazuje, jak přidat novou proměnnou vlastního prostředí do webové aplikace, která se používá PHP 5.4.</span><span class="sxs-lookup"><span data-stu-id="f3458-112">The following applicationHost.xdt sample shows how to add a new custom environment variable to a web app that uses PHP 5.4.</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <fastCgi>
      <application>
        <environmentVariables>
          <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
        </environmentVariables>
      </application>
    </fastCgi>
  </system.webServer>
</configuration>
```

<span data-ttu-id="f3458-113">Soubor protokolu s transform stav a podrobnosti o je k dispozici z kořenového adresáře serveru FTP v rámci LogFiles\Transform.</span><span class="sxs-lookup"><span data-stu-id="f3458-113">A log file with transform status and details is available from the FTP root under LogFiles\Transform.</span></span>

<span data-ttu-id="f3458-114">Další ukázky naleznete v části [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span><span class="sxs-lookup"><span data-stu-id="f3458-114">For additional samples, see [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span></span>

<span data-ttu-id="f3458-115">**Poznámka**</span><span class="sxs-lookup"><span data-stu-id="f3458-115">**Note**</span></span><br />
<span data-ttu-id="f3458-116">Elementy ze seznamu modulů podle `system.webServer` nemůže být odebrány nebo přeuspořádány, ale je možné, přidané do seznamu.</span><span class="sxs-lookup"><span data-stu-id="f3458-116">Elements from the list of modules under `system.webServer` cannot be removed or reordered, but additions to the list are possible.</span></span>

## <span data-ttu-id="f3458-117"><a id="extend"></a>Rozšíření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f3458-117"><a id="extend"></a> Extend your web app</span></span>
### <span data-ttu-id="f3458-118"><a id="overview"></a>Přehled rozšíření privátní webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f3458-118"><a id="overview"></a> Overview of private web app extensions</span></span>
<span data-ttu-id="f3458-119">App Service podporuje rozšíření webové aplikace jako bod rozšiřitelnosti pro akce správy.</span><span class="sxs-lookup"><span data-stu-id="f3458-119">App Service supports web app extensions as an extensibility point for administrative actions.</span></span> <span data-ttu-id="f3458-120">Ve skutečnosti některé funkce služby App Service jsou implementované jako předem nainstalovaná rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f3458-120">In fact, some App Service platform features are implemented as pre-installed extensions.</span></span> <span data-ttu-id="f3458-121">Při předinstalovány platforma rozšíření nemůže být upraven, můžete vytvořit a nakonfigurovat pro soukromá rozšíření pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f3458-121">While the pre-installed platform extensions cannot be modified, you can create and configure private extensions for your own web app.</span></span> <span data-ttu-id="f3458-122">Tato funkce také závisí na XDT deklarace.</span><span class="sxs-lookup"><span data-stu-id="f3458-122">This functionality also relies on XDT declarations.</span></span> <span data-ttu-id="f3458-123">Klíčové kroky pro vytvoření rozšíření privátní webové aplikace jsou následující:</span><span class="sxs-lookup"><span data-stu-id="f3458-123">The key steps for creating a private web app extension are the following:</span></span>

1. <span data-ttu-id="f3458-124">Webové aplikace rozšíření **obsah**: vytvoření žádné webové aplikace podporované službou App Service</span><span class="sxs-lookup"><span data-stu-id="f3458-124">Web app extension **content**: create any web application supported by App Service</span></span>
2. <span data-ttu-id="f3458-125">Webové aplikace rozšíření **deklarace**: Vytvořte soubor ApplicationHost.xdt</span><span class="sxs-lookup"><span data-stu-id="f3458-125">Web app extension **declaration**: create an ApplicationHost.xdt file</span></span>
3. <span data-ttu-id="f3458-126">Webové aplikace rozšíření **nasazení**: umístit obsah ve složce SiteExtensions v`root`</span><span class="sxs-lookup"><span data-stu-id="f3458-126">Web app extension **deployment**: place content in the SiteExtensions folder under `root`</span></span>

<span data-ttu-id="f3458-127">Interní odkazy pro webovou aplikaci by měla odkazovat na relativní cestu k aplikaci zadaný v souboru ApplicationHost.xdt cestu.</span><span class="sxs-lookup"><span data-stu-id="f3458-127">Internal links for the web app should point to a path relative to the application path specified in the ApplicationHost.xdt file.</span></span> <span data-ttu-id="f3458-128">Všechny změny do souboru ApplicationHost.xdt vyžaduje recyklaci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3458-128">Any change to the ApplicationHost.xdt file requires a web app recycle.</span></span>

<span data-ttu-id="f3458-129">**Poznámka:**: Další informace o těchto klíčové prvky je k dispozici na [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span><span class="sxs-lookup"><span data-stu-id="f3458-129">**Note**: Additional information for these key elements is available at [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span></span>

<span data-ttu-id="f3458-130">Podrobný příklad je obsažena znázorňující kroky pro vytvoření a povolení rozšíření privátní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3458-130">A detailed example is included to illustrate the steps for creating and enabling a private web app extension.</span></span> <span data-ttu-id="f3458-131">Zdrojový kód v příkladu správce PHP, která následuje si můžete stáhnout z [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span><span class="sxs-lookup"><span data-stu-id="f3458-131">The source code for the PHP Manager example that follows can be downloaded from [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span></span>

### <span data-ttu-id="f3458-132"><a id="SiteSample"></a>Příklad rozšíření webové aplikace: Správce PHP</span><span class="sxs-lookup"><span data-stu-id="f3458-132"><a id="SiteSample"></a> Web app extension example: PHP Manager</span></span>
<span data-ttu-id="f3458-133">Správce PHP je rozšíření webové aplikace, která správcům webové aplikace umožňuje snadno zobrazit a nakonfigurovat jejich nastavení PHP pomocí webové rozhraní, aniž by museli přímo upravit soubory INI PHP.</span><span class="sxs-lookup"><span data-stu-id="f3458-133">PHP Manager is a web app extension that allows web app administrators to easily view and configure their PHP settings using a web interface instead of having to modify PHP .ini files directly.</span></span> <span data-ttu-id="f3458-134">Běžné konfigurační soubory pro jazyk PHP zahrnout soubor php.ini umístěný ve složce Program Files a. user.ini umístěného v kořenové složce webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3458-134">Common configuration files for PHP include the php.ini file located under Program Files and the .user.ini file located in the root folder of your web app.</span></span> <span data-ttu-id="f3458-135">Vzhledem k tomu, že soubor php.ini není přímo upravovat na platformě App Service, používá rozšíření správce PHP. user.ini souboru použít změny nastavení.</span><span class="sxs-lookup"><span data-stu-id="f3458-135">Since the php.ini file is not directly editable on the App Service platform, the PHP Manager extension uses the .user.ini file to apply setting changes.</span></span>

#### <span data-ttu-id="f3458-136"><a id="PHPwebapp"></a>Správce PHP webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f3458-136"><a id="PHPwebapp"></a> The PHP Manager web application</span></span>
<span data-ttu-id="f3458-137">Toto je domovské stránce Správce PHP nasazení:</span><span class="sxs-lookup"><span data-stu-id="f3458-137">The following is the home page of the PHP Manager deployment:</span></span>

![TransformSitePHPUI][TransformSitePHPUI]

<span data-ttu-id="f3458-139">Jak je vidět, rozšíření webové aplikace je stejně jako regulární webovou aplikaci, ale s další soubor ApplicationHost.xdt umístěné v kořenové složce webové aplikace (Další informace o souboru ApplicationHost.xdt jsou k dispozici v další části tohoto článku).</span><span class="sxs-lookup"><span data-stu-id="f3458-139">As you can see, a web app extension is just like a regular web application, but with an additional ApplicationHost.xdt file placed in the root folder of the web app (more details about the ApplicationHost.xdt file are available in the next section of this article).</span></span>

<span data-ttu-id="f3458-140">Správce PHP rozšíření byl vytvořen pomocí šablony Visual Studio ASP.NET MVC 4 webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3458-140">The PHP Manager extension was created using the Visual Studio ASP.NET MVC 4 Web Application template.</span></span> <span data-ttu-id="f3458-141">V Průzkumníku řešení následující zobrazení ukazuje strukturu rozšíření správce PHP.</span><span class="sxs-lookup"><span data-stu-id="f3458-141">The following view from Solution Explorer shows the structure of the PHP Manager extension.</span></span>

![TransformSiteSolEx][TransformSiteSolEx]

<span data-ttu-id="f3458-143">Pouze speciální logiku potřebné pro soubor vstupně-výstupních operací je označíte, kde je umístěn daný adresář wwwroot webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3458-143">The only special logic needed for file I/O is to indicate where the wwwroot directory of the web app is located.</span></span> <span data-ttu-id="f3458-144">Jak ukazuje následující příklad kódu, v prostředí proměnné "HOME" znamená kořenová cesta webové aplikace a cestu wwwroot lze sestavit připojením "site\wwwroot":</span><span class="sxs-lookup"><span data-stu-id="f3458-144">As the following code example shows, the environment variable "HOME" indicates the web app's root path, and the wwwroot path can be constructed by appending "site\wwwroot":</span></span>

```csharp
/// <summary>
/// Gives the location of the .user.ini file, even if one doesn't exist yet
/// </summary>
private static string GetUserSettingsFilePath()
{
  var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
  if (rootPath == null)
  {
    rootPath = System.IO.Path.GetTempPath(); // For testing purposes
  };
  var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
  return userSettingsFile;
}
```


<span data-ttu-id="f3458-145">Až budete mít cesta k adresáři, můžete ke čtení a zápis do souborů běžným sdíleným vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="f3458-145">After you have the directory path, you can use regular file I/O operations to read and write to files.</span></span>

<span data-ttu-id="f3458-146">Jeden bod upozornění se rozšíření webové aplikace, pokud o zpracování interní odkazy.</span><span class="sxs-lookup"><span data-stu-id="f3458-146">One point of caution with web app extensions regards the handling of internal links.</span></span>  <span data-ttu-id="f3458-147">Pokud máte jakékoli odkazy v soubory ve formátu HTML, která umožňují absolutní cesty k interní odkazy na webové aplikace, je třeba zkontrolovat, že tyto odkazy se přidá jako předpona názvem svého rozšíření jako kořenového adresáře.</span><span class="sxs-lookup"><span data-stu-id="f3458-147">If you have any links in your HTML files that give absolute paths to internal links on your web app, you must ensure those links are prepended with your extension name as your root.</span></span> <span data-ttu-id="f3458-148">To je nutné, protože kořenová pro toto rozšíření je nyní "/`[your-extension-name]`/" místo se musí být právě "/", tak interní odkazy příslušným způsobem aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="f3458-148">This is needed because the root for your extension is now "/`[your-extension-name]`/" rather than being just "/", so any internal links must be updated accordingly.</span></span> <span data-ttu-id="f3458-149">Předpokládejme například, že váš kód obsahuje odkaz na následující:</span><span class="sxs-lookup"><span data-stu-id="f3458-149">For example, suppose your code includes a link to the following:</span></span>

`"<a href="/Home/Settings">PHP Settings</a>"`

<span data-ttu-id="f3458-150">Při odkazu je součástí rozšíření webové aplikace, musí být odkaz v následující podobě:</span><span class="sxs-lookup"><span data-stu-id="f3458-150">When the link is part of a web app extension, the link must be in the following form:</span></span>

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

<span data-ttu-id="f3458-151">Tento požadavek můžete obejít a to buď pomocí pouze relativní cesty v rámci webové aplikace nebo v případě aplikací ASP.NET, pomocí `@Html.ActionLink` metoda, kterou vytvoří na příslušné odkazy.</span><span class="sxs-lookup"><span data-stu-id="f3458-151">You can work around this requirement by either using only relative paths within your web application, or in the case of ASP.NET applications, by using the `@Html.ActionLink` method which creates the appropriate links for you.</span></span>

#### <span data-ttu-id="f3458-152"><a id="XDT"></a>Soubor applicationHost.xdt</span><span class="sxs-lookup"><span data-stu-id="f3458-152"><a id="XDT"></a> The applicationHost.xdt file</span></span>
<span data-ttu-id="f3458-153">Kód pro rozšíření vaší webové aplikace přejde pod %HOME%\SiteExtensions\[si název rozšíření].</span><span class="sxs-lookup"><span data-stu-id="f3458-153">The code for your web app extension goes under %HOME%\SiteExtensions\[your-extension-name].</span></span> <span data-ttu-id="f3458-154">Zavoláme vám to kořenového rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f3458-154">We'll call this the extension root.</span></span>  

<span data-ttu-id="f3458-155">Registrace rozšíření vaší webové aplikace pomocí souboru applicationHost.config, musíte umístit soubor s názvem ApplicationHost.xdt v kořenu rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f3458-155">To register your web app extension with the applicationHost.config file, you need to place a file called ApplicationHost.xdt in the extension root.</span></span> <span data-ttu-id="f3458-156">Obsah souboru ApplicationHost.xdt by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="f3458-156">The content of the ApplicationHost.xdt file should be as follows:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <sites>
      <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
        <!-- NOTE: Add your extension name in the application paths below -->
        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
          <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
        </application>
      </site>
    </sites>
  </system.applicationHost>
</configuration>
```

<span data-ttu-id="f3458-157">Název, který jste vybrali jako název rozšíření by měl mít stejný název jako kořenové složky rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f3458-157">The name you select as your extension name should have the same name as your extension root folder.</span></span>

<span data-ttu-id="f3458-158">To má za následek přidání nové aplikace cestu k `system.applicationHost` seznam stránek v lokalitě SCM.</span><span class="sxs-lookup"><span data-stu-id="f3458-158">This has the effect of adding a new application path to the `system.applicationHost` sites list under the SCM site.</span></span> <span data-ttu-id="f3458-159">Lokalita SCM je koncový bod správy lokality konkrétní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f3458-159">The SCM site is a site administration end point with specific access credentials.</span></span> <span data-ttu-id="f3458-160">Obsahuje adresu URL `https://[your-site-name].scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="f3458-160">It has the URL `https://[your-site-name].scm.azurewebsites.net`.</span></span>  

```xml
<system.applicationHost>
  ...       
  <sites>
    <site name="~1[your-website]" id="1716402716">
      <bindings>
        <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
        <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
      </bindings>
      <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
      <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
      <logFile logSiteId="false" />
      <application path="/" applicationPool="[your-website]">
        <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
      </application>
      <!-- Note the custom changes that go here -->
      <application path="/[your-extension-name]" applicationPool="[your-website]">
        <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
      </application>
    </site>
  </sites>
  ... 
</system.applicationHost>
```

### <span data-ttu-id="f3458-161"><a id="deploy"></a>Nasazení rozšíření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f3458-161"><a id="deploy"></a> Web app extension deployment</span></span>
<span data-ttu-id="f3458-162">K instalaci rozšíření vaší webové aplikace, můžete pomocí protokolu FTP zkopírujte všechny soubory vaší webové aplikace `\SiteExtensions\[your-extension-name]` složku, ve kterém chcete nainstalovat rozšíření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3458-162">To install your web app extension, you can use FTP to copy all the files of your web application to the `\SiteExtensions\[your-extension-name]` folder of the web app on which you want to install the extension.</span></span>  <span data-ttu-id="f3458-163">Nezapomeňte zkopírovat soubor ApplicationHost.xdt do tohoto umístění také.</span><span class="sxs-lookup"><span data-stu-id="f3458-163">Be sure to copy the ApplicationHost.xdt file to this location as well.</span></span> <span data-ttu-id="f3458-164">Restartujte vaší webové aplikace, která rozšíření aktivuje.</span><span class="sxs-lookup"><span data-stu-id="f3458-164">Restart your web app to enable the extension.</span></span>

<span data-ttu-id="f3458-165">Měli byste mít moci zobrazit vaše rozšíření webové aplikace na:</span><span class="sxs-lookup"><span data-stu-id="f3458-165">You should be able to see your web app extension at:</span></span>

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

<span data-ttu-id="f3458-166">Všimněte si, že adresa URL vypadá stejně jako adresu URL pro webovou aplikaci, s tím rozdílem, že se používá protokol HTTPS a obsahuje ".scm".</span><span class="sxs-lookup"><span data-stu-id="f3458-166">Note that the URL looks just like the URL for your web app, except that it uses HTTPS and contains ".scm".</span></span>

<span data-ttu-id="f3458-167">Je možné zakázat všechna rozšíření privátní (není předem nainstalovaná) pro webovou aplikaci během vývoje a vyšetřování přidáním nastavení aplikace s klíčem `WEBSITE_PRIVATE_EXTENSIONS` a hodnota `0`.</span><span class="sxs-lookup"><span data-stu-id="f3458-167">It is possible to disable all private (not pre-installed) extensions for your web app during development and investigations by adding an app settings with the key `WEBSITE_PRIVATE_EXTENSIONS` and a value of `0`.</span></span>

> [!NOTE]
> <span data-ttu-id="f3458-168">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f3458-168">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="f3458-169">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="f3458-169">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="f3458-170">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="f3458-170">What's changed</span></span>
* <span data-ttu-id="f3458-171">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="f3458-171">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

