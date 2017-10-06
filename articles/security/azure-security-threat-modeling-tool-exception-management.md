---
title: "aaaException Management - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 247096c10deeca94ebb9b19df7ba60e442ca1e4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-exception-management--mitigations"></a>Zabezpečení rámce: Správa výjimek | Způsoby zmírnění rizik 
| Produktům a službám | Článek |
| --------------- | ------- |
| **WCF** | <ul><li>[WCF – nezahrnujte serviceDebug uzlu v konfiguračním souboru](#servicedebug)</li><li>[WCF – nezahrnujte serviceMetadata uzlu v konfiguračním souboru](#servicemetadata)</li></ul> |
| **Webové rozhraní API** | <ul><li>[Zajistit, že je v rozhraní ASP.NET Web API správné zpracování výjimek](#exception)</li></ul> |
| **Webové aplikace** | <ul><li>[Podrobné informace o zabezpečení v chybových zprávách, neuvádějí](#messages)</li><li>[Implementace výchozí chyba zpracování stránky](#default)</li><li>[Nastavte metodu nasazení tooRetail ve službě IIS](#deployment)</li><li>[Výjimky má bezpečně selhat.](#fail)</li></ul> |

## <a id="servicedebug"></a>WCF – nezahrnujte serviceDebug uzlu v konfiguračním souboru

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, NET Framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | Komunikace Framework služby Windows (WCF) může být nakonfigurované tooexpose ladicí informace. Ladění informace nesmí použít v produkčním prostředí. Hello `<serviceDebug>` značky definuje, zda je povolena hello ladicí informace funkce služby WCF. Pokud atribut hello třídu includeExceptionDetailInFaults nastavena tootrue, informace o výjimce z aplikace hello, bude vrácen tooclients. Útočníci mohou využívat hello Další informace, které se získají z ladění výstupu toomount útoky cílené na hello framework, databázi nebo jiným prostředkům, které používá aplikace hello. |

### <a name="example"></a>Příklad
Hello následující konfigurační soubor obsahuje hello `<serviceDebug>` značky: 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
Zakažte ladicí informace ve službě hello. Toho lze dosáhnout odebráním hello `<serviceDebug>` značky z konfiguračního souboru aplikace. 

## <a id="servicemetadata"></a>WCF – nezahrnujte serviceMetadata uzlu v konfiguračním souboru

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Obecné, NET Framework 3 |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | Útočníkům cenné přehled o tom, jak může zneužít hello služby můžete poskytnout veřejně odhalení informací o služby. Hello `<serviceMetadata>` značky povolí funkci publikování metadat hello. Metadata služby můžou obsahovat citlivé informace, které by neměl být veřejně přístupná. Minimálně povolte jenom důvěryhodní uživatelé tooaccess hello metadata a ujistěte se, že nebude vystavena, víc informací. Ještě lepší zcela zakážete hello možnost toopublish metadat. Bezpečné konfigurace WCF nebude obsahovat hello `<serviceMetadata>` značky. |

## <a id="exception"></a>Zajistit, že je v rozhraní ASP.NET Web API správné zpracování výjimek

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC 5, 6 MVC |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Zpracování výjimek v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [modelu ověřování v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Kroky** | Ve výchozím nastavení jsou nejvíce nezachycená výjimek v rozhraní ASP.NET Web API přeložit na odpovědi HTTP se stavovým kódem`500, Internal Server Error`|

### <a name="example"></a>Příklad
toocontrol hello stavový kód vrácený hello rozhraní API, `HttpResponseException` je možné, jak je uvedeno níže: 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        throw new HttpResponseException(HttpStatusCode.NotFound);
    }
    return item;
}
```

### <a name="example"></a>Příklad
Pro další ovládání na odpověď výjimka hello hello `HttpResponseMessage` třídu lze použít, jak je uvedeno níže: 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        var resp = new HttpResponseMessage(HttpStatusCode.NotFound)
        {
            Content = new StringContent(string.Format("No product with ID = {0}", id)),
            ReasonPhrase = "Product ID Not Found"
        }
        throw new HttpResponseException(resp);
    }
    return item;
}
```
toocatch neošetřené výjimky, které nejsou typu hello `HttpResponseException`, lze použít filtry výjimek. Filtry výjimek implementovat hello `System.Web.Http.Filters.IExceptionFilter` rozhraní. Nejjednodušší způsob, jak toowrite Hello filtru výjimek je tooderive z hello `System.Web.Http.Filters.ExceptionFilterAttribute` třídy a přepsat metodu OnException hello. 

### <a name="example"></a>Příklad
Tady je filtr, který převádí `NotImplementedException` výjimky do stavového kódu protokolu HTTP `501, Not Implemented`: 
```C#
namespace ProductStore.Filters
{
    using System;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http.Filters;

    public class NotImplExceptionFilterAttribute : ExceptionFilterAttribute 
    {
        public override void OnException(HttpActionExecutedContext context)
        {
            if (context.Exception is NotImplementedException)
            {
                context.Response = new HttpResponseMessage(HttpStatusCode.NotImplemented);
            }
        }
    }
}
```

Existuje několik způsobů tooregister filtr výjimek webového rozhraní API:
- Akce
- Adaptérem
- Globálně

### <a name="example"></a>Příklad
tooapply hello filtrovat tooa určité akce, přidejte filtr hello jako atribut toohello akce: 
```C#
public class ProductsController : ApiController
{
    [NotImplExceptionFilter]
    public Contact GetContact(int id)
    {
        throw new NotImplementedException("This method is not implemented");
    }
}
```
### <a name="example"></a>Příklad
tooapply hello filtru tooall hello akcí na `controller`, přidejte filtr hello jako atributu toohello `controller` třídy: 

```C#
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a>Příklad
tooapply hello filtrovat globálně tooall webového rozhraní API řadiče, přidá instanci hello filtru toohello `GlobalConfiguration.Configuration.Filters` kolekce. Filtry výjimek v této kolekci použít tooany akce kontroleru webového rozhraní API. 
```C#
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a>Příklad
Pro ověření modelu můžete stav modelu hello předán tooCreateErrorResponse metoda, jak je uvedeno níže: 
```C#
public HttpResponseMessage PostProduct(Product item)
{
    if (!ModelState.IsValid)
    {
        return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
    }
    // Implementation not shown...
}
```

Zkontrolujte hello odkazy v části hello odkazy na další podrobnosti o výjimečných zpracování a ověření modelu v rozhraní ASP.Net Web API 

## <a id="messages"></a>Podrobné informace o zabezpečení v chybových zprávách, neuvádějí

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Obecné chybové zprávy jsou poskytovány přímo toohello uživatele bez včetně dat citlivé aplikaci. Příklady citlivých dat:</p><ul><li>Názvy serverů</li><li>Připojovací řetězce</li><li>Uživatelská jména</li><li>Hesla</li><li>Postupy SQL</li><li>Podrobnosti o selhání dynamické SQL</li><li>Trasování zásobníku a řádky kódu</li><li>Proměnné uložené v paměti</li><li>Umístění jednotky a složky</li><li>Body instalace aplikací</li><li>Nastavení konfigurace hostitele</li><li>Další podrobnosti o interní aplikace</li></ul><p>Zachycení všech chyb v aplikaci a poskytuje obecné chybové zprávy, jakož i povolení vlastní chyby v rámci služby IIS vám pomůže zabránili odhalení informací. Databáze systému SQL Server a .NET zpracování výjimek, mezi jiné chybě zpracování architektury, jsou uživatel se zlými úmysly zejména podrobné a velmi užitečné tooa profilace aplikace. To není přímo hello zobrazení obsahu třídy odvozené od třídy výjimky .NET hello a zajistěte, abyste měli správné zpracování výjimek, aby neočekávané výjimky nechtěně není vyvolána přímo toohello uživatele.</p><ul><li>Poskytují obecné chybové zprávy přímo toohello uživatele, který abstraktní tokeny konkrétní podrobnosti najít přímo v hello výjimky nebo chybové zprávě</li><li>Není obsah zobrazení hello výjimky .NET přímo třídu toohello uživatele</li><li>Depeše všechny chybové zprávy a podle potřeby informovat uživatele hello prostřednictvím obecnou chybovou zprávu odeslané toohello aplikace klienta</li><li>Nevystavujte hello obsah třídy výjimek hello přímo toohello uživatele, zejména hello návratová hodnota z `.ToString()`, nebo hello hodnoty vlastností hello zpráv nebo trasování zásobníku. Bezpečně protokolu tyto informace a zobrazit více neškodné uživatele toohello zpráv</li></ul>|

## <a id="default"></a>Implementace výchozí chyba zpracování stránky

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Upravit dialogové okno nastavení stránky ASP.NET chyby](https://technet.microsoft.com/library/dd569096(WS.10).aspx) |
| **Kroky** | <p>Pokud aplikace ASP.NET nezdaří a dojde HTTP/1.x 500 Vnitřní chyba serveru nebo konfigurace funkcí (například filtrování požadavků) zabraňuje zobrazení stránky, se budou generovat chybovou zprávu. Můžou správci rozhodnout, zda má aplikace hello zobrazit popisnou zprávu toohello klienta, podrobné chybové zprávy toohello klienta nebo podrobné chybové zprávy jenom toolocalhost. Hello <customErrors> v souboru web.config hello má tři režimy:</p><ul><li>**Na:** Určuje, že jsou povoleny vlastní chyby. Pokud není zadán žádný atribut defaultRedirect, uživatelé uvidí Obecná chyba. vlastní chyby Hello jsou zobrazeny toohello vzdálení klienti a toohello místního hostitele</li><li>**Vypnutí:** Určuje, že vlastní chyby jsou vypnuté. Hello podrobné chyby technologie ASP.NET se zobrazují toohello vzdálení klienti a toohello místního hostitele</li><li>**RemoteOnly:** Určuje, že vlastní chyby jsou zobrazeny pouze toohello vzdálení klienti a že ASP.NET chyby jsou zobrazeny toohello místního hostitele. Toto je výchozí hodnota hello</li></ul><p>Otevřete hello `web.config` souboru hello aplikace nebo webu a zkontrolujte, zda text hello značka má `<customErrors mode="RemoteOnly" />` nebo `<customErrors mode="On" />` definované.</p>|

## <a id="deployment"></a>Nastavte metodu nasazení tooRetail ve službě IIS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [nasazení – Element (schéma nastavení ASP.NET)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx) |
| **Kroky** | <p>Hello `<deployment retail>` přepínač je určená pro produkční servery služby IIS. Tento přepínač je použité toohelp se spouští s hello nejlepší možný výkon aplikace a nejmenší informace o zabezpečení, že úniků zakázáním hello aplikace možnost toogenerate výstup trasování na stránce, zákaz toodisplay možnost hello podrobná chyba Uživatelé tooend zprávy a deaktivace hello ladění přepínače.</p><p>Často, přepínače a možnosti, které jsou zaměřené na vývojáře, například se nezdařilo žádosti o trasování a ladění, jsou povolené během vývoje aktivní. Doporučuje se, že hello metodu nasazení na žádném serveru pro produkční nastavit tooretail. Otevřete soubor hello souboru machine.config a ujistěte se, že `<deployment retail="true" />` zůstane nastavit tootrue.</p>|

## <a id="fail"></a>Výjimky má bezpečně selhat.

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Bezpečně nezdaří](https://www.owasp.org/index.php/Fail_securely) |
| **Kroky** | Aplikace má bezpečně selhat. Libovolné metody, která vrací logickou hodnotu, podle které určité rozhoduje, měli pečlivě vytvořit bloku výjimky. Existuje mnoho logických chyb z důvodu nárůstu problémy zabezpečení toowhich v při neuváženě zápisu bloku výjimky hello.|

### <a name="example"></a>Příklad
```C#
        public static bool ValidateDomain(string pathToValidate, Uri currentUrl)
        {
            try
            {
                if (!string.IsNullOrWhiteSpace(pathToValidate))
                {
                    var domain = RetrieveDomain(currentUrl);
                    var replyPath = new Uri(pathToValidate);
                    var replyDomain = RetrieveDomain(replyPath);

                    if (string.Compare(domain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                    {
                        //// Adding additional check tooenable CMS urls if they are not hosted on same domain.
                        if (!string.IsNullOrWhiteSpace(Utilities.CmsBase))
                        {
                            var cmsDomain = RetrieveDomain(new Uri(Utilities.Base.Trim()));
                            if (string.Compare(cmDomain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                            {
                                return false;
                            }
                            else
                            {
                                return true;
                            }
                        }

                        return false;
                    }
                }

                return true;
            }
            catch (UriFormatException ex)
            {
                LogHelper.LogException("Utilities:ValidateDomain", ex);
                return true;
            }
        }
```
Hello výše metoda vždy vrátí hodnotu True, pokud dojde k výjimce. Pokud koncový uživatel hello poskytuje poškozená adresa URL, které hello ohledech prohlížeče, ale hello `Uri()` konstruktor není to vyvolá výjimku a postižené hello se provedou toohello platný, ale chybná adresa URL. 
