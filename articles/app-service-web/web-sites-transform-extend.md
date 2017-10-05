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
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Pokročilá konfigurace a rozšíření Azure App Service webové aplikace
Pomocí [transformace dokumentů XML](http://msdn.microsoft.com/library/dd465326.aspx) deklarace (XDT), můžete převést [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) souborů ve vaší webové aplikaci ve službě Azure App Service. Deklarace XDT můžete taky přidat pro soukromá rozšíření povolit akce správy vlastní webové aplikace. Tento článek obsahuje rozšíření aplikace webového správce PHP ukázkové, který umožňuje správu nastavení PHP prostřednictvím webového rozhraní.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="transform"></a>Pokročilá konfigurace prostřednictvím ApplicationHost.config
Platforma služby App Service poskytuje flexibilitu a řízení pro konfiguraci webové aplikace. I když není k dispozici pro přímé úpravy ve službě App Service standardní konfigurační soubor ApplicationHost.config služby IIS, platformu podporuje deklarativní model transformace ApplicationHost.config založené na XML dokumentu transformaci (XDT).

Tato funkce transformace využít, vytvořte soubor ApplicationHost.xdt s XDT obsahu a umístěte do kořenové lokality (d:\home\site) v [Kudu konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console). Musíte restartovat webovou aplikaci pro změny vstoupily v platnost.

Následující ukázka applicationHost.xdt ukazuje, jak přidat novou proměnnou vlastního prostředí do webové aplikace, která se používá PHP 5.4.

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

Soubor protokolu s transform stav a podrobnosti o je k dispozici z kořenového adresáře serveru FTP v rámci LogFiles\Transform.

Další ukázky naleznete v části [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Poznámka**<br />
Elementy ze seznamu modulů podle `system.webServer` nemůže být odebrány nebo přeuspořádány, ale je možné, přidané do seznamu.

## <a id="extend"></a>Rozšíření webové aplikace
### <a id="overview"></a>Přehled rozšíření privátní webové aplikace
App Service podporuje rozšíření webové aplikace jako bod rozšiřitelnosti pro akce správy. Ve skutečnosti některé funkce služby App Service jsou implementované jako předem nainstalovaná rozšíření. Při předinstalovány platforma rozšíření nemůže být upraven, můžete vytvořit a nakonfigurovat pro soukromá rozšíření pro webovou aplikaci. Tato funkce také závisí na XDT deklarace. Klíčové kroky pro vytvoření rozšíření privátní webové aplikace jsou následující:

1. Webové aplikace rozšíření **obsah**: vytvoření žádné webové aplikace podporované službou App Service
2. Webové aplikace rozšíření **deklarace**: Vytvořte soubor ApplicationHost.xdt
3. Webové aplikace rozšíření **nasazení**: umístit obsah ve složce SiteExtensions v`root`

Interní odkazy pro webovou aplikaci by měla odkazovat na relativní cestu k aplikaci zadaný v souboru ApplicationHost.xdt cestu. Všechny změny do souboru ApplicationHost.xdt vyžaduje recyklaci webové aplikace.

**Poznámka:**: Další informace o těchto klíčové prvky je k dispozici na [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Podrobný příklad je obsažena znázorňující kroky pro vytvoření a povolení rozšíření privátní webové aplikace. Zdrojový kód v příkladu správce PHP, která následuje si můžete stáhnout z [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

### <a id="SiteSample"></a>Příklad rozšíření webové aplikace: Správce PHP
Správce PHP je rozšíření webové aplikace, která správcům webové aplikace umožňuje snadno zobrazit a nakonfigurovat jejich nastavení PHP pomocí webové rozhraní, aniž by museli přímo upravit soubory INI PHP. Běžné konfigurační soubory pro jazyk PHP zahrnout soubor php.ini umístěný ve složce Program Files a. user.ini umístěného v kořenové složce webové aplikace. Vzhledem k tomu, že soubor php.ini není přímo upravovat na platformě App Service, používá rozšíření správce PHP. user.ini souboru použít změny nastavení.

#### <a id="PHPwebapp"></a>Správce PHP webové aplikace
Toto je domovské stránce Správce PHP nasazení:

![TransformSitePHPUI][TransformSitePHPUI]

Jak je vidět, rozšíření webové aplikace je stejně jako regulární webovou aplikaci, ale s další soubor ApplicationHost.xdt umístěné v kořenové složce webové aplikace (Další informace o souboru ApplicationHost.xdt jsou k dispozici v další části tohoto článku).

Správce PHP rozšíření byl vytvořen pomocí šablony Visual Studio ASP.NET MVC 4 webové aplikace. V Průzkumníku řešení následující zobrazení ukazuje strukturu rozšíření správce PHP.

![TransformSiteSolEx][TransformSiteSolEx]

Pouze speciální logiku potřebné pro soubor vstupně-výstupních operací je označíte, kde je umístěn daný adresář wwwroot webové aplikace. Jak ukazuje následující příklad kódu, v prostředí proměnné "HOME" znamená kořenová cesta webové aplikace a cestu wwwroot lze sestavit připojením "site\wwwroot":

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


Až budete mít cesta k adresáři, můžete ke čtení a zápis do souborů běžným sdíleným vstupně-výstupních operací.

Jeden bod upozornění se rozšíření webové aplikace, pokud o zpracování interní odkazy.  Pokud máte jakékoli odkazy v soubory ve formátu HTML, která umožňují absolutní cesty k interní odkazy na webové aplikace, je třeba zkontrolovat, že tyto odkazy se přidá jako předpona názvem svého rozšíření jako kořenového adresáře. To je nutné, protože kořenová pro toto rozšíření je nyní "/`[your-extension-name]`/" místo se musí být právě "/", tak interní odkazy příslušným způsobem aktualizuje. Předpokládejme například, že váš kód obsahuje odkaz na následující:

`"<a href="/Home/Settings">PHP Settings</a>"`

Při odkazu je součástí rozšíření webové aplikace, musí být odkaz v následující podobě:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Tento požadavek můžete obejít a to buď pomocí pouze relativní cesty v rámci webové aplikace nebo v případě aplikací ASP.NET, pomocí `@Html.ActionLink` metoda, kterou vytvoří na příslušné odkazy.

#### <a id="XDT"></a>Soubor applicationHost.xdt
Kód pro rozšíření vaší webové aplikace přejde pod %HOME%\SiteExtensions\[si název rozšíření]. Zavoláme vám to kořenového rozšíření.  

Registrace rozšíření vaší webové aplikace pomocí souboru applicationHost.config, musíte umístit soubor s názvem ApplicationHost.xdt v kořenu rozšíření. Obsah souboru ApplicationHost.xdt by měl vypadat takto:

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

Název, který jste vybrali jako název rozšíření by měl mít stejný název jako kořenové složky rozšíření.

To má za následek přidání nové aplikace cestu k `system.applicationHost` seznam stránek v lokalitě SCM. Lokalita SCM je koncový bod správy lokality konkrétní přihlašovací údaje. Obsahuje adresu URL `https://[your-site-name].scm.azurewebsites.net`.  

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

### <a id="deploy"></a>Nasazení rozšíření webové aplikace
K instalaci rozšíření vaší webové aplikace, můžete pomocí protokolu FTP zkopírujte všechny soubory vaší webové aplikace `\SiteExtensions\[your-extension-name]` složku, ve kterém chcete nainstalovat rozšíření webové aplikace.  Nezapomeňte zkopírovat soubor ApplicationHost.xdt do tohoto umístění také. Restartujte vaší webové aplikace, která rozšíření aktivuje.

Měli byste mít moci zobrazit vaše rozšíření webové aplikace na:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Všimněte si, že adresa URL vypadá stejně jako adresu URL pro webovou aplikaci, s tím rozdílem, že se používá protokol HTTPS a obsahuje ".scm".

Je možné zakázat všechna rozšíření privátní (není předem nainstalovaná) pro webovou aplikaci během vývoje a vyšetřování přidáním nastavení aplikace s klíčem `WEBSITE_PRIVATE_EXTENSIONS` a hodnota `0`.

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

