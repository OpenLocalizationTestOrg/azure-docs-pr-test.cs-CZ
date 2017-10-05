---
title: "Kurz: Azure Active Directory integrace s centrální plochy | Microsoft Docs"
description: "Další informace o použití centrální plochy s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: fe32c1d68040ceb9d9de2ad6c4a6dc9ea93f5aef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a><span data-ttu-id="d474a-103">Kurz: Azure Active Directory integrace s centrální plochy</span><span class="sxs-lookup"><span data-stu-id="d474a-103">Tutorial: Azure Active Directory integration with Central Desktop</span></span>
<span data-ttu-id="d474a-104">Cílem tohoto kurzu je zobrazit integraci Azure a centrální plochy.</span><span class="sxs-lookup"><span data-stu-id="d474a-104">The objective of this tutorial is to show the integration of Azure and Central Desktop.</span></span> <span data-ttu-id="d474a-105">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="d474a-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="d474a-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="d474a-106">A valid Azure subscription</span></span>
* <span data-ttu-id="d474a-107">Centrální plochy jednotné přihlašování v předplatném povolené / centrální plochy klienta</span><span class="sxs-lookup"><span data-stu-id="d474a-107">A Central desktop single sign on enabled subscription / Central desktop tenant</span></span>

<span data-ttu-id="d474a-108">Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d474a-108">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

* <span data-ttu-id="d474a-109">Povolení integrace aplikace pro centrální plochy</span><span class="sxs-lookup"><span data-stu-id="d474a-109">Enabling the application integration for Central Desktop</span></span>
* <span data-ttu-id="d474a-110">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="d474a-110">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="d474a-111">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="d474a-111">Configuring user provisioning</span></span>
* <span data-ttu-id="d474a-112">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="d474a-112">Assigning users</span></span>

<span data-ttu-id="d474a-113">![Scénář](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="d474a-113">![Scenario](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-central-desktop"></a><span data-ttu-id="d474a-114">Povolit integraci aplikace pro centrální plochu</span><span class="sxs-lookup"><span data-stu-id="d474a-114">Enable the application integration for Central Desktop</span></span>
<span data-ttu-id="d474a-115">Cílem této části se popisují postup povolení integrace aplikací pro centrální plochu.</span><span class="sxs-lookup"><span data-stu-id="d474a-115">The objective of this section is to outline how to enable the application integration for Central Desktop.</span></span>

<span data-ttu-id="d474a-116">**Pokud chcete povolit integraci aplikací pro centrální plochu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d474a-116">**To enable the application integration for Central Desktop, perform the following steps:**</span></span>

1. <span data-ttu-id="d474a-117">V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d474a-117">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="d474a-118">![Služby Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="d474a-118">![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="d474a-119">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="d474a-119">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="d474a-120">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="d474a-120">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="d474a-121">![Aplikace](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="d474a-121">![Applications](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="d474a-122">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="d474a-122">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="d474a-123">![Přidat aplikaci](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="d474a-123">![Add application](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="d474a-124">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="d474a-124">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="d474a-125">![Přidání aplikace z gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="d474a-125">![Add an application from gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="d474a-126">V **vyhledávacího pole**, typ **centrální plochy**.</span><span class="sxs-lookup"><span data-stu-id="d474a-126">In the **search box**, type **Central Desktop**.</span></span>
   
   <span data-ttu-id="d474a-127">![Galerie aplikací](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="d474a-127">![Application gallery](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Application gallery")</span></span>
7. <span data-ttu-id="d474a-128">V podokně výsledků vyberte **centrální plochy**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="d474a-128">In the results pane, select **Central Desktop**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="d474a-129">![Centrální plochy](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "centrální plochy")</span><span class="sxs-lookup"><span data-stu-id="d474a-129">![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Central Desktop")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="d474a-130">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d474a-130">Configure single sign-on</span></span>

<span data-ttu-id="d474a-131">Cílem této části se popisují, jak uživatelům povolit ověřování centrální plochu do svého účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="d474a-131">The objective of this section is to outline how to enable users to authenticate to Central Desktop with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="d474a-132">V rámci tohoto postupu je nutné odeslat kódování base-64 kódovaného certifikátu klienta centrální plochy.</span><span class="sxs-lookup"><span data-stu-id="d474a-132">As part of this procedure, you are required to upload a base-64 encoded certificate to your Central Desktop tenant.</span></span>  
<span data-ttu-id="d474a-133">Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="d474a-133">If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

<span data-ttu-id="d474a-134">**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d474a-134">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="d474a-135">Na portálu Azure classic na **centrální plochy** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete ** nakonfigurovat jednotné přihlašování ** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d474a-135">In the Azure classic portal, on the **Central Desktop** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.</span></span>
   
   <span data-ttu-id="d474a-136">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="d474a-136">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="d474a-137">Na **jak chcete uživatelům se přihlásit ploše centrální** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d474a-137">On the **How would you like users to sign on to Central Desktop** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="d474a-138">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="d474a-138">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="d474a-139">Na **konfigurace adresy URL aplikace** stránky, proveďte následující kroky a pak klikněte na tlačítko **Další**:</span><span class="sxs-lookup"><span data-stu-id="d474a-139">On the **Configure App URL** page, perform the following steps, and then click **Next**:</span></span> 
   
   1. <span data-ttu-id="d474a-140">V **centrální plochy přihlašovací v adrese URL** textovému poli, zadejte adresu URL vašeho klienta centrální Desktop (např: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="d474a-140">In the **Central Desktop Sign In URL** textbox, type the URL of your Central Desktop tenant (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   2. <span data-ttu-id="d474a-141">Do textového pole Adresa URL odpovědi centrální plochy zadejte vaše adresa URL centrální plochy AssertionConsumerService (např: https://contoso.centraldesktop.com/saml2-assertion.php).</span><span class="sxs-lookup"><span data-stu-id="d474a-141">In the Central  Desktop Reply URL textbox, type your Central Desktop AssertionConsumerService URL (e.g.:  https://contoso.centraldesktop.com/saml2-assertion.php).</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="d474a-142">Můžete získat hodnotu z centrální plochy metadat (např: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="d474a-142">You can get the value from the central desktop metadata (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   >  
   
   <span data-ttu-id="d474a-143">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="d474a-143">![Configure app URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Configure app URL")</span></span>
4. <span data-ttu-id="d474a-144">Na **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, stáhněte si certifikát, klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="d474a-144">On the **Configure single sign-on at Central Desktop** page, to download your certificate, click **Download certificate**, and then save the certificate file on your computer.</span></span>
   
  <span data-ttu-id="d474a-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="d474a-145">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="d474a-146">Přihlaste se k vaší **centrální plochy** klienta.</span><span class="sxs-lookup"><span data-stu-id="d474a-146">Log in to your **Central Desktop** tenant.</span></span>
6. <span data-ttu-id="d474a-147">Přejděte na **nastavení**, klikněte na tlačítko **Upřesnit**a potom klikněte na **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d474a-147">Go to **Settings**, click **Advanced**, and then click **Single Sign On**.</span></span>
   
  <span data-ttu-id="d474a-148">![Instalační program – pokročilé](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "instalační program – Rozšířená")</span><span class="sxs-lookup"><span data-stu-id="d474a-148">![Setup - Advanced](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - Advanced")</span></span>
7. <span data-ttu-id="d474a-149">Na **jeden znak v nastavení** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d474a-149">On the **Single Sign On Settings** page, perform the following steps:</span></span>
   
  <span data-ttu-id="d474a-150">![Jednotné přihlašování v nastavení](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="d474a-150">![Single Sign On Settings](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Single Sign On Settings")</span></span>
   
  1. <span data-ttu-id="d474a-151">Vyberte **povolit SAML v2 jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d474a-151">Select **Enable SAML v2 Single Sign On**.</span></span>
  2. <span data-ttu-id="d474a-152">Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, zkopírujte **URL vystavitele** hodnotu a vložte ji do **URL jednotného přihlašování k** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d474a-152">In the Azure classic portal, on the **Configure single sign-on at Central Desktop** page, copy the **Issuer URL** value, and then paste it into the **SSO URL** textbox.</span></span>
  3. <span data-ttu-id="d474a-153">Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, zkopírujte **vzdálené adresy URL pro přihlášení** hodnotu a vložte ji do **adresu URL pro přihlášení SSO** textové pole .</span><span class="sxs-lookup"><span data-stu-id="d474a-153">In the Azure classic portal, on the **Configure single sign-on at Central Desktop** page, copy the **Remote Login URL** value, and then paste it into the **SSO Login URL** textbox.</span></span>
  4. <span data-ttu-id="d474a-154">Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, zkopírujte **jednu adresu URL služby Sign-Out** hodnotu a vložte ji do **adresy URL odhlašovací jednotného přihlašování k**textové pole.</span><span class="sxs-lookup"><span data-stu-id="d474a-154">In the Azure classic portal, on the **Configure single sign-on at Central Desktop** page, copy the **Single Sign-Out Service URL** value, and then paste it into the **SSO Logout URL** textbox.</span></span>
8. <span data-ttu-id="d474a-155">V **metodu ověřování podpisu zpráva** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d474a-155">In the **Message Signature Verification Method** section, perform the following steps:</span></span>
   
   <span data-ttu-id="d474a-156">![Zpráva metodu ověřování podpisu](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "zprávy metodu ověřování podpisu")</span><span class="sxs-lookup"><span data-stu-id="d474a-156">![Message Signature Verification Method](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Message Signature Verification Method")</span></span>
   
  1. <span data-ttu-id="d474a-157">Vyberte **certifikát**.</span><span class="sxs-lookup"><span data-stu-id="d474a-157">Select **Certificate**.</span></span>
  2. <span data-ttu-id="d474a-158">Z **certifikát jednotného přihlašování k** seznamu, vyberte **RSH SHA256**.</span><span class="sxs-lookup"><span data-stu-id="d474a-158">From the **SSO Certificate** list, select **RSH SHA256**.</span></span>
  3. <span data-ttu-id="d474a-159">Vytvořte textový soubor z stažený certifikát, kopírovat obsah textového souboru a vložte ji do **certifikát jednotného přihlašování k** pole.</span><span class="sxs-lookup"><span data-stu-id="d474a-159">Create a text file from the downloaded certificate, copy the content of the text file, and then paste it into the **SSO Certificate** field.</span></span>  
     >[!TIP]
     ><span data-ttu-id="d474a-160">Další podrobnosti najdete v tématu [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="d474a-160">For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
      >  
   4. <span data-ttu-id="d474a-161">Vyberte **zobrazí odkaz na přihlašovací stránku vaší SAMLv2**.</span><span class="sxs-lookup"><span data-stu-id="d474a-161">Select **Display a link to your SAMLv2 login page**.</span></span>
9. <span data-ttu-id="d474a-162">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="d474a-162">Click **Update**.</span></span>
10. <span data-ttu-id="d474a-163">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d474a-163">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="d474a-164">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="d474a-164">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Configure single sign-on")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="d474a-165">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="d474a-165">Configure user provisioning</span></span>

<span data-ttu-id="d474a-166">AAD uživatelé moci přihlásit musí být zřízená aplikaci Centrální plochy.</span><span class="sxs-lookup"><span data-stu-id="d474a-166">For AAD users to be able to sign in, they must be provisioned to the Central Desktop application.</span></span> <span data-ttu-id="d474a-167">Tato část popisuje postup vytvoření AAD uživatelské účty v centrální plochy.</span><span class="sxs-lookup"><span data-stu-id="d474a-167">This section describes how to create AAD user accounts in Central Desktop.</span></span>

<span data-ttu-id="d474a-168">**Ke zřízení uživatelských účtů do centrální plochy:**</span><span class="sxs-lookup"><span data-stu-id="d474a-168">**To provision user accounts to Central Desktop:**</span></span>
1. <span data-ttu-id="d474a-169">Přihlaste se ke klientovi centrální plochy.</span><span class="sxs-lookup"><span data-stu-id="d474a-169">Log in to your Central Desktop tenant.</span></span>
2. <span data-ttu-id="d474a-170">Přejděte na **osoby \> vnitřní členy**.</span><span class="sxs-lookup"><span data-stu-id="d474a-170">Go to **People \> Internal Members**.</span></span>
3. <span data-ttu-id="d474a-171">Klikněte na tlačítko **přidat vnitřní členy**.</span><span class="sxs-lookup"><span data-stu-id="d474a-171">Click **Add Internal Members**.</span></span>
   
  <span data-ttu-id="d474a-172">![Lidé](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="d474a-172">![People](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "People")</span></span>
4. <span data-ttu-id="d474a-173">V **e-mailovou adresu nové členy** textovému poli, zadejte účet AAD určené ke zřízení a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d474a-173">In the **Email Address of New Members** textbox, type an AAD account you want to provision, and then click **Next**.</span></span>
   
  <span data-ttu-id="d474a-174">![E-mailové adresy nové členy](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "e-mailové adresy nové členy")</span><span class="sxs-lookup"><span data-stu-id="d474a-174">![Email Addresses of New Members](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Email Addresses of New Members")</span></span>
5. <span data-ttu-id="d474a-175">Klikněte na tlačítko **přidejte interní členy**.</span><span class="sxs-lookup"><span data-stu-id="d474a-175">Click **Add Internal member(s)**.</span></span>
   
  <span data-ttu-id="d474a-176">![Přidání interní člena](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "přidat vnitřní člen")</span><span class="sxs-lookup"><span data-stu-id="d474a-176">![Add Internal Member](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Add Internal Member")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="d474a-177">Uživatelé, pro které jste přidali obdrží e-mail, který obsahuje odkaz potvrzení, které potřebují k klikněte na možnost aktivovat účet.</span><span class="sxs-lookup"><span data-stu-id="d474a-177">The users you have added will receive an email that includes a confirmation link they need to click to activate the account.</span></span>
   > 

>[!NOTE]
><span data-ttu-id="d474a-178">Můžete použít jakékoli jiné centrální plochy uživatele účtu vytvoření nástroje nebo rozhraní API poskytované centrální plochy zřídit AAD uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="d474a-178">You can use any other Central Desktop user account creation tools or APIs provided by Central Desktop to provision AAD user accounts</span></span>
>  

## <a name="assign-users"></a><span data-ttu-id="d474a-179">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="d474a-179">Assign users</span></span>
<span data-ttu-id="d474a-180">Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="d474a-180">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="d474a-181">**Přiřazení uživatelů k centrální plochy, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d474a-181">**To assign users to Central Desktop, perform the following steps:**</span></span>

1. <span data-ttu-id="d474a-182">Na portálu Azure classic vytvořte zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="d474a-182">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="d474a-183">Na **centrální plochy** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="d474a-183">On the **Central Desktop** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="d474a-184">![Přiřazení uživatelů](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="d474a-184">![Assign users](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Assign users")</span></span>
3. <span data-ttu-id="d474a-185">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.</span><span class="sxs-lookup"><span data-stu-id="d474a-185">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="d474a-186">![Ano](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="d474a-186">![Yes](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="d474a-187">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="d474a-187">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="d474a-188">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d474a-188">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

