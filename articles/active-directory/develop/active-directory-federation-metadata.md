---
title: "aaaAzure AD federačních metadat | Microsoft Docs"
description: "Tento článek popisuje hello federační metadata dokument, který publikuje Azure Active Directory pro služby, které přijímají tokeny služby Azure Active Directory."
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
ms.openlocfilehash: 23535bcd5eeb3e9b2e17d89a9b0420fc98bd3895
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="federation-metadata"></a><span data-ttu-id="62959-103">Metadata federování</span><span class="sxs-lookup"><span data-stu-id="62959-103">Federation metadata</span></span>
<span data-ttu-id="62959-104">Azure Active Directory (Azure AD) publikuje federační metadata dokumentu pro služby, které je nakonfigurováno tokeny zabezpečení hello tooaccept, které Azure AD vydá.</span><span class="sxs-lookup"><span data-stu-id="62959-104">Azure Active Directory (Azure AD) publishes a federation metadata document for services that is configured tooaccept hello security tokens that Azure AD issues.</span></span> <span data-ttu-id="62959-105">Hello formát dokument federačních metadat je popsaná v hello [Web Services Federation Language (WS-Federation) verze 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), který rozšiřuje [Metadata pro hello OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span><span class="sxs-lookup"><span data-stu-id="62959-105">hello federation metadata document format is described in hello [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), which extends [Metadata for hello OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span></span>

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a><span data-ttu-id="62959-106">Specifické pro klienta a koncové body metadat nezávislé na klienta</span><span class="sxs-lookup"><span data-stu-id="62959-106">Tenant-specific and Tenant-independent metadata endpoints</span></span>
<span data-ttu-id="62959-107">Azure AD publikuje koncové body konkrétního klienta a nezávislé na klienta.</span><span class="sxs-lookup"><span data-stu-id="62959-107">Azure AD publishes tenant-specific and tenant-independent endpoints.</span></span>

<span data-ttu-id="62959-108">Koncové body konkrétního klienta jsou navrženy pro konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="62959-108">Tenant-specific endpoints are designed for a particular tenant.</span></span> <span data-ttu-id="62959-109">Hello konkrétního klienta federační metadata obsahují informace o klientovi hello, včetně vystavitele konkrétního klienta a informace o koncovém.</span><span class="sxs-lookup"><span data-stu-id="62959-109">hello tenant-specific federation metadata includes information about hello tenant, including tenant-specific issuer and endpoint information.</span></span> <span data-ttu-id="62959-110">Aplikace, které omezují přístup tooa jednoho klienta pomocí konkrétního klienta koncové body.</span><span class="sxs-lookup"><span data-stu-id="62959-110">Applications that restrict access tooa single tenant use tenant-specific endpoints.</span></span>

<span data-ttu-id="62959-111">Koncové body klienta nezávislé poskytují informace, které je běžné tooall klienty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62959-111">Tenant-independent endpoints provide information that is common tooall Azure AD tenants.</span></span> <span data-ttu-id="62959-112">Tyto informace se vztahují tootenants hostovanou na *login.microsoftonline.com* , že je sdílená mezi klienty.</span><span class="sxs-lookup"><span data-stu-id="62959-112">This information applies tootenants hosted at *login.microsoftonline.com* and is shared across tenants.</span></span> <span data-ttu-id="62959-113">Nezávislé na klienta koncových bodů – se doporučují pro víceklientské aplikace, vzhledem k tomu, že nejsou přidružené žádné konkrétní klienta.</span><span class="sxs-lookup"><span data-stu-id="62959-113">Tenant-independent endpoints are recommended for multi-tenant applications, since they are not associated with any particular tenant.</span></span>

## <a name="federation-metadata-endpoints"></a><span data-ttu-id="62959-114">Koncové body federačních metadat</span><span class="sxs-lookup"><span data-stu-id="62959-114">Federation metadata endpoints</span></span>
<span data-ttu-id="62959-115">Azure AD publikuje federační metadata v `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="62959-115">Azure AD publishes federation metadata at `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span>

<span data-ttu-id="62959-116">Pro **koncové body konkrétního klienta**, hello `TenantDomainName` může být jedna z hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="62959-116">For **tenant-specific endpoints**, hello `TenantDomainName` can be one of hello following types:</span></span>

* <span data-ttu-id="62959-117">Klienta registrovaný název domény služby Azure AD, jako například: `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="62959-117">A registered domain name of an Azure AD tenant, such as: `contoso.onmicrosoft.com`.</span></span>
* <span data-ttu-id="62959-118">Hello neměnné ID hello domény, jako například klienta `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span><span class="sxs-lookup"><span data-stu-id="62959-118">hello immutable tenant ID of hello domain, such as `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span></span>

<span data-ttu-id="62959-119">Pro **koncové body klienta nezávislé**, hello `TenantDomainName` je `common`.</span><span class="sxs-lookup"><span data-stu-id="62959-119">For **tenant-independent endpoints**, hello `TenantDomainName` is `common`.</span></span> <span data-ttu-id="62959-120">Tento dokument uvádí jenom prvky federačních metadat hello, které jsou společné tooall klienty Azure AD, které jsou hostované v login.microsoftonline.com.</span><span class="sxs-lookup"><span data-stu-id="62959-120">This document lists only hello Federation Metadata elements that are common tooall Azure AD tenants that are hosted at login.microsoftonline.com.</span></span>

<span data-ttu-id="62959-121">Koncový bod konkrétního klienta může být například `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="62959-121">For example, a tenant-specific endpoint might be `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span> <span data-ttu-id="62959-122">koncový bod Hello nezávislé na klienta je [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span><span class="sxs-lookup"><span data-stu-id="62959-122">hello tenant-independent endpoint is [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span></span> <span data-ttu-id="62959-123">Dokument metadat federace hello můžete zobrazit tak, že zadáte tuto adresu URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="62959-123">You can view hello federation metadata document by typing this URL in a browser.</span></span>

## <a name="contents-of-federation-metadata"></a><span data-ttu-id="62959-124">Obsah federační Metadata</span><span class="sxs-lookup"><span data-stu-id="62959-124">Contents of federation Metadata</span></span>
<span data-ttu-id="62959-125">Hello následující část obsahuje informace potřebné pro služby, které používají hello tokeny vydané službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62959-125">hello following section provides information needed by services that consume hello tokens issued by Azure AD.</span></span>

### <a name="entity-id"></a><span data-ttu-id="62959-126">Entity ID</span><span class="sxs-lookup"><span data-stu-id="62959-126">Entity ID</span></span>
<span data-ttu-id="62959-127">Hello `EntityDescriptor` obsahuje element `EntityID` atribut.</span><span class="sxs-lookup"><span data-stu-id="62959-127">hello `EntityDescriptor` element contains an `EntityID` attribute.</span></span> <span data-ttu-id="62959-128">Hello hodnotu hello `EntityID` atribut představuje hello vystavitele, tedy hello zabezpečení token služba TOKENŮ token tuto vydaných hello.</span><span class="sxs-lookup"><span data-stu-id="62959-128">hello value of hello `EntityID` attribute represents hello issuer, that is, hello security token service (STS) that issued hello token.</span></span> <span data-ttu-id="62959-129">Pokud obdržíte token, je důležité toovalidate hello vystavitele.</span><span class="sxs-lookup"><span data-stu-id="62959-129">It is important toovalidate hello issuer when you receive a token.</span></span>

<span data-ttu-id="62959-130">Hello následující metadata zobrazí ukázku konkrétního klienta `EntityDescriptor` element s `EntityID` elementu.</span><span class="sxs-lookup"><span data-stu-id="62959-130">hello following metadata shows a sample tenant-specific `EntityDescriptor` element with an `EntityID` element.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
<span data-ttu-id="62959-131">Můžete nahradit hello ID klienta v hello nezávislé na klienta endpoint toocreate ID vašeho klienta konkrétního klienta `EntityID` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="62959-131">You can replace hello tenant ID in hello tenant-independent endpoint with your tenant ID toocreate a tenant-specific `EntityID` value.</span></span> <span data-ttu-id="62959-132">Výsledná hodnota Hello bude hello stejná hodnota jako vydavatel tokenu hello.</span><span class="sxs-lookup"><span data-stu-id="62959-132">hello resulting value will be hello same as hello token issuer.</span></span> <span data-ttu-id="62959-133">strategie Hello umožňuje vystavitele hello toovalidate víceklientské aplikace pro daného klienta.</span><span class="sxs-lookup"><span data-stu-id="62959-133">hello strategy allows a multi-tenant application toovalidate hello issuer for a given tenant.</span></span>

<span data-ttu-id="62959-134">Hello následující metadata zobrazí ukázku nezávislé na klienta `EntityID` elementu.</span><span class="sxs-lookup"><span data-stu-id="62959-134">hello following metadata shows a sample tenant-independent `EntityID` element.</span></span> <span data-ttu-id="62959-135">Upozorňujeme, že hello `{tenant}` je literál, není zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="62959-135">Please note, that hello `{tenant}` is a literal, not a placeholder.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a><span data-ttu-id="62959-136">Podpisové certifikáty tokenu</span><span class="sxs-lookup"><span data-stu-id="62959-136">Token signing certificates</span></span>
<span data-ttu-id="62959-137">Když služba přijme token, který je vydaný klienta Azure AD, hello podpisu tokenu hello musí být ověřené pomocí podpisový klíč, který je publikován v dokumentu metadat federace hello.</span><span class="sxs-lookup"><span data-stu-id="62959-137">When a service receives a token that is issued by a Azure AD tenant, hello signature of hello token must be validated with a signing key that is published in hello federation metadata document.</span></span> <span data-ttu-id="62959-138">Hello federační metadata obsahují hello veřejnou část hello certifikátů, které hello klientům používat k podepisování tokenů.</span><span class="sxs-lookup"><span data-stu-id="62959-138">hello federation metadata includes hello public portion of hello certificates that hello tenants use for token signing.</span></span> <span data-ttu-id="62959-139">Nezpracovaná bajtů Hello certifikátu se zobrazí v hello `KeyDescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="62959-139">hello certificate raw bytes appear in hello `KeyDescriptor` element.</span></span> <span data-ttu-id="62959-140">podpisový certifikát tokenu Hello je platná pro podepisování pouze když hello hodnotu hello `use` atribut je `signing`.</span><span class="sxs-lookup"><span data-stu-id="62959-140">hello token signing certificate is valid for signing only when hello value of hello `use` attribute is `signing`.</span></span>

<span data-ttu-id="62959-141">Dokument metadat federace publikována ve službě Azure AD může mít několik podpisové klíče, jako je například pokud Azure AD se připravuje tooupdate hello podpisový certifikát.</span><span class="sxs-lookup"><span data-stu-id="62959-141">A federation metadata document published by Azure AD can have multiple signing keys, such as when Azure AD is preparing tooupdate hello signing certificate.</span></span> <span data-ttu-id="62959-142">Pokud dokument federačních metadat obsahuje více než jeden certifikát, služba, která ověřuje tokeny hello by měly podporovat všechny certifikáty v dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="62959-142">When a federation metadata document includes more than one certificate, a service that is validating hello tokens should support all certificates in hello document.</span></span>

<span data-ttu-id="62959-143">Hello následující metadata zobrazí ukázku `KeyDescriptor` element s podpisový klíč.</span><span class="sxs-lookup"><span data-stu-id="62959-143">hello following metadata shows a sample `KeyDescriptor` element with a signing key.</span></span>

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

<span data-ttu-id="62959-144">Hello `KeyDescriptor` prvek se zobrazuje na dvou místech v dokumentu metadat federace hello; v hello WS-Federation konkrétní a hello SAML konkrétní oddílu.</span><span class="sxs-lookup"><span data-stu-id="62959-144">hello `KeyDescriptor` element appears in two places in hello federation metadata document; in hello WS-Federation-specific section and hello SAML-specific section.</span></span> <span data-ttu-id="62959-145">certifikáty Hello publikované v obou částech bude hello stejné.</span><span class="sxs-lookup"><span data-stu-id="62959-145">hello certificates published in both sections will be hello same.</span></span>

<span data-ttu-id="62959-146">V části hello WS-Federation konkrétní by čtečku metadat specifikace WS-Federation číst hello certifikáty z `RoleDescriptor` element s hello `SecurityTokenServiceType` typu.</span><span class="sxs-lookup"><span data-stu-id="62959-146">In hello WS-Federation-specific section, a WS-Federation metadata reader would read hello certificates from a `RoleDescriptor` element with hello `SecurityTokenServiceType` type.</span></span>

<span data-ttu-id="62959-147">Hello následující metadata zobrazí ukázku `RoleDescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="62959-147">hello following metadata shows a sample `RoleDescriptor` element.</span></span>

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

<span data-ttu-id="62959-148">V části hello SAML konkrétní by čtečku metadat specifikace WS-Federation číst hello certifikáty z `IDPSSODescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="62959-148">In hello SAML-specific section, a WS-Federation metadata reader would read hello certificates from a `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="62959-149">Hello následující metadata zobrazí ukázku `IDPSSODescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="62959-149">hello following metadata shows a sample `IDPSSODescriptor` element.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
<span data-ttu-id="62959-150">Nejsou žádné rozdíly ve formátu hello certifikátů konkrétního klienta a nezávislé na klienta.</span><span class="sxs-lookup"><span data-stu-id="62959-150">There are no differences in hello format of tenant-specific and tenant-independent certificates.</span></span>

### <a name="ws-federation-endpoint-url"></a><span data-ttu-id="62959-151">Adresa URL koncového bodu služby WS-Federation</span><span class="sxs-lookup"><span data-stu-id="62959-151">WS-Federation endpoint URL</span></span>
<span data-ttu-id="62959-152">Hello federační metadata obsahují hello používaná pro jeden přihlašování a odhlašování v protokolu WS-Federation jedním URL, která je Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62959-152">hello federation metadata includes hello URL that is Azure AD uses for single sign-in and single sign-out in WS-Federation protocol.</span></span> <span data-ttu-id="62959-153">Tento koncový bod se zobrazí v hello `PassiveRequestorEndpoint` elementu.</span><span class="sxs-lookup"><span data-stu-id="62959-153">This endpoint appears in hello `PassiveRequestorEndpoint` element.</span></span>

<span data-ttu-id="62959-154">Hello následující metadata zobrazí ukázku `PassiveRequestorEndpoint` element pro koncový bod konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="62959-154">hello following metadata shows a sample `PassiveRequestorEndpoint` element for a tenant-specific endpoint.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
<span data-ttu-id="62959-155">Pro koncový bod hello nezávislé na klienta hello WS-Federation adresa URL se zobrazí v koncový bod hello WS-Federation, jak je znázorněno v následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="62959-155">For hello tenant-independent endpoint, hello WS-Federation URL appears in hello WS-Federation endpoint, as shown in hello following sample.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a><span data-ttu-id="62959-156">Adresa URL koncového bodu protokolu SAML</span><span class="sxs-lookup"><span data-stu-id="62959-156">SAML protocol endpoint URL</span></span>
<span data-ttu-id="62959-157">Hello federační metadata obsahují hello URL, která používá Azure AD pro jeden přihlašování a odhlašování v protokolu SAML 2.0 jednou.</span><span class="sxs-lookup"><span data-stu-id="62959-157">hello federation metadata includes hello URL that Azure AD uses for single sign-in and single sign-out in SAML 2.0 protocol.</span></span> <span data-ttu-id="62959-158">Tyto koncové body se zobrazí v hello `IDPSSODescriptor` elementu.</span><span class="sxs-lookup"><span data-stu-id="62959-158">These endpoints appear in hello `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="62959-159">Hello přihlášení a odhlášení adresy URL se zobrazí v hello `SingleSignOnService` a `SingleLogoutService` elementy.</span><span class="sxs-lookup"><span data-stu-id="62959-159">hello sign-in and sign-out URLs appear in hello `SingleSignOnService` and `SingleLogoutService` elements.</span></span>

<span data-ttu-id="62959-160">Hello následující metadata zobrazí ukázku `PassiveResistorEndpoint` pro koncový bod konkrétního klienta.</span><span class="sxs-lookup"><span data-stu-id="62959-160">hello following metadata shows a sample `PassiveResistorEndpoint` for a tenant-specific endpoint.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

<span data-ttu-id="62959-161">Podobně hello koncové body pro hello běžné koncové body protokolu SAML 2.0 jsou publikovány v hello nezávislé na klienta federačních metadat, jak je znázorněno v následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="62959-161">Similarly hello endpoints for hello common SAML 2.0 protocol endpoints are published in hello tenant-independent federation metadata, as shown in hello following sample.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
