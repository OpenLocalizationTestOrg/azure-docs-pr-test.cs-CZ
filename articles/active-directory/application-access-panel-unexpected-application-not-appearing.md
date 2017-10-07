---
title: "hello přístupového panelu se nezobrazují aaaAn přiřazené aplikace | Microsoft Docs"
description: "Poradce při potížích se aplikace nejsou zobrazeny v hello přístupového panelu"
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
ms.reviwer: japere
ms.openlocfilehash: 089883f406267df4552c7fc991883f335ad49fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="an-assigned-application-is-not-appearing-on-hello-access-panel"></a><span data-ttu-id="37ac7-103">Přiřazená aplikace se nezobrazují hello přístupového panelu</span><span class="sxs-lookup"><span data-stu-id="37ac7-103">An assigned application is not appearing on hello access panel</span></span>

<span data-ttu-id="37ac7-104">Hello přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spusťte cloudové aplikace této hello správce Azure AD udělil je přístup k.</span><span class="sxs-lookup"><span data-stu-id="37ac7-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="37ac7-105">Tyto aplikace jsou konfigurovány jménem uživatele hello na portálu Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="37ac7-106">Hello aplikace musí být správně nakonfigurované a přiřazené toohello uživatele nebo skupiny hello uživatel je členem toosee hello aplikace hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-106">hello application must be configured properly and assigned toohello user or a group hello user is a member of toosee hello application in hello Access Panel.</span></span>

<span data-ttu-id="37ac7-107">Typ Hello aplikací, které zobrazuje uživatel možná spadat hello následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="37ac7-107">hello type of apps a user may be seeing fall in hello following categories:</span></span>

-   <span data-ttu-id="37ac7-108">Aplikace Office 365</span><span class="sxs-lookup"><span data-stu-id="37ac7-108">Office 365 Applications</span></span>

-   <span data-ttu-id="37ac7-109">Společnost Microsoft a třetích stran aplikace, které jsou nakonfigurované pomocí jednotného přihlašování na základě federation</span><span class="sxs-lookup"><span data-stu-id="37ac7-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="37ac7-110">Aplikace založené na heslech jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="37ac7-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="37ac7-111">Aplikace s stávajících řešení pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="37ac7-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="37ac7-112">Obecné problémy toocheck nejprve</span><span class="sxs-lookup"><span data-stu-id="37ac7-112">General issues toocheck first</span></span>

-   <span data-ttu-id="37ac7-113">Pokud aplikaci jste právě přidali tooa uživatele, opakujte toosign a odhlašování do přístupového panelu hello uživatele po několika minutách toosee Pokud aplikace hello je přidána.</span><span class="sxs-lookup"><span data-stu-id="37ac7-113">If an application was just added tooa user, try toosign in and out again into hello user’s Access Panel after a few minutes toosee if hello application is added.</span></span>

-   <span data-ttu-id="37ac7-114">Pokud licence byla právě odebrána ze uživatele nebo skupiny hello uživatel je že členem skupiny to může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny pro toobe změny provedené.</span><span class="sxs-lookup"><span data-stu-id="37ac7-114">If a license was just removed from a user or group hello user is a member of this may take a long time, depending on hello size and complexity of hello group for changes toobe made.</span></span> <span data-ttu-id="37ac7-115">Povolit další dobu před přihlášením hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-115">Allow for extra time before signing into hello Access Panel.</span></span>

## <a name="problems-related-tooapplication-configuration"></a><span data-ttu-id="37ac7-116">Problémy související tooapplication konfigurace</span><span class="sxs-lookup"><span data-stu-id="37ac7-116">Problems related tooapplication configuration</span></span>

<span data-ttu-id="37ac7-117">Aplikace nemusí zobrazovaných v Panel přístupu uživatele, protože není správně nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="37ac7-117">An application may not be appearing in a user’s Access Panel because it is not configured properly.</span></span> <span data-ttu-id="37ac7-118">Tady jsou některé způsoby, jak můžete řešit problémy související tooapplication konfigurace:</span><span class="sxs-lookup"><span data-stu-id="37ac7-118">Below are some ways you can troubleshoot issues related tooapplication configuration:</span></span>

-   [<span data-ttu-id="37ac7-119">Jak tooconfigure federovaného jednotného přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="37ac7-119">How tooconfigure federated single sign-on for an Azure AD gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [<span data-ttu-id="37ac7-120">Jak tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="37ac7-120">How tooconfigure federated single sign-on for a non-gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="37ac7-121">Jak tooconfigure heslo jednotné přihlašování v aplikaci pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="37ac7-121">How tooconfigure a password single sign-on application for an Azure AD gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="37ac7-122">Jak tooconfigure heslo jednotné přihlašování aplikace pro aplikaci není Galerie</span><span class="sxs-lookup"><span data-stu-id="37ac7-122">How tooconfigure a password single sign-on application for a non-gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="37ac7-123">Jak tooconfigure federovaného jednotného přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="37ac7-123">How tooconfigure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="37ac7-124">Všechny aplikace v galerii Azure AD hello povolit funkce Enterprise Single Sign-On obsahuje podrobný kurz k dispozici.</span><span class="sxs-lookup"><span data-stu-id="37ac7-124">All applications in hello Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="37ac7-125">Dostanete hello [seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobností podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="37ac7-125">You can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="37ac7-126">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="37ac7-126">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="37ac7-127">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="37ac7-127">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="37ac7-128">Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="37ac7-128">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="37ac7-129">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="37ac7-129">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="37ac7-130">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="37ac7-130">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="37ac7-131">Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="37ac7-131">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="37ac7-132">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="37ac7-132">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="37ac7-133">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-133">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-134">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="37ac7-134">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="37ac7-135">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-135">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-136">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-136">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-137">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-137">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-138">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="37ac7-138">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="37ac7-139">V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-139">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="37ac7-140">Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37ac7-140">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="37ac7-141">Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="37ac7-141">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="37ac7-142">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ac7-142">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="37ac7-143">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-143">After a short period, you be able toosee hello application’s configuration blade.</span></span>

#### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="37ac7-144">Konfigurovat jednotné přihlašování pro aplikace z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="37ac7-144">Configure single sign-on for an application from hello Azure AD gallery</span></span>

<span data-ttu-id="37ac7-145">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-145">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-146">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-146">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="37ac7-147">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-147">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-148">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-148">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-149">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-149">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-150">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-150">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="37ac7-151">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-151">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="37ac7-152">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37ac7-152">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="37ac7-153">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-153">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="37ac7-154">Vyberte **na základě SAML přihlašování** z hello **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-154">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="37ac7-155">Zadejte hello požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-155">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="37ac7-156">Tyto hodnoty by měly získat od dodavatele aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-156">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="37ac7-157">aplikace hello tooconfigure jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="37ac7-157">tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span> <span data-ttu-id="37ac7-158">Některé aplikace hello identifikátor je také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="37ac7-158">For some applications, hello Identifier is also a required value.</span></span>

   2. <span data-ttu-id="37ac7-159">aplikace hello tooconfigure jako jednotné přihlašování iniciované IdP, hello adresa URL odpovědi je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="37ac7-159">tooconfigure hello application as IdP-initiated SSO, hello Reply URL it’s a required value.</span></span> <span data-ttu-id="37ac7-160">Některé aplikace hello identifikátor je také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="37ac7-160">For some applications, hello Identifier is also a required value.</span></span>

10. <span data-ttu-id="37ac7-161">**Volitelné:** klikněte na tlačítko **zobrazit upřesňující nastavení adresy URL** Pokud chcete, aby toosee hello-požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="37ac7-161">**Optional:** click **Show advanced URL settings** if you want toosee hello non-required values.</span></span>

11. <span data-ttu-id="37ac7-162">V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-162">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="37ac7-163">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="37ac7-163">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="37ac7-164">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="37ac7-164">tooadd an attribute:</span></span>

   1. <span data-ttu-id="37ac7-165">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-165">click **Add attribute**.</span></span> <span data-ttu-id="37ac7-166">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-166">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="37ac7-167">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-167">click **Save.**</span></span> <span data-ttu-id="37ac7-168">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-168">You see hello new attribute in hello table.</span></span>

13. <span data-ttu-id="37ac7-169">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-169">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="37ac7-170">Navíc má adres URL metadat hello a certifikát vyžadovaný toosetup jednotné přihlašování s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-170">Also, you has hello metadata URLs and certificate required toosetup SSO with hello application.</span></span>

14. <span data-ttu-id="37ac7-171">Klikněte na tlačítko **Uložit** toosave hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="37ac7-171">click **Save** toosave hello configuration.</span></span>

15. <span data-ttu-id="37ac7-172">Přiřazení uživatelů toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ac7-172">Assign users toohello application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="37ac7-173">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="37ac7-173">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="37ac7-174">tooselect hello identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-174">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-175">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-175">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="37ac7-176">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-176">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-177">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-177">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-178">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-178">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-179">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-179">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="37ac7-180">Pokud aplikace hello chcete tooshow vytvořit tady nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-180">If you do not see hello application you want tooshow up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="37ac7-181">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37ac7-181">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="37ac7-182">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-182">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="37ac7-183">V části hello **uživatelské atributy** vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-183">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="37ac7-184">Hello vybranou možnost musí toomatch hello očekávaná hodnota v uživatel hello tooauthenticate aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-184">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="37ac7-185">Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="37ac7-185">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="37ac7-186">Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="37ac7-186">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="37ac7-187">tooadd atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="37ac7-187">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="37ac7-188">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="37ac7-188">tooadd an attribute:</span></span>

   1. <span data-ttu-id="37ac7-189">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-189">click **Add attribute**.</span></span> <span data-ttu-id="37ac7-190">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-190">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="37ac7-191">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-191">click **Save.**</span></span> <span data-ttu-id="37ac7-192">Zobrazí se hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-192">You will see hello new attribute in hello table.</span></span>

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="37ac7-193">Stažení metadat hello Azure AD nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="37ac7-193">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="37ac7-194">metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-194">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-195">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-195">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="37ac7-196">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-196">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-197">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-197">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-198">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-198">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-199">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-199">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="37ac7-200">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-200">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="37ac7-201">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37ac7-201">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="37ac7-202">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-202">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="37ac7-203">Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="37ac7-203">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="37ac7-204">V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-204">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

    <span data-ttu-id="37ac7-205">Azure AD neposkytuje adresu URL tooget hello metadat.</span><span class="sxs-lookup"><span data-stu-id="37ac7-205">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="37ac7-206">Hello metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="37ac7-206">hello metadata can only be retrieved as a XML file.</span></span>

### <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="37ac7-207">Jak tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="37ac7-207">How tooconfigure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="37ac7-208">tooconfigure bez Galerie aplikace, budete muset toohave Azure AD premium a hello aplikace podporuje SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="37ac7-208">tooconfigure a non-gallery application, you need toohave Azure AD premium and hello application supports SAML 2.0.</span></span> <span data-ttu-id="37ac7-209">Další informace o verzích služby Azure AD, najdete v článku [ceny služby Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="37ac7-209">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="37ac7-210">Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="37ac7-210">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="37ac7-211">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="37ac7-211">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="37ac7-212">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="37ac7-212">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="37ac7-213">Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="37ac7-213">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

#### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="37ac7-214">Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="37ac7-214">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="37ac7-215">tooconfigure jednotné přihlašování pro aplikace, která není v galerii Azure AD hello, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-215">tooconfigure single sign-on for an application that is not in hello Azure AD gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-216">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-216">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="37ac7-217">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-217">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-218">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-218">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-219">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-219">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-220">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="37ac7-220">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="37ac7-221">Klikněte na tlačítko **aplikace bez Galerie** v hello **přidat vlastní aplikaci** části.</span><span class="sxs-lookup"><span data-stu-id="37ac7-221">click **Non-gallery application** in hello **Add your own app** section.</span></span>

7.  <span data-ttu-id="37ac7-222">Zadejte název hello aplikace hello v hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="37ac7-222">Enter hello name of hello application in hello **Name** textbox.</span></span>

8.  <span data-ttu-id="37ac7-223">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ac7-223">Click **Add** button, tooadd hello application.</span></span>

9.  <span data-ttu-id="37ac7-224">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-224">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="37ac7-225">Vyberte **na základě SAML přihlašování** v hello **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-225">Select **SAML-based Sign-on** in hello **Mode** dropdown.</span></span>

11. <span data-ttu-id="37ac7-226">Zadejte hello požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-226">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="37ac7-227">Tyto hodnoty by měly získat od dodavatele aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-227">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="37ac7-228">aplikace hello tooconfigure jako jednotné přihlašování iniciované deklarací identity, zadejte hello adresa URL odpovědi a hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="37ac7-228">tooconfigure hello application as IdP-initiated SSO, enter hello Reply URL and hello Identifier.</span></span>

   2.  <span data-ttu-id="37ac7-229">**Volitelné:** tooconfigure hello aplikace jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="37ac7-229">**Optional:** tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="37ac7-230">V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-230">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="37ac7-231">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="37ac7-231">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="37ac7-232">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="37ac7-232">tooadd an attribute:</span></span>

   1. <span data-ttu-id="37ac7-233">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-233">click **Add attribute**.</span></span> <span data-ttu-id="37ac7-234">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-234">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="37ac7-235">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-235">Click **Save.**</span></span> <span data-ttu-id="37ac7-236">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-236">You see hello new attribute in hello table.</span></span>

14. <span data-ttu-id="37ac7-237">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-237">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="37ac7-238">Navíc má Azure AD adresy URL a certifikát nutný pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-238">Also, you has Azure AD URLs and certificate required for hello application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="37ac7-239">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="37ac7-239">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="37ac7-240">tooselect hello identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-240">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-241">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-241">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="37ac7-242">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-242">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-243">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-243">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-244">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-244">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-245">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-245">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="37ac7-246">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-246">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="37ac7-247">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37ac7-247">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="37ac7-248">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-248">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="37ac7-249">V části hello **uživatelské atributy** vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-249">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="37ac7-250">Hello vybranou možnost musí toomatch hello očekávaná hodnota v uživatel hello tooauthenticate aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-250">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="37ac7-251">Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="37ac7-251">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="37ac7-252">Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="37ac7-252">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="37ac7-253">tooadd atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="37ac7-253">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="37ac7-254">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="37ac7-254">tooadd an attribute:</span></span>

   1. <span data-ttu-id="37ac7-255">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-255">click **Add attribute**.</span></span> <span data-ttu-id="37ac7-256">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-256">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="37ac7-257">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-257">Click **Save.**</span></span> <span data-ttu-id="37ac7-258">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-258">You see hello new attribute in hello table.</span></span>

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="37ac7-259">Stažení metadat hello Azure AD nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="37ac7-259">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="37ac7-260">metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-260">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-261">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-261">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="37ac7-262">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-262">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-263">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-263">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-264">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-264">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-265">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-265">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="37ac7-266">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-266">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="37ac7-267">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37ac7-267">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="37ac7-268">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-268">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="37ac7-269">Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="37ac7-269">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="37ac7-270">V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-270">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="37ac7-271">Azure AD neposkytuje adresu URL tooget hello metadat.</span><span class="sxs-lookup"><span data-stu-id="37ac7-271">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="37ac7-272">Hello metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="37ac7-272">hello metadata can only be retrieved as a XML file.</span></span>

### <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="37ac7-273">Jak tooconfigure heslo jednotného přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="37ac7-273">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="37ac7-274">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="37ac7-274">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="37ac7-275">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="37ac7-275">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="37ac7-276">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="37ac7-276">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="37ac7-277">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="37ac7-277">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="37ac7-278">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-278">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-279">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="37ac7-279">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="37ac7-280">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-280">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-281">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-281">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-282">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-282">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-283">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="37ac7-283">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="37ac7-284">V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-284">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="37ac7-285">Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37ac7-285">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="37ac7-286">Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="37ac7-286">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="37ac7-287">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ac7-287">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="37ac7-288">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-288">After a short period, you be able toosee hello application’s configuration blade.</span></span>

#### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="37ac7-289">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="37ac7-289">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="37ac7-290">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-290">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-291">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-291">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="37ac7-292">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-292">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-293">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-293">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-294">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-294">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-295">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-295">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="37ac7-296">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-296">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="37ac7-297">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37ac7-297">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="37ac7-298">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-298">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="37ac7-299">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-299">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="37ac7-300">[Přiřazení uživatelů s aplikací toohello](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="37ac7-300">[Assign users toohello application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="37ac7-301">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="37ac7-301">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="37ac7-302">Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.</span><span class="sxs-lookup"><span data-stu-id="37ac7-302">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

### <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="37ac7-303">Jak tooconfigure heslo jednotného přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="37ac7-303">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="37ac7-304">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="37ac7-304">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="37ac7-305">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="37ac7-305">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="37ac7-306">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="37ac7-306">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a><span data-ttu-id="37ac7-307">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="37ac7-307">Add a non-gallery application</span></span>

<span data-ttu-id="37ac7-308">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-308">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-309">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-309">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="37ac7-310">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-310">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-311">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-311">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-312">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-312">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-313">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="37ac7-313">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="37ac7-314">Klikněte na tlačítko **Galerie jiná aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-314">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="37ac7-315">Zadejte název vaší aplikace hello v hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="37ac7-315">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="37ac7-316">Vyberte **přidat.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-316">Select **Add.**</span></span>

<span data-ttu-id="37ac7-317">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-317">After a short period, you be able toosee hello application’s configuration blade.</span></span>

#### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="37ac7-318">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="37ac7-318">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="37ac7-319">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-319">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-320">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-320">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="37ac7-321">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-321">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-322">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-322">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-323">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-323">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-324">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-324">click **All Applications** tooview a list of all your applications.</span></span>

    1.  <span data-ttu-id="37ac7-325">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-325">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="37ac7-326">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37ac7-326">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="37ac7-327">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-327">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="37ac7-328">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-328">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="37ac7-329">Zadejte hello **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-329">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="37ac7-330">Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k.</span><span class="sxs-lookup"><span data-stu-id="37ac7-330">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="37ac7-331">Zkontrolujte, zda jsou viditelné na adrese URL hello hello přihlášení pole.</span><span class="sxs-lookup"><span data-stu-id="37ac7-331">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="37ac7-332">[Přiřazení uživatelů s aplikací toohello](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="37ac7-332">[Assign users toohello application](#how-to-assign-a-user-to-an-application-directly).</span></span>

11. <span data-ttu-id="37ac7-333">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="37ac7-333">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="37ac7-334">Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.</span><span class="sxs-lookup"><span data-stu-id="37ac7-334">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="problems-related-tooassigning-applications-toousers"></a><span data-ttu-id="37ac7-335">Problémy související tooassigning aplikace toousers</span><span class="sxs-lookup"><span data-stu-id="37ac7-335">Problems related tooassigning applications toousers</span></span>

<span data-ttu-id="37ac7-336">Uživatel nemusí být zobrazuje aplikace na jejich přístupového panelu nejsou přiřazeny toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ac7-336">A user may not be seeing an application on their Access Panel because they are not assigned toohello application.</span></span> <span data-ttu-id="37ac7-337">Tady jsou některé toocheck způsoby:</span><span class="sxs-lookup"><span data-stu-id="37ac7-337">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="37ac7-338">Zkontrolujte, pokud uživatel je přiřazená toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="37ac7-338">Check if a user is assigned toohello application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="37ac7-339">Jak tooassign tooan aplikace uživatele přímo</span><span class="sxs-lookup"><span data-stu-id="37ac7-339">How tooassign a user tooan application directly</span></span>](#how-to-assign-a-user-to-an-application-directly)

-   [<span data-ttu-id="37ac7-340">Zkontrolujte, zda uživatel je přiřazena licence tooa související toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="37ac7-340">Check if a user is assigned tooa license related toohello application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [<span data-ttu-id="37ac7-341">Jak tooassign uživatel tooa licencí</span><span class="sxs-lookup"><span data-stu-id="37ac7-341">How tooassign a license tooa user</span></span>](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-toohello-application"></a><span data-ttu-id="37ac7-342">Zkontrolujte, pokud uživatel je přiřazená toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="37ac7-342">Check if a user is assigned toohello application</span></span>

<span data-ttu-id="37ac7-343">toocheck Pokud uživatel přiřazený toohello aplikace, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="37ac7-343">toocheck if a user is assigned toohello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-344">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-344">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="37ac7-345">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-345">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-346">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-346">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-347">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-347">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-348">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-348">click **All Applications** tooview a list of all your applications.</span></span>

6.  <span data-ttu-id="37ac7-349">**Hledání** pro název hello aplikace hello nejistá.</span><span class="sxs-lookup"><span data-stu-id="37ac7-349">**Search** for hello name of hello application in question.</span></span>

7.  <span data-ttu-id="37ac7-350">Klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-350">click **Users and groups**.</span></span>

8.  <span data-ttu-id="37ac7-351">Toosee zkontrolujte, zda váš uživatel toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ac7-351">Check toosee if your user is assigned toohello application.</span></span>

   * <span data-ttu-id="37ac7-352">Pokud ne, postupujte podle kroků hello v "jak tooassign tooan aplikace uživatele přímo" toodo tak.</span><span class="sxs-lookup"><span data-stu-id="37ac7-352">If not follow hello steps in “How tooassign a user tooan application directly” toodo so.</span></span>

### <a name="how-tooassign-a-user-tooan-application-directly"></a><span data-ttu-id="37ac7-353">Jak tooassign tooan aplikace uživatele přímo</span><span class="sxs-lookup"><span data-stu-id="37ac7-353">How tooassign a user tooan application directly</span></span>

<span data-ttu-id="37ac7-354">tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-354">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-355">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-355">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="37ac7-356">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-356">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-357">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-357">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-358">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-358">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-359">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-359">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="37ac7-360">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-360">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="37ac7-361">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-361">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="37ac7-362">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-362">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="37ac7-363">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="37ac7-363">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="37ac7-364">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="37ac7-364">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="37ac7-365">Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="37ac7-365">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="37ac7-366">Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-366">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="37ac7-367">Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-367">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="37ac7-368">**Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-368">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="37ac7-369">Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ac7-369">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="37ac7-370">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="37ac7-370">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="37ac7-371">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="37ac7-371">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="37ac7-372">Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-372">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a><span data-ttu-id="37ac7-373">Zkontrolujte, jestli je uživatel v rámci licence související toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="37ac7-373">Check if a user is under a license related toohello application</span></span>

<span data-ttu-id="37ac7-374">toocheck uživatele je přiřazena licence, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-374">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-375">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-375">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="37ac7-376">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-376">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-377">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-377">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-378">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-378">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-379">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-379">click **All users**.</span></span>

6.  <span data-ttu-id="37ac7-380">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="37ac7-380">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="37ac7-381">Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="37ac7-381">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

  * <span data-ttu-id="37ac7-382">Pokud uživatel hello přiřazené tooan Office licence tato povolí tooappear aplikace první strany Office na Panel přístupu hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="37ac7-382">If hello user is assigned tooan Office license this enable First Party Office applications tooappear on hello user’s Access Panel.</span></span>

### <a name="how-tooassign-a-user-a-license"></a><span data-ttu-id="37ac7-383">Jak tooassign uživatelské licence</span><span class="sxs-lookup"><span data-stu-id="37ac7-383">How tooassign a user a license</span></span> 

<span data-ttu-id="37ac7-384">tooassign licence tooa uživatele, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="37ac7-384">tooassign a license tooa user, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-385">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-385">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="37ac7-386">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-386">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-387">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-387">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-388">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-388">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-389">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-389">click **All users**.</span></span>

6.  <span data-ttu-id="37ac7-390">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="37ac7-390">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="37ac7-391">Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="37ac7-391">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

8.  <span data-ttu-id="37ac7-392">Klikněte na tlačítko hello **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="37ac7-392">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="37ac7-393">Vyberte **jeden nebo více produktů** z hello seznam dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="37ac7-393">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="37ac7-394">**Volitelné** klikněte na tlačítko hello **přiřazení možností** položky toogranularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="37ac7-394">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="37ac7-395">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="37ac7-395">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="37ac7-396">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign tyto licence toothis uživatele.</span><span class="sxs-lookup"><span data-stu-id="37ac7-396">Click hello **Assign** button tooassign these licenses toothis user.</span></span>

## <a name="problems-related-tooassigning-applications-toogroups"></a><span data-ttu-id="37ac7-397">Problémy související tooassigning aplikace toogroups</span><span class="sxs-lookup"><span data-stu-id="37ac7-397">Problems related tooassigning applications toogroups</span></span>

<span data-ttu-id="37ac7-398">Uživatele může být zobrazuje aplikace na jejich přístupového panelu jsou součástí skupiny, která byla přiřazena aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-398">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned hello application.</span></span> <span data-ttu-id="37ac7-399">Tady jsou některé toocheck způsoby:</span><span class="sxs-lookup"><span data-stu-id="37ac7-399">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="37ac7-400">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="37ac7-400">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="37ac7-401">Jak tooassign aplikaci tooa skupiny přímo</span><span class="sxs-lookup"><span data-stu-id="37ac7-401">How tooassign an application tooa group directly</span></span>](#how-to-assign-an-application-to-a-group-directly)

-   [<span data-ttu-id="37ac7-402">Zkontrolujte, zda uživatel je součástí skupiny přiřazené licence tooa</span><span class="sxs-lookup"><span data-stu-id="37ac7-402">Check if a user is part of group assigned tooa license</span></span>](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [<span data-ttu-id="37ac7-403">Jak tooassign tooa skupiny licencí</span><span class="sxs-lookup"><span data-stu-id="37ac7-403">How tooassign a license tooa group</span></span>](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="37ac7-404">Zkontrolujte členství uživatele ve skupinách</span><span class="sxs-lookup"><span data-stu-id="37ac7-404">Check a user’s group memberships</span></span>

<span data-ttu-id="37ac7-405">toocheck členství ve skupině, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-405">toocheck a group’s membership, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-406">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-406">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="37ac7-407">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-407">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-408">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-408">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-409">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-409">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-410">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-410">click **All users**.</span></span>

6.  <span data-ttu-id="37ac7-411">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="37ac7-411">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="37ac7-412">Klikněte na tlačítko **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-412">click **Groups**.</span></span>

8.  <span data-ttu-id="37ac7-413">Zkontrolujte toosee, pokud uživatel je součástí aplikace toohello skupiny přiřazené.</span><span class="sxs-lookup"><span data-stu-id="37ac7-413">Check toosee if your user is part of a Group assigned toohello application.</span></span>

  * <span data-ttu-id="37ac7-414">Pokud chcete, aby tooremove hello uživatele ze skupiny hello **klikněte na řádek hello** hello skupiny a vyberte možnost odstranit.</span><span class="sxs-lookup"><span data-stu-id="37ac7-414">If you want tooremove hello user from hello group, **click hello row** of hello group and select delete.</span></span>

### <a name="how-tooassign-an-application-tooa-group-directly"></a><span data-ttu-id="37ac7-415">Jak tooassign aplikaci tooa skupiny přímo</span><span class="sxs-lookup"><span data-stu-id="37ac7-415">How tooassign an application tooa group directly</span></span>

<span data-ttu-id="37ac7-416">tooassign jeden nebo více skupin tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-416">tooassign one or more groups tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-417">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-417">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="37ac7-418">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-418">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-419">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-419">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-420">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-420">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-421">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="37ac7-421">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="37ac7-422">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-422">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="37ac7-423">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-423">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="37ac7-424">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-424">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="37ac7-425">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="37ac7-425">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="37ac7-426">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="37ac7-426">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="37ac7-427">Typ v hello **název úplné skupiny** hello skupiny, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="37ac7-427">Type in hello **full group name** of hello group you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="37ac7-428">Pozastavte ukazatel myši nad hello **skupiny** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-428">Hover over hello **group** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="37ac7-429">Klikněte na tlačítko hello políčko další toohello skupiny profilu fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-429">Click hello checkbox next toohello group’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="37ac7-430">**Volitelné:** Pokud byste chtěli příliš**přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole a klikněte na zaškrtávací políčko tooadd hello této skupiny toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-430">**Optional:** If you would like too**add more than one group**, type in another **full group name** into hello **Search by name or email address** search box, and click hello checkbox tooadd this group toohello **Selected** list.</span></span>

13. <span data-ttu-id="37ac7-431">Po dokončení výběru skupiny klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37ac7-431">When you are finished selecting groups, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="37ac7-432">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect toohello tooassign role skupinách vybrali.</span><span class="sxs-lookup"><span data-stu-id="37ac7-432">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello groups you have selected.</span></span>

15. <span data-ttu-id="37ac7-433">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybrané skupiny.</span><span class="sxs-lookup"><span data-stu-id="37ac7-433">Click hello **Assign** button tooassign hello application toohello selected groups.</span></span>

<span data-ttu-id="37ac7-434">Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="37ac7-434">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

### <a name="check-if-a-user-is-part-of-group-assigned-tooa-license"></a><span data-ttu-id="37ac7-435">Zkontrolujte, zda uživatel je součástí skupiny přiřazené licence tooa</span><span class="sxs-lookup"><span data-stu-id="37ac7-435">Check if a user is part of group assigned tooa license</span></span>

1.  <span data-ttu-id="37ac7-436">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-436">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="37ac7-437">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-437">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-438">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-438">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-439">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-439">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-440">Klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-440">click **All users**.</span></span>

6.  <span data-ttu-id="37ac7-441">**Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="37ac7-441">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="37ac7-442">Klikněte na tlačítko **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-442">click **Groups**.</span></span>

8.  <span data-ttu-id="37ac7-443">Klikněte na řádek hello určité skupiny.</span><span class="sxs-lookup"><span data-stu-id="37ac7-443">click hello row of a specific group.</span></span>

9.  <span data-ttu-id="37ac7-444">Klikněte na tlačítko **licence** toosee, které skupiny licencí hello přiřazenému tooit.</span><span class="sxs-lookup"><span data-stu-id="37ac7-444">click **Licenses** toosee which licenses hello group has assigned tooit.</span></span>

   * <span data-ttu-id="37ac7-445">Pokud skupina hello přiřazené tooan Office licence to může povolit určité aplikace tooappear první strany Office na Panel přístupu hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="37ac7-445">If hello group is assigned tooan Office license this may enable certain First Party Office applications tooappear on hello user’s Access Panel.</span></span>

### <a name="how-tooassign-a-license-tooa-group"></a><span data-ttu-id="37ac7-446">Jak tooassign tooa skupiny licencí</span><span class="sxs-lookup"><span data-stu-id="37ac7-446">How tooassign a license tooa group</span></span>

<span data-ttu-id="37ac7-447">tooassign tooa skupiny licencí, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="37ac7-447">tooassign a license tooa group, follow hello steps below:</span></span>

1.  <span data-ttu-id="37ac7-448">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="37ac7-448">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="37ac7-449">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-449">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="37ac7-450">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="37ac7-450">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="37ac7-451">Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="37ac7-451">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="37ac7-452">Klikněte na tlačítko **všechny skupiny**.</span><span class="sxs-lookup"><span data-stu-id="37ac7-452">click **All groups**.</span></span>

6.  <span data-ttu-id="37ac7-453">**Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="37ac7-453">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="37ac7-454">Klikněte na tlačítko **licence** toosee, které skupiny licencí hello má přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="37ac7-454">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

8.  <span data-ttu-id="37ac7-455">Klikněte na tlačítko hello **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="37ac7-455">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="37ac7-456">Vyberte **jeden nebo více produktů** z hello seznam dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="37ac7-456">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="37ac7-457">**Volitelné** klikněte na tlačítko hello **přiřazení možností** položky toogranularly přiřadit produkty.</span><span class="sxs-lookup"><span data-stu-id="37ac7-457">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="37ac7-458">Klikněte na tlačítko **Ok** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="37ac7-458">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="37ac7-459">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign těchto toothis skupin licencí.</span><span class="sxs-lookup"><span data-stu-id="37ac7-459">Click hello **Assign** button tooassign these licenses toothis group.</span></span> <span data-ttu-id="37ac7-460">To může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="37ac7-460">This may take a long time, depending on hello size and complexity of hello group.</span></span>

>[!NOTE]
><span data-ttu-id="37ac7-461">toodo to rychlejší, vezměte v úvahu dočasně přiřazení licence toohello uživatele přímo.</span><span class="sxs-lookup"><span data-stu-id="37ac7-461">toodo this faster, consider temporarily assigning a license toohello user directly.</span></span> 
>
>

## <a name="next-steps"></a><span data-ttu-id="37ac7-462">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37ac7-462">Next steps</span></span>
[<span data-ttu-id="37ac7-463">Přidat nové uživatele tooAzure služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="37ac7-463">Add new users tooAzure Active Directory</span></span>](active-directory-users-create-azure-portal.md)

