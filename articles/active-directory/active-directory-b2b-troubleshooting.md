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
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="fba88-103">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="fba88-103">Troubleshooting Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="fba88-104">Tady jsou některé náhrad pro běžné problémy s spolupráce B2B Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fba88-104">Here are some remedies for common problems with Azure Active Directory (Azure AD) B2B collaboration.</span></span>


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-hello-people-picker"></a><span data-ttu-id="fba88-105">Přidali jste externího uživatele, ale neviděli v globálním adresáři nebo výběr hello osoby.</span><span class="sxs-lookup"><span data-stu-id="fba88-105">I’ve added an external user but do not see them in my Global Address Book or in hello people picker</span></span>

<span data-ttu-id="fba88-106">V případech, kde nejsou naplněny externí uživatele v seznamu hello hello objekt může trvat několik minut tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="fba88-106">In cases where external users are not populated in hello list, hello object might take a few minutes tooreplicate.</span></span>

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a><span data-ttu-id="fba88-107">Uživatel guest B2B není zobrazovat na výběr SharePoint Online nebo OneDrive osoby.</span><span class="sxs-lookup"><span data-stu-id="fba88-107">A B2B guest user is not showing up in SharePoint Online/OneDrive people picker</span></span> 
 
<span data-ttu-id="fba88-108">Hello toosearch možnost pro existující uživatele typu Host v výběr osob hello SharePoint Online (SPO) je vypnuto ve výchozí toomatch starší verze chování.</span><span class="sxs-lookup"><span data-stu-id="fba88-108">hello ability toosearch for existing guest users in hello SharePoint Online (SPO) people picker is OFF by default toomatch legacy behavior.</span></span>

<span data-ttu-id="fba88-109">Tuto funkci můžete povolit pomocí nastavení 'ShowPeoplePickerSuggestionsForGuestUsers' v kolekci klienta a lokality hello úrovně hello.</span><span class="sxs-lookup"><span data-stu-id="fba88-109">You can enable this feature by using hello setting 'ShowPeoplePickerSuggestionsForGuestUsers' at hello tenant and site collection level.</span></span> <span data-ttu-id="fba88-110">Můžete nastavit pomocí Set-SPOTenant a Set-SPOSite hello rutin, které umožňuje členům funkce hello toosearch všechny existující uživatele typu Host v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="fba88-110">You can set hello feature using hello Set-SPOTenant and Set-SPOSite cmdlets, which allow members toosearch all existing guest users in hello directory.</span></span> <span data-ttu-id="fba88-111">Změny v obor klienta hello neovlivňují už zřízené SPO lokalit.</span><span class="sxs-lookup"><span data-stu-id="fba88-111">Changes in hello tenant scope do not affect already provisioned SPO sites.</span></span>

## <a name="invitations-have-been-disabled-for-directory"></a><span data-ttu-id="fba88-112">Pozvánek byla zakázána pro adresář</span><span class="sxs-lookup"><span data-stu-id="fba88-112">Invitations have been disabled for directory</span></span>

<span data-ttu-id="fba88-113">Pokud se upozornění, že nemáte oprávnění uživatelé tooinvite, ověřte, zda je váš uživatelský účet oprávnění tooinvite externí uživatele v části Nastavení uživatele:</span><span class="sxs-lookup"><span data-stu-id="fba88-113">If you are notified that you do not have permissions tooinvite users, verify that your user account is authorized tooinvite external users under User Settings:</span></span>

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

<span data-ttu-id="fba88-114">Pokud jste nedávno změnili tato nastavení nebo přiřazený hello pozvání hosta odeslal role tooa uživatel, může uběhnout 15 až 60 minut hello změny projeví.</span><span class="sxs-lookup"><span data-stu-id="fba88-114">If you have recently modified these settings or assigned hello Guest Inviter role tooa user, there might be a 15-60 minute delay before hello changes take effect.</span></span>

## <a name="hello-user-that-i-invited-is-receiving-an-error-during-redemption"></a><span data-ttu-id="fba88-115">Hello uživatele, který I pozvat přijímá chybu během uplatnění kódu.</span><span class="sxs-lookup"><span data-stu-id="fba88-115">hello user that I invited is receiving an error during redemption</span></span>

<span data-ttu-id="fba88-116">Běžné chyby patří:</span><span class="sxs-lookup"><span data-stu-id="fba88-116">Common errors include:</span></span>

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a><span data-ttu-id="fba88-117">Pozvaný správce zakázal EmailVerified uživatelé z vytváří v jejich klienta</span><span class="sxs-lookup"><span data-stu-id="fba88-117">Invitee’s Admin has disallowed EmailVerified Users from being created in their tenant</span></span>

<span data-ttu-id="fba88-118">Po vyzvání uživatele, jehož organizace používá Azure Active Directory, ale kde hello konkrétní uživatelský účet neexistuje (například hello uživatel neexistuje v doméně contoso.com Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fba88-118">When inviting users whose organization is using Azure Active Directory, but where hello specific user’s account does not exist (for example, hello user does not exist in Azure AD contoso.com).</span></span> <span data-ttu-id="fba88-119">Správce Hello contoso.com může mít zásadu na místě, které brání uživatelům v vytváří.</span><span class="sxs-lookup"><span data-stu-id="fba88-119">hello administrator of contoso.com may have a policy in place preventing users from being created.</span></span> <span data-ttu-id="fba88-120">Hello uživatel musí s jejich toodetermine správce zkontrolujte, zda externí uživatelé.</span><span class="sxs-lookup"><span data-stu-id="fba88-120">hello user must check with their admin toodetermine if external users are allowed.</span></span> <span data-ttu-id="fba88-121">Hello externího uživatele správce může být nutné tooallow ověřit e-mailu uživatelů v doméně (Toto [článku](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) na umožnit uživatelům ověřit e-mailu).</span><span class="sxs-lookup"><span data-stu-id="fba88-121">hello external user’s admin may need tooallow Email Verified users in their domain (see this [article](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) on allowing Email Verified Users).</span></span>

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a><span data-ttu-id="fba88-122">Externí uživatel neexistuje již ve federované domény</span><span class="sxs-lookup"><span data-stu-id="fba88-122">External user does not exist already in a federated domain</span></span>

<span data-ttu-id="fba88-123">Pokud používáte ověřování federace a hello uživatele v Azure Active Directory ještě neexistuje, nejde pozvat uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="fba88-123">If you are using federation authentication and hello user does not already exist in Azure Active Directory, hello user cannot be invited.</span></span>

<span data-ttu-id="fba88-124">tooresolve-li tento problém, hello externího uživatele správce musí synchronizovat tooAzure účet hello uživatele služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fba88-124">tooresolve this issue, hello external user’s admin must synchronize hello user’s account tooAzure Active Directory.</span></span>

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a><span data-ttu-id="fba88-125">Jak se nepodporuje\#', která obvykle není platný znak, synchronizace se službou Azure AD?</span><span class="sxs-lookup"><span data-stu-id="fba88-125">How does ‘\#’, which is not normally a valid character, sync with Azure AD?</span></span>

<span data-ttu-id="fba88-126">"\#" je vyhrazený znak v UPN pro spolupráci Azure AD B2B nebo externí uživatele, protože hello pozvat účet user@contoso.com stane user_contoso.com#EXT@fabrikam.onmicrosoft.com. Proto \# v UPN pocházejících z místní nejsou povoleny toosign v toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fba88-126">“\#” is a reserved character in UPNs for Azure AD B2B collaboration or external users, because hello invited account user@contoso.com becomes user_contoso.com#EXT@fabrikam.onmicrosoft.com. Therefore, \# in UPNs coming from on-premises aren't allowed toosign in toohello Azure portal.</span></span> 

## <a name="i-receive-an-error-when-adding-external-users-tooa-synchronized-group"></a><span data-ttu-id="fba88-127">Při přidávání externích uživatelů tooa synchronizovat skupiny se zobrazí chyba</span><span class="sxs-lookup"><span data-stu-id="fba88-127">I receive an error when adding external users tooa synchronized group</span></span>

<span data-ttu-id="fba88-128">Externí uživatele lze přidat pouze příliš "přiřazené" nebo "Zabezpečení" skupiny a ne toogroups, které jsou standardní místní.</span><span class="sxs-lookup"><span data-stu-id="fba88-128">External users can be added only too“assigned” or “Security” groups and not toogroups that are mastered on-premises.</span></span>

## <a name="my-external-user-did-not-receive-an-email-tooredeem"></a><span data-ttu-id="fba88-129">Moje externí uživatel neobdržel tooredeem e-mailu</span><span class="sxs-lookup"><span data-stu-id="fba88-129">My external user did not receive an email tooredeem</span></span>

<span data-ttu-id="fba88-130">Hello pozvaný potřeba zkontrolovat u svého poskytovatele internetových služeb nebo je povoleno tooensure filtr proti spamu, který hello následující adresy:Invites@microsoft.com</span><span class="sxs-lookup"><span data-stu-id="fba88-130">hello invitee should check with their ISP or spam filter tooensure that hello following address is allowed: Invites@microsoft.com</span></span>

## <a name="i-notice-that-hello-custom-message-does-not-get-included-with-invitation-messages-at-times"></a><span data-ttu-id="fba88-131">Všimli jsme si, že tento vlastní zprávu hello nezíská součástí pozvánku zprávy v některých případech</span><span class="sxs-lookup"><span data-stu-id="fba88-131">I notice that hello custom message does not get included with invitation messages at times</span></span>

<span data-ttu-id="fba88-132">toocomply s zákony o ochraně osobních údajů, rozhraní API nezahrnují vlastních zpráv v e-mailová pozvánka hello při:</span><span class="sxs-lookup"><span data-stu-id="fba88-132">toocomply with privacy laws, our APIs do not include custom messages in hello email invitation when:</span></span>

- <span data-ttu-id="fba88-133">Odesílatel Hello pozvánky nemá e-mailovou adresu v hello pozvání klienta</span><span class="sxs-lookup"><span data-stu-id="fba88-133">hello inviter doesn’t have an email address in hello inviting tenant</span></span>
- <span data-ttu-id="fba88-134">Když objektu zabezpečení služby App Service odešle Pozvánka hello</span><span class="sxs-lookup"><span data-stu-id="fba88-134">When an appservice principal sends hello invitation</span></span>

<span data-ttu-id="fba88-135">Pokud tento scénář je důležité tooyou, můžete potlačit e-mailová pozvánka naše API a odesílat prostřednictvím e-mailu mechanismus hello podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="fba88-135">If this scenario is important tooyou, you can suppress our API invitation email, and send it through hello email mechanism of your choice.</span></span> <span data-ttu-id="fba88-136">Podívejte se, že e-mailu, která posíláte tímto způsobem také odpovídá zákony o ochraně osobních údajů toomake právního zástupce vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="fba88-136">Consult your organization’s legal counsel toomake sure any email you send this way also complies with privacy laws.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fba88-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fba88-137">Next steps</span></span>

<span data-ttu-id="fba88-138">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="fba88-138">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="fba88-139">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="fba88-139">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="fba88-140">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="fba88-140">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="fba88-141">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="fba88-141">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="fba88-142">elementy Hello hello e-mailová pozvánka pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="fba88-142">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="fba88-143">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="fba88-143">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="fba88-144">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="fba88-144">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="fba88-145">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="fba88-145">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="fba88-146">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="fba88-146">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="fba88-147">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="fba88-147">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="fba88-148">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="fba88-148">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="fba88-149">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fba88-149">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
