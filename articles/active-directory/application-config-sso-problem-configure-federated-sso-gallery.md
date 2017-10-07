---
title: "aaaProblem konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD | Microsoft Docs"
description: "Některé běžné problémy hello se můžete setkat při konfiguraci federovaný jeden adres přihlášení pomocí SAML pro aplikace, které jsou uvedeny v hello Azure AD Application Gallery"
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
ms.openlocfilehash: 2ae1e511bd49b19159e1ab83cf79a7db5403b9a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="69a51-103">Problém konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="69a51-103">Problem configuring federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="69a51-104">Pokud dojde k potížím při konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="69a51-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="69a51-105">Ověřte, že jste provedli všechny kroky hello v kurzu hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="69a51-105">Verify you have followed all hello steps in hello tutorial for hello application.</span></span> <span data-ttu-id="69a51-106">V konfiguraci aplikace hello máte vložené dokumentaci na tom, jak tooconfigure hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="69a51-106">In hello application’s configuration, you have inline documentation on how tooconfigure hello application.</span></span> <span data-ttu-id="69a51-107">Navíc můžete přístup hello [seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobností podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="69a51-107">Also, you can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

## <a name="cant-add-another-instance-of-hello-application"></a><span data-ttu-id="69a51-108">Nelze přidat jiná instance aplikace hello</span><span class="sxs-lookup"><span data-stu-id="69a51-108">Can’t add another instance of hello application</span></span>

<span data-ttu-id="69a51-109">tooadd druhou instanci aplikace, je třeba toobe moci:</span><span class="sxs-lookup"><span data-stu-id="69a51-109">tooadd a second instance of an application, you need toobe able to:</span></span>

-   <span data-ttu-id="69a51-110">Jedinečný identifikátor pro druhou instanci hello nakonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="69a51-110">Configure a unique identifier for hello second instance.</span></span> <span data-ttu-id="69a51-111">Nebudete moct tooconfigure hello, používá stejný identifikátor pro první instanci hello.</span><span class="sxs-lookup"><span data-stu-id="69a51-111">You won’t be able tooconfigure hello same identifier used for hello first instance.</span></span>

-   <span data-ttu-id="69a51-112">Nakonfigurujte jiný certifikát než hello, jedna pro hello první instance.</span><span class="sxs-lookup"><span data-stu-id="69a51-112">Configure a different certificate than hello one used for hello first instance.</span></span>

<span data-ttu-id="69a51-113">Pokud hello aplikace nepodporuje žádné z výše uvedených hello.</span><span class="sxs-lookup"><span data-stu-id="69a51-113">If hello application doesn’t support any of hello above.</span></span> <span data-ttu-id="69a51-114">Potom nebudete moct tooconfigure druhou instanci.</span><span class="sxs-lookup"><span data-stu-id="69a51-114">Then, you won’t be able tooconfigure a second instance.</span></span>

## <a name="cant-add-hello-identifier-or-hello-reply-url"></a><span data-ttu-id="69a51-115">Nelze přidat hello identifikátor nebo hello adresa URL odpovědi</span><span class="sxs-lookup"><span data-stu-id="69a51-115">Can’t add hello Identifier or hello Reply URL</span></span>

<span data-ttu-id="69a51-116">Pokud nejste možné tooconfigure hello identifikátor nebo hello adresa URL odpovědi, potvrďte hello identifikátor a adresa URL odpovědi hodnoty odpovídat hello vzory předem nakonfigurované pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="69a51-116">If you’re not able tooconfigure hello Identifier or hello Reply URL, confirm hello Identifier and Reply URL values match hello patterns pre-configured for hello application.</span></span>

<span data-ttu-id="69a51-117">vzory hello tooknow předem nakonfigurované pro aplikaci hello:</span><span class="sxs-lookup"><span data-stu-id="69a51-117">tooknow hello patterns pre-configured for hello application:</span></span>

1.  <span data-ttu-id="69a51-118">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.** Přejděte toostep 7.</span><span class="sxs-lookup"><span data-stu-id="69a51-118">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.** Go toostep 7.</span></span> <span data-ttu-id="69a51-119">Pokud jste už v okně Konfigurace aplikace hello na Azure AD.</span><span class="sxs-lookup"><span data-stu-id="69a51-119">If you are already in hello application configuration blade on Azure AD.</span></span>

2.  <span data-ttu-id="69a51-120">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="69a51-120">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="69a51-121">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="69a51-121">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="69a51-122">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="69a51-122">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="69a51-123">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="69a51-123">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="69a51-124">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="69a51-124">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="69a51-125">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="69a51-125">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="69a51-126">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="69a51-126">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="69a51-127">Vyberte **na základě SAML přihlašování** z hello **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="69a51-127">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="69a51-128">Přejděte toohello **identifikátor** nebo **adresa URL odpovědi** textovému poli, v části hello **oddílu domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="69a51-128">Go toohello **Identifier** or **Reply URL** textbox, under hello **Domain and URLs section.**</span></span>

10. <span data-ttu-id="69a51-129">Existují tři způsoby tooknow hello podporované vzory pro aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="69a51-129">There are three ways tooknow hello supported patterns for hello application:</span></span>

   * <span data-ttu-id="69a51-130">V textovém poli hello, uvidíte nalezeny hello podporovány jako zástupný znak *příklad:* <https://contoso.com>.</span><span class="sxs-lookup"><span data-stu-id="69a51-130">In hello textbox, you see hello supported pattern(s) as a placeholder *Example:* <https://contoso.com>.</span></span>

   * <span data-ttu-id="69a51-131">Pokud vzor hello není podporován, zobrazí se při pokusu o tooenter hello hodnotu v textovém poli hello červený vykřičník.</span><span class="sxs-lookup"><span data-stu-id="69a51-131">if hello pattern is not supported, you see a red exclamation mark when you try tooenter hello value in hello textbox.</span></span> <span data-ttu-id="69a51-132">Umístěte ukazatel myši nad hello červený vykřičník, uvidíte vzory hello podporována.</span><span class="sxs-lookup"><span data-stu-id="69a51-132">If you hover your mouse over hello red exclamation mark, you see hello supported patterns.</span></span>

   * <span data-ttu-id="69a51-133">V kurzu hello hello aplikace můžete také načíst informace o vzory hello podporována.</span><span class="sxs-lookup"><span data-stu-id="69a51-133">In hello tutorial for hello application, you can also get information about hello supported patterns.</span></span> <span data-ttu-id="69a51-134">V části hello **konfigurovat Azure AD jednotné přihlašování** části.</span><span class="sxs-lookup"><span data-stu-id="69a51-134">Under hello **Configure Azure AD single sign-on** section.</span></span> <span data-ttu-id="69a51-135">Přejděte toohello krok pro hodnoty nakonfigurované hello pod hello **domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="69a51-135">Go toohello step for configured hello values under hello **Domain and URLs** section.</span></span>

<span data-ttu-id="69a51-136">Pokud hello se hodnoty neshodují s hello vzory předem nakonfigurované na Azure AD.</span><span class="sxs-lookup"><span data-stu-id="69a51-136">If hello values don’t match with hello patterns pre-configured on Azure AD.</span></span> <span data-ttu-id="69a51-137">Můžete:</span><span class="sxs-lookup"><span data-stu-id="69a51-137">You can:</span></span>

-   <span data-ttu-id="69a51-138">Práce s hello aplikace dodavatele tooget hodnot, které odpovídají hello vzor předem nakonfigurované na Azure AD</span><span class="sxs-lookup"><span data-stu-id="69a51-138">Work with hello application vendor tooget values that match hello pattern pre-configured on Azure AD</span></span>

-   <span data-ttu-id="69a51-139">Nebo se obrátit na tým Azure AD v < aadapprequest@microsoft.com > nebo komentář v hello kurz toorequest hello aktualizace schémat hello podporována pro aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="69a51-139">Or, you can contact Azure AD team at <aadapprequest@microsoft.com> or leave a comment in hello tutorial toorequest hello update of hello supported patterns for hello application</span></span>

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a><span data-ttu-id="69a51-140">Kde lze nastavit formát hello EntityID (identifikátor uživatele)</span><span class="sxs-lookup"><span data-stu-id="69a51-140">Where do I set hello EntityID (User Identifier) format</span></span>

<span data-ttu-id="69a51-141">Nebudete moct tooselect hello EntityID (identifikátor uživatele) formát, Azure AD odešle toohello aplikace v odpovědi hello po ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="69a51-141">You won’t be able tooselect hello EntityID (User Identifier) format that Azure AD sends toohello application in hello response after user authentication.</span></span>

<span data-ttu-id="69a51-142">Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="69a51-142">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="69a51-143">Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části hello NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="69a51-143">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy,</span></span>

## <a name="cant-find-hello-azure-ad-metadata-toocomplete-hello-configuration-with-hello-application"></a><span data-ttu-id="69a51-144">Nelze najít hello hello konfigurace toocomplete metadata Azure AD s aplikací hello</span><span class="sxs-lookup"><span data-stu-id="69a51-144">Can’t find hello Azure AD metadata toocomplete hello configuration with hello application</span></span>

<span data-ttu-id="69a51-145">metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="69a51-145">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="69a51-146">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="69a51-146">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="69a51-147">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="69a51-147">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="69a51-148">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="69a51-148">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="69a51-149">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="69a51-149">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="69a51-150">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="69a51-150">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="69a51-151">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="69a51-151">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="69a51-152">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="69a51-152">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="69a51-153">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="69a51-153">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="69a51-154">Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="69a51-154">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="69a51-155">V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="69a51-155">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="69a51-156">Azure AD neposkytuje adresu URL tooget hello metadat.</span><span class="sxs-lookup"><span data-stu-id="69a51-156">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="69a51-157">Hello metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="69a51-157">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a><span data-ttu-id="69a51-158">Nevíte, jak deklarace SAML toocustomize odeslané tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="69a51-158">Don't know how toocustomize SAML claims sent tooan application</span></span>

<span data-ttu-id="69a51-159">toolearn způsobu deklarací identity atributu toocustomize hello SAML odeslané tooyour aplikace, najdete v [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="69a51-159">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69a51-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69a51-160">Next steps</span></span>
[<span data-ttu-id="69a51-161">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="69a51-161">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
