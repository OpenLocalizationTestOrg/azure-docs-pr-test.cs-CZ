---
title: "aaaHow tooconfigure federovaného jednotného přihlašování pro aplikaci Galerie Azure AD | Microsoft Docs"
description: "Jak tooconfigure federovaného jednotného přihlašování pro existující Galerie Azure AD aplikace a použití kurzy tooget rychlý přechod"
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
ms.openlocfilehash: a93de021fddd253e4fe663c221b822d12625fd54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="51de5-103">Jak tooconfigure federovaného jednotného přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="51de5-103">How tooconfigure federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="51de5-104">Všechny aplikace v galerii hello Azure AD, které jsou povoleny v rámci Enterprise jeden přihlašování schopností obsahuje podrobný kurz k dispozici.</span><span class="sxs-lookup"><span data-stu-id="51de5-104">All applications in hello Azure AD gallery enabled with Enterprise single sign-on capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="51de5-105">Dostanete hello [seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobné podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="51de5-105">You can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for detailed step-by-step guidance.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="51de5-106">Přehled kroků potřebných</span><span class="sxs-lookup"><span data-stu-id="51de5-106">Overview of steps required</span></span>
<span data-ttu-id="51de5-107">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="51de5-107">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="51de5-108">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="51de5-108">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="51de5-109">Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="51de5-109">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="51de5-110">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="51de5-110">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="51de5-111">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="51de5-111">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="51de5-112">Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="51de5-112">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="51de5-113">Přiřazení uživatelů toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="51de5-113">Assign users toohello application</span></span>](#assign-users-to-the-application)

## <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="51de5-114">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="51de5-114">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="51de5-115">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="51de5-115">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="51de5-116">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="51de5-116">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="51de5-117">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-117">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="51de5-118">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="51de5-118">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="51de5-119">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51de5-119">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="51de5-120">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="51de5-120">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="51de5-121">V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-121">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="51de5-122">Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51de5-122">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="51de5-123">Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="51de5-123">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="51de5-124">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="51de5-124">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="51de5-125">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-125">After a short period of time, you be able toosee hello application’s configuration blade.</span></span>

## <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="51de5-126">Konfigurovat jednotné přihlašování pro aplikace z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="51de5-126">Configure single sign-on for an application from hello Azure AD gallery</span></span>

<span data-ttu-id="51de5-127">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="51de5-127">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="51de5-128">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **spolusprávce**.</span><span class="sxs-lookup"><span data-stu-id="51de5-128">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="51de5-129">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-129">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="51de5-130">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="51de5-130">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="51de5-131">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51de5-131">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="51de5-132">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="51de5-132">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="51de5-133">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="51de5-133">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="51de5-134">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51de5-134">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="51de5-135">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51de5-135">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="51de5-136">Vyberte **na základě SAML přihlašování** z hello **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="51de5-136">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="51de5-137">Zadejte hello požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="51de5-137">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="51de5-138">Tyto hodnoty by měly získat od dodavatele aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-138">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="51de5-139">aplikace hello tooconfigure jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="51de5-139">tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span> <span data-ttu-id="51de5-140">Některé aplikace hello identifikátor je také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="51de5-140">For some applications, hello Identifier is also a required value.</span></span>

   2. <span data-ttu-id="51de5-141">aplikace hello tooconfigure jako jednotné přihlašování iniciované IdP, hello adresa URL odpovědi je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="51de5-141">tooconfigure hello application as IdP-initiated SSO, hello Reply URL it’s a required value.</span></span> <span data-ttu-id="51de5-142">Některé aplikace hello identifikátor je také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="51de5-142">For some applications, hello Identifier is also a required value.</span></span>

10. <span data-ttu-id="51de5-143">**Volitelné:** klikněte na tlačítko **zobrazit upřesňující nastavení adresy URL** Pokud chcete, aby toosee hello-požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="51de5-143">**Optional:** click **Show advanced URL settings** if you want toosee hello non-required values.</span></span>

11. <span data-ttu-id="51de5-144">V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="51de5-144">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="51de5-145">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="51de5-145">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

  <span data-ttu-id="51de5-146">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="51de5-146">tooadd an attribute:</span></span>
   
   1. <span data-ttu-id="51de5-147">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="51de5-147">click **Add attribute**.</span></span> <span data-ttu-id="51de5-148">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-148">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   1. <span data-ttu-id="51de5-149">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="51de5-149">Click **Save.**</span></span> <span data-ttu-id="51de5-150">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-150">You see hello new attribute in hello table.</span></span>

13. <span data-ttu-id="51de5-151">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-151">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="51de5-152">Navíc má adres URL metadat hello a certifikát vyžadovaný toosetup jednotné přihlašování s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-152">Also, you has hello metadata URLs and certificate required toosetup SSO with hello application.</span></span>

14. <span data-ttu-id="51de5-153">Klikněte na tlačítko **Uložit** toosave hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="51de5-153">Click **Save** toosave hello configuration.</span></span>

15. <span data-ttu-id="51de5-154">Přiřazení uživatelů toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="51de5-154">Assign users toohello application.</span></span>

## <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="51de5-155">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="51de5-155">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="51de5-156">tooselect hello identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="51de5-156">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="51de5-157">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="51de5-157">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="51de5-158">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-158">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="51de5-159">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="51de5-159">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="51de5-160">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51de5-160">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="51de5-161">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="51de5-161">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="51de5-162">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="51de5-162">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="51de5-163">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51de5-163">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="51de5-164">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51de5-164">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="51de5-165">V části hello **uživatelské atributy** vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="51de5-165">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="51de5-166">Hello vybranou možnost musí toomatch hello očekávaná hodnota v uživatel hello tooauthenticate aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-166">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

  >[!NOTE] 
  ><span data-ttu-id="51de5-167">Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="51de5-167">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="51de5-168">Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="51de5-168">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
  >
  >

9.  <span data-ttu-id="51de5-169">tooadd atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="51de5-169">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="51de5-170">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="51de5-170">tooadd an attribute:</span></span>
  
   1. <span data-ttu-id="51de5-171">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="51de5-171">click **Add attribute**.</span></span> <span data-ttu-id="51de5-172">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-172">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="51de5-173">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="51de5-173">Click **Save**.</span></span> <span data-ttu-id="51de5-174">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-174">You see hello new attribute in hello table.</span></span>

## <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="51de5-175">Stažení metadat hello Azure AD nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="51de5-175">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="51de5-176">metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="51de5-176">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="51de5-177">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="51de5-177">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="51de5-178">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-178">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="51de5-179">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="51de5-179">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="51de5-180">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51de5-180">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="51de5-181">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="51de5-181">click **All Applications** tooview a list of all your applications.</span></span>

  *  <span data-ttu-id="51de5-182">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="51de5-182">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications**.</span></span>

6.  <span data-ttu-id="51de5-183">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51de5-183">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="51de5-184">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51de5-184">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="51de5-185">Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="51de5-185">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="51de5-186">V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="51de5-186">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="51de5-187">Azure AD neposkytuje adresu URL tooget hello metadat.</span><span class="sxs-lookup"><span data-stu-id="51de5-187">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="51de5-188">Hello metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="51de5-188">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-toohello-application"></a><span data-ttu-id="51de5-189">Přiřazení uživatelů toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="51de5-189">Assign users toohello application</span></span>

<span data-ttu-id="51de5-190">tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="51de5-190">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="51de5-191">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="51de5-191">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="51de5-192">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-192">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="51de5-193">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="51de5-193">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="51de5-194">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51de5-194">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="51de5-195">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="51de5-195">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="51de5-196">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="51de5-196">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="51de5-197">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-197">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="51de5-198">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51de5-198">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="51de5-199">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="51de5-199">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="51de5-200">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="51de5-200">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="51de5-201">Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="51de5-201">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="51de5-202">Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="51de5-202">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="51de5-203">Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="51de5-203">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="51de5-204">**Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="51de5-204">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="51de5-205">Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="51de5-205">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="51de5-206">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="51de5-206">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="51de5-207">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="51de5-207">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="51de5-208">Po krátké době být hello uživatelů, které jste vybrali možnost toolaunch hello tyto aplikace pomocí metody popsané v části popis řešení hello.</span><span class="sxs-lookup"><span data-stu-id="51de5-208">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="51de5-209">Přizpůsobují se deklarace identity SAML hello odeslané tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="51de5-209">Customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="51de5-210">toolearn způsobu deklarací identity atributu toocustomize hello SAML odeslané tooyour aplikace, najdete v [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="51de5-210">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51de5-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51de5-211">Next steps</span></span>
[<span data-ttu-id="51de5-212">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="51de5-212">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)



