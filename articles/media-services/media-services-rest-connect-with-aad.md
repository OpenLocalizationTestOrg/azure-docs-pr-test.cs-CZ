---
title: "aaaUse Azure AD authentication tooaccess rozhraní API služby Azure Media Services se zbytkem | Microsoft Docs"
description: "Zjistěte, jak tooaccess rozhraní API služby Azure Media Services pomocí ověřování Azure Active Directory pomocí REST."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 114f7bdde3a8f5fe6189d712e05b6afdc8a8787a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-ad-authentication-tooaccess-hello-azure-media-services-api-with-rest"></a>Použití Azure AD authentication tooaccess hello rozhraní API služby Azure Media Services s REST

tým služby Azure Media Services Hello vydala Podpora ověřování Azure Active Directory (Azure AD) pro přístup k Azure Media Services. Oznámeno také plány toodeprecate řízení přístupu Azure ověřování služby pro přístup k Media Services. Protože každé předplatné služby Azure a každý účet Media Services je klient připojené tooan Azure AD, Azure AD Podpora ověřování přináší spoustu výhod, zabezpečení. Podrobnosti o této změny a migrace (Pokud používáte hello sady Media Services .NET SDK pro aplikaci), najdete v tématu hello následující příspěvky blogu a články:

- [Azure Media Services ohlášen podpory pro Azure AD a vyřazení ověřování řízení přístupu](https://azure.microsoft.com/blog/azure%20media%20service%20aad%20auth%20and%20acs%20deprecation)
- [Přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md)
- [Používání služby Azure AD authentication tooaccess rozhraní API služby Azure Media Services pomocí rozhraní Microsoft .NET](media-services-dotnet-get-started-with-aad.md)
- [Začínáme s ověřováním Azure AD pomocí portálu Azure hello](media-services-portal-get-started-with-aad.md)

Někteří zákazníci potřebovat toodevelop svá řešení Media Services v části hello následující omezení:

*   Používají programovací jazyk, který není rozhraní Microsoft .NET nebo C# nebo hello běhové prostředí není systému Windows.
*   Knihovny Azure AD, jako je například knihovny ověřování služby Active Directory nejsou k dispozici pro hello programovací jazyk nebo nelze použít pro jejich prostředí runtime.

Někteří zákazníci mají aplikace vyvinuté pomocí rozhraní REST API pro řízení přístupu, ověřování a přístup k Azure Media Services. U těchto zákazníků potřebujete jenom hello způsob toouse REST API pro ověřování Azure AD a následné Azure Media Services přístup. Je třeba toonot spoléhají na žádném z knihovny hello Azure AD nebo na hello sady Media Services .NET SDK. V tomto článku jsme popisují řešení a poskytují ukázkový kód pro tento scénář. Protože kód hello je všech voláních REST API s žádná závislost na všechny Azure AD nebo knihovny Azure Media Services hello kódu můžete snadno být přeložené tooany ostatní programovací jazyk.

> [!IMPORTANT]
> V současné době Media Services podporuje model ověřování služby Řízení přístupu Azure hello. Řízení přístupu ověřování však bude zastaralá 1. června 2018. Doporučujeme, abyste co nejdříve migraci model ověřování toohello Azure AD.


## <a name="design"></a>Návrh

V tomto článku používáme hello následující návrhu ověřování a autorizace:

*  Povolení protokolu: OAuth 2.0. OAuth 2.0 je standard zabezpečení webové, které pokrývá ověřování a autorizace. Podporuje Google, Microsoft, Facebook a PayPal. Ho byl ratifikován říjen 2012. Společnost Microsoft podporuje pevně OAuth 2.0 a OpenID Connect. Obě tyto standardy jsou podporovány více služeb a knihovny klienta, včetně Azure Active Directory, Open Web Interface pro .NET (OWIN) Katana a knihovny Azure AD.
*  Udělit typu: Typ udělení pověření klienta. Pověření klienta je jeden z typů čtyři grant hello v OAuth 2.0. Často se používá pro přístup k Azure AD Microsoft Graph API.
*  Režim ověřování: instanční objekt. Hello jiných režim ověřování je uživatele nebo interaktivního ověřování.

Celkem čtyři služby nebo aplikace jsou součástí tok ověřování a autorizace hello Azure AD pro pomocí služby Media Services. Hello aplikací a služeb a hello tok, jsou popsané v následující tabulce hello:

|Typ aplikace |Aplikace |Tok|
|---|---|---|
|Klient | Zákazník aplikace nebo řešení | Tuto aplikaci (ve skutečnosti, jeho proxy) je zaregistrován v klientovi Azure AD hello, ve které hello Azure media předplatné a hello umístěn účet služby. Hello instanční objekt hello registrované aplikace se pak udělí s vlastníkem nebo přispěvatelem rolí v hello řízení přístupu (IAM) účtu služby media hello. Hello instanční objekt je reprezentována hello app ID a klienta tajný klíč klienta. |
|Zprostředkovatel identity (IDP) | Azure AD jako poskytovatel identity | Azure AD jako hello IDP ověření objektu služby registrovaná aplikace Hello (ID klienta a tajný klíč klienta). Toto ověřování se provádí interně a implicitně. Jako tok přihlašovacích údajů klienta klient hello ověřen místo hello uživatele. |
|Zabezpečení tokenu služby (STS) nebo OAuth server | Azure AD jako služba tokenů zabezpečení | Po ověření hello IDP (nebo OAuth serveru v podmínkách OAuth 2.0) na přístupový token nebo Token pro webové JSON (JWT) vydává Azure AD jako Server služby tokenů zabezpečení nebo OAuth pro prostředek střední vrstvy toohello přístupu: v našem případě hello koncový bod Media Services REST API. |
|Prostředek | Rozhraní REST API služby Media Services | Každé volání Media Services REST API je autorizovat přístupový token, který je vydán Azure AD jako služba tokenů zabezpečení nebo hello OAuth server. |

Pokud spustíte hello ukázkový kód a zachycení token JWT nebo přístupový token, má hello JWT hello následující atributy:

    aud: "https://rest.media.azure.net",

    iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    iat: 1497146280,

    nbf: 1497146280,
    exp: 1497150180,

    aio: "Y2ZgYDjuy7SptPzO/muf+uRu1B+ZDQA=",

    appid: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6",

    appidacr: "1",

    idp: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    oid: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    sub: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    tid: "72f988bf-86f1-41af-91ab-2d7cd011db47",

Zde jsou hello mapování mezi hello atributů v hello JWT a hello čtyři aplikace nebo služby v předcházející tabulce hello:

|Typ aplikace |Aplikace |Atribut JWT |
|---|---|---|
|Klient |Zákazník aplikace nebo řešení |AppID: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6". ID klienta Hello aplikace zaregistrujete tooAzure AD v další části hello. |
|Zprostředkovatel identity (IDP) | Azure AD jako poskytovatel identity |rozšíření IDP: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/".  Hello GUID je klient ID Microsoft hello (microsoft.onmicrosoft.com). Každý klient má svou vlastní, jedinečné ID. |
|Zabezpečení tokenu služby (STS) nebo OAuth server |Azure AD jako služba tokenů zabezpečení | iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/". Hello GUID je klient ID Microsoft hello (microsoft.onmicrosoft.com). |
|Prostředek | Rozhraní REST API služby Media Services |oblast: "https://rest.media.azure.net". příjemce Hello nebo cílovou skupinou, kterou hello přístupový token. |

## <a name="steps-for-setup"></a>Kroky pro instalaci

tooregister a nastavit aplikaci Azure AD pro ověřování Azure AD a tooobtain přístupový token pro volání hello koncový bod REST API služby Azure Media Services, dokončení hello následující kroky:

1.  V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), registraci aplikace (například wzmediaservice) toohello Azure AD klient Azure AD (například microsoft.onmicrosoft.com). Nezáleží, jestli je zaregistrovaný jako webovou aplikaci nebo nativní aplikaci. Můžete také všechny přihlašovací adresa URL a adresa URL odpovědi (například http://wzmediaservice.com pro oba).
2. V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), přejděte toohello **konfigurace** kartě vaší aplikace. Poznámka: hello **ID klienta**. Potom v části **klíče**, generovat **klíč klienta** (tajný klíč klienta). 

    > [!NOTE] 
    > Poznamenejte si hello tajný klíč klienta. Nebude se zobrazovat znovu.
    
3.  V hello [portál Azure](http://ms.portal.azure.com), přejděte toohello účtu Media Services. Vyberte hello **řízení přístupu** podokně (IAM). Přidání nového člena, která obsahuje vlastníka hello nebo role Přispěvatel hello. Pro objekt zabezpečení hello vyhledejte hello název aplikace, které zaregistrovanou v kroku 1 (v tomto příkladu wzmediaservice).

## <a name="info-toocollect"></a>Informace o toocollect

tooprepare REST kódování, shromažďování hello následující body tooinclude data v hello kódu:

*   Azure AD jako koncový bod služby tokenů zabezpečení: https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token. Z tohoto koncového bodu se požaduje přístupový token JWT. Kromě toho tooserving jako IDP, Azure AD slouží také jako služby tokenů zabezpečení. Azure AD vydá token JWT pro přístup k prostředkům (přístupový token). JWT token má různé deklarace identity.
*   Azure Media Services REST API jako prostředek nebo cílové skupiny: https://rest.media.azure.net.
*   ID klienta: Viz krok 2 v [kroky pro instalaci](#steps-for-setup).
*   Tajný klíč klienta: viz krok 2 v [kroky pro instalaci](#steps-for-setup).
*   Účet Media Services koncový bod REST API v hello následující formát:

    https://[media_service_account_name].restv2. [data_center].media.azure.net/API 

    Toto je hello koncového bodu, na které všechny Media Services REST API v aplikaci volání. Například https://willzhanmswjapan.restv2.japanwest.media.azure.net/API.

Potom můžete umístit těchto pět parametry v souboru web.config nebo app.config toouse ve vašem kódu.

## <a name="sample-code"></a>Ukázka kódu

Můžete najít hello ukázkový kód v [ověřování Azure AD pro Azure Media Services přístup: I přes REST API](https://github.com/willzhan/WAMSRESTSoln).

Ukázkový kód Hello má dvě části:

*   Projekt knihovny DLL, který má všechny kód hello REST API pro Azure AD ověřování a autorizaci. Je také metodu pro provedení volání toohello Media Services REST API koncový bod REST API, s hello přístupový token.
*   Klient test, konzoly, která zahájí ověřování Azure AD a volá jinou Media Services REST API.

Hello ukázkový projekt má tři funkce:

*   Ověření Azure AD pomocí pověření klienta hello udělit pomocí pouze hello REST API.
*   Azure Media Services přístup pomocí pouze hello REST API.
*   Rozhraní REST API Azure přístup k úložišti pomocí pouze hello (jako použité toocreate účet Media Services pomocí rozhraní REST API).


## <a name="where-is-hello-refresh-token"></a>Kde je token obnovení hello?

Může požádat některé čtečky: kde je token obnovení hello? Proč není zde použít token obnovení?

účelem Hello obnovovací token není toorefresh přístupový token. Místo toho je navrženou toobypass ověřování nebo uživatele zásahu koncového uživatele a stále získat token platný přístup, když vyprší platnost tokenu starší. Lepší název tokenu obnovení může být něco jako "Nepoužívat zpětný-registrace v token uživatele."

Pokud používáte hello tok (uživatelské jméno a heslo, které jednají jménem uživatele) poskytování autorizace OAuth 2.0, obnovovací token umožňuje získání tokenu pro přístup k obnovené bez zásahu uživatele. Pro hello toku, který jsme popisují v tomto článku udělení pověření klienta OAuth 2.0, ale hello klienta funguje na vlastní účet. Vůbec nepotřebujete zásahu uživatele, a server ověřování hello nepotřebuje příliš (a nebude) získáte token obnovení. Pokud ladíte hello **GetUrlEncodedJWT** metoda, zjistíte, že má hello odpověď z koncového bodu token hello přístupový token, ale žádné token obnovení.

## <a name="next-steps"></a>Další kroky

Začínáme s [nahrávání souborů tooyour účet](media-services-dotnet-upload-files.md).
