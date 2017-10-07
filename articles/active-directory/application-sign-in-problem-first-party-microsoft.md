---
title: "aaaProblems přihlášení tooa aplikace Microsoft | Microsoft Docs"
description: "Řešení běžných potíží potýkají při přihlášení výrobců toofirst Microsoft Applications pomocí služby Azure AD (např. Office 365)"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 35849ca8dbaa909d17b6d0da572f5c11041a8559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="problems-signing-in-tooa-microsoft-application"></a><span data-ttu-id="5712e-103">Potíže s přihlášením tooa aplikace Microsoft</span><span class="sxs-lookup"><span data-stu-id="5712e-103">Problems signing in tooa Microsoft application</span></span>

<span data-ttu-id="5712e-104">Microsoft Applications (např. Office 365 Exchange, SharePoint, Yammer atd.) jsou přiřazeny a trochu jinak než 3. aplikace SaaS strany nebo jiné aplikace, které integrují s Azure AD pro jednotné přihlašování na spravovat.</span><span class="sxs-lookup"><span data-stu-id="5712e-104">Microsoft Applications (like Office 365 Exchange, SharePoint, Yammer, etc.) are assigned and managed a bit differently than 3rd party SaaS applications or other applications you integrate with Azure AD for single sign on.</span></span>

<span data-ttu-id="5712e-105">Existují tři hlavní způsoby, které uživatel může získat přístup k tooa Microsoft publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="5712e-105">There are three main ways that a user can get access tooa Microsoft-published application.</span></span>

-   <span data-ttu-id="5712e-106">Pro aplikace v hello Office 365 nebo jiné placené sady, mají uživatelé udělen přístup prostřednictvím **přiřazení licence** buď přímo tootheir uživatelský účet, nebo prostřednictvím skupiny pomocí funkce přiřazení našeho na základě skupin licencí.</span><span class="sxs-lookup"><span data-stu-id="5712e-106">For applications in hello Office 365 or other paid suites, users are granted access through **license assignment** either directly tootheir user account, or through a group using our group-based license assignment capability.</span></span>

-   <span data-ttu-id="5712e-107">Pro aplikace společnosti Microsoft nebo třetích stran publikování, a to volně pro každý, kdo toouse, může být uživatelé udělen přístup prostřednictvím **souhlas uživatele**.</span><span class="sxs-lookup"><span data-stu-id="5712e-107">For applications that Microsoft or a Third Party publishes freely for anyone toouse, users may be granted access through **user consent**.</span></span> <span data-ttu-id="5712e-108">This0 znamená, že se přihlaste toohello aplikace s jejich Azure AD pracovního nebo školního účtu a povolit toohave přístup toosome omezenou sadu dat na svůj účet.</span><span class="sxs-lookup"><span data-stu-id="5712e-108">This0 means that they sign in toohello application with their Azure AD Work or School account and allow it toohave access toosome limited set of data on their account.</span></span>

-   <span data-ttu-id="5712e-109">Pro aplikace, že společnost Microsoft nebo 3rd strany publikuje volně pro každý, kdo toouse, uživatelé také udělit přístup přes **souhlas správce**.</span><span class="sxs-lookup"><span data-stu-id="5712e-109">For applications that Microsoft or a 3rd Party publishes freely for anyone toouse, users may also be granted access through **administrator consent**.</span></span> <span data-ttu-id="5712e-110">To znamená, že správce zjistí, že aplikace hello může být používán všichni uživatelé v organizaci hello, takže přihlásit toohello aplikaci pomocí účtu globálního správce a udělit přístup tooeveryone v organizaci hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-110">This means that an administrator has determined hello application may be used by everyone in hello organization, so they sign in toohello application with a Global Administrator account and grant access tooeveryone in hello organization.</span></span>

<span data-ttu-id="5712e-111">tootroubleshoot problém, začněte s hello [obecné problémových oblastí s přístup k aplikaci tooconsider](#general-problem-areas-with-application-access-to-consider) a potom si přečtěte hello [návod: kroky tootroubleshoot Microsoft Application přístup](#walkthrough-steps-to-troubleshoot-microsoft-application-access) tooget do hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5712e-111">tootroubleshoot your issue, start with hello [General Problem Areas with Application Access tooconsider](#general-problem-areas-with-application-access-to-consider) and then read hello [Walkthrough: Steps tootroubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access) tooget into hello details.</span></span>

## <a name="general-problem-areas-with-application-access-tooconsider"></a><span data-ttu-id="5712e-112">Obecné problémových oblastí s tooconsider přístup k aplikaci</span><span class="sxs-lookup"><span data-stu-id="5712e-112">General Problem Areas with Application Access tooconsider</span></span>

<span data-ttu-id="5712e-113">Níže uvádíme seznam hello obecné problémových oblastí, které můžete rozbalit Pokud máte představu o kterém se toostart, ale nemůžeme doporučujeme, číst tooget hello návodu budete rychle: [návod: kroky tootroubleshoot Microsoft Application přístup](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span><span class="sxs-lookup"><span data-stu-id="5712e-113">Below is a list of hello general problem areas that you can drill into if you have an idea of where toostart, but we recommend you read hello walkthrough tooget going quickly: [Walkthrough: Steps tootroubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span></span>

-   [<span data-ttu-id="5712e-114">Problémy s hello uživatelský účet</span><span class="sxs-lookup"><span data-stu-id="5712e-114">Problems with hello user’s account</span></span>](#problems-with-the-users-account)

-   [<span data-ttu-id="5712e-115">Problémy se skupinami</span><span class="sxs-lookup"><span data-stu-id="5712e-115">Problems with groups</span></span>](#problems-with-groups)

-   [<span data-ttu-id="5712e-116">Problémy se zásadami podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="5712e-116">Problems with conditional access policies</span></span>](#problems-with-conditional-access-policies)

-   [<span data-ttu-id="5712e-117">Problémy s aplikací souhlasu</span><span class="sxs-lookup"><span data-stu-id="5712e-117">Problems with application consent</span></span>](#problems-with-application-consent)

## <a name="steps-tootroubleshoot-microsoft-application-access"></a><span data-ttu-id="5712e-118">Kroky tootroubleshoot Microsoft Application přístup</span><span class="sxs-lookup"><span data-stu-id="5712e-118">Steps tootroubleshoot Microsoft Application access</span></span>

<span data-ttu-id="5712e-119">Níže jsou některé běžné problémy zaměstnance spustit do při jejich uživatelé nemůžete se přihlásit tooa aplikace společnost Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5712e-119">Below are some common issues folks run into when their users cannot sign in tooa Microsoft application.</span></span>

-   <span data-ttu-id="5712e-120">Obecné problémy toocheck nejprve</span><span class="sxs-lookup"><span data-stu-id="5712e-120">General issues toocheck first</span></span>

  * <span data-ttu-id="5712e-121">Zajistěte, aby uživatel hello přihlašuje toohello **opravte adresu URL** a ne místní aplikace adresy URL.</span><span class="sxs-lookup"><span data-stu-id="5712e-121">Make sure hello user is signing in toohello **correct URL** and not a local application URL.</span></span>

  * <span data-ttu-id="5712e-122">Zkontrolujte, zda text hello uživatelský účet je **není uzamčen.**</span><span class="sxs-lookup"><span data-stu-id="5712e-122">Make sure hello user’s account is **not locked out.**</span></span>

  * <span data-ttu-id="5712e-123">Ujistěte se, zda text hello **uživatelský účet existuje** v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5712e-123">Make sure hello **user’s account exists** in Azure Active Directory.</span></span> [<span data-ttu-id="5712e-124">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5712e-124">Check if a user account exists in Azure Active Directory</span></span>](#problems-with-the-users-account)

  * <span data-ttu-id="5712e-125">Zkontrolujte, zda text hello uživatelský účet je **povoleno** pro přihlášení. [Zkontrolujte stav účtu uživatele](#problems-with-the-users-account)</span><span class="sxs-lookup"><span data-stu-id="5712e-125">Make sure hello user’s account is **enabled** for sign ins. [Check a user’s account status](#problems-with-the-users-account)</span></span>

  * <span data-ttu-id="5712e-126">Ujistěte se, zda text hello uživatele **není platnost nebo zapomenete heslo.**</span><span class="sxs-lookup"><span data-stu-id="5712e-126">Make sure hello user’s **password is not expired or forgotten.**</span></span> <span data-ttu-id="5712e-127">[Resetovat heslo uživatele](#reset-a-users-password) nebo [povolit samoobslužné resetování hesla](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span><span class="sxs-lookup"><span data-stu-id="5712e-127">[Reset a user’s password](#reset-a-users-password) or [Enable self-service password reset](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span></span>

   * <span data-ttu-id="5712e-128">Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5712e-128">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span> <span data-ttu-id="5712e-129">[Zkontrolujte stav služby Multi-Factor authentication uživatele](#check-a-users-multi-factor-authentication-status) nebo [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="5712e-129">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

   * <span data-ttu-id="5712e-130">Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5712e-130">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span> <span data-ttu-id="5712e-131">[Zkontrolujte zásady podmíněného přístupu konkrétním](#problems-with-conditional-access-policies) nebo [zkontrolujte zásady podmíněného přístupu pro konkrétní aplikaci](#check-a-specific-applications-conditional-access-policy) nebo [zakázat zásadu podmíněného přístupu konkrétním](#disable-a-specific-conditional-access-policy)</span><span class="sxs-lookup"><span data-stu-id="5712e-131">[Check a specific conditional access policy](#problems-with-conditional-access-policies) or [Check a specific application’s conditional access policy](#check-a-specific-applications-conditional-access-policy) or [Disable a specific conditional access policy](#disable-a-specific-conditional-access-policy)</span></span>

   * <span data-ttu-id="5712e-132">Ujistěte se, že uživatele **ověřování kontaktní údaje** je toodate tooallow ověřování Multi-Factor Authentication nebo podmíněného přístupu zásady toobe vynucené.</span><span class="sxs-lookup"><span data-stu-id="5712e-132">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span> <span data-ttu-id="5712e-133">[Zkontrolujte stav služby Multi-Factor authentication uživatele](#check-a-users-multi-factor-authentication-status) nebo [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="5712e-133">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

-   <span data-ttu-id="5712e-134">Pro **Microsoft** **aplikace, které vyžadují licenci** (např. Office 365), tady jsou některé konkrétní problémy toocheck po nevyloučí hello obecné problémy výše:</span><span class="sxs-lookup"><span data-stu-id="5712e-134">For **Microsoft** **applications that require a license** (like Office365), here are some specific issues toocheck once you have ruled out hello general issues above:</span></span>

   * <span data-ttu-id="5712e-135">Ujistěte se, hello uživatele nebo má **přiřazenou licenci.**</span><span class="sxs-lookup"><span data-stu-id="5712e-135">Ensure hello user or has a **license assigned.**</span></span> <span data-ttu-id="5712e-136">[Zkontrolujte uživatele přiřazené licence](#check-a-users-assigned-licenses) nebo [Zkontrolujte skupiny přiřazené licence](#check-a-groups-assigned-licenses)</span><span class="sxs-lookup"><span data-stu-id="5712e-136">[Check a user’s assigned licenses](#check-a-users-assigned-licenses) or [Check a group’s assigned licenses](#check-a-groups-assigned-licenses)</span></span>

   * <span data-ttu-id="5712e-137">Pokud je licence hello **přiřazené tooa** **statická skupina**, ujistěte se, že hello **uživatel je členem** této skupiny.</span><span class="sxs-lookup"><span data-stu-id="5712e-137">If hello license is **assigned tooa** **static group**, ensure that hello **user is a member** of that group.</span></span> [<span data-ttu-id="5712e-138">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="5712e-138">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   * <span data-ttu-id="5712e-139">Pokud je licence hello **přiřazené tooa** **dynamická skupina**, ujistěte se, že hello **je dynamická skupina pravidlo správně nastavené**.</span><span class="sxs-lookup"><span data-stu-id="5712e-139">If hello license is **assigned tooa** **dynamic group**, ensure that hello **dynamic group rule is set correctly**.</span></span> [<span data-ttu-id="5712e-140">Zkontrolujte kritéria členství dynamické skupiny</span><span class="sxs-lookup"><span data-stu-id="5712e-140">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

   * <span data-ttu-id="5712e-141">Pokud je licence hello **přiřazené tooa** **dynamická skupina**, ujistěte se, že hello dynamická skupina má **bylo dokončeno zpracování** jeho členství a že hello **uživatel člen** (to může trvat nějakou dobu).</span><span class="sxs-lookup"><span data-stu-id="5712e-141">If hello license is **assigned tooa** **dynamic group**, ensure that hello dynamic group has **finished processing** its membership and that hello **user is a member** (this can take some time).</span></span> [<span data-ttu-id="5712e-142">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="5712e-142">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   *  <span data-ttu-id="5712e-143">Jakmile budete mít jistotu, hello licence se nepřiřadí, zkontrolujte, zda je licence hello **nevypršela**.</span><span class="sxs-lookup"><span data-stu-id="5712e-143">Once you make sure hello license is assigned, make sure hello license is **not expired**.</span></span>

   *  <span data-ttu-id="5712e-144">Zkontrolujte, zda je licence hello **aplikace hello** uživatelé přistupují.</span><span class="sxs-lookup"><span data-stu-id="5712e-144">Make sure hello license is **for hello application** they are accessing.</span></span>

-   <span data-ttu-id="5712e-145">Pro **Microsoft** **aplikace, které nevyžadují licenci**, zde jsou některé další toocheck věcí:</span><span class="sxs-lookup"><span data-stu-id="5712e-145">For **Microsoft** **applications that don’t require a license**, here are some other things toocheck:</span></span>

   * <span data-ttu-id="5712e-146">Pokud aplikace hello požaduje **oprávnění na úrovni uživatele** (například "přístup k poštovní schránka tohoto uživatele"), zajistěte, aby tento uživatel hello se přihlásil toohello aplikace a byla provedena **individuální souhlasu operace**  toolet hello aplikace přístup k jeho data.</span><span class="sxs-lookup"><span data-stu-id="5712e-146">If hello application is requesting **user-level permissions** (for example “Access this user’s mailbox”), make sure that hello user has signed in toohello application and has performed a **user-level consent operation** toolet hello application access her data.</span></span>

   * <span data-ttu-id="5712e-147">Pokud aplikace hello požaduje **oprávnění na úrovni správce** (například "přístup k poštovním schránkám všechny uživatelské"), ujistěte se, že byla provedena globálního správce **operace správce úroveň souhlasu s jménem všech uživatelů** v organizaci hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-147">If hello application is requesting **administrator-level permissions** (for example “Access all user’s mailboxes”), make sure that a Global Administrator has performed an **administrator-level consent operation on behalf of all users** in hello organization.</span></span>

## <a name="problems-with-hello-users-account"></a><span data-ttu-id="5712e-148">Problémy s hello uživatelský účet</span><span class="sxs-lookup"><span data-stu-id="5712e-148">Problems with hello user’s account</span></span>

<span data-ttu-id="5712e-149">Z důvodu tooa problém s uživatelem, který je přiřazen toohello aplikace můžete blokovat přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5712e-149">Application access can be blocked due tooa problem with a user that is assigned toohello application.</span></span> <span data-ttu-id="5712e-150">Tady jsou některé způsoby řešení problémů a řešení problémů s uživateli a jejich nastavení účtu:</span><span class="sxs-lookup"><span data-stu-id="5712e-150">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="5712e-151">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5712e-151">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="5712e-152">Zkontrolujte stav účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-152">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="5712e-153">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-153">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="5712e-154">Povolení samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="5712e-154">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="5712e-155">Zkontrolujte stav služby Multi-Factor authentication uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-155">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="5712e-156">Zkontrolovat kontaktní informace o ověřování uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-156">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="5712e-157">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="5712e-157">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="5712e-158">Zkontrolujte uživatele přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="5712e-158">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="5712e-159">Přiřazení licence uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-159">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="5712e-160">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5712e-160">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="5712e-161">toocheck, pokud se nachází uživatelský účet, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-161">toocheck if a user’s account is present, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-162">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-162">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-163">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-163">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-164">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-164">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-165">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-165">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-166">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5712e-166">click **All users**.</span></span>

6.  <span data-ttu-id="5712e-167">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-167">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-168">Zkontrolujte vlastnosti hello hello uživatele objektu toobe se, že zobrazují se podle očekávání a nebyla nalezena žádná data.</span><span class="sxs-lookup"><span data-stu-id="5712e-168">Check hello properties of hello user object toobe sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="5712e-169">Zkontrolujte stav účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-169">Check a user’s account status</span></span>

<span data-ttu-id="5712e-170">toocheck uživatele pro účet stavu, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-170">toocheck a user’s account status, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-171">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-171">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-172">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-172">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-173">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-173">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-174">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-174">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-175">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5712e-175">click **All users**.</span></span>

6.  <span data-ttu-id="5712e-176">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-176">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-177">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="5712e-177">click **Profile**.</span></span>

8.  <span data-ttu-id="5712e-178">V části **nastavení** Ujistěte se, že **bloku přihlásit** je nastaven příliš**ne**.</span><span class="sxs-lookup"><span data-stu-id="5712e-178">Under **Settings** ensure that **Block sign in** is set too**No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="5712e-179">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-179">Reset a user’s password</span></span>

<span data-ttu-id="5712e-180">tooreset heslo uživatele, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-180">tooreset a user’s password, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-181">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-181">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-182">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-182">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-183">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-183">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-184">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-184">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-185">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5712e-185">click **All users**.</span></span>

6.  <span data-ttu-id="5712e-186">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-186">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-187">Klikněte na tlačítko hello **resetovat heslo** tlačítko hello horní části okna hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="5712e-187">click hello **Reset password** button at hello top of hello user blade.</span></span>

8.  <span data-ttu-id="5712e-188">Klikněte na tlačítko hello **resetovat heslo** na hello tlačítko **resetovat heslo** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="5712e-188">click hello **Reset password** button on hello **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="5712e-189">Kopírování hello **dočasné heslo** nebo **zadat nové heslo** pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-189">Copy hello **temporary password** or **enter a new password** for hello user.</span></span>

10. <span data-ttu-id="5712e-190">Komunikaci tohoto nového uživatele toohello heslo, ale měly požadované toochange toto heslo při příštím přihlášení tooAzure služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5712e-190">Communicate this new password toohello user, they be required toochange this password during their next sign in tooAzure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="5712e-191">Povolit samoobslužné resetování hesla</span><span class="sxs-lookup"><span data-stu-id="5712e-191">Enable self-service password reset</span></span>

<span data-ttu-id="5712e-192">tooenable samoobslužné služby heslo resetovat, postupujte podle následujících pokynů hello nasazení:</span><span class="sxs-lookup"><span data-stu-id="5712e-192">tooenable self-service password reset, follow hello deployment steps below:</span></span>

-   [<span data-ttu-id="5712e-193">Povolit uživatelům tooreset hesel služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5712e-193">Enable users tooreset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="5712e-194">Povolit uživatelům tooreset nebo změnit své heslo místní služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="5712e-194">Enable users tooreset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="5712e-195">Zkontrolujte stav služby Multi-Factor authentication uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-195">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="5712e-196">toocheck uživatele je vícefaktorového ověřování stavu, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-196">toocheck a user’s multi-factor authentication status, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-197">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-197">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-198">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-198">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-199">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-199">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-200">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-200">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-201">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5712e-201">click **All users**.</span></span>

6.  <span data-ttu-id="5712e-202">Klikněte na tlačítko hello **Multi-Factor Authentication** tlačítko hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-202">click hello **Multi-Factor Authentication** button at hello top of hello blade.</span></span>

7.  <span data-ttu-id="5712e-203">Jednou hello **portálu pro správu služby Multi-Factor Authentication** zatížení, ujistěte se, jsou na hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="5712e-203">Once hello **Multi-Factor Authentication Administration Portal** loads, ensure you are on hello **Users** tab.</span></span>

8.  <span data-ttu-id="5712e-204">Najít hello uživatele v seznamu hello uživatelů ve vyhledávání, filtrování a řazení.</span><span class="sxs-lookup"><span data-stu-id="5712e-204">Find hello user in hello list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="5712e-205">Vyberte hello uživatele z hello seznam uživatelů a **povolit**, **zakázat**, nebo **vynutit** služby Multi-Factor authentication podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5712e-205">Select hello user from hello list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

  * <span data-ttu-id="5712e-206">**Poznámka:**: Pokud je uživatel v **vynucené** stavu, je pravděpodobně příliš nastavit**zakázané** dočasně toolet je zpět do svého účtu.</span><span class="sxs-lookup"><span data-stu-id="5712e-206">**Note**: If a user is in an **Enforced** state, you may set them too**Disabled** temporarily toolet them back into their account.</span></span> <span data-ttu-id="5712e-207">Jakmile se zpět, můžete změnit jejich stavu příliš**povoleno** znovu toorequire je toore registrace své kontaktní informace při příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5712e-207">Once they are back in, you can then change their state too**Enabled** again toorequire them toore-register their contact information during their next sign in.</span></span> <span data-ttu-id="5712e-208">Alternativně můžete provést kroky hello v hello [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info) tooverify nebo pro ně nastavit tato data.</span><span class="sxs-lookup"><span data-stu-id="5712e-208">Alternatively, you can follow hello steps in hello [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) tooverify or set this data for them.</span></span>

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="5712e-209">Zkontrolovat kontaktní informace o ověřování uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-209">Check a user’s authentication contact info</span></span>

<span data-ttu-id="5712e-210">toocheck ověřování uživatele kontaktní informace pro vícefaktorové ověřování, podmíněného přístupu, ochrany identit a resetování hesla, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="5712e-210">toocheck a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-211">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-211">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-212">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-212">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-213">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-213">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-214">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-214">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-215">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5712e-215">click **All users**.</span></span>

6.  <span data-ttu-id="5712e-216">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-216">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-217">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="5712e-217">click **Profile**.</span></span>

8.  <span data-ttu-id="5712e-218">Posuňte se dolů příliš**ověřování kontaktní údaje**.</span><span class="sxs-lookup"><span data-stu-id="5712e-218">Scroll down too**Authentication contact info**.</span></span>

9.  <span data-ttu-id="5712e-219">**Zkontrolujte** hello dat registrované pro uživatele hello a aktualizace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5712e-219">**Review** hello data registered for hello user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="5712e-220">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="5712e-220">Check a user’s group memberships</span></span>

<span data-ttu-id="5712e-221">toocheck uživatele na členství ve skupinách, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-221">toocheck a user’s group memberships, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-222">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-222">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-223">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-223">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-224">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-224">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-225">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-225">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-226">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5712e-226">click **All users**.</span></span>

6.  <span data-ttu-id="5712e-227">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-227">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-228">Klikněte na tlačítko **skupiny** toosee, které skupiny hello uživatel je členem skupiny.</span><span class="sxs-lookup"><span data-stu-id="5712e-228">click **Groups** toosee which groups hello user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="5712e-229">Zkontrolujte uživatele přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="5712e-229">Check a user’s assigned licenses</span></span>

<span data-ttu-id="5712e-230">toocheck uživatele je přiřazena licence, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-230">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-231">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-231">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-232">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-232">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-233">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-233">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-234">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-234">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-235">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5712e-235">click **All users**.</span></span>

6.  <span data-ttu-id="5712e-236">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-236">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-237">Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="5712e-237">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="5712e-238">Přiřazení licence uživatele</span><span class="sxs-lookup"><span data-stu-id="5712e-238">Assign a user a license</span></span> 

<span data-ttu-id="5712e-239">tooassign licence tooa uživatele, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="5712e-239">tooassign a license tooa user, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-240">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-240">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-241">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-241">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-242">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-242">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-243">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-243">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-244">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5712e-244">click **All users**.</span></span>

6.  <span data-ttu-id="5712e-245">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-245">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-246">Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="5712e-246">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

8.  <span data-ttu-id="5712e-247">Klikněte na tlačítko hello **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5712e-247">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="5712e-248">Vyberte **jeden nebo více produktů** z hello seznam dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="5712e-248">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="5712e-249">**Volitelné** klikněte na tlačítko hello **přiřazení možností** položky toogranularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="5712e-249">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="5712e-250">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="5712e-250">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="5712e-251">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign tyto licence toothis uživatele.</span><span class="sxs-lookup"><span data-stu-id="5712e-251">Click hello **Assign** button tooassign these licenses toothis user.</span></span>

## <a name="problems-with-groups"></a><span data-ttu-id="5712e-252">Problémy se skupinami</span><span class="sxs-lookup"><span data-stu-id="5712e-252">Problems with groups</span></span>

<span data-ttu-id="5712e-253">Z důvodu tooa problém s skupinu, která je přiřazena toohello aplikace můžete blokovat přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5712e-253">Application access can be blocked due tooa problem with a group that is assigned toohello application.</span></span> <span data-ttu-id="5712e-254">Tady jsou některé způsoby řešení problémů a řešení problémů s skupin a členství ve skupinách:</span><span class="sxs-lookup"><span data-stu-id="5712e-254">Below are some ways you can troubleshoot and solve problems with groups and group memberships:</span></span>

-   [<span data-ttu-id="5712e-255">Zkontrolovat členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="5712e-255">Check a group’s membership</span></span>](#check-a-groups-membership)

-   [<span data-ttu-id="5712e-256">Zkontrolujte kritéria členství dynamické skupiny</span><span class="sxs-lookup"><span data-stu-id="5712e-256">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

-   [<span data-ttu-id="5712e-257">Zkontrolujte skupiny přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="5712e-257">Check a group’s assigned licenses</span></span>](#check-a-groups-assigned-licenses)

-   [<span data-ttu-id="5712e-258">Znovu zpracovat skupině licencí</span><span class="sxs-lookup"><span data-stu-id="5712e-258">Reprocess a group’s licenses</span></span>](#reprocess-a-groups-licenses)

-   [<span data-ttu-id="5712e-259">Přiřazení skupiny licencí</span><span class="sxs-lookup"><span data-stu-id="5712e-259">Assign a group a license</span></span>](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a><span data-ttu-id="5712e-260">Zkontrolovat členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="5712e-260">Check a group’s membership</span></span>

<span data-ttu-id="5712e-261">toocheck členství ve skupině, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-261">toocheck a group’s membership, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-262">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-262">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-263">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-263">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-264">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-264">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-265">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-265">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-266">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="5712e-266">click **All groups**.</span></span>

6.  <span data-ttu-id="5712e-267">**Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-267">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-268">Klikněte na tlačítko **členy** toothis skupiny přiřazeny tooreview hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5712e-268">click **Members** tooreview hello list of users assigned toothis group.</span></span>

### <a name="check-a-dynamic-groups-membership-criteria"></a><span data-ttu-id="5712e-269">Zkontrolujte kritéria členství dynamické skupiny</span><span class="sxs-lookup"><span data-stu-id="5712e-269">Check a dynamic group’s membership criteria</span></span> 

<span data-ttu-id="5712e-270">toocheck kritéria členství dynamické skupiny, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-270">toocheck a dynamic group’s membership criteria, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-271">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-271">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-272">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-272">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-273">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-273">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-274">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-274">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-275">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="5712e-275">click **All groups**.</span></span>

6.  <span data-ttu-id="5712e-276">**Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-276">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-277">Klikněte na tlačítko **pravidla dynamické členství.**</span><span class="sxs-lookup"><span data-stu-id="5712e-277">click **Dynamic membership rules.**</span></span>

8.  <span data-ttu-id="5712e-278">Zkontrolujte hello **jednoduché** nebo **rozšířené** pravidlo definované pro tuto skupinu a ujistěte se, že hello uživatele chcete toobe členem této skupiny splňuje tato kritéria.</span><span class="sxs-lookup"><span data-stu-id="5712e-278">Review hello **simple** or **advanced** rule defined for this group and ensure that hello user you want toobe a member of this group meets these criteria.</span></span>

### <a name="check-a-groups-assigned-licenses"></a><span data-ttu-id="5712e-279">Zkontrolujte skupiny přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="5712e-279">Check a group’s assigned licenses</span></span>

<span data-ttu-id="5712e-280">toocheck skupinu na přiřadit licence, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-280">toocheck a group’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-281">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-281">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-282">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-282">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-283">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-283">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-284">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-284">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-285">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="5712e-285">click **All groups**.</span></span>

6.  <span data-ttu-id="5712e-286">**Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-286">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-287">Klikněte na tlačítko **licence** toosee, které skupiny licencí hello má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="5712e-287">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

### <a name="reprocess-a-groups-licenses"></a><span data-ttu-id="5712e-288">Znovu zpracovat skupině licencí</span><span class="sxs-lookup"><span data-stu-id="5712e-288">Reprocess a group’s licenses</span></span>

<span data-ttu-id="5712e-289">tooreprocess skupinu na přiřadit licence, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-289">tooreprocess a group’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-290">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-290">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-291">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-291">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-292">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-292">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-293">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-293">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-294">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="5712e-294">click **All groups**.</span></span>

6.  <span data-ttu-id="5712e-295">**Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-295">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-296">Klikněte na tlačítko **licence** toosee, které skupiny licencí hello má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="5712e-296">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

8.  <span data-ttu-id="5712e-297">Klikněte na tlačítko hello **znovu zpracovat** tooensure tlačítko, která hello členy skupiny toothis přiřazené licence jsou aktuální.</span><span class="sxs-lookup"><span data-stu-id="5712e-297">click hello **Reprocess** button tooensure that hello licenses assigned toothis group’s members are up-to-date.</span></span> <span data-ttu-id="5712e-298">To může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="5712e-298">This may take a long time, depending on hello size and complexity of hello group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="5712e-299">toodo to rychlejší, vezměte v úvahu dočasně přiřazení licence toohello uživatele přímo.</span><span class="sxs-lookup"><span data-stu-id="5712e-299">toodo this faster, consider temporarily assigning a license toohello user directly.</span></span> <span data-ttu-id="5712e-300">[Přiřazení licence uživatele](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="5712e-300">[Assign a user a license](#problems-with-application-consent).</span></span>
   >
   >

### <a name="assign-a-group-a-license"></a><span data-ttu-id="5712e-301">Přiřazení skupiny licencí</span><span class="sxs-lookup"><span data-stu-id="5712e-301">Assign a group a license</span></span>

<span data-ttu-id="5712e-302">tooassign tooa skupiny licencí, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5712e-302">tooassign a license tooa group, follow hello steps below:</span></span>

1.  <span data-ttu-id="5712e-303">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-303">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-304">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-304">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-305">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-305">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-306">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-306">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-307">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="5712e-307">click **All groups**.</span></span>

6.  <span data-ttu-id="5712e-308">**Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="5712e-308">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="5712e-309">Klikněte na tlačítko **licence** toosee, které skupiny licencí hello má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="5712e-309">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

8.  <span data-ttu-id="5712e-310">Klikněte na tlačítko hello **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5712e-310">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="5712e-311">Vyberte **jeden nebo více produktů** z hello seznam dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="5712e-311">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="5712e-312">**Volitelné** klikněte na tlačítko hello **přiřazení možností** položky toogranularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="5712e-312">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="5712e-313">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="5712e-313">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="5712e-314">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign těchto toothis skupin licencí.</span><span class="sxs-lookup"><span data-stu-id="5712e-314">Click hello **Assign** button tooassign these licenses toothis group.</span></span> <span data-ttu-id="5712e-315">To může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="5712e-315">This may take a long time, depending on hello size and complexity of hello group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="5712e-316">toodo to rychlejší, vezměte v úvahu dočasně přiřazení licence toohello uživatele přímo.</span><span class="sxs-lookup"><span data-stu-id="5712e-316">toodo this faster, consider temporarily assigning a license toohello user directly.</span></span> <span data-ttu-id="5712e-317">[Přiřazení licence uživatele](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="5712e-317">[Assign a user a license](#problems-with-application-consent).</span></span>
   > 
   >

## <a name="problems-with-conditional-access-policies"></a><span data-ttu-id="5712e-318">Problémy se zásadami podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="5712e-318">Problems with conditional access policies</span></span>

### <a name="check-a-specific-conditional-access-policy"></a><span data-ttu-id="5712e-319">Zkontrolujte zásady podmíněného přístupu konkrétním</span><span class="sxs-lookup"><span data-stu-id="5712e-319">Check a specific conditional access policy</span></span>

<span data-ttu-id="5712e-320">toocheck, případně k ověření zásad jednoho podmíněného přístupu:</span><span class="sxs-lookup"><span data-stu-id="5712e-320">toocheck or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="5712e-321">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-321">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-322">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-322">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-323">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-323">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-324">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-324">click **Enterprise applications** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-325">Klikněte na tlačítko hello **podmíněného přístupu** navigační položka.</span><span class="sxs-lookup"><span data-stu-id="5712e-325">click hello **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="5712e-326">Klikněte na tlačítko hello zásady, které vás zajímají kontroly.</span><span class="sxs-lookup"><span data-stu-id="5712e-326">click hello policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="5712e-327">Zkontrolujte, že neexistují žádné konkrétní podmínky, přiřazení nebo jiná nastavení, které blokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5712e-327">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

   >[!NOTE]
   ><span data-ttu-id="5712e-328">Může přát zakázat tootemporarily této zásady tooensure ji nemají vliv na sign in toodo se sada hello **povolit zásady** přepnutí příliš**ne** a klikněte na tlačítko hello **Uložit** tlačítko .</span><span class="sxs-lookup"><span data-stu-id="5712e-328">You may wish tootemporarily disable this policy tooensure it is not affecting sign ins. toodo this, set hello **Enable policy** toggle too**No** and click hello **Save** button.</span></span>
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a><span data-ttu-id="5712e-329">Zkontrolujte zásady podmíněného přístupu pro konkrétní aplikace</span><span class="sxs-lookup"><span data-stu-id="5712e-329">Check a specific application’s conditional access policy</span></span>

<span data-ttu-id="5712e-330">toocheck, případně k ověření zásad jednu aplikaci aktuálně nakonfigurovaný podmíněný přístup:</span><span class="sxs-lookup"><span data-stu-id="5712e-330">toocheck or validate a single application’s currently configured conditional access policy:</span></span>

1.  <span data-ttu-id="5712e-331">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-331">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-332">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-332">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-333">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-333">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-334">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-334">click **Enterprise applications** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-335">Klikněte na tlačítko **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5712e-335">click **All applications**.</span></span>

6.  <span data-ttu-id="5712e-336">Vyhledávání pro aplikace hello zájem o nebo hello uživatel se pokouší toosign v aplikaci tooby zobrazení id, nebo název aplikace.</span><span class="sxs-lookup"><span data-stu-id="5712e-336">Search for hello application you are interested in, or hello user is attempting toosign in tooby application display name or application id.</span></span>

     >[!NOTE]
     ><span data-ttu-id="5712e-337">Pokud nevidíte hello aplikace, které hledáte, klikněte na tlačítko hello **filtru** tlačítko a rozbalte hello oboru hello seznamu příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5712e-337">If you don’t see hello application you are looking for, click hello **Filter** button and expand hello scope of hello list too**All applications**.</span></span> <span data-ttu-id="5712e-338">Pokud chcete toosee více sloupců, klikněte na tlačítko hello **sloupce** tlačítko tooadd další podrobnosti pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="5712e-338">If you want toosee more columns, click hello **Columns** button tooadd additional details for your applications.</span></span>
     >
     >

7.  <span data-ttu-id="5712e-339">Klikněte na tlačítko hello **podmíněného přístupu** navigační položka.</span><span class="sxs-lookup"><span data-stu-id="5712e-339">click hello **Conditional access** navigation item.</span></span>

8.  <span data-ttu-id="5712e-340">Klikněte na tlačítko hello zásady, které vás zajímají kontroly.</span><span class="sxs-lookup"><span data-stu-id="5712e-340">click hello policy you are interested in inspecting.</span></span>

9.  <span data-ttu-id="5712e-341">Zkontrolujte, že neexistují žádné konkrétní podmínky, přiřazení nebo jiná nastavení, které blokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5712e-341">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

     >[!NOTE]
     ><span data-ttu-id="5712e-342">Může přát zakázat tootemporarily této zásady tooensure ji nemají vliv na sign in toodo se sada hello **povolit zásady** přepnutí příliš**ne** a klikněte na tlačítko hello **Uložit** tlačítko .</span><span class="sxs-lookup"><span data-stu-id="5712e-342">You may wish tootemporarily disable this policy tooensure it is not affecting sign ins. toodo this, set hello **Enable policy** toggle too**No** and click hello **Save** button.</span></span>
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a><span data-ttu-id="5712e-343">Zakázat zásadu podmíněného přístupu konkrétním</span><span class="sxs-lookup"><span data-stu-id="5712e-343">Disable a specific conditional access policy</span></span>

<span data-ttu-id="5712e-344">toocheck, případně k ověření zásad jednoho podmíněného přístupu:</span><span class="sxs-lookup"><span data-stu-id="5712e-344">toocheck or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="5712e-345">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5712e-345">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5712e-346">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-346">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5712e-347">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5712e-347">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5712e-348">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5712e-348">click **Enterprise applications** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5712e-349">Klikněte na tlačítko hello **podmíněného přístupu** navigační položka.</span><span class="sxs-lookup"><span data-stu-id="5712e-349">click hello **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="5712e-350">Klikněte na tlačítko hello zásady, které vás zajímají kontroly.</span><span class="sxs-lookup"><span data-stu-id="5712e-350">click hello policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="5712e-351">Zakažte zásadu hello nastavení hello **povolit zásady** přepnutí příliš**ne** a klikněte na tlačítko hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5712e-351">Disable hello policy by setting hello **Enable policy** toggle too**No** and click hello **Save** button.</span></span>

## <a name="problems-with-application-consent"></a><span data-ttu-id="5712e-352">Problémy s aplikací souhlasu</span><span class="sxs-lookup"><span data-stu-id="5712e-352">Problems with application consent</span></span>

<span data-ttu-id="5712e-353">Přístup k aplikaci můžete blokovat, protože nedošlo k operaci souhlasu hello příslušná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="5712e-353">Application access can be blocked because hello proper permissions consent operation has not occurred.</span></span> <span data-ttu-id="5712e-354">Tady jsou některé způsoby řešení problémů a vyřešit problémy s aplikací souhlasu:</span><span class="sxs-lookup"><span data-stu-id="5712e-354">Below are some ways you can troubleshoot and solve application consent issues:</span></span>

-   [<span data-ttu-id="5712e-355">Provedení operace individuální souhlasu</span><span class="sxs-lookup"><span data-stu-id="5712e-355">Perform a user-level consent operation</span></span>](#perform-a-user-level-consent-operation)

-   [<span data-ttu-id="5712e-356">Provedení operace správce úroveň souhlasu pro žádnou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5712e-356">Perform administrator-level consent operation for any application</span></span>](#perform-administrator-level-consent-operation-for-any-application)

-   [<span data-ttu-id="5712e-357">Provést na úrovni správce souhlasu pro jednoho klienta aplikace</span><span class="sxs-lookup"><span data-stu-id="5712e-357">Perform administrator-level consent for a single-tenant application</span></span>](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [<span data-ttu-id="5712e-358">Provést na úrovni správce souhlasu pro víceklientské aplikace</span><span class="sxs-lookup"><span data-stu-id="5712e-358">Perform administrator-level consent for a multi-tenant application</span></span>](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a><span data-ttu-id="5712e-359">Provedení operace individuální souhlasu</span><span class="sxs-lookup"><span data-stu-id="5712e-359">Perform a user-level consent operation</span></span>

-   <span data-ttu-id="5712e-360">Pro každou Open ID Connect-povolit aplikaci, která vyžaduje oprávnění navigace přihlašovací obrazovka, toohello aplikace provede uživatelská aplikace toohello úrovně souhlasu pro hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5712e-360">For any Open ID Connect-enabled application that requests permissions, navigating toohello application’s sign in screen performs a user level consent toohello application for hello signed-in user.</span></span>

-   <span data-ttu-id="5712e-361">Pokud chcete toodo to prostřednictvím kódu programu, najdete v části [vyžaduje souhlas jednotlivé uživatele](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span><span class="sxs-lookup"><span data-stu-id="5712e-361">If you wish toodo this programmatically, see [Requesting individual user consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span></span>

### <a name="perform-administrator-level-consent-operation-for-any-application"></a><span data-ttu-id="5712e-362">Provedení operace správce úroveň souhlasu pro žádnou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5712e-362">Perform administrator-level consent operation for any application</span></span>

-   <span data-ttu-id="5712e-363">Pro **jenom aplikace vyvinuté pomocí modelu aplikace hello V1**, můžete vynutit tento správce úrovně souhlasu toooccur přidáním "**? řádku = správce\_souhlas**" toohello konec Přihlašovací adresa URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="5712e-363">For **only applications developed using hello V1 application model**, you can force this administrator level consent toooccur by adding “**?prompt=admin\_consent**” toohello end of an application’s sign in URL.</span></span>

-   <span data-ttu-id="5712e-364">Pro **všechny aplikace vyvinuté pomocí modelu aplikace hello V2**, můžete vynutit tento správce úroveň souhlasu toooccur podle pokynů hello pod hello **žádostí o oprávnění hello z adresáře správce** části [používá koncový bod souhlas správce hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="5712e-364">For **any application developed using hello V2 application model**, you can enforce this administrator-level consent toooccur by following hello instructions under hello **Request hello permissions from a directory admin** section of [Using hello admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a><span data-ttu-id="5712e-365">Provést na úrovni správce souhlasu pro jednoho klienta aplikace</span><span class="sxs-lookup"><span data-stu-id="5712e-365">Perform administrator-level consent for a single-tenant application</span></span>

-   <span data-ttu-id="5712e-366">Pro **jednoho klienta aplikace** , žádostí o oprávnění (jako jsou ty, které jste vývoji nebo vlastní ve vaší organizaci), můžete provést **správu úroveň souhlasu** operace jménem všechny uživatelé přihlášení jako globální správce a kliknete na hello **udělit oprávnění** tlačítko hello horní části hello **aplikace registru -&gt; všechny aplikace -&gt; vyberte aplikaci - &gt; Požadovaných oprávnění** okno.</span><span class="sxs-lookup"><span data-stu-id="5712e-366">For **single-tenant applications** that request permissions (like those you are developing or own in your organization), you can perform an **administrative-level consent** operation on behalf of all users by signing in as a Global Administrator and clicking on hello **Grant permissions** button at hello top of hello **Application Registry -&gt; All Applications -&gt; Select an App -&gt; Required Permissions** blade.</span></span>

-   <span data-ttu-id="5712e-367">Pro **všechny aplikace vyvinuté pomocí hello V1 nebo V2 aplikačního modelu**, můžete vynutit tento správce úroveň souhlasu toooccur podle pokynů hello pod hello **žádostí o oprávnění hello z Správce Directory** části [používá koncový bod souhlas správce hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="5712e-367">For **any application developed using hello V1 or V2 application model**, you can enforce this administrator-level consent toooccur by following hello instructions under hello **Request hello permissions from a directory admin** section of [Using hello admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a><span data-ttu-id="5712e-368">Provést na úrovni správce souhlasu pro víceklientské aplikace</span><span class="sxs-lookup"><span data-stu-id="5712e-368">Perform administrator-level consent for a multi-tenant application</span></span>

-   <span data-ttu-id="5712e-369">Pro **víceklientským aplikacím** této žádosti o oprávnění (jako jsou aplikace třetích stran nebo sama vyvinula společnost Microsoft,), můžete provést **správu úroveň souhlasu** operaci.</span><span class="sxs-lookup"><span data-stu-id="5712e-369">For **multi-tenant applications** that request permissions (like an application a third party, or Microsoft, develops), you can perform an **administrative-level consent** operation.</span></span> <span data-ttu-id="5712e-370">Přihlaste se jako globální správce a kliknete na hello **udělit oprávnění** tlačítko hello **podnikové aplikace –&gt; všechny aplikace -&gt; vyberte aplikaci -&gt; Oprávnění** okno (k dispozici brzy).</span><span class="sxs-lookup"><span data-stu-id="5712e-370">Sign in as a Global Administrator and clicking on hello **Grant permissions** button under hello **Enterprise Applications -&gt; All Applications -&gt; Select an App -&gt; Permissions** blade (available soon).</span></span>

-   <span data-ttu-id="5712e-371">Tento správce úroveň souhlasu toooccur může také vynucovat podle pokynů hello pod hello **hello oprávnění požádat správce directory** části [pomocí koncový bod souhlas správce hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="5712e-371">You can also enforce this administrator-level consent toooccur by following hello instructions under hello **Request hello permissions from a directory admin** section of [Using hello admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5712e-372">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5712e-372">Next steps</span></span>
[<span data-ttu-id="5712e-373">Pomocí koncový bod souhlas správce hello</span><span class="sxs-lookup"><span data-stu-id="5712e-373">Using hello admin consent endpoint</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

