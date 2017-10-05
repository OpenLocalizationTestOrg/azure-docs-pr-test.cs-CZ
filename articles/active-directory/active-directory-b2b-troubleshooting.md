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
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="7dbb0-103">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="7dbb0-103">Troubleshooting Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="7dbb0-104">Tady jsou některé náhrad pro běžné problémy s spolupráce B2B Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7dbb0-104">Here are some remedies for common problems with Azure Active Directory (Azure AD) B2B collaboration.</span></span>


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a><span data-ttu-id="7dbb0-105">Přidali jste externího uživatele, ale neviděli v globálním adresáři nebo v dialogu pro výběr osob</span><span class="sxs-lookup"><span data-stu-id="7dbb0-105">I’ve added an external user but do not see them in my Global Address Book or in the people picker</span></span>

<span data-ttu-id="7dbb0-106">V případech, kde nejsou naplněny externí uživatele v seznamu objekt může trvat několik minut k replikaci.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-106">In cases where external users are not populated in the list, the object might take a few minutes to replicate.</span></span>

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a><span data-ttu-id="7dbb0-107">Uživatel guest B2B není zobrazovat na výběr SharePoint Online nebo OneDrive osoby.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-107">A B2B guest user is not showing up in SharePoint Online/OneDrive people picker</span></span> 
 
<span data-ttu-id="7dbb0-108">Umožňuje vyhledat existující uživatele typu Host v dialogu pro výběr osob SharePoint Online (SPO) je ve výchozím nastavení tak, aby odpovídaly starší verze chování OFF.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-108">The ability to search for existing guest users in the SharePoint Online (SPO) people picker is OFF by default to match legacy behavior.</span></span>

<span data-ttu-id="7dbb0-109">Tuto funkci můžete povolit pomocí nastavení 'ShowPeoplePickerSuggestionsForGuestUsers' na úrovni kolekce klienta a lokality.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-109">You can enable this feature by using the setting 'ShowPeoplePickerSuggestionsForGuestUsers' at the tenant and site collection level.</span></span> <span data-ttu-id="7dbb0-110">Můžete nastavit funkci pomocí rutiny Set-SPOTenant a Set-SPOSite, díky členy pro vyhledávání v adresáři všechny existující uživatele typu Host.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-110">You can set the feature using the Set-SPOTenant and Set-SPOSite cmdlets, which allow members to search all existing guest users in the directory.</span></span> <span data-ttu-id="7dbb0-111">Změny v oboru klienta neovlivní už zřízené SPO lokalit.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-111">Changes in the tenant scope do not affect already provisioned SPO sites.</span></span>

## <a name="invitations-have-been-disabled-for-directory"></a><span data-ttu-id="7dbb0-112">Pozvánek byla zakázána pro adresář</span><span class="sxs-lookup"><span data-stu-id="7dbb0-112">Invitations have been disabled for directory</span></span>

<span data-ttu-id="7dbb0-113">Pokud se upozornění, že nemáte oprávnění pro uživatele pozvat, ověřte, že je váš uživatelský účet oprávnění pro externí uživatele v části Nastavení uživatele pozvat:</span><span class="sxs-lookup"><span data-stu-id="7dbb0-113">If you are notified that you do not have permissions to invite users, verify that your user account is authorized to invite external users under User Settings:</span></span>

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

<span data-ttu-id="7dbb0-114">Pokud jste nedávno změnili tato nastavení nebo přiřazenou roli pozvání hosta odeslal na uživatele, může uběhnout 15 až 60 minut před změny se projeví.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-114">If you have recently modified these settings or assigned the Guest Inviter role to a user, there might be a 15-60 minute delay before the changes take effect.</span></span>

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a><span data-ttu-id="7dbb0-115">Uživatel, který I pozvat přijímá chybu během uplatnění kódu.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-115">The user that I invited is receiving an error during redemption</span></span>

<span data-ttu-id="7dbb0-116">Běžné chyby patří:</span><span class="sxs-lookup"><span data-stu-id="7dbb0-116">Common errors include:</span></span>

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a><span data-ttu-id="7dbb0-117">Pozvaný správce zakázal EmailVerified uživatelé z vytváří v jejich klienta</span><span class="sxs-lookup"><span data-stu-id="7dbb0-117">Invitee’s Admin has disallowed EmailVerified Users from being created in their tenant</span></span>

<span data-ttu-id="7dbb0-118">Když pozvat uživatele, jehož organizace používá Azure Active Directory, ale kde konkrétní uživatelský účet neexistuje (například uživatel neexistuje v doméně contoso.com Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7dbb0-118">When inviting users whose organization is using Azure Active Directory, but where the specific user’s account does not exist (for example, the user does not exist in Azure AD contoso.com).</span></span> <span data-ttu-id="7dbb0-119">Správce contoso.com může mít zásadu na místě, které brání uživatelům v vytváří.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-119">The administrator of contoso.com may have a policy in place preventing users from being created.</span></span> <span data-ttu-id="7dbb0-120">Uživatel musí zkontrolovat u jejich správce k určení, zda jsou externí uživatele povolen.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-120">The user must check with their admin to determine if external users are allowed.</span></span> <span data-ttu-id="7dbb0-121">Správce externího uživatele možná muset povolit ověřit e-mailu uživatelům ve své doméně (Toto [článku](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) na umožnit uživatelům ověřit e-mailu).</span><span class="sxs-lookup"><span data-stu-id="7dbb0-121">The external user’s admin may need to allow Email Verified users in their domain (see this [article](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) on allowing Email Verified Users).</span></span>

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a><span data-ttu-id="7dbb0-122">Externí uživatel neexistuje již ve federované domény</span><span class="sxs-lookup"><span data-stu-id="7dbb0-122">External user does not exist already in a federated domain</span></span>

<span data-ttu-id="7dbb0-123">Pokud používáte ověřování federace a uživatele ve službě Azure Active Directory ještě neexistuje, nejde pozvat uživatele.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-123">If you are using federation authentication and the user does not already exist in Azure Active Directory, the user cannot be invited.</span></span>

<span data-ttu-id="7dbb0-124">Chcete-li tento problém vyřešit, musí správce externího uživatele synchronizovat uživatelského účtu do služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-124">To resolve this issue, the external user’s admin must synchronize the user’s account to Azure Active Directory.</span></span>

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a><span data-ttu-id="7dbb0-125">Jak se nepodporuje\#', která obvykle není platný znak, synchronizace se službou Azure AD?</span><span class="sxs-lookup"><span data-stu-id="7dbb0-125">How does ‘\#’, which is not normally a valid character, sync with Azure AD?</span></span>

<span data-ttu-id="7dbb0-126">"\#" je vyhrazený znak v UPN pro spolupráci Azure AD B2B nebo externí uživatele, protože pozvané účet user@contoso.com stane user_contoso.com#EXT@fabrikam.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-126">“\#” is a reserved character in UPNs for Azure AD B2B collaboration or external users, because the invited account user@contoso.com becomes user_contoso.com#EXT@fabrikam.onmicrosoft.com.</span></span> <span data-ttu-id="7dbb0-127">Proto \# v UPN pocházejících z místní nejsou povoleny pro přihlášení k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-127">Therefore, \# in UPNs coming from on-premises aren't allowed to sign in to the Azure portal.</span></span> 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a><span data-ttu-id="7dbb0-128">Při přidávání externích uživatelů do skupiny synchronizované se zobrazí chyba</span><span class="sxs-lookup"><span data-stu-id="7dbb0-128">I receive an error when adding external users to a synchronized group</span></span>

<span data-ttu-id="7dbb0-129">Externí uživatelé lze přidat pouze do "přiřazené" nebo "Zabezpečení" skupiny a ne do skupiny, které jsou standardní místní.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-129">External users can be added only to “assigned” or “Security” groups and not to groups that are mastered on-premises.</span></span>

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a><span data-ttu-id="7dbb0-130">Moje externí uživatel neobdržel uplatnit e-mailu</span><span class="sxs-lookup"><span data-stu-id="7dbb0-130">My external user did not receive an email to redeem</span></span>

<span data-ttu-id="7dbb0-131">Pozvané potřeba zkontrolovat u svého poskytovatele internetových služeb nebo nevyžádané pošty filtru zajistit, že je povolené následující adresy:Invites@microsoft.com</span><span class="sxs-lookup"><span data-stu-id="7dbb0-131">The invitee should check with their ISP or spam filter to ensure that the following address is allowed: Invites@microsoft.com</span></span>

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a><span data-ttu-id="7dbb0-132">Všimli jsme si, že vlastní zprávu nezíská součástí pozvánku zprávy v některých případech</span><span class="sxs-lookup"><span data-stu-id="7dbb0-132">I notice that the custom message does not get included with invitation messages at times</span></span>

<span data-ttu-id="7dbb0-133">Abyste dosáhli souladu s zákony o ochraně osobních údajů, rozhraní API nezahrnují vlastních zpráv v e-mailová pozvánka při:</span><span class="sxs-lookup"><span data-stu-id="7dbb0-133">To comply with privacy laws, our APIs do not include custom messages in the email invitation when:</span></span>

- <span data-ttu-id="7dbb0-134">Pozvání odeslal nemá e-mailovou adresu v pozváním klienta</span><span class="sxs-lookup"><span data-stu-id="7dbb0-134">The inviter doesn’t have an email address in the inviting tenant</span></span>
- <span data-ttu-id="7dbb0-135">Když objektu zabezpečení služby App Service odešle pozvánku</span><span class="sxs-lookup"><span data-stu-id="7dbb0-135">When an appservice principal sends the invitation</span></span>

<span data-ttu-id="7dbb0-136">Pokud tento scénář je pro vás důležité, můžete potlačit e-mailová pozvánka naše API a odeslat e-mailu mechanismem podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-136">If this scenario is important to you, you can suppress our API invitation email, and send it through the email mechanism of your choice.</span></span> <span data-ttu-id="7dbb0-137">Poraďte vaší organizace právního zástupce a ujistěte se všechny e-mailu, že odesílat, že tak také odpovídá zákony o ochraně osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="7dbb0-137">Consult your organization’s legal counsel to make sure any email you send this way also complies with privacy laws.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7dbb0-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7dbb0-138">Next steps</span></span>

<span data-ttu-id="7dbb0-139">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="7dbb0-139">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="7dbb0-140">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="7dbb0-140">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="7dbb0-141">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="7dbb0-141">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="7dbb0-142">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="7dbb0-142">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="7dbb0-143">Elementy e-mail pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="7dbb0-143">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="7dbb0-144">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="7dbb0-144">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="7dbb0-145">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="7dbb0-145">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="7dbb0-146">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="7dbb0-146">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="7dbb0-147">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="7dbb0-147">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="7dbb0-148">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="7dbb0-148">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="7dbb0-149">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="7dbb0-149">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="7dbb0-150">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7dbb0-150">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
