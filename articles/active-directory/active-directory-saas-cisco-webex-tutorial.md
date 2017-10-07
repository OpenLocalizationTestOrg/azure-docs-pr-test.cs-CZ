---
title: 'Kurz: Azure Active Directory integrace s Cisco Webex | Microsoft Docs'
description: "Zjistěte, jak toouse Cisco Webex s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
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
ms.openlocfilehash: 9fc11e58a7acaa6fbfb32eeccbfbf85984950e67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a><span data-ttu-id="93ea7-103">Kurz: Azure Active Directory integrace s Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="93ea7-103">Tutorial: Azure Active Directory Integration with Cisco Webex</span></span>
<span data-ttu-id="93ea7-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="93ea7-104">hello objective of this tutorial is tooshow hello integration of Azure and Cisco Webex.</span></span>  
<span data-ttu-id="93ea7-105">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="93ea7-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="93ea7-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="93ea7-106">A valid Azure subscription</span></span>
* <span data-ttu-id="93ea7-107">Klient Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="93ea7-107">A Cisco Webex tenant</span></span>

<span data-ttu-id="93ea7-108">Po dokončení tohoto kurzu, hello uživatele Azure AD, které jste přiřadili tooCisco Webex bude možné toosingle přihlášení do aplikace hello ve vaší lokalitě společnosti Cisco Webex (služba Zprostředkovatel iniciované přihlašování) nebo pomocí hello [toohello Úvod Přístup k panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="93ea7-108">After completing this tutorial, hello Azure AD users you have assigned tooCisco Webex will be able toosingle sign into hello application at your Cisco Webex company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="93ea7-109">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="93ea7-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

* <span data-ttu-id="93ea7-110">Povolení integrace aplikace hello pro Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="93ea7-110">Enabling hello application integration for Cisco Webex</span></span>
* <span data-ttu-id="93ea7-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="93ea7-111">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="93ea7-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="93ea7-112">Configuring user provisioning</span></span>
* <span data-ttu-id="93ea7-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="93ea7-113">Assigning users</span></span>

<span data-ttu-id="93ea7-114">![Scénář](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="93ea7-114">![Scenario](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-cisco-webex"></a><span data-ttu-id="93ea7-115">Povolit integraci aplikace hello pro Cisco Webex</span><span class="sxs-lookup"><span data-stu-id="93ea7-115">Enable hello application integration for Cisco Webex</span></span>
<span data-ttu-id="93ea7-116">Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="93ea7-116">hello objective of this section is toooutline how tooenable hello application integration for Cisco Webex.</span></span>

<span data-ttu-id="93ea7-117">**Integrace aplikace hello tooenable pro Cisco Webex proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="93ea7-117">**tooenable hello application integration for Cisco Webex, perform hello following steps:**</span></span>

1. <span data-ttu-id="93ea7-118">V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="93ea7-119">![Služby Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="93ea7-119">![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="93ea7-120">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="93ea7-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="93ea7-121">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="93ea7-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="93ea7-122">![Aplikace](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="93ea7-122">![Applications](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="93ea7-123">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="93ea7-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="93ea7-124">![Přidat aplikaci](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="93ea7-124">![Add application](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="93ea7-125">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="93ea7-126">![Přidání aplikace z gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="93ea7-126">![Add an application from gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="93ea7-127">V hello **vyhledávacího pole**, typ **Cisco Webex**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-127">In hello **search box**, type **Cisco Webex**.</span></span>
   
   <span data-ttu-id="93ea7-128">![Galerie aplikací](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="93ea7-128">![Application Gallery](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Application Gallery")</span></span>
7. <span data-ttu-id="93ea7-129">V podokně výsledků hello, vyberte **Cisco Webex**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="93ea7-129">In hello results pane, select **Cisco Webex**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="93ea7-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span><span class="sxs-lookup"><span data-stu-id="93ea7-130">![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="93ea7-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="93ea7-131">Configure single sign-on</span></span>

<span data-ttu-id="93ea7-132">Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooCisco Webex ke svému účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="93ea7-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooCisco Webex with their account in Azure AD using federation based on hello SAML protocol.</span></span>  

<span data-ttu-id="93ea7-133">V rámci tohoto postupu jsou požadované toocreate kódování base-64 kódovaného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="93ea7-133">As part of this procedure, you are required toocreate a base-64 encoded certificate.</span></span> <span data-ttu-id="93ea7-134">Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="93ea7-134">If you are not familiar with this procedure, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="93ea7-135">**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="93ea7-135">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="93ea7-136">V portálu Azure classic, na hello hello **Cisco Webex** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="93ea7-136">In hello Azure classic portal, on hello **Cisco Webex** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="93ea7-137">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="93ea7-137">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="93ea7-138">Na hello **jak jste by například uživatelé toosign na tooCisco Webex** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-138">On hello **How would you like users toosign on tooCisco Webex** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="93ea7-139">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="93ea7-139">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="93ea7-140">Na hello **konfigurace adresy URL aplikace** stránky, proveďte hello následující kroky a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-140">On hello **Configure App URL** page, perform hello following steps, and then click **Next**.</span></span>
   
   <span data-ttu-id="93ea7-141">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="93ea7-141">![Configure app URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Configure app URL")</span></span>   
   1. <span data-ttu-id="93ea7-142">V hello **Sing – adresa URL** textovému poli, zadejte adresu URL klienta Cisco Webex (např: *http://contoso.webex.com*).</span><span class="sxs-lookup"><span data-stu-id="93ea7-142">In hello **Sing On URL** textbox, type your Cisco Webex tenant URL (e.g.: *http://contoso.webex.com*).</span></span>
   2. <span data-ttu-id="93ea7-143">V hello **adresa URL odpovědi Webex Cisco** textovému poli, typ vaše **Cisco Webex AssertionConsumerService URL** (např: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company* ).</span><span class="sxs-lookup"><span data-stu-id="93ea7-143">In hello **Cisco Webex Reply URL** textbox, type your **Cisco Webex AssertionConsumerService URL** (e.g.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).</span></span>
4. <span data-ttu-id="93ea7-144">Na hello **nakonfigurovat jednotné přihlašování v Cisco Webex** stránky, toodownload svůj certifikát, klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="93ea7-144">On hello **Configure single sign-on at Cisco Webex** page, toodownload your certificate, click **Download certificate**, and then save hello certificate file on your computer.</span></span>
   
   <span data-ttu-id="93ea7-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="93ea7-145">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="93ea7-146">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="93ea7-146">In a different web browser window, log into your Cisco Webex company site as an administrator.</span></span>
6. <span data-ttu-id="93ea7-147">V nabídce hello hello nahoře, klikněte na tlačítko **Správa webu**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-147">In hello menu on hello top, click **Site Administration**.</span></span>
   
   <span data-ttu-id="93ea7-148">![Správa lokalit](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "lokality správy")</span><span class="sxs-lookup"><span data-stu-id="93ea7-148">![Site Administration](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Site Administration")</span></span>
7. <span data-ttu-id="93ea7-149">V hello **spravovat lokality** klikněte na tlačítko **Konfigurace jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-149">In hello **Manage Site** section, click **SSO Configuration**.</span></span>
   
   <span data-ttu-id="93ea7-150">![Konfigurace jednotného přihlašování k](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "Konfigurace jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="93ea7-150">![SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO Configuration")</span></span>
8. <span data-ttu-id="93ea7-151">V hello federovaného jednotného přihlašování konfiguraci webové části proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="93ea7-151">In hello Federated Web SSO Configuration section, perform hello following steps:</span></span>
   
   <span data-ttu-id="93ea7-152">![Konfigurace jednotného přihlašování federovaného](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "federovaného jednotného přihlašování k konfigurace")</span><span class="sxs-lookup"><span data-stu-id="93ea7-152">![Federated SSO Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federated SSO Configuration")</span></span>  
   1. <span data-ttu-id="93ea7-153">Z hello **Federation protokol** seznamu, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-153">From hello **Federation Protocol** list, select **SAML 2.0**.</span></span>
   2. <span data-ttu-id="93ea7-154">Vytvoření **kódování Base-64** soubor stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="93ea7-154">Create a **Base-64 encoded** file from your downloaded certificate.</span></span>  
    >[!TIP]
    ><span data-ttu-id="93ea7-155">Další podrobnosti najdete v tématu [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="93ea7-155">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
    >  
   3. <span data-ttu-id="93ea7-156">V programu Poznámkový blok a potom hello kopírování obsahu je otevření kódovaného certifikátu kódování base-64.</span><span class="sxs-lookup"><span data-stu-id="93ea7-156">Open your base-64 encoded certificate in notepad, and then copy hello content of it.</span></span>
   4. <span data-ttu-id="93ea7-157">Klikněte na tlačítko **importovat Metadata SAML**a potom vložte vaše kódování base-64 kódovaného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="93ea7-157">Click **Import SAML Metadata**, and then paste your base-64 encoded certificate.</span></span>
   5. <span data-ttu-id="93ea7-158">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie hello **URL vystavitele** hodnotu a pak ji vložit do hello **vystavitele pro SAML (IdP ID)** textové pole.</span><span class="sxs-lookup"><span data-stu-id="93ea7-158">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, copy hello **Issuer URL** value, and then paste it into hello **Issuer for SAML (IdP ID)** textbox.</span></span>
   6. <span data-ttu-id="93ea7-159">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie hello **vzdálené adresy URL pro přihlášení** hodnotu a pak ji vložit do hello **zákazníka jednotného přihlašování služby přihlášení Adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="93ea7-159">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, copy hello **Remote Login URL** value, and then paste it into hello **Customer SSO Service Login URL** textbox.</span></span>
   7. <span data-ttu-id="93ea7-160">Z hello **NameID formátu** seznamu, vyberte **e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-160">From hello **NameID Format** list, select **Email address**.</span></span>
   8. <span data-ttu-id="93ea7-161">V hello **AuthnContextClassRef** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-161">In hello **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span>
   9. <span data-ttu-id="93ea7-162">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie hello **vzdálené adresy URL odhlašovací** hodnotu a pak ji vložit do hello **zákazníka jednotného přihlašování služby odhlášení Adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="93ea7-162">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, copy hello **Remote Logout URL** value, and then paste it into hello **Customer SSO Service Logout URL** textbox.</span></span>
   10. <span data-ttu-id="93ea7-163">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-163">Click **Update**.</span></span>
9. <span data-ttu-id="93ea7-164">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-164">In hello Azure classic portal, on hello **Configure single sign-on at Cisco Webex** dialog page, select hello single sign-on configuration confirmation, and then click **Complete**.</span></span>
   
   <span data-ttu-id="93ea7-165">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="93ea7-165">![Configure single sign-on](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Configure single sign-on")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="93ea7-166">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="93ea7-166">Configure user provisioning</span></span>

<span data-ttu-id="93ea7-167">V pořadí tooenable Azure AD Uživatelé toolog do Cisco Webex musí být zřízená do Cisco Webex.</span><span class="sxs-lookup"><span data-stu-id="93ea7-167">In order tooenable Azure AD users toolog into Cisco Webex, they must be provisioned into Cisco Webex.</span></span>  

* <span data-ttu-id="93ea7-168">V případě hello Cisco Webex zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="93ea7-168">In hello case of Cisco Webex, provisioning is a manual task.</span></span>

<span data-ttu-id="93ea7-169">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="93ea7-169">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="93ea7-170">Přihlaste se tooyour **Cisco Webex** klienta.</span><span class="sxs-lookup"><span data-stu-id="93ea7-170">Log in tooyour **Cisco Webex** tenant.</span></span>
2. <span data-ttu-id="93ea7-171">Přejděte příliš**spravovat uživatele \> přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-171">Go too**Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="93ea7-172">![Přidání uživatelů](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="93ea7-172">![Add users](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Add users")</span></span>
3. <span data-ttu-id="93ea7-173">V hello části přidat uživatele proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="93ea7-173">On hello Add User section, perform hello following steps:</span></span>
   
   <span data-ttu-id="93ea7-174">![Přidat uživatele](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="93ea7-174">![Add user](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Add user")</span></span>   
   1. <span data-ttu-id="93ea7-175">Jako **typ účtu**, vyberte **hostitele**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-175">As **Account Type**, select **Host**.</span></span>
   2. <span data-ttu-id="93ea7-176">Zadejte informace o hello stávajícího uživatele Azure AD do hello následující textových polí: **křestní jméno, příjmení**, **uživatelské jméno**, **e-mailu**, **heslo**, **Potvrzení hesla**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-176">Type hello information of an existing Azure AD user into hello following textboxes: **First name, Last name**, **User name**, **Email**, **Password**, **Confirm Password**.</span></span>
   3. <span data-ttu-id="93ea7-177">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-177">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="93ea7-178">Můžete použít všechny ostatní Cisco Webex uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Cisco Webex tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="93ea7-178">You can use any other Cisco Webex user account creation tools or APIs provided by Cisco Webex tooprovision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="93ea7-179">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="93ea7-179">Assign users</span></span>
<span data-ttu-id="93ea7-180">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="93ea7-180">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="93ea7-181">**tooassign uživatelé tooCisco Webex, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="93ea7-181">**tooassign users tooCisco Webex, perform hello following steps:**</span></span>

1. <span data-ttu-id="93ea7-182">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="93ea7-182">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="93ea7-183">Na hello **Cisco Webex** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="93ea7-183">On hello **Cisco Webex** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="93ea7-184">![Přiřazení uživatelů](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="93ea7-184">![Assign users](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Assign users")</span></span>
3. <span data-ttu-id="93ea7-185">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="93ea7-185">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="93ea7-186">![Ano](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="93ea7-186">![Yes](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="93ea7-187">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="93ea7-187">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="93ea7-188">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="93ea7-188">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

