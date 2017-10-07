---
title: "aaaDeveloper pokyny pro Azure Active Directory podmíněného přístupu | Microsoft Docs"
description: "Scénáře pro podmíněný přístup k Azure AD a Průvodce pro vývojáře"
services: active-directory
keywords: 
author: danieldobalian
manager: mbaldwin
editor: PatAltimore
ms.author: dadobali
ms.date: 07/19/2017
ms.assetid: 115bdab2-e1fd-4403-ac15-d4195e24ac95
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.openlocfilehash: 589393f5d084d64872b372d895dc889f300592bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>Průvodce pro vývojáře pro podmíněný přístup k Azure Active Directory

Azure Active Directory (AD) nabízí několik způsobů toosecure vaší aplikace a chránit službu.  Jedním z těchto jedinečné funkce je podmíněného přístupu.  Podmíněný přístup umožňuje vývojářům a podnikové zákazníky tooprotect služby v mnoho způsobů, včetně:

* Ověřování pomocí služby Multi-Factor Authentication
* Povolení jenom Intune zaregistrovaná zařízení tooaccess specifických služeb
* Omezení umístění uživatele a IP rozsahy

Další informace o všech možnostech hello podmíněného přístupu najdete v tématu [podmíněného přístupu v hello portál Azure classic](../active-directory-conditional-access.md). 

V tomto článku se zaměříme na jaké podmíněného přístupu znamená toodevelopers vytváření aplikací pro Azure AD.  Předpokládá znalost [jeden](active-directory-integrating-applications.md) a [víceklientské](active-directory-devhowto-multi-tenant-overview.md) aplikace a [běžných vzorů ověřování](active-directory-authentication-scenarios.md).

Jsme vám podrobně hello dopad přístup k prostředkům, které máte řízení přes, které mohou mít zásady podmíněného přístupu.  Kromě toho jsme hello dopad podmíněného přístupu v hello tok on-behalf-of, webové aplikace, prozkoumejte přístup hello Microsoft Graph a volání rozhraní API.

## <a name="how-does-conditional-access-impact-an-app"></a>Jak podmíněný přístup ovlivňuje aplikace?

### <a name="app-topologies-impacted"></a>Topologie aplikace dopad

Podmíněný přístup v nejběžnější případy, nezmění chování aplikace nebo vyžaduje změny z hello developer.  Pouze v určitých případech při aplikaci nepřímo nebo tiché požadavky token pro služby, aplikace vyžaduje kód změní toohandle podmíněný přístup "ověření".  To může být stejně jednoduché jako provádění požadavek interaktivní přihlašování. 

Konkrétně hello následující scénáře vyžadují kód toohandle podmíněného přístupu "výzvy": 

* Přístup hello Microsoft Graph aplikací
* Provádění hello tok on-behalf-of aplikace
* Přístup k více služby nebo prostředky aplikace
* Jednostránkové aplikace pomocí ADAL.js

Zásady podmíněného přístupu může být použité toohello aplikace, ale také může být použité tooa webového rozhraní API aplikace přistupuje. toolearn Další informace o tom, jak tooconfigure zásady podmíněného přístupu, najdete v tématu [Začínáme s Azure Active Directory podmíněného přístupu](../active-directory-conditional-access-azuread-connected-apps.md#configure-per-application-access-rules).

V závislosti na scénáři hello můžete použít podnikových zákazníků a kdykoli odebrat zásady podmíněného přístupu.  Aby vaše aplikace toocontinue funguje při použití nové zásady je nutné zpracování výzvy"tooimplement hello". Hello následující příklady znázorňují výzvy zpracování. 

### <a name="conditional-access-examples"></a>Příklady podmíněného přístupu

Některé scénáře vyžadují kód změny toohandle podmíněného přístupu, zatímco ostatní pracovat, jako je.  Tady je několik scénářů, pomocí podmíněného přístupu toodo služby Multi-Factor authentication, která poskytuje určitý pohled na hello rozdíl.

* Vytváříte jednoho klienta iOS aplikace a aplikovat zásady podmíněného přístupu.  aplikace Hello přihlásí uživatel a nemá požadovat přístup tooan API.  Když hello uživatel přihlásí, hello zásad se automaticky vyvolá a hello uživatel potřebuje tooperform vícefaktorové ověřování (MFA). 
* Vytváříte víceklientské webovou aplikaci, která používá hello Microsoft Graph tooaccess systému Exchange, mezi další služby.  Enterprise zákazníkovi, který přijme tuto aplikaci nastaví zásadu na SharePoint Online.  Pokud webová aplikace hello požádá o token pro MS Graph, se použije zásadu na všechny Microsoft Service (konkrétně služby, které je přístupné prostřednictvím grafu).  Tento koncový uživatel je vyzván k MFA. V případě hello hello koncový uživatel přihlášený pomocí platné tokeny, deklarace identity "výzvu" je vrácen toohello webové aplikace.  
* Vytváříte nativní aplikaci, která používá tooaccess hello střední vrstvy služby Microsoft Graph.  Enterprise zákazníka hello firmy pomocí této aplikace platí tooExchange zásad, který je Online.  Když koncový uživatel přihlásí, nativní aplikaci hello požaduje přístup toohello střední vrstvy a odešle hello token.  střední vrstvy Hello provede tok on-behalf-of toorequest přístup toohello MS Graph.  V tomto okamžiku zobrazí deklarace identity "výzvu" toohello střední vrstvy. Hello střední vrstvy zasílá hello výzvy back toohello nativní aplikaci, která potřebuje toocomply se zásadami podmíněného přístupu hello.

### <a name="complying-with-a-conditional-access-policy"></a>V souladu se zásadami podmíněného přístupu

Pro několik topologií jiné aplikace. zásady podmíněného přístupu vyhodnocení při hello relace.  Jako zásady podmíněného přístupu na hello členitost aplikace a služby, hello bod, ve kterém je volána výraznou závisí na scénáři hello se snažíte tooaccomplish.

Když se aplikace pokusí tooaccess služby se zásadami podmíněného přístupu, může dojít k výzvy podmíněného přístupu.  Tento problém je zakódován do hello `claims` parametr, který se dodává v odpovědi z Azure AD nebo hello Microsoft Graph.  Tady je příklad tohoto parametru výzvy: 

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Vývojáři můžou trvat tento problém a přidá je do nové žádosti o tooAzure AD.  Předávání tento stav vyzve všechny akce nezbytné toocomply se zásadami podmíněného přístupu hello tooperform hello koncového uživatele. V následujících scénářích hello jsou vysvětleny specifika hello chyby a jak tooextract hello parametr. 

## <a name="scenarios"></a>Scénáře

### <a name="prerequisites"></a>Požadavky

Podmíněný přístup pro Azure AD je součástí funkce [Azure AD Premium](../active-directory-whatis.md#choose-an-edition).  Další informace o licenční požadavky v hello [zprávu nelicencovaná využití](../active-directory-conditional-access-unlicensed-usage-report.md).  Vývojáři se můžete zapojit do hello [Microsoft Developer Network](https://msdn.microsoft.com/dn308572.aspx), což zahrnuje toohello bezplatné předplatné Enterprise Mobility Suite, které zahrnuje Azure AD Premium.

### <a name="considerations-for-specific-scenarios"></a>Důležité informace týkající se konkrétních scénářů

Následující informace pouze Hello platí v těchto scénářích podmíněného přístupu:

* Přístup hello Microsoft Graph aplikací
* Provádění hello tok on-behalf-of aplikace
* Přístup k více služby nebo prostředky aplikace
* Jednostránkové aplikace pomocí ADAL.js

V následujících částech hello budete získáme do běžné scénáře, ve kterých jsou složitější.  Princip funkce základní Hello je podmíněného přístupu, které se vyhodnocují zásady ve hello dobu, kdy se požaduje hello token pro hello službu, která se má použít, pokud je přistupuje prostřednictvím hello Microsoft Graph zásady podmíněného přístupu.

### <a name="scenario-app-accessing-hello-microsoft-graph"></a>Scénář: Aplikace přístup k hello Microsoft Graph

V tomto scénáři jsme provede hello případ když webové aplikace požaduje přístup toohello Microsoft Graph. zásady podmíněného přístupu Hello v takovém případě může být přiřazen tooSharePoint, Exchange nebo jiné služby, které je přístupné jako zatížení prostřednictvím hello Microsoft Graph.  V tomto příkladu předpokládejme, že je zásada podmíněného přístupu na Sharepoint Online.

![Hello Microsoft Graph vývojový diagram přístupu k aplikaci](media/active-directory-conditional-access-developer/app-accessing-microsoft-graph-scenario.png)

aplikace Hello poprvé požádá autorizace toohello Microsoft Graph který vyžaduje přístup k podřízené úlohy bez podmíněného přístupu.  Hello neproběhne bez vyvolání všechny zásady a aplikace hello přijímá tokeny pro Microsoft Graph.  V tomto okamžiku může aplikace hello používat hello přístupový token v nosiče požadavku pro koncový bod hello požadovaný. Nyní hello aplikace potřebuje tooaccess koncový bod služby Sharepoint Online z Microsoft Graph, například:`https://graph.microsoft.com/v1.0/me/mySite`

aplikace Hello již má platný token pro hello Microsoft Graph, aby mohl vykonávat novou žádost o hello bez vystavení nový token. Tento požadavek selže a je výzva deklarace identity vystavené Microsoft Graph tvar hello protokolu HTTP 403 Zakázáno s ```WWW-Authenticate``` výzvu.
Tady je příklad hello odpovědi: 

```
HTTP 403; Forbidden 
error=insufficient_claims
www-authenticate="Bearer realm="", authorization_uri="https://login.windows.net/common/oauth2/authorize", client_id="<GUID>", error=insufficient_claims, claims={"access_token":{"polids":{"essential":true,"values":["<GUID>"]}}}"
```

Hello výzvy deklarací identity je uvnitř hello ```WWW-Authenticate``` hlavičky, která může být parametr deklarace identity hello Analyzovaná tooextract pro další požadavek hello.  Jakmile je novou žádost o připojení toohello, Azure AD zná zásady podmíněného přístupu hello tooevaluate při přihlášení uživatele hello a hello aplikace je nyní souladu se zásadami podmíněného přístupu hello.  Koncový bod je úspěšné opakovaných hello požadavek toohello Sharepoint Online.

Ukázky kódu, která ukazují, jak toohandle hello deklarací výzvy, najdete v části toohello [ukázka kódu .NET plochy](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) ADAL .NET nebo hello [ukázka kódu On-behalf-of](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) pro ADAL .NET.

### <a name="scenario-app-performing-hello-on-behalf-of-flow"></a>Scénář: Aplikace provádění hello tok on-behalf-of

V tomto scénáři jsme provede hello případ, ve kterém nativní aplikaci volání webové služby nebo API.  Pak se tato služba nemá [hello "tok on-behalf-of"](active-directory-authentication-scenarios.md#application-types-and-scenarios) toocall podřízené služby.  V našem případě jsme provedli naše webu podřízené služby pro podmíněný přístup zásady toohello (webovém rozhraní API 2) a používají nativní aplikaci spíše než server/démon aplikace. 

![Aplikace provádění hello on-behalf-of vývojový diagram](media/active-directory-conditional-access-developer/app-performing-on-behalf-of-scenario.png)

Hello počáteční žádosti o token pro webové rozhraní API 1 nezobrazí výzvu hello koncového uživatele pro službu Multi-Factor authentication jako Web API 1 nemusí vždy dosáhl hello podřízené rozhraní API.  Jakmile Web API 1 pokusí toorequest tokenu hello on-behalf-of uživatele pro webové rozhraní API 2, hello požadavek selže, protože hello uživatel není přihlášený pomocí služby Multi-Factor authentication.

Azure AD vrátí odpověď HTTP s některé zajímavé údaje: 

> [!NOTE]
> V této instanci je popis chyby služby Multi-Factor authentication, ale je širokou škálu `interaction_required` možné odpovídající tooconditional přístup.  

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must use multi-factor authentication tooaccess '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

V našem webové rozhraní API 1, jsme catch hello chyba `error=interaction_required`a odeslat zpět hello `claims` výzvy toohello aplikace na ploše.  V tomto okamžiku můžete provést nové aplikace na ploše hello `acquireToken()` zavolat a připojit hello `claims`problém jako parametr řetězce dotazu navíc.  Tento nový požadavek vyžaduje hello uživatele toodo vícefaktorové ověřování a potom odešlete tento nový token back tooWeb 1 rozhraní API a kompletní hello on-behalf-of toku.

tootry se v tomto scénáři najdete v našich [ukázka kódu .NET](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  Ukazuje, jak toopass hello deklarace identity výzvy zpět z webové rozhraní API 1 toohello nativní aplikaci a vytvořit novou žádost o uvnitř hello klientskou aplikaci. 

### <a name="scenario-app-accessing-multiple-services"></a>Scénář: Aplikace přístup k několika služeb

V tomto scénáři jsme provede případ hello ve kterém přistupuje k webové aplikaci dvě služby, z nichž jeden je přiřazen zásady podmíněného přístupu.  V závislosti na logice aplikace můžou existovat cesta, ve kterém aplikace nevyžaduje přístup tooboth webové služby.  V tomto scénáři hello pořadí, ve kterém můžete požádat o token hraje důležitou roli v hello činnost koncového uživatele.

Předpokládejme máme webové služby A a B a webová služba B má naše zásady podmíněného přístupu použije.  Při požadavku počáteční interaktivní auth hello vyžaduje souhlas pro obě služby, hello zásady podmíněného přístupu není potřeba ve všech případech.  Pokud aplikace hello požádá o token pro webovou službu B, vyvolání hello zásad a následné žádosti pro webovou službu A také úspěšné následujícím způsobem.

![Aplikace přístup k více služeb vývojový diagram](media/active-directory-conditional-access-developer/app-accessing-multiple-services-scenario.png)

Případně pokud aplikace hello původně požaduje token pro webové služby A, koncových uživatelů hello nevyvolá hello zásady podmíněného přístupu.  To umožňuje vývojáři aplikace hello činnost koncového uživatele hello toocontrol a bez vynucení toobe zásady podmíněného přístupu hello vyvolat ve všech případech. Hello složité případem je, pokud aplikace hello následně vyžaduje token pro webové služby serveru B. V tomto okamžiku hello koncový uživatel musí toocomply se zásadami podmíněného přístupu hello.  Když se aplikace hello pokusí příliš`acquireToken`, to může generovat hello následující chybu (znázorněné v následujícím diagramu hello): 

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must use multi-factor authentication tooaccess '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
``` 

![Přístup k více služeb, které žádají o nový token aplikace](media/active-directory-conditional-access-developer/app-accessing-multiple-services-new-token.png)

Pokud aplikace hello je pomocí knihovny ADAL hello, token hello tooacquire selhání je vždy opakovat interaktivně.  Když dojde k této žádosti interaktivní, koncový uživatel hello musí hello možnost toocomply s podmíněným přístupem hello.  Toto je hodnota true, pokud je požadavek hello `AcquireTokenSilentAsync` nebo `PromptBehavior.Never` v takovém případě aplikace hello musí tooperform interaktivní ```AcquireToken``` požadavku toogive hello koncového užití hello možnost toocomply zásadám hello. 

### <a name="scenario-single-page-app-spa-using-adaljs"></a>Scénář: Jednu stránku aplikace (SPA) pomocí ADAL.js

V tomto scénáři jsme provede hello případ, když máme jednostránkové aplikace (SPA), pomocí ADAL.js toocall podmíněného přístupu chráněné webového rozhraní API.  To je jednoduché architekturou, ale má některé drobné odlišnosti, které je třeba vzít v úvahu při vývoji kolem podmíněného přístupu toobe.

V ADAL.js, existuje několik funkcí, které získat tokeny: `login()`, `acquireToken(...)`, `acquireTokenPopup(…)`, a `acquireTokenRedirect(…)`. 

* `login()`Získá token ID prostřednictvím požadavek interaktivní přihlašování, ale není získali přístupové tokeny pro všechny služby (včetně podmíněného přístupu k chráněným webového rozhraní API).  
* `acquireToken(…)`Poté můžete být použité toosilently získat token přístupu, což znamená, že se za žádných okolností nezobrazuje uživatelského rozhraní.  
* `acquireTokenPopup(…)`a `acquireTokenRedirect(…)` jsou obě použité toointeractively vyžádání tokenu pro prostředek, což znamená, budou vždy zobrazovat přihlašovací rozhraní.

Pokud aplikace potřebuje přístupu k tokenu toocall webového rozhraní API, všechny pokusy `acquireToken(…)`.  Pokud je platnost tokenu relace hello nebo potřebujeme toocomply se zásadami podmíněného přístupu, pak hello *acquireToken* funkce selže a hello používá aplikace `acquireTokenPopup()` nebo `acquireTokenRedirect()`.

![Jednostránkové aplikace pomocí ADAL vývojový diagram](media/active-directory-conditional-access-developer/spa-using-adal-scenario.png)

Projděme příklad s náš scénář podmíněného přístupu.  koncový uživatel Hello právě dostali v lokalitě hello a nemá relaci.  Můžeme provádět `login()` volání, získání ID tokenu bez ověřování Multi-Factor authentication.  Potom uživatel hello dotkne tlačítko, které vyžaduje hello aplikace toorequest dat z webového rozhraní API.  aplikace Hello pokusí toodo `acquireToken()` volání ale selže, protože hello uživatele služby Multi-Factor authentication dosud neprovedl a je třeba toocomply se zásadami podmíněného přístupu hello.

Azure AD odesílá zpět hello následující odpověď HTTP: 

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must use multi-factor authentication tooaccess '<Web API App/Client ID>'.
```

Naše aplikace potřebuje toocatch hello `error=interaction_required`.  Hello aplikace pak může použít buď `acquireTokenPopup()` nebo `acquireTokenRedirect()` na stejné hello prostředků.  uživatel Hello je toodo vynucené použití služby Multi-Factor authentication. Po dokončení hello vícefaktorového ověřování uživatele hello hello aplikace se objeví čerstvou přístupový token pro hello požadovaný prostředek.

tootry se v tomto scénáři najdete v našich [ukázka kódu On-behalf-of JS SPA](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  Tento příklad používá hello zásady podmíněného přístupu a webového rozhraní API je zaregistrována dříve JS SPA toodemonstrate tento scénář. Zobrazuje, jak tooproperly popisovač hello deklarací výzvu a získat přístupový token, který lze použít pro webové rozhraní API. Případně, najdete v článku věnovaném hello Obecné [ukázka kódu Angular.js](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) pokyny k úhlová SPA


## <a name="see-also"></a>Viz také

* najdete v části toolearn Další informace o možnosti hello [podmíněného přístupu ve službě Azure AD](../active-directory-conditional-access.md).
* Ukázky kódu více Azure AD, najdete v části [úložiště Github ukázek kódu](https://github.com/azure-samples?utf8=%E2%9C%93&q=active-directory). 
* Další informace o hello ADAL SDK a přístup hello referenční dokumentaci k nástroji, najdete v části [příručku knihovny](active-directory-authentication-libraries.md).
* toolearn Další informace o scénářích více klientů, najdete v části [jak toosign v uživatele, kteří používají vzor víceklientské hello](active-directory-devhowto-multi-tenant-overview.md).
