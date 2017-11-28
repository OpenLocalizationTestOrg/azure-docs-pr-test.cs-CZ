---
title: "Jeden protokol přihlášení na SAML aaaAzure | Microsoft Docs"
description: "Tento článek popisuje protokol jeden znak na SAML hello v Azure Active Directory"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: ad8437f5-b887-41ff-bd77-779ddafc33fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: 435cfe0e7be3f2defd34e8b6f6b0f08596ee1f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Jeden protokol přihlašování SAML
Tento článek popisuje požadavky na ověření hello SAML 2.0 a odpovědi, které podporuje Azure Active Directory (Azure AD) pro jednotné přihlašování.

Následující diagram protokolu Hello popisuje hello jeden přihlašování pořadí. Hello cloudové služby (Zprostředkovatel služby hello) používá toopass vazby přesměrování protokolu HTTP `AuthnRequest` (žádosti o ověření) element tooAzure AD (hello zprostředkovatele identity). Azure AD poté používá HTTP post vazby toopost `Response` element toohello cloudové služby.

![Jednotné přihlašování pracovního postupu](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## AuthnRequest
Odeslat toorequest ověřování uživatelů, cloudové služby `AuthnRequest` element tooAzure AD. Ukázka SAML 2.0 `AuthnRequest` může vypadat například takto:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parametr |  | Popis |
| --- | --- | --- |
| ID |Požadované |Tento atribut toopopulate hello používá Azure AD `InResponseTo` atribut hello vrátil odpověď. ID nesmí začínat číslicí, tak, aby strategie běžné tooprepend řetězec jako "id" toohello řetězcovou reprezentaci identifikátor GUID. Například `id6c1c178c166d486687be4aaf5e482730` je platné ID. |
| Verze |Požadované |Měl by být **2.0**. |
| IssueInstant |Požadované |Toto je datum a čas řetězec s hodnotou UTC a [odezvy formátu ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD očekává hodnotu DateTime tohoto typu, ale nemá vyhodnocení nebo použijte hodnotu hello. |
| AssertionConsumerServiceUrl |Volitelné |Pokud je zadán, musí odpovídat hello `RedirectUri` hello cloudové služby ve službě Azure AD. |
| ForceAuthn |Volitelné | Je to logická hodnota. Pokud nastavena hodnota true, to znamená, že hello uživatele vynucené toore-ověřit, i když mají platný relace s Azure AD. |
| IsPassive |Volitelné |To je logická hodnota, která určuje, zda Azure AD by měl ověřit uživatele hello tiše, bez zásahu uživatele pomocí souboru cookie relace hello, pokud nějaká existuje. Pokud je to pravda, Azure AD se pokusí tooauthenticate hello uživatele pomocí souboru cookie relace hello. |

Všechny ostatní `AuthnRequest` atributy, jako jsou souhlasu, cílový, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex a ProviderName se **Ignorovat**.

Azure AD také ignoruje hello `Conditions` element v `AuthnRequest`.

### Vystavitel
Hello `Issuer` element v `AuthnRequest` musí přesně shodovat s jedním z hello **ServicePrincipalNames** v hello cloudové služby ve službě Azure AD. Obvykle je nastavena v toohello **identifikátor ID URI aplikace** , který je určen při registraci aplikace.

Výňatek ze SAML ukázka obsahující hello `Issuer` element vypadá takto:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### NameIDPolicy
Tento element požaduje konkrétní název formátu ID v odpovědi hello a je v volitelné `AuthnRequest` elementy odeslané tooAzure AD.

Ukázka `NameIdPolicy` element vypadá takto:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Pokud `NameIDPolicy` je zadáno, můžete zahrnout jeho volitelné `Format` atribut. Hello `Format` atribut může mít pouze jeden z následujících hodnot hello; žádné jiné hodnoty dojde k chybě.

* `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory vystaví deklarace NameID hello jako pairwise identifikátor.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory vystaví deklarace identity NameID hello ve formátu e-mailovou adresu.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Tato hodnota umožňuje Azure Active Directory tooselect hello deklarace identity formátu. Azure Active Directory vystaví hello NameID jako pairwise identifikátor.
* `urn:oasis:names:tc:SAML:2.0:nameid-format:transient`: Azure Active Directory vystaví deklarace NameID hello jako náhodně generované hodnotu, která je jedinečný toohello aktuální operace jednotné přihlašování. To znamená, že hodnota hello je dočasný a nemůže být použité tooidentify hello ověřování uživatele.

Azure AD ignoruje hello `AllowCreate` atribut.

### RequestAuthnContext
Hello `RequestedAuthnContext` element určuje hello potřeby metody ověřování. Zadání je volitelné v `AuthnRequest` elementy odeslané tooAzure AD. Azure AD podporuje pouze jeden `AuthnContextClassRef` hodnota: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### Obor
Hello `Scoping` element, který obsahuje seznam zprostředkovatelů identity, je v volitelné `AuthnRequest` elementy odeslané tooAzure AD.

Pokud je zadán, nezahrnujte hello `ProxyCount` atribut `IDPListOption` nebo `RequesterID` elementu, jak nejsou podporovány.

### Podpis
Nezahrnovat `Signature` element v `AuthnRequest` elementy, protože Azure AD nepodporují podepsané žádosti o ověření.

### Předmět
Azure AD ignoruje hello `Subject` element `AuthnRequest` elementy.

## Odpověď
Pokud požadovaný přihlašování dokončí úspěšně, Azure AD odešle odpověď toohello cloudové služby. Ukázková odpověď tooa úspěšné přihlášení požadavku na vypadá takto:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### Odpověď
Hello `Response` element zahrnuje hello výsledek hello autorizace požadavku. Azure AD Nastaví hello `ID`, `Version` a `IssueInstant` hodnoty v hello `Response` elementu. Nastaví i hello následující atributy:

* `Destination`: Při přihlašování dokončí úspěšně, je nastavena v toohello `RedirectUri` hello zprostředkovatele (Cloudová služba).
* `InResponseTo`: Tato hodnota je nastavena toohello `ID` atribut hello `AuthnRequest` element, který iniciuje hello odpovědi.

### Vystavitel
Azure AD Nastaví hello `Issuer` element příliš`https://login.microsoftonline.com/<TenantIDGUID>/` kde <TenantIDGUID> je ID klienta hello klienta hello Azure AD.

Ukázková odpověď s elementem vystavitele může například vypadat například takto:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### Status
Hello `Status` vyjadřuje prvek hello úspěšná nebo neúspěšná přihlášení. Obsahuje hello `StatusCode` element, který obsahuje kód nebo sadu vnořené kódy, které představují hello stav hello požadavku. Zahrnuje také hello `StatusMessage` element, který obsahuje vlastní chybové zprávy, která jsou generována během procesu přihlášení hello.

<!-- TODO: Add a authentication protocol error reference -->

Následující Hello se SAML odpovědi tooan neúspěšných přihlášení o pokus.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: hello SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### Kontrolní výraz
V přidání toohello `ID`, `IssueInstant` a `Version`, Azure AD Nastaví hello následující prvky v hello `Assertion` element hello odpovědi.

#### Vystavitel
To je nastaven příliš`https://sts.windows.net/<TenantIDGUID>/`kde <TenantIDGUID> je hello ID klienta klienta hello Azure AD.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### Podpis
Azure AD podepisuje hello assertion v odpovědi tooa úspěšné přihlášení. Hello `Signature` element obsahuje digitální podpis, hello cloudové služby můžete použít tooauthenticate hello zdroj tooverify hello integritu hello assertion.

Tento digitální podpis, Azure AD používá toogenerate hello podpisový klíč v hello `IDPSSODescriptor` element jeho dokument metadat.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### Předmět
Toto nastavení určuje hello objekt zabezpečení, který je hello předmět hello příkazy v hello assertion. Obsahuje `NameID` element, který představuje hello ověřeného uživatele. Hello `NameID` hodnota je cílový identifikátor, který je poskytovatel směrovanou toohello pouze služby, který je cílovou skupinu hello hello token. Je trvalé – se dají odvolávat, ale nikdy opětovně přiřazován. Je také neprůhledné, v, aby neodhalí nic o hello uživateli a nelze použít jako identifikátor pro dotazy atributů.

Hello `Method` atribut hello `SubjectConfirmation` element vždy nastaven příliš`urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### Podmínky
Tento element určuje podmínky, které definují hello přijatelné pomocí kontrolních výrazů SAML.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

Hello `NotBefore` a `NotOnOrAfter` atributy zadejte hello interval, během které hello kontrolní výraz je platný.

* Hello hodnotu hello `NotBefore` atribut je rovna tooor mírně (méně než druhý) později než hodnota hello `IssueInstant` atribut hello `Assertion` elementu. Azure AD neanalyzuje žádné časový rozdíl mezi ním a hello cloudové služby (service provider) a nepřidá kdykoli toothis vyrovnávací paměti.
* Hello hodnotu hello `NotOnOrAfter` atribut je 70 minut později než hodnota hello hello `NotBefore` atribut.

#### Cílová skupina
Tato položka obsahuje identifikátor URI identifikující cílová skupina. Azure AD Nastaví hodnotu hello hodnota toohello element `Issuer` element hello `AuthnRequest` která inicializována hello přihlašování. tooevaluate hello `Audience` hodnoty, použijte hodnotu hello hello `App ID URI` zadaný během registrace aplikace.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Jako hello `Issuer` hodnoty hello `Audience` hodnota musí odpovídat přesně jeden z hello hlavní názvy služby představující hello cloudové služby ve službě Azure AD. Ale pokud hello hodnotu z hello `Issuer` element není hodnota identifikátoru URI, hello `Audience` v odpovědi hello hello hodnota `Issuer` hodnotu s předponou `spn:`.

#### AttributeStatement
Tato položka obsahuje deklarace identity o subjektu hello nebo uživatele. Hello následující výpis obsahuje ukázku `AttributeStatement` elementu. tři tečky Hello označuje, že hello element může obsahovat více atributy a hodnoty atributů.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```        

* **Název deklarace identity** : hello hodnotu hello `Name` atribut (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`), jako je hlavní název uživatele hello hello ověřeného uživatele, `testuser@managedtenant.com`.
* **Deklarace identity které** : hello hodnotu hello `ObjectIdentifier` atribut (`http://schemas.microsoft.com/identity/claims/objectidentifier`) je hello `ObjectId` hello objektu adresáře, který představuje hello ověřit uživatele ve službě Azure AD. `ObjectId`je nezměnitelná, globálně jedinečný, a znovu používat bezpečné identifikátor hello ověřeného uživatele.

#### AuthnStatement
Tento element vyhodnotí, že tento kontrolní výraz subjektu hello byl ověřen konkrétní prostředky v určitou dobu.

* Hello `AuthnInstant` atribut určuje hello čas, který hello uživatel ověřený službou Azure AD.
* Hello `AuthnContext` element určuje kontext ověřování hello používá tooauthenticate hello uživatele.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```