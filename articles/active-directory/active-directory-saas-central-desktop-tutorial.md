---
title: "Kurz: Azure Active Directory integrace s centrální plochy | Microsoft Docs"
description: "Zjistěte, jak toouse centrální plochy s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
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
ms.openlocfilehash: 93036ae801c446ce476288c00579931ba10a843b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a><span data-ttu-id="f0885-103">Kurz: Azure Active Directory integrace s centrální plochy</span><span class="sxs-lookup"><span data-stu-id="f0885-103">Tutorial: Azure Active Directory integration with Central Desktop</span></span>
<span data-ttu-id="f0885-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a centrální plochy.</span><span class="sxs-lookup"><span data-stu-id="f0885-104">hello objective of this tutorial is tooshow hello integration of Azure and Central Desktop.</span></span> <span data-ttu-id="f0885-105">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f0885-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="f0885-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="f0885-106">A valid Azure subscription</span></span>
* <span data-ttu-id="f0885-107">Centrální plochy jednotné přihlašování v předplatném povolené / centrální plochy klienta</span><span class="sxs-lookup"><span data-stu-id="f0885-107">A Central desktop single sign on enabled subscription / Central desktop tenant</span></span>

<span data-ttu-id="f0885-108">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="f0885-108">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

* <span data-ttu-id="f0885-109">Povolení integrace aplikace hello pro centrální plochy</span><span class="sxs-lookup"><span data-stu-id="f0885-109">Enabling hello application integration for Central Desktop</span></span>
* <span data-ttu-id="f0885-110">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="f0885-110">Configuring single sign-on (SSO)</span></span>
* <span data-ttu-id="f0885-111">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="f0885-111">Configuring user provisioning</span></span>
* <span data-ttu-id="f0885-112">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="f0885-112">Assigning users</span></span>

<span data-ttu-id="f0885-113">![Scénář](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="f0885-113">![Scenario](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-central-desktop"></a><span data-ttu-id="f0885-114">Povolit integraci aplikace hello pro centrální plochu</span><span class="sxs-lookup"><span data-stu-id="f0885-114">Enable hello application integration for Central Desktop</span></span>
<span data-ttu-id="f0885-115">Hello cílem této části je toooutline jak tooenable hello integraci aplikací pro centrální plochu.</span><span class="sxs-lookup"><span data-stu-id="f0885-115">hello objective of this section is toooutline how tooenable hello application integration for Central Desktop.</span></span>

<span data-ttu-id="f0885-116">**Integrace aplikace hello tooenable pro centrální plochy, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0885-116">**tooenable hello application integration for Central Desktop, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0885-117">V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f0885-117">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="f0885-118">![Služby Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="f0885-118">![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="f0885-119">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="f0885-119">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="f0885-120">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="f0885-120">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="f0885-121">![Aplikace](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="f0885-121">![Applications](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="f0885-122">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="f0885-122">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="f0885-123">![Přidat aplikaci](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="f0885-123">![Add application](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="f0885-124">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="f0885-124">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="f0885-125">![Přidání aplikace z gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="f0885-125">![Add an application from gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="f0885-126">V hello **vyhledávacího pole**, typ **centrální plochy**.</span><span class="sxs-lookup"><span data-stu-id="f0885-126">In hello **search box**, type **Central Desktop**.</span></span>
   
   <span data-ttu-id="f0885-127">![Galerie aplikací](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="f0885-127">![Application gallery](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Application gallery")</span></span>
7. <span data-ttu-id="f0885-128">V podokně výsledků hello, vyberte **centrální plochy**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0885-128">In hello results pane, select **Central Desktop**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="f0885-129">![Centrální plochy](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "centrální plochy")</span><span class="sxs-lookup"><span data-stu-id="f0885-129">![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Central Desktop")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="f0885-130">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0885-130">Configure single sign-on</span></span>

<span data-ttu-id="f0885-131">Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooCentral plochy ke svému účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="f0885-131">hello objective of this section is toooutline how tooenable users tooauthenticate tooCentral Desktop with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="f0885-132">V rámci tohoto postupu jsou požadované tooupload klienta centrální plochy tooyour kódování base-64 kódovaného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f0885-132">As part of this procedure, you are required tooupload a base-64 encoded certificate tooyour Central Desktop tenant.</span></span>  
<span data-ttu-id="f0885-133">Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="f0885-133">If you are not familiar with this procedure, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

<span data-ttu-id="f0885-134">**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f0885-134">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0885-135">V portálu Azure classic, na hello hello **centrální plochy** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello ** nakonfigurovat jednotné přihlašování ** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0885-135">In hello Azure classic portal, on hello **Central Desktop** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On ** dialog.</span></span>
   
   <span data-ttu-id="f0885-136">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="f0885-136">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="f0885-137">Na hello **jak jste by například uživatelé toosign na tooCentral plochy** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0885-137">On hello **How would you like users toosign on tooCentral Desktop** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="f0885-138">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="f0885-138">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Configure single sign-on")</span></span>
3. <span data-ttu-id="f0885-139">Na hello **konfigurace adresy URL aplikace** stránky, proveďte hello následující kroky a pak klikněte na tlačítko **Další**:</span><span class="sxs-lookup"><span data-stu-id="f0885-139">On hello **Configure App URL** page, perform hello following steps, and then click **Next**:</span></span> 
   
   1. <span data-ttu-id="f0885-140">V hello **centrální plochy přihlašovací v adrese URL** textovému poli, typ hello adresu URL vašeho klienta centrální Desktop (např: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="f0885-140">In hello **Central Desktop Sign In URL** textbox, type hello URL of your Central Desktop tenant (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   2. <span data-ttu-id="f0885-141">V textovém poli Adresa URL odpovědi centrální plochy hello, zadejte adresu URL centrální AssertionConsumerService plochy (např: https://contoso.centraldesktop.com/saml2-assertion.php).</span><span class="sxs-lookup"><span data-stu-id="f0885-141">In hello Central  Desktop Reply URL textbox, type your Central Desktop AssertionConsumerService URL (e.g.:  https://contoso.centraldesktop.com/saml2-assertion.php).</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="f0885-142">Hodnota hello můžete získat z centrální plochy metadat hello (např: *http://contoso.centraldesktop.com*).</span><span class="sxs-lookup"><span data-stu-id="f0885-142">You can get hello value from hello central desktop metadata (e.g.: *http://contoso.centraldesktop.com*).</span></span>
   >  
   
   <span data-ttu-id="f0885-143">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="f0885-143">![Configure app URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Configure app URL")</span></span>
4. <span data-ttu-id="f0885-144">Na hello **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, toodownload svůj certifikát, klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f0885-144">On hello **Configure single sign-on at Central Desktop** page, toodownload your certificate, click **Download certificate**, and then save hello certificate file on your computer.</span></span>
   
  <span data-ttu-id="f0885-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="f0885-145">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Configure single sign-on")</span></span>
5. <span data-ttu-id="f0885-146">Přihlaste se tooyour **centrální plochy** klienta.</span><span class="sxs-lookup"><span data-stu-id="f0885-146">Log in tooyour **Central Desktop** tenant.</span></span>
6. <span data-ttu-id="f0885-147">Přejděte příliš**nastavení**, klikněte na tlačítko **Upřesnit**a potom klikněte na **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f0885-147">Go too**Settings**, click **Advanced**, and then click **Single Sign On**.</span></span>
   
  <span data-ttu-id="f0885-148">![Instalační program – pokročilé](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "instalační program – Rozšířená")</span><span class="sxs-lookup"><span data-stu-id="f0885-148">![Setup - Advanced](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - Advanced")</span></span>
7. <span data-ttu-id="f0885-149">Na hello **jeden znak v nastavení** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0885-149">On hello **Single Sign On Settings** page, perform hello following steps:</span></span>
   
  <span data-ttu-id="f0885-150">![Jednotné přihlašování v nastavení](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="f0885-150">![Single Sign On Settings](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Single Sign On Settings")</span></span>
   
  1. <span data-ttu-id="f0885-151">Vyberte **povolit SAML v2 jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f0885-151">Select **Enable SAML v2 Single Sign On**.</span></span>
  2. <span data-ttu-id="f0885-152">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, kopie hello **URL vystavitele** hodnotu a pak ji vložit do hello **jednotného přihlašování k adrese URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f0885-152">In hello Azure classic portal, on hello **Configure single sign-on at Central Desktop** page, copy hello **Issuer URL** value, and then paste it into hello **SSO URL** textbox.</span></span>
  3. <span data-ttu-id="f0885-153">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, kopie hello **vzdálené adresy URL pro přihlášení** hodnotu a pak ji vložit do hello **adresu URL pro přihlášení SSO**textové pole.</span><span class="sxs-lookup"><span data-stu-id="f0885-153">In hello Azure classic portal, on hello **Configure single sign-on at Central Desktop** page, copy hello **Remote Login URL** value, and then paste it into hello **SSO Login URL** textbox.</span></span>
  4. <span data-ttu-id="f0885-154">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, kopie hello **jednu adresu URL služby Sign-Out** hodnotu a pak ji vložit do hello **adresy URL odhlašovací jednotného přihlašování k** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f0885-154">In hello Azure classic portal, on hello **Configure single sign-on at Central Desktop** page, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **SSO Logout URL** textbox.</span></span>
8. <span data-ttu-id="f0885-155">V hello **metodu ověřování podpisu zpráva** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f0885-155">In hello **Message Signature Verification Method** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="f0885-156">![Zpráva metodu ověřování podpisu](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "zprávy metodu ověřování podpisu")</span><span class="sxs-lookup"><span data-stu-id="f0885-156">![Message Signature Verification Method](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Message Signature Verification Method")</span></span>
   
  1. <span data-ttu-id="f0885-157">Vyberte **certifikát**.</span><span class="sxs-lookup"><span data-stu-id="f0885-157">Select **Certificate**.</span></span>
  2. <span data-ttu-id="f0885-158">Z hello **certifikát jednotného přihlašování k** seznamu, vyberte **RSH SHA256**.</span><span class="sxs-lookup"><span data-stu-id="f0885-158">From hello **SSO Certificate** list, select **RSH SHA256**.</span></span>
  3. <span data-ttu-id="f0885-159">Vytvořte textový soubor z hello stáhnout certifikát, hello kopírování obsahu hello textového souboru a pak ji vložit do hello **certifikát jednotného přihlašování k** pole.</span><span class="sxs-lookup"><span data-stu-id="f0885-159">Create a text file from hello downloaded certificate, copy hello content of hello text file, and then paste it into hello **SSO Certificate** field.</span></span>  
     >[!TIP]
     ><span data-ttu-id="f0885-160">Další podrobnosti najdete v tématu [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="f0885-160">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>
      >  
   4. <span data-ttu-id="f0885-161">Vyberte **zobrazit stránku přihlášení tooyour SAMLv2 odkaz**.</span><span class="sxs-lookup"><span data-stu-id="f0885-161">Select **Display a link tooyour SAMLv2 login page**.</span></span>
9. <span data-ttu-id="f0885-162">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="f0885-162">Click **Update**.</span></span>
10. <span data-ttu-id="f0885-163">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0885-163">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="f0885-164">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="f0885-164">![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Configure single sign-on")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="f0885-165">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="f0885-165">Configure user provisioning</span></span>

<span data-ttu-id="f0885-166">Pro AAD uživatelé toobe možné toosign v musí být zřízená toohello centrální plochy aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0885-166">For AAD users toobe able toosign in, they must be provisioned toohello Central Desktop application.</span></span> <span data-ttu-id="f0885-167">Tato část popisuje, jak toocreate AAD uživatelských účtů v centrální plochy.</span><span class="sxs-lookup"><span data-stu-id="f0885-167">This section describes how toocreate AAD user accounts in Central Desktop.</span></span>

<span data-ttu-id="f0885-168">**tooprovision uživatelské účty tooCentral plochy:**</span><span class="sxs-lookup"><span data-stu-id="f0885-168">**tooprovision user accounts tooCentral Desktop:**</span></span>
1. <span data-ttu-id="f0885-169">Přihlaste se tooyour centrální plochy klienta.</span><span class="sxs-lookup"><span data-stu-id="f0885-169">Log in tooyour Central Desktop tenant.</span></span>
2. <span data-ttu-id="f0885-170">Přejděte příliš**osoby \> vnitřní členy**.</span><span class="sxs-lookup"><span data-stu-id="f0885-170">Go too**People \> Internal Members**.</span></span>
3. <span data-ttu-id="f0885-171">Klikněte na tlačítko **přidat vnitřní členy**.</span><span class="sxs-lookup"><span data-stu-id="f0885-171">Click **Add Internal Members**.</span></span>
   
  <span data-ttu-id="f0885-172">![Lidé](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="f0885-172">![People](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "People")</span></span>
4. <span data-ttu-id="f0885-173">V hello **e-mailovou adresu nové členy** textovému poli, zadejte účet AAD tooprovision a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0885-173">In hello **Email Address of New Members** textbox, type an AAD account you want tooprovision, and then click **Next**.</span></span>
   
  <span data-ttu-id="f0885-174">![E-mailové adresy nové členy](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "e-mailové adresy nové členy")</span><span class="sxs-lookup"><span data-stu-id="f0885-174">![Email Addresses of New Members](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Email Addresses of New Members")</span></span>
5. <span data-ttu-id="f0885-175">Klikněte na tlačítko **přidejte interní členy**.</span><span class="sxs-lookup"><span data-stu-id="f0885-175">Click **Add Internal member(s)**.</span></span>
   
  <span data-ttu-id="f0885-176">![Přidání interní člena](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "přidat vnitřní člen")</span><span class="sxs-lookup"><span data-stu-id="f0885-176">![Add Internal Member](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Add Internal Member")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="f0885-177">Hello uživatelů, které jste přidali obdrží e-mail, který obsahuje odkaz potvrzení potřebují tooclick tooactivate hello účtu.</span><span class="sxs-lookup"><span data-stu-id="f0885-177">hello users you have added will receive an email that includes a confirmation link they need tooclick tooactivate hello account.</span></span>
   > 

>[!NOTE]
><span data-ttu-id="f0885-178">Můžete použít jakékoli jiné centrální plochy uživatele účtu vytvoření nástroje nebo rozhraní API poskytované centrální plochy tooprovision AAD uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="f0885-178">You can use any other Central Desktop user account creation tools or APIs provided by Central Desktop tooprovision AAD user accounts</span></span>
>  

## <a name="assign-users"></a><span data-ttu-id="f0885-179">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="f0885-179">Assign users</span></span>
<span data-ttu-id="f0885-180">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="f0885-180">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="f0885-181">**tooassign uživatelé tooCentral plochy, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0885-181">**tooassign users tooCentral Desktop, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0885-182">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="f0885-182">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="f0885-183">Na hello **centrální plochy** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="f0885-183">On hello **Central Desktop** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="f0885-184">![Přiřazení uživatelů](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="f0885-184">![Assign users](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Assign users")</span></span>
3. <span data-ttu-id="f0885-185">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="f0885-185">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="f0885-186">![Ano](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="f0885-186">![Yes](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="f0885-187">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f0885-187">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="f0885-188">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f0885-188">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

