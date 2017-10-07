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
# <a name="certificate-credentials-for-application-authentication"></a><span data-ttu-id="8229c-103">Přihlašovací údaje certifikátu pro ověřování aplikace</span><span class="sxs-lookup"><span data-stu-id="8229c-103">Certificate credentials for application authentication</span></span>

<span data-ttu-id="8229c-104">Azure Active Directory umožňuje aplikaci toouse svoje vlastní přihlašovací údaje pro ověřování, například v hello udělení pověření klienta OAuth 2.0 a tok On-Behalf-Of hello.</span><span class="sxs-lookup"><span data-stu-id="8229c-104">Azure Active Directory allows an application toouse its own credentials for authentication, for example, in hello OAuth 2.0 Client Credentials Grant flow and hello On-Behalf-Of flow.</span></span>
<span data-ttu-id="8229c-105">Jednu formu přihlašovacích údajů, které je možné je JSON webové Token(JWT) kontrolní výraz systému podepsané certifikátem, který aplikace hello vlastní.</span><span class="sxs-lookup"><span data-stu-id="8229c-105">One form of credential that can be used is a JSON Web Token(JWT) assertion signed with a certificate that hello application owns.</span></span>

## <a name="format-of-hello-assertion"></a><span data-ttu-id="8229c-106">Formát hello assertion</span><span class="sxs-lookup"><span data-stu-id="8229c-106">Format of hello assertion</span></span>
<span data-ttu-id="8229c-107">kontrolní výraz hello toocompute, budete ho zřejmě chtít toouse mezi hello mnoho [webových tokenů JSON](https://jwt.io/) knihovny v hello jazyk podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="8229c-107">toocompute hello assertion, you probably want toouse one of hello many [JSON Web Token](https://jwt.io/) libraries in hello language of your choice.</span></span> <span data-ttu-id="8229c-108">Hello informace přenášené podle hello token je:</span><span class="sxs-lookup"><span data-stu-id="8229c-108">hello information carried by hello token is:</span></span>

#### <a name="header"></a><span data-ttu-id="8229c-109">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="8229c-109">Header</span></span>

| <span data-ttu-id="8229c-110">Parametr</span><span class="sxs-lookup"><span data-stu-id="8229c-110">Parameter</span></span> |  <span data-ttu-id="8229c-111">Poznámka</span><span class="sxs-lookup"><span data-stu-id="8229c-111">Remark</span></span> |
| --- | --- | --- |
| `alg` | <span data-ttu-id="8229c-112">Musí být **RS256**</span><span class="sxs-lookup"><span data-stu-id="8229c-112">Should be **RS256**</span></span> |
| `typ` | <span data-ttu-id="8229c-113">Musí být **JWT**</span><span class="sxs-lookup"><span data-stu-id="8229c-113">Should be **JWT**</span></span> |
| `x5t` | <span data-ttu-id="8229c-114">Musí být kryptografický otisk certifikátu X.509 SHA-1 hello</span><span class="sxs-lookup"><span data-stu-id="8229c-114">Should be hello X.509 Certificate SHA-1 thumbprint</span></span> |

#### <a name="claims-payload"></a><span data-ttu-id="8229c-115">Deklarace identity (datovou část)</span><span class="sxs-lookup"><span data-stu-id="8229c-115">Claims (Payload)</span></span>

| <span data-ttu-id="8229c-116">Parametr</span><span class="sxs-lookup"><span data-stu-id="8229c-116">Parameter</span></span> |  <span data-ttu-id="8229c-117">Poznámka</span><span class="sxs-lookup"><span data-stu-id="8229c-117">Remark</span></span> |
| --- | --- | --- |
| `aud` | <span data-ttu-id="8229c-118">Cílová skupina: By měla být  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token **</span><span class="sxs-lookup"><span data-stu-id="8229c-118">Audience: Should be **https://login.microsoftonline.com/*tenant_Id*/oauth2/token**</span></span> |
| `exp` | <span data-ttu-id="8229c-119">Datum vypršení platnosti: hello datum vypršení platnosti tokenu hello.</span><span class="sxs-lookup"><span data-stu-id="8229c-119">Expiration date: hello date when hello token expires.</span></span> <span data-ttu-id="8229c-120">čas Hello je reprezentován jako hello počet sekund od 1. ledna 1970 (pod hodnotou 1970-01-01T0:0:0Z) až do vypršení platnosti tokenu ověření hello hello času UTC.</span><span class="sxs-lookup"><span data-stu-id="8229c-120">hello time is represented as hello number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until hello time hello token validity expires.</span></span>|
| `iss` | <span data-ttu-id="8229c-121">Vystavitel: by měla být client_id hello (Id aplikace hello klienta služby)</span><span class="sxs-lookup"><span data-stu-id="8229c-121">Issuer: should be hello client_id (Application Id of hello client service)</span></span> |
| `jti` | <span data-ttu-id="8229c-122">Identifikátor GUID: hello JWT ID</span><span class="sxs-lookup"><span data-stu-id="8229c-122">GUID: hello JWT ID</span></span> |
| `nbf` | <span data-ttu-id="8229c-123">Neplatný před: hello datum před které hello nelze použít token.</span><span class="sxs-lookup"><span data-stu-id="8229c-123">Not Before: hello date before which hello token cannot be used.</span></span> <span data-ttu-id="8229c-124">čas Hello je reprezentován jako hello počet sekund od 1. ledna 1970 (pod hodnotou 1970-01-01T0:0:0Z) dokud byl vydán token hello hello čas UTC.</span><span class="sxs-lookup"><span data-stu-id="8229c-124">hello time is represented as hello number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until hello time hello token was issued.</span></span> |
| `sub` | <span data-ttu-id="8229c-125">Předmět: jako u `iss`, by měla být client_id hello (Id aplikace hello klienta služby)</span><span class="sxs-lookup"><span data-stu-id="8229c-125">Subject: As for `iss`, should be hello client_id (Application Id of hello client service)</span></span> |

#### <a name="signature"></a><span data-ttu-id="8229c-126">Podpis</span><span class="sxs-lookup"><span data-stu-id="8229c-126">Signature</span></span>
<span data-ttu-id="8229c-127">podpis Hello je počítaný použití hello certifikátu, jak je popsáno v hello [JSON Web Token RFC7519 specifikace](https://tools.ietf.org/html/rfc7519)</span><span class="sxs-lookup"><span data-stu-id="8229c-127">hello signature is computed applying hello certificate as described in hello [JSON Web Token RFC7519 specification](https://tools.ietf.org/html/rfc7519)</span></span>

### <a name="example-of-a-decoded-jwt-assertion"></a><span data-ttu-id="8229c-128">Příklad dekódované assertion JWT</span><span class="sxs-lookup"><span data-stu-id="8229c-128">Example of a decoded JWT assertion</span></span>
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

### <a name="example-of-an-encoded-jwt-assertion"></a><span data-ttu-id="8229c-129">Příklad kódovaného JWT kontrolní výrazy</span><span class="sxs-lookup"><span data-stu-id="8229c-129">Example of an encoded JWT assertion</span></span>
<span data-ttu-id="8229c-130">Následující řetězec Hello je příkladem kódovaného kontrolní výraz.</span><span class="sxs-lookup"><span data-stu-id="8229c-130">hello following string is an example of encoded assertion.</span></span> <span data-ttu-id="8229c-131">Pokud se podíváte pečlivě, jste si všimli tři části oddělený tečkami (.).</span><span class="sxs-lookup"><span data-stu-id="8229c-131">If you look carefully, you notice three sections separated by dots (.).</span></span>
<span data-ttu-id="8229c-132">první část Hello kóduje hello záhlaví, hello druhý hello datové a hello je poslední hello podpis počítaný s certifikáty hello z hello obsah hello první dva oddíly.</span><span class="sxs-lookup"><span data-stu-id="8229c-132">hello first section encodes hello header, hello second hello payload, and hello last is hello signature computed with hello certificates from hello content of hello first two sections.</span></span>
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a><span data-ttu-id="8229c-133">Zaregistrujte svůj certifikát s Azure AD</span><span class="sxs-lookup"><span data-stu-id="8229c-133">Register your certificate with Azure AD</span></span>
<span data-ttu-id="8229c-134">tooassociate přihlašovací údaje hello certifikát s hello klientská aplikace ve službě Azure AD, budete potřebovat manifest aplikace hello tooedit.</span><span class="sxs-lookup"><span data-stu-id="8229c-134">tooassociate hello certificate credential with hello client application in Azure AD, you need tooedit hello application manifest.</span></span>
<span data-ttu-id="8229c-135">S podržení certifikát, je třeba toocompute:</span><span class="sxs-lookup"><span data-stu-id="8229c-135">Having hold of a certificate, you need toocompute:</span></span>
- <span data-ttu-id="8229c-136">`$base64Thumbprint`, který je hello kódování base64 hello certifikátu Hash</span><span class="sxs-lookup"><span data-stu-id="8229c-136">`$base64Thumbprint`, which is hello base64 encoding of hello certificate Hash</span></span>
- <span data-ttu-id="8229c-137">`$base64Value`, který je hello kódování base64 hello nezpracovaná data certifikátu</span><span class="sxs-lookup"><span data-stu-id="8229c-137">`$base64Value`, which is hello base64 encoding of hello certificate raw data</span></span>

<span data-ttu-id="8229c-138">musíte taky tooprovide klíč hello tooidentify identifikátor GUID v manifestu aplikace hello (`$keyId`)</span><span class="sxs-lookup"><span data-stu-id="8229c-138">you also need tooprovide a GUID tooidentify hello key in hello application manifest (`$keyId`)</span></span>

<span data-ttu-id="8229c-139">V hello registrace aplikace Azure pro hello klientskou aplikaci, otevřete manifest aplikace hello a nahraďte hello *keyCredentials* vlastnost s vaší nové informace o certifikátu pomocí hello následující schéma:</span><span class="sxs-lookup"><span data-stu-id="8229c-139">In hello Azure app registration for hello client application, open hello application manifest, and replace hello *keyCredentials* property with your new certificate information using hello following schema:</span></span>
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

<span data-ttu-id="8229c-140">Uložit manifest aplikace toohello hello úpravy a odeslat tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="8229c-140">Save hello edits toohello application manifest, and upload tooAzure AD.</span></span> <span data-ttu-id="8229c-141">Vlastnost keyCredentials Hello je více hodnot, takže nahrajete více certifikátů pro širší správu klíčů.</span><span class="sxs-lookup"><span data-stu-id="8229c-141">hello keyCredentials property is multi-valued, so you may upload multiple certificates for richer key management.</span></span>
