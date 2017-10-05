---
title: 'Kurz: Azure Active Directory integrace s Replicon | Microsoft Docs'
description: "Další informace o použití Replicon s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2aeeceb61191962b62892b8409218684f76c6fa8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a><span data-ttu-id="ac9b4-103">Kurz: Azure Active Directory integrace s Replicon</span><span class="sxs-lookup"><span data-stu-id="ac9b4-103">Tutorial: Azure Active Directory integration with Replicon</span></span>
<span data-ttu-id="ac9b4-104">Cílem tohoto kurzu je zobrazit integraci Azure a Replicon.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-104">The objective of this tutorial is to show the integration of Azure and Replicon.</span></span> <span data-ttu-id="ac9b4-105">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="ac9b4-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="ac9b4-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="ac9b4-106">A valid Azure subscription</span></span>
* <span data-ttu-id="ac9b4-107">Replicon klienta</span><span class="sxs-lookup"><span data-stu-id="ac9b4-107">A Replicon tenant</span></span>

<span data-ttu-id="ac9b4-108">Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace ve vaší lokalitě společnosti Replicon (služba Zprostředkovatel iniciované přihlašování) nebo pomocí uživatele Azure AD, které jste přiřadili Replicon [Úvod do přístupového panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ac9b4-108">After completing this tutorial, the Azure AD users you have assigned to Replicon will be able to single sign into the application at your Replicon company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="ac9b4-109">Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ac9b4-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="ac9b4-110">Povolení integrace aplikace pro Replicon</span><span class="sxs-lookup"><span data-stu-id="ac9b4-110">Enabling the application integration for Replicon</span></span>
2. <span data-ttu-id="ac9b4-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="ac9b4-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="ac9b4-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="ac9b4-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="ac9b4-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="ac9b4-113">Assigning users</span></span>

<span data-ttu-id="ac9b4-114">![Scénář](./media/active-directory-saas-replicon-tutorial/IC777798.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-114">![Scenario](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-replicon"></a><span data-ttu-id="ac9b4-115">Povolit integraci aplikace pro Replicon</span><span class="sxs-lookup"><span data-stu-id="ac9b4-115">Enable the application integration for Replicon</span></span>
<span data-ttu-id="ac9b4-116">Cílem této části se popisují postup povolení integrace aplikace pro Replicon.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-116">The objective of this section is to outline how to enable the application integration for Replicon.</span></span>

<span data-ttu-id="ac9b4-117">**Pokud chcete povolit integraci aplikací pro Replicon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ac9b4-117">**To enable the application integration for Replicon, perform the following steps:**</span></span>

1. <span data-ttu-id="ac9b4-118">V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="ac9b4-119">![Služby Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-119">![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="ac9b4-120">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="ac9b4-121">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="ac9b4-122">![Aplikace](./media/active-directory-saas-replicon-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-122">![Applications](./media/active-directory-saas-replicon-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="ac9b4-123">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-123">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="ac9b4-124">![Přidat aplikaci](./media/active-directory-saas-replicon-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-124">![Add application](./media/active-directory-saas-replicon-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="ac9b4-125">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="ac9b4-126">![Přidání aplikace z gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-126">![Add an application from gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="ac9b4-127">V **vyhledávacího pole**, typ **Replicon**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-127">In the **search box**, type **Replicon**.</span></span>
   
    <span data-ttu-id="ac9b4-128">![Galerie aplikací](./media/active-directory-saas-replicon-tutorial/IC777799.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-128">![Application gallery](./media/active-directory-saas-replicon-tutorial/IC777799.png "Application gallery")</span></span>
7. <span data-ttu-id="ac9b4-129">V podokně výsledků vyberte **Replicon**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-129">In the results pane, select **Replicon**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="ac9b4-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="ac9b4-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ac9b4-131">Configure single sign-on</span></span>

<span data-ttu-id="ac9b4-132">Cílem této části se popisují, jak uživatelům povolit ověřování na Replicon ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-132">The objective of this section is to outline how to enable users to authenticate to Replicon with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="ac9b4-133">**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ac9b4-133">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="ac9b4-134">Na portálu Azure classic na **Replicon** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-134">In the Azure classic portal, on the **Replicon** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="ac9b4-135">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777801.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-135">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777801.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="ac9b4-136">Na **jak chcete uživatelům se přihlásit Replicon** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-136">On the **How would you like users to sign on to Replicon** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="ac9b4-137">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777802.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-137">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777802.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="ac9b4-138">Na **konfigurace adresy URL aplikace** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ac9b4-138">On the **Configure App URL** page, perform the following steps:</span></span>
   
    <span data-ttu-id="ac9b4-139">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-replicon-tutorial/IC777803.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-139">![Configure app URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "Configure app URL")</span></span>
  1. <span data-ttu-id="ac9b4-140">V **Replicon přihlašovací adresa URL** textovému poli, zadejte adresu URL Replicon klienta (například: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span><span class="sxs-lookup"><span data-stu-id="ac9b4-140">In the **Replicon Sign On URL** textbox, type your Replicon tenant URL (e.g.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span></span>
  2. <span data-ttu-id="ac9b4-141">V **adresa URL odpovědi Replicon** textovému poli, zadejte vaše Replicon **AssertionConsumerService** adresa URL (např: *https://global.replicon.com/! / typu saml2 nebo společnosti nebo jednotného přihlašování nebo pozálohovacího*).</span><span class="sxs-lookup"><span data-stu-id="ac9b4-141">In the **Replicon Reply URL** textbox, type your Replicon **AssertionConsumerService** URL(e.g.: *https://global.replicon.com/!/saml2/company/sso/post*).</span></span>  
      
     >[!NOTE]
     ><span data-ttu-id="ac9b4-142">Můžete získat metadata Replicon na adresu URL: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-142">You can get the URL from the Replicon metadata at: **https://global.replicon.com/!/saml2/\<YourCompanyKey\>**.</span></span>
     > 
     > 
 
  3. <span data-ttu-id="ac9b4-143">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-143">Click **Next**.</span></span>

4. <span data-ttu-id="ac9b4-144">Na **nakonfigurovat jednotné přihlašování v Replicon** stránku a stáhnout metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte metadata ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-144">On the **Configure single sign-on at Replicon** page, to download your metadata, click **Download metadata**, and then save the metadata on your computer.</span></span>
   
    <span data-ttu-id="ac9b4-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777804.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-145">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777804.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="ac9b4-146">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Replicon.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-146">In a different web browser window, log into your Replicon company site as an administrator.</span></span>

6. <span data-ttu-id="ac9b4-147">Pokud chcete konfigurovat SAML 2.0, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ac9b4-147">To configure SAML 2.0, perform the following steps:</span></span>
   
    <span data-ttu-id="ac9b4-148">![Povolit ověřování SAML](./media/active-directory-saas-replicon-tutorial/IC777805.png "ověřování povolit SAML")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-148">![Enable SAML authentication](./media/active-directory-saas-replicon-tutorial/IC777805.png "Enable SAML authentication")</span></span>
  
  1. <span data-ttu-id="ac9b4-149">K zobrazení **EnableSAML Authentication2** dialogové okno, připojte na adresu URL, následující po vašeho klíče společnosti: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="ac9b4-149">To display the **EnableSAML Authentication2** dialog, append the following to your URL, after your company key: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>  
    * <span data-ttu-id="ac9b4-150">Následující obrázek znázorňuje schéma úplnou adresu URL:</span><span class="sxs-lookup"><span data-stu-id="ac9b4-150">The following shows the schema of the complete URL:</span></span>  
   <span data-ttu-id="ac9b4-151">**https://Na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="ac9b4-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>
   2. <span data-ttu-id="ac9b4-152">Klikněte na tlačítko  **+**  rozšířit **v20Configuration** části.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-152">Click the **+** to expand the **v20Configuration** section.</span></span>
   3. <span data-ttu-id="ac9b4-153">Klikněte na tlačítko  **+**  rozšířit **metaDataConfiguration** části.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-153">Click the **+** to expand the **metaDataConfiguration** section.</span></span>
   4. <span data-ttu-id="ac9b4-154">Klikněte na tlačítko **zvolit soubor**, a vyberte soubor XML identity zprostředkovatele metadat, klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-154">Click **Choose File**, to select your identity provider metadata XML file, and click **Submit**.</span></span>

7. <span data-ttu-id="ac9b4-155">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-155">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="ac9b4-156">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC778418.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-156">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC778418.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="ac9b4-157">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="ac9b4-157">Configure user provisioning</span></span>

<span data-ttu-id="ac9b4-158">Pokud chcete povolit uživatelům Azure AD přihlášení do Replicon, musí být zřízená do Replicon.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-158">In order to enable Azure AD users to log into Replicon, they must be provisioned into Replicon.</span></span>  

<span data-ttu-id="ac9b4-159">V případě Replicon zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-159">In the case of Replicon, provisioning is a manual task.</span></span>

<span data-ttu-id="ac9b4-160">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ac9b4-160">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="ac9b4-161">V okně webového prohlížeče Přihlaste se jako správce k serveru vaší společnosti Replicon.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-161">In a web browser window, log into your Replicon company site as an administrator.</span></span>
2. <span data-ttu-id="ac9b4-162">Přejděte na **správy \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-162">Go to **Administration \> Users**.</span></span>
   
    <span data-ttu-id="ac9b4-163">![Uživatelé](./media/active-directory-saas-replicon-tutorial/IC777806.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-163">![Users](./media/active-directory-saas-replicon-tutorial/IC777806.png "Users")</span></span>
3. <span data-ttu-id="ac9b4-164">Klikněte na tlačítko **+ přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-164">Click **+Add User**.</span></span>
   
    <span data-ttu-id="ac9b4-165">![Přidat uživatele](./media/active-directory-saas-replicon-tutorial/IC777807.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-165">![Add User](./media/active-directory-saas-replicon-tutorial/IC777807.png "Add User")</span></span>
4. <span data-ttu-id="ac9b4-166">V **profil uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ac9b4-166">In the **User Profile** section, perform the following steps:</span></span>
   
    <span data-ttu-id="ac9b4-167">![Profil uživatele](./media/active-directory-saas-replicon-tutorial/IC777808.png "profil uživatele")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-167">![User profile](./media/active-directory-saas-replicon-tutorial/IC777808.png "User profile")</span></span>
   
  1. <span data-ttu-id="ac9b4-168">V **přihlašovací jméno** textovému poli, e-mailovou adresu uživatele Azure AD, které chcete zřídit typu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-168">In the **Login Name** textbox, type the Azure AD email address of the Azure AD user you want to provision.</span></span>
  2. <span data-ttu-id="ac9b4-169">Jako **typ ověřování**, vyberte **jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-169">As **Authentication Type**, select **SSO**.</span></span>
  3. <span data-ttu-id="ac9b4-170">V **oddělení** textovému poli, zadejte uživatele oddělení.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-170">In the **Department** textbox, type the user’s department.</span></span>
  4. <span data-ttu-id="ac9b4-171">Jako **typ zaměstnance**, vyberte **správce**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-171">As **Employee Type**, select **Administrator**.</span></span>
  5. <span data-ttu-id="ac9b4-172">Klikněte na tlačítko **uložit profil uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-172">Click **Save User Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="ac9b4-173">Můžete použít všechny ostatní Replicon uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Replicon zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-173">You can use any other Replicon user account creation tools or APIs provided by Replicon to provision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="ac9b4-174">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="ac9b4-174">Assign users</span></span>
<span data-ttu-id="ac9b4-175">Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-175">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="ac9b4-176">**Přiřazení uživatelů k Replicon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ac9b4-176">**To assign users to Replicon, perform the following steps:**</span></span>

1. <span data-ttu-id="ac9b4-177">Na portálu Azure classic vytvořte zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-177">In the Azure classic portal, create a test account.</span></span>

2. <span data-ttu-id="ac9b4-178">Na **Replicon** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-178">On the **Replicon** application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="ac9b4-179">![Přiřazení uživatelů](./media/active-directory-saas-replicon-tutorial/IC777809.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-179">![Assign users](./media/active-directory-saas-replicon-tutorial/IC777809.png "Assign users")</span></span>

3. <span data-ttu-id="ac9b4-180">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-180">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
    <span data-ttu-id="ac9b4-181">![Ano](./media/active-directory-saas-replicon-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="ac9b4-181">![Yes](./media/active-directory-saas-replicon-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="ac9b4-182">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="ac9b4-182">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="ac9b4-183">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ac9b4-183">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

