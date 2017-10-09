---
title: "aaaSecure rozhraní API pomocí ověření klientského certifikátu ve službě API Management - Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toosecure přístup tooAPIs pomocí klientských certifikátů"
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: apimpm
ms.openlocfilehash: 6ff78bda3d429829da79d0dc4d652f19669cc919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-apis-using-client-certificate-authentication-in-api-management"></a>Jak toosecure rozhraní API pomocí klientských certifikátů ověřování v API Management

Služba API Management nabízí hello schopností toosecure přístup tooAPIs (tj, klient tooAPI Management) pomocí klientských certifikátů. V současné době můžete zkontrolovat hello kryptografický otisk certifikátu klienta pro požadovanou hodnotu. Můžete také zkontrolovat, že kryptografický otisk hello před existující certifikáty načtený tooAPI správy.  

Informace o zabezpečení služby back-end toohello přístup rozhraní API pomocí klientských certifikátů (tj, API Management tooback-end) najdete v tématu [jak toosecure back endové služby pomocí klientského certifikátu ověřování](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)

## <a name="checking-hello-expiration-date"></a>Kontrola, zda text hello datum vypršení platnosti

Níže zásad může být nakonfigurované toocheck, pokud vypršela platnost certifikátu hello:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-issuer-and-subject"></a>Kontrola, zda text hello vystavitele a předmět

Níže zásad může být nakonfigurované toocheck hello vystavitele a předmět certifikátu klienta:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-thumbprint"></a>Kontrola, zda text hello kryptografický otisk

Níže zásad může být nakonfigurované toocheck hello kryptografický otisk certifikátu klienta:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-tooapi-management"></a>Kontrola kryptografický otisk před certifikáty nahrán tooAPI správy

Hello následující příklad ukazuje, jak nahrát toocheck hello kryptografický otisk certifikátu klienta před certifikáty tooAPI správy: 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a>Další krok

*  [Jak toosecure back endové služby pomocí klientského certifikátu ověřování](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)
*  [Jak tooupload certifikáty](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

