---
title: "Potíže při přihlašování k aplikaci Microsoft | Microsoft Docs"
description: "Řešení běžných potíží potýkají při přihlašování k Applications Microsoft první strany pomocí služby Azure AD (např. Office 365)"
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
ms.openlocfilehash: 5638434270ee82d2b9737ea8eed8b5a8c62f7121
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
## <a name="problems-signing-in-to-a-microsoft-application"></a><span data-ttu-id="4d3e5-103">Potíže při přihlašování k aplikaci Microsoft</span><span class="sxs-lookup"><span data-stu-id="4d3e5-103">Problems signing in to a Microsoft application</span></span>

<span data-ttu-id="4d3e5-104">Microsoft Applications (např. Office 365 Exchange, SharePoint, Yammer atd.) jsou přiřazeny a trochu jinak než 3. aplikace SaaS strany nebo jiné aplikace, které integrují s Azure AD pro jednotné přihlašování na spravovat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-104">Microsoft Applications (like Office 365 Exchange, SharePoint, Yammer, etc.) are assigned and managed a bit differently than 3rd party SaaS applications or other applications you integrate with Azure AD for single sign on.</span></span>

<span data-ttu-id="4d3e5-105">Existují tři hlavní způsoby, že uživatel může získat přístup k aplikaci Microsoft publikovaná.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-105">There are three main ways that a user can get access to a Microsoft-published application.</span></span>

-   <span data-ttu-id="4d3e5-106">Pro aplikace v Office 365 nebo jiné placené sady, mají uživatelé udělen přístup prostřednictvím **přiřazení licence** buď přímo do uživatelského účtu, nebo prostřednictvím skupiny pomocí funkce přiřazení našeho na základě skupin licencí.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-106">For applications in the Office 365 or other paid suites, users are granted access through **license assignment** either directly to their user account, or through a group using our group-based license assignment capability.</span></span>

-   <span data-ttu-id="4d3e5-107">Pro aplikace, které Microsoftu nebo třetí strany publikuje pro všechny volně, může být uživatelé udělen přístup prostřednictvím **souhlas uživatele**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-107">For applications that Microsoft or a Third Party publishes freely for anyone to use, users may be granted access through **user consent**.</span></span> <span data-ttu-id="4d3e5-108">This0 znamená, že se přihlásit k aplikaci s jejich Azure AD pracovního nebo školního účtu a povolit ji tak, aby měl přístup k některé omezenou sadu dat na svůj účet.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-108">This0 means that they sign in to the application with their Azure AD Work or School account and allow it to have access to some limited set of data on their account.</span></span>

-   <span data-ttu-id="4d3e5-109">Pro aplikace, které společnost Microsoft nebo 3rd strany publikuje pro všechny volně, uživatelé také udělit přístup přes **souhlas správce**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-109">For applications that Microsoft or a 3rd Party publishes freely for anyone to use, users may also be granted access through **administrator consent**.</span></span> <span data-ttu-id="4d3e5-110">To znamená, že správce zjistí, že aplikace může být používán všichni uživatelé v organizaci, takže se přihlásit k aplikaci pomocí účtu globálního správce a udělit přístup všem uživatelům v organizaci.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-110">This means that an administrator has determined the application may be used by everyone in the organization, so they sign in to the application with a Global Administrator account and grant access to everyone in the organization.</span></span>

<span data-ttu-id="4d3e5-111">Chcete-li vyřešit problém, můžete začít s [obecné problémových oblastí aplikace přístup k zvažte](#general-problem-areas-with-application-access-to-consider) a přečtěte si [návod: postup řešení potíží s přístupem Microsoft Application](#walkthrough-steps-to-troubleshoot-microsoft-application-access) získat na podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-111">To troubleshoot your issue, start with the [General Problem Areas with Application Access to consider](#general-problem-areas-with-application-access-to-consider) and then read the [Walkthrough: Steps to troubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access) to get into the details.</span></span>

## <a name="general-problem-areas-with-application-access-to-consider"></a><span data-ttu-id="4d3e5-112">Obecné problémových oblastí aplikace přístup ke zvážení</span><span class="sxs-lookup"><span data-stu-id="4d3e5-112">General Problem Areas with Application Access to consider</span></span>

<span data-ttu-id="4d3e5-113">Níže uvádíme seznam obecné problémových oblastí, které můžete rozbalit Pokud máte představu o kde začít, ale doporučujeme, abyste si přečetli návodu tak, aby mohli rychle začít: [návod: postup řešení potíží s přístupem Microsoft Application](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span><span class="sxs-lookup"><span data-stu-id="4d3e5-113">Below is a list of the general problem areas that you can drill into if you have an idea of where to start, but we recommend you read the walkthrough to get going quickly: [Walkthrough: Steps to troubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span></span>

-   [<span data-ttu-id="4d3e5-114">Problémy s uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-114">Problems with the user’s account</span></span>](#problems-with-the-users-account)

-   [<span data-ttu-id="4d3e5-115">Problémy se skupinami</span><span class="sxs-lookup"><span data-stu-id="4d3e5-115">Problems with groups</span></span>](#problems-with-groups)

-   [<span data-ttu-id="4d3e5-116">Problémy se zásadami podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-116">Problems with conditional access policies</span></span>](#problems-with-conditional-access-policies)

-   [<span data-ttu-id="4d3e5-117">Problémy s aplikací souhlasu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-117">Problems with application consent</span></span>](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a><span data-ttu-id="4d3e5-118">Postup řešení potíží s přístupem Microsoft Application</span><span class="sxs-lookup"><span data-stu-id="4d3e5-118">Steps to troubleshoot Microsoft Application access</span></span>

<span data-ttu-id="4d3e5-119">Níže jsou některé běžné problémy zaměstnance spustit do při jejich uživatelé nemůžete se přihlásit k aplikaci společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-119">Below are some common issues folks run into when their users cannot sign in to a Microsoft application.</span></span>

-   <span data-ttu-id="4d3e5-120">Běžné problémy a proveďte nejprve kontrolu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-120">General issues to check first</span></span>

  * <span data-ttu-id="4d3e5-121">Ujistěte se, že uživatel je přihlášení k **opravte adresu URL** a ne místní aplikace adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-121">Make sure the user is signing in to the **correct URL** and not a local application URL.</span></span>

  * <span data-ttu-id="4d3e5-122">Zkontrolujte, zda je účet uživatele **není uzamčen.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-122">Make sure the user’s account is **not locked out.**</span></span>

  * <span data-ttu-id="4d3e5-123">Zajistěte, aby **uživatelský účet existuje** v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-123">Make sure the **user’s account exists** in Azure Active Directory.</span></span> [<span data-ttu-id="4d3e5-124">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4d3e5-124">Check if a user account exists in Azure Active Directory</span></span>](#problems-with-the-users-account)

  * <span data-ttu-id="4d3e5-125">Zkontrolujte, zda je účet uživatele **povoleno** pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-125">Make sure the user’s account is **enabled** for sign ins.</span></span> [<span data-ttu-id="4d3e5-126">Zkontrolujte stav účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-126">Check a user’s account status</span></span>](#problems-with-the-users-account)

  * <span data-ttu-id="4d3e5-127">Zkontrolujte, že uživatele **není platnost nebo zapomenete heslo.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-127">Make sure the user’s **password is not expired or forgotten.**</span></span> <span data-ttu-id="4d3e5-128">[Resetovat heslo uživatele](#reset-a-users-password) nebo [povolit samoobslužné resetování hesla](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span><span class="sxs-lookup"><span data-stu-id="4d3e5-128">[Reset a user’s password](#reset-a-users-password) or [Enable self-service password reset](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span></span>

   * <span data-ttu-id="4d3e5-129">Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-129">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span> <span data-ttu-id="4d3e5-130">[Zkontrolujte stav služby Multi-Factor authentication uživatele](#check-a-users-multi-factor-authentication-status) nebo [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="4d3e5-130">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

   * <span data-ttu-id="4d3e5-131">Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-131">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span> <span data-ttu-id="4d3e5-132">[Zkontrolujte zásady podmíněného přístupu konkrétním](#problems-with-conditional-access-policies) nebo [zkontrolujte zásady podmíněného přístupu pro konkrétní aplikaci](#check-a-specific-applications-conditional-access-policy) nebo [zakázat zásadu podmíněného přístupu konkrétním](#disable-a-specific-conditional-access-policy)</span><span class="sxs-lookup"><span data-stu-id="4d3e5-132">[Check a specific conditional access policy](#problems-with-conditional-access-policies) or [Check a specific application’s conditional access policy](#check-a-specific-applications-conditional-access-policy) or [Disable a specific conditional access policy](#disable-a-specific-conditional-access-policy)</span></span>

   * <span data-ttu-id="4d3e5-133">Ujistěte se, že uživatele **ověřování kontaktní údaje** je aktuální povolit Multi-Factor Authentication nebo podmíněného přístupu zásad, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-133">Make sure that a user’s **authentication contact info** is up to date to allow Multi-Factor Authentication or Conditional Access policies to be enforced.</span></span> <span data-ttu-id="4d3e5-134">[Zkontrolujte stav služby Multi-Factor authentication uživatele](#check-a-users-multi-factor-authentication-status) nebo [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="4d3e5-134">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

-   <span data-ttu-id="4d3e5-135">Pro **Microsoft** **aplikace, které vyžadují licenci** (např. Office 365), tady jsou některé specifické problémy zkontrolujte po nevyloučí obecné problémy výše:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-135">For **Microsoft** **applications that require a license** (like Office365), here are some specific issues to check once you have ruled out the general issues above:</span></span>

   * <span data-ttu-id="4d3e5-136">Ujistěte se, uživatele nebo má **přiřazenou licenci.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-136">Ensure the user or has a **license assigned.**</span></span> <span data-ttu-id="4d3e5-137">[Zkontrolujte uživatele přiřazené licence](#check-a-users-assigned-licenses) nebo [Zkontrolujte skupiny přiřazené licence](#check-a-groups-assigned-licenses)</span><span class="sxs-lookup"><span data-stu-id="4d3e5-137">[Check a user’s assigned licenses](#check-a-users-assigned-licenses) or [Check a group’s assigned licenses](#check-a-groups-assigned-licenses)</span></span>

   * <span data-ttu-id="4d3e5-138">Pokud je licence **přiřazené** **statická skupina**, ujistěte se, že **uživatel je členem** této skupiny.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-138">If the license is **assigned to a** **static group**, ensure that the **user is a member** of that group.</span></span> [<span data-ttu-id="4d3e5-139">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="4d3e5-139">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   * <span data-ttu-id="4d3e5-140">Pokud je licence **přiřazené** **dynamická skupina**, ujistěte se, že **je dynamická skupina pravidlo správně nastavené**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-140">If the license is **assigned to a** **dynamic group**, ensure that the **dynamic group rule is set correctly**.</span></span> [<span data-ttu-id="4d3e5-141">Zkontrolujte kritéria členství dynamické skupiny</span><span class="sxs-lookup"><span data-stu-id="4d3e5-141">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

   * <span data-ttu-id="4d3e5-142">Pokud je licence **přiřazené** **dynamická skupina**, zkontrolujte, zda dynamická skupina má **bylo dokončeno zpracování** jeho členství a že **uživatel je členem** (to může trvat nějakou dobu).</span><span class="sxs-lookup"><span data-stu-id="4d3e5-142">If the license is **assigned to a** **dynamic group**, ensure that the dynamic group has **finished processing** its membership and that the **user is a member** (this can take some time).</span></span> [<span data-ttu-id="4d3e5-143">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="4d3e5-143">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   *  <span data-ttu-id="4d3e5-144">Jakmile budete mít jistotu, je přiřazena licence, zkontrolujte, zda je licence **nevypršela**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-144">Once you make sure the license is assigned, make sure the license is **not expired**.</span></span>

   *  <span data-ttu-id="4d3e5-145">Zkontrolujte, zda je licence **pro aplikaci** uživatelé přistupují.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-145">Make sure the license is **for the application** they are accessing.</span></span>

-   <span data-ttu-id="4d3e5-146">Pro **Microsoft** **aplikace, které nevyžadují licenci**, zde jsou některé další body ke kontrole:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-146">For **Microsoft** **applications that don’t require a license**, here are some other things to check:</span></span>

   * <span data-ttu-id="4d3e5-147">Pokud aplikace požaduje **oprávnění na úrovni uživatele** (například "přístup k poštovní schránka tohoto uživatele"), ujistěte se, že uživatel přihlásí k aplikaci a byla provedena **individuální souhlasu operaci** umožníte přístup k jeho data aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-147">If the application is requesting **user-level permissions** (for example “Access this user’s mailbox”), make sure that the user has signed in to the application and has performed a **user-level consent operation** to let the application access her data.</span></span>

   * <span data-ttu-id="4d3e5-148">Pokud aplikace požaduje **oprávnění na úrovni správce** (například "přístup k poštovním schránkám všechny uživatelské"), ujistěte se, že byla provedena globálního správce **operace na úrovni správce souhlas jménem všichni uživatelé** v organizaci.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-148">If the application is requesting **administrator-level permissions** (for example “Access all user’s mailboxes”), make sure that a Global Administrator has performed an **administrator-level consent operation on behalf of all users** in the organization.</span></span>

## <a name="problems-with-the-users-account"></a><span data-ttu-id="4d3e5-149">Problémy s uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-149">Problems with the user’s account</span></span>

<span data-ttu-id="4d3e5-150">Z důvodu problému s uživatelem, který je přiřazen k aplikaci lze blokovat přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-150">Application access can be blocked due to a problem with a user that is assigned to the application.</span></span> <span data-ttu-id="4d3e5-151">Tady jsou některé způsoby řešení problémů a řešení problémů s uživateli a jejich nastavení účtu:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-151">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="4d3e5-152">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4d3e5-152">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="4d3e5-153">Zkontrolujte stav účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-153">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="4d3e5-154">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-154">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="4d3e5-155">Povolení samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="4d3e5-155">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="4d3e5-156">Zkontrolujte stav služby Multi-Factor authentication uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-156">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="4d3e5-157">Zkontrolovat kontaktní informace o ověřování uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-157">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="4d3e5-158">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="4d3e5-158">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="4d3e5-159">Zkontrolujte uživatele přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="4d3e5-159">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="4d3e5-160">Přiřazení licence uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-160">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="4d3e5-161">Zkontrolujte, jestli uživatelský účet existuje v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4d3e5-161">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="4d3e5-162">Pokud chcete zkontrolovat, zda účet uživatele je přítomen, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-162">To check if a user’s account is present, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-163">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-163">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-164">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-164">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-165">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-165">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-166">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-166">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-167">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-167">click **All users**.</span></span>

6.  <span data-ttu-id="4d3e5-168">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-168">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-169">Zkontrolujte vlastnosti objektu uživatele a ujistěte se, že zobrazují se podle očekávání a nebyla nalezena žádná data.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-169">Check the properties of the user object to be sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="4d3e5-170">Zkontrolujte stav účtu uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-170">Check a user’s account status</span></span>

<span data-ttu-id="4d3e5-171">Pokud chcete zkontrolovat stav účtu uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-171">To check a user’s account status, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-172">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-172">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-173">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-173">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-174">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-174">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-175">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-175">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-176">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-176">click **All users**.</span></span>

6.  <span data-ttu-id="4d3e5-177">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-177">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-178">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-178">click **Profile**.</span></span>

8.  <span data-ttu-id="4d3e5-179">V části **nastavení** Ujistěte se, že **bloku přihlásit** je nastaven na **ne**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-179">Under **Settings** ensure that **Block sign in** is set to **No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="4d3e5-180">Resetovat heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-180">Reset a user’s password</span></span>

<span data-ttu-id="4d3e5-181">Pokud chcete resetovat heslo uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-181">To reset a user’s password, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-182">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-182">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-183">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-183">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-184">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-184">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-185">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-185">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-186">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-186">click **All users**.</span></span>

6.  <span data-ttu-id="4d3e5-187">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-187">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-188">klikněte **resetovat heslo** tlačítka v horní části okna uživatele.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-188">click the **Reset password** button at the top of the user blade.</span></span>

8.  <span data-ttu-id="4d3e5-189">klikněte **resetovat heslo** na tlačítko **resetovat heslo** okno, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-189">click the **Reset password** button on the **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="4d3e5-190">Kopírování **dočasné heslo** nebo **zadat nové heslo** pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-190">Copy the **temporary password** or **enter a new password** for the user.</span></span>

10. <span data-ttu-id="4d3e5-191">Komunikaci tohoto nového hesla pro uživatele, budou muset změnit toto heslo při příštím přihlášení v do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-191">Communicate this new password to the user, they be required to change this password during their next sign in to Azure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="4d3e5-192">Povolit samoobslužné resetování hesla</span><span class="sxs-lookup"><span data-stu-id="4d3e5-192">Enable self-service password reset</span></span>

<span data-ttu-id="4d3e5-193">Pokud chcete povolit samoobslužné resetování hesla, postupujte podle následujících kroků nasazení:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-193">To enable self-service password reset, follow the deployment steps below:</span></span>

-   [<span data-ttu-id="4d3e5-194">Umožnění resetování hesel služby Azure Active Directory uživatelům</span><span class="sxs-lookup"><span data-stu-id="4d3e5-194">Enable users to reset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="4d3e5-195">Povolit uživatelům resetovat nebo změnit své heslo místní služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="4d3e5-195">Enable users to reset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="4d3e5-196">Zkontrolujte stav služby Multi-Factor authentication uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-196">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="4d3e5-197">Pokud chcete zkontrolovat stav uživatele služby Multi-Factor authentication, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-197">To check a user’s multi-factor authentication status, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-198">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-198">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-199">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-199">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-200">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-200">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-201">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-201">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-202">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-202">click **All users**.</span></span>

6.  <span data-ttu-id="4d3e5-203">klikněte **Multi-Factor Authentication** tlačítka v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-203">click the **Multi-Factor Authentication** button at the top of the blade.</span></span>

7.  <span data-ttu-id="4d3e5-204">Jednou **portálu pro správu služby Multi-Factor Authentication** zatížení, ujistěte se, jsou na **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-204">Once the **Multi-Factor Authentication Administration Portal** loads, ensure you are on the **Users** tab.</span></span>

8.  <span data-ttu-id="4d3e5-205">Vyhledejte uživatele pomocí vyhledávání, filtrování a řazení v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-205">Find the user in the list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="4d3e5-206">Vyberte uživatele ze seznamu uživatelů a **povolit**, **zakázat**, nebo **vynutit** služby Multi-Factor authentication podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-206">Select the user from the list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

  * <span data-ttu-id="4d3e5-207">**Poznámka:**: Pokud je uživatel v **vynucené** stavu, může je nastaven **zakázané** dočasně a dát jim zpět do svého účtu.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-207">**Note**: If a user is in an **Enforced** state, you may set them to **Disabled** temporarily to let them back into their account.</span></span> <span data-ttu-id="4d3e5-208">Jakmile se zpět, můžete změnit svůj stav na **povoleno** znovu, aby je znovu zaregistrovat své kontaktní informace při příštím přihlášení v vyžadují.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-208">Once they are back in, you can then change their state to **Enabled** again to require them to re-register their contact information during their next sign in.</span></span> <span data-ttu-id="4d3e5-209">Alternativně můžete podle kroků v [zkontrolovat kontaktní informace o ověřování uživatele](#check-a-users-authentication-contact-info) ověření nebo pro ně nastavit tato data.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-209">Alternatively, you can follow the steps in the [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) to verify or set this data for them.</span></span>

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="4d3e5-210">Zkontrolovat kontaktní informace o ověřování uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-210">Check a user’s authentication contact info</span></span>

<span data-ttu-id="4d3e5-211">Pokud chcete zkontrolovat uživatele ověřování kontaktní údaje pro službu Multi-Factor authentication, podmíněného přístupu, ochrany identit a resetování hesla, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-211">To check a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-212">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-212">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-213">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-213">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-214">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-214">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-215">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-215">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-216">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-216">click **All users**.</span></span>

6.  <span data-ttu-id="4d3e5-217">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-217">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-218">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-218">click **Profile**.</span></span>

8.  <span data-ttu-id="4d3e5-219">Přejděte dolů k položce **ověřování kontaktní údaje**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-219">Scroll down to **Authentication contact info**.</span></span>

9.  <span data-ttu-id="4d3e5-220">**Zkontrolujte** data zaregistrovaný pro uživatele a aktualizovat podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-220">**Review** the data registered for the user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="4d3e5-221">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="4d3e5-221">Check a user’s group memberships</span></span>

<span data-ttu-id="4d3e5-222">Pokud chcete zkontrolovat členství uživatele ve skupinách, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-222">To check a user’s group memberships, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-223">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-223">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-224">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-224">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-225">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-225">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-226">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-226">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-227">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-227">click **All users**.</span></span>

6.  <span data-ttu-id="4d3e5-228">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-228">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-229">Klikněte na tlačítko **skupiny** zobrazíte, které skupiny uživatel je členem skupiny.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-229">click **Groups** to see which groups the user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="4d3e5-230">Zkontrolujte uživatele přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="4d3e5-230">Check a user’s assigned licenses</span></span>

<span data-ttu-id="4d3e5-231">Pokud chcete zkontrolovat uživatele přiřazené licence, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-231">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-232">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-232">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-233">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-233">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-234">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-234">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-235">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-235">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-236">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-236">click **All users**.</span></span>

6.  <span data-ttu-id="4d3e5-237">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-237">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-238">Klikněte na tlačítko **licence** zobrazíte, které uživatel aktuálně licence jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-238">click **Licenses** to see which licenses the user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="4d3e5-239">Přiřazení licence uživatele</span><span class="sxs-lookup"><span data-stu-id="4d3e5-239">Assign a user a license</span></span> 

<span data-ttu-id="4d3e5-240">Chcete-li přiřadit licenci pro uživatele, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-240">To assign a license to a user, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-241">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-241">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-242">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-242">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-243">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-243">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-244">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-244">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-245">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-245">click **All users**.</span></span>

6.  <span data-ttu-id="4d3e5-246">**Hledání** pro uživatele, které vás zajímají a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-246">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-247">Klikněte na tlačítko **licence** zobrazíte, které uživatel aktuálně licence jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-247">click **Licenses** to see which licenses the user currently has assigned.</span></span>

8.  <span data-ttu-id="4d3e5-248">klikněte **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-248">click the **Assign** button.</span></span>

9.  <span data-ttu-id="4d3e5-249">Vyberte **jeden nebo více produktů** ze seznamu dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-249">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="4d3e5-250">**Volitelné** klikněte na tlačítko **přiřazení možností** položky granularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-250">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="4d3e5-251">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-251">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="4d3e5-252">Klikněte **přiřadit** tlačítko tyto licence přiřadit tomuto uživateli.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-252">Click the **Assign** button to assign these licenses to this user.</span></span>

## <a name="problems-with-groups"></a><span data-ttu-id="4d3e5-253">Problémy se skupinami</span><span class="sxs-lookup"><span data-stu-id="4d3e5-253">Problems with groups</span></span>

<span data-ttu-id="4d3e5-254">Z důvodu problému s skupinu, která je přiřazena k aplikaci lze blokovat přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-254">Application access can be blocked due to a problem with a group that is assigned to the application.</span></span> <span data-ttu-id="4d3e5-255">Tady jsou některé způsoby řešení problémů a řešení problémů s skupin a členství ve skupinách:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-255">Below are some ways you can troubleshoot and solve problems with groups and group memberships:</span></span>

-   [<span data-ttu-id="4d3e5-256">Zkontrolovat členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="4d3e5-256">Check a group’s membership</span></span>](#check-a-groups-membership)

-   [<span data-ttu-id="4d3e5-257">Zkontrolujte kritéria členství dynamické skupiny</span><span class="sxs-lookup"><span data-stu-id="4d3e5-257">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

-   [<span data-ttu-id="4d3e5-258">Zkontrolujte skupiny přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="4d3e5-258">Check a group’s assigned licenses</span></span>](#check-a-groups-assigned-licenses)

-   [<span data-ttu-id="4d3e5-259">Znovu zpracovat skupině licencí</span><span class="sxs-lookup"><span data-stu-id="4d3e5-259">Reprocess a group’s licenses</span></span>](#reprocess-a-groups-licenses)

-   [<span data-ttu-id="4d3e5-260">Přiřazení skupiny licencí</span><span class="sxs-lookup"><span data-stu-id="4d3e5-260">Assign a group a license</span></span>](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a><span data-ttu-id="4d3e5-261">Zkontrolovat členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="4d3e5-261">Check a group’s membership</span></span>

<span data-ttu-id="4d3e5-262">Pokud chcete zkontrolovat členství ve skupině, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-262">To check a group’s membership, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-263">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-263">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-264">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-264">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-265">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-265">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-266">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-266">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-267">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-267">click **All groups**.</span></span>

6.  <span data-ttu-id="4d3e5-268">**Hledání** pro skupinu vás zajímá a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-268">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-269">Klikněte na tlačítko **členy** zobrazíte seznam uživatelů přiřadí do této skupiny.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-269">click **Members** to review the list of users assigned to this group.</span></span>

### <a name="check-a-dynamic-groups-membership-criteria"></a><span data-ttu-id="4d3e5-270">Zkontrolujte kritéria členství dynamické skupiny</span><span class="sxs-lookup"><span data-stu-id="4d3e5-270">Check a dynamic group’s membership criteria</span></span> 

<span data-ttu-id="4d3e5-271">Pokud chcete zkontrolovat kritéria členství dynamické skupiny, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-271">To check a dynamic group’s membership criteria, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-272">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-272">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-273">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-273">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-274">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-274">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-275">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-275">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-276">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-276">click **All groups**.</span></span>

6.  <span data-ttu-id="4d3e5-277">**Hledání** pro skupinu vás zajímá a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-277">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-278">Klikněte na tlačítko **pravidla dynamické členství.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-278">click **Dynamic membership rules.**</span></span>

8.  <span data-ttu-id="4d3e5-279">Zkontrolujte **jednoduché** nebo **rozšířené** pravidlo definované pro tuto skupinu a zkontrolujte, zda mají být členem této skupiny uživatele splňuje tato kritéria.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-279">Review the **simple** or **advanced** rule defined for this group and ensure that the user you want to be a member of this group meets these criteria.</span></span>

### <a name="check-a-groups-assigned-licenses"></a><span data-ttu-id="4d3e5-280">Zkontrolujte skupiny přiřazené licence</span><span class="sxs-lookup"><span data-stu-id="4d3e5-280">Check a group’s assigned licenses</span></span>

<span data-ttu-id="4d3e5-281">Pokud chcete zkontrolovat skupiny přiřazené licence, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-281">To check a group’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-282">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-282">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-283">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-283">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-284">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-284">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-285">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-285">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-286">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-286">click **All groups**.</span></span>

6.  <span data-ttu-id="4d3e5-287">**Hledání** pro skupinu vás zajímá a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-287">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-288">Klikněte na tlačítko **licence** zobrazíte, které skupiny licencí aktuálně jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-288">click **Licenses** to see which licenses the group currently has assigned.</span></span>

### <a name="reprocess-a-groups-licenses"></a><span data-ttu-id="4d3e5-289">Znovu zpracovat skupině licencí</span><span class="sxs-lookup"><span data-stu-id="4d3e5-289">Reprocess a group’s licenses</span></span>

<span data-ttu-id="4d3e5-290">Opětovné zpracování skupiny přiřazené licence, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-290">To reprocess a group’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-291">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-291">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-292">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-292">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-293">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-293">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-294">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-294">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-295">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-295">click **All groups**.</span></span>

6.  <span data-ttu-id="4d3e5-296">**Hledání** pro skupinu vás zajímá a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-296">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-297">Klikněte na tlačítko **licence** zobrazíte, které skupiny licencí aktuálně jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-297">click **Licenses** to see which licenses the group currently has assigned.</span></span>

8.  <span data-ttu-id="4d3e5-298">klikněte **znovu zpracovat** tlačítko zkontrolujte, zda mají členy této skupiny přiřazené licence jsou aktuální.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-298">click the **Reprocess** button to ensure that the licenses assigned to this group’s members are up-to-date.</span></span> <span data-ttu-id="4d3e5-299">To může trvat dlouhou dobu, v závislosti na velikost a složitost skupiny.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-299">This may take a long time, depending on the size and complexity of the group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="4d3e5-300">Uděláte to tak rychleji, vezměte v úvahu dočasně přiřazení licence uživatele přímo.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-300">To do this faster, consider temporarily assigning a license to the user directly.</span></span> <span data-ttu-id="4d3e5-301">[Přiřazení licence uživatele](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="4d3e5-301">[Assign a user a license](#problems-with-application-consent).</span></span>
   >
   >

### <a name="assign-a-group-a-license"></a><span data-ttu-id="4d3e5-302">Přiřazení skupiny licencí</span><span class="sxs-lookup"><span data-stu-id="4d3e5-302">Assign a group a license</span></span>

<span data-ttu-id="4d3e5-303">Přiřadit licenci pro skupinu, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-303">To assign a license to a group, follow the steps below:</span></span>

1.  <span data-ttu-id="4d3e5-304">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-304">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-305">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-305">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-306">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-306">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-307">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-307">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-308">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-308">click **All groups**.</span></span>

6.  <span data-ttu-id="4d3e5-309">**Hledání** pro skupinu vás zajímá a **klikněte na řádek** vybrat.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-309">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="4d3e5-310">Klikněte na tlačítko **licence** zobrazíte, které skupiny licencí aktuálně jeho přiřazení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-310">click **Licenses** to see which licenses the group currently has assigned.</span></span>

8.  <span data-ttu-id="4d3e5-311">klikněte **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-311">click the **Assign** button.</span></span>

9.  <span data-ttu-id="4d3e5-312">Vyberte **jeden nebo více produktů** ze seznamu dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-312">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="4d3e5-313">**Volitelné** klikněte na tlačítko **přiřazení možností** položky granularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-313">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="4d3e5-314">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-314">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="4d3e5-315">Klikněte **přiřadit** tlačítko přiřadit tyto licence do této skupiny.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-315">Click the **Assign** button to assign these licenses to this group.</span></span> <span data-ttu-id="4d3e5-316">To může trvat dlouhou dobu, v závislosti na velikost a složitost skupiny.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-316">This may take a long time, depending on the size and complexity of the group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="4d3e5-317">Uděláte to tak rychleji, vezměte v úvahu dočasně přiřazení licence uživatele přímo.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-317">To do this faster, consider temporarily assigning a license to the user directly.</span></span> <span data-ttu-id="4d3e5-318">[Přiřazení licence uživatele](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="4d3e5-318">[Assign a user a license](#problems-with-application-consent).</span></span>
   > 
   >

## <a name="problems-with-conditional-access-policies"></a><span data-ttu-id="4d3e5-319">Problémy se zásadami podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-319">Problems with conditional access policies</span></span>

### <a name="check-a-specific-conditional-access-policy"></a><span data-ttu-id="4d3e5-320">Zkontrolujte zásady podmíněného přístupu konkrétním</span><span class="sxs-lookup"><span data-stu-id="4d3e5-320">Check a specific conditional access policy</span></span>

<span data-ttu-id="4d3e5-321">Zkontrolujte nebo ověřit zásady jednoho podmíněného přístupu:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-321">To check or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="4d3e5-322">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-322">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-323">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-323">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-324">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-324">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-325">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-325">click **Enterprise applications** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-326">klikněte **podmíněného přístupu** navigační položka.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-326">click the **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="4d3e5-327">Klikněte na zásadu, která vás zajímá kontroly.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-327">click the policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="4d3e5-328">Zkontrolujte, že neexistují žádné konkrétní podmínky, přiřazení nebo jiná nastavení, které blokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-328">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

   >[!NOTE]
   ><span data-ttu-id="4d3e5-329">Chcete dočasně zakázat tuto zásadu a zajistit tak nemají vliv přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-329">You may wish to temporarily disable this policy to ensure it is not affecting sign ins.</span></span> <span data-ttu-id="4d3e5-330">Chcete-li to provést, nastavte **povolit zásady** přepnutím **ne** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-330">To do this, set the **Enable policy** toggle to **No** and click the **Save** button.</span></span>
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a><span data-ttu-id="4d3e5-331">Zkontrolujte zásady podmíněného přístupu pro konkrétní aplikace</span><span class="sxs-lookup"><span data-stu-id="4d3e5-331">Check a specific application’s conditional access policy</span></span>

<span data-ttu-id="4d3e5-332">Zkontrolujte nebo ověřit jednu aplikaci aktuálně nakonfigurované zásady podmíněného přístupu:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-332">To check or validate a single application’s currently configured conditional access policy:</span></span>

1.  <span data-ttu-id="4d3e5-333">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-333">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-334">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-334">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-335">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-335">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-336">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-336">click **Enterprise applications** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-337">Klikněte na tlačítko **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-337">click **All applications**.</span></span>

6.  <span data-ttu-id="4d3e5-338">Vyhledejte aplikace, které vás zajímají, nebo uživatel se pokouší přihlásit k aplikaci zobrazovaný název nebo id aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-338">Search for the application you are interested in, or the user is attempting to sign in to by application display name or application id.</span></span>

     >[!NOTE]
     ><span data-ttu-id="4d3e5-339">Pokud nevidíte aplikaci, kterou hledáte, klikněte **filtru** tlačítko a rozbalte oboru v seznamu **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-339">If you don’t see the application you are looking for, click the **Filter** button and expand the scope of the list to **All applications**.</span></span> <span data-ttu-id="4d3e5-340">Pokud chcete zobrazit více sloupců, klikněte **sloupce** tlačítko přidáte další podrobnosti pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-340">If you want to see more columns, click the **Columns** button to add additional details for your applications.</span></span>
     >
     >

7.  <span data-ttu-id="4d3e5-341">klikněte **podmíněného přístupu** navigační položka.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-341">click the **Conditional access** navigation item.</span></span>

8.  <span data-ttu-id="4d3e5-342">Klikněte na zásadu, která vás zajímá kontroly.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-342">click the policy you are interested in inspecting.</span></span>

9.  <span data-ttu-id="4d3e5-343">Zkontrolujte, že neexistují žádné konkrétní podmínky, přiřazení nebo jiná nastavení, které blokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-343">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

     >[!NOTE]
     ><span data-ttu-id="4d3e5-344">Chcete dočasně zakázat tuto zásadu a zajistit tak nemají vliv přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-344">You may wish to temporarily disable this policy to ensure it is not affecting sign ins.</span></span> <span data-ttu-id="4d3e5-345">Chcete-li to provést, nastavte **povolit zásady** přepnutím **ne** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-345">To do this, set the **Enable policy** toggle to **No** and click the **Save** button.</span></span>
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a><span data-ttu-id="4d3e5-346">Zakázat zásadu podmíněného přístupu konkrétním</span><span class="sxs-lookup"><span data-stu-id="4d3e5-346">Disable a specific conditional access policy</span></span>

<span data-ttu-id="4d3e5-347">Zkontrolujte nebo ověřit zásady jednoho podmíněného přístupu:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-347">To check or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="4d3e5-348">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="4d3e5-348">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4d3e5-349">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-349">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4d3e5-350">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-350">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4d3e5-351">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-351">click **Enterprise applications** in the navigation menu.</span></span>

5.  <span data-ttu-id="4d3e5-352">klikněte **podmíněného přístupu** navigační položka.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-352">click the **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="4d3e5-353">Klikněte na zásadu, která vás zajímá kontroly.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-353">click the policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="4d3e5-354">Zakažte zásadu nastavením **povolit zásady** přepnutím **ne** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-354">Disable the policy by setting the **Enable policy** toggle to **No** and click the **Save** button.</span></span>

## <a name="problems-with-application-consent"></a><span data-ttu-id="4d3e5-355">Problémy s aplikací souhlasu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-355">Problems with application consent</span></span>

<span data-ttu-id="4d3e5-356">Přístup k aplikaci můžete blokovat, protože nedošlo k operaci souhlasu příslušná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-356">Application access can be blocked because the proper permissions consent operation has not occurred.</span></span> <span data-ttu-id="4d3e5-357">Tady jsou některé způsoby řešení problémů a vyřešit problémy s aplikací souhlasu:</span><span class="sxs-lookup"><span data-stu-id="4d3e5-357">Below are some ways you can troubleshoot and solve application consent issues:</span></span>

-   [<span data-ttu-id="4d3e5-358">Provedení operace individuální souhlasu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-358">Perform a user-level consent operation</span></span>](#perform-a-user-level-consent-operation)

-   [<span data-ttu-id="4d3e5-359">Provedení operace správce úroveň souhlasu pro žádnou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-359">Perform administrator-level consent operation for any application</span></span>](#perform-administrator-level-consent-operation-for-any-application)

-   [<span data-ttu-id="4d3e5-360">Provést na úrovni správce souhlasu pro jednoho klienta aplikace</span><span class="sxs-lookup"><span data-stu-id="4d3e5-360">Perform administrator-level consent for a single-tenant application</span></span>](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [<span data-ttu-id="4d3e5-361">Provést na úrovni správce souhlasu pro víceklientské aplikace</span><span class="sxs-lookup"><span data-stu-id="4d3e5-361">Perform administrator-level consent for a multi-tenant application</span></span>](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a><span data-ttu-id="4d3e5-362">Provedení operace individuální souhlasu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-362">Perform a user-level consent operation</span></span>

-   <span data-ttu-id="4d3e5-363">Pro každou Open ID Connect-povolit aplikaci, která vyžaduje oprávnění přejdete na přihlašovací obrazovka, aplikace provádí úrovni souhlasu uživatele k aplikaci pro přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-363">For any Open ID Connect-enabled application that requests permissions, navigating to the application’s sign in screen performs a user level consent to the application for the signed-in user.</span></span>

-   <span data-ttu-id="4d3e5-364">Pokud chcete provést programově, přečtěte si téma [vyžaduje souhlas jednotlivé uživatele](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span><span class="sxs-lookup"><span data-stu-id="4d3e5-364">If you wish to do this programmatically, see [Requesting individual user consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span></span>

### <a name="perform-administrator-level-consent-operation-for-any-application"></a><span data-ttu-id="4d3e5-365">Provedení operace správce úroveň souhlasu pro žádnou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-365">Perform administrator-level consent operation for any application</span></span>

-   <span data-ttu-id="4d3e5-366">Pro **jenom aplikace vyvinuté pomocí aplikačního modelu V1**, můžete vynutit této úrovně souhlasu správce proběhnout přidáním "**? řádku = správce\_souhlas**" na konec přihlašovací adresa URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-366">For **only applications developed using the V1 application model**, you can force this administrator level consent to occur by adding “**?prompt=admin\_consent**” to the end of an application’s sign in URL.</span></span>

-   <span data-ttu-id="4d3e5-367">Pro **všechny aplikace vyvinuté pomocí aplikačního modelu V2**, můžete vynutit svůj souhlas úrovni správce proběhnout podle pokynů v části **oprávnění požádat správce directory** části [pomocí koncový bod admin souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="4d3e5-367">For **any application developed using the V2 application model**, you can enforce this administrator-level consent to occur by following the instructions under the **Request the permissions from a directory admin** section of [Using the admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a><span data-ttu-id="4d3e5-368">Provést na úrovni správce souhlasu pro jednoho klienta aplikace</span><span class="sxs-lookup"><span data-stu-id="4d3e5-368">Perform administrator-level consent for a single-tenant application</span></span>

-   <span data-ttu-id="4d3e5-369">Pro **jednoho klienta aplikace** , žádostí o oprávnění (jako jsou ty, které jste vývoji nebo vlastní ve vaší organizaci), můžete provést **správu úroveň souhlasu** operace jménem všichni uživatelé přihlášení jako globální správce a kliknutím na **udělit oprávnění** tlačítka v horní části **registru aplikace -&gt; všechny aplikace -&gt; vyberte aplikaci -&gt; požadovaných oprávnění** okno.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-369">For **single-tenant applications** that request permissions (like those you are developing or own in your organization), you can perform an **administrative-level consent** operation on behalf of all users by signing in as a Global Administrator and clicking on the **Grant permissions** button at the top of the **Application Registry -&gt; All Applications -&gt; Select an App -&gt; Required Permissions** blade.</span></span>

-   <span data-ttu-id="4d3e5-370">Pro **všechny aplikace vyvinuté pomocí aplikačního modelu V1 nebo V2**, můžete vynutit svůj souhlas úrovni správce proběhnout podle pokynů v části **oprávnění požádat správce directory** části [pomocí koncový bod admin souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="4d3e5-370">For **any application developed using the V1 or V2 application model**, you can enforce this administrator-level consent to occur by following the instructions under the **Request the permissions from a directory admin** section of [Using the admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a><span data-ttu-id="4d3e5-371">Provést na úrovni správce souhlasu pro víceklientské aplikace</span><span class="sxs-lookup"><span data-stu-id="4d3e5-371">Perform administrator-level consent for a multi-tenant application</span></span>

-   <span data-ttu-id="4d3e5-372">Pro **víceklientským aplikacím** této žádosti o oprávnění (jako jsou aplikace třetích stran nebo sama vyvinula společnost Microsoft,), můžete provést **správu úroveň souhlasu** operaci.</span><span class="sxs-lookup"><span data-stu-id="4d3e5-372">For **multi-tenant applications** that request permissions (like an application a third party, or Microsoft, develops), you can perform an **administrative-level consent** operation.</span></span> <span data-ttu-id="4d3e5-373">Přihlaste se jako globální správce a kliknutím na **udělit oprávnění** tlačítko pod **podnikové aplikace –&gt; všechny aplikace -&gt; vyberte aplikaci -&gt; oprávnění** okno (k dispozici brzy).</span><span class="sxs-lookup"><span data-stu-id="4d3e5-373">Sign in as a Global Administrator and clicking on the **Grant permissions** button under the **Enterprise Applications -&gt; All Applications -&gt; Select an App -&gt; Permissions** blade (available soon).</span></span>

-   <span data-ttu-id="4d3e5-374">Může také vynucovat svůj souhlas úrovni správce proběhnout podle pokynů v části **oprávnění požádat správce directory** části [pomocí koncový bod admin souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="4d3e5-374">You can also enforce this administrator-level consent to occur by following the instructions under the **Request the permissions from a directory admin** section of [Using the admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d3e5-375">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d3e5-375">Next steps</span></span>
[<span data-ttu-id="4d3e5-376">Pomocí koncový bod admin souhlasu</span><span class="sxs-lookup"><span data-stu-id="4d3e5-376">Using the admin consent endpoint</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

