---
title: "aaaAzure Active Directory s B2B spolupráce rozhraní API a přizpůsobení | Microsoft Docs"
description: "Spolupráce Azure Active Directory s B2B podporuje vaše vztahy povolením obchodní partnery tooselectively přístup k podnikovým aplikacím"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: 2609971ffa5d2ebc9466c61f4e4af11f5b045ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení

Narazili jsme mnoho zákazníků, řekněte nám, jestli chtějí toocustomize hello pozvánku proces způsobem, který funguje nejlépe ve svých organizací. Naše rozhraním API můžete to udělat jen. [https://Developer.microsoft.com/Graph/Docs/API-Reference/V1.0/Resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-hello-invitation-api"></a>Možnosti hello pozvánku rozhraní API
Hello rozhraní API nabízí hello následující možnosti:

1. Pozvěte externího uživatele s *žádné* e-mailovou adresu.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Přizpůsobte, kam chcete vaši uživatelé tooland po přijetí jejich pozvánku.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Zvolte toosend hello standardní pozvánku pošty přes nám

    ```
    "sendInvitationMessage": true
    ```

  s toohello příjemce zprávu, kterou si můžete přizpůsobit

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. A zvolte toocc: osob, které mají tookeep v hello cykly o tento spolupracovník pozvání.

5. Nebo zcela přizpůsobit pozvánky a pracovní postup registrace výběrem není toosend oznámení prostřednictvím služby Azure AD.

    ```
    "sendInvitationMessage": false
    ```

  V takovém případě můžete adresu URL se od získat hello rozhraní API, které můžete vložit šablonu e-mailu, zasílání rychlých zpráv nebo jiné metody distribuce podle svého výběru.

6. Nakonec pokud jste správce, můžete tooinvite hello uživatel jako člen.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Modelu autorizace
Hello rozhraní API se může spouštět ve hello následující režimy ověřování:

### <a name="app--user-mode"></a>Aplikace + uživatelského režimu
V tomto režimu, kdo používá potřebám hello rozhraní API toohave hello oprávnění toobe vytvoření pozvánek B2B.

### <a name="app-only-mode"></a>Jenom režim aplikace
V kontextu pouze aplikace musí aplikace hello hello User.ReadWrite.All nebo Directory.ReadWrite.All oborů pro toosucceed pozvánku hello.

Další informace najdete v části: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Je nyní možné toouse prostředí PowerShell tooadd a pozvání externí uživatelé tooan organizace snadno. Vytvoření pozvánky pomocí rutiny hello:

```
New-AzureADMSInvitation
```

Můžete použít hello následující možnosti:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

Můžete se taky podívat na odkaz hello pozvánku k rozhraní API v [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [elementy Hello hello e-mailová pozvánka pro spolupráci B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
