---
title: "Potíže při přihlašování do aplikace bez Galerie konfigurován pro federované jednotné přihlašování | Microsoft Docs"
description: "Pokyny pro konkrétní problémy, kterým může být vystaven při přihlášení do aplikace nakonfigurovaná pro na základě SAML federované jednotné přihlašování s Azure AD"
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
ms.openlocfilehash: 3afc7bca878caef424d3fa3c64aa17df0fda7de5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-a-non-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="e7bbe-103">Potíže při přihlašování do aplikace bez Galerie nakonfigurovaný pro federované jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7bbe-103">Problems signing in to a non-gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="e7bbe-104">Chcete-li vyřešit problém, musíte ověřit konfiguraci aplikace ve službě Azure AD následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e7bbe-104">To troubleshoot your problem, you need to verify the application configuration in Azure AD as follow:</span></span>

-   <span data-ttu-id="e7bbe-105">Jste provedli všechny kroky konfigurace pro aplikaci Galerie Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-105">You have followed all the configuration steps for the Azure AD gallery application.</span></span>

-   <span data-ttu-id="e7bbe-106">Identifikátor a adresa URL odpovědi, které jsou nakonfigurované v AAD shodovat se očekávaných hodnot v aplikaci</span><span class="sxs-lookup"><span data-stu-id="e7bbe-106">The Identifier and Reply URL configured in AAD match they expected values in the application</span></span>

-   <span data-ttu-id="e7bbe-107">Přiřadili jste uživatele k aplikaci</span><span class="sxs-lookup"><span data-stu-id="e7bbe-107">You have assigned users to the application</span></span>

## <a name="application-not-found-in-directory"></a><span data-ttu-id="e7bbe-108">Aplikace nebyla nalezena v adresáři</span><span class="sxs-lookup"><span data-stu-id="e7bbe-108">Application not found in directory</span></span>

<span data-ttu-id="e7bbe-109">*Chyba AADSTS70001: Aplikace s identifikátorem 'https://contoso.com' nebyl nalezen v adresáři*.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-109">*Error AADSTS70001: Application with Identifier ‘https://contoso.com’ was not found in the directory*.</span></span>

<span data-ttu-id="e7bbe-110">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-110">**Possible cause**</span></span>

<span data-ttu-id="e7bbe-111">Vystavitel, který odešle atribut z aplikace do služby Azure AD v žádosti SAML neodpovídá identifikátoru hodnotu nakonfigurovanou v aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-111">The Issuer attribute sends from the application to Azure AD in the SAML request doesn’t match the Identifier value configured in the application Azure AD.</span></span>

<span data-ttu-id="e7bbe-112">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-112">**Resolution**</span></span>

<span data-ttu-id="e7bbe-113">Ujistěte se, že vystavitel atribut v požadavku SAML ho je odpovídající identifikátor hodnotu nakonfigurovanou v Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e7bbe-113">Ensure that the Issuer attribute in the SAML request it’s matching the Identifier value configured in Azure AD:</span></span>

1.  <span data-ttu-id="e7bbe-114">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-114">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="e7bbe-115">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-115">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e7bbe-116">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-116">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e7bbe-117">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-117">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e7bbe-118">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-118">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="e7bbe-119">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-119">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="e7bbe-120">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-120">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="e7bbe-121">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-121">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="e7bbe-122"><span id="_Hlk477190042" class="anchor"></span>Přejděte na **domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-122"><span id="_Hlk477190042" class="anchor"></span>Go to **Domain and URLs** section.</span></span> <span data-ttu-id="e7bbe-123">Ověřte, zda je hodnota v textovém poli identifikátor odpovídající hodnota pro identifikátor hodnota zobrazená v chybě.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-123">Verify that the value in the Identifier textbox is matching the value for the identifier value displayed in the error.</span></span>

<span data-ttu-id="e7bbe-124">Jakmile jste aktualizovali hodnota identifikátoru ve službě Azure AD a odešle hodnotu ho je odpovídající aplikace v žádosti SAML, byste měli moct přihlásit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-124">After you have updated the Identifier value in Azure AD and it’s matching the value sends by the application in the SAML request, you should be able to sign in to the application.</span></span>

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a><span data-ttu-id="e7bbe-125">Adresa pro odeslání odpovědi neodpovídá adresy odpovědět, nakonfigurované pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-125">The reply address does not match the reply addresses configured for the application.</span></span> 

<span data-ttu-id="e7bbe-126">*Chyba AADSTS50011: Adresa odpovědi 'https://contoso.com' neodpovídá adresy odpovědět, nakonfigurované pro aplikaci*</span><span class="sxs-lookup"><span data-stu-id="e7bbe-126">*Error AADSTS50011: The reply address ‘https://contoso.com’ does not match the reply addresses configured for the application*</span></span> 

<span data-ttu-id="e7bbe-127">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-127">**Possible cause**</span></span> 

<span data-ttu-id="e7bbe-128">Hodnota AssertionConsumerServiceURL v žádosti SAML neodpovídá hodnotu adresa URL odpovědi nebo vzor nakonfigurovat ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-128">The AssertionConsumerServiceURL value in the SAML request doesn't match the Reply URL value or pattern configured in Azure AD.</span></span> <span data-ttu-id="e7bbe-129">Hodnota AssertionConsumerServiceURL v žádosti SAML je adresa URL se zobrazí v chybě.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-129">The AssertionConsumerServiceURL value in the SAML request is the URL you see in the error.</span></span> 

<span data-ttu-id="e7bbe-130">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-130">**Resolution**</span></span> 

<span data-ttu-id="e7bbe-131">Ujistěte se, že hodnota AssertionConsumerServiceURL v žádosti SAML ho je adresa URL odpovědi odpovídající hodnotu nakonfigurovanou v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-131">Ensure that the AssertionConsumerServiceURL value in the SAML request it's matching the Reply URL value configured in Azure AD.</span></span> 
 
1.  <span data-ttu-id="e7bbe-132">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-132">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span> 

2.  <span data-ttu-id="e7bbe-133">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-133">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span> 

3.  <span data-ttu-id="e7bbe-134">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-134">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span> 

4.  <span data-ttu-id="e7bbe-135">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-135">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span> 

5.  <span data-ttu-id="e7bbe-136">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-136">click **All Applications** to view a list of all your applications.</span></span> 

  * <span data-ttu-id="e7bbe-137">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-137">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and       set the **Show** option to **All Applications.**</span></span>
  
6.  <span data-ttu-id="e7bbe-138">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7bbe-138">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="e7bbe-139">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-139">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="e7bbe-140">Přejděte na **domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-140">Go to **Domain and URLs** section.</span></span> <span data-ttu-id="e7bbe-141">Ověřte nebo aktualizujte hodnotu v textovém poli Adresa URL odpovědi odpovídat hodnotě AssertionConsumerServiceURL v žádosti SAML.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-141">Verify or update the value in the Reply URL textbox to match the AssertionConsumerServiceURL value in the SAML request.</span></span>

  * <span data-ttu-id="e7bbe-142">Pokud nevidíte textového pole Adresa URL odpovědi, vyberte **zobrazit upřesňující nastavení adresy URL** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-142">If you don't see the Reply URL textbox, select the **Show advanced URL settings** checkbox.</span></span> 

<span data-ttu-id="e7bbe-143">Poté, co jste aktualizovali hodnota adresa URL odpovědi ve službě Azure AD a odešle hodnotu ho je odpovídající aplikace v žádosti SAML, byste měli moct přihlásit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-143">After you have updated the Reply URL value in Azure AD and it’s matching the value sends by the application in the SAML request, you should be able to sign in to the application.</span></span>

## <a name="user-not-assigned-a-role"></a><span data-ttu-id="e7bbe-144">Není přiřazen k roli uživatele</span><span class="sxs-lookup"><span data-stu-id="e7bbe-144">User not assigned a role</span></span>

<span data-ttu-id="e7bbe-145">*Chyba AADSTS50105: Přihlášeného uživatele 'brian@contoso.com' není přiřazen do rolí pro aplikace*</span><span class="sxs-lookup"><span data-stu-id="e7bbe-145">*Error AADSTS50105: The signed in user 'brian@contoso.com' is not assigned to a role for the application*</span></span>

<span data-ttu-id="e7bbe-146">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-146">**Possible cause**</span></span>

<span data-ttu-id="e7bbe-147">Uživateli nebyl udělen přístup k aplikaci ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-147">The user has not been granted access to the application in Azure AD.</span></span>

<span data-ttu-id="e7bbe-148">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-148">**Resolution**</span></span>

<span data-ttu-id="e7bbe-149">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="e7bbe-149">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="e7bbe-150">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-150">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="e7bbe-151">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-151">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e7bbe-152">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-152">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e7bbe-153">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-153">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e7bbe-154">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-154">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="e7bbe-155">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-155">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="e7bbe-156">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-156">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="e7bbe-157">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-157">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="e7bbe-158">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-158">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="e7bbe-159">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-159">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="e7bbe-160">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-160">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="e7bbe-161">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-161">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="e7bbe-162">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-162">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="e7bbe-163">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-163">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="e7bbe-164">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-164">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="e7bbe-165">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-165">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="e7bbe-166">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-166">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="e7bbe-167">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace pomocí metody popsané v části popis řešení.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-167">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="not-a-valid-saml-request"></a><span data-ttu-id="e7bbe-168">Není platný SAML požadavku na</span><span class="sxs-lookup"><span data-stu-id="e7bbe-168">Not a valid SAML Request</span></span>

<span data-ttu-id="e7bbe-169">*Chyba AADSTS75005: Žádosti není platný zprávu protokolu typu Saml2.*</span><span class="sxs-lookup"><span data-stu-id="e7bbe-169">*Error AADSTS75005: The request is not a valid Saml2 protocol message.*</span></span>

<span data-ttu-id="e7bbe-170">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-170">**Possible cause**</span></span>

<span data-ttu-id="e7bbe-171">Azure AD nepodporuje SAML požadavků odeslaných aplikací pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-171">Azure AD doesn’t support the SAML Request sent by the application for Single Sign-on.</span></span> <span data-ttu-id="e7bbe-172">Jsou některé běžné problémy:</span><span class="sxs-lookup"><span data-stu-id="e7bbe-172">Some common issues are:</span></span>

-   <span data-ttu-id="e7bbe-173">Chybí povinná pole v požadavku SAML</span><span class="sxs-lookup"><span data-stu-id="e7bbe-173">Missing required fields in the SAML request</span></span>

-   <span data-ttu-id="e7bbe-174">Metoda požadavku kódovaný SAML</span><span class="sxs-lookup"><span data-stu-id="e7bbe-174">SAML request encoded method</span></span>

<span data-ttu-id="e7bbe-175">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-175">**Resolution**</span></span>

1.  <span data-ttu-id="e7bbe-176">Zaznamenejte žádost SAML.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-176">Capture SAML request.</span></span> <span data-ttu-id="e7bbe-177">postupujte podle kurzu na [ladění na základě SAML jednotného přihlašování k aplikacím v Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) se dozvíte, jak chcete zaznamenat žádost SAML.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-177">follow the tutorial on [how to debug SAML-based single sign-on to applications in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) to learn how to capture the SAML request.</span></span>

2.  <span data-ttu-id="e7bbe-178">Obraťte se na dodavatele aplikace a sdílené složky:</span><span class="sxs-lookup"><span data-stu-id="e7bbe-178">Contact the application vendor and share:</span></span>

    -   <span data-ttu-id="e7bbe-179">Žádost SAML</span><span class="sxs-lookup"><span data-stu-id="e7bbe-179">SAML request</span></span>

    -   [<span data-ttu-id="e7bbe-180">Protokol požadavky pro Azure AD jedním přihlašování SAML</span><span class="sxs-lookup"><span data-stu-id="e7bbe-180">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

<span data-ttu-id="e7bbe-181">Se musí ověřit podporují implementace Azure AD SAML pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-181">They should validate they support the Azure AD SAML implementation for Single Sign-on.</span></span>

## <a name="no-resource-in-requiredresourceaccess-list"></a><span data-ttu-id="e7bbe-182">Žádný prostředek v seznamu requiredResourceAccess</span><span class="sxs-lookup"><span data-stu-id="e7bbe-182">No resource in requiredResourceAccess list</span></span>

<span data-ttu-id="e7bbe-183">*Chyba AADSTS65005: Klientská aplikace vyžaduje přístup k prostředku, 00000002-0000-0000-c000-000000000000'. Tento požadavek se nezdařil, protože klient nebyl zadán tento prostředek ve svém seznamu requiredResourceAccess*.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-183">*Error AADSTS65005: The client application has requested access to resource '00000002-0000-0000-c000-000000000000'. This request has failed because the client has not specified this resource in its requiredResourceAccess list*.</span></span>

<span data-ttu-id="e7bbe-184">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-184">**Possible cause**</span></span>

<span data-ttu-id="e7bbe-185">Objekt aplikace je poškozen.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-185">The application object is corrupted.</span></span>

<span data-ttu-id="e7bbe-186">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-186">**Resolution**</span></span>

<span data-ttu-id="e7bbe-187">Chcete-li problém vyřešit, odeberte z adresáře aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-187">To solve the problem, remove the application from the directory.</span></span> <span data-ttu-id="e7bbe-188">Potom přidat a změnit konfiguraci aplikace, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="e7bbe-188">Then, add and reconfigure the application, follow the steps below:</span></span>

1.  <span data-ttu-id="e7bbe-189">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-189">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="e7bbe-190">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-190">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e7bbe-191">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-191">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e7bbe-192">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-192">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e7bbe-193">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-193">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="e7bbe-194">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-194">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="e7bbe-195">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-195">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="e7bbe-196">Klikněte na tlačítko **odstranit** v levé horní aplikace **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-196">Click **Delete** at the top-left of the application **Overview** blade.</span></span>

8.  <span data-ttu-id="e7bbe-197">Aktualizujte Azure AD a přidat aplikaci z Galerie Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-197">Refresh Azure AD and Add the application from the Azure AD gallery.</span></span> <span data-ttu-id="e7bbe-198">Potom nakonfigurujte aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-198">Then, Configure the application again.</span></span>

<span data-ttu-id="e7bbe-199">Po překonfigurování aplikaci, byste měli být moct přihlásit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-199">After reconfiguring the application, you should be able to sign in to the application.</span></span>

## <a name="certificate-or-key-not-configured"></a><span data-ttu-id="e7bbe-200">Certifikát nebo klíče není nakonfigurováno</span><span class="sxs-lookup"><span data-stu-id="e7bbe-200">Certificate or key not configured</span></span>

<span data-ttu-id="e7bbe-201">Chyba AADSTS50003: Žádné podpisový klíč nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-201">Error AADSTS50003: No signing key configured.</span></span>

<span data-ttu-id="e7bbe-202">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-202">**Possible cause**</span></span>

<span data-ttu-id="e7bbe-203">Objekt aplikace je poškozený a Azure AD nerozpoznal certifikát nakonfigurovaný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-203">The application object is corrupted and Azure AD doesn’t recognize the certificate configured for the application.</span></span>

<span data-ttu-id="e7bbe-204">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-204">**Resolution**</span></span>

<span data-ttu-id="e7bbe-205">Pokud chcete odstranit a vytvořit nový certifikát, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="e7bbe-205">To delete and create a new certificate, follow the steps below:</span></span>

1.  <span data-ttu-id="e7bbe-206">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-206">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="e7bbe-207">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-207">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e7bbe-208">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-208">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e7bbe-209">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-209">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e7bbe-210">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-210">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="e7bbe-211">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-211">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="e7bbe-212">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-212">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="e7bbe-213">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-213">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="e7bbe-214">Klikněte na tlačítko **vytvořit nový certifikát** pod **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-214">click **Create new certificate** under the **SAML signing Certificate** section.</span></span>

9.  <span data-ttu-id="e7bbe-215">Vyberte datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-215">Select Expiration date.</span></span> <span data-ttu-id="e7bbe-216">Potom klikněte na **uložit.**</span><span class="sxs-lookup"><span data-stu-id="e7bbe-216">Then, click **Save.**</span></span>

10. <span data-ttu-id="e7bbe-217">Zkontrolujte **aktivujte nový certifikát** přepsat aktivní certifikát.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-217">Check **Make new certificate active** to override the active certificate.</span></span> <span data-ttu-id="e7bbe-218">Potom klikněte na **Uložit** v horní části okna a přijměte aktivovat certifikát výměny.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-218">Then, click **Save** at the top of the blade and accept to activate the rollover certificate.</span></span>

11. <span data-ttu-id="e7bbe-219">V části **SAML podpisový certifikát** klikněte na tlačítko **odebrat** odebrat **nepoužitý** certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-219">Under the **SAML Signing Certificate** section, click **remove** to remove the **Unused** certificate.</span></span>

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a><span data-ttu-id="e7bbe-220">Problém při přizpůsobení SAML deklarace identity odeslat do aplikace</span><span class="sxs-lookup"><span data-stu-id="e7bbe-220">Problem when customizing the SAML claims sent to an application</span></span>

<span data-ttu-id="e7bbe-221">Naučte se přizpůsobovat deklarací identity atributu SAML odeslaných do vaší aplikace, najdete v tématu [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e7bbe-221">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7bbe-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7bbe-222">Next steps</span></span>
[<span data-ttu-id="e7bbe-223">Protokol požadavky pro Azure AD jedním přihlašování SAML</span><span class="sxs-lookup"><span data-stu-id="e7bbe-223">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)