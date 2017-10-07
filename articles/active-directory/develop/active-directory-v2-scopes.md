---
title: "aaaAzure služby Active Directory v2.0 obory, oprávnění a souhlasu | Microsoft Docs"
description: "Popis autorizace v koncového bodu v2.0 hello Azure AD, včetně obory, oprávnění a souhlasu."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 5721d368c435868bfb4ae91cff7fbb9bc4a79b66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scopes-permissions-and-consent-in-hello-azure-active-directory-v20-endpoint"></a>Obory, oprávnění a souhlasu v koncového bodu v2.0 hello Azure Active Directory
Aplikace, které se integrují s Azure Active Directory (Azure AD), postupujte podle modelu autorizace, který nabízí uživatelům kontrolu nad přístupu svá data aplikace. implementace v2.0 Hello modelu autorizace hello byl aktualizován a změní způsob, jakým aplikace musí komunikovat s Azure AD. Tento článek se zabývá základními koncepty hello tohoto modelu autorizace, včetně obory, oprávnění a souhlasu.

> [!NOTE]
> koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce. toodetermine zda byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
>
>

## <a name="scopes-and-permissions"></a>Obory a oprávnění
Azure AD implementuje hello [OAuth 2.0](active-directory-v2-protocols.md) protokol autorizace. OAuth 2.0 je metoda, pomocí kterého aplikace třetích stran mají přístup k webové hostované prostředkům jménem uživatele. Všechny webové hostované prostředek, který se integruje se službou Azure AD má identifikátor prostředku nebo *identifikátor ID URI aplikace*. Například některé společnosti Microsoft hostuje webové prostředky zahrnují:

* Hello unifikované API e-mailu aplikace Office 365:`https://outlook.office.com`
* Hello Azure AD Graph API:`https://graph.windows.net`
* Microsoft Graph:`https://graph.microsoft.com`

Hello totéž platí pro všechny prostředky třetích stran, které mají integrované s Azure AD. Kterýkoli z těchto prostředků také můžete definovat sadu oprávnění, které se dají použít toodivide hello funkce prostředku do menší skupiny. Jako příklad [Microsoft Graph](https://graph.microsoft.io) nemá definovánu hello toodo oprávnění následující úlohy, mimo jiné:

* Číst kalendář uživatele
* Zápis kalendáře tooa uživatele
* Odesílání pošty jménem uživatele

Definováním tyto typy oprávnění má hello prostředků jemně odstupňovanou kontrolu nad jeho data a jak je vystaven hello data. Aplikace jiných výrobců může požádat o oprávnění z aplikace uživatele. uživatel aplikace Hello musí před hello aplikace může fungovat jménem uživatele hello schválit hello oprávnění. Podle rozdělování funkce hello prostředků do menší sady oprávnění, může být aplikace jiných výrobců integrovaný toorequest pouze hello konkrétní oprávnění, které potřebují tooperform jejich funkce. Uživatelé aplikace můžete věděli, přesně jak aplikace bude používat svá data a může se jednat o větší jistotu, že tuto aplikaci hello není chovají se zlými úmysly.

Ve službě Azure AD a OAuth, se nazývají tyto typy oprávnění *obory*. Jsou také někdy označují tooas *oAuth2Permissions*. Obor představuje ve službě Azure AD hodnotu řetězce. Příklad Microsoft Graph hello budete pokračovat, hello oboru hodnota pro každé oprávnění je:

* Číst kalendář uživatele pomocí`Calendar.Read`
* Zapsat kalendáře tooa uživatele pomocí`Mail.ReadWrite`
* Odesílat poštu jménem uživatele pomocí podle`Mail.Send`

Aplikace může požádat o tato oprávnění zadáním hello obory v koncového bodu v2.0 toohello požadavky.

## <a name="openid-connect-scopes"></a>OpenID Connect oborů
Hello v2.0 implementaci OpenID Connect obsahuje několik dobře definovaný obory, které se nevztahují tooa konkrétní prostředek: `openid`, `email`, `profile`, a `offline_access`.

### <a name="openid"></a>openid
Pokud aplikace provede přihlášení pomocí [OpenID Connect](active-directory-v2-protocols.md), musí požádat hello `openid` oboru. Hello `openid` oboru ukazuje na hello pracovní účet souhlas stránky jako hello oprávnění "Přihlásit" a na osobní účet Microsoft hello souhlas stránky jako hello "Zobrazit váš profil a připojit tooapps a služby pomocí účtu Microsoft" oprávnění. Aplikace s tímto oprávněním může přijímat jedinečný identifikátor pro uživatele hello hello tvar hello `sub` deklarací identity. Dává také koncový bod hello aplikace přístup toohello informací o uživateli. Hello `openid` oboru může být použit ve hello v2.0 koncový bod tokenu tooacquire tokeny typu ID, které můžou být použité toosecure HTTP volání mezi různými součástmi aplikace.

### <a name="email"></a>E-mailu
Hello `email` oboru lze použít s hello `openid` oboru a všechny další. Nabízí hello aplikace přístup toohello primární e-mailovou adresu uživatele v hello formu hello `email` deklarací identity. Hello `email` deklarace identity je součástí token pouze v případě, že je přidružen hello uživatelský účet, který není vždy hello případ e-mailovou adresu. Pokud používá hello `email` oboru, vaše aplikace by měla být připravené toohandle velká písmena v které hello `email` deklarace identity v tokenu hello neexistuje.

### <a name="profile"></a>Profil
Hello `profile` oboru lze použít s hello `openid` oboru a všechny další. Nabízí hello aplikace přístup tooa vyžadovat značné množství informací o uživateli hello. zahrnuje Hello informace, které má přístup, ale není omezena na hello uživatele křestní jméno, příjmení, upřednostňované uživatelské jméno a ID objektu Úplný seznam hello profil deklarací identity v parametru id_tokens hello k dispozici pro konkrétního uživatele, najdete v části hello [v2.0 tokeny odkaz](active-directory-v2-tokens.md).

### <a name="offlineaccess"></a>offline_access
Hello [ `offline_access` oboru](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) dává tooresources vaší aplikace přístup jménem uživatele hello delší dobu. Na stránce hello pracovní účet souhlasu se tento obor jako hello "Přístup k datům kdykoli" oprávnění. Na hello osobní stránky účtu Microsoft souhlasu zobrazí se jako hello "Přístup k informacím kdykoli" oprávnění. Když uživatel schválí hello `offline_access` oboru, vaše aplikace může přijímat tokeny obnovení z tokenu koncového bodu v2.0 hello. Tokeny obnovení je dlouhodobé. Aplikaci můžete získat nové přístupové tokeny, protože platnost vyprší starší.

Pokud aplikace nevyžaduje hello `offline_access` oboru, neobdrží obnovovacích tokenů. To znamená, že když uplatnit autorizační kód v hello [toku kódu autorizace OAuth 2.0](active-directory-v2-protocols.md), dostanete pouze přístupový token z hello `/token` koncový bod. Hello přístupový token je platný po krátkou dobu. Hello přístupovému tokenu vyprší platnost obvykle za jednu hodinu. V tomto bodě vaše aplikace musí tooredirect hello uživatele zpět toohello `/authorize` tooget koncový bod nové autorizační kód. Během této přesměrování, v závislosti na typu hello aplikace může uživatel hello potřebovat tooenter jejich přihlašovací údaje znovu nebo souhlas toopermissions znovu.

Další informace o tom, jak tooget a použití obnovovacích tokenů, najdete v části hello [referenci na protokol v2.0](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Vyžaduje souhlas jednotlivé uživatele
V [OpenID Connect nebo OAuth 2.0](active-directory-v2-protocols.md) požádat o autorizaci, aplikace můžete požádat o oprávnění hello je nutné pomocí hello `scope` parametr dotazu. Například pokud se uživatel přihlásí v aplikaci tooan, hello aplikace odešle požadavek jako hello následující ukázka (pomocí konců řádků přidat pro čitelnost):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

Hello `scope` parametr se požaduje seznam obory, které aplikace hello oddělených mezerami. Každý obor je indikován připojování identifikátor hello oboru hodnota toohello prostředku (hello identifikátor ID URI aplikace). V příkladu požadavek hello hello aplikace potřebuje oprávnění tooread hello kalendáře uživatele a odeslání e-mailu jako uživatel hello.

Po hello uživatel zadá své přihlašovací údaje, koncového bodu v2.0 hello vyhledává odpovídající záznam *souhlas uživatele*. Pokud se nedala souhlas uživatele hello tooany hello požadovaná oprávnění v minulosti, hello koncového bodu v2.0 hello požádá uživatele toogrant hello hello požadovaná oprávnění.

![Pracovní účet souhlasu](../../media/active-directory-v2-flows/work_account_consent.png)

Když uživatel hello schválí hello oprávnění, hello souhlasu se zaznamenává tak, aby hello uživatel nemá tooconsent znovu na následující účet přihlášení.

## <a name="requesting-consent-for-an-entire-tenant"></a>Vyžaduje souhlas pro celý klienta
Často organizace zakoupí licence nebo předplatné pro aplikaci, hello organizace chce toofully zřídit hello aplikace pro své zaměstnance. V rámci tohoto procesu můžete udělit správce pro tooact aplikace hello souhlas jménem libovolného zaměstnance. Pokud dobrý den, správce uděluje souhlas pro celý klienta hello, hello zaměstnanci zobrazen na stránce souhlasu pro aplikaci hello.

toorequest souhlasu pro všechny uživatele v klientovi, vaše aplikace můžete použít hello koncový bod souhlas správce.

## <a name="admin-restricted-scopes"></a>Správce omezený oborů
Některé vysoká oprávnění v ekosystému Microsoft hello lze nastavit příliš*omezený správce*. Tyto druhy obory příklady hello následující oprávnění:

* Čtení dat adresáře organizace pomocí`Directory.Read`
* Zápis dat tooan adresáři organizace pomocí`Directory.ReadWrite`
* Číst pomocí skupin zabezpečení v adresáři organizace`Groups.Read.All`

I když uživatel příjemce může udělit toothis přístup aplikaci typ dat, jsou omezené na organizační uživatele z udělení přístupu toohello stejná sada citlivá firemní data. Pokud vaše aplikace požaduje přístup tooone těchto oprávnění od organizace uživatele, uživatel hello obdrží chybovou zprávu, která uvádí, že se nejedná o oprávnění autorizovaný tooconsent tooyour aplikace.

Pokud vaše aplikace vyžaduje přístup omezený tooadmin obory pro organizace, měli byste požádat o je přímo ze Správce společnosti, také pomocí hello správce souhlasu endpoint, která je popsána dále.

Když správce uděluje, že tato oprávnění prostřednictvím Dobrý den, správce souhlas koncový bod, je pro všechny uživatele v klientovi hello udělen souhlas.

## <a name="using-hello-admin-consent-endpoint"></a>Pomocí koncový bod souhlas správce hello
Pokud budete postupovat podle těchto kroků, vaše aplikace může shromažďovat oprávnění pro všechny uživatele v klientovi, včetně správce omezený obory. toosee ukázka kódu, který implementuje hello kroky v tématu hello [omezený správce obory ukázka](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-hello-permissions-in-hello-app-registration-portal"></a>Žádostí o oprávnění hello v portálu pro registraci aplikace hello
1. Přejděte tooyour aplikace hello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo [vytvořit aplikaci](active-directory-v2-app-registration.md) Pokud jste tak ještě neučinili.
2. Vyhledejte hello **Microsoft Graph oprávnění** části a poté přidejte hello oprávnění, která vaše aplikace vyžaduje.
3. Ověřte, že je **Uložit** hello registrace aplikací.

### <a name="recommended-sign-hello-user-in-tooyour-app"></a>Doporučená: Přihlašovací hello uživatele v aplikaci tooyour
Obvykle když vytvoříte aplikaci hello správce souhlasu koncový bod, který používá, aplikace hello musí stránku nebo zobrazení, ve které hello můžete schválit správce oprávnění aplikace hello. Tato stránka může být součástí procesu registrace tok hello aplikace, součást aplikace hello nastavení, nebo může být vyhrazený "připojit" toku. V mnoha případech má smysl pro tooshow aplikace hello to "připojit" Zobrazit pouze po je uživatel přihlášený pomocí pracovního nebo školního účtu Microsoft.

Když se přihlásíte hello uživatele v aplikaci tooyour, můžete identifikovat hello organizace toowhich Dobrý den, správce patří před výzvou tooapprove hello potřebná oprávnění. I když je to nezbytně nutné, ho můžete vytvořit více intuitivní prostředí pro organizační uživatele. toosign hello uživatele v, postupujte podle našich [v2.0 protokol kurzy](active-directory-v2-protocols.md).

### <a name="request-hello-permissions-from-a-directory-admin"></a>Požádat správce directory hello oprávnění
Pokud jste připravené toorequest oprávnění správce vaší organizace, můžete přesměrovat hello uživatele toohello v2.0 *koncový bod admin souhlasu*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting hello below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametr | Podmínka | Popis |
| --- | --- | --- |
| Klienta |Požaduje se |Hello directory klienta, který má oprávnění toorequest z. Lze zadat ve formátu popisný název nebo identifikátor GUID. |
| client_id |Požaduje se |ID tohoto hello Hello aplikace [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) přiřazené tooyour aplikace. |
| redirect_uri |Požaduje se |identifikátor URI, kam chcete toobe odpovědi hello odeslané pro vaše aplikace toohandle přesměrování Hello. Se musí přesně shodovat s jedním hello přesměrování identifikátory URI, který je zaregistrovaný v portálu pro registraci aplikace hello. |
| state |Doporučené |Hodnota součástí hello požadavek, který bude vrácen také v odpovědi tokenu hello. Může být řetězec o délce požadovaný obsah. Použijte hello stavu tooencode informace o stavu hello uživatele v aplikaci hello předtím, než požadavek na ověření hello došlo k chybě, například stránku hello nebo zobrazení, které byly na. |

V tomto okamžiku vyžaduje Azure AD toosign správce klienta v toocomplete hello požadavku. Hello správce se zobrazí výzva, tooapprove všechny hello oprávnění, které jste požadovali pro aplikaci v portálu pro registraci aplikace hello.

#### <a name="successful-response"></a>Úspěšná odpověď
Pokud dobrý den, správce schválí hello oprávnění pro vaši aplikaci, úspěšné odpovědi hello vypadá takto:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametr | Popis |
| --- | --- | --- |
| Klienta |Hello directory klienta, který hello oprávnění vaší aplikace je požadovaná, ve formátu GUID. |
| state |Hodnota součástí hello požadavek, který bude také vrácen v odpovědi tokenu hello. Může být řetězec o délce požadovaný obsah. Stav Hello je použité tooencode informace o stavu hello uživatele v aplikaci hello před došlo k požadavku hello ověřování, jako je například stránku hello nebo zobrazení, které byly na. |
| admin_consent |Se nastaví příliš**true**. |

#### <a name="error-response"></a>Chybové odpovědi
Pokud dobrý den, správce nebude schvalovat hello oprávnění pro vaši aplikaci, se nezdařilo hello odpovědi vypadá takto:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametr | Popis |
| --- | --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby. |

Po přijetí úspěšná odpověď z koncového bodu souhlas správce hello si získávají aplikace hello oprávnění, která byla požadována. Dále může požádat o token pro hello prostředek, který chcete.

## <a name="using-permissions"></a>Použití oprávnění
Po hello uživatel souhlasí toopermissions pro vaši aplikaci, vaše aplikace může získat přístupové tokeny, které představují vaší aplikace oprávnění tooaccess prostředku v některých kapacity. Přístupový token lze použít pouze pro jediný zdroj, ale kódovaný uvnitř hello přístupový token je každý oprávnění, kterému byla udělena vaší aplikace pro tento prostředek. tooacquire přístupový token, aplikace můžete provést žádost toohello tokenu koncovým bodem v2.0, například takto:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Hello výsledný token přístupu můžete použít v prostředku toohello požadavky HTTP. Označuje spolehlivě toohello prostředků, které má vaše aplikace hello správné oprávnění tooperform konkrétní úlohu.  

Další informace o hello OAuth 2.0 protokolu a jak tooget přístupových tokenů, najdete v části hello [referenci na protokol koncový bod v2.0](active-directory-v2-protocols.md).
