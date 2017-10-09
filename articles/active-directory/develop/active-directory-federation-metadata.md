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
# <a name="federation-metadata"></a>Metadata federování
Azure Active Directory (Azure AD) publikuje federační metadata dokumentu pro služby, které je nakonfigurováno tokeny zabezpečení hello tooaccept, které Azure AD vydá. Hello formát dokument federačních metadat je popsaná v hello [Web Services Federation Language (WS-Federation) verze 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), který rozšiřuje [Metadata pro hello OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Specifické pro klienta a koncové body metadat nezávislé na klienta
Azure AD publikuje koncové body konkrétního klienta a nezávislé na klienta.

Koncové body konkrétního klienta jsou navrženy pro konkrétního klienta. Hello konkrétního klienta federační metadata obsahují informace o klientovi hello, včetně vystavitele konkrétního klienta a informace o koncovém. Aplikace, které omezují přístup tooa jednoho klienta pomocí konkrétního klienta koncové body.

Koncové body klienta nezávislé poskytují informace, které je běžné tooall klienty Azure AD. Tyto informace se vztahují tootenants hostovanou na *login.microsoftonline.com* , že je sdílená mezi klienty. Nezávislé na klienta koncových bodů – se doporučují pro víceklientské aplikace, vzhledem k tomu, že nejsou přidružené žádné konkrétní klienta.

## <a name="federation-metadata-endpoints"></a>Koncové body federačních metadat
Azure AD publikuje federační metadata v `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Pro **koncové body konkrétního klienta**, hello `TenantDomainName` může být jedna z hello následující typy:

* Klienta registrovaný název domény služby Azure AD, jako například: `contoso.onmicrosoft.com`.
* Hello neměnné ID hello domény, jako například klienta `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Pro **koncové body klienta nezávislé**, hello `TenantDomainName` je `common`. Tento dokument uvádí jenom prvky federačních metadat hello, které jsou společné tooall klienty Azure AD, které jsou hostované v login.microsoftonline.com.

Koncový bod konkrétního klienta může být například `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. koncový bod Hello nezávislé na klienta je [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Dokument metadat federace hello můžete zobrazit tak, že zadáte tuto adresu URL v prohlížeči.

## <a name="contents-of-federation-metadata"></a>Obsah federační Metadata
Hello následující část obsahuje informace potřebné pro služby, které používají hello tokeny vydané službou Azure AD.

### <a name="entity-id"></a>Entity ID
Hello `EntityDescriptor` obsahuje element `EntityID` atribut. Hello hodnotu hello `EntityID` atribut představuje hello vystavitele, tedy hello zabezpečení token služba TOKENŮ token tuto vydaných hello. Pokud obdržíte token, je důležité toovalidate hello vystavitele.

Hello následující metadata zobrazí ukázku konkrétního klienta `EntityDescriptor` element s `EntityID` elementu.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Můžete nahradit hello ID klienta v hello nezávislé na klienta endpoint toocreate ID vašeho klienta konkrétního klienta `EntityID` hodnotu. Výsledná hodnota Hello bude hello stejná hodnota jako vydavatel tokenu hello. strategie Hello umožňuje vystavitele hello toovalidate víceklientské aplikace pro daného klienta.

Hello následující metadata zobrazí ukázku nezávislé na klienta `EntityID` elementu. Upozorňujeme, že hello `{tenant}` je literál, není zástupný symbol.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Podpisové certifikáty tokenu
Když služba přijme token, který je vydaný klienta Azure AD, hello podpisu tokenu hello musí být ověřené pomocí podpisový klíč, který je publikován v dokumentu metadat federace hello. Hello federační metadata obsahují hello veřejnou část hello certifikátů, které hello klientům používat k podepisování tokenů. Nezpracovaná bajtů Hello certifikátu se zobrazí v hello `KeyDescriptor` elementu. podpisový certifikát tokenu Hello je platná pro podepisování pouze když hello hodnotu hello `use` atribut je `signing`.

Dokument metadat federace publikována ve službě Azure AD může mít několik podpisové klíče, jako je například pokud Azure AD se připravuje tooupdate hello podpisový certifikát. Pokud dokument federačních metadat obsahuje více než jeden certifikát, služba, která ověřuje tokeny hello by měly podporovat všechny certifikáty v dokumentu hello.

Hello následující metadata zobrazí ukázku `KeyDescriptor` element s podpisový klíč.

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

Hello `KeyDescriptor` prvek se zobrazuje na dvou místech v dokumentu metadat federace hello; v hello WS-Federation konkrétní a hello SAML konkrétní oddílu. certifikáty Hello publikované v obou částech bude hello stejné.

V části hello WS-Federation konkrétní by čtečku metadat specifikace WS-Federation číst hello certifikáty z `RoleDescriptor` element s hello `SecurityTokenServiceType` typu.

Hello následující metadata zobrazí ukázku `RoleDescriptor` elementu.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

V části hello SAML konkrétní by čtečku metadat specifikace WS-Federation číst hello certifikáty z `IDPSSODescriptor` elementu.

Hello následující metadata zobrazí ukázku `IDPSSODescriptor` elementu.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Nejsou žádné rozdíly ve formátu hello certifikátů konkrétního klienta a nezávislé na klienta.

### <a name="ws-federation-endpoint-url"></a>Adresa URL koncového bodu služby WS-Federation
Hello federační metadata obsahují hello používaná pro jeden přihlašování a odhlašování v protokolu WS-Federation jedním URL, která je Azure AD. Tento koncový bod se zobrazí v hello `PassiveRequestorEndpoint` elementu.

Hello následující metadata zobrazí ukázku `PassiveRequestorEndpoint` element pro koncový bod konkrétního klienta.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Pro koncový bod hello nezávislé na klienta hello WS-Federation adresa URL se zobrazí v koncový bod hello WS-Federation, jak je znázorněno v následující ukázka hello.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>Adresa URL koncového bodu protokolu SAML
Hello federační metadata obsahují hello URL, která používá Azure AD pro jeden přihlašování a odhlašování v protokolu SAML 2.0 jednou. Tyto koncové body se zobrazí v hello `IDPSSODescriptor` elementu.

Hello přihlášení a odhlášení adresy URL se zobrazí v hello `SingleSignOnService` a `SingleLogoutService` elementy.

Hello následující metadata zobrazí ukázku `PassiveResistorEndpoint` pro koncový bod konkrétního klienta.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Podobně hello koncové body pro hello běžné koncové body protokolu SAML 2.0 jsou publikovány v hello nezávislé na klienta federačních metadat, jak je znázorněno v následující ukázka hello.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
