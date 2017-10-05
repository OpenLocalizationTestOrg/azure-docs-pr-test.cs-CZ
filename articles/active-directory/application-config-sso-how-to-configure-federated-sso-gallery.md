---
title: "Postup konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD | Microsoft Docs"
description: "Jak nakonfigurovat federovaného jednotného přihlašování pro existující aplikaci Galerie Azure AD a pomocí kurzy mohli rychle začít."
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
ms.openlocfilehash: 1b1d00718981b2c7d11f5b88428d02e16dd0b34d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="57ba9-103">Postup konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="57ba9-103">How to configure federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="57ba9-104">Všechny aplikace v galerii Azure AD povolit Enterprise jeden přihlašování schopností obsahuje podrobný kurz k dispozici.</span><span class="sxs-lookup"><span data-stu-id="57ba9-104">All applications in the Azure AD gallery enabled with Enterprise single sign-on capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="57ba9-105">Dostanete [seznam kurzů k integraci aplikací SaaS v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobné podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="57ba9-105">You can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for detailed step-by-step guidance.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="57ba9-106">Přehled kroků potřebných</span><span class="sxs-lookup"><span data-stu-id="57ba9-106">Overview of steps required</span></span>
<span data-ttu-id="57ba9-107">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="57ba9-107">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="57ba9-108">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="57ba9-108">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="57ba9-109">Nakonfigurujte hodnoty metadata aplikace ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="57ba9-109">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="57ba9-110">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="57ba9-110">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="57ba9-111">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="57ba9-111">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="57ba9-112">Konfigurovat Azure AD metadata hodnoty v aplikaci (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="57ba9-112">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="57ba9-113">Přiřazení uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="57ba9-113">Assign users to the application</span></span>](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="57ba9-114">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="57ba9-114">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="57ba9-115">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="57ba9-115">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="57ba9-116">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="57ba9-116">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="57ba9-117">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="57ba9-117">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="57ba9-118">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="57ba9-118">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="57ba9-119">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="57ba9-119">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="57ba9-120">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="57ba9-120">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="57ba9-121">V **zadejte název** textbox z **přidat z Galerie** části, zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="57ba9-121">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="57ba9-122">Vyberte aplikaci, kterou chcete nakonfigurovat pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="57ba9-122">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="57ba9-123">Před přidáním aplikace, můžete změnit její název ze **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="57ba9-123">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="57ba9-124">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="57ba9-124">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="57ba9-125">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="57ba9-125">After a short period of time, you be able to see the application’s configuration blade.</span></span>

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="57ba9-126">Konfigurovat jednotné přihlašování pro aplikace z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="57ba9-126">Configure single sign-on for an application from the Azure AD gallery</span></span>

<span data-ttu-id="57ba9-127">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="57ba9-127">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="57ba9-128">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **spolusprávce**.</span><span class="sxs-lookup"><span data-stu-id="57ba9-128">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="57ba9-129">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="57ba9-129">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="57ba9-130">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="57ba9-130">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="57ba9-131">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="57ba9-131">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="57ba9-132">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="57ba9-132">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="57ba9-133">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="57ba9-133">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="57ba9-134">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="57ba9-134">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="57ba9-135">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="57ba9-135">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="57ba9-136">Vyberte **na základě SAML přihlašování** z **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="57ba9-136">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="57ba9-137">Zadejte požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="57ba9-137">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="57ba9-138">Tyto hodnoty by měly získat od dodavatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="57ba9-138">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="57ba9-139">Chcete-li nakonfigurovat aplikaci jako spouštěná SP jednotného přihlašování adresa URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="57ba9-139">To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span> <span data-ttu-id="57ba9-140">Některé aplikace je identifikátor také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="57ba9-140">For some applications, the Identifier is also a required value.</span></span>

   2. <span data-ttu-id="57ba9-141">Chcete-li nakonfigurovat aplikaci jako spouštěná IdP SSO adresa URL odpovědi je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="57ba9-141">To configure the application as IdP-initiated SSO, the Reply URL it’s a required value.</span></span> <span data-ttu-id="57ba9-142">Některé aplikace je identifikátor také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="57ba9-142">For some applications, the Identifier is also a required value.</span></span>

10. <span data-ttu-id="57ba9-143">**Volitelné:** klikněte na tlačítko **zobrazit upřesňující nastavení adresy URL** Pokud budete chtít prohlédnout-požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="57ba9-143">**Optional:** click **Show advanced URL settings** if you want to see the non-required values.</span></span>

11. <span data-ttu-id="57ba9-144">V **uživatelské atributy**, vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="57ba9-144">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="57ba9-145">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="57ba9-145">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

  <span data-ttu-id="57ba9-146">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="57ba9-146">To add an attribute:</span></span>
   
   1. <span data-ttu-id="57ba9-147">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="57ba9-147">click **Add attribute**.</span></span> <span data-ttu-id="57ba9-148">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="57ba9-148">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   1. <span data-ttu-id="57ba9-149">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="57ba9-149">Click **Save.**</span></span> <span data-ttu-id="57ba9-150">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="57ba9-150">You see the new attribute in the table.</span></span>

13. <span data-ttu-id="57ba9-151">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  dokumentace přístupu na tom, jak nakonfigurovat jednotné přihlašování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="57ba9-151">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="57ba9-152">Navíc má adres URL metadat a certifikát nutný k nastavení jednotného přihlašování k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="57ba9-152">Also, you has the metadata URLs and certificate required to setup SSO with the application.</span></span>

14. <span data-ttu-id="57ba9-153">Klikněte na tlačítko **Uložit** konfiguraci uložíte.</span><span class="sxs-lookup"><span data-stu-id="57ba9-153">Click **Save** to save the configuration.</span></span>

15. <span data-ttu-id="57ba9-154">Přiřazení uživatelů k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="57ba9-154">Assign users to the application.</span></span>

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="57ba9-155">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="57ba9-155">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="57ba9-156">K výběru identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="57ba9-156">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="57ba9-157">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="57ba9-157">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="57ba9-158">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="57ba9-158">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="57ba9-159">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="57ba9-159">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="57ba9-160">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="57ba9-160">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="57ba9-161">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="57ba9-161">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="57ba9-162">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="57ba9-162">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="57ba9-163">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="57ba9-163">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="57ba9-164">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="57ba9-164">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="57ba9-165">V části **uživatelské atributy** vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="57ba9-165">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="57ba9-166">Vybraná možnost musí odpovídat očekávaná hodnota v aplikaci k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="57ba9-166">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

  >[!NOTE] 
  ><span data-ttu-id="57ba9-167">Azure AD vyberte formát pro atribut NameID (identifikátor uživatele) na základě hodnoty vybraných nebo formát požadovanou aplikaci v SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="57ba9-167">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="57ba9-168">Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="57ba9-168">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
  >
  >

9.  <span data-ttu-id="57ba9-169">Chcete-li přidat atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="57ba9-169">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="57ba9-170">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="57ba9-170">To add an attribute:</span></span>
  
   1. <span data-ttu-id="57ba9-171">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="57ba9-171">click **Add attribute**.</span></span> <span data-ttu-id="57ba9-172">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="57ba9-172">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="57ba9-173">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="57ba9-173">Click **Save**.</span></span> <span data-ttu-id="57ba9-174">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="57ba9-174">You see the new attribute in the table.</span></span>

## <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="57ba9-175">Stáhnout Azure AD metadata nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="57ba9-175">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="57ba9-176">Chcete-li stáhnout metadata aplikace nebo certifikát z Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="57ba9-176">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="57ba9-177">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="57ba9-177">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="57ba9-178">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="57ba9-178">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="57ba9-179">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="57ba9-179">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="57ba9-180">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="57ba9-180">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="57ba9-181">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="57ba9-181">click **All Applications** to view a list of all your applications.</span></span>

  *  <span data-ttu-id="57ba9-182">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="57ba9-182">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications**.</span></span>

6.  <span data-ttu-id="57ba9-183">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="57ba9-183">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="57ba9-184">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="57ba9-184">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="57ba9-185">Přejděte na **SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="57ba9-185">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="57ba9-186">V závislosti na tom, jaké aplikace vyžaduje konfiguraci jednotné přihlašování uvidíte buď možnost Stáhnout soubor XML s metadaty nebo certifikát.</span><span class="sxs-lookup"><span data-stu-id="57ba9-186">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="57ba9-187">Azure AD neposkytuje adresu URL se získat metadata.</span><span class="sxs-lookup"><span data-stu-id="57ba9-187">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="57ba9-188">Metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="57ba9-188">The metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-to-the-application"></a><span data-ttu-id="57ba9-189">Přiřazení uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="57ba9-189">Assign users to the application</span></span>

<span data-ttu-id="57ba9-190">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="57ba9-190">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="57ba9-191">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="57ba9-191">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="57ba9-192">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="57ba9-192">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="57ba9-193">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="57ba9-193">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="57ba9-194">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="57ba9-194">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="57ba9-195">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="57ba9-195">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="57ba9-196">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="57ba9-196">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="57ba9-197">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="57ba9-197">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="57ba9-198">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="57ba9-198">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="57ba9-199">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="57ba9-199">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="57ba9-200">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="57ba9-200">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="57ba9-201">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="57ba9-201">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="57ba9-202">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="57ba9-202">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="57ba9-203">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="57ba9-203">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="57ba9-204">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="57ba9-204">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="57ba9-205">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="57ba9-205">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="57ba9-206">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="57ba9-206">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="57ba9-207">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="57ba9-207">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="57ba9-208">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace pomocí metody popsané v části popis řešení.</span><span class="sxs-lookup"><span data-stu-id="57ba9-208">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="customizing-the-saml-claims-sent-to-an-application"></a><span data-ttu-id="57ba9-209">Přizpůsobují se deklarace identity SAML, odeslané do aplikace</span><span class="sxs-lookup"><span data-stu-id="57ba9-209">Customizing the SAML claims sent to an application</span></span>

<span data-ttu-id="57ba9-210">Naučte se přizpůsobovat deklarací identity atributu SAML odeslaných do vaší aplikace, najdete v tématu [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="57ba9-210">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57ba9-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57ba9-211">Next steps</span></span>
[<span data-ttu-id="57ba9-212">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="57ba9-212">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)



