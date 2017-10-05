---
title: "Podmíněný přístup pro uživatele spolupráce Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce Azure Active Directory s B2B podporuje vícefaktorové ověřování (MFA) pro výběrový přístup k podnikovým aplikacím"
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
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: d85f711d6551a68d1248ae8ec61e2ecc1ddc8ecd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a><span data-ttu-id="c8513-103">Podmíněný přístup pro uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-103">Conditional access for B2B collaboration users</span></span>

## <a name="multi-factor-authentication-for-b2b-users"></a><span data-ttu-id="c8513-104">Víceúrovňové ověřování pro uživatele s B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-104">Multi-factor authentication for B2B users</span></span>
<span data-ttu-id="c8513-105">S spolupráce Azure AD B2B organizace můžete vynutit zásady vícefaktorového ověřování (MFA) pro uživatele B2B.</span><span class="sxs-lookup"><span data-stu-id="c8513-105">With Azure AD B2B collaboration, organizations can enforce multi-factor authentication (MFA) policies for B2B users.</span></span> <span data-ttu-id="c8513-106">Tyto zásady lze vynutit na klienta, aplikace nebo na úrovni jednotlivých uživatelů, stejným způsobem, že jsou povoleny pro zaměstnance na plný úvazek a členy organizace.</span><span class="sxs-lookup"><span data-stu-id="c8513-106">These policies can be enforced at the tenant, app, or individual user level, the same way that they are enabled for full-time employees and members of the organization.</span></span> <span data-ttu-id="c8513-107">Zásady vícefaktorového ověřování se vynucují v organizaci poskytující prostředky.</span><span class="sxs-lookup"><span data-stu-id="c8513-107">MFA policies are enforced at the resource organization.</span></span>

<span data-ttu-id="c8513-108">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c8513-108">Example:</span></span>
1. <span data-ttu-id="c8513-109">Správce nebo informace pracovního procesu ve společnosti A vyzývá uživatele ze společnosti B k aplikaci *Foo* ve společnosti A.</span><span class="sxs-lookup"><span data-stu-id="c8513-109">Admin or information worker in Company A invites user from company B to an application *Foo* in company A.</span></span>
2. <span data-ttu-id="c8513-110">Aplikace *Foo* ve společnosti je nakonfigurován A aby se vyžadovalo při přístupu.</span><span class="sxs-lookup"><span data-stu-id="c8513-110">Application *Foo* in company A is configured to require MFA on access.</span></span>
3. <span data-ttu-id="c8513-111">Když uživatel ze společnosti B pokusí o přístup k aplikaci *Foo* ve společnosti klienta, zobrazí se výzva k dokončení výzvu vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="c8513-111">When the user from company B attempts to access app *Foo* in the company A tenant, they are asked to complete an MFA challenge.</span></span>
4. <span data-ttu-id="c8513-112">Uživatel můžete nastavit jejich MFA s společnosti A a vybere možnost jejich vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="c8513-112">The user can set up their MFA with company A, and chooses their MFA option.</span></span>
5. <span data-ttu-id="c8513-113">Tento scénář se dá použít pro všechny identity (Azure AD nebo účet spravované služby, například, pokud uživatelé ve společnosti B ověření pomocí sociálních ID)</span><span class="sxs-lookup"><span data-stu-id="c8513-113">This scenario works for any identity (Azure AD or MSA, for example, if users in Company B authenticate using social ID)</span></span>
6. <span data-ttu-id="c8513-114">Společnost A musí mít dostatek licencí Azure AD Premium, které podporují vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="c8513-114">Company A must have sufficient Premium Azure AD licenses that support MFA.</span></span> <span data-ttu-id="c8513-115">Uživatel ze společnosti B využívá tuto licenci od společnosti A.</span><span class="sxs-lookup"><span data-stu-id="c8513-115">The user from company B consumes this license from company A.</span></span>

<span data-ttu-id="c8513-116">Pozváním klientů je zodpovědná za vícefaktorového ověřování pro uživatele od organizace partnera vždy, i v případě, že partnerské organizace má možnosti vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="c8513-116">The inviting tenancy is always responsible for MFA for users from the partner organization, even if the partner organization has MFA capabilities.</span></span>

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a><span data-ttu-id="c8513-117">Nastavení vícefaktorového ověřování pro uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-117">Setting up MFA for B2B collaboration users</span></span>
<span data-ttu-id="c8513-118">Chcete-li zjistit, jak je snadné pro nastavení vícefaktorového ověřování pro uživatele spolupráce B2B, přečtěte si téma Jak v následujícím videu:</span><span class="sxs-lookup"><span data-stu-id="c8513-118">To discover how easy it is to set up MFA for B2B collaboration users, see how in the following video:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a><span data-ttu-id="c8513-119">MFA prostředí pro uživatele B2B nabízejí uplatnění kódu.</span><span class="sxs-lookup"><span data-stu-id="c8513-119">B2B users MFA experience for offer redemption</span></span>
<span data-ttu-id="c8513-120">Podívejte se na následující animace zobrazíte uplatnění prostředí:</span><span class="sxs-lookup"><span data-stu-id="c8513-120">Check out the following animation to see the redemption experience:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a><span data-ttu-id="c8513-121">MFA resetovat pro uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-121">MFA reset for B2B collaboration users</span></span>
<span data-ttu-id="c8513-122">V současné době správce můžete vyžadovat spolupráce B2B uživatelům ověření až znovu jen pomocí následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c8513-122">Currently, the admin can require B2B collaboration users to proof up again only by using the following PowerShell cmdlets:</span></span>

1. <span data-ttu-id="c8513-123">Připojení k Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8513-123">Connect to Azure AD</span></span>

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. <span data-ttu-id="c8513-124">Získání všech uživatelů s potvrzením až metody</span><span class="sxs-lookup"><span data-stu-id="c8513-124">Get all users with proof up methods</span></span>

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  <span data-ttu-id="c8513-125">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="c8513-125">Here is an example:</span></span>

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. <span data-ttu-id="c8513-126">Reset – metoda MFA pro konkrétního uživatele, aby vyžadovala od uživatele spolupráce B2B se znovu nastavit metody výš.</span><span class="sxs-lookup"><span data-stu-id="c8513-126">Reset the MFA method for a specific user to require the B2B collaboration user to set proof-up methods again.</span></span> <span data-ttu-id="c8513-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c8513-127">Example:</span></span>

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a><span data-ttu-id="c8513-128">Proč jsme provedu MFA v klientů prostředků?</span><span class="sxs-lookup"><span data-stu-id="c8513-128">Why do we perform MFA at the resource tenancy?</span></span>

<span data-ttu-id="c8513-129">V aktuální verzi MFA je vždy v prostředku klientů, z důvodů předvídatelnost.</span><span class="sxs-lookup"><span data-stu-id="c8513-129">In the current release, MFA is always in the resource tenancy, for reasons of predictability.</span></span> <span data-ttu-id="c8513-130">Řekněme například, uživatel Contoso (Jan) vyzván k Fabrikam a Fabrikam se povolit MFA pro uživatele B2B.</span><span class="sxs-lookup"><span data-stu-id="c8513-130">For example, let’s say a Contoso user (Sally) is invited to Fabrikam and Fabrikam has enabled MFA for B2B users.</span></span>

<span data-ttu-id="c8513-131">Pokud společnosti Contoso má povolenou zásadu vícefaktorového ověřování pro App1, ale není na počítači App2, pak pokud se podíváme na Contoso MFA deklarace identity v tokenu, jsme setkat s následujícím problémem:</span><span class="sxs-lookup"><span data-stu-id="c8513-131">If Contoso has MFA policy enabled for App1 but not App2, then if we look at the Contoso MFA claim in the token, we might see the following issue:</span></span>

* <span data-ttu-id="c8513-132">Den 1: Uživatel má MFA ve společnosti Contoso a přistupuje k počítači App1 a pak žádné další MFA řádku se zobrazí ve firmě Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="c8513-132">Day 1: A user has MFA in Contoso and is accessing App1, then no additional MFA prompt is shown in Fabrikam.</span></span>

* <span data-ttu-id="c8513-133">Den 2: Uživatel přistupoval 2 aplikace ve společnosti Contoso, takže teď při přístupu k Fabrikam, se musí v MFA zaregistrovat existuje.</span><span class="sxs-lookup"><span data-stu-id="c8513-133">Day 2: The user has accessed App 2 in Contoso, so now when accessing Fabrikam, they must register for MFA there.</span></span>

<span data-ttu-id="c8513-134">Tento proces může být matoucí a může vést k vyřadit v dokončených přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c8513-134">This process can be confusing and could lead to drop in sign-in completions.</span></span>

<span data-ttu-id="c8513-135">Kromě toho i v případě, že Contoso má schopnost MFA, není vždy, že tento případ Fabrikam by důvěřovat zásad vícefaktorového ověřování Contoso.</span><span class="sxs-lookup"><span data-stu-id="c8513-135">Moreover, even if Contoso has MFA capability, it is not always the case the Fabrikam would trust the Contoso MFA policy.</span></span>

<span data-ttu-id="c8513-136">Nakonec prostředků klienta MFA funguje taky pro účty spravované služby a sociálních ID a orgs partnera, které nemají MFA nastavit.</span><span class="sxs-lookup"><span data-stu-id="c8513-136">Finally, resource tenant MFA also works for MSAs and social IDs and for partner orgs that do not have MFA set up.</span></span>

<span data-ttu-id="c8513-137">Doporučení pro MFA pro uživatele B2B je proto vždy vyžadovat vícefaktorové ověřování v pozváním klientovi.</span><span class="sxs-lookup"><span data-stu-id="c8513-137">Therefore, the recommendation for MFA for B2B users is to always require MFA in the inviting tenant.</span></span> <span data-ttu-id="c8513-138">Tento požadavek může vést k dvojité vícefaktorového ověřování v některých případech, ale vždy, když přistupují k pozváním klienta, se koncovým uživatelům prostředí předvídatelný: Jan musí v MFA zaregistrovat s pozváním klienta.</span><span class="sxs-lookup"><span data-stu-id="c8513-138">This requirement could lead to double MFA in some cases, but whenever accessing the inviting tenant, the end-users experience is predictable: Sally must register for MFA with the inviting tenant.</span></span>

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a><span data-ttu-id="c8513-139">Na zařízení, na základě polohy a na základě riziko podmíněného přístupu pro uživatele s B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-139">Device-based, location-based, and risk-based conditional access for B2B users</span></span>

<span data-ttu-id="c8513-140">Pokud Contoso povolí zásady podmíněného přístupu na základě zařízení pro svoje firemní data, je přístup zabránila zařízení, která nejsou spravovaná ve společnosti Contoso a není kompatibilní se zásadami společnosti Contoso zařízení.</span><span class="sxs-lookup"><span data-stu-id="c8513-140">When Contoso enables device-based conditional access policies for their corporate data, access is prevented from devices that are not managed by Contoso and not compliant with the Contoso device policies.</span></span>

<span data-ttu-id="c8513-141">Pokud uživatel B2B zařízení nespravuje ve společnosti Contoso, zablokuje se přístup B2B uživatelů z partnerských organizací v jakémkoli kontextu tyto zásady se vynucují.</span><span class="sxs-lookup"><span data-stu-id="c8513-141">If the B2B user’s device isn't managed by Contoso, access of B2B users from the partner organizations is blocked in whatever context these policies are enforced.</span></span> <span data-ttu-id="c8513-142">Contoso však můžete vytvořit seznamy vyloučení obsahující konkrétní partnerskou uživatele z nich vyloučit ze zásad podmíněného přístupu podle zařízení.</span><span class="sxs-lookup"><span data-stu-id="c8513-142">However, Contoso can create exclusion lists containing specific partner users to exclude them from the device-based conditional access policy.</span></span>

#### <a name="location-based-conditional-access-for-b2b"></a><span data-ttu-id="c8513-143">Na základě polohy podmíněný přístup pro B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-143">Location-based conditional access for B2B</span></span>

<span data-ttu-id="c8513-144">Zásady podmíněného přístupu na základě umístění můžete vynutit pro B2B uživatele, pokud pozváním organizace je možné vytvořit důvěryhodný rozsah IP adres, který definuje jejich organizace partnera.</span><span class="sxs-lookup"><span data-stu-id="c8513-144">Location-based conditional access policies can be enforced for B2B users if the inviting organization is able to create a trusted IP address range that defines their partner organizations.</span></span>

#### <a name="risk-based-conditional-access-for-b2b"></a><span data-ttu-id="c8513-145">Podmíněný přístup využívající riziko pro B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-145">Risk-based conditional access for B2B</span></span>

<span data-ttu-id="c8513-146">V současné době přihlášení zásad založených na riziko nelze použít uživatelům B2B, protože vyhodnocení rizik se provádí na domovskou organizaci B2B uživatele.</span><span class="sxs-lookup"><span data-stu-id="c8513-146">Currently, risk-based sign-in policies cannot be applied to B2B users because the risk evaluation is performed at the B2B user’s home organization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8513-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8513-147">Next steps</span></span>

<span data-ttu-id="c8513-148">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="c8513-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="c8513-149">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="c8513-149">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="c8513-150">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="c8513-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="c8513-151">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="c8513-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="c8513-152">Elementy e-mail pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-152">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="c8513-153">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-153">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="c8513-154">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="c8513-154">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="c8513-155">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="c8513-155">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="c8513-156">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="c8513-156">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="c8513-157">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="c8513-157">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="c8513-158">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="c8513-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="c8513-159">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c8513-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
