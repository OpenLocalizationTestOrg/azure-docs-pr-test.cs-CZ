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
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Pokročilá konfigurace a rozšíření Azure App Service webové aplikace
Pomocí [transformace dokumentů XML](http://msdn.microsoft.com/library/dd465326.aspx) deklarace (XDT), můžete převést hello [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) souborů ve vaší webové aplikaci ve službě Azure App Service. Můžete také použít XDT deklarace tooadd pro soukromá rozšíření tooenable vlastní webové aplikace akce správy. Tento článek obsahuje rozšíření aplikace webového správce PHP ukázkové, který umožňuje správu nastavení PHP prostřednictvím webového rozhraní.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="transform"></a>Pokročilá konfigurace prostřednictvím ApplicationHost.config
Hello platformě App Service poskytuje flexibilitu a řízení pro konfiguraci webové aplikace. I když hello standardní konfigurační soubor ApplicationHost.config služby IIS není k dispozici pro přímé úpravy ve službě App Service, hello platforma podporuje deklarativní model transformace ApplicationHost.config založené na XML dokumentu transformaci (XDT).

tooleverage tato funkce transformace, vytvořte soubor ApplicationHost.xdt s XDT obsahu a umístěte do kořenové lokality hello (d:\home\site) v hello [Kudu konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console). Toorestart hello webové aplikace může být nutné pro změny tootake vliv.

Následující ukázka applicationHost.xdt Hello ukazuje, jak tooadd nové proměnné tooa vlastní prostředí webové aplikaci, která používá PHP 5.4.

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

Soubor protokolu s transform stav a podrobnosti o je k dispozici z kořenového hello FTP v rámci LogFiles\Transform.

Další ukázky naleznete v části [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Poznámka**<br />
Elementy z hello seznam modulů v části `system.webServer` nemůže být odebrány nebo přeuspořádány, ale je možné, dodatky toohello seznamu.

## <a id="extend"></a>Rozšíření webové aplikace
### <a id="overview"></a>Přehled rozšíření privátní webové aplikace
App Service podporuje rozšíření webové aplikace jako bod rozšiřitelnosti pro akce správy. Ve skutečnosti některé funkce služby App Service jsou implementované jako předem nainstalovaná rozšíření. Při hello předinstalovány platforma rozšíření nemůže být upraven, můžete vytvořit a nakonfigurovat pro soukromá rozšíření pro webovou aplikaci. Tato funkce také závisí na XDT deklarace. Hello klíčové kroky pro vytvoření rozšíření privátní webové aplikace jsou hello následující:

1. Webové aplikace rozšíření **obsah**: vytvoření žádné webové aplikace podporované službou App Service
2. Webové aplikace rozšíření **deklarace**: Vytvořte soubor ApplicationHost.xdt
3. Webové aplikace rozšíření **nasazení**: umístit obsah složky SiteExtensions hello v části`root`

Interní odkazy pro webovou aplikaci hello by měla odkazovat tooa cesta relativní toohello cestu aplikace zadané v souboru ApplicationHost.xdt hello. Všechny změny toohello ApplicationHost.xdt soubor vyžaduje recyklaci webové aplikace.

**Poznámka:**: Další informace o těchto klíčové prvky je k dispozici na [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Podrobný příklad je součástí tooillustrate hello kroky pro vytvoření a povolení rozšíření privátní webové aplikace. Hello zdrojového kódu pro příklad správce PHP hello způsobem si můžete stáhnout z [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

### <a id="SiteSample"></a>Příklad rozšíření webové aplikace: Správce PHP
Správce PHP je rozšíření webové aplikace, která umožňuje správci aplikace webového tooeasily zobrazení a konfigurace jejich nastavení PHP, místo nutnosti soubory INI PHP toomodify přímo pomocí webového rozhraní. Běžné konfigurační soubory pro jazyk PHP zahrnout soubor php.ini hello umístěny ve složce Program Files and hello. user.ini umístěného v kořenové složce hello vaší webové aplikace. Vzhledem k tomu, že soubor php.ini hello není přímo upravovat na hello platformě App Service, hello rozšíření PHP správce používá hello. tooapply souboru user.ini změny nastavení.

#### <a id="PHPwebapp"></a>Hello správce PHP webové aplikace
Hello následuje hello domovské stránce hello správce PHP nasazení:

![TransformSitePHPUI][TransformSitePHPUI]

Jak můžete vidět, je stejně jako regulární webovou aplikaci, ale s další soubor ApplicationHost.xdt umístit do kořenové složky hello hello webové aplikace (Další informace o souboru ApplicationHost.xdt hello jsou k dispozici v další části hello tohoto rozšíření webové aplikace článek).

Správce PHP rozšíření Hello byl vytvořen pomocí šablony Visual Studio MVC 4 webovou aplikaci ASP.NET hello. Hello následující zobrazení v Průzkumníku řešení ukazuje hello strukturu hello správce PHP rozšíření.

![TransformSiteSolEx][TransformSiteSolEx]

Hello jenom speciální logika potřebné pro soubor vstupně-výstupních operací je tooindicate kde hello adresář wwwroot hello webové aplikace se nachází. Jako hello následující příklad ukazuje kód hello proměnné prostředí "HOME" označuje hello kořenová cesta webové aplikace a cestu hello wwwroot lze sestavit připojením "site\wwwroot":

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


Až budete mít hello cesta k adresáři, můžete použít běžný soubor tooread vstupně-výstupních operací a zápisu toofiles.

Jeden bod upozornění se rozšíření webové aplikace, pokud o hello zpracování interní odkazy.  Pokud máte jakékoli odkazy v soubory ve formátu HTML, která umožňují absolutní cesty toointernal odkazy na webové aplikace, je třeba zkontrolovat, že tyto odkazy se přidá jako předpona názvem svého rozšíření jako kořenového adresáře. To je nutné, protože kořenová hello pro toto rozšíření je nyní "/`[your-extension-name]`/" místo se musí být právě "/", tak interní odkazy příslušným způsobem aktualizuje. Například předpokládejme, že kód obsahuje toohello následující odkaz:

`"<a href="/Home/Settings">PHP Settings</a>"`

Při propojení hello je součástí rozšíření webové aplikace, musí být odkaz hello hello následující formulář:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Tento požadavek můžete obejít buď pomocí pouze relativní cesty v rámci webové aplikace nebo v případě aplikací ASP.NET hello pomocí hello `@Html.ActionLink` metoda, kterou vytvoří hello příslušné odkazy.

#### <a id="XDT"></a>soubor applicationHost.xdt Hello
Hello kód pro rozšíření vaší webové aplikace přejde pod %HOME%\SiteExtensions\[si název rozšíření]. Zavoláme vám tato rozšíření kořenové hello.  

tooregister rozšíření vaší webové aplikace pomocí souboru applicationHost.config hello, budete potřebovat tooplace do souboru s názvem ApplicationHost.xdt v kořenovém rozšíření hello. Hello obsah souboru ApplicationHost.xdt hello by měl vypadat takto:

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

Hello název, který jste vybrali jako název rozšíření by měl mít hello stejný název jako kořenové složky rozšíření.

Tato akce nemá vliv hello přidávání nové toohello cesta aplikace `system.applicationHost` seznam stránek v lokalitě SCM hello. lokalita SCM Hello je koncový bod správy lokality konkrétní přihlašovací údaje. Obsahuje adresu URL hello `https://[your-site-name].scm.azurewebsites.net`.  

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

### <a id="deploy"></a>Nasazení rozšíření webové aplikace
tooinstall rozšíření vaší webové aplikace, FTP toocopy můžete použít všechny soubory hello vaší webové aplikace toohello `\SiteExtensions\[your-extension-name]` složky hello webové aplikace, na kterém chcete tooinstall hello rozšíření.  Zda toocopy hello ApplicationHost.xdt toothis umístění souboru také být. Restartujte rozšíření hello tooenable vaší webové aplikace.

Byste měli mít toosee rozšíření vaší webové aplikace na:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Všimněte si, že adresa URL vypadá stejně jako hello adresu URL pro webovou aplikaci, s tím rozdílem, že se používá protokol HTTPS a obsahuje ".scm" hello.

Je rozšíření všechny privátní možné toodisable (není předinstalovaným) pro webovou aplikaci během vývoje a vyšetřování přidáním nastavení aplikace s klíčem hello `WEBSITE_PRIVATE_EXTENSIONS` a hodnota `0`.

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

