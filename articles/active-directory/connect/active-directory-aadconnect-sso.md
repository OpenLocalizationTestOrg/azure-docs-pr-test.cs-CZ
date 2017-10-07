---
title: "Azure AD Connect: Bezproblémové jednotné přihlašování | Microsoft Docs"
description: "Toto téma popisuje Azure Active Directory (Azure AD) bezproblémové jednotné přihlašování a jak umožňuje vám tooprovide true jednotné přihlašování pro podnikové ploše uživatele uvnitř firemní sítě."
services: active-directory
keywords: "Co je Azure AD Connect, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 11c778c37ee39866dcc3a14ab648cfcd8e322e47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory bezproblémové jednotné přihlašování

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Co je Azure Active Directory bezproblémové jednotné přihlašování?

Azure Active Directory bezproblémové jednotné přihlašování (Azure AD bezproblémové jednotné přihlašování) automaticky přihlásí uživatele v když jsou v podnikové síti připojené tooyour firemní zařízení. Když je povolené, nejsou uživatelé potřebovat tootype v jejich hesla toosign v tooAzure AD a obvykle i zadejte ve svých uživatelských jmen. Tato funkce poskytuje snadný přístup vašich uživatelů tooyour cloudové aplikace bez nutnosti jakékoli další místní komponenty.

Bezproblémové jednotného přihlašování je možné kombinovat s buď hello [synchronizaci hodnoty Hash hesla](active-directory-aadconnectsync-implement-password-synchronization.md) nebo [předávací ověřování](active-directory-aadconnect-pass-through-authentication.md) metody přihlašování.

![Bezproblémové jednotné přihlašování](./media/active-directory-aadconnect-sso/sso1.png)

>[!IMPORTANT]
>Bezproblémové jednotného přihlašování je aktuálně ve verzi preview. Tato funkce je _není_ použít tooActive Directory Federation Services (ADFS).

## <a name="key-benefits"></a>Klíčové výhody

- *Vysoký výkon uživatele*
  - Uživatelé jsou automaticky přihlášeni k místní i cloudové aplikace.
  - Uživatelé nemají tooenter hesla opakovaně.
- *Snadno toodeploy & Spravovat*
  - Žádné další součásti potřebné místní toomake činnost.
  - Funguje s jakékoli metody objektu cloudové ověřování - [synchronizaci hodnoty Hash hesla](active-directory-aadconnectsync-implement-password-synchronization.md) nebo [předávací ověřování](active-directory-aadconnect-pass-through-authentication.md).
  - Můžete se vrátit na toosome nebo všech uživatelů pomocí zásad skupiny.
  - Registrace zařízení s Windows 10 s Azure AD bez nutnosti hello jakékoliv infrastruktury služby AD FS. Tato funkce potřebuje toouse verze 2.1 nebo novější z hello [připojení k pracovišti klienta](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Označuje funkce

- Přihlašovací uživatelské jméno může být buď hello místní výchozí uživatelské jméno (`userPrincipalName`) nebo jiný atribut, které jsou nakonfigurované v Azure AD Connect (`Alternate ID`). Obě použít případy práce, protože bezproblémové jednotného přihlašování k používá hello `securityIdentifier` deklarace identity v toolook lístek protokolu Kerberos hello až hello odpovídající objekt uživatele ve službě Azure AD.
- Bezproblémové jednotného přihlašování je oportunistické funkce. Pokud z nějakého důvodu selže, hello přihlášení činnost koncového uživatele přejde zpět regulární chování tooits – tj, hello uživatel potřebuje tooenter své heslo na přihlašovací stránku hello.
- Pokud aplikace předává `domain_hint` (OpenID Connect) nebo `whr` (SAML) parametr - identifikaci vašeho klienta nebo `login_hint` parametr - identifikace hello uživatele v Azure AD přihlášení požadavku, uživatelé se automaticky přihlásíte bez nich zadávat uživatelská jména a hesla.
- Může být povoleno přes Azure AD Connect.
- Je bezplatné funkce a nepotřebujete žádné placené edice Azure AD toouse ho.
- Je podporováno v webovými klienty založené na prohlížeči a klienti Office, které podporují [moderní ověřování](https://aka.ms/modernauthga) na platformách a prohlížeče podporující ověřování protokolu Kerberos:

| OS\Browser |Internet Explorer|Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Ano|Ne|Ano|Ano\*|Není k dispozici
|Windows 8.1|Ano|Není k dispozici|Ano|Ano\*|Není k dispozici
|Windows 8|Ano|Není k dispozici|Ano|Ano\*|Není k dispozici
|Windows 7|Ano|Není k dispozici|Ano|Ano\*|Není k dispozici
|Mac OS X|Není k dispozici|Není k dispozici|Ano\*|Ano\*|Ano\*

\*Vyžaduje [další konfigurace](active-directory-aadconnect-sso-quick-start.md#browser-considerations)

>[!IMPORTANT]
>Jsme nedávno vrácena podporu pro hraniční tooinvestigate zákazníka hlášené problémy.

>[!NOTE]
>Pro Windows 10, hello doporučuje toouse [Azure AD Join](../active-directory-azureadjoin-overview.md) pro hello optimální jeden přihlašování s Azure AD.

## <a name="next-steps"></a>Další kroky

- [**Rychlý Start** ](active-directory-aadconnect-sso-quick-start.md) – zprovoznění a systémem Azure bezproblémové jednotného přihlašování k AD.
- [**Podrobné technické informace** ](active-directory-aadconnect-sso-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Nejčastější dotazy** ](active-directory-aadconnect-sso-faq.md) -odpovědi toofrequently dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
