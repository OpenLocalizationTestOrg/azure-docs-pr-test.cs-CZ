---
title: 'Kurz: Azure Active Directory integrace s Cisco Webex | Microsoft Docs'
description: "Další informace o použití Cisco Webex s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: b44b1a5b3e988a51db3325ec8a181651fa84e768
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a><span data-ttu-id="1838f-103">Kurz: Azure Active Directory integrace s Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="1838f-103">Tutorial: Azure Active Directory Integration with Cisco Webex</span></span>
<span data-ttu-id="1838f-104">Cílem tohoto kurzu je zobrazit integraci Azure a Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="1838f-104">The objective of this tutorial is to show the integration of Azure and Cisco Webex.</span></span>  
<span data-ttu-id="1838f-105">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="1838f-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="1838f-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="1838f-106">A valid Azure subscription</span></span>
* <span data-ttu-id="1838f-107">Klient Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="1838f-107">A Cisco Webex tenant</span></span>

<span data-ttu-id="1838f-108">Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace na serveru Cisco Webex vaší společnosti (služba Zprostředkovatel iniciované přihlašování) nebo pomocí uživatele Azure AD, které jste přiřadili Cisco Webex [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1838f-108">After completing this tutorial, the Azure AD users you have assigned to Cisco Webex will be able to single sign into the application at your Cisco Webex company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="1838f-109">Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="1838f-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

* <span data-ttu-id="1838f-110">Povolení integrace aplikace pro Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="1838f-110">Enabling the application integration for Cisco Webex</span></span>
* <span data-ttu-id="1838f-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="1838f-111">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="1838f-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="1838f-112">Configuring user provisioning</span></span>
* <span data-ttu-id="1838f-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="1838f-113">Assigning users</span></span>

<span data-ttu-id="1838f-114">![Scénář](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="1838f-114">![Scenario](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-cisco-webex"></a><span data-ttu-id="1838f-115">Povolit integraci aplikace pro Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="1838f-115">Enable the application integration for Cisco Webex</span></span>
<span data-ttu-id="1838f-116">Cílem této části se popisují postup povolení integrace aplikace pro Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="1838f-116">The objective of this section is to outline how to enable the application integration for Cisco Webex.</span></span>

<span data-ttu-id="1838f-117">**Pokud chcete povolit integraci aplikací pro Cisco Webex, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1838f-117">**To enable the application integration for Cisco Webex, perform the following steps:**</span></span>

1. <span data-ttu-id="1838f-118">V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1838f-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="1838f-119">![Služby Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="1838f-119">![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="1838f-120">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="1838f-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="1838f-121">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="1838f-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="1838f-122">![Aplikace](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="1838f-122">![Applications](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="1838f-123">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="1838f-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="1838f-124">![Přidat aplikaci](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="1838f-124">![Add application](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="1838f-125">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="1838f-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="1838f-126">![Přidání aplikace z gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="1838f-126">![Add an application from gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="1838f-127">V **vyhledávacího pole**, typ **Cisco Webex**.</span><span class="sxs-lookup"><span data-stu-id="1838f-127">In the **search box**, type **Cisco Webex**.</span></span>
   
   <span data-ttu-id="1838f-128">![Galerie aplikací](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="1838f-128">![Application Gallery](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Application Gallery")</span></span>
7. <span data-ttu-id="1838f-129">V podokně výsledků vyberte **Cisco Webex**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="1838f-129">In the results pane, select **Cisco Webex**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="1838f-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span><span class="sxs-lookup"><span data-stu-id="1838f-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="1838f-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1838f-131">Configure single sign-on</span></span>

<span data-ttu-id="1838f-132">Cílem této části se popisují, jak uživatelům povolit ověřování na Cisco Webex ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="1838f-132">The objective of this section is to outline how to enable users to authenticate to Cisco Webex with their account in Azure AD using federation based on the SAML protocol.</span></span>  

<span data-ttu-id="1838f-133">V rámci tohoto postupu je nutné vytvořit certifikát kódováním base-64.</span><span class="sxs-lookup"><span data-stu-id="1838f-133">As part of this procedure, you are required to create a base-64 encoded certificate.</span></span> <span data-ttu-id="1838f-134">Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="1838f-134">If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="1838f-135">**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1838f-135">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="1838f-136">Na portálu Azure classic na **Cisco Webex** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1838f-136">In the Azure classic portal, on the **Cisco Webex** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="1838f-137">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="1838f-137">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="1838f-138">Na **jak chcete uživatelům se přihlásit Cisco Webex** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="1838f-138">On the **How would you like users to sign on to Cisco Webex** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="1838f-139">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="1838f-139">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="1838f-140">Na **konfigurace adresy URL aplikace** stránky, proveďte následující kroky a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1838f-140">On the **Configure App URL** page, perform the following steps, and then click **Next**.</span></span>
   
   <span data-ttu-id="1838f-141">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="1838f-141">![Configure app URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Configure app URL")</span></span>   
   1. <span data-ttu-id="1838f-142">V **Sing – adresa URL** textovému poli, zadejte adresu URL klienta Cisco Webex (např: *http://contoso.webex.com*).</span><span class="sxs-lookup"><span data-stu-id="1838f-142">In the **Sing On URL** textbox, type your Cisco Webex tenant URL (e.g.: *http://contoso.webex.com*).</span></span>
   2. <span data-ttu-id="1838f-143">V **adresa URL odpovědi Webex Cisco** textovému poli, typ vaše **Cisco Webex AssertionConsumerService URL** (např: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).</span><span class="sxs-lookup"><span data-stu-id="1838f-143">In the **Cisco Webex Reply URL** textbox, type your **Cisco Webex AssertionConsumerService URL** (e.g.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).</span></span>
4. <span data-ttu-id="1838f-144">Na **nakonfigurovat jednotné přihlašování v Cisco Webex** stránky, stáhněte si certifikát, klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="1838f-144">On the **Configure single sign-on at Cisco Webex** page, to download your certificate, click **Download certificate**, and then save the certificate file on your computer.</span></span>
   
   <span data-ttu-id="1838f-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="1838f-145">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="1838f-146">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="1838f-146">In a different web browser window, log into your Cisco Webex company site as an administrator.</span></span>
6. <span data-ttu-id="1838f-147">V nabídce v horní části, klikněte na tlačítko **Správa webu**.</span><span class="sxs-lookup"><span data-stu-id="1838f-147">In the menu on the top, click **Site Administration**.</span></span>
   
   <span data-ttu-id="1838f-148">![Správa lokalit](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "lokality správy")</span><span class="sxs-lookup"><span data-stu-id="1838f-148">![Site Administration](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Site Administration")</span></span>
7. <span data-ttu-id="1838f-149">V **spravovat lokality** klikněte na tlačítko **Konfigurace jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="1838f-149">In the **Manage Site** section, click **SSO Configuration**.</span></span>
   
   <span data-ttu-id="1838f-150">![Konfigurace jednotného přihlašování k](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "Konfigurace jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="1838f-150">![SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO Configuration")</span></span>
8. <span data-ttu-id="1838f-151">V federovaného jednotného přihlašování konfiguraci webové části proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1838f-151">In the Federated Web SSO Configuration section, perform the following steps:</span></span>
   
   <span data-ttu-id="1838f-152">![Konfigurace jednotného přihlašování federovaného](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "federovaného jednotného přihlašování k konfigurace")</span><span class="sxs-lookup"><span data-stu-id="1838f-152">![Federated SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federated SSO Configuration")</span></span>  
   1. <span data-ttu-id="1838f-153">Z **Federation protokol** seznamu, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="1838f-153">From the **Federation Protocol** list, select **SAML 2.0**.</span></span>
   2. <span data-ttu-id="1838f-154">Vytvoření **kódování Base-64** soubor stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="1838f-154">Create a **Base-64 encoded** file from your downloaded certificate.</span></span>  
    >[!TIP]
    ><span data-ttu-id="1838f-155">Další podrobnosti najdete v tématu [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="1838f-155">For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
    >  
   3. <span data-ttu-id="1838f-156">Otevření kódovaného certifikátu kódování base-64 v programu Poznámkový blok a zkopírujte obsah ho.</span><span class="sxs-lookup"><span data-stu-id="1838f-156">Open your base-64 encoded certificate in notepad, and then copy the content of it.</span></span>
   4. <span data-ttu-id="1838f-157">Klikněte na tlačítko **importovat Metadata SAML**a potom vložte vaše kódování base-64 kódovaného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="1838f-157">Click **Import SAML Metadata**, and then paste your base-64 encoded certificate.</span></span>
   5. <span data-ttu-id="1838f-158">Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie **URL vystavitele** hodnotu a vložte ji do **vystavitele pro SAML (IdP ID)** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1838f-158">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, copy the **Issuer URL** value, and then paste it into the **Issuer for SAML (IdP ID)** textbox.</span></span>
   6. <span data-ttu-id="1838f-159">Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie **vzdálené adresy URL pro přihlášení** hodnotu a vložte ji do **zákazníka jednotného přihlašování služby přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1838f-159">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, copy the **Remote Login URL** value, and then paste it into the **Customer SSO Service Login URL** textbox.</span></span>
   7. <span data-ttu-id="1838f-160">Z **NameID formátu** seznamu, vyberte **e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="1838f-160">From the **NameID Format** list, select **Email address**.</span></span>
   8. <span data-ttu-id="1838f-161">V **AuthnContextClassRef** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="1838f-161">In the **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span>
   9. <span data-ttu-id="1838f-162">Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie **vzdálené adresy URL odhlašovací** hodnotu a vložte ji do **adresy URL odhlašovací zákazníka jednotného přihlašování služby** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1838f-162">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, copy the **Remote Logout URL** value, and then paste it into the **Customer SSO Service Logout URL** textbox.</span></span>
   10. <span data-ttu-id="1838f-163">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="1838f-163">Click **Update**.</span></span>
9. <span data-ttu-id="1838f-164">Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="1838f-164">In the Azure classic portal, on the **Configure single sign-on at Cisco Webex** dialog page, select the single sign-on configuration confirmation, and then click **Complete**.</span></span>
   
   <span data-ttu-id="1838f-165">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="1838f-165">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="1838f-166">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="1838f-166">Configure user provisioning</span></span>

<span data-ttu-id="1838f-167">Pokud chcete povolit uživatelům Azure AD přihlášení do Cisco Webex, musí být zřízená do Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="1838f-167">In order to enable Azure AD users to log into Cisco Webex, they must be provisioned into Cisco Webex.</span></span>  

* <span data-ttu-id="1838f-168">V případě Cisco Webex zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="1838f-168">In the case of Cisco Webex, provisioning is a manual task.</span></span>

<span data-ttu-id="1838f-169">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1838f-169">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="1838f-170">Přihlaste se k vaší **Cisco Webex** klienta.</span><span class="sxs-lookup"><span data-stu-id="1838f-170">Log in to your **Cisco Webex** tenant.</span></span>
2. <span data-ttu-id="1838f-171">Přejděte na **Správa uživatelů \> přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="1838f-171">Go to **Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="1838f-172">![Přidání uživatelů](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="1838f-172">![Add users](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Add users")</span></span>
3. <span data-ttu-id="1838f-173">V části přidat uživatele proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1838f-173">On the Add User section, perform the following steps:</span></span>
   
   <span data-ttu-id="1838f-174">![Přidat uživatele](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="1838f-174">![Add user](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Add user")</span></span>   
   1. <span data-ttu-id="1838f-175">Jako **typ účtu**, vyberte **hostitele**.</span><span class="sxs-lookup"><span data-stu-id="1838f-175">As **Account Type**, select **Host**.</span></span>
   2. <span data-ttu-id="1838f-176">Zadejte informace o stávajícího uživatele Azure AD do následujících textových polí: **křestní jméno, příjmení**, **uživatelské jméno**, **e-mailu**, **heslo**, **Potvrdit heslo**.</span><span class="sxs-lookup"><span data-stu-id="1838f-176">Type the information of an existing Azure AD user into the following textboxes: **First name, Last name**, **User name**, **Email**, **Password**, **Confirm Password**.</span></span>
   3. <span data-ttu-id="1838f-177">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="1838f-177">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="1838f-178">Můžete použít všechny ostatní Cisco Webex uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Cisco Webex zřizovat AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="1838f-178">You can use any other Cisco Webex user account creation tools or APIs provided by Cisco Webex to provision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="1838f-179">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="1838f-179">Assign users</span></span>
<span data-ttu-id="1838f-180">Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="1838f-180">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="1838f-181">**Přiřazení uživatelů k Cisco Webex, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1838f-181">**To assign users to Cisco Webex, perform the following steps:**</span></span>

1. <span data-ttu-id="1838f-182">Na portálu Azure classic vytvořte zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="1838f-182">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="1838f-183">Na **Cisco Webex** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="1838f-183">On the **Cisco Webex** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="1838f-184">![Přiřazení uživatelů](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="1838f-184">![Assign users](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Assign users")</span></span>
3. <span data-ttu-id="1838f-185">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.</span><span class="sxs-lookup"><span data-stu-id="1838f-185">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="1838f-186">![Ano](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="1838f-186">![Yes](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="1838f-187">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="1838f-187">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="1838f-188">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1838f-188">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

