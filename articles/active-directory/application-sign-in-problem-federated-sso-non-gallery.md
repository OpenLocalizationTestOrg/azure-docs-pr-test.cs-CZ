---
title: "aaaProblems přihlášení aplikace bez Galerie tooa konfigurovaná pro federované jednotné přihlašování | Microsoft Docs"
description: "Pokyny pro hello konkrétních problémů, kterým může být vystaven při přihlášení tooan aplikace konfigurovaná pro na základě SAML federované jednotné přihlašování s Azure AD"
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
ms.openlocfilehash: 1243456695c097f404a66fc89893efa2afdaaf22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooa-non-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="631e8-103">Potíže s přihlášením aplikace bez Galerie tooa konfigurovaná pro federované jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="631e8-103">Problems signing in tooa non-gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="631e8-104">tootroubleshoot problém, je třeba konfigurace aplikace hello tooverify ve službě Azure AD jako postupujte podle kroků:</span><span class="sxs-lookup"><span data-stu-id="631e8-104">tootroubleshoot your problem, you need tooverify hello application configuration in Azure AD as follow:</span></span>

-   <span data-ttu-id="631e8-105">Jste provedli všechny kroky konfigurace hello pro hello galerii aplikací Azure AD.</span><span class="sxs-lookup"><span data-stu-id="631e8-105">You have followed all hello configuration steps for hello Azure AD gallery application.</span></span>

-   <span data-ttu-id="631e8-106">Hello identifikátor a adresa URL odpovědi, které jsou nakonfigurované v AAD shodovat se očekávaných hodnot v aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="631e8-106">hello Identifier and Reply URL configured in AAD match they expected values in hello application</span></span>

-   <span data-ttu-id="631e8-107">Přiřadili jste uživatelé toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="631e8-107">You have assigned users toohello application</span></span>

## <a name="application-not-found-in-directory"></a><span data-ttu-id="631e8-108">Aplikace nebyla nalezena v adresáři</span><span class="sxs-lookup"><span data-stu-id="631e8-108">Application not found in directory</span></span>

<span data-ttu-id="631e8-109">*Chyba AADSTS70001: Aplikace s identifikátorem 'https://contoso.com' nebyl nalezen v adresáři hello*.</span><span class="sxs-lookup"><span data-stu-id="631e8-109">*Error AADSTS70001: Application with Identifier ‘https://contoso.com’ was not found in hello directory*.</span></span>

<span data-ttu-id="631e8-110">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="631e8-110">**Possible cause**</span></span>

<span data-ttu-id="631e8-111">atribut vystavitele Hello odešle z tooAzure hello aplikací že AD v hello SAML požadavku se neshoduje se hello identifikátor hodnotu nakonfigurovanou v hello aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="631e8-111">hello Issuer attribute sends from hello application tooAzure AD in hello SAML request doesn’t match hello Identifier value configured in hello application Azure AD.</span></span>

<span data-ttu-id="631e8-112">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="631e8-112">**Resolution**</span></span>

<span data-ttu-id="631e8-113">Ujistěte se, tento atribut hello vystavitele v hello SAML požadavku, že je jeho odpovídající hello identifikátor hodnotu nakonfigurovanou v Azure AD:</span><span class="sxs-lookup"><span data-stu-id="631e8-113">Ensure that hello Issuer attribute in hello SAML request it’s matching hello Identifier value configured in Azure AD:</span></span>

1.  <span data-ttu-id="631e8-114">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="631e8-114">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="631e8-115">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-115">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="631e8-116">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="631e8-116">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="631e8-117">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="631e8-117">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="631e8-118">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="631e8-118">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="631e8-119">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="631e8-119">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="631e8-120">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="631e8-120">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="631e8-121">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="631e8-121">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="631e8-122"><span id="_Hlk477190042" class="anchor"></span>Přejděte příliš**domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="631e8-122"><span id="_Hlk477190042" class="anchor"></span>Go too**Domain and URLs** section.</span></span> <span data-ttu-id="631e8-123">Ověřte, zda text hello hodnota v hello identifikátor textové pole je odpovídající hodnota hello hodnota identifikátoru hello zobrazí chyba hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-123">Verify that hello value in hello Identifier textbox is matching hello value for hello identifier value displayed in hello error.</span></span>

<span data-ttu-id="631e8-124">Poté, co jste aktualizovali hodnota identifikátoru hello ve službě Azure AD a jeho odpovídající hello hodnotu odešle hello aplikací v žádosti SAML hello, musí být schopný toosign v toohello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="631e8-124">After you have updated hello Identifier value in Azure AD and it’s matching hello value sends by hello application in hello SAML request, you should be able toosign in toohello application.</span></span>

## <a name="hello-reply-address-does-not-match-hello-reply-addresses-configured-for-hello-application"></a><span data-ttu-id="631e8-125">Adresa pro odpověď Hello neodpovídá hello odpovědi adresy nakonfigurované pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-125">hello reply address does not match hello reply addresses configured for hello application.</span></span> 

<span data-ttu-id="631e8-126">*Chyba AADSTS50011: adresa odpovědi hello 'https://contoso.com' neodpovídá hello odpovědi adresy nakonfigurované pro aplikace hello*</span><span class="sxs-lookup"><span data-stu-id="631e8-126">*Error AADSTS50011: hello reply address ‘https://contoso.com’ does not match hello reply addresses configured for hello application*</span></span> 

<span data-ttu-id="631e8-127">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="631e8-127">**Possible cause**</span></span> 

<span data-ttu-id="631e8-128">Hello žádost SAML hello hodnotu AssertionConsumerServiceURL neodpovídá hello adresa URL odpovědi hodnotou nebo vzorem, které jsou nakonfigurované ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="631e8-128">hello AssertionConsumerServiceURL value in hello SAML request doesn't match hello Reply URL value or pattern configured in Azure AD.</span></span> <span data-ttu-id="631e8-129">Hello AssertionConsumerServiceURL hodnota v žádosti SAML hello je hello URL se zobrazí chyba hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-129">hello AssertionConsumerServiceURL value in hello SAML request is hello URL you see in hello error.</span></span> 

<span data-ttu-id="631e8-130">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="631e8-130">**Resolution**</span></span> 

<span data-ttu-id="631e8-131">Zkontrolujte hodnotu AssertionConsumerServiceURL hello do žádost SAML hello jeho odpovídající hello adresa URL odpovědi hodnota nakonfigurovat ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="631e8-131">Ensure that hello AssertionConsumerServiceURL value in hello SAML request it's matching hello Reply URL value configured in Azure AD.</span></span> 
 
1.  <span data-ttu-id="631e8-132">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="631e8-132">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span> 

2.  <span data-ttu-id="631e8-133">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-133">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span> 

3.  <span data-ttu-id="631e8-134">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="631e8-134">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span> 

4.  <span data-ttu-id="631e8-135">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="631e8-135">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span> 

5.  <span data-ttu-id="631e8-136">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="631e8-136">click **All Applications** tooview a list of all your applications.</span></span> 

  * <span data-ttu-id="631e8-137">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="631e8-137">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and       set hello **Show** option too**All Applications.**</span></span>
  
6.  <span data-ttu-id="631e8-138">Vyberte hello aplikaci tooconfigure jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="631e8-138">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="631e8-139">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="631e8-139">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="631e8-140">Přejděte příliš**domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="631e8-140">Go too**Domain and URLs** section.</span></span> <span data-ttu-id="631e8-141">Ověřte nebo aktualizujte hodnotu hello v hello adresa URL odpovědi textbox toomatch hello žádost SAML hello AssertionConsumerServiceURL hodnotu.</span><span class="sxs-lookup"><span data-stu-id="631e8-141">Verify or update hello value in hello Reply URL textbox toomatch hello AssertionConsumerServiceURL value in hello SAML request.</span></span>

  * <span data-ttu-id="631e8-142">Pokud nevidíte textbox hello adresa URL odpovědi, vyberte hello **zobrazit upřesňující nastavení adresy URL** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="631e8-142">If you don't see hello Reply URL textbox, select hello **Show advanced URL settings** checkbox.</span></span> 

<span data-ttu-id="631e8-143">Poté, co byly aktualizovány hello adresa URL odpovědi hodnotu ve službě Azure AD a má odpovídající hodnotu hello odešle hello aplikací v hello žádost SAML, byste měli mít toosign v toohello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="631e8-143">After you have updated hello Reply URL value in Azure AD and it’s matching hello value sends by hello application in hello SAML request, you should be able toosign in toohello application.</span></span>

## <a name="user-not-assigned-a-role"></a><span data-ttu-id="631e8-144">Není přiřazen k roli uživatele</span><span class="sxs-lookup"><span data-stu-id="631e8-144">User not assigned a role</span></span>

<span data-ttu-id="631e8-145">*Chyba AADSTS50105: hello přihlášení uživatele sebrian@contoso.com' není přiřazena role tooa aplikace hello*</span><span class="sxs-lookup"><span data-stu-id="631e8-145">*Error AADSTS50105: hello signed in user 'brian@contoso.com' is not assigned tooa role for hello application*</span></span>

<span data-ttu-id="631e8-146">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="631e8-146">**Possible cause**</span></span>

<span data-ttu-id="631e8-147">Hello uživateli nebyl udělen přístup toohello aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="631e8-147">hello user has not been granted access toohello application in Azure AD.</span></span>

<span data-ttu-id="631e8-148">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="631e8-148">**Resolution**</span></span>

<span data-ttu-id="631e8-149">tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="631e8-149">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="631e8-150">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="631e8-150">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="631e8-151">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-151">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="631e8-152">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="631e8-152">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="631e8-153">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="631e8-153">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="631e8-154">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="631e8-154">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="631e8-155">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="631e8-155">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="631e8-156">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-156">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="631e8-157">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="631e8-157">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="631e8-158">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="631e8-158">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="631e8-159">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="631e8-159">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="631e8-160">Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="631e8-160">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="631e8-161">Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="631e8-161">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="631e8-162">Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="631e8-162">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="631e8-163">**Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="631e8-163">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="631e8-164">Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="631e8-164">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="631e8-165">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="631e8-165">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="631e8-166">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="631e8-166">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="631e8-167">Po krátké době být hello uživatelů, které jste vybrali možnost toolaunch hello tyto aplikace pomocí metody popsané v části popis řešení hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-167">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="not-a-valid-saml-request"></a><span data-ttu-id="631e8-168">Není platný SAML požadavku na</span><span class="sxs-lookup"><span data-stu-id="631e8-168">Not a valid SAML Request</span></span>

<span data-ttu-id="631e8-169">*Chyba AADSTS75005: hello požadavku není platný zprávu protokolu typu Saml2.*</span><span class="sxs-lookup"><span data-stu-id="631e8-169">*Error AADSTS75005: hello request is not a valid Saml2 protocol message.*</span></span>

<span data-ttu-id="631e8-170">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="631e8-170">**Possible cause**</span></span>

<span data-ttu-id="631e8-171">Azure AD nepodporuje hello SAML požadavku odeslaná hello aplikací pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="631e8-171">Azure AD doesn’t support hello SAML Request sent by hello application for Single Sign-on.</span></span> <span data-ttu-id="631e8-172">Jsou některé běžné problémy:</span><span class="sxs-lookup"><span data-stu-id="631e8-172">Some common issues are:</span></span>

-   <span data-ttu-id="631e8-173">Chybí povinná pole v požadavku SAML hello</span><span class="sxs-lookup"><span data-stu-id="631e8-173">Missing required fields in hello SAML request</span></span>

-   <span data-ttu-id="631e8-174">Metoda požadavku kódovaný SAML</span><span class="sxs-lookup"><span data-stu-id="631e8-174">SAML request encoded method</span></span>

<span data-ttu-id="631e8-175">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="631e8-175">**Resolution**</span></span>

1.  <span data-ttu-id="631e8-176">Zaznamenejte žádost SAML.</span><span class="sxs-lookup"><span data-stu-id="631e8-176">Capture SAML request.</span></span> <span data-ttu-id="631e8-177">postupujte podle kurzu hello na [jak toodebug na základě SAML jeden přihlašování tooapplications ve službě Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn, jak požádat o toocapture hello SAML.</span><span class="sxs-lookup"><span data-stu-id="631e8-177">follow hello tutorial on [how toodebug SAML-based single sign-on tooapplications in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn how toocapture hello SAML request.</span></span>

2.  <span data-ttu-id="631e8-178">Obraťte se na dodavatele aplikace hello a sdílené složky:</span><span class="sxs-lookup"><span data-stu-id="631e8-178">Contact hello application vendor and share:</span></span>

    -   <span data-ttu-id="631e8-179">Žádost SAML</span><span class="sxs-lookup"><span data-stu-id="631e8-179">SAML request</span></span>

    -   [<span data-ttu-id="631e8-180">Protokol požadavky pro Azure AD jedním přihlašování SAML</span><span class="sxs-lookup"><span data-stu-id="631e8-180">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

<span data-ttu-id="631e8-181">Se musí ověřit podporují implementace hello Azure AD SAML pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="631e8-181">They should validate they support hello Azure AD SAML implementation for Single Sign-on.</span></span>

## <a name="no-resource-in-requiredresourceaccess-list"></a><span data-ttu-id="631e8-182">Žádný prostředek v seznamu requiredResourceAccess</span><span class="sxs-lookup"><span data-stu-id="631e8-182">No resource in requiredResourceAccess list</span></span>

<span data-ttu-id="631e8-183">*Chyba AADSTS65005: hello klientská aplikace vyžaduje přístup tooresource ' 00000002-0000-0000-c000-000000000000'. Tento požadavek se nezdařil, protože hello klienta nebyl zadán tento prostředek ve svém seznamu requiredResourceAccess*.</span><span class="sxs-lookup"><span data-stu-id="631e8-183">*Error AADSTS65005: hello client application has requested access tooresource '00000002-0000-0000-c000-000000000000'. This request has failed because hello client has not specified this resource in its requiredResourceAccess list*.</span></span>

<span data-ttu-id="631e8-184">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="631e8-184">**Possible cause**</span></span>

<span data-ttu-id="631e8-185">objekt aplikace Hello je poškozený.</span><span class="sxs-lookup"><span data-stu-id="631e8-185">hello application object is corrupted.</span></span>

<span data-ttu-id="631e8-186">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="631e8-186">**Resolution**</span></span>

<span data-ttu-id="631e8-187">toosolve hello problém, aplikace hello odebrat z adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-187">toosolve hello problem, remove hello application from hello directory.</span></span> <span data-ttu-id="631e8-188">Pak přidejte a překonfigurujte hello aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="631e8-188">Then, add and reconfigure hello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="631e8-189">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="631e8-189">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="631e8-190">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-190">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="631e8-191">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="631e8-191">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="631e8-192">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="631e8-192">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="631e8-193">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="631e8-193">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="631e8-194">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="631e8-194">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="631e8-195">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="631e8-195">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="631e8-196">Klikněte na tlačítko **odstranit** v levém horním hello aplikace hello **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="631e8-196">Click **Delete** at hello top-left of hello application **Overview** blade.</span></span>

8.  <span data-ttu-id="631e8-197">Aktualizujte Azure AD a přidání aplikace hello z Galerie Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-197">Refresh Azure AD and Add hello application from hello Azure AD gallery.</span></span> <span data-ttu-id="631e8-198">Potom nakonfigurujte hello aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="631e8-198">Then, Configure hello application again.</span></span>

<span data-ttu-id="631e8-199">Po překonfigurování hello aplikace, musí být schopný toosign v toohello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="631e8-199">After reconfiguring hello application, you should be able toosign in toohello application.</span></span>

## <a name="certificate-or-key-not-configured"></a><span data-ttu-id="631e8-200">Certifikát nebo klíče není nakonfigurováno</span><span class="sxs-lookup"><span data-stu-id="631e8-200">Certificate or key not configured</span></span>

<span data-ttu-id="631e8-201">Chyba AADSTS50003: Žádné podpisový klíč nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="631e8-201">Error AADSTS50003: No signing key configured.</span></span>

<span data-ttu-id="631e8-202">**Možná příčina**</span><span class="sxs-lookup"><span data-stu-id="631e8-202">**Possible cause**</span></span>

<span data-ttu-id="631e8-203">objekt aplikace Hello je poškozený a Azure AD nerozpoznal hello certifikát nakonfigurovaný pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-203">hello application object is corrupted and Azure AD doesn’t recognize hello certificate configured for hello application.</span></span>

<span data-ttu-id="631e8-204">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="631e8-204">**Resolution**</span></span>

<span data-ttu-id="631e8-205">toodelete a vytvořte nový certifikát, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="631e8-205">toodelete and create a new certificate, follow hello steps below:</span></span>

1.  <span data-ttu-id="631e8-206">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="631e8-206">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="631e8-207">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-207">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="631e8-208">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="631e8-208">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="631e8-209">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="631e8-209">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="631e8-210">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="631e8-210">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="631e8-211">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="631e8-211">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="631e8-212">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="631e8-212">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="631e8-213">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="631e8-213">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="631e8-214">Klikněte na tlačítko **vytvořit nový certifikát** pod hello **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="631e8-214">click **Create new certificate** under hello **SAML signing Certificate** section.</span></span>

9.  <span data-ttu-id="631e8-215">Vyberte datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="631e8-215">Select Expiration date.</span></span> <span data-ttu-id="631e8-216">Potom klikněte na **uložit.**</span><span class="sxs-lookup"><span data-stu-id="631e8-216">Then, click **Save.**</span></span>

10. <span data-ttu-id="631e8-217">Zkontrolujte **aktivujte nový certifikát** toooverride hello active certifikátu.</span><span class="sxs-lookup"><span data-stu-id="631e8-217">Check **Make new certificate active** toooverride hello active certificate.</span></span> <span data-ttu-id="631e8-218">Potom klikněte na **Uložit** hello horní části okna hello a přijmout certifikát výměny tooactivate hello.</span><span class="sxs-lookup"><span data-stu-id="631e8-218">Then, click **Save** at hello top of hello blade and accept tooactivate hello rollover certificate.</span></span>

11. <span data-ttu-id="631e8-219">V části hello **SAML podpisový certifikát** klikněte na tlačítko **odebrat** tooremove hello **nepoužitý** certifikátu.</span><span class="sxs-lookup"><span data-stu-id="631e8-219">Under hello **SAML Signing Certificate** section, click **remove** tooremove hello **Unused** certificate.</span></span>

## <a name="problem-when-customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="631e8-220">Problém při přizpůsobení deklarace SAML hello odeslané tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="631e8-220">Problem when customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="631e8-221">toolearn způsobu deklarací identity atributu toocustomize hello SAML odeslané tooyour aplikace, najdete v [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="631e8-221">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="631e8-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="631e8-222">Next steps</span></span>
[<span data-ttu-id="631e8-223">Protokol požadavky pro Azure AD jedním přihlašování SAML</span><span class="sxs-lookup"><span data-stu-id="631e8-223">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)
