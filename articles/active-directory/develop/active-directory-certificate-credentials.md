---
title: "Certifikát přihlašovacích údajů ve službě Azure AD | Microsoft Docs"
description: "Tento článek popisuje registrace a používání certifikát přihlašovacích údajů pro ověřování aplikace"
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
ms.openlocfilehash: 08bb5140bb35bbd120aaa506afeab8ad247f81e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="certificate-credentials-for-application-authentication"></a><span data-ttu-id="350c6-103">Přihlašovací údaje certifikátu pro ověřování aplikace</span><span class="sxs-lookup"><span data-stu-id="350c6-103">Certificate credentials for application authentication</span></span>

<span data-ttu-id="350c6-104">Azure Active Directory umožňuje aplikacím používat svoje vlastní přihlašovací údaje pro ověřování, například v toku udělení pověření klienta OAuth 2.0 a tok On-Behalf-Of.</span><span class="sxs-lookup"><span data-stu-id="350c6-104">Azure Active Directory allows an application to use its own credentials for authentication, for example, in the OAuth 2.0 Client Credentials Grant flow and the On-Behalf-Of flow.</span></span>
<span data-ttu-id="350c6-105">Jednu formu přihlašovacích údajů, které je možné je Token(JWT) webové JSON kontrolní výraz systému podepsané certifikátem, který vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="350c6-105">One form of credential that can be used is a JSON Web Token(JWT) assertion signed with a certificate that the application owns.</span></span>

## <a name="format-of-the-assertion"></a><span data-ttu-id="350c6-106">Formát kontrolní výraz</span><span class="sxs-lookup"><span data-stu-id="350c6-106">Format of the assertion</span></span>
<span data-ttu-id="350c6-107">Vypočítat kontrolní výraz, budete pravděpodobně chtít použít jednu z dalších [webových tokenů JSON](https://jwt.io/) knihovny v jazyce podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="350c6-107">To compute the assertion, you probably want to use one of the many [JSON Web Token](https://jwt.io/) libraries in the language of your choice.</span></span> <span data-ttu-id="350c6-108">Informace, které token je:</span><span class="sxs-lookup"><span data-stu-id="350c6-108">The information carried by the token is:</span></span>

#### <a name="header"></a><span data-ttu-id="350c6-109">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="350c6-109">Header</span></span>

| <span data-ttu-id="350c6-110">Parametr</span><span class="sxs-lookup"><span data-stu-id="350c6-110">Parameter</span></span> |  <span data-ttu-id="350c6-111">Poznámka</span><span class="sxs-lookup"><span data-stu-id="350c6-111">Remark</span></span> |
| --- | --- | --- |
| `alg` | <span data-ttu-id="350c6-112">Musí být **RS256**</span><span class="sxs-lookup"><span data-stu-id="350c6-112">Should be **RS256**</span></span> |
| `typ` | <span data-ttu-id="350c6-113">Musí být **JWT**</span><span class="sxs-lookup"><span data-stu-id="350c6-113">Should be **JWT**</span></span> |
| `x5t` | <span data-ttu-id="350c6-114">Musí být kryptografický otisk certifikátu X.509 SHA-1</span><span class="sxs-lookup"><span data-stu-id="350c6-114">Should be the X.509 Certificate SHA-1 thumbprint</span></span> |

#### <a name="claims-payload"></a><span data-ttu-id="350c6-115">Deklarace identity (datovou část)</span><span class="sxs-lookup"><span data-stu-id="350c6-115">Claims (Payload)</span></span>

| <span data-ttu-id="350c6-116">Parametr</span><span class="sxs-lookup"><span data-stu-id="350c6-116">Parameter</span></span> |  <span data-ttu-id="350c6-117">Poznámka</span><span class="sxs-lookup"><span data-stu-id="350c6-117">Remark</span></span> |
| --- | --- | --- |
| `aud` | <span data-ttu-id="350c6-118">Cílová skupina: By měla být  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token **</span><span class="sxs-lookup"><span data-stu-id="350c6-118">Audience: Should be **https://login.microsoftonline.com/*tenant_Id*/oauth2/token**</span></span> |
| `exp` | <span data-ttu-id="350c6-119">Datum vypršení platnosti: datum vypršení platnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="350c6-119">Expiration date: the date when the token expires.</span></span> <span data-ttu-id="350c6-120">Čas je reprezentován jako počet sekund od 1. ledna 1970 (pod hodnotou 1970-01-01T0:0:0Z) UTC až do okamžiku vypršení platnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="350c6-120">The time is represented as the number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until the time the token validity expires.</span></span>|
| `iss` | <span data-ttu-id="350c6-121">Vystavitel: by měla být client_id (Id aplikace služby klienta)</span><span class="sxs-lookup"><span data-stu-id="350c6-121">Issuer: should be the client_id (Application Id of the client service)</span></span> |
| `jti` | <span data-ttu-id="350c6-122">Identifikátor GUID: JWT ID</span><span class="sxs-lookup"><span data-stu-id="350c6-122">GUID: the JWT ID</span></span> |
| `nbf` | <span data-ttu-id="350c6-123">Neplatný před: datum, před kterou nelze použít daný token.</span><span class="sxs-lookup"><span data-stu-id="350c6-123">Not Before: the date before which the token cannot be used.</span></span> <span data-ttu-id="350c6-124">Čas je reprezentován jako počet sekund od 1. ledna 1970 (pod hodnotou 1970-01-01T0:0:0Z) UTC až do okamžiku byl token vydán.</span><span class="sxs-lookup"><span data-stu-id="350c6-124">The time is represented as the number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until the time the token was issued.</span></span> |
| `sub` | <span data-ttu-id="350c6-125">Předmět: jako u `iss`, by měla být client_id (Id aplikace služby klienta)</span><span class="sxs-lookup"><span data-stu-id="350c6-125">Subject: As for `iss`, should be the client_id (Application Id of the client service)</span></span> |

#### <a name="signature"></a><span data-ttu-id="350c6-126">Podpis</span><span class="sxs-lookup"><span data-stu-id="350c6-126">Signature</span></span>
<span data-ttu-id="350c6-127">Podpis je počítaný použití certifikátu, jak je popsáno v [JSON Web Token RFC7519 specifikace](https://tools.ietf.org/html/rfc7519)</span><span class="sxs-lookup"><span data-stu-id="350c6-127">The signature is computed applying the certificate as described in the [JSON Web Token RFC7519 specification](https://tools.ietf.org/html/rfc7519)</span></span>

### <a name="example-of-a-decoded-jwt-assertion"></a><span data-ttu-id="350c6-128">Příklad dekódované assertion JWT</span><span class="sxs-lookup"><span data-stu-id="350c6-128">Example of a decoded JWT assertion</span></span>
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

### <a name="example-of-an-encoded-jwt-assertion"></a><span data-ttu-id="350c6-129">Příklad kódovaného JWT kontrolní výrazy</span><span class="sxs-lookup"><span data-stu-id="350c6-129">Example of an encoded JWT assertion</span></span>
<span data-ttu-id="350c6-130">Následující řetězec je příkladem kódovaného kontrolní výraz.</span><span class="sxs-lookup"><span data-stu-id="350c6-130">The following string is an example of encoded assertion.</span></span> <span data-ttu-id="350c6-131">Pokud se podíváte pečlivě, jste si všimli tři části oddělený tečkami (.).</span><span class="sxs-lookup"><span data-stu-id="350c6-131">If you look carefully, you notice three sections separated by dots (.).</span></span>
<span data-ttu-id="350c6-132">V první části kóduje záhlaví, druhý datové části a poslední je podpis počítaný s certifikáty z obsahu první dva oddíly.</span><span class="sxs-lookup"><span data-stu-id="350c6-132">The first section encodes the header, the second the payload, and the last is the signature computed with the certificates from the content of the first two sections.</span></span>
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a><span data-ttu-id="350c6-133">Zaregistrujte svůj certifikát s Azure AD</span><span class="sxs-lookup"><span data-stu-id="350c6-133">Register your certificate with Azure AD</span></span>
<span data-ttu-id="350c6-134">Pokud chcete přiřadit certifikát přihlašovacích údajů klientská aplikace ve službě Azure AD, budete muset upravit manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="350c6-134">To associate the certificate credential with the client application in Azure AD, you need to edit the application manifest.</span></span>
<span data-ttu-id="350c6-135">S podržení certifikát, je třeba k výpočtu:</span><span class="sxs-lookup"><span data-stu-id="350c6-135">Having hold of a certificate, you need to compute:</span></span>
- <span data-ttu-id="350c6-136">`$base64Thumbprint`, což je Base64, pomocí kódování hodnota Hash certifikátu</span><span class="sxs-lookup"><span data-stu-id="350c6-136">`$base64Thumbprint`, which is the base64 encoding of the certificate Hash</span></span>
- <span data-ttu-id="350c6-137">`$base64Value`, což je Base64, pomocí kódování nezpracovaná data certifikátu</span><span class="sxs-lookup"><span data-stu-id="350c6-137">`$base64Value`, which is the base64 encoding of the certificate raw data</span></span>

<span data-ttu-id="350c6-138">také musíte zadat identifikátor GUID k identifikaci klíče v manifest aplikace (`$keyId`)</span><span class="sxs-lookup"><span data-stu-id="350c6-138">you also need to provide a GUID to identify the key in the application manifest (`$keyId`)</span></span>

<span data-ttu-id="350c6-139">V registraci aplikace Azure pro klientskou aplikaci, otevřete manifest aplikace a nahradit *keyCredentials* vlastnost s vaší nové informace o certifikátu pomocí následující schématu:</span><span class="sxs-lookup"><span data-stu-id="350c6-139">In the Azure app registration for the client application, open the application manifest, and replace the *keyCredentials* property with your new certificate information using the following schema:</span></span>
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

<span data-ttu-id="350c6-140">Chcete uložit změny do manifestu aplikace a nahrajte do Azure AD.</span><span class="sxs-lookup"><span data-stu-id="350c6-140">Save the edits to the application manifest, and upload to Azure AD.</span></span> <span data-ttu-id="350c6-141">Vlastnost keyCredentials je více hodnot, takže nahrajete více certifikátů pro širší správu klíčů.</span><span class="sxs-lookup"><span data-stu-id="350c6-141">The keyCredentials property is multi-valued, so you may upload multiple certificates for richer key management.</span></span>
