---
title: "aaaSign v prostředí s Azure AD Identity Protection | Microsoft Docs"
description: "Obsahuje přehled hello uživatelské prostředí, když má omezeny nebo opraven uživatele nebo když je službu Multi-Factor authentication požadované zásady ochrany identit."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: fbdca5b86ed93d0a2f2b6df1dd0150da9c0c85c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a><span data-ttu-id="c7b02-104">Možnosti přihlášení s Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="c7b02-104">Sign-in experiences with Azure AD Identity Protection</span></span>
<span data-ttu-id="c7b02-105">S Azure Active Directory Identity Protection můžete:</span><span class="sxs-lookup"><span data-stu-id="c7b02-105">With Azure Active Directory Identity Protection, you can:</span></span>

* <span data-ttu-id="c7b02-106">vyžadovat tooregister uživatele u služby Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="c7b02-106">require users tooregister for multi-factor authentication</span></span>
* <span data-ttu-id="c7b02-107">zpracování rizikové přihlášení a ohrožení zabezpečení uživatelů</span><span class="sxs-lookup"><span data-stu-id="c7b02-107">handle risky sign-ins and compromised users</span></span>

<span data-ttu-id="c7b02-108">vzhledem k tomu, že právě přímo přihlášení zadáním uživatelského jména a hesla nebude možné už Hello odpověď problémy toothese hello systému má vliv na uživatele přihlašování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c7b02-108">hello response of hello system toothese issues has an impact on a user's sign-in experience because just directly signing-in by providing a user name and a password won't be possible anymore.</span></span> <span data-ttu-id="c7b02-109">Další kroky jsou požadované tooget uživatele bezpečně zpět do firmy.</span><span class="sxs-lookup"><span data-stu-id="c7b02-109">Additional steps are required tooget a user safely back into business.</span></span>

<span data-ttu-id="c7b02-110">Toto téma poskytuje přehled možností přihlašování uživatele pro všechny případy, které se můžou vyskytnout.</span><span class="sxs-lookup"><span data-stu-id="c7b02-110">This topic gives you an overview of a user's sign-in experience for all cases that can occur.</span></span>

<span data-ttu-id="c7b02-111">**Multi-Factor Authentication**</span><span class="sxs-lookup"><span data-stu-id="c7b02-111">**Multi-factor authentication**</span></span>

* <span data-ttu-id="c7b02-112">Registrace služby Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="c7b02-112">Multi-factor authentication registration</span></span>

<span data-ttu-id="c7b02-113">**Přihlášení v ohrožení**</span><span class="sxs-lookup"><span data-stu-id="c7b02-113">**Sign-in at risk**</span></span>

* <span data-ttu-id="c7b02-114">Obnovení rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="c7b02-114">Risky sign-in recovery</span></span>
* <span data-ttu-id="c7b02-115">Rizikové přihlášení blokován</span><span class="sxs-lookup"><span data-stu-id="c7b02-115">Risky sign-in blocked</span></span>
* <span data-ttu-id="c7b02-116">Registrace služby Multi-Factor authentication během rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="c7b02-116">Multi-factor authentication registration during a risky sign-in</span></span>

<span data-ttu-id="c7b02-117">**Uživatel v ohrožení**</span><span class="sxs-lookup"><span data-stu-id="c7b02-117">**User at risk**</span></span>

* <span data-ttu-id="c7b02-118">Obnovení ohrožení bezpečnosti účtu</span><span class="sxs-lookup"><span data-stu-id="c7b02-118">Compromised account recovery</span></span>
* <span data-ttu-id="c7b02-119">Ohrožení bezpečnosti účtu blokován</span><span class="sxs-lookup"><span data-stu-id="c7b02-119">Compromised account blocked</span></span>

## <a name="multi-factor-authentication-registration"></a><span data-ttu-id="c7b02-120">Registrace služby Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="c7b02-120">Multi-factor authentication registration</span></span>
<span data-ttu-id="c7b02-121">Dobrý den nejlepší uživatelské prostředí pro obě, hello tok obnovení ohrožení bezpečnosti účtu a hello rizikové toku přihlášení, je, když uživatel hello můžete samoobslužné obnovení.</span><span class="sxs-lookup"><span data-stu-id="c7b02-121">hello best user experience for both, hello compromised account recovery flow and hello risky sign-in flow, is when hello user can self-recover.</span></span> <span data-ttu-id="c7b02-122">Pokud jsou uživatelé zaregistrovaní pro službu Multi-Factor authentication, už mají telefonní číslo přidružené ke svému účtu, který lze použít toopass výzvy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c7b02-122">If users are registered for multi-factor authentication, they already have a phone number associated with their account that can be used toopass security challenges.</span></span> <span data-ttu-id="c7b02-123">Žádné pomoc HelpDesk nebo správce zapojení je potřebné toorecover před ohrožením účet.</span><span class="sxs-lookup"><span data-stu-id="c7b02-123">No help desk or administrator involvement is needed toorecover from account compromise.</span></span> <span data-ttu-id="c7b02-124">Proto má vysokou doporučuje tooget uživatelům zaregistrovat u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="c7b02-124">Thus, it’s highly recommended tooget your users registered for multi-factor authentication.</span></span> 

<span data-ttu-id="c7b02-125">Správci můžou:</span><span class="sxs-lookup"><span data-stu-id="c7b02-125">Administrators can:</span></span>

* <span data-ttu-id="c7b02-126">Nastavte zásady, které vyžaduje další bezpečnostní ověření uživatelé tooset svých účtů.</span><span class="sxs-lookup"><span data-stu-id="c7b02-126">set a policy that requires users tooset up their accounts for additional security verification.</span></span> 
* <span data-ttu-id="c7b02-127">Povolit přeskočení registrace služby Multi-Factor authentication pro až too30 dnů v případě, že chtějí uživatelé toogive období odkladu před registrací.</span><span class="sxs-lookup"><span data-stu-id="c7b02-127">allow skipping multi-factor authentication registration for up too30 days, in case they want toogive users a grace period before registering.</span></span>

<span data-ttu-id="c7b02-128">**registrace služby Multi-Factor authentication Hello má tři kroky:**</span><span class="sxs-lookup"><span data-stu-id="c7b02-128">**hello multi-factor authentication registration has three steps:**</span></span>

1. <span data-ttu-id="c7b02-129">V prvním kroku hello hello uživatel získá oznámení o hello požadavek tooset hello účtu službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="c7b02-129">In hello first step, hello user gets a notification about hello requirement tooset hello account up for multi-factor authentication.</span></span> 
   
    <span data-ttu-id="c7b02-130">![Náprava](./media/active-directory-identityprotection-flows/140.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-130">![Remediation](./media/active-directory-identityprotection-flows/140.png "Remediation")</span></span>
2. <span data-ttu-id="c7b02-131">Služba Multi-Factor authentication tooset nahoru, je nutné toolet hello systému vědět, jak chcete toobe kontaktovat.</span><span class="sxs-lookup"><span data-stu-id="c7b02-131">tooset multi-factor authentication up, you need toolet hello system know how you want toobe contacted.</span></span>
   
    <span data-ttu-id="c7b02-132">![Náprava](./media/active-directory-identityprotection-flows/141.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-132">![Remediation](./media/active-directory-identityprotection-flows/141.png "Remediation")</span></span>
3. <span data-ttu-id="c7b02-133">Hello systému odesílá výzva tooyou a je třeba toorespond.</span><span class="sxs-lookup"><span data-stu-id="c7b02-133">hello system submits a challenge tooyou and you need toorespond.</span></span>
   
    <span data-ttu-id="c7b02-134">![Náprava](./media/active-directory-identityprotection-flows/142.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-134">![Remediation](./media/active-directory-identityprotection-flows/142.png "Remediation")</span></span>

## <a name="risky-sign-in-recovery"></a><span data-ttu-id="c7b02-135">Obnovení rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="c7b02-135">Risky sign-in recovery</span></span>
<span data-ttu-id="c7b02-136">Jestliže správce konfiguroval zásady pro přihlášení rizika, hello vliv na uživatele upozorněni snaží toosign-in.</span><span class="sxs-lookup"><span data-stu-id="c7b02-136">When an administrator has configured a policy for sign-in risks, hello affected users are notified when they try toosign-in.</span></span> 

<span data-ttu-id="c7b02-137">**Hello rizikové přihlášení toku má dva kroky:**</span><span class="sxs-lookup"><span data-stu-id="c7b02-137">**hello risky sign-in flow has two steps:**</span></span> 

1. <span data-ttu-id="c7b02-138">uživatel Hello je informován něco neobvyklého zjistilo o jejich přihlášení, jako je například přihlašujete z nového místa, zařízení nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7b02-138">hello user is informed that something unusual was detected about their sign-in, such as signing in from a new location, device, or app.</span></span> 
   
    <span data-ttu-id="c7b02-139">![Náprava](./media/active-directory-identityprotection-flows/120.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-139">![Remediation](./media/active-directory-identityprotection-flows/120.png "Remediation")</span></span>
2. <span data-ttu-id="c7b02-140">uživatel Hello je požadovaná tooprove svou identitu řešení zabezpečení výzvu.</span><span class="sxs-lookup"><span data-stu-id="c7b02-140">hello user is required tooprove their identity by solving a security challenge.</span></span> <span data-ttu-id="c7b02-141">Pokud je uživatel hello zaregistrován u služby Multi-Factor authentication potřebují tooround cestě zabezpečení kód tootheir telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="c7b02-141">If hello user is registered for multi-factor authentication they need tooround-trip a security code tootheir phone number.</span></span> <span data-ttu-id="c7b02-142">Vzhledem k tomu, že toto je jenom rizikové přihlášení a ohrožení bezpečnosti účtu, hello uživatel nebude mít toochange hello heslo v tomto toku.</span><span class="sxs-lookup"><span data-stu-id="c7b02-142">Since this is a just a risky sign in and not a compromised account, hello user won’t have toochange hello password in this flow.</span></span> 
   
    <span data-ttu-id="c7b02-143">![Náprava](./media/active-directory-identityprotection-flows/121.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-143">![Remediation](./media/active-directory-identityprotection-flows/121.png "Remediation")</span></span>

## <a name="risky-sign-in-blocked"></a><span data-ttu-id="c7b02-144">Rizikové přihlášení blokován</span><span class="sxs-lookup"><span data-stu-id="c7b02-144">Risky sign-in blocked</span></span>
<span data-ttu-id="c7b02-145">Můžete také správci tooset riziko přihlášení uživatelů tooblock zásady při přihlašování v závislosti na úroveň rizika hello.</span><span class="sxs-lookup"><span data-stu-id="c7b02-145">Administrators can also choose tooset a Sign-In Risk policy tooblock users upon sign-in depending on hello risk level.</span></span> <span data-ttu-id="c7b02-146">tooget odblokováno, koncoví uživatelé musí obraťte se na správce nebo HelpDesk, nebo se mohou zkuste se přihlásit ze známé umístění nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="c7b02-146">tooget unblocked, end users must contact an administrator or help desk, or they can try signing in from a familiar location or device.</span></span> <span data-ttu-id="c7b02-147">Samoobslužné obnovení pomocí řešení služby Multi-Factor authentication není možné v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="c7b02-147">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="c7b02-148">![Náprava](./media/active-directory-identityprotection-flows/200.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-148">![Remediation](./media/active-directory-identityprotection-flows/200.png "Remediation")</span></span>

## <a name="compromised-account-recovery"></a><span data-ttu-id="c7b02-149">Obnovení ohrožení bezpečnosti účtu</span><span class="sxs-lookup"><span data-stu-id="c7b02-149">Compromised account recovery</span></span>
<span data-ttu-id="c7b02-150">Když je nakonfigurován uživatelských zásad zabezpečení riziko, uživatele, kteří splní hello uživatele riziko úroveň určená v zásadách hello (a proto se předpokládá, že dojde k ohrožení) musí projít tok obnovení ohrožení hello uživatele předtím, než se můžete přihlásit.</span><span class="sxs-lookup"><span data-stu-id="c7b02-150">When a user risk security policy has been configured, users who meet hello user risk level specified in hello policy (and are therefore assumed compromised) must go through hello user compromise recovery flow before they can sign-in.</span></span> 

<span data-ttu-id="c7b02-151">**tok obnovení ohrožení Hello uživatel má tři kroky:**</span><span class="sxs-lookup"><span data-stu-id="c7b02-151">**hello user compromise recovery flow has three steps:**</span></span>

1. <span data-ttu-id="c7b02-152">uživatel Hello je informován, že jejich zabezpečení účtu je ohrožena kvůli podezřelé aktivity nebo úniku přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="c7b02-152">hello user is informed that their account security is at risk because of suspicious activity or leaked credentials.</span></span>
   
    <span data-ttu-id="c7b02-153">![Náprava](./media/active-directory-identityprotection-flows/101.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-153">![Remediation](./media/active-directory-identityprotection-flows/101.png "Remediation")</span></span>
2. <span data-ttu-id="c7b02-154">uživatel Hello je požadovaná tooprove svou identitu řešení zabezpečení výzvu.</span><span class="sxs-lookup"><span data-stu-id="c7b02-154">hello user is required tooprove their identity by solving a security challenge.</span></span> <span data-ttu-id="c7b02-155">Pokud je uživatel hello zaregistrován u služby Multi-Factor authentication může samoobslužné obnovení před ohrožením..</span><span class="sxs-lookup"><span data-stu-id="c7b02-155">If hello user is registered for multi-factor authentication they can self-recover from being compromised.</span></span> <span data-ttu-id="c7b02-156">Potřebují tooround cestě zabezpečení kód tootheir telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="c7b02-156">They will need tooround-trip a security code tootheir phone number.</span></span> 
   
   <span data-ttu-id="c7b02-157">![Náprava](./media/active-directory-identityprotection-flows/110.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-157">![Remediation](./media/active-directory-identityprotection-flows/110.png "Remediation")</span></span>
3. <span data-ttu-id="c7b02-158">Nakonec hello uživatele je vynucené toochange své heslo, protože někdo jiný měl účet pro přístup k tootheir.</span><span class="sxs-lookup"><span data-stu-id="c7b02-158">Finally, hello user is forced toochange their password since someone else may have had access tootheir account.</span></span> 
   <span data-ttu-id="c7b02-159">Níže jsou snímky obrazovky toto prostředí.</span><span class="sxs-lookup"><span data-stu-id="c7b02-159">Screenshots of this experience are below.</span></span>
   
   <span data-ttu-id="c7b02-160">![Náprava](./media/active-directory-identityprotection-flows/111.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-160">![Remediation](./media/active-directory-identityprotection-flows/111.png "Remediation")</span></span>

## <a name="compromised-account-blocked"></a><span data-ttu-id="c7b02-161">Ohrožení bezpečnosti účtu blokován</span><span class="sxs-lookup"><span data-stu-id="c7b02-161">Compromised account blocked</span></span>
<span data-ttu-id="c7b02-162">tooget uživatel, který byl zablokován pomocí zásad zabezpečení riziko uživatele odblokováno, hello uživatele obraťte se na správce nebo odbornou pomoc.</span><span class="sxs-lookup"><span data-stu-id="c7b02-162">tooget a user that was blocked by a user risk security policy unblocked, hello user must contact an administrator or help desk.</span></span> <span data-ttu-id="c7b02-163">Samoobslužné obnovení pomocí řešení služby Multi-Factor authentication není možné v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="c7b02-163">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="c7b02-164">![Náprava](./media/active-directory-identityprotection-flows/104.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-164">![Remediation](./media/active-directory-identityprotection-flows/104.png "Remediation")</span></span>

## <a name="reset-password"></a><span data-ttu-id="c7b02-165">Resetování hesla</span><span class="sxs-lookup"><span data-stu-id="c7b02-165">Reset password</span></span>
<span data-ttu-id="c7b02-166">Pokud jsou ohrožené uživatelé blokováni v přihlášení, Správce může generovat dočasné heslo pro ně.</span><span class="sxs-lookup"><span data-stu-id="c7b02-166">If compromised users are blocked from signing in, an administrator can generate a temporary password for them.</span></span> <span data-ttu-id="c7b02-167">Hello uživatelé budou mít toochange své heslo při příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c7b02-167">hello users will have toochange their password during a next sign-in.</span></span>

<span data-ttu-id="c7b02-168">![Náprava](./media/active-directory-identityprotection-flows/160.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="c7b02-168">![Remediation](./media/active-directory-identityprotection-flows/160.png "Remediation")</span></span>

## <a name="see-also"></a><span data-ttu-id="c7b02-169">Viz také</span><span class="sxs-lookup"><span data-stu-id="c7b02-169">See also</span></span>
* [<span data-ttu-id="c7b02-170">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c7b02-170">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md) 

