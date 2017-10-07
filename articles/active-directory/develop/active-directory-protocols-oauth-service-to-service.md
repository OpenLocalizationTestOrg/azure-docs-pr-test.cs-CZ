---
title: "Služba AD tooService Auth aaaAzure pomocí OAuth2.0 | Microsoft Docs"
description: "Tento článek popisuje, jak toouse HTTP zprávy tooimplement služby tooservice ověřování pomocí tok udělení přihlašovacích údajů klienta OAuth2.0 hello."
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: a7f939d9-532d-4b6d-b6d3-95520207965d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f4bfd4ea8a7de1929c7dcf7ad65a156edff74f71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Volání tooservice služby pomocí pověření klienta (sdílený tajný klíč nebo certifikát)
Hello toku OAuth 2.0 klienta pověření Grant umožňuje webové služby (*důvěrné klienta*) toouse svoje vlastní přihlašovací údaje namísto zosobňování uživatele, tooauthenticate při volání metody jiné webové služby. V tomto scénáři hello klienta je obvykle střední vrstvy webové služby, služba démon nebo webu. Pro vyšší úroveň záruky Azure AD umožňuje také hello volání služby toouse jako pověření certifikát (namísto sdílený tajný klíč).

## Vývojový diagram udělení pověření klienta
Hello následující diagram popisuje, jak udělit pověření klienta hello toku funguje v Azure Active Directory (Azure AD).

![OAuth2.0 tok udělení přihlašovacích údajů klienta](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. klientská aplikace Hello ověřuje koncový bod vystavování tokenů toohello Azure AD a požadavky přístupový token.
2. Hello Azure AD vystavování tokenů koncový bod problémy hello přístupový token.
3. Hello přístupový token je použité tooauthenticate toohello zabezpečené prostředků.
4. Data z hello zabezpečené prostředků se vrátí toohello webové aplikace.

## Zaregistrovat hello služby ve službě Azure AD
Zaregistrujte hello volání služby a hello přijetí služby v Azure Active Directory (Azure AD). Podrobné pokyny najdete v tématu [integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md).

## Žádost o Token přístupu
toorequest přístupový token, použijte koncový bod HTTP POST toohello konkrétního klienta Azure AD.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## Žádosti o token Service-to-service přístup
Jsou dva případy, v závislosti na tom, zda klientská aplikace hello zvolí toobe zabezpečeny sdílený tajný klíč nebo certifikát.

### Nejprve případ: žádosti o token přístupu s sdílený tajný klíč
Pokud používáte sdílený tajný klíč, obsahuje žádosti o token service-to-service přístup hello následující parametry:

| Parametr |  | Popis |
| --- | --- | --- |
| grant_type |Požadované |Určuje hello požadovaný typ udělení. V toku udělení pověření klienta, musí být hodnota hello **client_credentials**. |
| client_id |Požadované |Určuje id klienta Azure AD hello hello volání webové služby. volání ID klienta aplikace, v hello hello toofind [portál Azure](https://portal.azure.com), klikněte na tlačítko **služby Active Directory**, přepínač adresář, klikněte na aplikace hello. Hello client_id je hello *ID aplikace* |
| tajný klíč client_secret |Požadované |Zadejte klíč zaregistrovat pro hello volání webové služby nebo proces démon aplikace ve službě Azure AD. Klikněte na klíč, v hello portál Azure, toocreate **služby Active Directory**, přepínač adresář, klikněte na tlačítko hello aplikace, klikněte na **nastavení**, klikněte na tlačítko **klíče**, a přidejte klíč.|
| Prostředek |Požadované |Zadejte hello identifikátor ID URI aplikace z hello přijetí webové služby. toofind hello identifikátor ID URI aplikace v hello portál Azure, klikněte na tlačítko **služby Active Directory**, přepínač adresář, klikněte hello aplikace služby a pak klikněte na tlačítko **nastavení** a **vlastnosti** |

#### Příklad
Hello následující HTTP POST požadavků přístupový token pro hello https://service.contoso.com/ webovou službu. Hello `client_id` identifikuje hello webová služba, která požaduje hello přístupový token.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### Druhé případ: tokenu žádosti o přístup pomocí certifikátu
Žádosti o token service-to-service přístup pomocí certifikátu obsahuje hello následující parametry:

| Parametr |  | Popis |
| --- | --- | --- |
| grant_type |Požadované |Určuje hello požadovaný typ odpovědi. V toku udělení pověření klienta, musí být hodnota hello **client_credentials**. |
| client_id |Požadované |Určuje id klienta Azure AD hello hello volání webové služby. volání ID klienta aplikace, v hello hello toofind [portál Azure](https://portal.azure.com), klikněte na tlačítko **služby Active Directory**, přepínač adresář, klikněte na aplikace hello. Hello client_id je hello *ID aplikace* |
| client_assertion_type |Požadované |musí být hodnota Hello`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Požadované | Kontrolní výrazy (webového tokenu JSON), je nutné toocreate a přihlášení s hello certifikátů můžete zaregistrovat jako přihlašovací údaje pro vaši aplikaci. Přečtěte si informace o [certifikát přihlašovacích údajů](active-directory-certificate-credentials.md) toolearn jak tooregister váš certifikát a hello formát hello assertion.|
| Prostředek | Požadované |Zadejte hello identifikátor ID URI aplikace z hello přijetí webové služby. Klikněte na tlačítko toofind hello identifikátor ID URI aplikace v portálu Azure, hello **služby Active Directory**, klikněte na adresář hello, klikněte na tlačítko aplikace hello a pak klikněte na tlačítko **konfigurace**. |

Všimněte si, že jsou parametry hello téměř hello stejné jako v případě hello hello požadavku podle sdílený tajný klíč s tím rozdílem, že parametr tajný klíč client_secret hello je nahrazena dva parametry: client_assertion_type a client_assertion.

#### Příklad
Hello následující HTTP POST požadavků přístupový token pro hello https://service.contoso.com/ webové služby pomocí certifikátu. Hello `client_id` identifikuje hello webová služba, která požaduje hello přístupový token.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### Odpovědi tokenu přístupu Service-to-Service

Úspěšná odpověď obsahuje odpověď JSON OAuth 2.0 s hello následující parametry:

| Parametr | Popis |
| --- | --- |
| access_token |Hello požadovaný přístupový token. Hello volání webové služby můžete použít tento token tooauthenticate toohello přijetí webové služby. |
| token_type |Určuje hodnotu hello typ tokenu. Hello pouze typ, který podporuje Azure AD je **nosiče**. Další informace o nosné tokeny, najdete v části hello [Framework autorizace OAuth 2.0: použití tokenů nosiče (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| expires_in |Jak dlouho hello přístupový token je platný (v sekundách). |
| expires_on |Hello čas, kdy vyprší platnost hello přístupový token. Datum Hello je reprezentován jako hello počet sekund od pod hodnotou 1970-01-01T0:0:0Z UTC dokud hello čas vypršení platnosti. Tato hodnota je použité toodetermine hello Životnost mezipaměti tokenů. |
| not_before |Hello čase, ze které hello začne přístupový token použitelné. Datum Hello je reprezentován jako hello počet sekund od pod hodnotou 1970-01-01T0:0:0Z UTC dokud čas platnosti tokenu hello.|
| Prostředek |Hello identifikátor ID URI aplikace z hello přijetí webové služby. |

#### Příklad odpovědi
Hello následující příklad ukazuje tooa žádost o úspěšné odpovědi pro webové služby tooa tokenu přístupu.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## Viz také
* [OAuth 2.0 ve službě Azure AD](active-directory-protocols-oauth-code.md)
* [Ukázka v jazyce C# tooservice volání služby hello s sdílený tajný klíč](https://github.com/Azure-Samples/active-directory-dotnet-daemon) a [ukázka v jazyce C# volání tooservice hello služby pomocí certifikátu](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)
