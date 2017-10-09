---
title: "aaaAuthentication scénáře pro Azure AD | Microsoft Docs"
description: "Přehled hello pět nejběžnější scénáře ověřování pro Azure Active Directory (AAD)"
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: 5b391e3f096fcb52934c4a8aff08688352bfd3dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-scenarios-for-azure-ad"></a>Scénáře ověřování pro Azure AD
Azure Active Directory (Azure AD) zjednodušuje tím, že poskytuje identity jako služba, se podpora pro standardní protokoly například OAuth 2.0 a OpenID Connect, jakož i opensourcové knihovny pro různé platformy toohelp ověřování pro vývojáře psaní se rychle. Tento dokument vám pomůže pochopit hello podporuje různé scénáře služby Azure AD a ukáže vám, jak tooget spuštěna. Je rozdělený do hello následující části:

* [Základní informace o ověřování ve službě Azure AD](#basics-of-authentication-in-azure-ad)
* [Deklarace identity v tokenech zabezpečení Azure AD](#claims-in-azure-ad-security-tokens)
* [Základní informace o registraci aplikace ve službě Azure AD](#basics-of-registering-an-application-in-azure-ad)
* [Typy aplikací a scénáře](#application-types-and-scenarios)
  
  * [Webové prohlížeče tooWeb aplikace](#web-browser-to-web-application)
  * [Jednostránkové aplikace (SPA)](#single-page-application-spa)
  * [Nativní aplikace tooWeb rozhraní API](#native-application-to-web-api)
  * [Webové aplikace tooWeb rozhraní API](#web-application-to-web-api)
  * [Démon procesu nebo serverové aplikace tooWeb rozhraní API](#daemon-or-server-application-to-web-api)

## <a name="basics-of-authentication-in-azure-ad"></a>Základní informace o ověřování ve službě Azure AD
Pokud jste obeznámeni s základní koncepty ověřování ve službě Azure AD, přečtěte si v této části. V ostatních případech můžete chtít tooskip dolů příliš[typy aplikací a scénáře](#application-types-and-scenarios).

Pojďme se hello nejzákladnější scénář, kdy je potřeba identity: uživatel ve webovém prohlížeči potřebuje tooauthenticate tooa webové aplikace. Tento scénář je podrobně popsané v větší hello [webový prohlížeč tooWeb aplikace](#web-browser-to-web-application) části, ale je užitečná, pokud počáteční bod tooillustrate hello možnosti služby Azure AD a conceptualize fungování hello scénář. Vezměte v úvahu hello následující diagram pro tento scénář:

![Přehled aplikace tooweb přihlášení](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

S hello diagramu výše na paměti, zde je co potřebujete tooknow o jeho různé komponenty:

* Azure AD je zprostředkovatel identity hello, zodpovědná za ověření hello identity uživatelů a aplikací, které existují v adresáři organizace a nakonec vystavování tokenů zabezpečení po úspěšném ověření těchto uživatelů a aplikací.
* Aplikace, která chce toooutsource ověřování tooAzure AD musí být zaregistrované ve službě Azure AD, která registruje a jednoznačně identifikuje aplikaci hello v adresáři hello.
* Vývojáři mohou použít hello Azure AD s otevřeným zdrojem ověřování knihovny toomake ověřování snadno zpracováním podrobností protokolu hello za vás. V tématu [knihovny Azure Active Directory Authentication](active-directory-authentication-libraries.md) Další informace.

• Jednou uživatel byl ověřen, aplikace hello tooensure tokenu zabezpečení hello uživatele musíte ověřit, že ověření proběhlo úspěšně, pro hello určený strany. Vývojáři mohou použít hello poskytuje knihovny toohandle ověřováním žádné tokenu z Azure AD, včetně webových tokenů JSON (JWT) nebo SAML 2.0. Pokud chcete tooperform ověření ručně, najdete v části hello [obslužná rutina tokenu JWT](https://msdn.microsoft.com/library/dn205065.aspx) dokumentaci.

> [!IMPORTANT]
> Azure AD prostředky používá tokeny toosign kryptografie využívající veřejného klíče a ověřte, zda je platný. V tématu [důležité informace o podpisový klíč výměny ve službě Azure AD](active-directory-signing-key-rollover.md) Další informace o potřebné logiky hello, musíte mít ve vaší aplikaci tooensure je vždy aktualizovat hello nejnovější klíče.
> 
> 

• hello toku požadavků a odpovědí pro proces ověřování hello je dáno hello ověřovací protokol, který byl použit, jako je například OAuth 2.0, OpenID Connect, WS-Federation nebo SAML 2.0. Tyto protokoly jsou podrobněji popsána v hello [Azure Active Directory ověřovací protokoly](active-directory-authentication-protocols.md) tématu a v následujících částech hello.

> [!NOTE]
> Azure AD podporuje hello OAuth 2.0 a OpenID Connect standardy, které rozsáhlé použití tokenů nosiče, včetně nosné tokeny vyjádřené tokeny Jwt. Token lightweight zabezpečení, že uděluje hello "nosiče" přístup tooa chráněný prostředek, je token nosiče. V tomto smyslu je hello "nosiče" libovolné strany, který může být hello token. I když může strana musí nejprve ověřit pomocí služby Azure AD tooreceive hello nosný token, zda text hello požadované kroky nejsou přijata toosecure hello token v přenos a ukládání, může být zachyceny a použity nezamýšleným strana. I když některé tokeny zabezpečení má integrovanou mechanismus, který brání neoprávněným stranám jejich používání, nosné tokeny nemají tento mechanismus a musí být přenosu v zabezpečený kanál, jako je například transport layer security (HTTPS). Pokud token nosiče je přenesen v hello jasné, střední útok man-in hello mohou být využívána token škodlivý strany tooacquire hello a použít jej pro prostředek tooa chráněné neoprávněného přístupu. Hello stejné zabezpečení zásady platí při ukládání nebo ukládání do mezipaměti nosné tokeny pro pozdější použití. Vždy zajistěte, aby vaše aplikace odesílá a ukládá nosné tokeny zabezpečeným způsobem. Další aspekty zabezpečení na nosné tokeny, najdete v části [RFC 6750 část 5](http://tools.ietf.org/html/rfc6750).
> 
> 

Teď, když máte přehled hello základy, přečtěte si hello oddílech toounderstand jak zřizování funguje ve službě Azure AD a hello běžné scénáře služby Azure AD podporuje.

## <a name="claims-in-azure-ad-security-tokens"></a>Deklarace identity v tokenech zabezpečení Azure AD
Zabezpečení tokeny vydané službou Azure AD obsahují deklarace identity a tvrzením informací o hello subjektu, který byl ověřen. Tyto deklarace lze hello aplikací pro různé úlohy. Například se může být použité toovalidate hello token, identifikovat klienta directory hello předmětu, zobrazit informace o uživateli, určit hello subjektu autorizace a tak dále. deklarace identity Hello nenachází v žádné tokenu zabezpečení jsou závislé na hello typ tokenu, hello typ pověření používaná tooauthenticate hello uživatele a konfigurace aplikace hello. Stručný popis každého typu deklarace identity vygenerované službou Azure AD najdete v tabulce hello. Další informace najdete v části příliš[podporované tokenu a typy deklarací identity](active-directory-token-and-claims.md).

| Deklarovat | Popis |
| --- | --- |
| ID aplikace |Identifikuje hello aplikaci, která používá hello token. |
| Cílová skupina |Identifikuje příjemce prostředků hello hello token je určený pro. |
| Application Authentication Context Class Reference |Určuje, jak hello klienta byla ověřený (veřejné klient oproti důvěrné klienta). |
| Ověřování prostřednictvím rychlých |Zaznamenává hello datum a čas, kdy došlo k chybě ověřování hello. |
| Metoda ověřování |Určuje, jak byla ověřena hello subjektu hello tokenu (heslo, certifikát, atd.). |
| Jméno |Poskytuje hello křestní jméno uživatele hello jako sada ve službě Azure AD. |
| Skupiny |Obsahuje objekt skupiny ID Azure AD, které hello uživatel je členem skupiny. |
| Zprostředkovatel identity |Zprostředkovatel identity hello záznamy, předmět hello hello token k ověření. |
| Vydané v |V které hello byl vydán token, často používají pro token aktuálnosti čas hello záznamy. |
| Vystavitel |Identifikuje hello služby tokenů zabezpečení, které vygenerované hello token, jakož i hello klienta Azure AD. |
| Příjmení |Poskytuje hello Přezdívka uživatele hello jako sada ve službě Azure AD. |
| Name (Název) |Poskytuje lidského čitelný hodnotu, která identifikuje hello subjektu hello tokenu. |
| Id objektu |Obsahuje neměnné jedinečný identifikátor hello subjektu ve službě Azure AD. |
| Role |Obsahuje popisné názvy z Azure AD aplikační role byla udělena tento uživatel hello. |
| Rozsah |Označuje hello oprávnění udělené toohello klientské aplikace. |
| Předmět |Označuje hello objekt zabezpečení, o které hello token vyhodnotí informace. |
| Id klienta |Obsahuje neměnné jedinečný identifikátor klienta hello directory, která vydala hello token. |
| Životnost tokenu |Definuje hello časový interval, ve kterém je token platný. |
| Hlavní název uživatele |Obsahuje hlavní název uživatele hello subjektu hello. |
| Verze |Obsahuje číslo verze hello hello tokenu. |

## <a name="basics-of-registering-an-application-in-azure-ad"></a>Základní informace o registraci aplikace ve službě Azure AD
Jakékoli aplikace, která outsources tooAzure ověřování, které AD musí být zaregistrovaný v adresáři. Tento krok zahrnuje Azure AD informacemi o vaší aplikaci, včetně hello adresu URL, kde je umístěný, adresa URL toosend hello reaguje po ověření hello URI tooidentify aplikace a další. Tyto informace jsou nezbytné pro klíče z několika důvodů:

* Azure AD musí souřadnice toocommunicate s aplikací hello při zpracování přihlášení nebo výměnou tokeny. Mezi ně patří hello následující:
  
  * Identifikátor ID URI aplikace: hello identifikátor pro aplikaci. Tato hodnota se odesílají tooAzure AD během tooindicate ověřování, které aplikace hello volající chce token pro. Kromě toho tato hodnota je obsažena v tokenu hello tak, aby aplikace hello ví, že bylo, že hello určené cílové.
  * Adresu URL a identifikátor URI pro přesměrování odpovědi: V případě hello webového rozhraní API nebo webová aplikace hello adresa URL odpovědi je toowhich umístění hello Azure AD bude posílat odpovědi ověřování hello, včetně token, pokud ověření bylo úspěšné. V případě hello nativní aplikace hello identifikátor URI pro přesměrování je že jedinečný identifikátor toowhich Azure AD přesměruje uživatelského agenta hello v jednom požadavku OAuth 2.0.
  * ID aplikace: hello ID pro aplikaci, která se generují pomocí Azure AD při registraci aplikace hello. Při žádosti autorizační kód nebo token, jsou odesílány hello ID aplikace a klíč tooAzure AD během ověřování.
  * : Hello klíče odeslaný společně ID aplikace při ověřování tooAzure AD toocall webového rozhraní API.
* Azure AD potřeb, které má aplikace hello tooensure hello požadovaná oprávnění tooaccess data vašeho adresáře, jinými aplikacemi ve vaší organizaci a tak dále

Zřizování se změní jasnější, pokud víte, že existují dvě kategorie aplikací, které je možné vyvinuté a integrovat se službou Azure AD:

* Jednotné klienta aplikací: aplikace jednoho klienta je určena pro použití v jedné organizaci. Toto jsou obvykle-obchodní (LoB) aplikace napsané pomocí vývojář enterprise. Jednoho klienta aplikace potřebuje pouze toobe přístup uživatelé v jednom adresáři, a v důsledku toho je nutné pouze toobe zřídit v jednom adresáři. Tyto aplikace jsou běžně registrovány vývojáři v organizaci hello.
* Víceklientské aplikace: víceklientské aplikace určena pro použití v mnoha organizacích není právě jedné organizace. Toto jsou obvykle softwaru jako služba (SaaS) aplikace napsané pomocí nezávislý dodavatel softwaru (ISV). Víceklientské aplikace potřebují toobe zřízené v každý adresář, kde se bude používat, který vyžaduje souhlas tooregister uživatel nebo správce je. Tento proces souhlasu spustí, když aplikace je zaregistrován v adresáři hello a je zadána přístup toohello Graph API nebo možná jiného webového rozhraní API. Pokud uživatel nebo správce z jiné organizace zaregistruje toouse hello aplikace, zobrazí se dialogové okno se zobrazí hello oprávnění, které hello aplikace vyžaduje. Hello uživatel nebo správce může potom aplikaci toohello souhlasu, což dává toohello přístupu aplikace hello uvádí data a nakonec registry hello aplikace v jejich adresáře. Další informace najdete v tématu [přehled hello souhlas Framework](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Některé další aspekty nastat při vývoji aplikace víceklientské místo jednoho klienta aplikace. Například pokud bude v několika adresářích jsou k dispozici toousers vaší aplikace, musíte mechanismus toodetermine klienta, které jsou ve. Jednoho klienta potřebuje pouze toolook v její vlastní adresář pro uživatele, zatímco víceklientské aplikace potřebuje tooidentify konkrétního uživatele ze všech hello adresářů ve službě Azure AD. tooaccomplish tuto úlohu Azure AD poskytuje společný koncový bod ověřování kde jakékoli víceklientské aplikace můžete nastavit žádostí o přihlášení, místo konkrétního klienta endpoint. Tento koncový bod je https://login.microsoftonline.com/common pro všechny adresáře ve službě Azure AD, že koncový bod konkrétního klienta může být https://login.microsoftonline.com/contoso.onmicrosoft.com. běžné koncový bod Hello je obzvláště důležité tooconsider při vývoji aplikace, protože budete potřebovat hello potřebné logiky toohandle více klientů při přihlášení, odhlášení a ověření tokenu.

Pokud jsou aktuálně vývoj aplikace s jednoho klienta, ale chcete toomake je k dispozici toomany organizace, můžete snadno provádět změny toohello aplikace a jeho konfigurace v Azure AD toomake ho víceklientské podporující. Kromě toho Azure AD používá hello stejné podpisového klíče pro všechny tokeny ve všech adresářích, jestli se tím ověřování na jednoho klienta nebo v aplikaci více klientů.

Každý scénář uvedené v tomto dokumentu obsahuje dílčí oddíl, který popisuje požadavky na jeho zřizování. Podrobnější informace o zřizování aplikace ve službě Azure AD a hello rozdíly mezi jednou či více klientů aplikace najdete v tématu [integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md) Další informace. Pokračujte ve čtení toounderstand hello běžné scénáře aplikací ve službě Azure AD.

## <a name="application-types-and-scenarios"></a>Typy aplikací a scénáře
Každý z hello scénáře popsaného v tomto dokumentu lze vytvořit pomocí různé jazyky a platformy. Všechny jsou zajišťované pomocí ukázky dokončení kódu, které jsou k dispozici v našem [Průvodce ukázky kódu](active-directory-code-samples.md), nebo přímo z odpovídající hello [ukázka úložišť GitHub](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Kromě toho pokud aplikace potřebuje konkrétní nebo segmentu začátku do konce scénáře, ve většině případů této funkce lze přidat nezávisle. Například pokud máte nativní aplikaci, která volá webové rozhraní API, můžete snadno přidat webovou aplikaci, která volá také hello webového rozhraní API. Hello následující diagram znázorňuje tyto scénáře a typy aplikací, a jak jiné součásti, lze přidat:

![Typy aplikací a scénáře](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Toto jsou hello pět primární aplikace scénáře podporované službou Azure AD:

* [Webové prohlížeče tooWeb aplikace](#web-browser-to-web-application): uživatel potřebuje toosign ve tooa webové aplikaci, která je zabezpečená službou Azure AD.
* [Jedna stránka aplikace (SPA)](#single-page-application-spa): uživatel potřebuje toosign tooa jednostránkové aplikace, která je zabezpečená službou Azure AD.
* [Nativní aplikace tooWeb rozhraní API](#native-application-to-web-api): nativní aplikaci spuštěnou v telefonu, tabletu nebo tooauthenticate musí počítač uživatele tooget prostředky z webového rozhraní API, která je zabezpečená službou Azure AD.
* [Webové rozhraní API tooWeb aplikace](#web-application-to-web-api): webová aplikace musí tooget prostředky z webového rozhraní API, které jsou zabezpečené službou Azure AD.
* [Démon procesu nebo serverové aplikace tooWeb rozhraní API](#daemon-or-server-application-to-web-api): démon aplikaci nebo serveru bez webového uživatelského rozhraní musí tooget prostředky z webového rozhraní API, které jsou zabezpečené službou Azure AD.

### <a name="web-browser-tooweb-application"></a>Webové prohlížeče tooWeb aplikace
Tato část popisuje aplikaci, která ověřuje uživatele ve webové aplikaci tooa webového prohlížeče. V tomto scénáři hello webové aplikace přesměruje toosign prohlížeče hello uživatele je v tooAzure AD. Azure AD vrátí odpověď přihlášení prostřednictvím prohlížeče hello uživatele, který obsahuje deklarace identity o uživateli hello v tokenu zabezpečení. Tento scénář podporuje přihlašování pomocí hello protokoly WS-Federation, SAML 2.0 a OpenID Connect.

#### <a name="diagram"></a>Diagram
![Tok ověření pro aplikace tooweb prohlížeče](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Popis protokolu toku
1. Když uživatel navštíví hello aplikace a potřebuje toosign v, je přesměrován prostřednictvím koncový bod požádat o přihlášení toohello ověřování ve službě Azure AD.
2. Hello uživatel přihlásí na přihlašovací stránku hello.
3. Pokud je ověření úspěšné, Azure AD vytvoří ověřovací token a vrátí adresa URL odpovědi aplikace toohello přihlášení odpověď, který jste nakonfigurovali v hello portálu Azure. Pro produkční aplikace musí být tato adresa URL odpovědi protokolu HTTPS. Vrátí token Hello obsahuje deklarace identity o uživateli hello a Azure AD, které jsou vyžadované token hello toovalidate aplikace hello.
4. aplikace Hello ověří hello token pomocí veřejného podpisového klíče a vystavitele informace, které jsou k dispozici v hello federační metadata dokumentu pro Azure AD. Po aplikace hello ověří hello token, Azure AD spustí novou relaci s hello uživatelem. Tato relace umožňuje hello uživatele tooaccess hello aplikaci do vypršení jeho platnosti.

#### <a name="code-samples"></a>Ukázky kódů
V tématu hello ukázky kódu pro scénáře aplikací tooWeb webového prohlížeče. A, kontrolujte pravidelně – přidáme nové ukázky celou dobu hello. [Webové prohlížeče tooWeb aplikace](active-directory-code-samples.md#web-browser-to-web-application).

#### <a name="registering"></a>Registrace
* Jednoho klienta: Pokud vytváříte aplikace právě pro vaši organizaci, se musí být zaregistrován v adresáři vaší společnosti pomocí hello portálu Azure.
* Víceklientské: Pokud vytváříte aplikace, která mohou být využívána uživatelé mimo vaši organizaci, musí být zaregistrovaný v adresáři vaší společnosti, ale také musí být zaregistrovaný v adresáři každé organizace, který bude používat aplikace hello. toomake v jejich adresář vaší aplikace, můžete zahrnout procesu registrace pro vaše zákazníky, kteří jim umožňuje tooconsent tooyour aplikace. Při registraci vaší aplikace, zobrazí se dialogové okno, který ukazuje hello oprávnění hello aplikace vyžaduje a pak hello tooconsent možnost. V závislosti na hello vyžaduje oprávnění správce v hello druhé organizace může být potřeba toogive souhlasu. Pokud souhlasí hello uživatel nebo správce, aplikace hello se registruje v jejich adresář. Další informace najdete v tématu [integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Vypršení platnosti tokenu
Hello uživatelské relace vyprší platnost, když vyprší platnost hello životnost hello tokenem vydaným službou Azure AD. Aplikace můžete zmenšit tohoto časového období, v případě potřeby, jako je například odhlášení uživatele založené na období nečinnosti. Když vyprší platnost hello relace, bude uživatel hello výzvami toosign v znovu.

### <a name="single-page-application-spa"></a>Jednostránkové aplikace (SPA)
Tato část popisuje ověřování pro jednu aplikaci, stránka, která používá Azure AD a hello implicitní autorizace OAuth 2.0 udělit toosecure ukončit jeho webového rozhraní API zpět. Jednostránkové aplikace mají obvykle strukturu prezentační vrstvou JavaScript (front-end), která běží v prohlížeči hello a webového rozhraní API back-end, který běží na serveru a implementuje hello obchodní logiku aplikace. toolearn Další informace o udělení hello implicitní autorizace a pomáhá můžete rozhodnout, zda je pro váš scénář aplikace správný, najdete v části [Principy hello OAuth2 implicitní tok v Azure Active Directory poskytování](active-directory-dev-understanding-oauth2-implicit-grant.md).

V tomto scénáři při přihlášení uživatele hello hello používá front end JavaScript [Active Directory Authentication Library pro jazyk JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) a implicitní autorizace hello udělit tooobtain token ID (požadavku id_token) z Azure AD. Hello token se uloží do mezipaměti a hello klienta ji připojí požadavek toohello jako token nosiče hello při provádění volání tooits webového rozhraní API back-end, který zabezpečené pomocí middlewaru OWIN hello. 

#### <a name="diagram"></a>Diagram
![Diagram jedné stránky aplikace](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Popis protokolu toku
1. Hello uživatel přejde toohello webové aplikace.
2. aplikace Hello vrátí hello JavaScript front-endu (prezentační vrstva) toohello prohlížeče.
3. Hello uživatel zahájí přihlášení, například kliknutím přihlašovací odkaz. Hello prohlížeč odešle ID token toorequest koncový bod autorizace GET toohello Azure AD. Tato žádost obsahuje hello ID a odpovědi adresa URL aplikace v hello parametry dotazu.
4. Azure AD ověří hello adresa URL odpovědi proti hello zaregistrován adresa URL odpovědi, která byla konfigurována hello portálu Azure.
5. Hello uživatel přihlásí na přihlašovací stránku hello.
6. Pokud je ověření úspěšné, Azure AD vytvoří ID token a vrátí ji jako adresa URL odpovědi aplikace toohello na adresu URL fragment (#). Pro produkční aplikace musí být tato adresa URL odpovědi protokolu HTTPS. Vrátí token Hello obsahuje deklarace identity o uživateli hello a Azure AD, které jsou vyžadované token hello toovalidate aplikace hello.
7. Hello JavaScript klienta kód spuštěný v prohlížeči hello extrahuje hello token z toouse odpovědi hello k zabezpečení volání rozhraní API webové aplikace toohello zpět ukončení.
8. volání prohlížeče Hello hello webové aplikace API zpět končit hello přístupový token v hlavičce autorizace hello.

#### <a name="code-samples"></a>Ukázky kódů
V tématu hello ukázky kódu pro scénáře jedné stránky aplikace (SPA). Zda toocheck zpět se často – přidáme nové ukázky celou dobu hello. [Jedna stránka aplikace (SPA)](active-directory-code-samples.md#single-page-application-spa).

#### <a name="registering"></a>Registrace
* Jednoho klienta: Pokud vytváříte aplikace právě pro vaši organizaci, se musí být zaregistrován v adresáři vaší společnosti pomocí hello portálu Azure.
* Víceklientské: Pokud vytváříte aplikace, která mohou být využívána uživatelé mimo vaši organizaci, musí být zaregistrovaný v adresáři vaší společnosti, ale také musí být zaregistrovaný v adresáři každé organizace, který bude používat aplikace hello. toomake v jejich adresář vaší aplikace, můžete zahrnout procesu registrace pro vaše zákazníky, kteří jim umožňuje tooconsent tooyour aplikace. Při registraci vaší aplikace, zobrazí se dialogové okno, který ukazuje hello oprávnění hello aplikace vyžaduje a pak hello tooconsent možnost. V závislosti na hello vyžaduje oprávnění správce v hello druhé organizace může být potřeba toogive souhlasu. Pokud souhlasí hello uživatel nebo správce, aplikace hello se registruje v jejich adresář. Další informace najdete v tématu [integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md).

Po registraci hello aplikace, musí být nakonfigurované toouse protokol OAuth 2.0 implicitní Grant. Ve výchozím nastavení je tento protokol zakázaná pro aplikace. hello tooenable protokol OAuth2 implicitní udělte pro aplikaci, upravte jeho manifest aplikace z portálu Azure hello a nastavte tootrue hodnotu "oauth2AllowImplicitFlow" hello. Podrobné pokyny najdete v tématu [povolení OAuth 2.0 implicitní Grant pro jednostránkové aplikace](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Vypršení platnosti tokenu
Když použijete ADAL.js toomanage ověřování s Azure AD, můžete využívat výhod několik funkcí, které usnadňují aktualizace tokenu vypršela platnost a také jak získat tokeny pro více webových rozhraní API prostředky, které může být volána aplikace hello. Když se uživatel hello úspěšně s Azure AD ověřuje, zabezpečené pomocí souboru cookie relace pro uživatele hello mezi hello prohlížeče a Azure AD. Je důležité toonote, který existuje hello relace mezi hello uživatele a Azure AD a není mezi hello uživatele a hello webová aplikace běží na serveru pro hello. Když vyprší platnost tokenu, ADAL.js používá tuto relaci toosilently získat další token. Je to provádí pomocí toosend skrytá iFrame a přijmout žádost o hello pomocí protokolu OAuth implicitní Grant hello. ADAL.js můžete také použít tento stejný mechanismus toosilently získali přístupové tokeny z Azure AD pro jiné webové rozhraní API prostředky, volání aplikace hello tak dlouho, dokud tyto prostředky podporu (CORS), sdílení prostředků různého původu jsou registrované v adresáři hello uživatele, a všechny požadované souhlasu byl zadán uživatelem hello během přihlášení.

### <a name="native-application-tooweb-api"></a>Nativní aplikace tooWeb rozhraní API
Tato část popisuje nativní aplikaci, která volá webové rozhraní API jménem uživatele. Tento scénář je založený na hello OAuth 2.0 autorizační kód typ udělení pomocí veřejného klienta, jak je popsáno v části 4.1 hello [specifikace OAuth 2.0](http://tools.ietf.org/html/rfc6749). nativní aplikace Hello získá přístupový token pro uživatele hello pomocí protokolu hello OAuth 2.0. Tento přístupový token se pak odešlou v hello požadavek toohello webové rozhraní API, která povolí hello uživatele a vrátí hello požadovaného prostředku.

#### <a name="diagram"></a>Diagram
![Nativní aplikace tooWeb Diagram rozhraní API](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-tooapi"></a>Tok ověření pro tooAPI nativní aplikace
#### <a name="description-of-protocol-flow"></a>Popis protokolu toku
Pokud používáte hello AD knihovny ověřování, jsou zpracovávány většinu podrobnosti protokolu hello popsané níže, například automaticky otevírané okno prohlížeče hello, ukládání tokenu do mezipaměti a zpracování tokeny obnovení.

1. Pomocí automaticky otevíraná okna prohlížeče, mohou nativní aplikace hello koncový bod žádosti toohello autorizace ve službě Azure AD. Tato žádost obsahuje hello ID aplikace a identifikátor URI nativní aplikace hello hello přesměrování, jak je znázorněno v hello portálu Azure a identifikátor ID URI aplikace hello hello webového rozhraní API. Pokud uživatel hello nebyla již přihlášeni, jsou výzvami toosign v znovu
2. Azure AD ověřuje uživatele hello. Pokud je aplikace více klientů a souhlasu je požadovaná toouse hello aplikace, bude uživatel hello požadované tooconsent, pokud se tak již neučinili. Po udělení souhlasu a po úspěšném ověření Azure AD vydá identifikátor URI pro přesměrování autorizační kód odpovědi back toohello klientské aplikace.
3. Pokud Azure AD vydá kód odpovědi ověřování zpět toohello identifikátor URI pro přesměrování, hello klienta aplikace přestane prohlížeče interakce a extrahuje hello autorizační kód z odpovědi hello. Pomocí tohoto kódu autorizace, hello klientská aplikace odešle požadavek tooAzure AD na koncový bod tokenu zahrnující hello autorizační kód, podrobnosti o hello klientská aplikace (ID aplikace a identifikátor URI pro přesměrování) a hello požadovaného prostředku (ID aplikace Identifikátor URI pro hello webového rozhraní API).
4. Hello autorizační kód a informace o hello klientská aplikace a webové rozhraní API se ověří pomocí Azure AD. Po úspěšném ověření Azure AD vrátí dva tokeny: přístupový token JWT a obnovovací token JWT. Kromě toho Azure AD vrátí základní informace o hello uživateli, jako je například jejich zobrazovaný název a klienta ID.
5. Přes protokol HTTPS klientská aplikace hello používá hello vrácená JWT přístup k tokenu tooadd hello JWT řetězec, který má označení "Nosiče" v hlavičce autorizace hello hello požadavek toohello webových rozhraní API. Hello webového rozhraní API pak ověří hello JWT token, a pokud je ověření úspěšné, vrátí hello požadovaného prostředku.
6. Pokud hello přístupovému tokenu vyprší platnost, obdrží klientské aplikace hello k chybě, která označuje, že uživatel hello potřebuje tooauthenticate znovu. Pokud aplikace hello má platný obnovovací token, může být použité tooacquire nový přístupový token bez zobrazení výzvy hello toosign uživatele v akci. Pokud vyprší platnost tokenu obnovení hello, bude nutné aplikace hello toointeractively ověření uživatele hello ještě jednou.

> [!NOTE]
> Hello obnovovací token vydán Azure AD může být použité tooaccess několik prostředků. Například pokud máte klientské aplikace, který má oprávnění toocall dva webové rozhraní API, může být hello obnovovací token použité tooget přístupu k tokenu toohello jiných webového rozhraní API také.
> 
> 

#### <a name="code-samples"></a>Ukázky kódů
V tématu hello ukázky kódu pro scénáře rozhraní API tooWeb nativní aplikaci. A, kontrolujte pravidelně – přidáme nové ukázky celou dobu hello. [Nativní aplikace tooWeb rozhraní API](active-directory-code-samples.md#native-application-to-web-api).

#### <a name="registering"></a>Registrace
* Jednoho klienta: Obě hello nativní aplikaci a hello webového rozhraní API musí být zaregistrovaný v hello stejný adresář ve službě Azure AD. Hello webového rozhraní API může být nakonfigurované tooexpose sadu oprávnění, které jsou používané toolimit hello nativní přístup tooits prostředky aplikace. klientská aplikace Hello potom vybere hello požadovaných oprávnění z nabídky hello rozevíracího seznamu "TooOther oprávnění aplikace" v hello portálu Azure.
* Víceklientské: Nejdřív nativní aplikace hello vždy jen zaregistrovat ve hello vývojáře nebo adresář vydavatele. Nativní aplikace hello druhou, je nakonfigurovaná tooindicate hello oprávnění vyžaduje toobe funkční. Tento seznam požadovaných oprávnění se zobrazí v dialogu, když uživatel nebo správce v cílovém adresáři hello dává souhlasu toohello aplikace, takže je k dispozici tootheir organizace. Některé aplikace vyžadují právě individuální oprávnění, které může každý uživatel v organizaci hello souhlas. Jiné aplikace vyžadují oprávnění na úrovni správce, které nelze souhlas uživatele v organizaci hello. Pouze správce adresáře může poskytnout tooapplications souhlasu, které vyžadují tato úroveň oprávnění. Pokud souhlasí hello uživatel nebo správce, pouze hello webového rozhraní API se registruje v jejich adresář. Další informace najdete v tématu [integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Vypršení platnosti tokenu
Pokud nativní aplikace hello používá jeho autorizační kód tooget přístupový token JWT, dále přijímá obnovovací token JWT. V případě, že hello přístupovému tokenu vyprší platnost, hello obnovovací token můžou být použité toore-ověření uživatele hello bez nutnosti jejich znovu toosign v. Tento token obnovení je pak použít tooauthenticate hello uživatele, výsledkem je pro přístup k nový token a aktualizujte tokenu.

### <a name="web-application-tooweb-api"></a>Webové aplikace tooWeb rozhraní API
Tato část popisuje webové aplikace, které potřebuje tooget prostředky z webového rozhraní API. V tomto scénáři jsou dva typy identity, které hello webové aplikace můžete použít tooauthenticate a volání webového rozhraní API hello: Identita aplikace, nebo delegovaný uživatel identity.

*Identita aplikace:* tento scénář používá pověření klienta OAuth 2.0 udělit tooauthenticate jako aplikace hello a přístup hello webové rozhraní API. Při použití identity aplikací, hello webové rozhraní API pouze může zjistit, že hello webové aplikace je volání, jako hello webového rozhraní API nepřijímá žádné informace o uživateli hello. Pokud aplikace hello obdrží informace o uživateli hello, bude odeslána prostřednictvím protokolu aplikace hello a není podepsán serverem Azure AD. Hello webového rozhraní API důvěřuje, že webová aplikace hello ověřil hello uživatele. Z tohoto důvodu se tento vzor nazývá důvěryhodný subsystém.

*Delegované identity uživatele:* tento scénář lze provést dvěma způsoby: OpenID Connect a udělení autorizačního kódu OAuth 2.0 pomocí důvěrné klienta. Hello webová aplikace získá přístupový token pro hello uživatele, který prokáže toohello webové rozhraní API této hello uživatel úspěšně ověřený toohello webové aplikace a webové aplikace hello bylo možné tooobtain delegovaný uživatel identity toocall hello webového rozhraní API. Tento přístupový token se odešlou v hello požadavek toohello webové rozhraní API, která povolí hello uživatele a vrátí hello požadovaného prostředku.

#### <a name="diagram"></a>Diagram
![Webové aplikace tooWeb rozhraní API diagram](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Popis protokolu toku
Obě hello identita aplikací a typů identit delegovaný uživatel jsou popsané v toku hello níže. Hello klíčovým rozdílem mezi nimi je, že identita uživatele hello delegovaný musíte nejprve získat autorizační kód před hello uživatel může přihlásit a získat přístup toohello webového rozhraní API.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Udělení přihlašovací údaje Identity aplikace pomocí klienta OAuth 2.0
1. Uživatel je přihlášený tooAzure AD ve webové aplikaci hello (viz hello [webový prohlížeč tooWeb aplikace](#web-browser-to-web-application) výše).
2. Hello webová aplikace musí tooacquire token přístupu, aby mohli ověřit toohello webového rozhraní API a načíst hello požadovaného prostředku. Umožňuje tooAzure požadavek na AD koncovému bodu tokenu, poskytnutí přihlašovacích údajů hello, ID aplikace a aplikace webového rozhraní API je identifikátor ID URI.
3. Azure AD ověřuje hello aplikace a vrátí JWT přístupový token, který je použité toocall hello webového rozhraní API.
4. Přes protokol HTTPS hello webová aplikace používá hello vrácená JWT přístup k tokenu tooadd hello JWT řetězec, který má označení "Nosiče" v hlavičce autorizace hello hello požadavek toohello webových rozhraní API. Hello webového rozhraní API pak ověří hello JWT token, a pokud je ověření úspěšné, vrátí hello požadovaného prostředku.

##### <a name="delegated-user-identity-with-openid-connect"></a>Identita uživatele delegovaný s OpenID Connect
1. Uživatel je přihlášený tooa webové aplikace pomocí služby Azure AD (viz hello [webový prohlížeč tooWeb aplikace](#web-browser-to-web-application) část výše). Pokud uživatel hello hello webové aplikace nebyla dosud dá souhlas tooallowing hello webové aplikace toocall hello webového rozhraní API jeho jménem, je třeba uživatel hello tooconsent. Hello aplikace se zobrazí hello oprávnění, která vyžaduje, a pokud některá z těchto oprávnění na úrovni správce, normální uživatele v adresáři hello nebude možné tooconsent. Tento proces souhlasu platí jenom toomulti klienta aplikace, není jednoho klienta aplikace, jako aplikace hello budou již hello potřebná oprávnění. Když hello uživatele přihlášení, hello webové aplikace přijala token ID s informacemi o hello uživatele, jakož i autorizační kód.
2. Pomocí hello autorizační kód vydané službou Azure AD, hello webové aplikace odešle požadavek tooAzure AD na koncový bod tokenu zahrnující hello autorizační kód, podrobnosti o hello klientská aplikace (ID aplikace a identifikátor URI pro přesměrování) a hello požadovaných prostředků ( ID identifikátor URI aplikace hello webového rozhraní API).
3. Hello autorizační kód a informace o hello webové aplikace a webové rozhraní API se ověří pomocí Azure AD. Po úspěšném ověření Azure AD vrátí dva tokeny: přístupový token JWT a obnovovací token JWT.
4. Přes protokol HTTPS hello webová aplikace používá hello vrácená JWT přístup k tokenu tooadd hello JWT řetězec, který má označení "Nosiče" v hlavičce autorizace hello hello požadavek toohello webových rozhraní API. Hello webového rozhraní API pak ověří hello JWT token, a pokud je ověření úspěšné, vrátí hello požadovaného prostředku.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Identita uživatele delegovaný s udělení autorizačního kódu OAuth 2.0
1. Uživatel již přihlášen tooa webové aplikace, jejichž mechanismus ověřování je nezávislé na Azure AD.
2. Hello webové aplikace vyžaduje povolení kód tooacquire přístupový token, takže vydává požadavek prostřednictvím koncového bodu autorizace hello prohlížeče tooAzure na AD, poskytuje hello ID aplikace a identifikátor URI přesměrování pro webovou aplikaci hello po úspěšné ověřování. přihlášení uživatele Hello tooAzure AD.
3. Pokud uživatel hello hello webové aplikace nebyla dosud dá souhlas tooallowing hello webové aplikace toocall hello webového rozhraní API jeho jménem, je třeba uživatel hello tooconsent. Hello aplikace se zobrazí hello oprávnění, která vyžaduje, a pokud některá z těchto oprávnění na úrovni správce, normální uživatele v adresáři hello nebude možné tooconsent. Svůj souhlas se vztahuje tooboth jeden a více klientů aplikace.  V případě hello jednoho klienta může správce provést tooconsent souhlas správce jménem uživatelů.  To lze provést pomocí hello `Grant Permissions` tlačítka na hello [portálu Azure](https://portal.azure.com). 
4. Po hello uživatel souhlasí, obdrží hello webové aplikace hello autorizační kód, je nutné tooacquire přístupový token.
5. Pomocí hello autorizační kód vydané službou Azure AD, hello webové aplikace odešle požadavek tooAzure AD na koncový bod tokenu zahrnující hello autorizační kód, podrobnosti o hello klientská aplikace (ID aplikace a identifikátor URI pro přesměrování) a hello požadovaných prostředků ( ID identifikátor URI aplikace hello webového rozhraní API).
6. Hello autorizační kód a informace o hello webové aplikace a webové rozhraní API se ověří pomocí Azure AD. Po úspěšném ověření Azure AD vrátí dva tokeny: přístupový token JWT a obnovovací token JWT.
7. Přes protokol HTTPS hello webová aplikace používá hello vrácená JWT přístup k tokenu tooadd hello JWT řetězec, který má označení "Nosiče" v hlavičce autorizace hello hello požadavek toohello webových rozhraní API. Hello webového rozhraní API pak ověří hello JWT token, a pokud je ověření úspěšné, vrátí hello požadovaného prostředku.

#### <a name="code-samples"></a>Ukázky kódů
V tématu hello ukázky kódu pro webovou aplikaci tooWeb rozhraní API scénáře. A, kontrolujte pravidelně – přidáme nové ukázky celou dobu hello. Webové [tooWeb aplikace API](active-directory-code-samples.md#web-application-to-web-api).

#### <a name="registering"></a>Registrace
* Jednoho klienta: Identita aplikace hello a delegovaný uživatel identity případech hello webové aplikace a hello webového rozhraní API musí být zaregistrovaný v hello stejný adresář ve službě Azure AD. Hello webového rozhraní API může být nakonfigurované tooexpose sadu oprávnění, které jsou používané toolimit hello webové aplikace přístup k tooits prostředkům. Pokud se používá typ identity delegované uživatele, hello webová aplikace musí tooselect hello požadovaných oprávnění z nabídky hello rozevíracího seznamu "TooOther oprávnění aplikace" v hello portálu Azure. Tento krok se nevyžaduje, pokud se používá typ identity aplikace hello.
* Víceklientské: Nejdřív hello webové aplikace je nakonfigurovaná tooindicate hello oprávnění vyžaduje toobe funkční. Tento seznam požadovaných oprávnění se zobrazí v dialogu, když uživatel nebo správce v cílovém adresáři hello dává souhlasu toohello aplikace, takže je k dispozici tootheir organizace. Některé aplikace vyžadují právě individuální oprávnění, které může každý uživatel v organizaci hello souhlas. Jiné aplikace vyžadují oprávnění na úrovni správce, které nelze souhlas uživatele v organizaci hello. Pouze správce adresáře může poskytnout tooapplications souhlasu, které vyžadují tato úroveň oprávnění. Když hello uživatel nebo správce souhlas všech uživatelů, hello webová aplikace a hello webové rozhraní API je zaregistrovaný v jejich adresáře.

#### <a name="token-expiration"></a>Vypršení platnosti tokenu
Pokud webová aplikace hello používá jeho autorizační kód tooget přístupový token JWT, dále přijímá obnovovací token JWT. V případě, že hello přístupovému tokenu vyprší platnost, hello obnovovací token můžou být použité toore-ověření uživatele hello bez nutnosti jejich znovu toosign v. Tento token obnovení je pak použít tooauthenticate hello uživatele, výsledkem je pro přístup k nový token a aktualizujte tokenu.

### <a name="daemon-or-server-application-tooweb-api"></a>Démon procesu nebo serverové aplikace tooWeb rozhraní API
Tato část popisuje proces démon nebo server aplikace, která potřebuje tooget prostředky z webového rozhraní API. Existují dva dílčí scénáře, které se vztahují části toothis: démon vyžadující toocall web API, založený na typ udělení pověření klienta OAuth 2.0; a serverová aplikace (například webové rozhraní API), který potřebuje toocall webového rozhraní API, založený na specifikaci koncept OAuth 2.0 On-Behalf-Of.

Pro scénář hello při démon aplikace potřebuje toocall webového rozhraní API, je důležité toounderstand pár věcí. Nejprve interakci s uživatelem není možné s démon aplikací, které vyžaduje toohave aplikace hello vlastní identity. Příkladem démon aplikace je dávkovou úlohu nebo služba operačního systému spuštěné v pozadí hello. Tento typ aplikace požadavků přístupový token pomocí svou identitu aplikace a prezentování jeho ID aplikace, přihlašovací údaje (heslo nebo certifikát) a tooAzure identifikátor ID URI aplikace AD. Po úspěšném ověření hello démon obdrží token přístupu z Azure AD, která je pak použít toocall hello webového rozhraní API.

Pro scénář hello když serverová aplikace potřebuje toocall webového rozhraní API, je užitečné toouse příklad. Představte si, že byl uživatel ověřen na nativní aplikaci a tato nativní aplikace musí toocall webového rozhraní API. Azure AD vydá token JWT přístup k tokenu toocall hello webového rozhraní API. Pokud hello webového rozhraní API potřebuje toocall jiné podřízené webové rozhraní API, může použít hello tok on-behalf-of toodelegate hello identitu uživatele a ověření toohello sekundární webové rozhraní API.

#### <a name="diagram"></a>Diagram
![Diagram tooWeb rozhraní API démon nebo serverové aplikace](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Popis protokolu toku
##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Udělení přihlašovací údaje Identity aplikace pomocí klienta OAuth 2.0
1. Aplikace serveru hello nejprve musí tooauthenticate s Azure AD jako samostatně, bez jakékoli zásahem ze strany například dialogovým interaktivní přihlašování. Umožňuje tooAzure požadavek na AD koncovému bodu tokenu, poskytnutí přihlašovacích údajů hello ID aplikace a identifikátor ID URI aplikace.
2. Azure AD ověřuje hello aplikace a vrátí JWT přístupový token, který je použité toocall hello webového rozhraní API.
3. Přes protokol HTTPS hello webová aplikace používá hello vrácená JWT přístup k tokenu tooadd hello JWT řetězec, který má označení "Nosiče" v hlavičce autorizace hello hello požadavek toohello webových rozhraní API. Hello webového rozhraní API pak ověří hello JWT token, a pokud je ověření úspěšné, vrátí hello požadovaného prostředku.

##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Identita uživatele delegovaný specifikace On-Behalf-Of koncept OAuth 2.0
tok Hello popsané níže předpokládá, že uživatel byl ověřen na jinou aplikaci (například nativní aplikace), a jejich identitu uživatele byl tooacquire používané přístupu k tokenu toohello první vrstvu webového rozhraní API.

1. nativní aplikace Hello odešle hello přístup tokenu toohello první vrstvu webového rozhraní API.
2. Hello první vrstvu webového rozhraní API odešle žádost tooAzure AD na koncový bod tokenu, poskytuje jeho ID aplikace a přihlašovací údaje, stejně jako hello uživatele přístupový token. Kromě toho je hello požadavek odeslán s on_behalf_of parametr, který určuje hello webové rozhraní API požaduje nové tokeny toocall podřízené webové rozhraní API jménem uživatele původní hello.
3. Azure AD ověřuje, že hello první vrstvu webového rozhraní API má oprávnění tooaccess hello sekundární webového rozhraní API a ověří žádost hello vrácení, že přístupový token JWT a token JWT aktualizace tokenu toohello první vrstvu webového rozhraní API.
4. Přes protokol HTTPS hello první úroveň webová rozhraní API pak zavolá hello sekundární webové rozhraní API připojením hello tokenu řetězec v hello autorizační hlavičky v požadavku hello. Hello první vrstvu webového rozhraní API můžete dál toocall hello sekundární webové rozhraní API, dokud jsou platné hello přístupový token a obnovovacích tokenů.

#### <a name="code-samples"></a>Ukázky kódů
V tématu hello ukázky kódu pro scénáře tooWeb rozhraní API démon nebo serverové aplikace. A, kontrolujte pravidelně – přidáme nové ukázky celou dobu hello. [Server nebo aplikaci démon tooWeb rozhraní API](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Registrace
* Jednoho klienta: Identita aplikace hello a delegovaný uživatel identity případech aplikace hello démon nebo server musí být zaregistrovaná v hello stejný adresář ve službě Azure AD. Hello webového rozhraní API může být nakonfigurované tooexpose sadu oprávnění, které jsou používané toolimit hello démon nebo prostředky tooits přístup k serveru. Pokud se používá typ identity delegované uživatele, aplikace server hello musí tooselect hello požadovaných oprávnění z nabídky hello rozevíracího seznamu "TooOther oprávnění aplikace" v hello portálu Azure. Tento krok se nevyžaduje, pokud se používá typ identity aplikace hello.
* Víceklientské: Nejdřív hello démon nebo server aplikace je nakonfigurovaná tooindicate hello oprávnění vyžaduje toobe funkční. Tento seznam požadovaných oprávnění se zobrazí v dialogu, když uživatel nebo správce v cílovém adresáři hello dává souhlasu toohello aplikace, takže je k dispozici tootheir organizace. Některé aplikace vyžadují právě individuální oprávnění, které může každý uživatel v organizaci hello souhlas. Jiné aplikace vyžadují oprávnění na úrovni správce, které nelze souhlas uživatele v organizaci hello. Pouze správce adresáře může poskytnout tooapplications souhlasu, které vyžadují tato úroveň oprávnění. Když hello uživatel nebo správce souhlasí, obě hello webových rozhraní API jsou v jejich directory zaregistrované.

#### <a name="token-expiration"></a>Vypršení platnosti tokenu
Při první aplikace hello používá jeho autorizační kód tooget přístupový token JWT, dále přijímá obnovovací token JWT. V případě, že hello přístupovému tokenu vyprší platnost, hello obnovovací token můžou být použité toore-ověření uživatele hello bez zobrazení výzvy k zadání pověření. Tento token obnovení je pak použít tooauthenticate hello uživatele, výsledkem je pro přístup k nový token a aktualizujte tokenu.

## <a name="see-also"></a>Viz také
[Příručka pro vývojáře Azure Active Directory](active-directory-developers-guide.md)

[Ukázky kódu Azure Active Directory](active-directory-code-samples.md)

[Důležité informace o podepisování výměna klíče ve službě Azure AD](active-directory-signing-key-rollover.md)

[OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)

