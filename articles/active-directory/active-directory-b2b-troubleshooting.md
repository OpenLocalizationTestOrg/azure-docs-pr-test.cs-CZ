---
title: "Řešení potíží s Azure Active Directory s B2B spolupráce | Microsoft Docs"
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
ms.openlocfilehash: 2009cfc956a2703e268c9364996aa2d0fbd8f279
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Řešení potíží s spolupráce Azure Active Directory s B2B

Tady jsou některé náhrad pro běžné problémy s spolupráce B2B Azure Active Directory (Azure AD).


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>Přidali jste externího uživatele, ale neviděli v globálním adresáři nebo v dialogu pro výběr osob

V případech, kde nejsou naplněny externí uživatele v seznamu objekt může trvat několik minut k replikaci.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>Uživatel guest B2B není zobrazovat na výběr SharePoint Online nebo OneDrive osoby. 
 
Umožňuje vyhledat existující uživatele typu Host v dialogu pro výběr osob SharePoint Online (SPO) je ve výchozím nastavení tak, aby odpovídaly starší verze chování OFF.

Tuto funkci můžete povolit pomocí nastavení 'ShowPeoplePickerSuggestionsForGuestUsers' na úrovni kolekce klienta a lokality. Můžete nastavit funkci pomocí rutiny Set-SPOTenant a Set-SPOSite, díky členy pro vyhledávání v adresáři všechny existující uživatele typu Host. Změny v oboru klienta neovlivní už zřízené SPO lokalit.

## <a name="invitations-have-been-disabled-for-directory"></a>Pozvánek byla zakázána pro adresář

Pokud se upozornění, že nemáte oprávnění pro uživatele pozvat, ověřte, že je váš uživatelský účet oprávnění pro externí uživatele v části Nastavení uživatele pozvat:

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

Pokud jste nedávno změnili tato nastavení nebo přiřazenou roli pozvání hosta odeslal na uživatele, může uběhnout 15 až 60 minut před změny se projeví.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>Uživatel, který I pozvat přijímá chybu během uplatnění kódu.

Běžné chyby patří:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Pozvaný správce zakázal EmailVerified uživatelé z vytváří v jejich klienta

Když pozvat uživatele, jehož organizace používá Azure Active Directory, ale kde konkrétní uživatelský účet neexistuje (například uživatel neexistuje v doméně contoso.com Azure AD). Správce contoso.com může mít zásadu na místě, které brání uživatelům v vytváří. Uživatel musí zkontrolovat u jejich správce k určení, zda jsou externí uživatele povolen. Správce externího uživatele možná muset povolit ověřit e-mailu uživatelům ve své doméně (Toto [článku](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) na umožnit uživatelům ověřit e-mailu).

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Externí uživatel neexistuje již ve federované domény

Pokud používáte ověřování federace a uživatele ve službě Azure Active Directory ještě neexistuje, nejde pozvat uživatele.

Chcete-li tento problém vyřešit, musí správce externího uživatele synchronizovat uživatelského účtu do služby Azure Active Directory.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Jak se nepodporuje\#', která obvykle není platný znak, synchronizace se službou Azure AD?

"\#" je vyhrazený znak v UPN pro spolupráci Azure AD B2B nebo externí uživatele, protože pozvané účet user@contoso.com stane user_contoso.com#EXT@fabrikam.onmicrosoft.com. Proto \# v UPN pocházejících z místní nejsou povoleny pro přihlášení k portálu Azure. 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Při přidávání externích uživatelů do skupiny synchronizované se zobrazí chyba

Externí uživatelé lze přidat pouze do "přiřazené" nebo "Zabezpečení" skupiny a ne do skupiny, které jsou standardní místní.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>Moje externí uživatel neobdržel uplatnit e-mailu

Pozvané potřeba zkontrolovat u svého poskytovatele internetových služeb nebo nevyžádané pošty filtru zajistit, že je povolené následující adresy:Invites@microsoft.com

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Všimli jsme si, že vlastní zprávu nezíská součástí pozvánku zprávy v některých případech

Abyste dosáhli souladu s zákony o ochraně osobních údajů, rozhraní API nezahrnují vlastních zpráv v e-mailová pozvánka při:

- Pozvání odeslal nemá e-mailovou adresu v pozváním klienta
- Když objektu zabezpečení služby App Service odešle pozvánku

Pokud tento scénář je pro vás důležité, můžete potlačit e-mailová pozvánka naše API a odeslat e-mailu mechanismem podle svého výběru. Poraďte vaší organizace právního zástupce a ujistěte se všechny e-mailu, že odesílat, že tak také odpovídá zákony o ochraně osobních údajů.

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [Elementy e-mail pozvánku spolupráce B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení](active-directory-b2b-api.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
