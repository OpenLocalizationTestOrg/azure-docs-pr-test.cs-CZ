---
title: "aaaAzure API Management šablony datový model odkaz | Microsoft Docs"
description: "Další informace o hello entitu a typ reprezentace pro běžné položky, které se používá v hello datové modely pro šablony portálu hello vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 7d049d8ecc9e597cf48ce0c820c172798bcf86de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-template-data-model-reference"></a>Referenční model dat šablony Azure API Management
Toto téma popisuje hello entitu a typ reprezentace pro běžné položky, které se používá v hello datové modely pro šablony portálu hello vývojáře ve službě Azure API Management.  
  
 Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
-   [Rozhraní API](#API)  
-   [Souhrn rozhraní API](#APISummary)  
-   [Aplikace](#Application)  
-   [Přílohy](#Attachment)  
-   [Ukázka kódu](#Sample)  
-   [Komentář](#Comment)  
-   [Filtrování](#Filtering)  
-   [Záhlaví](#Header)  
-   [Požadavek HTTP](#HTTPRequest)  
-   [Odpověď HTTP](#HTTPResponse)  
-   [Problém](#Issue)  
-   [Operace](#Operation)  
-   [Operace nabídky](#Menu)  
-   [Operace položky nabídky](#MenuItem)  
-   [Stránkování](#Paging)  
-   [Parametr](#Parameter)  
-   [Produktu](#Product)  
-   [Poskytovatel](#Provider)  
-   [Reprezentace](#Representation)  
-   [Předplatné](#Subscription)  
-   [Předplatné souhrn](#SubscriptionSummary)  
-   [Informace o účtu uživatele](#UserAccountInfo)  
-   [Přihlášení uživatele](#UseSignIn)  
-   [Registrace uživatele](#UserSignUp)  
  
##  <a name="API"></a>ROZHRANÍ API  
 Hello `API` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|id|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje hello rozhraní API v rámci hello aktuální instanci služby API Management. Hello hodnota je platná relativní adresa URL ve formátu hello `apis/{id}` kde `{id}` je identifikátor rozhraní API. Tato vlastnost je jen pro čtení.|  
|jméno|Řetězec|Název hello rozhraní API. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|description|Řetězec|Popis hello rozhraní API. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
|serviceUrl|Řetězec|Absolutní adresa URL back-end službu hello implementace toto rozhraní API.|  
|Cesta|Řetězec|Jednoznačná identifikace toto rozhraní API a všechny jeho cesty prostředku v rámci instance služby API Management hello relativní adresu URL. Se připojí základní adresu URL koncového bodu toohello rozhraní API zadaný během tooform vytvoření instance služby hello veřejnou adresu URL pro toto rozhraní API.|  
|protokoly|pole čísla|Popisuje, na které protokoly hello může vyvolat operace v tomto rozhraní API. Povolené hodnoty jsou `1 - http` a `2 - https`, nebo obojí.|  
|authenticationSettings|[Nastavení ověřování serveru ověřování](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Kolekce nastavení ověřování, které jsou zahrnuté v tomto rozhraní API.|  
|subscriptionKeyParameterNames|Objekt|Volitelná vlastnost, kterou lze použít toospecify vlastní názvy parametrů dotazu nebo hlavičku obsahující klíč předplatného hello. Když se nachází tato vlastnost musí obsahovat alespoň jeden ze dvou následujících vlastností hello.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a>Souhrn rozhraní API  
 Hello `API summary` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|id|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje hello rozhraní API v rámci hello aktuální instanci služby API Management. Hello hodnota je platná relativní adresa URL ve formátu hello `apis/{id}` kde `{id}` je identifikátor rozhraní API. Tato vlastnost je jen pro čtení.|  
|jméno|Řetězec|Název hello rozhraní API. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|description|Řetězec|Popis hello rozhraní API. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
  
##  <a name="Application"></a>Aplikace  
 Hello `application` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Hello jedinečný identifikátor aplikace hello.|  
|Název|Řetězec|Hello název aplikace hello.|  
|Popis|Řetězec|Popis Hello aplikace hello.|  
|URL|IDENTIFIKÁTOR URI|Hello identifikátor URI pro aplikaci hello.|  
|Verze|Řetězec|Informace o verzi pro aplikaci hello.|  
|Požadavky|Řetězec|Popis požadavky aplikace hello.|  
|Stav|Číslo|Hello aktuální stav aplikace hello.<br /><br /> -0 - zaregistrován<br /><br /> -1 - odeslání<br /><br /> -2 - publikována<br /><br /> -3 - odmítnut<br /><br /> -4 - Nepublikováno|  
|RegistrationDate|Data a času|aplikace Hello datum a čas hello byla zaregistrována.|  
|CategoryId|Číslo|kategorie Hello aplikace hello (Finance, zábava atd.)|  
|DeveloperId|Řetězec|Jedinečný identifikátor Hello hello vývojáře, které aplikace hello odeslána.|  
|Přílohy|Kolekce [přílohy](#Attachment) entity.|Přílohy hello aplikace, jako jsou snímky obrazovky nebo ikon.|  
|Ikona|[Přílohy](#Attachment)|Ikona hello Hello aplikace hello.|  
  
##  <a name="Attachment"></a>Přílohy  
 Hello `attachment` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Jedinečné ID|Řetězec|Hello jedinečný identifikátor pro přílohy hello.|  
|URL|Řetězec|Adresa URL Hello hello prostředku.|  
|Typ|Řetězec|Typ Hello přílohy.|  
|Typ obsahu|Řetězec|typ média Hello hello přílohy.|  
  
##  <a name="Sample"></a>Ukázka kódu  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Název|Řetězec|Název Hello hello operace.|  
|fragment kódu|Řetězec|Tato vlastnost je zastaralá a by se neměla používat.|  
|štětce|Řetězec|Které kódu toobe šablony použité při zobrazování ukázka kódu hello zvýrazňování syntaxe. Povolené hodnoty jsou `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, a `csharp`.|  
|šablona|Řetězec|Hello název této šablony ukázkový kód.|  
|Text|Řetězec|Zástupný symbol pro část ukázkový kód hello hello fragment kódu.|  
|– Metoda|Řetězec|Metoda HTTP operace hello Hello.|  
|Schéma|Řetězec|Hello toouse protokol pro žádost o operaci hello.|  
|Cesta|Řetězec|Cesta Hello hello operace.|  
|query|Řetězec|Příklad řetězce dotazu s definovaných parametrů.|  
|hostitele|Řetězec|Adresa URL Hello hello rozhraní API správy služby brány pro hello rozhraní API, která obsahuje tuto operaci.|  
|Záhlaví|Kolekce [záhlaví](#Header) entity.|Hlavičky pro tuto operaci.|  
|parameters|Kolekce [parametr](#Parameter) entity.|Parametry, které jsou definované pro tuto operaci.|  
  
##  <a name="Comment"></a>Komentář  
 Hello `API` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Číslo|id Hello hello komentáře.|  
|Commenttext –|Řetězec|Hello textu hello komentáře. Může zahrnovat HTML.|  
|DeveloperCompany|Řetězec|Název společnosti Hello vývojáře hello.|  
|PostedOn|Data a času|Komentář Hello datum a čas hello byl publikován.|  
  
##  <a name="Issue"></a>Problém  
 Hello `issue` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Hello jedinečný identifikátor pro hello problém.|  
|ApiID|Řetězec|id Hello hello rozhraní API, pro který byla nahlášena tento problém.|  
|Název|Řetězec|Název hello problém.|  
|Popis|Řetězec|Popis problému hello.|  
|SubscriptionDeveloperName|Řetězec|Křestní jméno hello vývojáře, která použije v hlášení hello problém.|  
|IssueState|Řetězec|aktuální stav Hello hello problému. Možné hodnoty jsou navržený, otevřeno, Uzavřeno.|  
|ReportedOn|Data a času|byla nahlášena problém hello Hello datum a čas.|  
|Komentáře|Kolekce [komentář](#Comment) entity.|Komentáře k tomuto problému.|  
|Přílohy|Kolekce [přílohy](api-management-template-data-model-reference.md#Attachment) entity.|Jakýkoli problém toohello přílohy.|  
|Služby|Kolekce [rozhraní API](#API) entity.|Hello rozhraní API předplatné tooby hello uživatele, který selhala hello problém.|  
  
##  <a name="Filtering"></a>Filtrování  
 Hello `filtering` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|vzor|Řetězec|Hello aktuální hledaný termín; nebo `null` Pokud neexistuje žádný hledaný termín.|  
|Zástupný symbol|Řetězec|toodisplay textu hello vyhledávacího pole text Hello, pokud neexistuje žádný hledaný termín, který je zadán.|  
  
##  <a name="Header"></a>Záhlaví  
 Tato část popisuje hello `parameter` reprezentace.  
  
|Vlastnost|Popis|Typ|  
|--------------|-----------------|----------|  
|jméno|Řetězec|Název parametru.|  
|description|Řetězec|Popis parametru.|  
|hodnota|Řetězec|Hodnota hlavičky.|  
|TypeName|Řetězec|Datový typ hodnoty hlavičky.|  
|Možnosti|Řetězec|Možnosti.|  
|Požadované|Logická hodnota|Jestli je vyžadováno hello záhlaví.|  
|Jen pro čtení|Logická hodnota|Jestli hlavička hello je jen pro čtení.|  
  
##  <a name="HTTPRequest"></a>Požadavek HTTP  
 Tato část popisuje hello `request` reprezentace.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|description|Řetězec|Popis žádost o operaci.|  
|Záhlaví|pole [záhlaví](#Header) entity.|Hlavičky požadavku.|  
|parameters|pole [parametr](#Parameter)|Kolekce parametrů žádost o operaci.|  
|reprezentace|pole [reprezentace](#Representation)|Kolekce reprezentace žádost o operaci.|  
  
##  <a name="HTTPResponse"></a>Odpověď HTTP  
 Tato část popisuje hello `response` reprezentace.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|statusCode|Kladné celé číslo|Stavový kód odpovědi operaci.|  
|description|Řetězec|Operace popis odpovědi.|  
|reprezentace|pole [reprezentace](#Representation)|Kolekce reprezentace odpověď operace.|  
  
##  <a name="Operation"></a>Operace  
 Hello `operation` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|id|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje hello operace v rámci hello aktuální instanci služby API Management. Hello hodnota je platná relativní adresa URL ve formátu hello `apis/{aid}/operations/{id}` kde `{aid}` je identifikátor rozhraní API a `{id}` je identifikátor operaci. Tato vlastnost je jen pro čtení.|  
|jméno|Řetězec|Název operace hello. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|description|Řetězec|Popis hello operace. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
|Schéma|Řetězec|Popisuje, na které protokoly hello může vyvolat operace v tomto rozhraní API. Povolené hodnoty jsou `http`, `https`, nebo obojí `http` a `https`.|  
|uriTemplate|Řetězec|Relativní adresa URL Šablona identifikace hello cílový prostředek pro tuto operaci. Může obsahovat parametry. Příklad:`customers/{cid}/orders/{oid}/?date={date}`|  
|hostitele|Řetězec|Hello API Management gateway adresa URL, který je hostitelem rozhraní API hello.|  
|HttpMethod|Řetězec|Metoda operaci HTTP.|  
|Požadavek|[Požadavek HTTP](#HTTPRequest)|Entita obsahující podrobnosti požadavku.|  
|odpovědi|pole [odpovědi HTTP](#HTTPResponse)|Pole operace [odpověď HTTP](#HTTPResponse) entity.|  
  
##  <a name="Menu"></a>Operace nabídky  
 Hello `operation menu` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ApiId|Řetězec|id Hello hello aktuální rozhraní API.|  
|CurrentOperationId|Řetězec|id Hello hello aktuální operace.|  
|Akce|Řetězec|Typ nabídky Hello.|  
|Vlastnost MenuItems|Kolekce [položky nabídky operaci](#MenuItem) entity.|Hello operací pro aktuální API hello.|  
  
##  <a name="MenuItem"></a>Operace položky nabídky  
 Hello `operation menu item` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Hello id operace hello.|  
|Název|Řetězec|Popis Hello hello operace.|  
|HttpMethod|Řetězec|Metoda Http operace hello Hello.|  
  
##  <a name="Paging"></a>Stránkování  
 Hello `paging` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Stránka|Číslo|Hello aktuální číslo stránky.|  
|PageSize|Číslo|Hello toobe maximální výsledky zobrazené na jedné stránce.|  
|TotalItemCount|Číslo|Hello počet položek pro zobrazení.|  
|ShowAll|Logická hodnota|Jestli toosho všechny výsledky na jedné stránce.|  
|PageCount|Číslo|Hello počet stránek výsledků.|  
  
##  <a name="Parameter"></a>Parametr  
 Tato část popisuje hello `parameter` reprezentace.  
  
|Vlastnost|Popis|Typ|  
|--------------|-----------------|----------|  
|jméno|Řetězec|Název parametru.|  
|description|Řetězec|Popis parametru.|  
|hodnota|Řetězec|Hodnota parametru.|  
|Možnosti|Pole řetězců|Hodnot definovaných pro hodnoty parametru dotazu.|  
|Požadované|Logická hodnota|Určuje, zda parametr je povinný, nebo ne.|  
|Typ|Číslo|Jestli je tento parametr parametr cesty (1), nebo parametr řetězce dotazu (2).|  
|TypeName|Řetězec|Typ parametru.|  
  
##  <a name="Product"></a>Produktu  
 Hello `product` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje hello produktu v aktuální instanci služby API Management hello. Hello hodnota je platná relativní adresa URL ve formátu hello `products/{pid}` kde `{pid}` je identifikátor produktu. Tato vlastnost je jen pro čtení.|  
|Název|Řetězec|Název produktu hello. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|Popis|Řetězec|Popis produktu hello. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
|Výrazy|Řetězec|Produkt podmínky použití. Vývojáři při toosubscribe toohello produktu zobrazí a vyžaduje tooaccept tyto podmínky před dokončením procesu předplatné hello.|  
|ProductState|Číslo|Určuje, zda text hello produkt publikovaný nebo ne. Publikované produkty jsou zjistitelný vývojáři na portál pro vývojáře hello. Není publikována produkty jsou viditelné pouze tooadministrators.<br /><br /> Hello povolené hodnoty pro stav produktu jsou:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|Logická hodnota|Určuje, jestli uživatel může mít několik odběrů toothis produktu v hello stejnou dobu.|  
|MultipleSubscriptionsCount|Číslo|Hello počet předplatných toothis produktu hello aktuální uživatel.|  
  
##  <a name="Provider"></a>Zprostředkovatel  
 Hello `provider` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Vlastnosti|slovník řetězců.|Vlastnosti pro tohoto zprostředkovatele ověřování.|  
|authenticationType.|Řetězec|Typ zprostředkovatele Hello. (Azure Active Directory, Facebook přihlášení, účet Google, Microsoft Account, služby Twitter).|  
|Popisek|Řetězec|Zobrazovaný název zprostředkovatele hello.|  
  
##  <a name="Representation"></a>Reprezentace  
 Tato část popisuje `representation`.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|Typ obsahu|Řetězec|Určuje registrované nebo vlastní typ obsahu pro tento reprezentace, například `application/xml`.|  
|Ukázka|Řetězec|Příkladem reprezentace hello.|  
  
##  <a name="Subscription"></a>Předplatné  
 Hello `subscription` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje hello předplatného v rámci hello aktuální instanci služby API Management. Hello hodnota je platná relativní adresa URL ve formátu hello `subscriptions/{sid}` kde `{sid}` je identifikátor předplatného. Tato vlastnost je jen pro čtení.|  
|productId|Řetězec|identifikátor prostředku produktu Hello hello přihlásit k odběru produktu. Hello hodnota je platná relativní adresa URL ve formátu hello `products/{pid}` kde `{pid}` je identifikátor produktu.|  
|ProductTitle|Řetězec|Název produktu hello. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|Popisvýrobku|Řetězec|Popis produktu hello. Nesmí být prázdný. Může zahrnovat formátování značky HTML. Maximální délka je 1 000 znaků.|  
|ProductDetailsUrl|Řetězec|Relativní adresa URL toohello podrobnosti o produktu.|  
|state|Řetězec|Stav Hello hello předplatného. Je možné stavy:<br /><br /> - `0 - suspended`– hello předplatného je zablokovaná, a hello odběratele nelze volat všechny rozhraní API produktu hello.<br /><br /> - `1 - active`– hello předplatné je aktivní.<br /><br /> - `2 - expired`– hello předplatné dosaženo datum vypršení platnosti a bylo deaktivováno.<br /><br /> - `3 - submitted`– žádosti o odběr hello nebylo provedeno hello developer, ale má ještě schválení nebo odmítnutí.<br /><br /> - `4 - rejected`– hello předplatné požadavek byl odmítnut správcem.<br /><br /> - `5 - cancelled`– hello předplatné zrušil správce nebo vývojáře hello.|  
|displayName|Řetězec|Zobrazovaný název hello předplatného.|  
|Datum vytvoření|Data a času|Hello datum hello odběr byl vytvořen, ve formátu ISO 8601: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|Logická hodnota|Jestli hello předplatné můžete zrušit aktuální uživatel hello.|  
|IsAwaitingApproval|Logická hodnota|Jestli hello předplatného čeká na schválení.|  
|Počátečním|Data a času|Hello počáteční datum pro předplatné hello ve formátu ISO 8601: `2014-06-24T16:25:00Z`.|  
|Datumvypršení platnosti|Data a času|Datum vypršení platnosti Hello hello předplatné, ve formátu ISO 8601: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Data a času|data oznámení Hello hello odběru, ve formátu ISO 8601: `2014-06-24T16:25:00Z`.|  
|primaryKey|Řetězec|Hello předplatné primární klíč. Maximální délka je 256 znaků.|  
|sekundární klíč|Řetězec|klíč Hello sekundární předplatného. Maximální délka je 256 znaků.|  
|CanBeRenewed|Logická hodnota|Jestli může být hello předplatné obnoveno hello aktuální uživatel.|  
|HasExpired|Logická hodnota|Zda vypršela platnost předplatného hello.|  
|IsRejected|Logická hodnota|Tom, zda text hello předplatné žádost byla zamítnuta.|  
|cancelUrl|Řetězec|Hello relativní adresa Url toocancel hello předplatné.|  
|RenewUrl|Řetězec|Hello relativní adresa Url toorenew hello předplatné.|  
  
##  <a name="SubscriptionSummary"></a>Předplatné souhrn  
 Hello `subscription summary` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|ID|Řetězec|Identifikátor prostředku. Jednoznačně identifikuje hello předplatného v rámci hello aktuální instanci služby API Management. Hello hodnota je platná relativní adresa URL ve formátu hello `subscriptions/{sid}` kde `{sid}` je identifikátor předplatného. Tato vlastnost je jen pro čtení.|  
|displayName|Řetězec|Hello zobrazovaný název předplatného hello|  
  
##  <a name="UserAccountInfo"></a>Informace o účtu uživatele  
 Hello `user account info` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|FirstName|Řetězec|Křestní jméno. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|Příjmení|Řetězec|Příjmení. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|E-mail|Řetězec|E-mailovou adresu. Nesmí být prázdný a musí být jedinečné v rámci instance služby hello. Maximální délka je 254 znaků.|  
|Heslo|Řetězec|Heslo uživatelského účtu.|  
|NameIdentifier|Řetězec|Identifikátor účtů, hello stejné jako hello uživatele e-mailu.|  
|ProviderName|Řetězec|Název zprostředkovatele ověřování.|  
|IsBasicAccount|Logická hodnota|Hodnota TRUE, pokud tento účet byl zaregistrován pomocí e-mailu a heslo; false, pokud účet hello byl zaregistrován pomocí zprostředkovatele.|  
  
##  <a name="UseSignIn"></a>Přihlášení uživatele  
 Hello `user sign in` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|E-mail|Řetězec|E-mailovou adresu. Nesmí být prázdný a musí být jedinečné v rámci instance služby hello. Maximální délka je 254 znaků.|  
|Heslo|Řetězec|Heslo uživatelského účtu.|  
|ReturnUrl|Řetězec|Adresa URL Hello hello stránky, kde uživatel hello klikl na přihlášení.|  
|Obdobou|Logická hodnota|Jestli toosave hello informace o aktuálním uživateli.|  
|RegistrationEnabled|Logická hodnota|Zda je povolena registrace.|  
|DelegationEnabled|Logická hodnota|Jestli je povolené delegované přihlášení.|  
|DelegationUrl|Řetězec|Hello delegována přihlašovací adresa url, pokud je povoleno.|  
|SsoSignUpUrl|Řetězec|Hello jeden přihlaste na adresu URL pro uživatele hello, pokud je k dispozici.|  
|AuxServiceUrl|Řetězec|Pokud hello aktuální uživatel není správcem, je to instance služby toohello odkaz v hello portálu Azure Classic.|  
|Poskytovatelé|Kolekce [zprostředkovatele](#Provider) entity|Hello zprostředkovatele ověřování pro tohoto uživatele.|  
|UserRegistrationTerms|Řetězec|Podmínky, které musí uživatel souhlasit toobefore přihlášení.|  
|UserRegistrationTermsEnabled|Logická hodnota|Jestli jsou povolené podmínky.|  
  
##  <a name="UserSignUp"></a>Registrace uživatele  
 Hello `user sign up` entita, která má následující vlastnosti hello.  
  
|Vlastnost|Typ|Popis|  
|--------------|----------|-----------------|  
|PasswordConfirm|Logická hodnota|Hodnota používaná metodou hello [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|Heslo|Řetězec|Heslo uživatelského účtu.|  
|PasswordVerdictLevel|Číslo|Hodnota používaná metodou hello [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|UserRegistrationTerms|Řetězec|Podmínky, které musí uživatel souhlasit toobefore přihlášení.|  
|UserRegistrationTermsOptions|Číslo|Hodnota používaná metodou hello [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|ConsentAccepted|Logická hodnota|Hodnota používaná metodou hello [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|E-mail|Řetězec|E-mailovou adresu. Nesmí být prázdný a musí být jedinečné v rámci instance služby hello. Maximální délka je 254 znaků.|  
|FirstName|Řetězec|Křestní jméno. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|Příjmení|Řetězec|Příjmení. Nesmí být prázdný. Maximální délka je 100 znaků.|  
|UserData|Řetězec|Hodnota používaná metodou hello [registrace](api-management-page-controls.md#sign-up) ovládacího prvku.|  
|NameIdentifier|Řetězec|Hodnota používaná metodou hello [registrace](api-management-page-controls.md#sign-up)registrace ovládacího prvku.|  
|ProviderName|Řetězec|Název zprostředkovatele ověřování.|

## <a name="next-steps"></a>Další kroky
Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).
