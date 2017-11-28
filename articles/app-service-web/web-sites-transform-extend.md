---
title: "aaaAzure webové aplikace App Service pokročilá konfigurace a rozšíření"
description: "Použijte soubor ApplicationHost.config Transformation(XDT) dokumentu XML deklarace tootransform hello ve vaší službě Azure App Service webové aplikace a tooadd pro soukromá rozšíření tooenable vlastní akce správy."
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
ms.openlocfilehash: 873347ac13113d1ac989cba29128382c81dcfcca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a><span data-ttu-id="de7d7-103">Pokročilá konfigurace a rozšíření Azure App Service webové aplikace</span><span class="sxs-lookup"><span data-stu-id="de7d7-103">Azure App Service web app advanced config and extensions</span></span>
<span data-ttu-id="de7d7-104">Pomocí [transformace dokumentů XML](http://msdn.microsoft.com/library/dd465326.aspx) deklarace (XDT), můžete převést hello [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) souborů ve vaší webové aplikaci ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="de7d7-104">By using [XML Document Transformation](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) declarations, you can transform hello [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) file in your web app in Azure App Service.</span></span> <span data-ttu-id="de7d7-105">Můžete také použít XDT deklarace tooadd pro soukromá rozšíření tooenable vlastní webové aplikace akce správy.</span><span class="sxs-lookup"><span data-stu-id="de7d7-105">You can also use XDT declarations tooadd private extensions tooenable custom web app administration actions.</span></span> <span data-ttu-id="de7d7-106">Tento článek obsahuje rozšíření aplikace webového správce PHP ukázkové, který umožňuje správu nastavení PHP prostřednictvím webového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="de7d7-106">This article includes a sample PHP Manager web app extension that enables management of PHP settings through a web interface.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="de7d7-107"><a id="transform"></a>Pokročilá konfigurace prostřednictvím ApplicationHost.config</span><span class="sxs-lookup"><span data-stu-id="de7d7-107"><a id="transform"></a>Advanced configuration through ApplicationHost.config</span></span>
<span data-ttu-id="de7d7-108">Hello platformě App Service poskytuje flexibilitu a řízení pro konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de7d7-108">hello App Service platform provides flexibility and control for web app configuration.</span></span> <span data-ttu-id="de7d7-109">I když hello standardní konfigurační soubor ApplicationHost.config služby IIS není k dispozici pro přímé úpravy ve službě App Service, hello platforma podporuje deklarativní model transformace ApplicationHost.config založené na XML dokumentu transformaci (XDT).</span><span class="sxs-lookup"><span data-stu-id="de7d7-109">Although hello standard IIS ApplicationHost.config configuration file is not available for direct editing in App Service, hello platform supports a declarative ApplicationHost.config transform model based on XML Document Transformation (XDT).</span></span>

<span data-ttu-id="de7d7-110">tooleverage tato funkce transformace, vytvořte soubor ApplicationHost.xdt s XDT obsahu a umístěte do kořenové lokality hello (d:\home\site) v hello [Kudu konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="de7d7-110">tooleverage this transform functionality, you create an ApplicationHost.xdt file with XDT content and place under hello site root (d:\home\site) in hello [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span></span> <span data-ttu-id="de7d7-111">Toorestart hello webové aplikace může být nutné pro změny tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="de7d7-111">You may need toorestart hello Web App for changes tootake effect.</span></span>

<span data-ttu-id="de7d7-112">Následující ukázka applicationHost.xdt Hello ukazuje, jak tooadd nové proměnné tooa vlastní prostředí webové aplikaci, která používá PHP 5.4.</span><span class="sxs-lookup"><span data-stu-id="de7d7-112">hello following applicationHost.xdt sample shows how tooadd a new custom environment variable tooa web app that uses PHP 5.4.</span></span>

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

<span data-ttu-id="de7d7-113">Soubor protokolu s transform stav a podrobnosti o je k dispozici z kořenového hello FTP v rámci LogFiles\Transform.</span><span class="sxs-lookup"><span data-stu-id="de7d7-113">A log file with transform status and details is available from hello FTP root under LogFiles\Transform.</span></span>

<span data-ttu-id="de7d7-114">Další ukázky naleznete v části [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span><span class="sxs-lookup"><span data-stu-id="de7d7-114">For additional samples, see [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span></span>

<span data-ttu-id="de7d7-115">**Poznámka**</span><span class="sxs-lookup"><span data-stu-id="de7d7-115">**Note**</span></span><br />
<span data-ttu-id="de7d7-116">Elementy z hello seznam modulů v části `system.webServer` nemůže být odebrány nebo přeuspořádány, ale je možné, dodatky toohello seznamu.</span><span class="sxs-lookup"><span data-stu-id="de7d7-116">Elements from hello list of modules under `system.webServer` cannot be removed or reordered, but additions toohello list are possible.</span></span>

## <span data-ttu-id="de7d7-117"><a id="extend"></a>Rozšíření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="de7d7-117"><a id="extend"></a> Extend your web app</span></span>
### <span data-ttu-id="de7d7-118"><a id="overview"></a>Přehled rozšíření privátní webové aplikace</span><span class="sxs-lookup"><span data-stu-id="de7d7-118"><a id="overview"></a> Overview of private web app extensions</span></span>
<span data-ttu-id="de7d7-119">App Service podporuje rozšíření webové aplikace jako bod rozšiřitelnosti pro akce správy.</span><span class="sxs-lookup"><span data-stu-id="de7d7-119">App Service supports web app extensions as an extensibility point for administrative actions.</span></span> <span data-ttu-id="de7d7-120">Ve skutečnosti některé funkce služby App Service jsou implementované jako předem nainstalovaná rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de7d7-120">In fact, some App Service platform features are implemented as pre-installed extensions.</span></span> <span data-ttu-id="de7d7-121">Při hello předinstalovány platforma rozšíření nemůže být upraven, můžete vytvořit a nakonfigurovat pro soukromá rozšíření pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="de7d7-121">While hello pre-installed platform extensions cannot be modified, you can create and configure private extensions for your own web app.</span></span> <span data-ttu-id="de7d7-122">Tato funkce také závisí na XDT deklarace.</span><span class="sxs-lookup"><span data-stu-id="de7d7-122">This functionality also relies on XDT declarations.</span></span> <span data-ttu-id="de7d7-123">Hello klíčové kroky pro vytvoření rozšíření privátní webové aplikace jsou hello následující:</span><span class="sxs-lookup"><span data-stu-id="de7d7-123">hello key steps for creating a private web app extension are hello following:</span></span>

1. <span data-ttu-id="de7d7-124">Webové aplikace rozšíření **obsah**: vytvoření žádné webové aplikace podporované službou App Service</span><span class="sxs-lookup"><span data-stu-id="de7d7-124">Web app extension **content**: create any web application supported by App Service</span></span>
2. <span data-ttu-id="de7d7-125">Webové aplikace rozšíření **deklarace**: Vytvořte soubor ApplicationHost.xdt</span><span class="sxs-lookup"><span data-stu-id="de7d7-125">Web app extension **declaration**: create an ApplicationHost.xdt file</span></span>
3. <span data-ttu-id="de7d7-126">Webové aplikace rozšíření **nasazení**: umístit obsah složky SiteExtensions hello v části`root`</span><span class="sxs-lookup"><span data-stu-id="de7d7-126">Web app extension **deployment**: place content in hello SiteExtensions folder under `root`</span></span>

<span data-ttu-id="de7d7-127">Interní odkazy pro webovou aplikaci hello by měla odkazovat tooa cesta relativní toohello cestu aplikace zadané v souboru ApplicationHost.xdt hello.</span><span class="sxs-lookup"><span data-stu-id="de7d7-127">Internal links for hello web app should point tooa path relative toohello application path specified in hello ApplicationHost.xdt file.</span></span> <span data-ttu-id="de7d7-128">Všechny změny toohello ApplicationHost.xdt soubor vyžaduje recyklaci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de7d7-128">Any change toohello ApplicationHost.xdt file requires a web app recycle.</span></span>

<span data-ttu-id="de7d7-129">**Poznámka:**: Další informace o těchto klíčové prvky je k dispozici na [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span><span class="sxs-lookup"><span data-stu-id="de7d7-129">**Note**: Additional information for these key elements is available at [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span></span>

<span data-ttu-id="de7d7-130">Podrobný příklad je součástí tooillustrate hello kroky pro vytvoření a povolení rozšíření privátní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de7d7-130">A detailed example is included tooillustrate hello steps for creating and enabling a private web app extension.</span></span> <span data-ttu-id="de7d7-131">Hello zdrojového kódu pro příklad správce PHP hello způsobem si můžete stáhnout z [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span><span class="sxs-lookup"><span data-stu-id="de7d7-131">hello source code for hello PHP Manager example that follows can be downloaded from [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span></span>

### <span data-ttu-id="de7d7-132"><a id="SiteSample"></a>Příklad rozšíření webové aplikace: Správce PHP</span><span class="sxs-lookup"><span data-stu-id="de7d7-132"><a id="SiteSample"></a> Web app extension example: PHP Manager</span></span>
<span data-ttu-id="de7d7-133">Správce PHP je rozšíření webové aplikace, která umožňuje správci aplikace webového tooeasily zobrazení a konfigurace jejich nastavení PHP, místo nutnosti soubory INI PHP toomodify přímo pomocí webového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="de7d7-133">PHP Manager is a web app extension that allows web app administrators tooeasily view and configure their PHP settings using a web interface instead of having toomodify PHP .ini files directly.</span></span> <span data-ttu-id="de7d7-134">Běžné konfigurační soubory pro jazyk PHP zahrnout soubor php.ini hello umístěny ve složce Program Files and hello. user.ini umístěného v kořenové složce hello vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de7d7-134">Common configuration files for PHP include hello php.ini file located under Program Files and hello .user.ini file located in hello root folder of your web app.</span></span> <span data-ttu-id="de7d7-135">Vzhledem k tomu, že soubor php.ini hello není přímo upravovat na hello platformě App Service, hello rozšíření PHP správce používá hello. tooapply souboru user.ini změny nastavení.</span><span class="sxs-lookup"><span data-stu-id="de7d7-135">Since hello php.ini file is not directly editable on hello App Service platform, hello PHP Manager extension uses hello .user.ini file tooapply setting changes.</span></span>

#### <span data-ttu-id="de7d7-136"><a id="PHPwebapp"></a>Hello správce PHP webové aplikace</span><span class="sxs-lookup"><span data-stu-id="de7d7-136"><a id="PHPwebapp"></a> hello PHP Manager web application</span></span>
<span data-ttu-id="de7d7-137">Hello následuje hello domovské stránce hello správce PHP nasazení:</span><span class="sxs-lookup"><span data-stu-id="de7d7-137">hello following is hello home page of hello PHP Manager deployment:</span></span>

![TransformSitePHPUI][TransformSitePHPUI]

<span data-ttu-id="de7d7-139">Jak můžete vidět, je stejně jako regulární webovou aplikaci, ale s další soubor ApplicationHost.xdt umístit do kořenové složky hello hello webové aplikace (Další informace o souboru ApplicationHost.xdt hello jsou k dispozici v další části hello tohoto rozšíření webové aplikace článek).</span><span class="sxs-lookup"><span data-stu-id="de7d7-139">As you can see, a web app extension is just like a regular web application, but with an additional ApplicationHost.xdt file placed in hello root folder of hello web app (more details about hello ApplicationHost.xdt file are available in hello next section of this article).</span></span>

<span data-ttu-id="de7d7-140">Správce PHP rozšíření Hello byl vytvořen pomocí šablony Visual Studio MVC 4 webovou aplikaci ASP.NET hello.</span><span class="sxs-lookup"><span data-stu-id="de7d7-140">hello PHP Manager extension was created using hello Visual Studio ASP.NET MVC 4 Web Application template.</span></span> <span data-ttu-id="de7d7-141">Hello následující zobrazení v Průzkumníku řešení ukazuje hello strukturu hello správce PHP rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de7d7-141">hello following view from Solution Explorer shows hello structure of hello PHP Manager extension.</span></span>

![TransformSiteSolEx][TransformSiteSolEx]

<span data-ttu-id="de7d7-143">Hello jenom speciální logika potřebné pro soubor vstupně-výstupních operací je tooindicate kde hello adresář wwwroot hello webové aplikace se nachází.</span><span class="sxs-lookup"><span data-stu-id="de7d7-143">hello only special logic needed for file I/O is tooindicate where hello wwwroot directory of hello web app is located.</span></span> <span data-ttu-id="de7d7-144">Jako hello následující příklad ukazuje kód hello proměnné prostředí "HOME" označuje hello kořenová cesta webové aplikace a cestu hello wwwroot lze sestavit připojením "site\wwwroot":</span><span class="sxs-lookup"><span data-stu-id="de7d7-144">As hello following code example shows, hello environment variable "HOME" indicates hello web app's root path, and hello wwwroot path can be constructed by appending "site\wwwroot":</span></span>

```csharp
/// <summary>
/// Gives hello location of hello .user.ini file, even if one doesn't exist yet
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


<span data-ttu-id="de7d7-145">Až budete mít hello cesta k adresáři, můžete použít běžný soubor tooread vstupně-výstupních operací a zápisu toofiles.</span><span class="sxs-lookup"><span data-stu-id="de7d7-145">After you have hello directory path, you can use regular file I/O operations tooread and write toofiles.</span></span>

<span data-ttu-id="de7d7-146">Jeden bod upozornění se rozšíření webové aplikace, pokud o hello zpracování interní odkazy.</span><span class="sxs-lookup"><span data-stu-id="de7d7-146">One point of caution with web app extensions regards hello handling of internal links.</span></span>  <span data-ttu-id="de7d7-147">Pokud máte jakékoli odkazy v soubory ve formátu HTML, která umožňují absolutní cesty toointernal odkazy na webové aplikace, je třeba zkontrolovat, že tyto odkazy se přidá jako předpona názvem svého rozšíření jako kořenového adresáře.</span><span class="sxs-lookup"><span data-stu-id="de7d7-147">If you have any links in your HTML files that give absolute paths toointernal links on your web app, you must ensure those links are prepended with your extension name as your root.</span></span> <span data-ttu-id="de7d7-148">To je nutné, protože kořenová hello pro toto rozšíření je nyní "/`[your-extension-name]`/" místo se musí být právě "/", tak interní odkazy příslušným způsobem aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="de7d7-148">This is needed because hello root for your extension is now "/`[your-extension-name]`/" rather than being just "/", so any internal links must be updated accordingly.</span></span> <span data-ttu-id="de7d7-149">Například předpokládejme, že kód obsahuje toohello následující odkaz:</span><span class="sxs-lookup"><span data-stu-id="de7d7-149">For example, suppose your code includes a link toohello following:</span></span>

`"<a href="/Home/Settings">PHP Settings</a>"`

<span data-ttu-id="de7d7-150">Při propojení hello je součástí rozšíření webové aplikace, musí být odkaz hello hello následující formulář:</span><span class="sxs-lookup"><span data-stu-id="de7d7-150">When hello link is part of a web app extension, hello link must be in hello following form:</span></span>

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

<span data-ttu-id="de7d7-151">Tento požadavek můžete obejít buď pomocí pouze relativní cesty v rámci webové aplikace nebo v případě aplikací ASP.NET hello pomocí hello `@Html.ActionLink` metoda, kterou vytvoří hello příslušné odkazy.</span><span class="sxs-lookup"><span data-stu-id="de7d7-151">You can work around this requirement by either using only relative paths within your web application, or in hello case of ASP.NET applications, by using hello `@Html.ActionLink` method which creates hello appropriate links for you.</span></span>

#### <span data-ttu-id="de7d7-152"><a id="XDT"></a>soubor applicationHost.xdt Hello</span><span class="sxs-lookup"><span data-stu-id="de7d7-152"><a id="XDT"></a> hello applicationHost.xdt file</span></span>
<span data-ttu-id="de7d7-153">Hello kód pro rozšíření vaší webové aplikace přejde pod %HOME%\SiteExtensions\[si název rozšíření].</span><span class="sxs-lookup"><span data-stu-id="de7d7-153">hello code for your web app extension goes under %HOME%\SiteExtensions\[your-extension-name].</span></span> <span data-ttu-id="de7d7-154">Zavoláme vám tato rozšíření kořenové hello.</span><span class="sxs-lookup"><span data-stu-id="de7d7-154">We'll call this hello extension root.</span></span>  

<span data-ttu-id="de7d7-155">tooregister rozšíření vaší webové aplikace pomocí souboru applicationHost.config hello, budete potřebovat tooplace do souboru s názvem ApplicationHost.xdt v kořenovém rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="de7d7-155">tooregister your web app extension with hello applicationHost.config file, you need tooplace a file called ApplicationHost.xdt in hello extension root.</span></span> <span data-ttu-id="de7d7-156">Hello obsah souboru ApplicationHost.xdt hello by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="de7d7-156">hello content of hello ApplicationHost.xdt file should be as follows:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <sites>
      <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
        <!-- NOTE: Add your extension name in hello application paths below -->
        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
          <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
        </application>
      </site>
    </sites>
  </system.applicationHost>
</configuration>
```

<span data-ttu-id="de7d7-157">Hello název, který jste vybrali jako název rozšíření by měl mít hello stejný název jako kořenové složky rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de7d7-157">hello name you select as your extension name should have hello same name as your extension root folder.</span></span>

<span data-ttu-id="de7d7-158">Tato akce nemá vliv hello přidávání nové toohello cesta aplikace `system.applicationHost` seznam stránek v lokalitě SCM hello.</span><span class="sxs-lookup"><span data-stu-id="de7d7-158">This has hello effect of adding a new application path toohello `system.applicationHost` sites list under hello SCM site.</span></span> <span data-ttu-id="de7d7-159">lokalita SCM Hello je koncový bod správy lokality konkrétní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="de7d7-159">hello SCM site is a site administration end point with specific access credentials.</span></span> <span data-ttu-id="de7d7-160">Obsahuje adresu URL hello `https://[your-site-name].scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="de7d7-160">It has hello URL `https://[your-site-name].scm.azurewebsites.net`.</span></span>  

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
      <!-- Note hello custom changes that go here -->
      <application path="/[your-extension-name]" applicationPool="[your-website]">
        <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
      </application>
    </site>
  </sites>
  ... 
</system.applicationHost>
```

### <span data-ttu-id="de7d7-161"><a id="deploy"></a>Nasazení rozšíření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="de7d7-161"><a id="deploy"></a> Web app extension deployment</span></span>
<span data-ttu-id="de7d7-162">tooinstall rozšíření vaší webové aplikace, FTP toocopy můžete použít všechny soubory hello vaší webové aplikace toohello `\SiteExtensions\[your-extension-name]` složky hello webové aplikace, na kterém chcete tooinstall hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de7d7-162">tooinstall your web app extension, you can use FTP toocopy all hello files of your web application toohello `\SiteExtensions\[your-extension-name]` folder of hello web app on which you want tooinstall hello extension.</span></span>  <span data-ttu-id="de7d7-163">Zda toocopy hello ApplicationHost.xdt toothis umístění souboru také být.</span><span class="sxs-lookup"><span data-stu-id="de7d7-163">Be sure toocopy hello ApplicationHost.xdt file toothis location as well.</span></span> <span data-ttu-id="de7d7-164">Restartujte rozšíření hello tooenable vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="de7d7-164">Restart your web app tooenable hello extension.</span></span>

<span data-ttu-id="de7d7-165">Byste měli mít toosee rozšíření vaší webové aplikace na:</span><span class="sxs-lookup"><span data-stu-id="de7d7-165">You should be able toosee your web app extension at:</span></span>

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

<span data-ttu-id="de7d7-166">Všimněte si, že adresa URL vypadá stejně jako hello adresu URL pro webovou aplikaci, s tím rozdílem, že se používá protokol HTTPS a obsahuje ".scm" hello.</span><span class="sxs-lookup"><span data-stu-id="de7d7-166">Note that hello URL looks just like hello URL for your web app, except that it uses HTTPS and contains ".scm".</span></span>

<span data-ttu-id="de7d7-167">Je rozšíření všechny privátní možné toodisable (není předinstalovaným) pro webovou aplikaci během vývoje a vyšetřování přidáním nastavení aplikace s klíčem hello `WEBSITE_PRIVATE_EXTENSIONS` a hodnota `0`.</span><span class="sxs-lookup"><span data-stu-id="de7d7-167">It is possible toodisable all private (not pre-installed) extensions for your web app during development and investigations by adding an app settings with hello key `WEBSITE_PRIVATE_EXTENSIONS` and a value of `0`.</span></span>

> [!NOTE]
> <span data-ttu-id="de7d7-168">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="de7d7-168">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="de7d7-169">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="de7d7-169">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="de7d7-170">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="de7d7-170">What's changed</span></span>
* <span data-ttu-id="de7d7-171">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="de7d7-171">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

