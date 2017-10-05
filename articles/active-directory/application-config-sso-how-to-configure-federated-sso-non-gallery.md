---
title: "Postup konfigurace federované jednotné přihlašování pro aplikace bez Galerie | Microsoft Docs"
description: "Postup konfigurace federované jednotné přihlašování pro vlastní aplikace bez galerie, která chcete integrovat se službou Azure AD"
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
ms.openlocfilehash: da7bc05c6452cde4d0236806f249559f178dd4e1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="53234-103">Postup konfigurace federované jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="53234-103">How to configure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="53234-104">Konfigurace aplikace bez galerie, potřebujete Azure AD premium a aplikace podporuje SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="53234-104">To configure a non-gallery application, you need to have Azure AD premium and the application supports SAML 2.0.</span></span> <span data-ttu-id="53234-105">Další informace o verzích služby Azure AD, najdete v článku [ceny služby Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="53234-105">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="53234-106">Přehled kroků potřebných</span><span class="sxs-lookup"><span data-stu-id="53234-106">Overview of steps required</span></span>
<span data-ttu-id="53234-107">Zde je podrobný přehled kroků nezbytných ke konfiguraci federované jednotné přihlašování pro jiný Galerie (například vlastní) aplikace.</span><span class="sxs-lookup"><span data-stu-id="53234-107">Below is a high level overview of the steps required to configure federated single sign-on for a non-gallery (e.g., custom) application.</span></span>

-   [<span data-ttu-id="53234-108">Nakonfigurujte hodnoty metadata aplikace ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="53234-108">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#_Configuring_single_sign-on)

-   [<span data-ttu-id="53234-109">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="53234-109">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="53234-110">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="53234-110">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="53234-111">Konfigurovat Azure AD metadata hodnoty v aplikaci (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="53234-111">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#_Configuring_single_sign-on)

-   [<span data-ttu-id="53234-112">Přiřazení uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="53234-112">Assign users to the application</span></span>](#_Assign_users_to_the_application)

## <a name="configuring-single-sign-on-to-non-gallery-applications"></a><span data-ttu-id="53234-113">Konfigurace jednotného přihlašování k aplikacím jiných Galerie</span><span class="sxs-lookup"><span data-stu-id="53234-113">Configuring single sign-on to non-gallery applications</span></span>

<span data-ttu-id="53234-114">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, která není v galerii Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="53234-114">To configure single sign-on for an application that is not in the Azure AD gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="53234-115">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="53234-115">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="53234-116">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="53234-116">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="53234-117">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="53234-117">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="53234-118">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="53234-118">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="53234-119">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="53234-119">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="53234-120">Klikněte na tlačítko **aplikace bez Galerie** v **přidat vlastní aplikaci** části</span><span class="sxs-lookup"><span data-stu-id="53234-120">click **Non-gallery application** in the **Add your own app** section</span></span>

7.  <span data-ttu-id="53234-121">Zadejte název aplikace **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="53234-121">Enter the name of the application in the **Name** textbox.</span></span>

8.  <span data-ttu-id="53234-122">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="53234-122">Click **Add** button, to add the application.</span></span>

9.  <span data-ttu-id="53234-123">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="53234-123">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="53234-124">Vyberte **na základě SAML přihlašování** v **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="53234-124">Select **SAML-based Sign-on** in the **Mode** dropdown.</span></span>

11. <span data-ttu-id="53234-125">Zadejte požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="53234-125">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="53234-126">Tyto hodnoty by měly získat od dodavatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="53234-126">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="53234-127">Chcete-li nakonfigurovat aplikaci jako jednotné přihlašování iniciované deklarací identity, zadejte adresa URL odpovědi a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="53234-127">To configure the application as IdP-initiated SSO, enter the Reply URL and the Identifier.</span></span>

   2. <span data-ttu-id="53234-128">**Volitelné:** konfigurace aplikace jako spouštěná SP jednotného přihlašování adresa URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="53234-128">**Optional:** To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="53234-129">V **uživatelské atributy**, vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="53234-129">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="53234-130">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="53234-130">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="53234-131">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="53234-131">To add an attribute:</span></span>

   1. <span data-ttu-id="53234-132">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="53234-132">click **Add attribute**.</span></span> <span data-ttu-id="53234-133">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="53234-133">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="53234-134">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="53234-134">Click **Save.**</span></span> <span data-ttu-id="53234-135">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="53234-135">You see the new attribute in the table.</span></span>

14. <span data-ttu-id="53234-136">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  dokumentace přístupu na tom, jak nakonfigurovat jednotné přihlašování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="53234-136">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="53234-137">Navíc má Azure AD adresy URL a certifikát nutný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="53234-137">Also, you has Azure AD URLs and certificate required for the application.</span></span>

15. [<span data-ttu-id="53234-138">Přiřazení uživatelů k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="53234-138">Assign users to the application.</span></span>](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="53234-139">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="53234-139">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="53234-140">K výběru identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="53234-140">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="53234-141">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="53234-141">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="53234-142">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="53234-142">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="53234-143">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="53234-143">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="53234-144">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="53234-144">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="53234-145">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="53234-145">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="53234-146">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="53234-146">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="53234-147">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="53234-147">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="53234-148">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="53234-148">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="53234-149">V části **uživatelské atributy** vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="53234-149">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="53234-150">Vybraná možnost musí odpovídat očekávaná hodnota v aplikaci k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="53234-150">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

 ><span data-ttu-id="53234-151">[! Poznámka} Azure AD vyberte formát požadovanou aplikaci v SAML AuthRequest nebo formát pro atribut NameID (identifikátor uživatele) na základě vybrané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="53234-151">[!NOTE} Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="53234-152">Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="53234-152">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
 >
 >

9.  <span data-ttu-id="53234-153">Chcete-li přidat atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="53234-153">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="53234-154">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="53234-154">To add an attribute:</span></span>

   1. <span data-ttu-id="53234-155">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="53234-155">click **Add attribute**.</span></span> <span data-ttu-id="53234-156">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="53234-156">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="53234-157">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="53234-157">Click **Save.**</span></span> <span data-ttu-id="53234-158">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="53234-158">You see the new attribute in the table.</span></span>

## <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="53234-159">Stáhnout Azure AD metadata nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="53234-159">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="53234-160">Chcete-li stáhnout metadata aplikace nebo certifikát z Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="53234-160">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="53234-161">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="53234-161">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="53234-162">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="53234-162">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="53234-163">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="53234-163">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="53234-164">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="53234-164">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="53234-165">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="53234-165">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="53234-166">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="53234-166">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="53234-167">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="53234-167">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="53234-168">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="53234-168">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="53234-169">Přejděte na **SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="53234-169">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="53234-170">V závislosti na tom, jaké aplikace vyžaduje konfiguraci jednotné přihlašování uvidíte buď možnost Stáhnout soubor XML s metadaty nebo certifikát.</span><span class="sxs-lookup"><span data-stu-id="53234-170">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="53234-171">Azure AD neposkytuje adresu URL se získat metadata.</span><span class="sxs-lookup"><span data-stu-id="53234-171">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="53234-172">Metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="53234-172">The metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-to-the-application"></a><span data-ttu-id="53234-173">Přiřazení uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="53234-173">Assign users to the application</span></span>

<span data-ttu-id="53234-174">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="53234-174">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="53234-175">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="53234-175">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="53234-176">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="53234-176">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="53234-177">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="53234-177">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="53234-178">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="53234-178">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="53234-179">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="53234-179">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="53234-180">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="53234-180">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="53234-181">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="53234-181">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="53234-182">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="53234-182">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="53234-183">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="53234-183">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="53234-184">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="53234-184">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="53234-185">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="53234-185">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="53234-186">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="53234-186">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="53234-187">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="53234-187">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="53234-188">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="53234-188">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="53234-189">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="53234-189">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="53234-190">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="53234-190">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="53234-191">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="53234-191">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="53234-192">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace pomocí metody popsané v části popis řešení.</span><span class="sxs-lookup"><span data-stu-id="53234-192">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="customizing-the-saml-claims-sent-to-an-application"></a><span data-ttu-id="53234-193">Přizpůsobují se deklarace identity SAML, odeslané do aplikace</span><span class="sxs-lookup"><span data-stu-id="53234-193">Customizing the SAML claims sent to an application</span></span>

<span data-ttu-id="53234-194">Naučte se přizpůsobovat deklarací identity atributu SAML odeslaných do vaší aplikace, najdete v tématu [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="53234-194">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="53234-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53234-195">Next steps</span></span>
[<span data-ttu-id="53234-196">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="53234-196">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
