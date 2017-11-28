---
title: "Přihlášení vyskytne s Azure AD Identity Protection | Microsoft Docs"
description: "Poskytuje přehled činnost koncového uživatele při Identity Protection má omezeny nebo opraven uživatele nebo když služby Multi-Factor authentication je potřeba zásady."
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
ms.openlocfilehash: e45936280b51fb2e54012a688fceddcc8dabe984
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a><span data-ttu-id="47a75-104">Možnosti přihlášení s Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="47a75-104">Sign-in experiences with Azure AD Identity Protection</span></span>
<span data-ttu-id="47a75-105">S Azure Active Directory Identity Protection můžete:</span><span class="sxs-lookup"><span data-stu-id="47a75-105">With Azure Active Directory Identity Protection, you can:</span></span>

* <span data-ttu-id="47a75-106">vyžaduje, aby uživatel zaregistrovat pro službu Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="47a75-106">require users to register for multi-factor authentication</span></span>
* <span data-ttu-id="47a75-107">zpracování rizikové přihlášení a ohrožení zabezpečení uživatelů</span><span class="sxs-lookup"><span data-stu-id="47a75-107">handle risky sign-ins and compromised users</span></span>

<span data-ttu-id="47a75-108">Odpověď systému na tyto problémy má dopad na možnosti přihlášení pro uživatele, protože právě přímo přihlášení tím, že poskytuje uživatelské jméno a heslo nebude už možné.</span><span class="sxs-lookup"><span data-stu-id="47a75-108">The response of the system to these issues has an impact on a user's sign-in experience because just directly signing-in by providing a user name and a password won't be possible anymore.</span></span> <span data-ttu-id="47a75-109">Další kroky jsou požadovány pro uživatele bezpečně zpět do firmy.</span><span class="sxs-lookup"><span data-stu-id="47a75-109">Additional steps are required to get a user safely back into business.</span></span>

<span data-ttu-id="47a75-110">Toto téma poskytuje přehled možností přihlašování uživatele pro všechny případy, které se můžou vyskytnout.</span><span class="sxs-lookup"><span data-stu-id="47a75-110">This topic gives you an overview of a user's sign-in experience for all cases that can occur.</span></span>

<span data-ttu-id="47a75-111">**Multi-Factor Authentication**</span><span class="sxs-lookup"><span data-stu-id="47a75-111">**Multi-factor authentication**</span></span>

* <span data-ttu-id="47a75-112">Registrace služby Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="47a75-112">Multi-factor authentication registration</span></span>

<span data-ttu-id="47a75-113">**Přihlášení v ohrožení**</span><span class="sxs-lookup"><span data-stu-id="47a75-113">**Sign-in at risk**</span></span>

* <span data-ttu-id="47a75-114">Obnovení rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="47a75-114">Risky sign-in recovery</span></span>
* <span data-ttu-id="47a75-115">Rizikové přihlášení blokován</span><span class="sxs-lookup"><span data-stu-id="47a75-115">Risky sign-in blocked</span></span>
* <span data-ttu-id="47a75-116">Registrace služby Multi-Factor authentication během rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="47a75-116">Multi-factor authentication registration during a risky sign-in</span></span>

<span data-ttu-id="47a75-117">**Uživatel v ohrožení**</span><span class="sxs-lookup"><span data-stu-id="47a75-117">**User at risk**</span></span>

* <span data-ttu-id="47a75-118">Obnovení ohrožení bezpečnosti účtu</span><span class="sxs-lookup"><span data-stu-id="47a75-118">Compromised account recovery</span></span>
* <span data-ttu-id="47a75-119">Ohrožení bezpečnosti účtu blokován</span><span class="sxs-lookup"><span data-stu-id="47a75-119">Compromised account blocked</span></span>

## <a name="multi-factor-authentication-registration"></a><span data-ttu-id="47a75-120">Registrace služby Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="47a75-120">Multi-factor authentication registration</span></span>
<span data-ttu-id="47a75-121">Nejlepších výsledků v obou případech tok obnovení ohrožení bezpečnosti účtu a rizikové tok přihlášení, je, když uživatel může samoobslužné obnovení.</span><span class="sxs-lookup"><span data-stu-id="47a75-121">The best user experience for both, the compromised account recovery flow and the risky sign-in flow, is when the user can self-recover.</span></span> <span data-ttu-id="47a75-122">Pokud jsou uživatelé zaregistrovaní pro službu Multi-Factor authentication, už mají telefonní číslo přidružené k účtu, který slouží k předávání výzvy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="47a75-122">If users are registered for multi-factor authentication, they already have a phone number associated with their account that can be used to pass security challenges.</span></span> <span data-ttu-id="47a75-123">Žádné pomoc HelpDesk nebo správce zapojení je potřebný k obnovení před ohrožením účet.</span><span class="sxs-lookup"><span data-stu-id="47a75-123">No help desk or administrator involvement is needed to recover from account compromise.</span></span> <span data-ttu-id="47a75-124">Proto se důrazně doporučujeme pro vaši uživatelé zaregistrovat u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="47a75-124">Thus, it’s highly recommended to get your users registered for multi-factor authentication.</span></span> 

<span data-ttu-id="47a75-125">Správci můžou:</span><span class="sxs-lookup"><span data-stu-id="47a75-125">Administrators can:</span></span>

* <span data-ttu-id="47a75-126">Nastavte zásady, které vyžaduje, aby uživatelé nastavit své účty pro další ověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="47a75-126">set a policy that requires users to set up their accounts for additional security verification.</span></span> 
* <span data-ttu-id="47a75-127">Povolit přeskočení registrace služby Multi-Factor authentication po dobu 30 dnů, v případě, že chtějí umožnit uživateli a období odkladu před registrací.</span><span class="sxs-lookup"><span data-stu-id="47a75-127">allow skipping multi-factor authentication registration for up to 30 days, in case they want to give users a grace period before registering.</span></span>

<span data-ttu-id="47a75-128">**Registrace služby Multi-Factor authentication má tři kroky:**</span><span class="sxs-lookup"><span data-stu-id="47a75-128">**The multi-factor authentication registration has three steps:**</span></span>

1. <span data-ttu-id="47a75-129">V prvním kroku uživatel obdrží oznámení o požadavku na nastavení účtu službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="47a75-129">In the first step, the user gets a notification about the requirement to set the account up for multi-factor authentication.</span></span> 
   
    <span data-ttu-id="47a75-130">![Náprava](./media/active-directory-identityprotection-flows/140.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-130">![Remediation](./media/active-directory-identityprotection-flows/140.png "Remediation")</span></span>
2. <span data-ttu-id="47a75-131">Pokud chcete nastavit vícefaktorové ověřování, musíte vědět, jak chcete kontaktovat systém.</span><span class="sxs-lookup"><span data-stu-id="47a75-131">To set multi-factor authentication up, you need to let the system know how you want to be contacted.</span></span>
   
    <span data-ttu-id="47a75-132">![Náprava](./media/active-directory-identityprotection-flows/141.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-132">![Remediation](./media/active-directory-identityprotection-flows/141.png "Remediation")</span></span>
3. <span data-ttu-id="47a75-133">Odešle systému může být obtížné jste a potřebujete, aby odpovídal.</span><span class="sxs-lookup"><span data-stu-id="47a75-133">The system submits a challenge to you and you need to respond.</span></span>
   
    <span data-ttu-id="47a75-134">![Náprava](./media/active-directory-identityprotection-flows/142.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-134">![Remediation](./media/active-directory-identityprotection-flows/142.png "Remediation")</span></span>

## <a name="risky-sign-in-recovery"></a><span data-ttu-id="47a75-135">Obnovení rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="47a75-135">Risky sign-in recovery</span></span>
<span data-ttu-id="47a75-136">Jestliže správce konfiguroval zásady pro přihlášení rizika, ovlivnění uživatelé upozorněni při pokusu o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="47a75-136">When an administrator has configured a policy for sign-in risks, the affected users are notified when they try to sign-in.</span></span> 

<span data-ttu-id="47a75-137">**Rizikové toku přihlášení má dva kroky:**</span><span class="sxs-lookup"><span data-stu-id="47a75-137">**The risky sign-in flow has two steps:**</span></span> 

1. <span data-ttu-id="47a75-138">Uživatel je informován něco neobvyklého zjistilo o jejich přihlášení, jako je například přihlašujete z nového místa, zařízení nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="47a75-138">The user is informed that something unusual was detected about their sign-in, such as signing in from a new location, device, or app.</span></span> 
   
    <span data-ttu-id="47a75-139">![Náprava](./media/active-directory-identityprotection-flows/120.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-139">![Remediation](./media/active-directory-identityprotection-flows/120.png "Remediation")</span></span>
2. <span data-ttu-id="47a75-140">Uživatel musí k prokázání své identity tím řešení bezpečnostní kontroly.</span><span class="sxs-lookup"><span data-stu-id="47a75-140">The user is required to prove their identity by solving a security challenge.</span></span> <span data-ttu-id="47a75-141">Pokud je uživatel zaregistrován u služby Multi-Factor authentication potřebují k odezvě na zabezpečovací kód, abyste své telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="47a75-141">If the user is registered for multi-factor authentication they need to round-trip a security code to their phone number.</span></span> <span data-ttu-id="47a75-142">Vzhledem k tomu, že toto je jenom rizikové přihlášení a ohrožení bezpečnosti účtu, uživatel nebude muset změnit heslo v tomto toku.</span><span class="sxs-lookup"><span data-stu-id="47a75-142">Since this is a just a risky sign in and not a compromised account, the user won’t have to change the password in this flow.</span></span> 
   
    <span data-ttu-id="47a75-143">![Náprava](./media/active-directory-identityprotection-flows/121.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-143">![Remediation](./media/active-directory-identityprotection-flows/121.png "Remediation")</span></span>

## <a name="risky-sign-in-blocked"></a><span data-ttu-id="47a75-144">Rizikové přihlášení blokován</span><span class="sxs-lookup"><span data-stu-id="47a75-144">Risky sign-in blocked</span></span>
<span data-ttu-id="47a75-145">Můžete také správci nastavit přihlášení riziko zásady pro blokování uživatele při přihlášení v závislosti na úroveň rizika.</span><span class="sxs-lookup"><span data-stu-id="47a75-145">Administrators can also choose to set a Sign-In Risk policy to block users upon sign-in depending on the risk level.</span></span> <span data-ttu-id="47a75-146">Koncoví uživatelé musí získat odblokuje, obraťte se na správce nebo odbornou pomoc nebo se mohou zkuste se přihlásit ze známé umístění nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="47a75-146">To get unblocked, end users must contact an administrator or help desk, or they can try signing in from a familiar location or device.</span></span> <span data-ttu-id="47a75-147">Samoobslužné obnovení pomocí řešení služby Multi-Factor authentication není možné v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="47a75-147">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="47a75-148">![Náprava](./media/active-directory-identityprotection-flows/200.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-148">![Remediation](./media/active-directory-identityprotection-flows/200.png "Remediation")</span></span>

## <a name="compromised-account-recovery"></a><span data-ttu-id="47a75-149">Obnovení ohrožení bezpečnosti účtu</span><span class="sxs-lookup"><span data-stu-id="47a75-149">Compromised account recovery</span></span>
<span data-ttu-id="47a75-150">Když je nakonfigurován uživatelských zásad zabezpečení riziko, uživatele, kteří splní uživatele riziko úroveň určená v zásadách (a proto se předpokládá, že dojde k ohrožení) musí projít tok obnovení ohrožení zabezpečení uživatele, než se můžete přihlásit.</span><span class="sxs-lookup"><span data-stu-id="47a75-150">When a user risk security policy has been configured, users who meet the user risk level specified in the policy (and are therefore assumed compromised) must go through the user compromise recovery flow before they can sign-in.</span></span> 

<span data-ttu-id="47a75-151">**Tok obnovení ohrožení zabezpečení uživatele má tři kroky:**</span><span class="sxs-lookup"><span data-stu-id="47a75-151">**The user compromise recovery flow has three steps:**</span></span>

1. <span data-ttu-id="47a75-152">Uživatel je informován, že jejich zabezpečení účtu je ohrožena kvůli podezřelé aktivity nebo úniku přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="47a75-152">The user is informed that their account security is at risk because of suspicious activity or leaked credentials.</span></span>
   
    <span data-ttu-id="47a75-153">![Náprava](./media/active-directory-identityprotection-flows/101.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-153">![Remediation](./media/active-directory-identityprotection-flows/101.png "Remediation")</span></span>
2. <span data-ttu-id="47a75-154">Uživatel musí k prokázání své identity tím řešení bezpečnostní kontroly.</span><span class="sxs-lookup"><span data-stu-id="47a75-154">The user is required to prove their identity by solving a security challenge.</span></span> <span data-ttu-id="47a75-155">Pokud je uživatel zaregistrován u služby Multi-Factor authentication může samoobslužné obnovení před ohrožením..</span><span class="sxs-lookup"><span data-stu-id="47a75-155">If the user is registered for multi-factor authentication they can self-recover from being compromised.</span></span> <span data-ttu-id="47a75-156">Bude nutné k odezvě na zabezpečovací kód, abyste své telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="47a75-156">They will need to round-trip a security code to their phone number.</span></span> 
   
   <span data-ttu-id="47a75-157">![Náprava](./media/active-directory-identityprotection-flows/110.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-157">![Remediation](./media/active-directory-identityprotection-flows/110.png "Remediation")</span></span>
3. <span data-ttu-id="47a75-158">Nakonec uživatel bude muset změnit své heslo, protože někdo jiný měl přístup ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="47a75-158">Finally, the user is forced to change their password since someone else may have had access to their account.</span></span> 
   <span data-ttu-id="47a75-159">Níže jsou snímky obrazovky toto prostředí.</span><span class="sxs-lookup"><span data-stu-id="47a75-159">Screenshots of this experience are below.</span></span>
   
   <span data-ttu-id="47a75-160">![Náprava](./media/active-directory-identityprotection-flows/111.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-160">![Remediation](./media/active-directory-identityprotection-flows/111.png "Remediation")</span></span>

## <a name="compromised-account-blocked"></a><span data-ttu-id="47a75-161">Ohrožení bezpečnosti účtu blokován</span><span class="sxs-lookup"><span data-stu-id="47a75-161">Compromised account blocked</span></span>
<span data-ttu-id="47a75-162">Uživatel musí získat uživatele, který byl zablokován pomocí zásad zabezpečení riziko uživatele odblokováno, obraťte se na správce nebo HelpDesk.</span><span class="sxs-lookup"><span data-stu-id="47a75-162">To get a user that was blocked by a user risk security policy unblocked, the user must contact an administrator or help desk.</span></span> <span data-ttu-id="47a75-163">Samoobslužné obnovení pomocí řešení služby Multi-Factor authentication není možné v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="47a75-163">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="47a75-164">![Náprava](./media/active-directory-identityprotection-flows/104.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-164">![Remediation](./media/active-directory-identityprotection-flows/104.png "Remediation")</span></span>

## <a name="reset-password"></a><span data-ttu-id="47a75-165">Resetování hesla</span><span class="sxs-lookup"><span data-stu-id="47a75-165">Reset password</span></span>
<span data-ttu-id="47a75-166">Pokud jsou ohrožené uživatelé blokováni v přihlášení, Správce může generovat dočasné heslo pro ně.</span><span class="sxs-lookup"><span data-stu-id="47a75-166">If compromised users are blocked from signing in, an administrator can generate a temporary password for them.</span></span> <span data-ttu-id="47a75-167">Uživatelé budou muset změnit své heslo při příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="47a75-167">The users will have to change their password during a next sign-in.</span></span>

<span data-ttu-id="47a75-168">![Náprava](./media/active-directory-identityprotection-flows/160.png "nápravy")</span><span class="sxs-lookup"><span data-stu-id="47a75-168">![Remediation](./media/active-directory-identityprotection-flows/160.png "Remediation")</span></span>

## <a name="see-also"></a><span data-ttu-id="47a75-169">Viz také</span><span class="sxs-lookup"><span data-stu-id="47a75-169">See also</span></span>
* [<span data-ttu-id="47a75-170">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="47a75-170">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md) 

