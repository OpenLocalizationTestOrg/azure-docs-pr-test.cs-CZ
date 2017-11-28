---
title: 'Kurz: Azure Active Directory integrace s Replicon | Microsoft Docs'
description: "Zjistěte, jak toouse Replicon s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
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
ms.openlocfilehash: 4949eaf959265cfa4f732a2b73317fffe6312a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a><span data-ttu-id="03fba-103">Kurz: Azure Active Directory integrace s Replicon</span><span class="sxs-lookup"><span data-stu-id="03fba-103">Tutorial: Azure Active Directory integration with Replicon</span></span>
<span data-ttu-id="03fba-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a Replicon.</span><span class="sxs-lookup"><span data-stu-id="03fba-104">hello objective of this tutorial is tooshow hello integration of Azure and Replicon.</span></span> <span data-ttu-id="03fba-105">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="03fba-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="03fba-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="03fba-106">A valid Azure subscription</span></span>
* <span data-ttu-id="03fba-107">Replicon klienta</span><span class="sxs-lookup"><span data-stu-id="03fba-107">A Replicon tenant</span></span>

<span data-ttu-id="03fba-108">Po dokončení tohoto kurzu, budou uživatelé hello Azure AD jste přiřadili tooReplicon možné toosingle přihlášení do aplikace hello na váš web společnosti Replicon (služba Zprostředkovatel iniciované přihlašování) nebo pomocí hello [Úvod toohello přístup Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="03fba-108">After completing this tutorial, hello Azure AD users you have assigned tooReplicon will be able toosingle sign into hello application at your Replicon company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="03fba-109">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="03fba-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="03fba-110">Povolení integrace aplikace hello pro Replicon</span><span class="sxs-lookup"><span data-stu-id="03fba-110">Enabling hello application integration for Replicon</span></span>
2. <span data-ttu-id="03fba-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="03fba-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="03fba-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="03fba-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="03fba-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="03fba-113">Assigning users</span></span>

<span data-ttu-id="03fba-114">![Scénář](./media/active-directory-saas-replicon-tutorial/IC777798.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="03fba-114">![Scenario](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-replicon"></a><span data-ttu-id="03fba-115">Povolit integraci aplikace hello pro Replicon</span><span class="sxs-lookup"><span data-stu-id="03fba-115">Enable hello application integration for Replicon</span></span>
<span data-ttu-id="03fba-116">Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Replicon.</span><span class="sxs-lookup"><span data-stu-id="03fba-116">hello objective of this section is toooutline how tooenable hello application integration for Replicon.</span></span>

<span data-ttu-id="03fba-117">**Integrace aplikace hello tooenable pro Replicon, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="03fba-117">**tooenable hello application integration for Replicon, perform hello following steps:**</span></span>

1. <span data-ttu-id="03fba-118">V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="03fba-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="03fba-119">![Služby Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="03fba-119">![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="03fba-120">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="03fba-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="03fba-121">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="03fba-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="03fba-122">![Aplikace](./media/active-directory-saas-replicon-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="03fba-122">![Applications](./media/active-directory-saas-replicon-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="03fba-123">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="03fba-123">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="03fba-124">![Přidat aplikaci](./media/active-directory-saas-replicon-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="03fba-124">![Add application](./media/active-directory-saas-replicon-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="03fba-125">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="03fba-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="03fba-126">![Přidání aplikace z gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="03fba-126">![Add an application from gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="03fba-127">V hello **vyhledávacího pole**, typ **Replicon**.</span><span class="sxs-lookup"><span data-stu-id="03fba-127">In hello **search box**, type **Replicon**.</span></span>
   
    <span data-ttu-id="03fba-128">![Galerie aplikací](./media/active-directory-saas-replicon-tutorial/IC777799.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="03fba-128">![Application gallery](./media/active-directory-saas-replicon-tutorial/IC777799.png "Application gallery")</span></span>
7. <span data-ttu-id="03fba-129">V podokně výsledků hello, vyberte **Replicon**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="03fba-129">In hello results pane, select **Replicon**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="03fba-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span><span class="sxs-lookup"><span data-stu-id="03fba-130">![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="03fba-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="03fba-131">Configure single sign-on</span></span>

<span data-ttu-id="03fba-132">Hello cílem této části je toooutline jak tooReplicon tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="03fba-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooReplicon with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="03fba-133">**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="03fba-133">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="03fba-134">V portálu Azure classic, na hello hello **Replicon** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="03fba-134">In hello Azure classic portal, on hello **Replicon** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="03fba-135">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777801.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="03fba-135">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777801.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="03fba-136">Na hello **jak jste by například uživatelé toosign na tooReplicon** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="03fba-136">On hello **How would you like users toosign on tooReplicon** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="03fba-137">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777802.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="03fba-137">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777802.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="03fba-138">Na hello **konfigurace adresy URL aplikace** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="03fba-138">On hello **Configure App URL** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="03fba-139">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-replicon-tutorial/IC777803.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="03fba-139">![Configure app URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "Configure app URL")</span></span>
  1. <span data-ttu-id="03fba-140">V hello **Replicon přihlašovací adresa URL** textovému poli, zadejte adresu URL Replicon klienta (například: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span><span class="sxs-lookup"><span data-stu-id="03fba-140">In hello **Replicon Sign On URL** textbox, type your Replicon tenant URL (e.g.: *https://na2.replicon.com/company/saml2/sp-sso/post*).</span></span>
  2. <span data-ttu-id="03fba-141">V hello **adresa URL odpovědi Replicon** textovému poli, zadejte vaše Replicon **AssertionConsumerService** adresa URL (např: *https://global.replicon.com/! / typu saml2 nebo společnosti nebo jednotného přihlašování nebo pozálohovacího*).</span><span class="sxs-lookup"><span data-stu-id="03fba-141">In hello **Replicon Reply URL** textbox, type your Replicon **AssertionConsumerService** URL(e.g.: *https://global.replicon.com/!/saml2/company/sso/post*).</span></span>  
      
     >[!NOTE]
     ><span data-ttu-id="03fba-142">Můžete získat adresu URL hello hello Replicon metadata v: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.</span><span class="sxs-lookup"><span data-stu-id="03fba-142">You can get hello URL from hello Replicon metadata at: **https://global.replicon.com/!/saml2/\<YourCompanyKey\>**.</span></span>
     > 
     > 
 
  3. <span data-ttu-id="03fba-143">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="03fba-143">Click **Next**.</span></span>

4. <span data-ttu-id="03fba-144">Na hello **nakonfigurovat jednotné přihlašování v Replicon** stránky, toodownload metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte hello metadata ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="03fba-144">On hello **Configure single sign-on at Replicon** page, toodownload your metadata, click **Download metadata**, and then save hello metadata on your computer.</span></span>
   
    <span data-ttu-id="03fba-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777804.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="03fba-145">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC777804.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="03fba-146">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Replicon.</span><span class="sxs-lookup"><span data-stu-id="03fba-146">In a different web browser window, log into your Replicon company site as an administrator.</span></span>

6. <span data-ttu-id="03fba-147">tooconfigure SAML 2.0, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="03fba-147">tooconfigure SAML 2.0, perform hello following steps:</span></span>
   
    <span data-ttu-id="03fba-148">![Povolit ověřování SAML](./media/active-directory-saas-replicon-tutorial/IC777805.png "ověřování povolit SAML")</span><span class="sxs-lookup"><span data-stu-id="03fba-148">![Enable SAML authentication](./media/active-directory-saas-replicon-tutorial/IC777805.png "Enable SAML authentication")</span></span>
  
  1. <span data-ttu-id="03fba-149">toodisplay hello **EnableSAML Authentication2** dialogu připojit hello následující adresu URL tooyour, po vašeho klíče společnosti: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="03fba-149">toodisplay hello **EnableSAML Authentication2** dialog, append hello following tooyour URL, after your company key: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>  
    * <span data-ttu-id="03fba-150">Následující Hello znázorňuje schéma hello hello úplnou adresu URL:</span><span class="sxs-lookup"><span data-stu-id="03fba-150">hello following shows hello schema of hello complete URL:</span></span>  
   <span data-ttu-id="03fba-151">**https://Na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span><span class="sxs-lookup"><span data-stu-id="03fba-151">**https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**</span></span>
   2. <span data-ttu-id="03fba-152">Klikněte na tlačítko hello  **+**  tooexpand hello **v20Configuration** části.</span><span class="sxs-lookup"><span data-stu-id="03fba-152">Click hello **+** tooexpand hello **v20Configuration** section.</span></span>
   3. <span data-ttu-id="03fba-153">Klikněte na tlačítko hello  **+**  tooexpand hello **metaDataConfiguration** části.</span><span class="sxs-lookup"><span data-stu-id="03fba-153">Click hello **+** tooexpand hello **metaDataConfiguration** section.</span></span>
   4. <span data-ttu-id="03fba-154">Klikněte na tlačítko **zvolit soubor**, tooselect vaší identity zprostředkovatele metadat XML soubor a klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="03fba-154">Click **Choose File**, tooselect your identity provider metadata XML file, and click **Submit**.</span></span>

7. <span data-ttu-id="03fba-155">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="03fba-155">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="03fba-156">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC778418.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="03fba-156">![Configure single sign-on](./media/active-directory-saas-replicon-tutorial/IC778418.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="03fba-157">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="03fba-157">Configure user provisioning</span></span>

<span data-ttu-id="03fba-158">V pořadí tooenable Azure AD Uživatelé toolog do Replicon musí být zřízená do Replicon.</span><span class="sxs-lookup"><span data-stu-id="03fba-158">In order tooenable Azure AD users toolog into Replicon, they must be provisioned into Replicon.</span></span>  

<span data-ttu-id="03fba-159">V případě hello Replicon zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="03fba-159">In hello case of Replicon, provisioning is a manual task.</span></span>

<span data-ttu-id="03fba-160">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="03fba-160">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="03fba-161">V okně webového prohlížeče Přihlaste se jako správce k serveru vaší společnosti Replicon.</span><span class="sxs-lookup"><span data-stu-id="03fba-161">In a web browser window, log into your Replicon company site as an administrator.</span></span>
2. <span data-ttu-id="03fba-162">Přejděte příliš**správy \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="03fba-162">Go too**Administration \> Users**.</span></span>
   
    <span data-ttu-id="03fba-163">![Uživatelé](./media/active-directory-saas-replicon-tutorial/IC777806.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="03fba-163">![Users](./media/active-directory-saas-replicon-tutorial/IC777806.png "Users")</span></span>
3. <span data-ttu-id="03fba-164">Klikněte na tlačítko **+ přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="03fba-164">Click **+Add User**.</span></span>
   
    <span data-ttu-id="03fba-165">![Přidat uživatele](./media/active-directory-saas-replicon-tutorial/IC777807.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="03fba-165">![Add User](./media/active-directory-saas-replicon-tutorial/IC777807.png "Add User")</span></span>
4. <span data-ttu-id="03fba-166">V hello **profil uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="03fba-166">In hello **User Profile** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="03fba-167">![Profil uživatele](./media/active-directory-saas-replicon-tutorial/IC777808.png "profil uživatele")</span><span class="sxs-lookup"><span data-stu-id="03fba-167">![User profile](./media/active-directory-saas-replicon-tutorial/IC777808.png "User profile")</span></span>
   
  1. <span data-ttu-id="03fba-168">V hello **přihlašovací jméno** textovému poli, typ hello Azure AD e-mailová adresa uživatele hello Azure AD, bude tooprovision.</span><span class="sxs-lookup"><span data-stu-id="03fba-168">In hello **Login Name** textbox, type hello Azure AD email address of hello Azure AD user you want tooprovision.</span></span>
  2. <span data-ttu-id="03fba-169">Jako **typ ověřování**, vyberte **jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="03fba-169">As **Authentication Type**, select **SSO**.</span></span>
  3. <span data-ttu-id="03fba-170">V hello **oddělení** textovému poli, zadejte oddělení hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="03fba-170">In hello **Department** textbox, type hello user’s department.</span></span>
  4. <span data-ttu-id="03fba-171">Jako **typ zaměstnance**, vyberte **správce**.</span><span class="sxs-lookup"><span data-stu-id="03fba-171">As **Employee Type**, select **Administrator**.</span></span>
  5. <span data-ttu-id="03fba-172">Klikněte na tlačítko **uložit profil uživatele**.</span><span class="sxs-lookup"><span data-stu-id="03fba-172">Click **Save User Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="03fba-173">Můžete použít všechny ostatní Replicon uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Replicon tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="03fba-173">You can use any other Replicon user account creation tools or APIs provided by Replicon tooprovision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="03fba-174">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="03fba-174">Assign users</span></span>
<span data-ttu-id="03fba-175">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="03fba-175">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="03fba-176">**tooassign tooReplicon uživatele, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="03fba-176">**tooassign users tooReplicon, perform hello following steps:**</span></span>

1. <span data-ttu-id="03fba-177">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="03fba-177">In hello Azure classic portal, create a test account.</span></span>

2. <span data-ttu-id="03fba-178">Na hello **Replicon** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="03fba-178">On hello **Replicon** application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="03fba-179">![Přiřazení uživatelů](./media/active-directory-saas-replicon-tutorial/IC777809.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="03fba-179">![Assign users](./media/active-directory-saas-replicon-tutorial/IC777809.png "Assign users")</span></span>

3. <span data-ttu-id="03fba-180">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="03fba-180">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
    <span data-ttu-id="03fba-181">![Ano](./media/active-directory-saas-replicon-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="03fba-181">![Yes](./media/active-directory-saas-replicon-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="03fba-182">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="03fba-182">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="03fba-183">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="03fba-183">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

