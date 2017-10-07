---
title: "aaaAzure Glosář vývojáře Active Directory | Microsoft Docs"
description: "Seznam podmínek pro běžně používané funkce a koncepty pro vývojáře Azure Active Directory."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 551512df-46fb-4219-a14b-9c9fc23998ba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 32a6a6194b8d01409159a2b60263bb66a87a843c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-developer-glossary"></a>Glosář vývojáře Azure Active Directory
Tento článek obsahuje definice pro některé hello základní Azure Active Directory (AD) koncepty pro vývojáře, což je užitečné při získávání informací o vývoj aplikací pro Azure AD.

## <a name="access-token"></a>Přístupový token
Typ [token zabezpečení](#security-token) vydaný [serveru ověřování](#authorization-server)a použít [klientská aplikace](#client-application) v pořadí tooaccess [chráněný server prostředků ](#resource-server). Obvykle ve formě hello [JSON Web Token (JWT)][JWT], hello token ztělesňuje hello registraci toohello klienta vydaného hello [vlastník prostředku](#resource-owner), pro požadované úrovně přístupu. Hello token obsahuje všechny použitelné [deklarace identity](#claim) o subjektu hello povolení hello klienta aplikace toouse ji jako formulář pověření při přístupu k danému prostředku. Tím se eliminuje také hello potřebu hello prostředků vlastníka tooexpose pověření toohello klienta.

Přístupové tokeny jsou někdy označují tooas "Uživatele + aplikace" nebo "Aplikaci jen", v závislosti na hello přihlašovací údaje reprezentované. Například, když používá klientská aplikace:

* [Udělení autorizace "Autorizační kód"](#authorization-grant)hello koncový uživatel se ověřuje nejprve jako vlastník prostředku hello, delegování autorizace toohello klienta tooaccess hello prostředků. Hello klienta ověřuje následně při získávání hello přístupový token. Hello token může být někdy označují toomore konkrétně jako token "Uživatele + aplikace", jak reprezentuje obou hello uživatele, který oprávnění hello klientská aplikace a aplikace hello.
* [Udělení autorizace "Pověření klienta"](#authorization-grant)hello klienta poskytuje hello jedinou ověřování, funguje bez hello prostředků vlastníka ověřování/autorizace, tak hello token v některých případech může být odkazované tooas token "Jen aplikace".

V tématu [odkaz tokenu Azure AD] [ AAD-Tokens-Claims] další podrobnosti.

## <a name="application-manifest"></a>Manifest aplikace
Funkce poskytované hello [portál Azure][AZURE-portal], který vytvoří reprezentaci JSON konfigurace identity aplikace hello, používá jako mechanismus pro aktualizaci přidružené [ Aplikace] [ AAD-Graph-App-Entity] a [ServicePrincipal] [ AAD-Graph-Sp-Entity] entity. V tématu [manifest aplikace Azure Active Directory hello Principy] [ AAD-App-Manifest] další podrobnosti.

## <a name="application-object"></a>objekt aplikace
Když je registrace nebo aktualizovat aplikaci hello [portál Azure][AZURE-portal], portál hello vytvoření nebo aktualizace objekt aplikace a odpovídající [objekt zabezpečení služby](#service-principal-object)pro tohoto klienta. objekt aplikace Hello *definuje* hello konfigurace identity aplikace globálně (v rámci všech klientů, kde má přístup k), poskytuje šablony, ze kterého se odpovídající hlavní objekty služby  *odvozené* pro použití místně za běhu (v konkrétní klienta).

V tématu [aplikací a hlavní objekty služeb] [ AAD-App-SP-Objects] Další informace.

## <a name="application-registration"></a>registrace aplikace
V pořadí tooallow služby toointegrate aplikace s a delegáta identita a správa přístupu funkce tooAzure AD, musí být zaregistrován u služby Azure AD [klienta](#tenant). Při registraci vaší aplikace s Azure AD, poskytují informace o nastavení identity pro vaši aplikaci umožní tak jeho toointegrate s Azure AD a pomocí funkce, jako například:

* Robustní správu z jednotného přihlašování pomocí Azure AD Identity Management a [OpenID Connect] [ OpenIDConnect] implementace protokolu
* Zprostředkované přístup příliš[chráněné zdroje](#resource-server) podle [klientské aplikace](#client-application), přes Azure AD OAuth 2.0 [serveru ověřování](#authorization-server) implementace
* [Souhlas framework](#consent) pro správu prostředků tooprotected klienta přístup podle autorizace vlastníka prostředku.

V tématu [integrace aplikací s Azure Active Directory] [ AAD-Integrating-Apps] další podrobnosti.

## <a name="authentication"></a>Ověřování
Hello operace náročné strany na oprávněné pověření, poskytuje hello základ pro vytvoření hlavní toobe zabezpečení používaných pro řízení přístupu a identit. Během [udělení autorizace OAuth2](#authorization-grant) například hello strany ověřování probíhá naplňování hello role buď [vlastník prostředku](#resource-owner) nebo [klientská aplikace](#client-application), v závislosti na udělení Hello používá.

## <a name="authorization"></a>Autorizace
Hello act udělení něco toodo hlavní oprávnění ověření zabezpečení. Existují dva případy použití primární v programovací model hello Azure AD:

* Během [udělení autorizace OAuth2](#authorization-grant) toku: když hello [vlastník prostředku](#resource-owner) uděluje autorizace toohello [klientská aplikace](#client-application), umožňuje tak klientským hello tooaccess hello vlastník prostředku prostředky.
* Během přístupu k prostředkům klientem hello: jak je implementované hello [server prostředků](#resource-server), pomocí hello [deklarace identity](#claim) hodnoty k dispozici v hello [přístupový token](#access-token) toomake řízení přístupu rozhodnutí, která na jejich základě.

## <a name="authorization-code"></a>Autorizační kód
Krátkou životnost "token" zadat tooa [klientská aplikace](#client-application) podle hello [koncový bod autorizace](#authorization-endpoint), v rámci toku "autorizační kód" hello, jeden z hello čtyři OAuth2 [uděluje autorizace ](#authorization-grant). Hello kód je vrácený v odpovědi tooauthentication z toohello klientská aplikace [vlastník prostředku](#resource-owner), označující vlastník prostředku hello delegoval autorizace tooaccess hello požadované prostředky. V rámci toku hello kód hello později uplatněný pro [přístupový token](#access-token).

## <a name="authorization-endpoint"></a>Koncový bod autorizace
Jeden z koncových bodů hello implementované hello [serveru ověřování](#authorization-server), použít toointeract s hello [vlastník prostředku](#resource-owner) v pořadí tooprovide [udělení autorizace](#authorization-grant) během tok udělení autorizace OAuth2. V závislosti na hello autorizace tok poskytování používá, hello skutečné grant zadat se může lišit, včetně [autorizační kód](#authorization-code) nebo [token zabezpečení](#security-token).

Najdete hello OAuth2 specifikaci [typy udělení autorizace] [ OAuth2-AuthZ-Grant-Types] a [koncový bod autorizace] [ OAuth2-AuthZ-Endpoint] části a hello [OpenIDConnect specifikace] [ OpenIDConnect-AuthZ-Endpoint] další podrobnosti.

## <a name="authorization-grant"></a>udělení autorizace
Pověření představující hello [vlastník prostředku](#resource-owner) [autorizace](#authorization) tooaccess jeho chráněným prostředkům udělena tooa [klientská aplikace](#client-application). Klientská aplikace můžete použít jednu z hello [čtyři typy definované hello OAuth2 autorizace Framework udělení] [ OAuth2-AuthZ-Grant-Types] tooobtain udělení, v závislosti na typu nebo požadavky na klienta: "udělení autorizačního kódu", " udělení pověření klienta","implicitní grant"a"udělit oprávnění hesla vlastníka prostředku". pověření Hello vrátila toohello klienta je buď [přístupový token](#access-token), nebo [autorizační kód](#authorization-code) (vyměňují později pro přístupový token), v závislosti na typu hello používá udělení autorizace.

## <a name="authorization-server"></a>Autorizace serveru
Podle definice hello [OAuth2 autorizace Framework][OAuth2-Role-Def], server hello odpovědné za vydání přístup tokeny toohello [klienta](#client-application) po úspěšném ověření Hello [vlastník prostředku](#resource-owner) a získání jeho povolení. A [klientská aplikace](#client-application) komunikuje se službou serveru ověřování hello za běhu prostřednictvím jeho [autorizace](#authorization-endpoint) a [tokenu](#token-endpoint) koncových bodů, v souladu s hello OAuth2 definované [autorizace uděluje](#authorization-grant).

V případě hello integrace aplikace Azure AD, Azure AD implementuje hello autorizace role serveru pro aplikace Azure AD a služby Microsoft rozhraní API, například [Microsoft Graph API][Microsoft-Graph].

## <a name="claim"></a>Deklarace identity
A [token zabezpečení](#security-token) obsahuje deklarace identity, které poskytují kontrolní výrazy o jedné entity (například [klientská aplikace](#client-application) nebo [vlastník prostředku](#resource-owner)) tooanother entity (například hello [server prostředků](#resource-server)). Deklarace identity jsou páry název/hodnota, které předávání faktů o token subjektu hello (například objekt zabezpečení hello, který byl ověřována hello [serveru ověřování](#authorization-server)). deklarace identity Hello token obsahuje daný jsou závislé na několika proměnných, včetně hello typ tokenu, hello typ pověření používaná tooauthenticate hello předmětu, konfigurace aplikace hello atd.

V tématu [odkaz tokenu Azure AD] [ AAD-Tokens-Claims] další podrobnosti.

## <a name="client-application"></a>klientské aplikace
Podle definice hello [OAuth2 autorizace Framework][OAuth2-Role-Def], aplikace, která umožňuje chráněných prostředků požadavky jménem hello [vlastník prostředku](#resource-owner). Hello termín "client" není určeno žádné implementace vlastností konkrétní hardwaru (například jestli hello aplikace spustí na serveru, ploše nebo jiné zařízení).  

Klientská aplikace požaduje [autorizace](#authorization) z tooparticipate vlastníka prostředků v [udělení autorizace OAuth2](#authorization-grant) toku a může získat přístup k rozhraní API nebo data jménem vlastník prostředku hello. Hello Framework ověřování OAuth2 [definuje dva typy klientů][OAuth2-Client-Types], "důvěrné informace" a "veřejná", podle hello klienta možnost toomaintain hello důvěrnost svoje přihlašovací údaje. Aplikace můžete implementovat [webového klienta (důvěrné)](#web-client) na webovém serveru, která se spouští [nativního klienta (veřejných)](#native-client) instalovat na zařízení, nebo [klienta na základě uživatelského agenta (veřejných)](#user-agent-based-client)která se spouští v prohlížeči zařízení.

## <a name="consent"></a>Vyjádřit souhlas.
Hello proces [vlastník prostředku](#resource-owner) udělení autorizace tooa [klientská aplikace](#client-application), tooaccess chráněné zdroje v konkrétní [oprávnění](#permissions), jménem hello vlastník prostředku. V závislosti na požadované klientem hello hello oprávnění správce nebo uživatel bude požádán souhlasu tooallow přístup tootheir organizace nebo jednotlivce dat v uvedeném pořadí. Poznámka: v [víceklientské](#multi-tenant-application) scénář, aplikace hello [instanční objekt](#service-principal-object) je také zaznamenávají v klientovi hello hello souhlas uživatele.

## <a name="id-token"></a>ID tokenu
[OpenID Connect] [ OpenIDConnect-ID-Token] [token zabezpečení](#security-token) poskytované [serveru ověřování](#authorization-server) [koncový bod autorizace](#authorization-endpoint), který obsahuje [deklarace identity](#claim) odpovídající toohello ověřování koncového uživatele [vlastník prostředku](#resource-owner). Jako přístupový token, jsou také reprezentované tokeny typu ID jako digitálně podepsaných [JSON Web Token (JWT)][JWT]. Na rozdíl od přístupový token, ale ID token deklarace nepoužívají se pro účely související tooresource přístup a konkrétně řízení přístupu.

V tématu [odkaz tokenu Azure AD] [ AAD-Tokens-Claims] další podrobnosti.

## <a name="multi-tenant-application"></a>víceklientské aplikace
Třída aplikace, která umožňuje přihlášení a [souhlas](#consent) uživatelé zřízené v žádné službě Azure AD [klienta](#tenant), včetně klienty než hello jeden kde hello klient zaregistrovaný. [Nativní klient](#native-client) aplikace jsou víceklientské ve výchozím nastavení, zatímco [webového klienta](#web-client) a [webové prostředků nebo rozhraní API](#resource-server) aplikací mít možnost tooselect hello mezi jedním nebo více klientů. Naopak webové aplikace registrovaný jako jednoho klienta, by pouze povolí přihlášení z uživatelské účty, které jsou zřízené v hello stejné klienta jako hello jeden kde registrovaná aplikace hello.

V tématu [jak toosign v žádné uživatele Azure AD pomocí hello vzor aplikací víceklientské] [ AAD-Multi-Tenant-Overview] další podrobnosti.

## <a name="native-client"></a>Nativní klient systému
Typ [klientská aplikace](#client-application) je nativní aplikace na zařízení. Vzhledem k tomu, že veškerý kód se spustí na zařízení, bude považován za "veřejná" klienta z důvodu tooits nemohou toostore pověření soukromě nebo jako s důvěrnými. V tématu [OAuth2 klienta typy a profily] [ OAuth2-Client-Types] další podrobnosti.

## <a name="permissions"></a>Oprávnění
A [klientská aplikace](#client-application) zvýšení přístup tooa [server prostředků](#resource-server) deklarováním žádosti o oprávnění. K dispozici jsou dva typy:

* "Delegovaný" oprávnění, které určují [obor](#scopes) přístup pomocí delegovaného autorizace z hello přihlášeného [vlastník prostředku](#resource-owner), jsou uvedené toohello prostředků v době spuštění jako ["spojovací bod služby "deklarace identity](#claim) v klientovi hello [přístupový token](#access-token).
* Oprávnění "Aplikace", které určují [na základě rolí](#roles) přistupovat pomocí přihlašovacích údajů nebo identitu aplikace hello klienta, jsou uvedené toohello prostředků v době spuštění jako [deklarace identity "role"](#claim) v hello token přístupu klienta.

Budou také surface během hello [souhlas](#consent) procesu, udělíte hello správce nebo prostředků vlastníka hello možnost toogrant nebo odepřít hello klienta přístup tooresources daného klienta.

Žádosti o oprávnění jsou nakonfigurované na hello "Aplikace" / "Nastavení" kartě v hello [portál Azure][AZURE-portal], v části "Požadovaná oprávnění", výběrem hello potřeby "Delegovaná oprávnění" a " Oprávnění aplikací"(hello pozdější vyžaduje členství v roli globálního správce hello). Protože [veřejné klienta](#client-application) nelze bezpečně udržovat přihlašovací údaje, může požadovat pouze delegovaných oprávnění, při [důvěrné klienta](#client-application) má hello možnost toorequest delegovanou a aplikace oprávnění. Hello klienta [objektu application](#application-object) úložiště hello deklarovaný oprávnění v jeho [requiredResourceAccess vlastnost][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>vlastník prostředku
Podle definice hello [OAuth2 autorizace Framework][OAuth2-Role-Def], entity umožňující udělení přístupu tooa chráněný prostředek. Pokud vlastník prostředku hello osoba, je odkazované tooas koncový uživatel. Například když [klientská aplikace](#client-application) chce tooaccess poštovní schránky uživatele prostřednictvím hello [Microsoft Graph API][Microsoft-Graph], vyžaduje oprávnění od vlastníka prostředku hello Hello poštovní schránky.

## <a name="resource-server"></a>server prostředků
Podle definice hello [OAuth2 autorizace Framework][OAuth2-Role-Def], že hostitelé chráněné zdroje, schopný přijímat a reagovat tooprotected prostředků serveru požadavky od [klienta aplikace](#client-application) této přítomen [přístupový token](#access-token). Také označované jako chráněného prostředku serveru nebo prostředků aplikace.

Server prostředků zpřístupňuje rozhraní API a vynucuje přístup k prostředkům tooits chráněné přes [obory](#scopes) a [role](#roles), pomocí hello Framework autorizace OAuth 2.0. Mezi příklady patří hello Azure AD Graph API, která poskytuje přístup k datům klienta tooAzure AD a hello rozhraní API Office 365, které poskytují přístup toodata například pošty a kalendáři. Obě tyto jsou také přístupné prostřednictvím hello [Microsoft Graph API][Microsoft-Graph].  

Stejně jako klientskou aplikaci, je stanoven konfigurace identity prostředků aplikace pomocí [registrace](#application-registration) v klienta Azure AD, poskytuje hello aplikace a služby objekt zabezpečení. Některé poskytovaný společností Microsoft rozhraní API, jako je například hello Azure AD Graph API, předem zaregistrovali objekty služby, které jsou k dispozici ve všech klientů při zřizování.

## <a name="roles"></a>role
Jako [obory](#scopes), role poskytují způsob, jak [server prostředků](#resource-server) tooits toogovern přístup k chráněným prostředkům. Existují dva typy: roli "user" implementuje řízení přístupu na základě rolí pro uživatele nebo skupiny, které vyžadují přístup k prostředku toohello, zatímco implementuje o roli "aplikace" hello stejný pro [klientské aplikace](#client-application) , které vyžadují přístup.

Role jsou definované prostředků řetězců (například "výdajů schvalovatel", "Jen pro čtení", "Directory.ReadWrite.All"), kterou spravuje v hello [portál Azure] [ AZURE-portal] prostřednictvím hello prostředků [aplikace manifest](#application-manifest)a uloženy v hello prostředků [appRoles vlastnost][AAD-Graph-Sp-Entity]. Hello portál Azure je také použít tooassign uživatele příliš role "user" a nakonfigurovat klienta [oprávnění aplikací](#permissions) tooaccess o roli "aplikace".

Podrobnou diskuzi o hello aplikační role vystavené Azure AD Graph API, najdete v části [obory oprávnění rozhraní API grafu][AAD-Graph-Perm-Scopes]. Implementace podrobný příklad najdete v tématu [řízení přístupu v cloudových aplikacích pomocí služby Azure AD na základě Role][Duyshant-Role-Blog].

## <a name="scopes"></a>Obory
Jako [role](#roles), obory poskytují způsob, jak [server prostředků](#resource-server) tooits toogovern přístup k chráněným prostředkům. Obory jsou použité tooimplement [obor] [ OAuth2-Access-Token-Scopes] pro přístup k řízení, [klientská aplikace](#client-application) , nebyla zadána Delegovaný přístup toohello prostředků jeho vlastníka.

Obory jsou definované prostředků řetězců (například "Mail.Read", "Directory.ReadWrite.All"), spravované v hello [portál Azure] [ AZURE-portal] prostřednictvím hello prostředků [manifest aplikace](#application-manifest)a uloženy v hello prostředků [oauth2Permissions vlastnost][AAD-Graph-Sp-Entity]. Hello portál Azure je také použít tooconfigure klientská aplikace [delegovaná oprávnění](#permissions) tooaccess obor.

Nejlepší postup zásady vytváření názvů, je toouse formátu "resource.operation.constraint". Podrobnou diskuzi o hello obory vystavené Azure AD Graph API, najdete v části [obory oprávnění rozhraní API grafu][AAD-Graph-Perm-Scopes]. Obory vystavené službám Office 365, najdete v části [referenční dokumentace rozhraní API Office 365 oprávnění][O365-Perm-Ref].

## <a name="security-token"></a>token zabezpečení
Podepsaný dokument obsahující deklarace identity, například tokenu OAuth2 nebo kontrolního výrazu SAML 2.0. Pro OAuth2 [udělení autorizace](#authorization-grant), [přístupový token](#access-token) (OAuth2) a [ID Token](http://openid.net/specs/openid-connect-core-1_0.html#IDToken) jsou typy tokenů zabezpečení, které jsou implementované jako [JSON Web Token (JWT)][JWT].

## <a name="service-principal-object"></a>objekt zabezpečení služby
Když jste registrace nebo aktualizace aplikace hello [portál Azure][AZURE-portal], portál hello vytvoření nebo aktualizace i [objektu application](#application-object) a odpovídající objekt služby objekt pro tohoto klienta. objekt aplikace Hello *definuje* hello konfigurace identity aplikace globálně (v rámci všech klientů kde hello přidružené aplikace udělen přístup), a je hello šablonu, ze které jeho odpovídající služby hlavní objekty jsou *odvozené* pro použití místně za běhu (v konkrétní klienta).

V tématu [aplikací a hlavní objekty služeb] [ AAD-App-SP-Objects] Další informace.

## <a name="sign-in"></a>Přihlášení
Hello proces [klientská aplikace](#client-application) inicializaci ověřování koncového uživatele a zaznamenávání stavu hello za účelem získání související [token zabezpečení](#security-token) a rozsahu toothat relace aplikace hello stav. Stavu může obsahovat artefaktů, jako jsou informace o profilu uživatele a informace odvozené z tokenu deklarací identity.

Hello přihlášení funkce aplikace, je obvykle používanými tooimplement jednotného přihlašování (SSO). Ho může také předcházet "registrace" funkce, jako hello vstupní bod pro aplikaci koncový uživatel toogain přístup tooan (při prvním přihlášení). Hello registrace funkce je použité toogather a zachovat další stav toohello konkrétní uživatele a může vyžadovat [souhlas uživatele](#consent).

## <a name="sign-out"></a>odhlášení
Hello zrušení ověřovací proces koncového uživatele, odpojení stavu uživatele hello přidružené hello [klientská aplikace](#client-application) během relaci [přihlášení](#sign-in)

## <a name="tenant"></a>Klienta
Instance adresáře služby Azure AD je odkazované tooas klient služby Azure AD. Poskytuje celou řadu funkcí, včetně:

* Služba registru pro integrované aplikace
* ověřování uživatelských účtů a registrované aplikace
* Koncové body REST požadované toosupport různých protokolů, včetně OAuth2 a SAML, včetně hello [koncový bod autorizace](#authorization-endpoint), [koncový bod tokenu](#token-endpoint) a hello "běžné" koncovým bodem používaným [ víceklientským aplikacím](#multi-tenant-application).

Klient je taky přiřazený k Azure AD nebo předplatné služeb Office 365 během zřizování hello předplatného, nabízí funkce Správa identit a přístupu pro předplatné hello. V tématu [jak tooget Azure Active Directory klienta] [ AAD-How-To-Tenant] podrobnosti o hello různé způsoby, kterými můžete získat přístup k tooa klienta. V tématu [asociování předplatných Azure se službou Azure Active Directory] [ AAD-How-Subscriptions-Assoc] podrobnosti o hello vztahu mezi předplatnými a klient služby Azure AD.

## <a name="token-endpoint"></a>Koncový bod tokenu
Jeden z koncových bodů hello implementované hello [serveru ověřování](#authorization-server) toosupport OAuth2 [autorizace uděluje](#authorization-grant). V závislosti na hello udělení, může být použité tooacquire [přístupový token](#access-token) (a související token "aktualizovat") tooa [klienta](#client-application), nebo [ID token](#ID-token) při použití s hello [ OpenID Connect] [ OpenIDConnect] protokolu.

## <a name="user-agent-based-client"></a>Na základě uživatelského agenta klienta
Typ [klientská aplikace](#client-application) zda stáhne kód z webového serveru a spustí v rámci uživatelského agenta (například webový prohlížeč), jako je například jedné stránce aplikace (SPA). Vzhledem k tomu, že veškerý kód se spustí na zařízení, bude považován za "veřejná" klienta z důvodu tooits nemohou toostore pověření soukromě nebo jako s důvěrnými. V tématu [OAuth2 klienta typy a profily] [ OAuth2-Client-Types] další podrobnosti.

## <a name="user-principal"></a>hlavní název uživatele
Podobným způsobem toohello objekt zabezpečení služby je použité toorepresent instance aplikace objekt zabezpečení uživatele je jiný typ objektu, zabezpečení, který reprezentuje uživatele. Hello Azure AD Graph [entitu uživatele] [ AAD-Graph-User-Entity] definuje hello schéma pro objekt uživatele, včetně vlastnosti související s uživatelem, například křestní jméno a příjmení, hlavní název uživatele, členství v rolích directory atd. To poskytuje hello konfigurace identity uživatele pro Azure AD tooestablish uživatele hlavní za běhu. Hello hlavní název uživatele je použité toorepresent ověřeného uživatele pro jednotné přihlašování, zaznamenávání [souhlas](#consent) delegování, provedení rozhodnutí o řízení přístupu, atd.

## <a name="web-client"></a>Webový klient
Typ [klientská aplikace](#client-application) , která se spouští všechny kódu na webovém serveru a schopný toofunction jako "důvěrné informace" klienta bezpečně uloží pověření uživatele na serveru hello. V tématu [OAuth2 klienta typy a profily] [ OAuth2-Client-Types] další podrobnosti.

## <a name="next-steps"></a>Další kroky
Hello [Příručka vývojáře pro Azure AD] [ AAD-Dev-Guide] je hello portálu toouse pro všechny služby Azure AD vývoj související témata, včetně přehledu [integraci aplikací] [ AAD-How-To-Integrate] a hello Základy [ověřování Azure AD a scénáře podporované ověřování][AAD-Auth-Scenarios].

Použijte hello následující komentáře části tooprovide zpětnou vazbu a Pomozte nám vylepšit a utvářejí náš obsah, včetně žádostí pro nové definice nebo aktualizuje existující!

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ../active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
