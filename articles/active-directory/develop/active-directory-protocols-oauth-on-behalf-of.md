---
title: "aaaAzure AD služby tooservice ověřování pomocí OAuth2.0 On-Behalf-Of koncept specifikace | Microsoft Docs"
description: "Tento článek popisuje, jak toouse HTTP zprávy tooimplement služby tooservice ověřování pomocí hello OAuth2.0 tok On-Behalf-Of."
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 55b7fcfe6c0223bddedd8d8fa2defcb5769b43c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Služba tooservice volá pomocí identity delegovaný uživatel v hello tok On-Behalf-Of
Hello toku OAuth 2.0 On-Behalf-Of slouží hello případ použití kde aplikace volá služby nebo webové rozhraní API, který se pak musí toocall jiná služba nebo webové rozhraní API. Rada Hello je, že toopropagate hello delegovaná oprávnění prostřednictvím hello řetězce požadavků a identity uživatele. Pro hello střední vrstvy toomake ověření požadavků toohello podřízené služby musí toosecure přístupový token ze služby Azure Active Directory (Azure AD) jménem uživatele hello.

## On-Behalf-Of vývojový diagram
Předpokládejme, tento uživatel hello byl ověřen na aplikaci pomocí hello [tok poskytování autorizačních kódů OAuth 2.0](active-directory-protocols-oauth-code.md). V tomto okamžiku aplikace hello má přístupový token (token A) s hello uživatele deklarace identity a souhlasu tooaccess hello střední vrstvu webového rozhraní API (rozhraní API A). Rozhraní API A potřebuje teď, toomake ověřeného požadavku toohello podřízené webové rozhraní API (API B).

Hello kroky, které následují tvoří hello tok On-Behalf-Of a jsou vysvětleny hello pomoci hello následující diagram.

![OAuth2.0 tok On-Behalf-Of](media/active-directory-protocols-oauth-on-behalf-of/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. klienta aplikace Hello zadá požadavek tooAPI A s hello tokenu A.
2. Rozhraní API A ověřuje koncový bod vystavování tokenů toohello Azure AD a požadavky tokenu tooaccess rozhraní API B.
3. koncový bod vystavování tokenů Hello Azure AD ověřuje přihlašovací údaje A rozhraní API se token a problémy hello přístupového tokenu pro rozhraní API B (token B).
4. Hello token B je nastavena v záhlaví autorizace hello hello požadavek tooAPI B.
5. Vrátí data z hello zabezpečené prostředků API B.

## Registrovat hello aplikace a služby ve službě Azure AD
Hello klientská aplikace a služby střední vrstvy hello zaregistrujte ve službě Azure AD.
### Registrace služby střední vrstvy hello
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello, klikněte na tlačítko na vašem účtu a v části hello **Directory** vyberte klienta hello služby Active Directory, kde chcete tooregister vaší aplikace.
3. Klikněte na **více služeb** v hello navigaci vlevo a zvolte **Azure Active Directory**.
4. Klikněte na **registrace aplikace** a zvolte **nové registrace aplikace**.
5. Zadejte popisný název aplikace hello a vyberte typ aplikace hello. Založený na typu aplikace hello hello přihlašování adresu URL sady nebo přesměrování URL toohello základní adresa URL. Klikněte na **vytvořit** toocreate hello aplikace.
6. Při stále v hello portálu Azure, vyberte aplikaci a klikněte na **nastavení**. Z nabídky nastavení hello, zvolte **klíče** a přidejte klíč – vyberte dobu trvání klíče 1 rok nebo 2 roky. Při ukládání této stránky, hello hodnota klíče se zobrazí, zkopírujte a uložte do bezpečného umístění hello hodnotu – budete potřebovat tento klíč novější tooconfigure hello nastavení aplikace v implementaci - touto hodnotou klíče nebude znovu zobrazit, ani se dá načíst pomocí libovolné jiným způsobem, proto prosím záznam ho hned, jak je vidět hello portálu Azure.

### Registrace hello klientské aplikace
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello, klikněte na tlačítko na vašem účtu a v části hello **Directory** vyberte klienta hello služby Active Directory, kde chcete tooregister vaší aplikace.
3. Klikněte na **více služeb** v hello navigaci vlevo a zvolte **Azure Active Directory**.
4. Klikněte na **registrace aplikace** a zvolte **nové registrace aplikace**.
5. Zadejte popisný název aplikace hello a vyberte typ aplikace hello. Založený na typu aplikace hello hello přihlašování adresu URL sady nebo přesměrování URL toohello základní adresa URL. Klikněte na **vytvořit** toocreate hello aplikace.
6. Konfigurace oprávnění pro vaši aplikaci – v nabídce nastavení hello, zvolte hello **požadovaná oprávnění** části, klikněte na **přidat**, pak **vybrat rozhraní API**a typ hello název služby střední vrstvy hello v textovém poli hello. Potom klikněte na **vyberte oprávnění** a vyberte možnost ' přístup *název služby*'.

### Konfigurace známé klientské aplikace
V tomto scénáři má služby střední vrstvy hello žádné uživatelské interakce tooobtain hello uživatele souhlas tooaccess hello podřízené API. Proto hello možnost toogrant přístup toohello podřízené API musí být uvedené předem jako součást kroku souhlasu hello během ověřování.
tooachieve tento, postupujte podle hello kroků tooexplicitly vazby hello klientské aplikace registrace ve službě Azure AD s registrací hello hello střední vrstvy služby, která sloučí hello souhlas potřebný hello klienta a střední vrstvy do jednoho dialogové okno.
1. Přejděte registraci toohello střední vrstvy služby a klikněte na **Manifest** tooopen hello manifestu editor.
2. V manifestu hello najít hello `knownClientApplications` pole vlastnosti, a přidejte hello ID klienta aplikace hello klienta jako element.
3. Kliknutím na tlačítko Uložit hello uložte hello manifest.

## Žádost o služby tooservice přístup tokenu
toorequest přístupový token, ujistěte se, koncový bod HTTP POST toohello konkrétního klienta Azure AD s hello následující parametry.

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```
Jsou dva případy, v závislosti na tom, zda klientská aplikace hello zvolí toobe zabezpečeny sdílený tajný klíč nebo certifikát.

### Nejprve případ: žádosti o token přístupu s sdílený tajný klíč
Pokud používáte sdílený tajný klíč, obsahuje žádosti o token service-to-service přístup hello následující parametry:

| Parametr |  | Popis |
| --- | --- | --- |
| grant_type |Požadované | Typ Hello hello žádosti o token. Pro žádost o pomocí token JWT, musí být hodnota hello **urn: ietf:params:oauth:grant – typ: jwt-nosiče**. |
| Kontrolní výraz |Požadované | Hodnota Hello hello token používaný v žádosti o hello. |
| client_id |Požadované | Hello ID aplikace přiřazené toohello volání služby během registrace s Azure AD. Klikněte na tlačítko toofind hello ID aplikace v portálu pro správu Azure, hello **služby Active Directory**, klikněte na tlačítko hello adresář a potom klikněte na název aplikace hello. |
| tajný klíč client_secret |Požadované | klíč Hello zaregistrovaný pro hello volání služby ve službě Azure AD. Tato hodnota je měli poznamenat hello během registrace. |
| Prostředek |Požadované | Hello identifikátor ID URI aplikace z hello přijetí služby (zabezpečené prostředků). Klikněte na tlačítko toofind hello identifikátor ID URI aplikace v hello portálu pro správu Azure, **služby Active Directory**, klikněte na tlačítko hello adresář, klikněte na název aplikace hello, klikněte na **všechna nastavení** a pak klikněte na **vlastnosti** . |
| requested_token_use |Požadované | Určuje, jak by měla být hello požadavek zpracovat. V hello tok On-Behalf-Of, musí být hodnota hello **on_behalf_of**. |
| Obor |Požadované | Mezeru oddělený seznam obory pro žádosti o token hello. Pro OpenID Connect, hello oboru **openid** musí být zadán.|

#### Příklad
Hello následující HTTP POST požadavků přístupový token pro hello https://graph.windows.net webové rozhraní API. Hello `client_id` identifikuje hello služba, která požaduje hello přístupový token.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_secret=0Y1W%2BY3yYb3d9N8vSjvm8WrGzVZaAaHbHHcGbcgG%2BoI%3D
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

### Druhé případ: tokenu žádosti o přístup pomocí certifikátu
Žádosti o token service-to-service přístup pomocí certifikátu obsahuje hello následující parametry:

| Parametr |  | Popis |
| --- | --- | --- |
| grant_type |Požadované | Typ Hello hello žádosti o token. Pro žádost o pomocí token JWT, musí být hodnota hello **urn: ietf:params:oauth:grant – typ: jwt-nosiče**. |
| Kontrolní výraz |Požadované | Hodnota Hello hello token používaný v žádosti o hello. |
| client_id |Požadované | Hello ID aplikace přiřazené toohello volání služby během registrace s Azure AD. Klikněte na tlačítko toofind hello ID aplikace v portálu pro správu Azure, hello **služby Active Directory**, klikněte na tlačítko hello adresář a potom klikněte na název aplikace hello. |
| client_assertion_type |Požadované |musí být hodnota Hello`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Požadované | Kontrolní výrazy (webového tokenu JSON), je nutné toocreate a přihlášení s hello certifikátů můžete zaregistrovat jako přihlašovací údaje pro vaši aplikaci.  Přečtěte si informace o [certifikát přihlašovacích údajů](active-directory-certificate-credentials.md) toolearn jak tooregister váš certifikát a hello formát hello assertion.|
| Prostředek |Požadované | Hello identifikátor ID URI aplikace z hello přijetí služby (zabezpečené prostředků). Klikněte na tlačítko toofind hello identifikátor ID URI aplikace v hello portálu pro správu Azure, **služby Active Directory**, klikněte na tlačítko hello adresář, klikněte na název aplikace hello, klikněte na **všechna nastavení** a pak klikněte na **vlastnosti** . |
| requested_token_use |Požadované | Určuje, jak by měla být hello požadavek zpracovat. V hello tok On-Behalf-Of, musí být hodnota hello **on_behalf_of**. |
| Obor |Požadované | Mezeru oddělený seznam obory pro žádosti o token hello. Pro OpenID Connect, hello oboru **openid** musí být zadán.|

Všimněte si, že jsou parametry hello téměř hello stejné jako v případě hello hello požadavku podle sdílený tajný klíč s tím rozdílem, že parametr tajný klíč client_secret hello je nahrazena dva parametry: client_assertion_type a client_assertion.

#### Příklad
Hello následující HTTP POST požadavků přístupový token pro hello https://graph.windows.net webové rozhraní API s certifikátem. Hello `client_id` identifikuje hello služba, která požaduje hello přístupový token.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

## Služba tooservice odpovědi tokenu přístupu
Úspěšná odpověď je odpověď JSON OAuth 2.0 se hello následující parametry.

| Parametr | Popis |
| --- | --- |
| token_type |Určuje hodnotu hello typ tokenu. Hello pouze typ, který podporuje Azure AD je **nosiče**. Další informace o nosné tokeny, najdete v části hello [Framework autorizace OAuth 2.0: použití tokenů nosiče (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| Obor |rozsah Hello udělení v hello tokenu přístupu. |
| expires_in |Délka Hello čas hello přístupový token je platný (v sekundách). |
| expires_on |Hello čas, kdy vyprší platnost hello přístupový token. Datum Hello je reprezentován jako hello počet sekund od pod hodnotou 1970-01-01T0:0:0Z UTC dokud hello čas vypršení platnosti. Tato hodnota je použité toodetermine hello Životnost mezipaměti tokenů. |
| Prostředek |Hello identifikátor ID URI aplikace z hello přijetí služby (zabezpečené prostředků). |
| access_token |Hello požadovaný přístupový token. Hello volání služby můžete použít tento token tooauthenticate toohello přijímající služby. |
| požadavku id_token |token požadovanému id Hello. Hello volání služby můžete použít identita uživatele tooverify hello a zahájit relaci s hello uživatele. |
| refresh_token |Hello obnovovací token pro hello požadovaný přístupový token. Hello volání služby můžete použít tento token toorequest dalšího přístupového tokenu, po vypršení platnosti hello aktuální přístupový token. |

### Příklad úspěšné odpovědi
Hello následující příklad ukazuje úspěšné odpovědi tooa žádost o token přístupu pro hello https://graph.windows.net webové rozhraní API.

```
{
    "token_type":"Bearer",
    "scope":"User.Read",
    "expires_in":"43482",
    "ext_expires_in":"302683",
    "expires_on":"1493466951",
    "not_before":"1493423168",
    "resource":"https://graph.windows.net",
    "access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ",
    "refresh_token":"AQABAAAAAABnfiG-mA6NTae7CdWW7QfdjKGu9-t1scy_TDEmLi4eLQMjJGt_nAoVu6A4oSu1KsRiz8XyQIPKQxSGfbf2FoSK-hm2K8TYzbJuswYusQpJaHUQnSqEvdaCeFuqXHBv84wjFhuanzF9dQZB_Ng5za9xKlUENrNtlq9XuLNVKzxEyeUM7JyxzdY7JiEphWImwgOYf6II316d0Z6-H3oYsFezf4Xsjz-MOBYEov0P64UaB5nJMvDyApV-NWpgklLASfNoSPGb67Bc02aFRZrm4kLk-xTl6eKE6hSo0XU2z2t70stFJDxvNQobnvNHrAmBaHWPAcC3FGwFnBOojpZB2tzG1gLEbmdROVDp8kHEYAwnRK947Py12fJNKExUdN0njmXrKxNZ_fEM33LHW1Tf4kMX_GvNmbWHtBnIyG0w5emb-b54ef5AwV5_tGUeivTCCysgucEc-S7G8Cz0xNJ_BOiM_4bAv9iFmrm9STkltpz0-Tftg8WKmaJiC0xXj6uTf4ZkX79mJJIuuM7XP4ARIcLpkktyg2Iym9jcZqymRkGH2Rm9sxBwC4eeZXM7M5a7TJ-5CqOdfuE3sBPq40RdEWMFLcrAzFvP0VDR8NKHIrPR1AcUruat9DETmTNJukdlJN3O41nWdZOVoJM-uKN3uz2wQ2Ld1z0Mb9_6YfMox9KTJNzRzcL52r4V_y3kB6ekaOZ9wQ3HxGBQ4zFt-2U0mSszIAA",
    "id_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8yNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IvIiwiaWF0IjoxNDkzNDIzMTY4LCJuYmYiOjE0OTM0MjMxNjgsImV4cCI6MTQ5MzQ2Njk1MSwiYW1yIjpbInB3ZCJdLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXRpIjoieEN3ZnpoYS1QMFdKUU9MeENHZ0tBQSIsInZlciI6IjEuMCJ9."
}
```

### Příklad chybové odpovědi
Chybnou odpověď vrátí koncový bod tokenu Azure AD při pokusu o tooacquire přístupový token pro příjem dat API hello, pokud má zásady podmíněného přístupu, jako je vícefaktorové ověřování u něho nastavený hello podřízené rozhraní API. Hello střední vrstvy služby by měl surface této chyby toohello klientské aplikace, tak, aby aplikace hello klienta může poskytovat hello uživatelské interakce toosatisfy hello zásady podmíněného přístupu.

```
{
    "error":"interaction_required",
    "error_description":"AADSTS50079: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must enroll in multi-factor authentication tooaccess 'bf8d80f9-9098-4972-b203-500f535113b1'.\r\nTrace ID: b72a68c3-0926-4b8e-bc35-3150069c2800\r\nCorrelation ID: 73d656cf-54b1-4eb2-b429-26d8165a52d7\r\nTimestamp: 2017-05-01 22:43:20Z",
    "error_codes":[50079],
    "timestamp":"2017-05-01 22:43:20Z",
    "trace_id":"b72a68c3-0926-4b8e-bc35-3150069c2800",
    "correlation_id":"73d656cf-54b1-4eb2-b429-26d8165a52d7",
    "claims":"{\"access_token\":{\"polids\":{\"essential\":true,\"values\":[\"9ab03e19-ed42-4168-b6b7-7001fb3e933a\"]}}}"
}
```

## Použití hello přístup tokenu tooaccess hello zabezpečené prostředků
Nyní hello střední vrstvy služby pomocí hello tokenu získal výše toomake ověření požadavků toohello po proudu webové rozhraní API, podle nastavení hello token v hello `Authorization` záhlaví.

### Příklad
```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```

## Další kroky
Další informace o protokolu OAuth 2.0 hello a jiný způsob tooperform služby tooservice ověřování pomocí přihlašovacích údajů klienta.
* [Ověřování služby tooservice pomocí udělení pověření klienta OAuth 2.0 ve službě Azure AD](active-directory-protocols-oauth-service-to-service.md)
* [OAuth 2.0 ve službě Azure AD](active-directory-protocols-oauth-code.md)
