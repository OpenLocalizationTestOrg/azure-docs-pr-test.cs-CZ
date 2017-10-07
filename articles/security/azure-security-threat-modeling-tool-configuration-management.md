---
title: "aaaConfiguration Management - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
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
ms.openlocfilehash: 77aa4352fa61e928a1b7a4ff1d488a55d3d9b970
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-configuration-management--mitigations"></a>Zabezpečení rámce: Správa konfigurace | Způsoby zmírnění rizik 
| Produktům a službám | Článek |
| --------------- | ------- |
| **Webové aplikace** | <ul><li>[Implementace obsahu zásady zabezpečení (CSP) a zakažte vložené javascript](#csp-js)</li><li>[Povolit filtr XSS prohlížeče](#xss-filter)</li><li>[Aplikace ASP.NET musíte zakázat trasování a ladění předchozí toodeployment](#trace-deploy)</li><li>[JavaScripty třetí strany přístup pouze z důvěryhodných zdrojů](#js-trusted)</li><li>[Ujistěte se, že ověřený stránek ASP.NET začlenit nápravu uživatelského rozhraní nebo obrany opěry pro klikněte na](#ui-defenses)</li><li>[Zajistit, že pouze důvěryhodné zdroje jsou povolené, pokud je povoleno CORS na webové aplikace ASP.NET](#cors-aspnet)</li><li>[Povolit Atribut ValidateRequest na stránkách ASP.NET](#validate-aspnet)</li><li>[Použít místně hostované nejnovější verze knihoven jazyka JavaScript](#local-js)</li><li>[Zakázat automatické sledování toku dat MIME](#mime-sniff)</li><li>[Odebrat záhlaví standardní server na tímto způsobem tooavoid weby systému Windows Azure](#standard-finger)</li></ul> |
| **Database** | <ul><li>[Konfigurace brány Windows Firewall pro přístup k databázovému stroji](#firewall-db)</li></ul> |
| **Webové rozhraní API** | <ul><li>[Zajistit, že pouze důvěryhodného zdroje jsou povolené, pokud je povoleno CORS na rozhraní ASP.NET Web API](#cors-api)</li><li>[Zašifrování částí webového rozhraní API konfigurační soubory, které obsahují citlivá data](#config-sensitive)</li></ul> |
| **Zařízení IoT** | <ul><li>[Všechna rozhraní správce jsou zabezpečená s silné přihlašovací údaje](#admin-strong)</li><li>[Ujistěte se, že neznámý kód nelze provést na zařízeních](#unknown-exe)</li><li>[Šifrování operačního systému a další oddíly zařízení IoT bit schránku](#partition-iot)</li><li>[Ujistěte se, že na zařízení jsou povolené jenom hello minimální služeb nebo funkcí](#min-enable)</li></ul> |
| **Brána pole IoT** | <ul><li>[Šifrování operačního systému a další oddíly brána pole IoT se bit schránku](#field-bit-locker)</li><li>[Ujistěte se, že se změnila hello výchozí přihlašovací údaje brány pole hello během instalace](#default-change)</li></ul> |
| **Brána IoT cloudu** | <ul><li>[Ujistěte se, že tento hello Cloudová brána implementuje firmwarem proces tookeep hello připojené zařízení až toodate](#cloud-firmware)</li></ul> |
| **Počítač hranice vztahů důvěryhodnosti** | <ul><li>[Zajistěte, aby zařízení kontrolních mechanismů pro zabezpečení koncový bod nakonfigurovaný podle zásady organizace](#controls-policies)</li></ul> |
| **Azure Storage** | <ul><li>[Zajištění zabezpečení správy přístupových klíčů k úložišti Azure](#secure-keys)</li><li>[Zajistit, že pouze důvěryhodného zdroje jsou povolené, pokud je povoleno CORS na úložiště Azure](#cors-storage)</li></ul> |
| **WCF** | <ul><li>[Povolení služby WCF na funkci omezení](#throttling)</li><li>[Zpřístupnění informací WCF prostřednictvím metadat](#info-metadata)</li></ul> | 

## <a id="csp-js"></a>Implementace obsahu zásady zabezpečení (CSP) a zakažte vložené javascript

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Úvod tooContent zásady zabezpečení](http://www.html5rocks.com/en/tutorials/security/content-security-policy/), [obsahu referenční informace o zásadách zabezpečení](http://content-security-policy.com/), [funkce zabezpečení](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide/security/), [zásady zabezpečení toocontent ÚVOD](https://docs.webplatform.org/wiki/tutorials/content-security-policy), [Můžete použít zprostředkovatele kryptografických služeb?](http://caniuse.com/#feat=contentsecuritypolicy) |
| **Kroky** | <p>Obsahu zásady zabezpečení (CSP) je obrany do hloubky zabezpečení mechanismus, W3C standardní, který umožňuje řízení toohave vlastníci webové aplikace na hello obsah vložený do své lokality. Zprostředkovatel kryptografických služeb je přidána jako hlavičku HTTP odpovědi na webovém serveru hello a na straně klienta hello vynucováno prohlížeče. Je zásadu na základě seznamu povolených IP adres – web můžou deklarovat sadu důvěryhodných domén ze které aktivní obsah, jako je JavaScript je možné načíst.</p><p>Zprostředkovatel kryptografických služeb poskytuje následující výhody zabezpečení hello:</p><ul><li>**Ochrana proti XSS:** Pokud na stránce je snadno napadnutelný tooXSS, útočník ho může zneužít 2 způsoby:<ul><li>Vložit `<script>malicious code</script>`. Tato zneužití nebude fungovat kvůli tooCSP na základní omezení-1</li><li>Vložit `<script src=”http://attacker.com/maliciousCode.js”/>`. Tato zneužití nebude fungovat, protože hello útočník řídí domény nebude v seznamu povolených IP adres zprostředkovatele kryptografických služeb je domén</li></ul></li><li>**Kontrolu nad data exfiltration:** Pokud žádný škodlivý obsah na webové stránce pokusí tooconnect tooan externí web a získá data, hello připojení bude přerušeno CSP. Důvodem je, že hello cílové domény nebude v seznamu povolených IP adres na CSP</li><li>**Obrana proti opěry pro klikněte na tlačítko:** opěry pro klikněte na tlačítko se útoku technika pomocí, který nežádoucí osoba můžete rámce originální webu a vynutit tooclick uživatele na prvky uživatelského rozhraní. Aktuálně obrana proti opěry pro klikněte na tlačítko dosáhnete pomocí konfigurace odpovědi hlavičku X-Frame-Options. Některé prohlížeče respektují tuto hlavičku a budete dopředného CSP bude standardním způsobem toodefend proti opěry pro klikněte na</li><li>**Vytváření sestav v reálném čase útoku:** při vkládání útok na webu povoleno CSP prohlížeče automaticky aktivuje koncový bod oznámení tooan nakonfigurován na webovém serveru hello. Tímto způsobem CSP slouží jako upozornění systému v reálném čase.</li></ul> |

### <a name="example"></a>Příklad
Příklad zásady: 
```C#
Content-Security-Policy: default-src 'self'; script-src 'self' www.google-analytics.com 
```
Tato zásada umožňuje tooload skripty pouze ze serveru a google analytics server hello webové aplikace. Skripty načtené z jiné lokality budou odmítnuty. Pokud zprostředkovatel kryptografických služeb je povoleno na webu, hello následující funkce jsou útoky XSS toomitigate automaticky zakázaná. 

### <a name="example"></a>Příklad
Vložené skripty nebude spustit. Následují příklady vložené skripty 
```javascript
<script> some Javascript code </script>
Event handling attributes of HTML tags (e.g., <button onclick=”function(){}”>
javascript:alert(1);
```

### <a name="example"></a>Příklad
Řetězce, nebudou hodnocené jako kód. 
```javascript
Example: var str="alert(1)"; eval(str);
```

## <a id="xss-filter"></a>Povolit filtr XSS prohlížeče

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Filtr XSS ochrany](https://www.owasp.org/index.php/List_of_useful_HTTP_headers#X-XSS-Protection) |
| **Kroky** | <p>Ovládací prvky konfigurace hlavičky X-XSS ochrany odpovědi hello prohlížeče více webů skriptu filtru. Tuto hlavičku odpovědi může mít následující hodnoty:</p><ul><li>`0:`Tato akce zakáže filtr hello</li><li>`1: Filter enabled`Pokud je zjištěn útoku skriptování webů, v pořadí toostop hello útok, hello prohlížeč bude úpravu stránku hello</li><li>`1: mode=block : Filter enabled`. Místo úpravu stránku hello, pokud je zjištěn útoky XSS, prohlížeč hello zabrání vykreslování stránky hello</li><li>`1: report=http://[YOURDOMAIN]/your_report_URI : Filter enabled`. Hello prohlížeč bude úpravu porušení hello hello stránky a sestavy.</li></ul><p>Toto je funkce chromu využitím CSP porušení sestavy toosend podrobnosti tooa URI podle svého výběru. Hello poslední 2 možnosti jsou považovány za bezpečné hodnoty.</p>|

## <a id="trace-deploy"></a>Aplikace ASP.NET musíte zakázat trasování a ladění předchozí toodeployment

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Přehled ladění ASP.NET](http://msdn2.microsoft.com/library/ms227556.aspx), [ASP.NET trasování – přehled](http://msdn2.microsoft.com/library/bb386420.aspx), [postupy: Povolení trasování pro aplikace ASP.NET](http://msdn2.microsoft.com/library/0x5wc973.aspx), [postupy: povolení ladění pro aplikace ASP.NET](http://msdn2.microsoft.com/library/e8z01xdh(VS.80).aspx) |
| **Kroky** | Při zapnutém trasování pro stránku hello, každý prohlížeč také o to požádá získá informace o hello trasování, která obsahuje data o stavu interního serveru a pracovní postup. Tyto informace může být citlivé z hlediska zabezpečení. Pokud je povoleno ladění pro stránku hello, zobrazí chyby děje na výsledek server hello v dat trasování zásobníku úplné toohello prohlížeče. Tato data vystavuje bezpečnostní informace o postupu hello serveru. |

## <a id="js-trusted"></a>JavaScripty třetí strany přístup pouze z důvěryhodných zdrojů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | pouze z důvěryhodných zdrojů by měla odkazovat JavaScripty třetích stran. Koncové body Hello odkaz musí být vždy na protokol SSL. |

## <a id="ui-defenses"></a>Ujistěte se, že ověřený stránek ASP.NET začlenit nápravu uživatelského rozhraní nebo obrany opěry pro klikněte na

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Klikněte na tlačítko-opěry pro list cheaty obrany OWASP](https://www.owasp.org/index.php/Clickjacking_Defense_Cheat_Sheet), [IE Internals - boje proti opěry pro klikněte na tlačítko s X-Frame-Options](https://blogs.msdn.microsoft.com/ieinternals/2010/03/30/combating-click-jacking-with-x-frame-options/) |
| **Kroky** | <p>Opěry pro klikněte na tlačítko, také známé jako "uživatelského rozhraní nápravu útoku", je když útočník pomocí více vrstev průhledného nebo neprůhledného tootrick uživatele do kliknutím na tlačítko nebo odkaz na další stránce, když se měla záměrné tooclick na stránku hello nejvyšší úrovně.</p><p>Toto rozvrstvení je dosaženo tím, že vytvoří škodlivé stránky pomocí elementu iframe, který načte stránku hello napadeného počítače. Proto útočník hello "zneužívá" klikne na tlačítko určené výhradně pro jejich stránky a směrování je tooanother stránky, pravděpodobně vlastníkem jiné aplikace, domény nebo obojí. útoky opěry pro klikněte na tlačítko tooprevent sadu hello správné X-Frame-Options hlavičky HTTP odpovědi které pokyn hello prohlížeče toonot povolit rámcovacích z jiných domén</p>|

### <a name="example"></a>Příklad
Hlavička X-FRAME-OPTIONS Hello lze nastavit pomocí souboru web.config služby IIS. Soubor Web.config pro lokality, které by měly být nikdy ohraničeny fragment kódu: 
```C#
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="DENY"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

### <a name="example"></a>Příklad
Kód web.config pro lokality, které by měly být pouze ohraničeny pomocí stránky v hello stejné domény: 
```C#
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="SAMEORIGIN"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

## <a id="cors-aspnet"></a>Zajistit, že pouze důvěryhodné zdroje jsou povolené, pokud je povoleno CORS na webové aplikace ASP.NET

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Webové formuláře, MVC5 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Zabezpečení prohlížeče brání provedení domény tooanother požadavky AJAX na webové stránce. Toto omezení se nazývá zásada stejné původu hello a zabrání škodlivé weby čtení citlivá data z jiné lokality. Ale v některých případech může být požadované tooexpose rozhraní API bezpečně které můžou využívat ostatních lokalit. Mezi sdílení prostředků zdroji (CORS) je standard W3C, který umožňuje serveru toorelax hello stejného původu zásad. Pomocí CORS, server explicitně povolit některé žádostí napříč zdroji při odmítnutí ostatní.</p><p>CORS je bezpečnější a flexibilnější než dřívější techniky, jako je například JSONP. Jádro aplikace povolení CORS překládá tooadding několik hlavičky HTTP odpovědi (Access - Control-*) toohello webové aplikace a to lze provést několika způsoby.</p>|

### <a name="example"></a>Příklad
Pokud přístup tooWeb.config je k dispozici, můžete CORS přidány prostřednictvím hello následující kód: 
```XML
<system.webServer>
    <httpProtocol>
      <customHeaders>
        <clear />
        <add name="Access-Control-Allow-Origin" value="http://example.com" />
      </customHeaders>
    </httpProtocol>
```

### <a name="example"></a>Příklad
Pokud přístup tooweb.config není k dispozici, CORS mohou být konfigurovány tak, že přidáte hello následující CSharp kódu: 
```C#
HttpContext.Response.AppendHeader("Access-Control-Allow-Origin", "http://example.com")
```

Nastavte prosím Poznámka: je důležité tooensure, který hello seznam původů v atributu "Access-Control-Allow-Origin" tooa omezené a důvěryhodných sadu původu. Selhání tooconfigure tomto nesprávně (například nastavení hodnoty hello jako ' *') vám umožní škodlivé weby tootrigger mezi žádostí zdroji toohello webové aplikace > bez jakýchkoli omezení, a díky hello aplikace tooCSRF stát terčem útoků. 

## <a id="validate-aspnet"></a>Povolit Atribut ValidateRequest na stránkách ASP.NET

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Webové formuláře, MVC5 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Žádosti o ověření - prevence útoků skriptu](http://www.asp.net/whitepapers/request-validation) |
| **Kroky** | <p>Ověření žádosti, funkce technologie ASP.NET od verze 1.1, znemožníte hello server přijímat obsahu obsahující bez kódování HTML. Tato funkce je určena toohelp zabránit útokům některé vložení skriptu, které kód skriptu klienta HTML nesmí být nechtěně odeslaná tooa server, uložené a poté jsou předloženy tooother uživatelé. Stále důrazně doporučujeme, aby ověření všech vstupních dat a jeho v případě nutnosti kódování HTML.</p><p>Žádost o ověření se provádí tak, že porovnáte všechny vstupní data tooa seznam potenciálně nebezpečné hodnoty. Pokud je nalezena shoda, ASP.NET vyvolá `HttpRequestValidationException`. Žádosti o ověření funkce je ve výchozím nastavení povolena.</p>|

### <a name="example"></a>Příklad
Tuto funkci však lze vypnout na úrovni stránky: 
```XML
<%@ Page validateRequest="false" %> 
```
nebo na úrovni aplikace 
```XML
<configuration>
   <system.web>
      <pages validateRequest="false" />
   </system.web>
</configuration>
```
Všimněte si tuto funkci žádosti o ověření se nepodporuje a není součástí MVC6 kanálu. 

## <a id="local-js"></a>Použít místně hostované nejnovější verze knihoven jazyka JavaScript

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Vývojáře, kteří používají standardní knihovny jazyka JavaScript, jako je nutné použít JQuery schválené verzích běžné knihovny jazyka JavaScript, které neobsahují nedostatky zabezpečení. Doporučeným postupem je toouse hello nejvíce nejnovější verzi hello knihovny, protože obsahují opravy zabezpečení pro známých slabých míst v jejich starší verze.</p><p>Pokud nemůžete používat hello nejnovější verzi z důvodu toocompatibility důvodů, je třeba použít hello nižší než minimální verze.</p><p>Přijatelné minimální verze:</p><ul><li>**JQuery**<ul><li>JQuery 1.7.1</li><li>JQueryUI 1.10.0</li><li>JQuery ověření 1.9</li><li>JQuery Mobile 1.0.1</li><li>Cyklus JQuery 2.99</li><li>JQuery DataTables 1.9.0</li></ul></li><li>**Sadu ovládacích prvků AJAX**<ul><li>Sadu ovládacích prvků AJAX 40412</li></ul></li><li>**Webové formuláře ASP.NET a Ajax**<ul><li>Webové formuláře ASP.NET a Ajax 4</li><li>Technologie ASP.NET Ajax 3.5</li></ul></li><li>**ASP.NET MVC**<ul><li>ASP.NET MVC 3.0</li></ul></li></ul><p>Nikdy načíst žádnou knihovnu JavaScript z externí webů jako veřejné sítím CDN</p>|

## <a id="mime-sniff"></a>Zakázat automatické sledování toku dat MIME

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [IE8 zabezpečení část V: komplexní ochranu](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx), [typ MIME](http://en.wikipedia.org/wiki/Mime_type) |
| **Kroky** | záhlaví Hello X obsah typu možnosti je hlavičky protokolu HTTP, která vývojářům umožní toospecify, že jejich obsah by neměl být MIME zachycení. Tuto hlavičku je navrženou toomitigate sledování toku dat MIME útoky. Pro jednotlivé stránky, která by mohla obsahovat ovladatelné obsah uživatele, je nutné použít hello HTTP hlavičky X-obsahu – typ-možnosti: nosniff. hello požadovaná hlavička tooenable globálně pro všechny stránky v aplikaci hello, můžete provést jednu z následujících akcí hello|

### <a name="example"></a>Příklad
Přidáte hlavičku hello v souboru web.config hello, pokud je aplikace hello hostovaná pomocí Internetové informační služby (IIS) 7 a vyšší. 
```XML
<system.webServer>
<httpProtocol>
<customHeaders>
<add name="X-Content-Type-Options" value="nosniff"/>
</customHeaders>
</httpProtocol>
</system.webServer>
```

### <a name="example"></a>Příklad
Přidat hlavičku hello prostřednictvím hello globální aplikace\_BeginRequest 
```C#
void Application_BeginRequest(object sender, EventArgs e)
{
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
}
```

### <a name="example"></a>Příklad
Implementace vlastního modulu HTTP 
```C#
public class XContentTypeOptionsModule : IHttpModule
{
#region IHttpModule Members
public void Dispose()
{
}
public void Init(HttpApplication context)
{
context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders);
}
#endregion
void context_PreSendRequestHeaders(object sender, EventArgs e)
{
HttpApplication application = sender as HttpApplication;
if (application == null)
  return;
if (application.Response.Headers["X-Content-Type-Options "] != null)
  return;
application.Response.Headers.Add("X-Content-Type-Options ", "nosniff");
}
}
```

### <a name="example"></a>Příklad
Požadovaná hlavička hello pouze pro konkrétní stránky můžete povolit přidáním tooindividual odpovědí: 

```C#
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
```

## <a id="standard-finger"></a>Odebrat záhlaví standardní server na tímto způsobem tooavoid weby systému Windows Azure

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | EnvironmentType – Azure |
| **Odkazy**              | [Odebrání serveru standardní hlavičky na weby systému Windows Azure](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/) |
| **Kroky** | Záhlaví, jako je například Server X-používá technologii-, verze X AspNet odhalit informace o serveru hello a hello základní technologie. Je doporučeno toosuppress tyto hlavičky a zabrání tímto způsobem hello aplikace |

## <a id="firewall-db"></a>Konfigurace brány Windows Firewall pro přístup k databázovému stroji

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | SQL Azure a místní |
| **Atributy**              | Není k dispozici, verzi SQL - 12 |
| **Odkazy**              | [Jak tooconfigure Azure SQL databáze brány firewall](https://azure.microsoft.com/documentation/articles/sql-database-firewall-configure/), [konfigurace brány Windows Firewall pro přístup k databázovému stroji](https://msdn.microsoft.com/library/ms175043) |
| **Kroky** | Brány firewall systémů pomáhá zabránit neoprávněnému přístupu toocomputer prostředky. tooaccess instanci databázového stroje SQL Server hello přes bránu firewall, musíte nakonfigurovat bránu firewall hello hello počítače se systémem SQL Server tooallow přístup |

## <a id="cors-api"></a>Zajistit, že pouze důvěryhodného zdroje jsou povolené, pokud je povoleno CORS na rozhraní ASP.NET Web API

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC 5 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Povolení žádostí napříč zdroji v rozhraní ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api), [rozhraní ASP.NET Web API – podpora CORS v rozhraní ASP.NET Web API 2](https://msdn.microsoft.com/magazine/dn532203.aspx) |
| **Kroky** | <p>Zabezpečení prohlížeče brání provedení domény tooanother požadavky AJAX na webové stránce. Toto omezení se nazývá zásada stejné původu hello a zabrání škodlivé weby čtení citlivá data z jiné lokality. Ale v některých případech může být požadované tooexpose rozhraní API bezpečně které můžou využívat ostatních lokalit. Mezi sdílení prostředků zdroji (CORS) je standard W3C, který umožňuje serveru toorelax hello stejného původu zásad.</p><p>Pomocí CORS, server explicitně povolit některé žádostí napříč zdroji při odmítnutí ostatní. CORS je bezpečnější a flexibilnější než dřívější techniky, jako je například JSONP.</p>|

### <a name="example"></a>Příklad
Hello App_Start/WebApiConfig.cs přidejte následující kód toohello WebApiConfig.Register metoda hello 
```C#
using System.Web.Http;
namespace WebService
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // New code
            config.EnableCors();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
        }
    }
}
```

### <a name="example"></a>Příklad
Atribut EnableCors může být metody použité tooaction v řadiči následujícím způsobem: 

```C#
public class ResourcesController : ApiController
{
  [EnableCors("http://localhost:55912", // Origin
              null,                     // Request headers
              "GET",                    // HTTP methods
              "bar",                    // Response headers
              SupportsCredentials=true  // Allow credentials
  )]
  public HttpResponseMessage Get(int id)
  {
    var resp = Request.CreateResponse(HttpStatusCode.NoContent);
    resp.Headers.Add("bar", "a bar value");
    return resp;
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "PUT",                          // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "POST",                         // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
}
```

Nastavte prosím Poznámka: je důležité tooensure, který hello seznam původů v atributu EnableCors tooa omezené a důvěryhodných sadu původu. Selhání tooconfigure tomto nesprávně (například nastavení hodnoty hello jako ' *') vám umožní škodlivé weby tootrigger křížové počátek požadavky toohello rozhraní API bez jakýchkoli omezení > a díky hello rozhraní API tooCSRF stát terčem útoků. EnableCors může být doplněný o atribut na úrovni kontroleru. 

### <a name="example"></a>Příklad
toodisable CORS na konkrétní metodu v třídě, hello DisableCors atributu je možné, jak je uvedeno níže: 
```C#
[EnableCors("http://example.com", "Accept, Origin, Content-Type", "POST")]
public class ResourcesController : ApiController
{
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  // CORS not allowed because of hello [DisableCors] attribute
  [DisableCors]
  public HttpResponseMessage Delete(int id)
  {
    return Request.CreateResponse(HttpStatusCode.NoContent);
  }
}
```

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC 6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Povolení žádostí napříč zdroji (CORS) v ASP.NET Core 1.0](https://docs.asp.net/en/latest/security/cors.html) |
| **Kroky** | <p>V technologii ASP.NET Core 1.0 CORS se dá nastavit pomocí middlewaru nebo pomocí rozhraní MVC. Při použití MVC tooenable CORS hello se používají stejné CORS služby, ale hello CORS middleware není.</p>|

**Postup-1** povolení CORS s middleware: tooenable CORS pro celou aplikaci hello přidat hello CORS middleware toohello požadavku kanálu pomocí metody rozšíření UseCors hello. Zásadu cross-origin lze při přidávání middleware CORS hello pomocí třídy CorsPolicyBuilder hello. Existují dva způsoby toodo toto:

### <a name="example"></a>Příklad
Hello je nejdřív toocall UseCors s lambda. Hello lambda přebírá objekt CorsPolicyBuilder: 
```C#
public void Configure(IApplicationBuilder app)
{
    app.UseCors(builder =>
        builder.WithOrigins("http://example.com")
        .WithMethods("GET", "POST", "HEAD")
        .WithHeaders("accept", "content-type", "origin", "x-custom-header"));
}
```

### <a name="example"></a>Příklad
Hello druhou je toodefine jeden nebo více s názvem zásady CORS a pak vyberte hello zásady podle názvu v době běhu. 
```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowSpecificOrigin",
            builder => builder.WithOrigins("http://example.com"));
    });
}
public void Configure(IApplicationBuilder app)
{
    app.UseCors("AllowSpecificOrigin");
    app.Run(async (context) =>
    {
        await context.Response.WriteAsync("Hello World!");
    });
}
```

**Způsob 2** povolení CORS v MVC: vývojáři taky můžete použít MVC tooapply konkrétní CORS na každou akci, na jeden kontroler nebo globálně pro všechny řadiče.

### <a name="example"></a>Příklad
Na každou akci: hello [EnableCors] atribut toohello akce pro přidání toospecify zásadu CORS pro určité akce. Zadejte název zásady hello. 
```C#
public class HomeController : Controller
{
    [EnableCors("AllowSpecificOrigin")] 
    public IActionResult Index()
    {
        return View();
    }
```

### <a name="example"></a>Příklad
Pro každý řadič: 
```C#
[EnableCors("AllowSpecificOrigin")]
public class HomeController : Controller
{
```

### <a name="example"></a>Příklad
Globálně: 
```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.Configure<MvcOptions>(options =>
    {
        options.Filters.Add(new CorsAuthorizationFilterFactory("AllowSpecificOrigin"));
    });
}
```
Nastavte prosím Poznámka: je důležité tooensure, který hello seznam původů v atributu EnableCors tooa omezené a důvěryhodných sadu původu. Selhání tooconfigure tomto nesprávně (například nastavení hodnoty hello jako ' *') vám umožní škodlivé weby tootrigger křížové počátek požadavky toohello rozhraní API bez jakýchkoli omezení > a díky hello rozhraní API tooCSRF stát terčem útoků. 

### <a name="example"></a>Příklad
toodisable CORS pro kontroler nebo akce, použít atribut hello [DisableCors]. 
```C#
[DisableCors]
    public IActionResult About()
    {
        return View();
    }
```

## <a id="config-sensitive"></a>Zašifrování částí webového rozhraní API konfigurační soubory, které obsahují citlivá data

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Postupy: Šifrování konfigurační oddíly funkce v technologii ASP.NET 2.0 pomocí rozhraní DPAPI](https://msdn.microsoft.com/library/ff647398.aspx), [určení konfigurace poskytovatele chráněné](https://msdn.microsoft.com/library/68ze1hb2.aspx), [tajné klíče aplikace tooprotect pomocí Azure Key Vault](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Kroky** | Konfigurační soubory, jako je například hello souboru Web.config, appSettings.JSON určený se často používá toohold citlivé informace, včetně uživatelská jména, hesla, databázové připojovací řetězce a šifrovací klíče. Pokud není chránit tyto informace, vaše aplikace je snadno napadnutelný tooattackers nebo uživatelé se zlými úmysly získání citlivé informace, jako je například účet uživatelská jména a hesla, názvy databáze a názvy serverů. Podle typu nasazení hello (azure nebo místní), zašifrujte hello citlivé části konfigurační soubory pomocí rozhraní DPAPI nebo služby, jako je Azure Key Vault. |

## <a id="admin-strong"></a>Všechna rozhraní správce jsou zabezpečená s silné přihlašovací údaje

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Zařízení IoT | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Všechny správu rozhraní hello zařízení nebo zpřístupňuje brána pole by měly být zabezpečeny pomocí silné přihlašovací údaje. Také dalších zveřejněné rozhraní jako Wi-Fi, SSH, sdílené složky, FTP, by měly být zabezpečeny s silné přihlašovací údaje. Nepoužívejte slabé výchozí hesla. |

## <a id="unknown-exe"></a>Ujistěte se, že neznámý kód nelze provést na zařízeních

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Zařízení IoT | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Povolení zabezpečeného spouštění a šifrování zařízení bit schránku na jádro IoT Windows 10](https://developer.microsoft.com/windows/iot/win10/sb_bl) |
| **Kroky** | Zabezpečené spouštění UEFI omezuje hello systému tooonly umožní provádění podepsaný zadaný autoritou binární soubory. Tato funkce zabrání neznámý kód spustí na platformě hello a potenciálně oslabení postavení zabezpečení hello ho. Povolení zabezpečeného spouštění UEFI a omezit hello seznam certifikačních autorit, které jsou důvěryhodné pro podepisování kódu. Zaregistrujte všechny kód, který je nasazen na hello zařízení pomocí jedné z hello důvěryhodné autority. |

## <a id="partition-iot"></a>Šifrování operačního systému a další oddíly zařízení IoT bit schránku

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Zařízení IoT | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Jádro IoT Windows 10 implementuje odlehčenou verzi šifrování zařízení bit schránku, která silné závisí na přítomnosti hello čipu TPM na platformě hello, včetně hello nezbytné preOS protokolu v rozhraní UEFI, který provádí nezbytné měření hello. Tato měření preOS Ujistěte se, že hello OS novějším je spolehlivý záznam o tom, jak byla spuštěná hello operačního systému. Šifrování oddíly operačního systému pomocí bit schránku a všechny další oddíly, také v případě, že budou ukládat všechny citlivá data. |

## <a id="min-enable"></a>Ujistěte se, že na zařízení jsou povolené jenom hello minimální služeb nebo funkcí

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Zařízení IoT | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Zapnout nebo vypnout všechny funkce a služby v hello operačního systému, který není nutný pro hello fungování hello řešení. Pro například pokud hello zařízení nevyžaduje uživatelského rozhraní toobe, nasazení, instalace jádro IoT Windows v režimu bez periferních zařízení. |

## <a id="field-bit-locker"></a>Šifrování operačního systému a další oddíly brána pole IoT se bit schránku

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána pole IoT | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Jádro IoT Windows 10 implementuje odlehčenou verzi šifrování zařízení bit schránku, která silné závisí na přítomnosti hello čipu TPM na platformě hello, včetně hello nezbytné preOS protokolu v rozhraní UEFI, který provádí nezbytné měření hello. Tato měření preOS Ujistěte se, že hello OS novějším je spolehlivý záznam o tom, jak byla spuštěná hello operačního systému. Šifrování oddíly operačního systému pomocí bit schránku a všechny další oddíly, také v případě, že budou ukládat všechny citlivá data. |

## <a id="default-change"></a>Ujistěte se, že se změnila hello výchozí přihlašovací údaje brány pole hello během instalace

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána pole IoT | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Ujistěte se, že se změnila hello výchozí přihlašovací údaje brány pole hello během instalace |

## <a id="cloud-firmware"></a>Ujistěte se, že tento hello Cloudová brána implementuje firmwarem proces tookeep hello připojené zařízení až toodate

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána IoT cloudu | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Volba brány - Azure IoT Hub |
| **Odkazy**              | [Přehled správy zařízení IoT Hub](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-overview/), [jak tooupdate firmwaru zařízení](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-device-jobs/) |
| **Kroky** | LWM2M je protokol z hello Open Mobile Alliance pro správu zařízení IoT. Správa zařízení Azure IoT umožňuje toointeract s fyzického zařízení pomocí úlohy zařízení. Zkontrolujte, zda že tento hello Cloudová brána implementuje proces tooroutinely zachovat hello zařízení a další konfigurační data se toodate pomocí správou zařízení IoT Hub Azure. |

## <a id="controls-policies"></a>Zajistěte, aby zařízení kontrolních mechanismů pro zabezpečení koncový bod nakonfigurovaný podle zásady organizace

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Počítač hranice vztahů důvěryhodnosti | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Zkontrolujte, zda zařízení mít ovládací prvky zabezpečení koncový bod například bit schránku pro šifrování na úrovni disku, se aktualizované podpisy antivirové, hostitele na základě brány firewall, upgrady operačního systému, skupiny zásad atd., jsou nakonfigurované podle zásad zabezpečení organizace. |

## <a id="secure-keys"></a>Zajištění zabezpečení správy přístupových klíčů k úložišti Azure

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Příručka zabezpečení Azure Storage – Správa vaše klíče účtu úložiště](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_managing-your-storage-account-keys) |
| **Kroky** | <p>Úložiště klíčů: Doporučujeme toostore hello Azure přístupových klíčů k úložišti v Azure Key Vault jako tajný klíč a mít aplikace hello načíst klíč hello z trezoru klíčů. Tato možnost se doporučuje kvůli toohello následujících důvodů:</p><ul><li>Hello aplikace bude mít nikdy klíče pevně zakódované hello úložiště v konfigurační soubor, který odebere tento způsob někdo získávání klíčů toohello bez konkrétní oprávnění přístupu</li><li>Přístupové klávesy toohello se dá řídit pomocí Azure Active Directory. To znamená, že účet vlastníka můžete udělit přístup toohello několik aplikací, které potřebují tooretrieve hello klíče z Azure Key Vault. Jiné aplikace nebudou mít tooaccess hello klíče bez toho, abyste je konkrétně oprávnění</li><li>Opětovné generování klíče: Doporučujeme toohave procesu v místě tooregenerate přístup k úložišti Azure klíče z bezpečnostních důvodů. Podrobnosti o tom, jak a proč tooplan pro opětovné generování klíče jsou dokumentovány v článku hello Průvodce zabezpečením úložiště Azure odkazovat článku</li></ul>|

## <a id="cors-storage"></a>Zajistit, že pouze důvěryhodného zdroje jsou povolené, pokud je povoleno CORS na úložiště Azure

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Podpora CORS pro hello služby úložiště Azure](https://msdn.microsoft.com/library/azure/dn535601.aspx) |
| **Kroky** | Úložiště Azure vám umožní tooenable CORS – křížové sdílení prostředků původu. Pro každý účet úložiště můžete zadat domén, které mají přístup k prostředkům hello v daném účtu úložiště. Ve výchozím nastavení je CORS na všechny služby zakázána. CORS můžete povolit pomocí hello REST API nebo hello úložiště klienta knihovny toocall jednu z hello metody tooset hello služby zásad. |

## <a id="throttling"></a>Povolení služby WCF na funkci omezení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Rozhraní .NET framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | <p>Není umístění omezení na hello pomocí systému, který prostředků může způsobit vyčerpání prostředků a nakonec odepření služby.</p><ul><li>**Vysvětlení:** Windows Communication Foundation (WCF) nabízí možnost hello, toothrottle žádosti o služby. Povolení příliš mnoho požadavků klienta můžete vyplnění systému a vyčerpat její prostředky. Na hello můžete druhé straně, umožňuje pouze malý počet požadavků služby tooa bránit pomocí služby hello oprávněných uživatelů. Každá služba musí být jednotlivě ujít tooand nakonfigurované tooallow hello odpovídající velikostí prostředků.</li><li>**DOPORUČENÍ** povolit WCF omezení funkce služby a nastavení omezení vhodné pro vaši aplikaci.</li></ul>|

### <a name="example"></a>Příklad
Hello tady je příklad konfigurace s povoleno omezení:
```
<system.serviceModel> 
  <behaviors>
    <serviceBehaviors>
    <behavior name="Throttled">
    <serviceThrottling maxConcurrentCalls="[YOUR SERVICE VALUE]" maxConcurrentSessions="[YOUR SERVICE VALUE]" maxConcurrentInstances="[YOUR SERVICE VALUE]" /> 
  ...
</system.serviceModel> 
```

## <a id="info-metadata"></a>Zpřístupnění informací WCF prostřednictvím metadat

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Rozhraní .NET framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | Metadata může pomoci útočníci Další informace o systému hello a plánování forma útoku. Služby WCF může být nakonfigurované tooexpose metadat. Metadata poskytuje informace o popis podrobné služby a nesmí být všesměrového vysílání v produkčním prostředí. Hello `HttpGetEnabled`  /  `HttpsGetEnabled` vlastností třídy ServiceMetaData hello definuje, zda služba bude vystavovat hello metadat | 

### <a name="example"></a>Příklad
Následující kód Hello dá pokyn WCF toobroadcast metadat služby
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb); 
```
Není všesměrového vysílání metadata služby v produkčním prostředí. Nastavit hello HttpGetEnabled / HttpsGetEnabled vlastnosti hello ServiceMetaData tříd toofalse. 

### <a name="example"></a>Příklad
Následující kód Hello pokyn WCF toonot vysílání metadata služby. 
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior(); 
smb.HttpGetEnabled = false; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb);
```
