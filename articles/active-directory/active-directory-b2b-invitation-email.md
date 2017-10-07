---
title: "prvky aaaThe e-mailová pozvánka hello Azure Active Directory s B2B spolupráce | Microsoft Docs"
description: "Azure Active Directory s B2B spolupráce pozvánku e-mailové šablony"
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: f4908014d71a63442bbdca2182f54c7a79675a82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-elements-of-hello-b2b-collaboration-invitation-email"></a>elementy Hello hello e-mailová pozvánka pro spolupráci B2B

E-mailů pozvánku jsou partnery toobring zásadní součástí na palubě jako uživatelé spolupráce B2B ve službě Azure AD. Můžete je používat důvěryhodnosti tooincrease hello příjemce. můžete přidat legitimitu a sociálních doklad toohello e-mailu, zda text hello příjemce toomake funguje celý výběr hello **Začínáme** tlačítko tooaccept hello pozvánku. Tento vztah důvěryhodnosti je, že klíč znamená tooreduce sdílení tření. A je také potřeba toomake hello e-mailu vzhledu skvělé!

![E-mailová pozvánka Azure AD s B2b](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-hello-email"></a>Vysvětlení hello e-mailu
Podívejme na několik elementy hello e-mailů, abyste věděli, jak nejlepší toouse jejich možnosti.

### <a name="subject"></a>Předmět
Hello předmět e-mailu hello následuje hello následující vzor: přijměte naše pozvání toohello &lt;tenantname&gt; organizace

### <a name="from-address"></a>Z adresy
Používáme LinkedIn jako vzor pro hello z adresy.  Měli byste být zrušte kdo je hello pozvánky a ze společnosti a také vysvětlení, že e-mailové hello pochází z e-mailovou adresu společnosti Microsoft. Formát Hello: &lt;zobrazovaný název pozvánky&gt; z &lt;tenantname&gt; (přes Microsoft) <invites@microsoft.com&gt;

### <a name="reply-to"></a>Odpovědět
Hello odpovědi tooemail nastavena e-mailu pozval toohello vás, pokud je k dispozici, tak, aby odeslání odpovědi, že odešle e-mailu toohello back toohello pozvánky e-mailu.

### <a name="branding"></a>Branding
Hello pozvánku e-mailů z vašeho klienta použít hello firemní branding, který může mít nastavíte pro vašeho klienta. Pokud chcete tuto funkci využít tootake [sem](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) jsou hello podrobnosti o tom, tooconfigure ho. Hello banner s logem se zobrazí v e-mailu hello. Postupujte podle hello velikost bitové kopie kvality pokyny a [sem](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) pro dosažení co nejlepších výsledků. Kromě toho název společnosti hello taky se zobrazí v tooaction volání hello.

### <a name="call-tooaction"></a>Volání tooaction
Hello volání tooaction se skládá ze dvou částí: vysvětlením, proč hello příjemce přijal hello e-mailu a jaké příjemce hello je požadováno toodo o něm.
- Hello "Proč" části se dají řešit pomocí hello následující vzor: jste byla pozvané tooaccess aplikace v hello &lt;tenantname&gt; organizace

- A hello "co jste se se zobrazí dotaz, toodo" část je indikován hello přítomnost hello **Začínáme** tlačítko. Pokud příjemce hello přidala bez nutnosti hello pozvánky, toto tlačítko nezobrazí.

### <a name="inviters-information"></a>Pozval vás na informace
Odesílatel Hello pozvánky zobrazovaný název je součástí hello e-mailu. A kromě toho, pokud jste nastavili profilový obrázek pro váš účet Azure AD, hello pozvání e-mailu budou obsahovat tento obrázek také. Obě jsou určený tooincrease vaše příjemce důvěru hello e-mailu.

Pokud jste ještě profilový obrázek, se zobrazí ikona s pozval hello vás iniciály místo hello obrázek:

  ![zobrazení iniciály pozvánky hello](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a>Tělo
Hello text obsahuje uvítací zprávu této pozvánky hello vytvoří nebo předána hello pozvánku rozhraní API. Je textová oblast, takže nezpracovává značky HTML z bezpečnostních důvodů.

### <a name="footer-section"></a>Sekce zápatí
zápatí Hello obsahuje hello značky společnosti Microsoft a umožňuje hello příjemce vědět, pokud e-mailu hello byla odeslaná z Nesledované alias. Zvláštní případy:

- Odesílatel Hello pozvánky nemá e-mailovou adresu v hello pozvání klientů

  ![Obrázek pozval vás nemá e-mailovou adresu v hello pozvání klientů](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- příjemce Hello nepotřebuje tooredeem hello Pozvánka

  ![Pokud příjemce nepotřebuje tooredeem Pozvánka](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Další kroky

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
