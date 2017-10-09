---
title: "Jeden protokol přihlašovací Out SAML aaaAzure | Microsoft Docs"
description: "Tento článek popisuje hello jeden protokol SAML Sign-Out v Azure Active Directory"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: 889c9b3397a601c16ba6971d2b15bfee305576de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Protokol jeden odhlašování SAML
Azure Active Directory (Azure AD) hello podporuje SAML 2.0 webové prohlížeče jediného odhlašování profilu. Pro jeden odhlašování toowork správně, hello **LogoutURL** pro hello aplikace musí být explicitně zaregistrované v Azure AD při registraci aplikace. Azure AD používá hello LogoutURL tooredirect uživatelé po se odhlásili.

Tento diagram zobrazuje pracovní postup hello hello Azure AD jeden proces přihlášení.

![Jednotné přihlašování se pracovní postup](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## LogoutRequest
Hello zasílá cloudové služby `LogoutRequest` tooindicate tooAzure AD zpráva, že relace byla ukončena. Hello následující výňatek ze zobrazí ukázku `LogoutRequest` elementu.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### LogoutRequest
Hello `LogoutRequest` element odeslané tooAzure AD vyžaduje hello následující atributy:

* `ID`: Toto identifikuje hello odhlašování požadavku. Hello hodnota `ID` nesmí začínat číslem. Typické postupem Hello je tooappend **id** toohello řetězcovou reprezentaci identifikátor GUID.
* `Version`: Nastavte hodnotu hello tohoto elementu příliš**2.0**. Tato hodnota se vyžaduje.
* `IssueInstant`: Toto je `DateTime` řetězec s hodnotou koordinaci světový čas (UTC) a [odezvy formátu ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD očekává hodnotu typu, ale nedokáže vynutit.

### Vystavitel
Hello `Issuer` element v `LogoutRequest` musí přesně shodovat s jedním z hello **ServicePrincipalNames** v hello cloudové služby ve službě Azure AD. Obvykle je nastavena v toohello **identifikátor ID URI aplikace** , který je určen při registraci aplikace.

### NameID
Hello hodnotu hello `NameID` element musí přesně shodovat hello `NameID` hello uživatele, který je právě odhlášení.

## LogoutResponse
Odešle Azure AD `LogoutResponse` v odpovědi tooa `LogoutRequest` element. Hello následující výňatek ze zobrazí ukázku `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### LogoutResponse
Azure AD Nastaví hello `ID`, `Version` a `IssueInstant` hodnoty v hello `LogoutResponse` elementu. Nastaví taky hello `InResponseTo` element toohello hodnotu hello `ID` atribut hello `LogoutRequest` který vyvolaná hello odpovědi.

### Vystavitel
Azure AD nastavuje tuto hodnotu příliš`https://login.microsoftonline.com/<TenantIdGUID>/` kde <TenantIdGUID> je ID klienta hello klienta hello Azure AD.

Hodnota hello tooevaluate hello `Issuer` elementu, použijte hodnotu hello hello **identifikátor ID URI aplikace** zadané při registraci aplikace.

### Status
Azure AD používá hello `StatusCode` element v hello `Status` element tooindicate hello úspěch nebo neúspěch odhlášení. Při pokusu o odhlášení hello selže, hello `StatusCode` element může také obsahovat vlastní chybové zprávy.
