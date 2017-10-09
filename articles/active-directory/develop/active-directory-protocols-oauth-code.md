---
title: "aaaUnderstand hello toku kódu autorizace OAuth 2.0 ve službě Azure AD | Microsoft Docs"
description: "Tento článek popisuje, jak přistupovat k toouse HTTP zprávy tooauthorize tooweb aplikací a webových rozhraní API ve vašem klientovi pomocí Azure Active Directory a OAuth 2.0."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: de3412cb-5fde-4eca-903a-4e9c74db68f2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4a6fe67d786a5fcb87d1059c2e94ba0c88d26cd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Autorizace přístupu tooweb aplikací pomocí OAuth 2.0 a Azure Active Directory
Azure Active Directory (Azure AD) používá tooenable OAuth 2.0, můžete tooauthorize přístup tooweb aplikací a webových rozhraní API v klientovi služby Azure AD. Tento průvodce je závislý na jazyce a popisuje, jak toosend a přijímat zprávy HTTP bez použití některé z našich knihovny open-source.

Hello toku kódu autorizace OAuth 2.0 je popsaná v [části 4.1 hello OAuth 2.0 specifikace](https://tools.ietf.org/html/rfc6749#section-4.1). Je použité tooperform ověřování a autorizace ve většině typy aplikací, včetně webových aplikací a nativně nainstalované aplikace.

[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)]

## Tok ověřování OAuth 2.0
Na vysoké úrovni tok hello celý autorizace pro aplikaci trochu vypadat třeba takto:

![Ověřování kódu toku OAuth](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)

## Žádost o autorizační kód
tok autorizačního kódu Hello začíná textem hello klienta odkazovat hello uživatele toohello `/authorize` koncový bod. V této žádosti o hello klient určuje hello oprávnění potřebná tooacquire od uživatele hello. Koncové body hello OAuth 2.0 můžete získat ze stránky vaší aplikace na portálu Azure Classic, hello **zobrazit koncové body** tlačítko v zásuvce dolní hello.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parametr |  | Popis |
| --- | --- | --- |
| Klienta |Požadované |Hello `{tenant}` hodnota v cestě hello hello žádosti může být použité toocontrol, který se můžete přihlásit do aplikace hello.  Hello povolené hodnoty jsou identifikátory klienta, například `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` nebo `contoso.onmicrosoft.com` nebo `common` pro tokeny nezávislé na klienta |
| client_id |Požadované |Hello Id aplikace přiřazené tooyour aplikace při registraci s Azure AD. Tento nástroj naleznete v hello portálu Azure. Klikněte na tlačítko **služby Active Directory**, klikněte na adresář hello, zvolte hello aplikace a klikněte na **konfigurace** |
| response_type |Požadované |Musí zahrnovat `code` pro tok autorizačního kódu hello. |
| redirect_uri |Doporučená |Hello redirect_uri vaší aplikace, kde můžete odesílat a přijímat aplikace odpovědi ověřování.  Se musí přesně shodovat s jedním z hello redirect_uris, registrován v hello portál, s výjimkou musí být kódovaná adresou url.  Nativní & mobilní aplikace, měli byste použít výchozí hodnotu hello `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode |Doporučená |Určuje metodu hello, který by měl být použité toosend hello výsledný token back tooyour aplikace.  Může být `query` nebo `form_post`. |
| state |Doporučená |Hodnota součástí hello požadavek, který je také vrácený v odpovědi tokenu hello. Náhodně generované jedinečné hodnoty se obvykle používá u [prevence útoků padělání požadavku posílaného mezi weby](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav Hello je také použít tooencode informace o stavu hello uživatele v aplikaci hello před došlo k požadavku hello ověřování, jako je například stránku hello nebo zobrazení, které byly na. |
| Prostředek |Volitelné |Hello identifikátor ID URI aplikace hello webových rozhraní API (zabezpečené prostředků). Klikněte na tlačítko toofind hello identifikátor ID URI aplikace hello webových rozhraní API, v hello portálu Azure, **služby Active Directory**, klikněte na tlačítko hello adresář, klikněte na tlačítko hello aplikace a pak klikněte na **konfigurace**. |
| řádku |Volitelné |Označuje typ hello interakce s uživatelem, který je vyžadován.<p> Platné hodnoty jsou: <p> *přihlášení*: hello uživatel by měl být výzvami tooreauthenticate. <p> *souhlas*: souhlas uživatele byla udělena, je však nutné toobe aktualizovat. Hello uživatel by měl být výzvami tooconsent. <p> *admin_consent*: Správce by měl být výzvami tooconsent jménem všichni uživatelé v organizaci |
| login_hint |Volitelné |Může být použité toopre výplně hello uživatelského jména nebo e-mailovou adresu pole hello přihlašovací stránky pro uživatele hello, pokud znáte svoje uživatelské jméno předem.  Tento parametr použijte často aplikace během opětovné ověření, uživatelské jméno hello s z předchozí přihlášení už extrahovat pomocí hello `preferred_username` deklarací identity. |
| domain_hint |Volitelné |Poskytuje nápovědu o hello klienta nebo doménu, která hello uživatel by měl používat toosign v. Hodnota Hello hello domain_hint je registrované domény pro klienta hello. Pokud klient hello federované tooan místní adresář, přesměruje AAD toohello zadaného klienta federační server. |

> [!NOTE]
> Pokud je uživatel hello součástí organizace, správce organizace hello souhlas nebo odmítnout jménem uživatele hello nebo povolit tooconsent hello uživatele. Hello uživatel hello možnost tooconsent jenom v případě, že správce hello povolí ho.
>
>

V tomto okamžiku se hello uživateli se zobrazí výzva tooenter své přihlašovací údaje a souhlasu toohello oprávnění uvedené v hello `scope` parametr dotazu. Jakmile uživatel hello ověřuje a uděluje souhlas, Azure AD odešle tooyour aplikace na odpovědi na hello `redirect_uri` adres ve vaší žádosti.

### Úspěšná odpověď
Úspěšná odpověď může vypadat například takto:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parametr | Popis |
| --- | --- |
| admin_consent |Hodnota Hello má hodnotu True, pokud správce dá souhlas tooa souhlasu požadavek řádku. |
| Kód |Hello autorizační kód, který aplikace hello požadavek. Hello aplikace můžete použít pro cílový prostředek hello hello autorizační kód toorequest přístupový token. |
| session_state |Jedinečná hodnota, která identifikuje hello se aktuální uživatelská relace. Tato hodnota je identifikátor GUID, ale má být považována za neprůhledné hodnoty, které jsou předávány bez prozkoumání. |
| state |Pokud parametr stavu je zahrnuta v žádosti o hello, hello stejnou hodnotu by se zobrazit v odpovědi hello. Je vhodné pro tooverify aplikace hello, hodnot stavu hello v hello žádostí a odpovědí se shodují před použitím hello odpovědi. To pomáhá toodetect [webů požadavku padělání (proti útokům CSRF) před útoky](https://tools.ietf.org/html/rfc6749#section-10.12) proti hello klienta. |

### Chybové odpovědi
Chybové odpovědi se taky může odeslat toohello `redirect_uri` tak, aby aplikace hello můžete správně zpracovat.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| --- | --- |
| error |K chybě kódu hodnota definovaná v části 5.2 hello [Framework autorizace OAuth 2.0](http://tools.ietf.org/html/rfc6749). Další tabulka Hello popisuje hello chybové kódy, které vrátí Azure AD. |
| error_description |Podrobnější popis chyby hello. Tato zpráva není určen popisný toobe koncového uživatele. |
| state |hodnota stavu Hello je náhodně generované-znovu použít hodnotu, která se odesílají v hello požadavek a vrátila útocích hello odpovědi tooprevent požadavku posílaného mezi weby padělání (proti útokům CSRF). |

#### Kódy chyb pro chyb koncový bod autorizace
Hello následující tabulka popisuje hello různé kódy chyb, které mohou být vráceny v hello `error` parametr hello chybové odpovědi.

| Kód chyby | Popis | Akce klienta |
| --- | --- | --- |
| invalid_request |Chyba protokolu, například chybějící povinný parametr. |Vyřešte a znovu odešlete žádost hello. To je chyba vývoj a je obvykle zachycena během počáteční testování. |
| unauthorized_client |Hello klientská aplikace není povolená toorequest autorizační kód. |K tomu obvykle dojde, když hello klientská aplikace není registrovaný ve službě Azure AD nebo klienta Azure AD toohello uživatele nebyla přidána. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |
| ACCESS_DENIED |Vlastník prostředku odepřen souhlasu |klientská aplikace Hello uvědomí hello uživatele, který nemůže pokračovat, dokud hello uživatel souhlasí. |
| unsupported_response_type |Hello autorizace server nepodporuje typ odpovědi hello v žádosti o hello. |Vyřešte a znovu odešlete žádost hello. To je chyba vývoj a je obvykle zachycena během počáteční testování. |
| server_error |Hello serveru došlo k neočekávané chybě. |Opakujte žádost hello. Tyto chyby může být důsledkem dočasné stavy. klientská aplikace Hello toohello uživatele mohou vysvětlit, že odpovědi je zpožděno z důvodu tooa dočasné chybě. |
| temporarily_unavailable |Hello server je dočasně zaneprázdněn toohandle hello požadavku. |Opakujte žádost hello. klientská aplikace Hello mohou vysvětlit toohello uživatele, odpovědi je zpožděno z důvodu tooa dočasné podmínce. |
| invalid_resource |Hello cílového prostředku je neplatný, protože neexistuje, Azure AD ji nemůže najít, nebo není správně nakonfigurována. |To znamená, že prostředek hello, pokud existuje, není nakonfigurované v klientovi hello. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |

## Použít hello autorizační kód toorequest přístupový token
Teď, když jste získali autorizační kód a bylo uděleno oprávnění uživatelem hello, lze uplatnit hello kód pro prostředek tokenu toohello potřeby přístup odesláním toohello požadavek POST `/token` koncový bod:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parametr |  | Popis |
| --- | --- | --- |
| Klienta |Požadované |Hello `{tenant}` hodnota v cestě hello hello žádosti může být použité toocontrol, který se můžete přihlásit do aplikace hello.  Hello povolené hodnoty jsou identifikátory klienta, například `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` nebo `contoso.onmicrosoft.com` nebo `common` pro tokeny nezávislé na klienta |
| client_id |Požadované |Hello Id aplikace přiřazené tooyour aplikace při registraci s Azure AD. Tento nástroj naleznete v hello portálu Azure Classic. Klikněte na tlačítko **služby Active Directory**, klikněte na adresář hello, zvolte hello aplikace a klikněte na **konfigurace** |
| grant_type |Požadované |Musí být `authorization_code` pro tok autorizačního kódu hello. |
| Kód |Požadované |Hello `authorization_code` kterou jste získali v předchozí části hello |
| redirect_uri |Požadované |Hello stejné `redirect_uri` hodnotu, která byla hello použité tooacquire `authorization_code`. |
| tajný klíč client_secret |vyžaduje se pro webové aplikace |Hello aplikace tajný klíč, který jste vytvořili v portálu pro registraci aplikace hello pro vaši aplikaci.  Není vhodné jej použít v nativní aplikaci, protože client_secrets nelze uložit spolehlivě na zařízení.  Je vyžadováno pro webové aplikace a webové rozhraní API, které mají hello možnost toostore hello `client_secret` bezpečně na straně serveru hello. |
| Prostředek |vyžaduje se, pokud zadaný v autorizační kód požadavku, jinak volitelné |Hello identifikátor ID URI aplikace hello webových rozhraní API (zabezpečené prostředků). |

Klikněte na tlačítko toofind hello identifikátor ID URI aplikace v portálu pro správu Azure, hello **služby Active Directory**, klikněte na adresář hello, klikněte na tlačítko aplikace hello a pak klikněte na tlačítko **konfigurace**.

### Úspěšná odpověď
Azure AD vrátí přístupový token při úspěšné odpovědi. toominimize sítě volání z aplikace hello klienta a jejich přidružené latence, by měl hello klientská aplikace mezipaměti přístupových tokenů pro hello životnost tokenu, který je uveden v hello odpovědi OAuth 2.0. životnost tokenu hello toodetermine, použijte buď hello `expires_in` nebo `expires_on` hodnoty parametrů.

Webové rozhraní API prostředků vrátí-li `invalid_token` kódu chyby to může znamenat, že hello prostředků bylo zjištěno, že hello tokenu vypršela platnost. Pokud hello klienta a prostředků hodiny časy jsou různé (známé jako "zkosení čas"), zvažte hello prostředků, než se vymaže hello tokenu z mezipaměti klienta hello vypršelo tokenu toobe hello. Pokud k tomu dojde, zrušte hello tokenu z mezipaměti hello, i když je stále v rámci počítané celé jeho životnosti.

Úspěšná odpověď může vypadat například takto:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
  "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ."
}

```

| Parametr | Popis |
| --- | --- |
| access_token |Hello požadovaný přístupový token. Hello aplikace můžete používat tento token tooauthenticate toohello zabezpečeným prostředkům, například webového rozhraní API. |
| token_type |Určuje hodnotu hello typ tokenu. Hello pouze zadejte, že podporuje Azure AD je nosiče. Další informace o nosné tokeny najdete v tématu [OAuth2.0 autorizace Framework: použití tokenů nosiče (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt) |
| expires_in |Jak dlouho hello přístupový token je platný (v sekundách). |
| expires_on |Hello čas, kdy vyprší platnost hello přístupový token. Datum Hello je reprezentován jako hello počet sekund od pod hodnotou 1970-01-01T0:0:0Z UTC dokud hello čas vypršení platnosti. Tato hodnota je použité toodetermine hello Životnost mezipaměti tokenů. |
| Prostředek |Hello identifikátor ID URI aplikace hello webových rozhraní API (zabezpečené prostředků). |
| Obor |Udělení oprávnění zosobnění toohello klientské aplikace. Hello výchozí oprávnění je `user_impersonation`. Vlastník Hello hello zabezpečené prostředků můžete zaregistrovat další hodnoty ve službě Azure AD. |
| refresh_token |Aktualizace tokenu OAuth 2.0. Hello aplikace můžete používat tento token tooacquire další přístupové tokeny, po vypršení platnosti hello aktuální přístupový token.  Aktualizujte tokeny je dlouhodobé a může být použité tooretain přístup tooresources pro dlouhou dobu. |
| požadavku id_token |Nepodepsané JSON Web Token (JWT). Hello aplikace může base64Url dekódovat hello segmenty tento token toorequest informací o hello uživatele, který přihlášení. aplikace Hello můžete hello hodnoty do mezipaměti a jejich zobrazení, ale na ně neměli spoléhat pro žádné hranice zabezpečení nebo autorizace. |

### Deklarace tokenů JWT
token JWT Hello v hello hodnotu hello `id_token` parametr lze dekódovat do hello následující deklarace identity:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

Další informace o webových tokenů JSON najdete v tématu hello [specifikaci JWT IETF koncept](http://go.microsoft.com/fwlink/?LinkId=392344). Další informace o hello typy tokenů a deklarací identity, najdete v tématu [podporované tokenu a typy deklarací identity](active-directory-token-and-claims.md)

Hello `id_token` parametr obsahuje hello následující typy deklarací identity:

| Typ deklarace identity | Popis |
| --- | --- |
| oblast |Cílová skupina hello tokenu. Pokud hello token je vydán tooa klientskou aplikaci, cílová skupina hello je hello `client_id` hello klienta. |
| Exp |Čas vypršení platnosti. Hello čas, kdy vyprší platnost tokenu hello. Hello tokenu toobe platný, hello aktuální datum a čas musí být menší než nebo rovna toohello `exp` hodnotu. čas Hello je reprezentován jako hello počet sekund od 1. ledna 1970 (pod hodnotou 1970-01-01T0:0:0Z) dokud byl vydán token hello hello čas UTC. |
| family_name |Uživatelské poslední jméno nebo příjmení. Tuto hodnotu můžete zobrazit aplikace Hello. |
| given_name |Křestní jméno uživatele. Tuto hodnotu můžete zobrazit aplikace Hello. |
| IAT |Vydané v čase. Hello čas, kdy byl vydán hello JWT. čas Hello je reprezentován jako hello počet sekund od 1. ledna 1970 (pod hodnotou 1970-01-01T0:0:0Z) dokud byl vydán token hello hello čas UTC. |
| iss |Identifikuje vydavatel tokenu hello |
| NBF |Neplatný před čas. Hello čas, kdy začne platit hello token. Pro hello tokenu toobe platný hello aktuální datum a čas musí být větší než nebo rovna toohello Nbf hodnota. čas Hello je reprezentován jako hello počet sekund od 1. ledna 1970 (pod hodnotou 1970-01-01T0:0:0Z) dokud byl vydán token hello hello čas UTC. |
| OID |Identifikátor objektu (ID) hello objektu uživatele ve službě Azure AD. |
| Sub – |Identifikátor tokenu subjektu. Toto je trvalé a nezměnitelné identifikátor pro popisuje hello uživatele, který hello tokenu. Použijte tuto hodnotu v ukládání do mezipaměti logiku. |
| TID |Klienta identifikátor (ID) klienta hello Azure AD, která vydala hello token. |
| unique_name |Jedinečný identifikátor, který může být zobrazených toohello uživatele. Je to obvykle hlavní název uživatele (UPN). |
| hlavní název uživatele |Hlavní název uživatele hello uživatele. |
| ver |Verze. verze Hello hello token JWT, obvykle 1.0. |

### Chybové odpovědi
Hello vystavování tokenů koncový bod chyby jsou kódy chyb protokolu HTTP, protože volání klienta hello hello koncový bod vystavování tokenů přímo. Kromě toho toohello HTTP stavový kód, koncový bod vystavování tokenů Azure AD hello také vrátí hodnotu dokumentu JSON s objekty, které popisují chybu hello.

Ukázková chyba odpověď může vypadat například takto:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: hello provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |
| error_codes |Seznam tokenů zabezpečení specifické chybové kódy, které vám můžou pomoct při diagnostiky. |
| časové razítko |Hello čas, kdy došlo k chybě hello. |
| trace_id |Jedinečný identifikátor pro hello požadavek, který vám pomůže v diagnostice. |
| correlation_id |Jedinečný identifikátor pro hello požadavek, který vám pomůže v diagnostice mezi komponentami. |

#### Stavové kódy HTTP
Hello následující tabulka uvádí hello HTTP stavové kódy, které hello vrátí koncový bod vystavování tokenů. V některých případech je kód chyby hello dostatečná toodescribe hello odpovědi, ale pokud nejsou chyby, je třeba tooparse hello doplňujícími JSON dokumentu a zkontrolujte jeho kód chyby.

| Kód HTTP | Popis |
| --- | --- |
| 400 |Výchozí kód HTTP. Používá se ve většině případů a obvykle kvůli tooa chybně vytvořený požadavek. Vyřešte a znovu odešlete žádost hello. |
| 401 |Ověření se nezdařilo. Například hello v požadavku chybí parametr tajný klíč client_secret hello. |
| 403 |Ověření se nezdařilo. Například hello uživatel nemá oprávnění tooaccess hello prostředků. |
| 500 |U služby hello došlo k vnitřní chybě. Opakujte žádost hello. |

#### Kódy chyb pro koncový bod tokenu chyby
| Kód chyby | Popis | Akce klienta |
| --- | --- | --- |
| invalid_request |Chyba protokolu, například chybějící povinný parametr. |Vyřešte a znovu odešlete žádost hello |
| invalid_grant |Hello autorizační kód je neplatná nebo skončila jeho platnost. |Zkuste nové žádosti o toohello `/authorize` koncový bod |
| unauthorized_client |Hello ověřený klient není autorizován toouse Tato autorizace udělit typu. |K tomu obvykle dojde, když hello klientská aplikace není registrovaný ve službě Azure AD nebo klienta Azure AD toohello uživatele nebyla přidána. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |
| invalid_client |Ověření klienta se nezdařilo. |pověření klienta Hello nejsou platné. toofix, Správce aplikací hello aktualizuje hello přihlašovací údaje. |
| unsupported_grant_type |autorizace serveru Hello nepodporuje typ udělení autorizace hello. |Změna hello udělit typu v žádosti o hello. Tento typ chyby provedeno pouze během vývoje a zjistit během počáteční testování. |
| invalid_resource |Hello cílového prostředku je neplatný, protože neexistuje, Azure AD ji nemůže najít, nebo není správně nakonfigurována. |To znamená, že prostředek hello, pokud existuje, není nakonfigurované v klientovi hello. aplikace Hello můžete vyzvat uživatele hello s pokyny pro instalaci aplikace hello a její přidání tooAzure AD. |
| interaction_required |žádost o Hello vyžaduje interakci uživatele. Na další ověřování krok je třeba požadovaný. | Místo neinteraktivní žádost, opakujte akci s požadavek interaktivní autorizace pro hello stejné prostředků. |
| temporarily_unavailable |Hello server je dočasně zaneprázdněn toohandle hello požadavku. |Opakujte žádost hello. klientská aplikace Hello mohou vysvětlit toohello uživatele, odpovědi je zpožděno z důvodu tooa dočasné podmínce. |

## Použít hello přístup tokenu tooaccess hello prostředků
Teď, když jste byla úspěšně načtena `access_token`, můžete použít hello token v žádosti o tooWeb rozhraní API, včetně v hello `Authorization` záhlaví. Hello [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) specifikace vysvětluje, jak toouse nosné tokeny v tooaccess požadavky HTTP chráněné zdroje.

### Ukázková žádost
```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### Chybové odpovědi
Zabezpečené prostředky, které implementují stavové kódy HTTP problém RFC 6750. Pokud požadavek hello nezahrnuje pověření pro ověření nebo chybí hello odpovědi tokenu, hello zahrnuje `WWW-Authenticate` záhlaví. Pokud se požadavek nezdaří, hello server prostředků odpoví s kódem stavu HTTP hello a chybový kód.

Hello tady je příklad úspěšné odpovědi, když požadavek klienta hello nezahrnuje token nosiče hello:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.microsoftonline.com/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="hello access token is missing.",
```

#### Parametry chyby
| Parametr | Popis |
| --- | --- |
| authorization_uri |identifikátor URI (fyzické koncový bod) serveru ověřování hello Hello. Tato hodnota se také používá jako vyhledávací key tooget Další informace o serveru hello z koncového bodu zjišťování. <p><p> Hello klienta musíte ověřit, že hello serveru ověřování je důvěryhodný. Když hello prostředků je chráněný službou Azure AD, je dostatečná tooverify, který hello adresa URL začíná https://login.microsoftonline.com nebo jiný název hostitele, který podporuje Azure AD. Prostředek konkrétního klienta by měl vrátit vždy identifikátor URI autorizace konkrétního klienta. |
| error |K chybě kódu hodnota definovaná v části 5.2 hello [Framework autorizace OAuth 2.0](http://tools.ietf.org/html/rfc6749). |
| error_description |Podrobnější popis chyby hello. Tato zpráva není určen popisný toobe koncového uživatele. |
| ID_prostředku |Vrátí hello jedinečný identifikátor prostředku hello. Hello klientská aplikace můžete použít tento identifikátor jako hodnota hello hello `resource` parametr při požadavku na token pro prostředek hello. <p><p> Je důležité pro hello klienta aplikace tooverify tuto hodnotu, jinak škodlivý služba může být schopný tooinduce **zvýšení oprávnění** útoku <p><p> Hello Doporučená strategie pro zabránění útoku je tooverify, který hello `resource_id` odpovídá hello základ hello webové adresy URL rozhraní API, která přistupuje. Například pokud https://service.contoso.com/data se přistupuje, hello `resource_id` může být htttps://service.contoso.com/. klientská aplikace Hello musí odmítnout `resource_id` , nemá na začátku základní adresu URL hello pokud existuje id spolehlivé jiný způsob, jak tooverify hello. |

#### Kódy chyb nosiče schéma
Hello specifikaci RFC 6750 definuje hello následujícím chybám pro prostředky, které používají hlavičky WWW-Authenticate hello a schéma nosiče v odpovědi hello.

| Kód stavu HTTP | Kód chyby | Popis | Akce klienta |
| --- | --- | --- | --- |
| 400 |invalid_request |Hello požadavku není ve správném formátu. Například může být chybějící parametr nebo pomocí hello stejný parametr dvakrát. |Opravte chybu hello a opakujte žádost hello. Tento typ chyby provedeno pouze během vývoje a zjistit počáteční testování v. |
| 401 |invalid_token |Hello přístupový token chybí, je neplatný nebo je odvolané. Hello hodnota parametru error_description hello poskytuje další podrobnosti. |Ze serveru ověřování hello požádat o nový token. Pokud se nezdaří, nový token hello došlo k neočekávané chybě. Odeslat uživatelé toohello zpráva Chyba a opakujte po náhodné zpoždění. |
| 403 |insufficient_scope |Hello přístupový token neobsahuje hello zosobnění oprávnění požadované tooaccess hello prostředků. |Odešlete nového autorizace požadavku toohello autorizaci koncového bodu. Pokud hello odpovědi obsahuje parametr rozsahu hello, použijte hodnotu oboru hello hello požadavek toohello prostředku. |
| 403 |insufficient_access |Hello subjektu hello tokenu nemá oprávnění hello, které jsou požadované tooaccess hello prostředků. |Výzva hello uživatele toouse jiný účet nebo toorequest oprávnění toohello zadaný prostředek. |

## Aktualizace hello přístupové tokeny
Přístupové tokeny jsou krátkodobou a je třeba aktualizovat po vypršení jejich platnosti toocontinue přístupu k prostředkům. Můžete obnovit hello `access_token` odesláním jiné `POST` požadavku toohello `/token` koncový bod, ale tentokrát poskytování hello `refresh_token` místo hello `code`.

Aktualizujte tokeny nemají zadaný životnosti. Hello životnosti tokenů aktualizace jsou obvykle poměrně dlouho. Ale v některých případech tokeny obnovení vyprší, byly odvolány nebo nemají dostatečná oprávnění pro potřeby hello akci. Aplikace musí tooexpect a řešit chyby vrácené hello vystavování tokenů endpoint správně.

Jakmile se zobrazí odpověď se chyba tokenu aktualizace, zahodit hello aktuální obnovovací token a nové autorizační kód požadavku nebo přístupový token. Zejména při použití aktualizace tokenu v hello tok udělení autorizačního kódu, pokud se zobrazí odpověď se hello `interaction_required` nebo `invalid_grant` kódy chyb, zrušíte hello obnovovací token a požádejte o nový kód autorizace.

Toohello požadavku ukázkové **konkrétního klienta** koncový bod (můžete použít také hello **běžné** koncového bodu) tooget nový přístupový token pomocí tokenu obnovení vypadá takto:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

### Úspěšná odpověď
Úspěšné odpovědi tokenu bude vypadat jako:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```
| Parametr | Popis |
| --- | --- |
| token_type |Typ tokenu Hello. Hello podporována pouze hodnota je **nosiče**. |
| expires_in |Hello zbývající doba platnosti tokenu hello v sekundách. Typické hodnota je 3600 (jedna hodina). |
| expires_on |Hello datum a čas, kdy vyprší platnost tokenu hello. Datum Hello je reprezentován jako hello počet sekund od pod hodnotou 1970-01-01T0:0:0Z UTC dokud hello čas vypršení platnosti. |
| Prostředek |Identifikuje hello zabezpečené prostředků token přístupu hello může být použité tooaccess. |
| Obor |Udělení oprávnění zosobnění toohello nativní klientskou aplikaci. Hello výchozí oprávnění je **user_impersonation**. Vlastník Hello hello cílový prostředek můžete zaregistrovat alternativní hodnoty ve službě Azure AD. |
| access_token |Hello nový přístupový token, který byl požadován. |
| refresh_token |Nový refresh_token OAuth 2.0, který lze použít toorequest nové přístupové tokeny, když vyprší platnost hello, jeden v této odpovědi. |

### Chybové odpovědi
Ukázková chyba odpověď může vypadat například takto:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: hello application named https://foo.microsoft.com/mail.read was not found in hello tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if hello application has not been installed by hello administrator of hello tenant or consented tooby any user in hello tenant.  You might have sent your authentication request toohello wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, který lze použít tooclassify typů chyb, ke kterým došlo a může být použité tooreact tooerrors. |
| error_description |Konkrétní chybová zpráva, která může pomoci vývojář identifikovat hello hlavní příčinu chyby ověřování. |
| error_codes |Seznam tokenů zabezpečení specifické chybové kódy, které vám můžou pomoct při diagnostiky. |
| časové razítko |Hello čas, kdy došlo k chybě hello. |
| trace_id |Jedinečný identifikátor pro hello požadavek, který vám pomůže v diagnostice. |
| correlation_id |Jedinečný identifikátor pro hello požadavek, který vám pomůže v diagnostice mezi komponentami. |

Popis hello kódy chyb a hello Doporučená akce klienta najdete v tématu [kódy chyb pro koncový bod tokenu chyby](#error-codes-for-token-endpoint-errors).
