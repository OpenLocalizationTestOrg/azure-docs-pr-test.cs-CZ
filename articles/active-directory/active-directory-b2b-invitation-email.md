---
title: "Elementy pozvánku e-mail spolupráce B2B – Azure Active Directory | Microsoft Docs"
description: "Azure Active Directory s B2B spolupráce pozvánku e-mailové šablony"
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.custom: seohack1
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: b84dba6aa7ac11aef6d958d72564b8b83514a753
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>Elementy pozvánku e-mail spolupráce B2B – Azure Active Directory

E-mailů pozvánku je zásadní součástí a převeďte partnery na palubě jako uživatelé spolupráce B2B ve službě Azure AD. Můžete je používat k příjemce důvěryhodnosti zvýšit. můžete přidat legitimitu a sociálních ověření k e-mailu, abyste měli jistotu příjemce funguje celý výběr **Začínáme** tlačítko pro přijetí pozvánky. Tento vztah důvěryhodnosti je, že klíč znamená snížení sdílení tření. A budete chtít také zajistit e-mailu vypadají skvěle!

![E-mailová pozvánka Azure AD s B2b](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-the-email"></a>Vysvětlení e-mailu
Podívejme se na několik elementy e-mailu, abyste věděli, jak nejlépe používat jejich funkce.

### <a name="subject"></a>Předmět
Předmět e-mailu se následující následující: přijměte naše pozvání &lt;tenantname&gt; organizace

### <a name="from-address"></a>Adresa odesílatele
Používáme LinkedIn jako vzor pro adresa odesílatele.  Musí být jasné, kdo je pozvání odeslal a ze společnosti a také vysvětlení, že e-mailu, pochází z Microsoftu e-mailovou adresu. Formát je: &lt;zobrazovaný název pozvánky&gt; z &lt;tenantname&gt; (přes Microsoft) <invites@microsoft.com&gt;

### <a name="reply-to"></a>Zpáteční adresa
Odpověď pro e-mailu je nastavena k e-mailu pozval vás, pokud je k dispozici, takže odpovídání na e-mailu, odešle e-mailem zpátky do pozvání odeslal.

### <a name="branding"></a>Branding
Pozvánku e-mailů z vašeho používání klienta firemního brandingu, které může nastavili pro vašeho klienta. Pokud budete chtít využít tuto funkci využít [sem](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) jsou uvedeny podrobnosti o tom, jak ho nakonfigurovat. Banner s logem se zobrazí v e-mailu. Postupujte podle velikost bitové kopie kvality pokyny a [sem](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) pro dosažení co nejlepších výsledků. Kromě toho název společnosti také se zobrazí v volání akce.

### <a name="call-to-action"></a>Výzva k akci
Volání akce se skládá ze dvou částí: vysvětlením, proč příjemce přijal e-mailu a co příjemce je se zobrazí dotaz, jak je řešit.
- V části "Proč" lze řešit pomocí následujícího vzorce: jste byli pozváni přístup k aplikacím v &lt;tenantname&gt; organizace

- A "co zobrazí se výzva k provést" části je indikován přítomnost **Začínáme** tlačítko. Pokud příjemce byl přidán bez nutnosti pozvánky, toto tlačítko nezobrazí.

### <a name="inviters-information"></a>Pozval vás na informace
Pozvání odeslal na zobrazované jméno je obsažena v e-mailu. A kromě toho, pokud jste nastavili profilový obrázek pro váš účet Azure AD, pozváním e-mailu, budou obsahovat tento obrázek také. Oba mají zvýšit důvěru vaše příjemce e-mailu.

Pokud jste ještě profilový obrázek, se zobrazí ikona s iniciály pozvání odeslal na místo na obrázku:

  ![Zobrazení pozvánky iniciály](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a>Tělo
Text obsahuje zprávu, která vytvoří pozvání odeslal nebo předána pozvánku rozhraní API. Je textová oblast, takže nezpracovává značky HTML z bezpečnostních důvodů.

### <a name="footer-section"></a>Sekce zápatí
Zápatí obsahuje značky společnosti Microsoft a umožňuje příjemce vědět, pokud e-mailu byla odeslaná z Nesledované alias. Zvláštní případy:

- Pozvání odeslal nemá e-mailovou adresu v pozváním klientů

  ![Obrázek pozval vás nemá e-mailovou adresu v pozváním klientů](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- Příjemce nemusí uplatnit pozvánku

  ![Pokud není třeba uplatnit pozvánku k příjemce](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Další postup

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce Azure AD B2B](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení](active-directory-b2b-api.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
