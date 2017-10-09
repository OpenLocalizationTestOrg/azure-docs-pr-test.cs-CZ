---
title: "aaaHow tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie | Microsoft Docs"
description: "Jak tooconfigure federovaného jednotného přihlašování pro vlastní aplikaci bez galerie, které chcete toointegrate s Azure AD"
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
ms.openlocfilehash: f4a37cb500c075d0ce917ad8f13411f2ce208c56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="bbc2a-103">Jak tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="bbc2a-103">How tooconfigure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="bbc2a-104">tooconfigure bez Galerie aplikace, budete muset toohave Azure AD premium a hello aplikace podporuje SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-104">tooconfigure a non-gallery application, you need toohave Azure AD premium and hello application supports SAML 2.0.</span></span> <span data-ttu-id="bbc2a-105">Další informace o verzích služby Azure AD, najdete v článku [ceny služby Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="bbc2a-105">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="bbc2a-106">Přehled kroků potřebných</span><span class="sxs-lookup"><span data-stu-id="bbc2a-106">Overview of steps required</span></span>
<span data-ttu-id="bbc2a-107">Zde je podrobný přehled hello kroky požadované tooconfigure federovaného jednotného přihlašování pro jiný Galerie (například vlastní) aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-107">Below is a high level overview of hello steps required tooconfigure federated single sign-on for a non-gallery (e.g., custom) application.</span></span>

-   [<span data-ttu-id="bbc2a-108">Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="bbc2a-108">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#_Configuring_single_sign-on)

-   [<span data-ttu-id="bbc2a-109">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="bbc2a-109">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="bbc2a-110">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="bbc2a-110">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="bbc2a-111">Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="bbc2a-111">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#_Configuring_single_sign-on)

-   [<span data-ttu-id="bbc2a-112">Přiřazení uživatelů toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="bbc2a-112">Assign users toohello application</span></span>](#_Assign_users_to_the_application)

## <a name="configuring-single-sign-on-toonon-gallery-applications"></a><span data-ttu-id="bbc2a-113">Konfigurace jednoho přihlášení toonon Galerie aplikací</span><span class="sxs-lookup"><span data-stu-id="bbc2a-113">Configuring single sign-on toonon-gallery applications</span></span>

<span data-ttu-id="bbc2a-114">tooconfigure jednotné přihlašování pro aplikace, která není v galerii Azure AD hello, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="bbc2a-114">tooconfigure single sign-on for an application that is not in hello Azure AD gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="bbc2a-115">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-115">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bbc2a-116">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-116">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bbc2a-117">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-117">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bbc2a-118">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-118">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bbc2a-119">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-119">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="bbc2a-120">Klikněte na tlačítko **aplikace bez Galerie** v hello **přidat vlastní aplikaci** části</span><span class="sxs-lookup"><span data-stu-id="bbc2a-120">click **Non-gallery application** in hello **Add your own app** section</span></span>

7.  <span data-ttu-id="bbc2a-121">Zadejte název hello aplikace hello v hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-121">Enter hello name of hello application in hello **Name** textbox.</span></span>

8.  <span data-ttu-id="bbc2a-122">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-122">Click **Add** button, tooadd hello application.</span></span>

9.  <span data-ttu-id="bbc2a-123">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-123">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="bbc2a-124">Vyberte **na základě SAML přihlašování** v hello **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-124">Select **SAML-based Sign-on** in hello **Mode** dropdown.</span></span>

11. <span data-ttu-id="bbc2a-125">Zadejte hello požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-125">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="bbc2a-126">Tyto hodnoty by měly získat od dodavatele aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-126">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="bbc2a-127">aplikace hello tooconfigure jako jednotné přihlašování iniciované deklarací identity, zadejte hello adresa URL odpovědi a hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-127">tooconfigure hello application as IdP-initiated SSO, enter hello Reply URL and hello Identifier.</span></span>

   2. <span data-ttu-id="bbc2a-128">**Volitelné:** tooconfigure hello aplikace jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-128">**Optional:** tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="bbc2a-129">V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-129">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="bbc2a-130">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-130">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="bbc2a-131">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="bbc2a-131">tooadd an attribute:</span></span>

   1. <span data-ttu-id="bbc2a-132">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-132">click **Add attribute**.</span></span> <span data-ttu-id="bbc2a-133">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-133">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="bbc2a-134">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-134">Click **Save.**</span></span> <span data-ttu-id="bbc2a-135">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-135">You see hello new attribute in hello table.</span></span>

14. <span data-ttu-id="bbc2a-136">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-136">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="bbc2a-137">Navíc má Azure AD adresy URL a certifikát nutný pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-137">Also, you has Azure AD URLs and certificate required for hello application.</span></span>

15. [<span data-ttu-id="bbc2a-138">Přiřazení uživatelů toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-138">Assign users toohello application.</span></span>](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="bbc2a-139">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="bbc2a-139">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="bbc2a-140">tooselect hello identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="bbc2a-140">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="bbc2a-141">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-141">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bbc2a-142">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-142">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bbc2a-143">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-143">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bbc2a-144">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-144">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bbc2a-145">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-145">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="bbc2a-146">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-146">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bbc2a-147">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-147">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="bbc2a-148">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-148">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bbc2a-149">V části hello **uživatelské atributy** vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-149">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="bbc2a-150">Hello vybranou možnost musí toomatch hello očekávaná hodnota v uživatel hello tooauthenticate aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-150">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

 ><span data-ttu-id="bbc2a-151">[! Poznámka:} Azure AD, vyberte formát hello atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello hello aplikací v hello SAML AuthRequest požadovaný formát.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-151">[!NOTE} Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="bbc2a-152">Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-152">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
 >
 >

9.  <span data-ttu-id="bbc2a-153">tooadd atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-153">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="bbc2a-154">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="bbc2a-154">tooadd an attribute:</span></span>

   1. <span data-ttu-id="bbc2a-155">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-155">click **Add attribute**.</span></span> <span data-ttu-id="bbc2a-156">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-156">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="bbc2a-157">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-157">Click **Save.**</span></span> <span data-ttu-id="bbc2a-158">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-158">You see hello new attribute in hello table.</span></span>

## <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="bbc2a-159">Stažení metadat hello Azure AD nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="bbc2a-159">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="bbc2a-160">metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="bbc2a-160">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="bbc2a-161">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-161">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bbc2a-162">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-162">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bbc2a-163">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-163">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bbc2a-164">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-164">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bbc2a-165">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-165">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="bbc2a-166">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-166">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bbc2a-167">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-167">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="bbc2a-168">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-168">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bbc2a-169">Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-169">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="bbc2a-170">V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-170">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="bbc2a-171">Azure AD neposkytuje adresu URL tooget hello metadat.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-171">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="bbc2a-172">Hello metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-172">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-toohello-application"></a><span data-ttu-id="bbc2a-173">Přiřazení uživatelů toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="bbc2a-173">Assign users toohello application</span></span>

<span data-ttu-id="bbc2a-174">tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="bbc2a-174">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="bbc2a-175">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-175">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="bbc2a-176">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-176">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bbc2a-177">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-177">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bbc2a-178">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-178">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bbc2a-179">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-179">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="bbc2a-180">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="bbc2a-180">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bbc2a-181">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-181">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="bbc2a-182">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-182">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bbc2a-183">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-183">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="bbc2a-184">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-184">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="bbc2a-185">Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-185">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="bbc2a-186">Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-186">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="bbc2a-187">Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-187">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="bbc2a-188">**Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-188">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="bbc2a-189">Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-189">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="bbc2a-190">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-190">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="bbc2a-191">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-191">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="bbc2a-192">Po krátké době být hello uživatelů, které jste vybrali možnost toolaunch hello tyto aplikace pomocí metody popsané v části popis řešení hello.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-192">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="bbc2a-193">Přizpůsobují se deklarace identity SAML hello odeslané tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="bbc2a-193">Customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="bbc2a-194">toolearn způsobu deklarací identity atributu toocustomize hello SAML odeslané tooyour aplikace, najdete v [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-194">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbc2a-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bbc2a-195">Next steps</span></span>
[<span data-ttu-id="bbc2a-196">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="bbc2a-196">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
