---
title: "aaaMicrosoft Azure Multi-Factor Authentication uživatele stavy"
description: "Další informace o stavu uživatele v Azure MFA."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: cf5b977b09d09330b7b3bc668abd79e602d62015
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorequire-two-step-verification-for-a-user-or-group"></a>Jak toorequire dvoustupňové ověřování pro uživatele nebo skupinu

Existují dva přístupy pro vyžádání dvoustupňové ověřování. Hello první možností je tooenable každý jednotlivý uživatel pro Azure Multi-Factor Authentication (MFA). Pokud uživatelé jsou povolené jednotlivě, fungují vždy dvoustupňové ověření (s několika výjimkami, jako při přihlášení z důvěryhodných adres IP, nebo pokud hello zapamatovaných zařízení funkce je zapnutá). Druhá možnost Hello je tooset si zásadu podmíněného přístupu, která vyžaduje dvoustupňové ověřování za určitých podmínek.

>[!TIP] 
>Vyberte jednu z těchto metod toorequire dvoustupňové ověření, nikoli oba dva. Povolení uživatele pro Azure MFA potlačí všechny zásady podmíněného přístupu.

## <a name="which-option-is-right-for-you"></a>Možnosti, kterou je pro vás nejvhodnější

**Povolení Azure MFA změnou stavů uživatele** je hello tradiční přístup pro vyžádání dvoustupňové ověřování. Funguje pro oba Azure MFA v cloudu hello a Azure MFA serveru. Všichni uživatelé hello povolíte musí mít hello stejné prostředí, které je tooperform dvoustupňové ověřování při každém přihlášení. Povolení uživatele potlačí všechny zásady podmíněného přístupu, které může mít vliv na tohoto uživatele. 

**Povolení Azure MFA se zásadami podmíněného přístupu** je flexibilnější přístup pro vyžádání dvoustupňové ověřování. Ho fungovat pouze pro Azure MFA v cloudu hello, ale a podmíněného přístupu je [placené funkce služby Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Můžete vytvořit zásady podmíněného přístupu, které se vztahují toogroups, jakož i jednotlivé uživatele. S vysokým rizikem skupiny je možné přidělit další omezení než nízkým rizikem skupiny nebo dvoustupňové ověřování můžete vyžaduje se jenom pro vysoce rizikové cloudové aplikace a pro ty, které jsou nízkým rizikem přeskočeno. 

Obě tyto možnosti výzvu tooregister uživatelů pro Azure Multi-Factor Authentication hello při prvním přihlášení po zapnutí hello požadavky. Obě možnosti spolupracovat taky s nástrojem hello konfigurovat [nastavení ověřování Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md)

## <a name="enable-azure-mfa-by-changing-user-status"></a>Povolit Azure MFA změnou stavu uživatele

Uživatelské účty v Azure Multi-Factor Authentication mají hello následující tři jedinečné stavy:

| Status | Popis | Neprohlížečové aplikace vliv |
|:---:|:---:|:---:|
| Zakázáno |Hello výchozího stavu pro nového uživatele, která nejsou zaregistrovaná Azure Multi-Factor Authentication (MFA). |Ne |
| Povoleno |Hello uživatel byl zaregistrován ke službě Azure MFA, ale není registrován. Hello výzvami tooregister budou při příštím přihlášení. |Ne.  Budou pokračovat v práci toowork až do dokončení procesu registrace hello. |
| Vynuceno |Hello uživatel byl zaregistrován a dokončil proces hello registrace pro Azure MFA. |Ano.  Aplikace potřebujete hesla aplikace. |

Stav uživatele zobrazuje, zda správce zapsal je v Azure MFA, a zda jejich dokončení procesu registrace hello.

Všichni uživatelé spustí *zakázáno*. Při zápisu uživatele v Azure MFA, jejich změny stavu *povoleno*. Při povolení uživatelé přihlášení a dokončení procesu registrace hello, jejich stav se změní příliš*vynucené*.  

### <a name="view-hello-status-for-a-user"></a>Zobrazit stav hello pro uživatele

Hello použijte následující kroky tooaccess hello stránky, kde můžete zobrazit a spravovat stavů uživatele:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako správce.
2. Přejděte příliš**Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**.
3. Vyberte **služby Multi-Factor Authentication**.
   ![Vyberte služby Multi-Factor Authentication](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. Otevře se nová stránka, která zobrazuje stavů uživatele hello.
   ![Stav uživatele služby Multi-Factor authentication – snímek obrazovky](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-hello-status-for-a-user"></a>Změnit stav hello pro uživatele

1. Použijte hello předchozích kroků tooget toohello služby Multi-Factor authentication uživatelům stránky.
2. Najít hello uživatele, že chcete tooenable pro Azure MFA. Může být nutné toochange hello zobrazení v horní části hello. 
   ![Najít uživatele – snímek obrazovky](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Zkontrolujte, zda text hello pole Další tootheir název.
4. Na pravé straně v části Rychlé kroky hello, zvolte **povolit** nebo **zakázat**.
   ![Povolit vybraného uživatele – snímek obrazovky](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >*Povolit* uživatelé automaticky přepínají příliš*vynucené* při registraci pro Azure MFA. Neměňte ručně tooenforced stavu uživatele hello. 

5. Potvrďte výběr v hello místní okno, které se otevře. 

Po povolení uživatelů, je vhodné seznámit e-mailem. Sdělte jim, že vyzve tooregister hello při příštím přihlášení. Navíc pokud vaše organizace používá neprohlížečové aplikace, které nepodporují moderní ověřování, budou potřebovat toocreate hesel aplikací. Můžete použít také odkaz tooour [Průvodce pro koncové uživatele Azure MFA](./end-user/multi-factor-authentication-end-user.md) toohelp je začít pracovat.

### <a name="use-powershell"></a>Použití prostředí PowerShell
Stav toochange hello uživatele stav pomocí [Azure AD PowerShell](/powershell/azure/overview), změňte `$st.State`. Existují tři možné stavy:

* Povoleno
* Vynuceno
* Zakázáno  

Nelze přesunout uživatele přímo toohello *vynucené* stavu. Aplikace založené na jiné prohlížeči se zastaví pracovat, protože nebyl hello uživatel neprošel registrací MFA a získat [heslo aplikace](multi-factor-authentication-whats-next.md#app-passwords). 

Pomocí prostředí PowerShell je vhodný, pokud budete potřebovat toobulk povolení uživatelů. Vytvořte Powershellový skript, který prochází seznam uživatelů a umožňuje jim:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Zde naleznete příklad:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Povolit Azure MFA se zásadami podmíněného přístupu

Podmíněný přístup je placené funkce služby Azure Active Directory, s mnoha možností možné konfigurace. Tyto kroky provede jedním ze způsobů toocreate zásadu. Další informace najdete v tématu o [podmíněného přístupu v Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako správce.
2. Přejděte příliš**Azure Active Directory** > **podmíněného přístupu**.
3. Vyberte **nové zásady**.
4. V části **přiřazení**, vyberte **uživatelů a skupin**. Použití hello **zahrnout** a **vyloučit** karty toospecify, kteří uživatelé a skupiny bude spravovat zásady hello.
5. V části **přiřazení**, vyberte **cloudových aplikací**. Zvolte tooinclude **všech cloudových aplikací**.
6. V části **přístup k ovládacím prvkům**, vyberte **Grant**. Zvolte **vyžadovat vícefaktorové ověřování**.
7. Zapnout **povolit zásady** příliš**na** a pak vyberte **Uložit**.

Hello další možnosti v zásadách podmíněného přístupu hello umožní toospecify přesně v případě, že by měl být požadované dvoustupňové ověřování. Například můžete dokonce vytvářet zásady, které stavy: Pokud dodavatelů pokusí tooaccess naše aplikace Nákup z nedůvěryhodné sítě na zařízení, které nejsou připojené k doméně, vyžaduje dvoustupňové ověřování. 

## <a name="next-steps"></a>Další kroky

- Získejte tipy pro hello [osvědčené postupy pro podmíněný přístup](../active-directory/active-directory-conditional-access-best-practices.md)

- Správa nastavení služby Multi-Factor Authentication pro [uživatelům a jejich zařízení](multi-factor-authentication-manage-users-and-devices.md)