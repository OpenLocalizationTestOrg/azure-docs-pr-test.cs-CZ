---
title: "Zabezpečení rozhraní API klienta pomocí certifikátu ověřování ve službě API Management - Azure API Management | Microsoft Docs"
description: "Zjistěte, jak zabezpečit přístup k rozhraní API pomocí klientských certifikátů"
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
ms.openlocfilehash: d3d51d0575a6d2dacced931601d48eb1e51a4051
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a><span data-ttu-id="964ad-103">Zabezpečení rozhraní API pomocí klienta pro ověřování pomocí certifikátu ve službě API Management</span><span class="sxs-lookup"><span data-stu-id="964ad-103">How to secure APIs using client certificate authentication in API Management</span></span>

<span data-ttu-id="964ad-104">API Management poskytuje schopnost zabezpečit přístup k rozhraní API (tj. klienta k API Management) pomocí klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="964ad-104">API Management provides the capability to secure access to APIs (i.e., client to API Management) using client certificates.</span></span> <span data-ttu-id="964ad-105">V současné době můžete zkontrolovat kryptografický otisk certifikátu klienta pro požadovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="964ad-105">Currently, you can check the thumbprint of a client certificate against a desired value.</span></span> <span data-ttu-id="964ad-106">Můžete také zkontrolovat kryptografický otisk pro existující certifikátů odeslaných do rozhraní API Management.</span><span class="sxs-lookup"><span data-stu-id="964ad-106">You can also check the thumbprint against existing certificates uploaded to API Management.</span></span>  

<span data-ttu-id="964ad-107">Informace o zabezpečení přístupu ke službě back endové rozhraní API pomocí klientských certifikátů (tj, rozhraní API správy na back-end) najdete v tématu [jak zabezpečit back endové služby pomocí klienta pro ověřování pomocí certifikátu](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)</span><span class="sxs-lookup"><span data-stu-id="964ad-107">For information about securing access to the back-end service of an API using client certificates (i.e., API Management to back-end), see [How to secure back-end services using client certificate authentication](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)</span></span>

## <a name="checking-the-expiration-date"></a><span data-ttu-id="964ad-108">Kontrola datum vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="964ad-108">Checking the expiration date</span></span>

<span data-ttu-id="964ad-109">Níže zásady mohou být konfigurovány ke kontrole, pokud vypršela platnost certifikátu:</span><span class="sxs-lookup"><span data-stu-id="964ad-109">Below policies can be configured to check if the certificate is expired:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-issuer-and-subject"></a><span data-ttu-id="964ad-110">Kontrola vystavitele a předmět</span><span class="sxs-lookup"><span data-stu-id="964ad-110">Checking the issuer and subject</span></span>

<span data-ttu-id="964ad-111">Ke kontrole vystavitele a předmět certifikátu klienta lze nakonfigurovat následující zásady:</span><span class="sxs-lookup"><span data-stu-id="964ad-111">Below policies can be configured to check the issuer and subject of a client certificate:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-thumbprint"></a><span data-ttu-id="964ad-112">Kontrola kryptografický otisk</span><span class="sxs-lookup"><span data-stu-id="964ad-112">Checking the thumbprint</span></span>

<span data-ttu-id="964ad-113">Níže zásady můžete nakonfigurovat zkontrolujte kryptografický otisk certifikátu klienta:</span><span class="sxs-lookup"><span data-stu-id="964ad-113">Below policies can be configured to check the thumbprint of a client certificate:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a><span data-ttu-id="964ad-114">Kontrola kryptografický otisk před certifikáty nahrán do rozhraní API Management</span><span class="sxs-lookup"><span data-stu-id="964ad-114">Checking a thumbprint against certificates uploaded to API Management</span></span>

<span data-ttu-id="964ad-115">Následující příklad ukazuje, jak zkontrolujte kryptografický otisk certifikátu klienta vůči certifikátů odeslaných do rozhraní API správy:</span><span class="sxs-lookup"><span data-stu-id="964ad-115">The following example shows how to check the thumbprint of a client certificate against certificates uploaded to API Management:</span></span> 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a><span data-ttu-id="964ad-116">Další krok</span><span class="sxs-lookup"><span data-stu-id="964ad-116">Next step</span></span>

*  [<span data-ttu-id="964ad-117">Jak zabezpečit back endové služby pomocí klienta pro ověřování pomocí certifikátu</span><span class="sxs-lookup"><span data-stu-id="964ad-117">How to secure back-end services using client certificate authentication</span></span>](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)
*  [<span data-ttu-id="964ad-118">Postup nahrání certifikátů</span><span class="sxs-lookup"><span data-stu-id="964ad-118">How to upload certificates</span></span>](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

