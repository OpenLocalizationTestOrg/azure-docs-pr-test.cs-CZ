---
title: "Federační Metadata Azure AD | Microsoft Docs"
description: "Tento článek popisuje federační metadata dokumentu, který publikuje Azure Active Directory pro služby, které přijímají tokeny služby Azure Active Directory."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c2d5f80b-aa74-452c-955b-d8eb3ed62652
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ecafb02a6ac13d1c3cd1fe77ef710cd8525e32b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="federation-metadata"></a><span data-ttu-id="6ead1-103">Metadata federování</span><span class="sxs-lookup"><span data-stu-id="6ead1-103">Federation metadata</span></span>
<span data-ttu-id="6ead1-104">Azure Active Directory (Azure AD) publikuje dokumentu metadat federace pro služby nakonfigurovaný tak, aby přijímal tokeny zabezpečení, které Azure AD vydá.</span><span class="sxs-lookup"><span data-stu-id="6ead1-104">Azure Active Directory (Azure AD) publishes a federation metadata document for services that is configured to accept the security tokens that Azure AD issues.</span></span> <span data-ttu-id="6ead1-105">Formát dokumentu metadat federace je popsaný v tématu [Web Services Federation Language (WS-Federation) verze 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), který rozšiřuje [Metadata pro v2.0 OASIS Security Assertion Markup Language (SAML)](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span><span class="sxs-lookup"><span data-stu-id="6ead1-105">The federation metadata document format is described in the [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), which extends [Metadata for the OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span></span>

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a><span data-ttu-id="6ead1-106">Specifické pro klienta a koncové body metadat nezávislé na klienta</span><span class="sxs-lookup"><span data-stu-id="6ead1-106">Tenant-specific and Tenant-independent metadata endpoints</span></span>
<span data-ttu-id="6ead1-107">Azure AD publikuje koncové body konkrétního klienta a nezávislé na klienta.</span><span class="sxs-lookup"><span data-stu-id="6ead1-107">Azure AD publishes tenant-specific and tenant-independent endpoints.</span></span>

<span data-ttu-id="6ead1-108">Koncové body konkrétního klienta jsou navrženy pro konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="6ead1-108">Tenant-specific endpoints are designed for a particular tenant.</span></span> <span data-ttu-id="6ead1-109">Specifické pro klienta federační metadata obsahují informace o klientovi, včetně informací o konkrétního klienta vystavitele a koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-109">The tenant-specific federation metadata includes information about the tenant, including tenant-specific issuer and endpoint information.</span></span> <span data-ttu-id="6ead1-110">Aplikace, které omezují přístup na jednoho klienta pomocí konkrétního klienta koncové body.</span><span class="sxs-lookup"><span data-stu-id="6ead1-110">Applications that restrict access to a single tenant use tenant-specific endpoints.</span></span>

<span data-ttu-id="6ead1-111">Koncové body klienta nezávislé poskytují informace, které jsou společné pro všechny klienty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6ead1-111">Tenant-independent endpoints provide information that is common to all Azure AD tenants.</span></span> <span data-ttu-id="6ead1-112">Tyto informace platí pro klienty hostovanou na *login.microsoftonline.com* , že je sdílená mezi klienty.</span><span class="sxs-lookup"><span data-stu-id="6ead1-112">This information applies to tenants hosted at *login.microsoftonline.com* and is shared across tenants.</span></span> <span data-ttu-id="6ead1-113">Nezávislé na klienta koncových bodů – se doporučují pro víceklientské aplikace, vzhledem k tomu, že nejsou přidružené žádné konkrétní klienta.</span><span class="sxs-lookup"><span data-stu-id="6ead1-113">Tenant-independent endpoints are recommended for multi-tenant applications, since they are not associated with any particular tenant.</span></span>

## <a name="federation-metadata-endpoints"></a><span data-ttu-id="6ead1-114">Koncové body federačních metadat</span><span class="sxs-lookup"><span data-stu-id="6ead1-114">Federation metadata endpoints</span></span>
<span data-ttu-id="6ead1-115">Azure AD publikuje federační metadata v `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="6ead1-115">Azure AD publishes federation metadata at `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span>

<span data-ttu-id="6ead1-116">Pro **koncové body konkrétního klienta**, `TenantDomainName` může být jedna z následujících typů:</span><span class="sxs-lookup"><span data-stu-id="6ead1-116">For **tenant-specific endpoints**, the `TenantDomainName` can be one of the following types:</span></span>

* <span data-ttu-id="6ead1-117">Klienta registrovaný název domény služby Azure AD, jako například: `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="6ead1-117">A registered domain name of an Azure AD tenant, such as: `contoso.onmicrosoft.com`.</span></span>
* <span data-ttu-id="6ead1-118">Neměnné ID domény, jako například klienta `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span><span class="sxs-lookup"><span data-stu-id="6ead1-118">The immutable tenant ID of the domain, such as `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span></span>

<span data-ttu-id="6ead1-119">Pro **koncové body klienta nezávislé**, `TenantDomainName` je `common`.</span><span class="sxs-lookup"><span data-stu-id="6ead1-119">For **tenant-independent endpoints**, the `TenantDomainName` is `common`.</span></span> <span data-ttu-id="6ead1-120">Tento dokument uvádí jenom prvky federačních metadat, které jsou společné pro všechny klienty Azure AD, které jsou hostované v login.microsoftonline.com.</span><span class="sxs-lookup"><span data-stu-id="6ead1-120">This document lists only the Federation Metadata elements that are common to all Azure AD tenants that are hosted at login.microsoftonline.com.</span></span>

<span data-ttu-id="6ead1-121">Koncový bod konkrétního klienta může být například `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="6ead1-121">For example, a tenant-specific endpoint might be `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span> <span data-ttu-id="6ead1-122">Koncový bod nezávislé na klienta je [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span><span class="sxs-lookup"><span data-stu-id="6ead1-122">The tenant-independent endpoint is [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span></span> <span data-ttu-id="6ead1-123">Dokument metadat federace můžete zobrazit tak, že zadáte tuto adresu URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6ead1-123">You can view the federation metadata document by typing this URL in a browser.</span></span>

## <a name="contents-of-federation-metadata"></a><span data-ttu-id="6ead1-124">Obsah federační Metadata</span><span class="sxs-lookup"><span data-stu-id="6ead1-124">Contents of federation Metadata</span></span>
<span data-ttu-id="6ead1-125">Následující část obsahuje informace potřebné pro služby, které používají tokeny vydané službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6ead1-125">The following section provides information needed by services that consume the tokens issued by Azure AD.</span></span>

### <a name="entity-id"></a><span data-ttu-id="6ead1-126">Entity ID</span><span class="sxs-lookup"><span data-stu-id="6ead1-126">Entity ID</span></span>
<span data-ttu-id="6ead1-127">`EntityDescriptor` Obsahuje element `EntityID` atribut.</span><span class="sxs-lookup"><span data-stu-id="6ead1-127">The `EntityDescriptor` element contains an `EntityID` attribute.</span></span> <span data-ttu-id="6ead1-128">Hodnota `EntityID` atribut představuje vystavitele, tedy služby tokenů zabezpečení (STS) vydaný token.</span><span class="sxs-lookup"><span data-stu-id="6ead1-128">The value of the `EntityID` attribute represents the issuer, that is, the security token service (STS) that issued the token.</span></span> <span data-ttu-id="6ead1-129">Je důležité k ověření vystavitele, když obdrží token.</span><span class="sxs-lookup"><span data-stu-id="6ead1-129">It is important to validate the issuer when you receive a token.</span></span>

<span data-ttu-id="6ead1-130">Následující metadata zobrazí ukázku konkrétního klienta `EntityDescriptor` element s `EntityID` elementu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-130">The following metadata shows a sample tenant-specific `EntityDescriptor` element with an `EntityID` element.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
<span data-ttu-id="6ead1-131">ID klienta v koncovém bodě nezávislé na klienta můžete nahradit vaše ID klienta k vytvoření konkrétního klienta `EntityID` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-131">You can replace the tenant ID in the tenant-independent endpoint with your tenant ID to create a tenant-specific `EntityID` value.</span></span> <span data-ttu-id="6ead1-132">Výsledná hodnota bude stejné jako vydavatel tokenu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-132">The resulting value will be the same as the token issuer.</span></span> <span data-ttu-id="6ead1-133">Strategie umožňuje víceklientské aplikace k ověření vystavitele pro daného klienta.</span><span class="sxs-lookup"><span data-stu-id="6ead1-133">The strategy allows a multi-tenant application to validate the issuer for a given tenant.</span></span>

<span data-ttu-id="6ead1-134">Následující metadata zobrazí ukázku nezávislé na klienta `EntityID` elementu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-134">The following metadata shows a sample tenant-independent `EntityID` element.</span></span> <span data-ttu-id="6ead1-135">Upozorňujeme, že `{tenant}` je literál, není zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="6ead1-135">Please note, that the `{tenant}` is a literal, not a placeholder.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a><span data-ttu-id="6ead1-136">Podpisové certifikáty tokenu</span><span class="sxs-lookup"><span data-stu-id="6ead1-136">Token signing certificates</span></span>
<span data-ttu-id="6ead1-137">Když služba přijme token, který je vydaný klienta Azure AD, musí být podpis tokenu ověřené pomocí podpisový klíč, který je publikován v dokument federačních metadat.</span><span class="sxs-lookup"><span data-stu-id="6ead1-137">When a service receives a token that is issued by a Azure AD tenant, the signature of the token must be validated with a signing key that is published in the federation metadata document.</span></span> <span data-ttu-id="6ead1-138">Federační metadata obsahují veřejnou část certifikátů, které klienti používat k podepisování tokenů.</span><span class="sxs-lookup"><span data-stu-id="6ead1-138">The federation metadata includes the public portion of the certificates that the tenants use for token signing.</span></span> <span data-ttu-id="6ead1-139">Nezpracovaná bajtů certifikátu se zobrazí v `KeyDescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-139">The certificate raw bytes appear in the `KeyDescriptor` element.</span></span> <span data-ttu-id="6ead1-140">Podpisový certifikát tokenu je platný pro podepisování pouze tehdy, když hodnota `use` atribut je `signing`.</span><span class="sxs-lookup"><span data-stu-id="6ead1-140">The token signing certificate is valid for signing only when the value of the `use` attribute is `signing`.</span></span>

<span data-ttu-id="6ead1-141">Dokument metadat federace publikována ve službě Azure AD může mít několik podpisové klíče, například když se připravuje Azure AD k aktualizaci podpisového certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-141">A federation metadata document published by Azure AD can have multiple signing keys, such as when Azure AD is preparing to update the signing certificate.</span></span> <span data-ttu-id="6ead1-142">Pokud dokument federačních metadat obsahuje více než jeden certifikát, služba, která je ověřování tokenů by měly podporovat všechny certifikáty v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-142">When a federation metadata document includes more than one certificate, a service that is validating the tokens should support all certificates in the document.</span></span>

<span data-ttu-id="6ead1-143">Následující metadata zobrazí ukázku `KeyDescriptor` element s podpisový klíč.</span><span class="sxs-lookup"><span data-stu-id="6ead1-143">The following metadata shows a sample `KeyDescriptor` element with a signing key.</span></span>

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

<span data-ttu-id="6ead1-144">`KeyDescriptor` Prvek se zobrazuje na dvou místech v dokument federačních metadat, v části WS-Federation konkrétní a v části konkrétní SAML.</span><span class="sxs-lookup"><span data-stu-id="6ead1-144">The `KeyDescriptor` element appears in two places in the federation metadata document; in the WS-Federation-specific section and the SAML-specific section.</span></span> <span data-ttu-id="6ead1-145">Certifikáty, které jsou publikovány v obou částech budou stejné.</span><span class="sxs-lookup"><span data-stu-id="6ead1-145">The certificates published in both sections will be the same.</span></span>

<span data-ttu-id="6ead1-146">V části WS-Federation konkrétní by čtečku metadat specifikace WS-Federation číst certifikáty z `RoleDescriptor` element s `SecurityTokenServiceType` typu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-146">In the WS-Federation-specific section, a WS-Federation metadata reader would read the certificates from a `RoleDescriptor` element with the `SecurityTokenServiceType` type.</span></span>

<span data-ttu-id="6ead1-147">Následující metadata zobrazí ukázku `RoleDescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-147">The following metadata shows a sample `RoleDescriptor` element.</span></span>

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

<span data-ttu-id="6ead1-148">V části konkrétní SAML by čtečku metadat specifikace WS-Federation číst certifikáty z `IDPSSODescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-148">In the SAML-specific section, a WS-Federation metadata reader would read the certificates from a `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="6ead1-149">Následující metadata zobrazí ukázku `IDPSSODescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-149">The following metadata shows a sample `IDPSSODescriptor` element.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
<span data-ttu-id="6ead1-150">Nejsou žádné rozdíly ve formátu konkrétního klienta a nezávislé na klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="6ead1-150">There are no differences in the format of tenant-specific and tenant-independent certificates.</span></span>

### <a name="ws-federation-endpoint-url"></a><span data-ttu-id="6ead1-151">Adresa URL koncového bodu služby WS-Federation</span><span class="sxs-lookup"><span data-stu-id="6ead1-151">WS-Federation endpoint URL</span></span>
<span data-ttu-id="6ead1-152">Federační metadata obsahují adresu URL, která se používá Azure AD pro jeden přihlašování a odhlašování v protokolu WS-Federation jednou.</span><span class="sxs-lookup"><span data-stu-id="6ead1-152">The federation metadata includes the URL that is Azure AD uses for single sign-in and single sign-out in WS-Federation protocol.</span></span> <span data-ttu-id="6ead1-153">Tento koncový bod se zobrazí v `PassiveRequestorEndpoint` elementu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-153">This endpoint appears in the `PassiveRequestorEndpoint` element.</span></span>

<span data-ttu-id="6ead1-154">Následující metadata zobrazí ukázku `PassiveRequestorEndpoint` element pro koncový bod konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="6ead1-154">The following metadata shows a sample `PassiveRequestorEndpoint` element for a tenant-specific endpoint.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
<span data-ttu-id="6ead1-155">Pro koncový bod nezávislé na klienta adresa URL služby WS-Federation se zobrazí v koncovém bodě WS-Federation, jak znázorňuje následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="6ead1-155">For the tenant-independent endpoint, the WS-Federation URL appears in the WS-Federation endpoint, as shown in the following sample.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a><span data-ttu-id="6ead1-156">Adresa URL koncového bodu protokolu SAML</span><span class="sxs-lookup"><span data-stu-id="6ead1-156">SAML protocol endpoint URL</span></span>
<span data-ttu-id="6ead1-157">Federační metadata obsahují adresu URL, která používá Azure AD pro jeden přihlašování a odhlašování v protokolu SAML 2.0 jednou.</span><span class="sxs-lookup"><span data-stu-id="6ead1-157">The federation metadata includes the URL that Azure AD uses for single sign-in and single sign-out in SAML 2.0 protocol.</span></span> <span data-ttu-id="6ead1-158">Tyto koncové body se zobrazí v `IDPSSODescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="6ead1-158">These endpoints appear in the `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="6ead1-159">Přihlášení a odhlášení adresy URL se zobrazí v `SingleSignOnService` a `SingleLogoutService` elementy.</span><span class="sxs-lookup"><span data-stu-id="6ead1-159">The sign-in and sign-out URLs appear in the `SingleSignOnService` and `SingleLogoutService` elements.</span></span>

<span data-ttu-id="6ead1-160">Následující metadata zobrazí ukázku `PassiveResistorEndpoint` pro koncový bod konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="6ead1-160">The following metadata shows a sample `PassiveResistorEndpoint` for a tenant-specific endpoint.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

<span data-ttu-id="6ead1-161">Podobně jako koncové body pro běžné koncové body protokolu SAML 2.0 jsou publikovány v nezávislé na klienta federačních metadat, jak znázorňuje následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="6ead1-161">Similarly the endpoints for the common SAML 2.0 protocol endpoints are published in the tenant-independent federation metadata, as shown in the following sample.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
