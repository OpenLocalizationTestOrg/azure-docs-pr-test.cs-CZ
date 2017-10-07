---
title: "aaaConditional přístup pro uživatele spolupráce Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce Azure Active Directory s B2B podporuje vícefaktorové ověřování (MFA) pro podnikové aplikace tooyour výběrový přístup"
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
ms.openlocfilehash: 3a05be4393f74ff8e87f32432a222a5fbac9af62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a><span data-ttu-id="ad52f-103">Podmíněný přístup pro uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-103">Conditional access for B2B collaboration users</span></span>

## <a name="multi-factor-authentication-for-b2b-users"></a><span data-ttu-id="ad52f-104">Víceúrovňové ověřování pro uživatele s B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-104">Multi-factor authentication for B2B users</span></span>
<span data-ttu-id="ad52f-105">S spolupráce Azure AD B2B organizace můžete vynutit zásady vícefaktorového ověřování (MFA) pro uživatele B2B.</span><span class="sxs-lookup"><span data-stu-id="ad52f-105">With Azure AD B2B collaboration, organizations can enforce multi-factor authentication (MFA) policies for B2B users.</span></span> <span data-ttu-id="ad52f-106">Tyto zásady je možné vynutit na hello klienta, aplikace nebo na úrovni jednotlivých uživatelských hello stejným způsobem, že jsou povoleny pro zaměstnance na plný úvazek a členy hello organizace.</span><span class="sxs-lookup"><span data-stu-id="ad52f-106">These policies can be enforced at hello tenant, app, or individual user level, hello same way that they are enabled for full-time employees and members of hello organization.</span></span> <span data-ttu-id="ad52f-107">Zásady vícefaktorového ověřování se vynucují v organizaci poskytující prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="ad52f-107">MFA policies are enforced at hello resource organization.</span></span>

<span data-ttu-id="ad52f-108">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ad52f-108">Example:</span></span>
1. <span data-ttu-id="ad52f-109">Správce nebo informace pracovního procesu ve společnosti A vyzývá uživatele z aplikace společnosti B tooan *Foo* ve společnosti A.</span><span class="sxs-lookup"><span data-stu-id="ad52f-109">Admin or information worker in Company A invites user from company B tooan application *Foo* in company A.</span></span>
2. <span data-ttu-id="ad52f-110">Aplikace *Foo* ve společnosti A je nakonfigurovaný toorequire MFA na přístup.</span><span class="sxs-lookup"><span data-stu-id="ad52f-110">Application *Foo* in company A is configured toorequire MFA on access.</span></span>
3. <span data-ttu-id="ad52f-111">Při pokusu uživatele hello od společnosti B tooaccess aplikace *Foo* ve společnosti hello klienta, budou vyzváni toocomplete výzvu vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="ad52f-111">When hello user from company B attempts tooaccess app *Foo* in hello company A tenant, they are asked toocomplete an MFA challenge.</span></span>
4. <span data-ttu-id="ad52f-112">uživatel Hello můžete nastavit jejich MFA s společnosti A a vybere možnost jejich vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="ad52f-112">hello user can set up their MFA with company A, and chooses their MFA option.</span></span>
5. <span data-ttu-id="ad52f-113">Tento scénář se dá použít pro všechny identity (Azure AD nebo účet spravované služby, například, pokud uživatelé ve společnosti B ověření pomocí sociálních ID)</span><span class="sxs-lookup"><span data-stu-id="ad52f-113">This scenario works for any identity (Azure AD or MSA, for example, if users in Company B authenticate using social ID)</span></span>
6. <span data-ttu-id="ad52f-114">Společnost A musí mít dostatek licencí Azure AD Premium, které podporují vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="ad52f-114">Company A must have sufficient Premium Azure AD licenses that support MFA.</span></span> <span data-ttu-id="ad52f-115">Hello uživatel ze společnosti B spotřebuje tuto licenci od společnosti A.</span><span class="sxs-lookup"><span data-stu-id="ad52f-115">hello user from company B consumes this license from company A.</span></span>

<span data-ttu-id="ad52f-116">Hello pozváním klientů je zodpovědná za vícefaktorového ověřování pro uživatele z hello partnerské organizaci, vždy i v případě, že organizace partnera hello má možnosti vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="ad52f-116">hello inviting tenancy is always responsible for MFA for users from hello partner organization, even if hello partner organization has MFA capabilities.</span></span>

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a><span data-ttu-id="ad52f-117">Nastavení vícefaktorového ověřování pro uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-117">Setting up MFA for B2B collaboration users</span></span>
<span data-ttu-id="ad52f-118">toodiscover jak snadné je tooset vícefaktorového ověřování pro službu pro uživatele spolupráce B2B, najdete v části Jak v hello následující video:</span><span class="sxs-lookup"><span data-stu-id="ad52f-118">toodiscover how easy it is tooset up MFA for B2B collaboration users, see how in hello following video:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a><span data-ttu-id="ad52f-119">MFA prostředí pro uživatele B2B nabízejí uplatnění kódu.</span><span class="sxs-lookup"><span data-stu-id="ad52f-119">B2B users MFA experience for offer redemption</span></span>
<span data-ttu-id="ad52f-120">Podívejte se na následující animace toosee hello uplatnění prostředí hello:</span><span class="sxs-lookup"><span data-stu-id="ad52f-120">Check out hello following animation toosee hello redemption experience:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a><span data-ttu-id="ad52f-121">MFA resetovat pro uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-121">MFA reset for B2B collaboration users</span></span>
<span data-ttu-id="ad52f-122">V současné době Dobrý den, Správce může vyžadovat tooproof uživatelé spolupráce B2B až znovu pouze pomocí hello následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ad52f-122">Currently, hello admin can require B2B collaboration users tooproof up again only by using hello following PowerShell cmdlets:</span></span>

1. <span data-ttu-id="ad52f-123">Připojit tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="ad52f-123">Connect tooAzure AD</span></span>

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. <span data-ttu-id="ad52f-124">Získání všech uživatelů s potvrzením až metody</span><span class="sxs-lookup"><span data-stu-id="ad52f-124">Get all users with proof up methods</span></span>

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  <span data-ttu-id="ad52f-125">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="ad52f-125">Here is an example:</span></span>

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. <span data-ttu-id="ad52f-126">Reset – metoda MFA hello konkrétního uživatele toorequire hello B2B spolupráce uživatele tooset výš metod znovu.</span><span class="sxs-lookup"><span data-stu-id="ad52f-126">Reset hello MFA method for a specific user toorequire hello B2B collaboration user tooset proof-up methods again.</span></span> <span data-ttu-id="ad52f-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ad52f-127">Example:</span></span>

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-hello-resource-tenancy"></a><span data-ttu-id="ad52f-128">Proč jsme provedu MFA v hello prostředků klientů?</span><span class="sxs-lookup"><span data-stu-id="ad52f-128">Why do we perform MFA at hello resource tenancy?</span></span>

<span data-ttu-id="ad52f-129">V aktuální verzi hello MFA je vždy v hello prostředků klientů, z důvodů předvídatelnost.</span><span class="sxs-lookup"><span data-stu-id="ad52f-129">In hello current release, MFA is always in hello resource tenancy, for reasons of predictability.</span></span> <span data-ttu-id="ad52f-130">Předpokládejme například že pozvané tooFabrikam je uživatel Contoso (Jan) a Fabrikam se povolit MFA pro uživatele B2B.</span><span class="sxs-lookup"><span data-stu-id="ad52f-130">For example, let’s say a Contoso user (Sally) is invited tooFabrikam and Fabrikam has enabled MFA for B2B users.</span></span>

<span data-ttu-id="ad52f-131">Pokud společnosti Contoso má povolenou zásadu vícefaktorového ověřování pro App1, ale není na počítači App2, pak pokud se podíváme na hello deklarace identity společnosti Contoso MFA v tokenu hello jsme může se zobrazit hello následující potíže:</span><span class="sxs-lookup"><span data-stu-id="ad52f-131">If Contoso has MFA policy enabled for App1 but not App2, then if we look at hello Contoso MFA claim in hello token, we might see hello following issue:</span></span>

* <span data-ttu-id="ad52f-132">Den 1: Uživatel má MFA ve společnosti Contoso a přistupuje k počítači App1 a pak žádné další MFA řádku se zobrazí ve firmě Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="ad52f-132">Day 1: A user has MFA in Contoso and is accessing App1, then no additional MFA prompt is shown in Fabrikam.</span></span>

* <span data-ttu-id="ad52f-133">Den 2: hello přístup uživatelů k aplikaci 2 ve společnosti Contoso, takže teď při přístupu k Fabrikam, se musí v MFA zaregistrovat existuje.</span><span class="sxs-lookup"><span data-stu-id="ad52f-133">Day 2: hello user has accessed App 2 in Contoso, so now when accessing Fabrikam, they must register for MFA there.</span></span>

<span data-ttu-id="ad52f-134">Tento proces může být matoucí a může vést toodrop v dokončených přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ad52f-134">This process can be confusing and could lead toodrop in sign-in completions.</span></span>

<span data-ttu-id="ad52f-135">Kromě toho i v případě, že Contoso má schopnost MFA, jeho není vždy hello případu hello Fabrikam by důvěřovat hello zásad vícefaktorového ověřování Contoso.</span><span class="sxs-lookup"><span data-stu-id="ad52f-135">Moreover, even if Contoso has MFA capability, it is not always hello case hello Fabrikam would trust hello Contoso MFA policy.</span></span>

<span data-ttu-id="ad52f-136">Nakonec prostředků klienta MFA funguje taky pro účty spravované služby a sociálních ID a orgs partnera, které nemají MFA nastavit.</span><span class="sxs-lookup"><span data-stu-id="ad52f-136">Finally, resource tenant MFA also works for MSAs and social IDs and for partner orgs that do not have MFA set up.</span></span>

<span data-ttu-id="ad52f-137">Hello doporučení pro MFA pro uživatele B2B je proto, že tooalways vyžadovat vícefaktorové ověřování v hello pozvání klienta.</span><span class="sxs-lookup"><span data-stu-id="ad52f-137">Therefore, hello recommendation for MFA for B2B users is tooalways require MFA in hello inviting tenant.</span></span> <span data-ttu-id="ad52f-138">Tento požadavek, mohla způsobit toodouble vícefaktorového ověřování v některých případech, ale vždy, když přistupují k hello pozváním klienta, je předvídatelný hello koncovým uživatelům prostředí: Jan musí v MFA zaregistrovat s pozváním klienta hello.</span><span class="sxs-lookup"><span data-stu-id="ad52f-138">This requirement could lead toodouble MFA in some cases, but whenever accessing hello inviting tenant, hello end-users experience is predictable: Sally must register for MFA with hello inviting tenant.</span></span>

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a><span data-ttu-id="ad52f-139">Na zařízení, na základě polohy a na základě riziko podmíněného přístupu pro uživatele s B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-139">Device-based, location-based, and risk-based conditional access for B2B users</span></span>

<span data-ttu-id="ad52f-140">Pokud Contoso povolí zásady podmíněného přístupu na základě zařízení pro svoje firemní data, je přístup zabránila zařízení, která se nespravují ve společnosti Contoso a nejsou kompatibilní s zásady zařízení Contoso hello.</span><span class="sxs-lookup"><span data-stu-id="ad52f-140">When Contoso enables device-based conditional access policies for their corporate data, access is prevented from devices that are not managed by Contoso and not compliant with hello Contoso device policies.</span></span>

<span data-ttu-id="ad52f-141">Pokud uživatel hello B2B zařízení nespravuje ve společnosti Contoso, zablokuje se přístup B2B uživatelů z partnerských organizací hello v jakémkoli kontextu tyto zásady se vynucují.</span><span class="sxs-lookup"><span data-stu-id="ad52f-141">If hello B2B user’s device isn't managed by Contoso, access of B2B users from hello partner organizations is blocked in whatever context these policies are enforced.</span></span> <span data-ttu-id="ad52f-142">Contoso však můžete vytvořit vyloučení seznamy obsahující konkrétní partnerskou uživatelé tooexclude je z hello zásady podmíněného přístupu podle zařízení.</span><span class="sxs-lookup"><span data-stu-id="ad52f-142">However, Contoso can create exclusion lists containing specific partner users tooexclude them from hello device-based conditional access policy.</span></span>

#### <a name="location-based-conditional-access-for-b2b"></a><span data-ttu-id="ad52f-143">Na základě polohy podmíněný přístup pro B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-143">Location-based conditional access for B2B</span></span>

<span data-ttu-id="ad52f-144">Zásady podmíněného přístupu na základě umístění můžete vynutit pro B2B uživatele, pokud hello pozváním organizace je možné toocreate důvěryhodné rozsah IP adres, který definuje jejich organizace partnera.</span><span class="sxs-lookup"><span data-stu-id="ad52f-144">Location-based conditional access policies can be enforced for B2B users if hello inviting organization is able toocreate a trusted IP address range that defines their partner organizations.</span></span>

#### <a name="risk-based-conditional-access-for-b2b"></a><span data-ttu-id="ad52f-145">Podmíněný přístup využívající riziko pro B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-145">Risk-based conditional access for B2B</span></span>

<span data-ttu-id="ad52f-146">V současné době přihlášení zásad založených na riziko nemůže být použité tooB2B uživatele, protože vyhodnocení rizik hello se provádí na domovskou organizaci hello B2B uživatele.</span><span class="sxs-lookup"><span data-stu-id="ad52f-146">Currently, risk-based sign-in policies cannot be applied tooB2B users because hello risk evaluation is performed at hello B2B user’s home organization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad52f-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad52f-147">Next steps</span></span>

<span data-ttu-id="ad52f-148">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="ad52f-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="ad52f-149">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="ad52f-149">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="ad52f-150">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="ad52f-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="ad52f-151">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="ad52f-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="ad52f-152">elementy Hello hello e-mailová pozvánka pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-152">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="ad52f-153">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-153">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="ad52f-154">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="ad52f-154">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="ad52f-155">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="ad52f-155">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="ad52f-156">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="ad52f-156">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="ad52f-157">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="ad52f-157">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="ad52f-158">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="ad52f-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="ad52f-159">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ad52f-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
