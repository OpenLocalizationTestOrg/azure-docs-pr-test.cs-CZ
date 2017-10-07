---
title: "spolupráce Azure Active Directory s B2B aaaTroubleshooting | Microsoft Docs"
description: "Náhrad pro běžné problémy se spoluprací Azure Active Directory s B2B"
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
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: 6fcfd7e543cd7bb833225f8aa56e332e7a989faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Řešení potíží s spolupráce Azure Active Directory s B2B

Tady jsou některé náhrad pro běžné problémy s spolupráce B2B Azure Active Directory (Azure AD).


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-hello-people-picker"></a>Přidali jste externího uživatele, ale neviděli v globálním adresáři nebo výběr hello osoby.

V případech, kde nejsou naplněny externí uživatele v seznamu hello hello objekt může trvat několik minut tooreplicate.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>Uživatel guest B2B není zobrazovat na výběr SharePoint Online nebo OneDrive osoby. 
 
Hello toosearch možnost pro existující uživatele typu Host v výběr osob hello SharePoint Online (SPO) je vypnuto ve výchozí toomatch starší verze chování.

Tuto funkci můžete povolit pomocí nastavení 'ShowPeoplePickerSuggestionsForGuestUsers' v kolekci klienta a lokality hello úrovně hello. Můžete nastavit pomocí Set-SPOTenant a Set-SPOSite hello rutin, které umožňuje členům funkce hello toosearch všechny existující uživatele typu Host v adresáři hello. Změny v obor klienta hello neovlivňují už zřízené SPO lokalit.

## <a name="invitations-have-been-disabled-for-directory"></a>Pozvánek byla zakázána pro adresář

Pokud se upozornění, že nemáte oprávnění uživatelé tooinvite, ověřte, zda je váš uživatelský účet oprávnění tooinvite externí uživatele v části Nastavení uživatele:

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

Pokud jste nedávno změnili tato nastavení nebo přiřazený hello pozvání hosta odeslal role tooa uživatel, může uběhnout 15 až 60 minut hello změny projeví.

## <a name="hello-user-that-i-invited-is-receiving-an-error-during-redemption"></a>Hello uživatele, který I pozvat přijímá chybu během uplatnění kódu.

Běžné chyby patří:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Pozvaný správce zakázal EmailVerified uživatelé z vytváří v jejich klienta

Po vyzvání uživatele, jehož organizace používá Azure Active Directory, ale kde hello konkrétní uživatelský účet neexistuje (například hello uživatel neexistuje v doméně contoso.com Azure AD). Správce Hello contoso.com může mít zásadu na místě, které brání uživatelům v vytváří. Hello uživatel musí s jejich toodetermine správce zkontrolujte, zda externí uživatelé. Hello externího uživatele správce může být nutné tooallow ověřit e-mailu uživatelů v doméně (Toto [článku](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) na umožnit uživatelům ověřit e-mailu).

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Externí uživatel neexistuje již ve federované domény

Pokud používáte ověřování federace a hello uživatele v Azure Active Directory ještě neexistuje, nejde pozvat uživatele hello.

tooresolve-li tento problém, hello externího uživatele správce musí synchronizovat tooAzure účet hello uživatele služby Active Directory.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Jak se nepodporuje\#', která obvykle není platný znak, synchronizace se službou Azure AD?

"\#" je vyhrazený znak v UPN pro spolupráci Azure AD B2B nebo externí uživatele, protože hello pozvat účet user@contoso.com stane user_contoso.com#EXT@fabrikam.onmicrosoft.com. Proto \# v UPN pocházejících z místní nejsou povoleny toosign v toohello portálu Azure. 

## <a name="i-receive-an-error-when-adding-external-users-tooa-synchronized-group"></a>Při přidávání externích uživatelů tooa synchronizovat skupiny se zobrazí chyba

Externí uživatele lze přidat pouze příliš "přiřazené" nebo "Zabezpečení" skupiny a ne toogroups, které jsou standardní místní.

## <a name="my-external-user-did-not-receive-an-email-tooredeem"></a>Moje externí uživatel neobdržel tooredeem e-mailu

Hello pozvaný potřeba zkontrolovat u svého poskytovatele internetových služeb nebo je povoleno tooensure filtr proti spamu, který hello následující adresy:Invites@microsoft.com

## <a name="i-notice-that-hello-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Všimli jsme si, že tento vlastní zprávu hello nezíská součástí pozvánku zprávy v některých případech

toocomply s zákony o ochraně osobních údajů, rozhraní API nezahrnují vlastních zpráv v e-mailová pozvánka hello při:

- Odesílatel Hello pozvánky nemá e-mailovou adresu v hello pozvání klienta
- Když objektu zabezpečení služby App Service odešle Pozvánka hello

Pokud tento scénář je důležité tooyou, můžete potlačit e-mailová pozvánka naše API a odesílat prostřednictvím e-mailu mechanismus hello podle svého výběru. Podívejte se, že e-mailu, která posíláte tímto způsobem také odpovídá zákony o ochraně osobních údajů toomake právního zástupce vaší organizace.

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [elementy Hello hello e-mailová pozvánka pro spolupráci B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení](active-directory-b2b-api.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
