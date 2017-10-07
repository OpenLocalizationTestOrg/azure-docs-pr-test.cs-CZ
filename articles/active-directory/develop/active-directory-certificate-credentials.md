---
title: "přihlašovací údaje aaaCertificate ve službě Azure AD | Microsoft Docs"
description: "Tento článek popisuje hello registrace a používání certifikát přihlašovacích údajů pro ověřování aplikace"
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 3508803112ac06268d553db86ab74812aa53e455
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="certificate-credentials-for-application-authentication"></a>Přihlašovací údaje certifikátu pro ověřování aplikace

Azure Active Directory umožňuje aplikaci toouse svoje vlastní přihlašovací údaje pro ověřování, například v hello udělení pověření klienta OAuth 2.0 a tok On-Behalf-Of hello.
Jednu formu přihlašovacích údajů, které je možné je JSON webové Token(JWT) kontrolní výraz systému podepsané certifikátem, který aplikace hello vlastní.

## <a name="format-of-hello-assertion"></a>Formát hello assertion
kontrolní výraz hello toocompute, budete ho zřejmě chtít toouse mezi hello mnoho [webových tokenů JSON](https://jwt.io/) knihovny v hello jazyk podle vašeho výběru. Hello informace přenášené podle hello token je:

#### <a name="header"></a>Záhlaví

| Parametr |  Poznámka |
| --- | --- | --- |
| `alg` | Musí být **RS256** |
| `typ` | Musí být **JWT** |
| `x5t` | Musí být kryptografický otisk certifikátu X.509 SHA-1 hello |

#### <a name="claims-payload"></a>Deklarace identity (datovou část)

| Parametr |  Poznámka |
| --- | --- | --- |
| `aud` | Cílová skupina: By měla být  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token ** |
| `exp` | Datum vypršení platnosti: hello datum vypršení platnosti tokenu hello. čas Hello je reprezentován jako hello počet sekund od 1. ledna 1970 (pod hodnotou 1970-01-01T0:0:0Z) až do vypršení platnosti tokenu ověření hello hello času UTC.|
| `iss` | Vystavitel: by měla být client_id hello (Id aplikace hello klienta služby) |
| `jti` | Identifikátor GUID: hello JWT ID |
| `nbf` | Neplatný před: hello datum před které hello nelze použít token. čas Hello je reprezentován jako hello počet sekund od 1. ledna 1970 (pod hodnotou 1970-01-01T0:0:0Z) dokud byl vydán token hello hello čas UTC. |
| `sub` | Předmět: jako u `iss`, by měla být client_id hello (Id aplikace hello klienta služby) |

#### <a name="signature"></a>Podpis
podpis Hello je počítaný použití hello certifikátu, jak je popsáno v hello [JSON Web Token RFC7519 specifikace](https://tools.ietf.org/html/rfc7519)

### <a name="example-of-a-decoded-jwt-assertion"></a>Příklad dekódované assertion JWT
```
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,  
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"

```

### <a name="example-of-an-encoded-jwt-assertion"></a>Příklad kódovaného JWT kontrolní výrazy
Následující řetězec Hello je příkladem kódovaného kontrolní výraz. Pokud se podíváte pečlivě, jste si všimli tři části oddělený tečkami (.).
první část Hello kóduje hello záhlaví, hello druhý hello datové a hello je poslední hello podpis počítaný s certifikáty hello z hello obsah hello první dva oddíly.
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a>Zaregistrujte svůj certifikát s Azure AD
tooassociate přihlašovací údaje hello certifikát s hello klientská aplikace ve službě Azure AD, budete potřebovat manifest aplikace hello tooedit.
S podržení certifikát, je třeba toocompute:
- `$base64Thumbprint`, který je hello kódování base64 hello certifikátu Hash
- `$base64Value`, který je hello kódování base64 hello nezpracovaná data certifikátu

musíte taky tooprovide klíč hello tooidentify identifikátor GUID v manifestu aplikace hello (`$keyId`)

V hello registrace aplikace Azure pro hello klientskou aplikaci, otevřete manifest aplikace hello a nahraďte hello *keyCredentials* vlastnost s vaší nové informace o certifikátu pomocí hello následující schéma:
```
"keyCredentials": [
    {
        "customKeyIdentifier": "$base64Thumbprint",
        "keyId": "$keyid",
        "type": "AsymmetricX509Cert",
        "usage": "Verify",
        "value":  "$base64Value"
    }
]
```

Uložit manifest aplikace toohello hello úpravy a odeslat tooAzure AD. Vlastnost keyCredentials Hello je více hodnot, takže nahrajete více certifikátů pro širší správu klíčů.
