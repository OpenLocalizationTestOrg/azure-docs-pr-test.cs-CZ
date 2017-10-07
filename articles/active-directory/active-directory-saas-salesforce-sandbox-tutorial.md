---
title: "Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce | Microsoft Docs"
description: "Zjistěte, jak toouse Salesforce izolovaného prostoru s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: deccd6a07b57c8ba56b7e1c3f3ab311813ca017b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="618ee-103">Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce</span><span class="sxs-lookup"><span data-stu-id="618ee-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="618ee-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a izolovaného prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="618ee-104">hello objective of this tutorial is tooshow hello integration of Azure and Salesforce Sandbox.</span></span>  

>[!TIP]
><span data-ttu-id="618ee-105">Pro zpětnou vazbu, najdete v části hello [stránky podpory Azure](http://go.microsoft.com/fwlink/?LinkId=521878).</span><span class="sxs-lookup"><span data-stu-id="618ee-105">For feedback, see hello [Azure support page](http://go.microsoft.com/fwlink/?LinkId=521878).</span></span> 
> 

<span data-ttu-id="618ee-106">Udělte izolovaných prostorů můžete hello možnost toocreate více kopií vaší organizace v samostatných prostředí pro jiné účely, například vývoj, testování a cvičení bez ohrožení hello datům a aplikacím v provozním Salesforce organizace.</span><span class="sxs-lookup"><span data-stu-id="618ee-106">Sandboxes give you hello ability toocreate multiple copies of your organization in separate environments for a variety of purposes, such as development, testing, and training, without compromising hello data and applications in your Salesforce production organization.</span></span>  

<span data-ttu-id="618ee-107">Další podrobnosti najdete v tématu [izolovaného prostoru Přehled](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span><span class="sxs-lookup"><span data-stu-id="618ee-107">For more details, see [Sandbox Overview](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)</span></span>

<span data-ttu-id="618ee-108">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="618ee-108">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="618ee-109">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="618ee-109">A valid Azure subscription</span></span>
* <span data-ttu-id="618ee-110">Izolovaného prostoru v Salesforce.com</span><span class="sxs-lookup"><span data-stu-id="618ee-110">A sandbox in Salesforce.com</span></span>

<span data-ttu-id="618ee-111">Pokud platný izolovaného prostoru v Salesforce.com zatím nemáte, je třeba toocontact Salesforce.</span><span class="sxs-lookup"><span data-stu-id="618ee-111">If you don’t have a valid sandbox in Salesforce.com yet, you need toocontact Salesforce.</span></span>

<span data-ttu-id="618ee-112">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="618ee-112">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="618ee-113">Povolení hello integraci aplikací pro izolovaný prostor Salesforce</span><span class="sxs-lookup"><span data-stu-id="618ee-113">Enabling hello application integration for Salesforce Sandbox</span></span>
2. <span data-ttu-id="618ee-114">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="618ee-114">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="618ee-115">Povolení vaší doméně</span><span class="sxs-lookup"><span data-stu-id="618ee-115">Enabling your domain</span></span>
4. <span data-ttu-id="618ee-116">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="618ee-116">Configuring user provisioning</span></span>
5. <span data-ttu-id="618ee-117">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="618ee-117">Assigning users</span></span>

<span data-ttu-id="618ee-118">![Scénář](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="618ee-118">![Scenario](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-salesforce-sandbox"></a><span data-ttu-id="618ee-119">Povolit integraci aplikace hello pro izolovaný prostor Salesforce</span><span class="sxs-lookup"><span data-stu-id="618ee-119">Enable hello application integration for Salesforce Sandbox</span></span>
<span data-ttu-id="618ee-120">Hello cílem této části je toooutline jak tooenable hello integraci aplikací pro izolovaný prostor Salesforce.</span><span class="sxs-lookup"><span data-stu-id="618ee-120">hello objective of this section is toooutline how tooenable hello application integration for Salesforce sandbox.</span></span>

<span data-ttu-id="618ee-121">**Integrace aplikace hello tooenable pro izolovaný prostor Salesforce, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="618ee-121">**tooenable hello application integration for Salesforce sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="618ee-122">V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="618ee-122">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="618ee-123">![Služby Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="618ee-123">![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="618ee-124">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="618ee-124">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="618ee-125">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="618ee-125">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="618ee-126">![Aplikace](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="618ee-126">![Applications](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="618ee-127">tooopen hello **galerii aplikací**, klikněte na tlačítko **přidat aplikaci**a potom klikněte na **přidat aplikaci pro Moje organizace toouse**.</span><span class="sxs-lookup"><span data-stu-id="618ee-127">tooopen hello **Application Gallery**, click **Add An App**, and then click **Add an application for my organization toouse**.</span></span>
   
   <span data-ttu-id="618ee-128">![Co chcete toodo? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Co chcete toodo?")</span><span class="sxs-lookup"><span data-stu-id="618ee-128">![What do you want toodo?](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "What do you want toodo?")</span></span>
5. <span data-ttu-id="618ee-129">V hello **vyhledávacího pole**, typ **izolovaného prostoru Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="618ee-129">In hello **search box**, type **Salesforce Sandbox**.</span></span>
   
   <span data-ttu-id="618ee-130">![Galerie aplikací](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="618ee-130">![Application Gallery](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Application Gallery")</span></span>
6. <span data-ttu-id="618ee-131">V podokně výsledků hello, vyberte **izolovaného prostoru Salesforce**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="618ee-131">In hello results pane, select **Salesforce Sandbox**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="618ee-132">![Izolovaný prostor Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "izolovaného prostoru Salesforce")</span><span class="sxs-lookup"><span data-stu-id="618ee-132">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")</span></span>
   
## <a name="configur-single-sign-on-sso"></a><span data-ttu-id="618ee-133">Configur jednotné přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="618ee-133">Configur single sign-on (SSO)</span></span>

<span data-ttu-id="618ee-134">Hello cílem této části je toooutline jak tooSalesforce tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="618ee-134">hello objective of this section is toooutline how tooenable users tooauthenticate tooSalesforce with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="618ee-135">**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="618ee-135">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="618ee-136">V portálu Azure classic, na hello hello **izolovaného prostoru Salesforce** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="618ee-136">In hello Azure classic portal, on hello **Salesforce Sandbox** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="618ee-137">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="618ee-137">![Configure single sign-on](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="618ee-138">Na hello **jak jste by například uživatelé toosign na tooSalesforce izolovaného prostoru** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="618ee-138">On hello **How would you like users toosign on tooSalesforce Sandbox** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="618ee-139">![Izolovaný prostor Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "izolovaného prostoru Salesforce")</span><span class="sxs-lookup"><span data-stu-id="618ee-139">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")</span></span>
3. <span data-ttu-id="618ee-140">Na hello **konfigurace adresy URL aplikace** stránku hello **přihlašovací adresa URL** textovému poli, zadejte URL pomocí hello následující vzor `http://company.my.salesforce.com`a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="618ee-140">On hello **Configure App URL** page, in hello **Sign On URL** textbox, type your URL using hello following pattern `http://company.my.salesforce.com`, and then click **Next**.</span></span>
   
   <span data-ttu-id="618ee-141">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="618ee-141">![Configure App URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Configure App URL")</span></span>
4. <span data-ttu-id="618ee-142">Pokud jste již nakonfigurovali jednotné přihlašování pro jiná instance Salesforce izolovaného prostoru v adresáři, pak je také potřeba nakonfigurovat hello **identifikátor** toohave hello stejnou hodnotu jako hello **přihlásit na adrese URL**.</span><span class="sxs-lookup"><span data-stu-id="618ee-142">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure hello **Identifier** toohave hello same value as hello **Sign on URL**.</span></span> 
 * <span data-ttu-id="618ee-143">Hello **identifikátor** pole naleznete kontrolou hello **zobrazit upřesňující nastavení** zaškrtávací políčko je na hello **konfigurace adresy URL aplikace** stránku hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="618ee-143">hello **Identifier** field can be found by checking hello **Show advanced settings** checkbox on hello **Configure App URL** page of hello dialog.</span></span>
5. <span data-ttu-id="618ee-144">Na hello **nakonfigurovat jednotné přihlašování v izolovaném prostoru Salesforce** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="618ee-144">On hello **Configure single sign-on at Salesforce Sandbox** page, click **Download certificate**, and then save hello certificate file on your computer.</span></span>
   
   <span data-ttu-id="618ee-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="618ee-145">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="618ee-146">V okně prohlížeče jiný web Přihlaste se jako správce do izolovaného prostoru vaší služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="618ee-146">In a different web browser window, log into your Salesforce sandbox as an administrator.</span></span>
7. <span data-ttu-id="618ee-147">V nabídce hello hello nahoře, klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="618ee-147">In hello menu on hello top, click **Setup**.</span></span>
   
   <span data-ttu-id="618ee-148">![Instalační program](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="618ee-148">![Setup](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")</span></span>
8. <span data-ttu-id="618ee-149">V navigačním podokně hello na levé straně hello, klikněte na tlačítko **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="618ee-149">In hello navigation pane on hello left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="618ee-150">![Jednotné přihlašování v nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="618ee-150">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Single Sign-On Settings")</span></span>
9. <span data-ttu-id="618ee-151">V části Nastavení jednotného přihlašování hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="618ee-151">On hello Single Sign-On Settings section, perform hello following steps:</span></span>
   
   <span data-ttu-id="618ee-152">![Jednotné přihlašování v nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="618ee-152">![Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Single Sign-On Settings")</span></span>  
 1.  <span data-ttu-id="618ee-153">Vyberte **povoleno SAML**.</span><span class="sxs-lookup"><span data-stu-id="618ee-153">Select **SAML Enabled**.</span></span> 
 2.  <span data-ttu-id="618ee-154">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="618ee-154">Click **New**.</span></span>
10. <span data-ttu-id="618ee-155">V hello oddílu SAML jeden přihlašování nastavení proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="618ee-155">On hello SAML Single Sign-On Settings section, perform hello following steps:</span></span>
    
    <span data-ttu-id="618ee-156">![SAML jeden nastavení přihlášení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML jeden nastavení přihlášení")</span><span class="sxs-lookup"><span data-stu-id="618ee-156">![SAML Single Sign-On Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML Single Sign-On Settings")</span></span>  
 1. <span data-ttu-id="618ee-157">Do textového pole Název hello, zadejte název hello hello konfigurace (např: *SPSSOWAAD\_Test*).</span><span class="sxs-lookup"><span data-stu-id="618ee-157">In hello Name textbox, type hello name of hello configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 
 2. <span data-ttu-id="618ee-158">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v izolovaném prostoru Salesforce** dialogu stránky, kopie hello **URL vystavitele** hodnotu a pak ji vložit do hello **vystavitele**textové pole.</span><span class="sxs-lookup"><span data-stu-id="618ee-158">In hello Azure classic portal, on hello **Configure single sign-on at Salesforce Sandbox** dialogue page, copy hello **Issuer URL** value, and then paste it into hello **Issuer** textbox.</span></span>
 3. <span data-ttu-id="618ee-159">V hello **Entity Id** textovému poli, typ **https://test.salesforce.com** Pokud se jedná o první instance Salesforce izolovaného prostoru hello přidáváte tooyour adresáře.</span><span class="sxs-lookup"><span data-stu-id="618ee-159">In hello **Entity Id** textbox, type **https://test.salesforce.com** if this is hello first Salesforce Sandbox instance that you are adding tooyour directory.</span></span> <span data-ttu-id="618ee-160">Pokud jste již přidali instance Salesforce karantény, pak pro hello **Entity ID** typu v hello **přihlašovací adresa URL**, což by mělo být v tomto formátu:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="618ee-160">If you have already added an instance of Salesforce Sandbox, then for hello **Entity ID** type in hello **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>   
 4. <span data-ttu-id="618ee-161">Klikněte na tlačítko **Procházet** tooupload hello stáhnout certifikát.</span><span class="sxs-lookup"><span data-stu-id="618ee-161">Click **Browse** tooupload hello downloaded certificate.</span></span>  
 5. <span data-ttu-id="618ee-162">Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje hello federace z objektu uživatele hello**.</span><span class="sxs-lookup"><span data-stu-id="618ee-162">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span> 
 6. <span data-ttu-id="618ee-163">Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier hello hello subjektu příkaz**.</span><span class="sxs-lookup"><span data-stu-id="618ee-163">As **SAML Identity Location**, select **Identity is in hello NameIdentifier element of hello Subject statement**.</span></span>
 7. <span data-ttu-id="618ee-164">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v izolovaném prostoru Salesforce** dialogu stránky, kopie hello **vzdálené adresy URL pro přihlášení** hodnotu a pak ji vložit do hello **zprostředkovatele Identity Adresa URL pro přihlášení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="618ee-164">In hello Azure classic portal, on hello **Configure single sign-on at Salesforce Sandbox** dialogue page, copy hello **Remote Login URL** value, and then paste it into hello **Identity Provider Login URL** textbox.</span></span> 
 8. <span data-ttu-id="618ee-165">SFDC nepodporuje SAML odhlášení.</span><span class="sxs-lookup"><span data-stu-id="618ee-165">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="618ee-166">Jako alternativní řešení, vložte 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' do hello **adresa URL odhlašovací zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="618ee-166">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into hello **Identity Provider Logout URL** textbox.</span></span>
 9. <span data-ttu-id="618ee-167">Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="618ee-167">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 
 10. <span data-ttu-id="618ee-168">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="618ee-168">Click **Save**.</span></span>
11. <span data-ttu-id="618ee-169">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="618ee-169">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="618ee-170">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="618ee-170">![Configure Single Sign-On](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Configure Single Sign-On")</span></span>

## <a name="enable-your-domain"></a><span data-ttu-id="618ee-171">Povolit doménu</span><span class="sxs-lookup"><span data-stu-id="618ee-171">Enable your domain</span></span>
<span data-ttu-id="618ee-172">V této části se předpokládá, že jste již vytvořili domény.</span><span class="sxs-lookup"><span data-stu-id="618ee-172">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="618ee-173">Další podrobnosti najdete v tématu [definování váš název domény](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="618ee-173">For more details, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="618ee-174">**tooenable doménu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="618ee-174">**tooenable your domain, perform hello following steps:**</span></span>

1. <span data-ttu-id="618ee-175">V levém navigačním podokně hello, klikněte na **Správa domén**a pak klikněte na tlačítko **Moje domény.**</span><span class="sxs-lookup"><span data-stu-id="618ee-175">In hello left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
   <span data-ttu-id="618ee-176">![Moje doména](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="618ee-176">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "My Domain")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="618ee-177">Přesvědčte se, že doménu je správně nakonfigurováno.</span><span class="sxs-lookup"><span data-stu-id="618ee-177">Please make sure that your domain has been configured correctly.</span></span> 
   > 
2. <span data-ttu-id="618ee-178">V hello **nastavení přihlašovací stránky** klikněte na tlačítko **upravit**, pak jako **ověřovací služby**, vyberte název hello hello SAML jeden přihlašování nastavení z předchozí hello část a nakonec klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="618ee-178">In hello **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select hello name of hello SAML Single Sign-On Setting from hello previous section, and finally click **Save**.</span></span>
   
   <span data-ttu-id="618ee-179">![Moje doména](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="618ee-179">![My Domain](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "My Domain")</span></span>

<span data-ttu-id="618ee-180">Jakmile máte doménu nakonfigurován, uživatelé měli používat hello domény adresy URL toologin toohello Salesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="618ee-180">As soon as you have a domain configured, your users should use hello domain URL toologin toohello Salesforce sandbox.</span></span>  

<span data-ttu-id="618ee-181">Hodnota hello tooget hello adresy URL, klikněte na tlačítko hello jednotného přihlašování k profilu, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="618ee-181">tooget hello value of hello URL, click hello SSO profile you have created in hello previous section.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="618ee-182">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="618ee-182">Configure user provisioning</span></span>
<span data-ttu-id="618ee-183">Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele účtů tooSalesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="618ee-183">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooSalesforce Sandbox.</span></span>

<span data-ttu-id="618ee-184">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="618ee-184">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="618ee-185">Salesforce portálu hello v horním navigačním panelu hello vyberte vaše tooexpand název nabídky vaše uživatele:</span><span class="sxs-lookup"><span data-stu-id="618ee-185">In hello Salesforce portal, in hello top navigation bar, select your name tooexpand your user menu:</span></span>
   
   <span data-ttu-id="618ee-186">![Nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="618ee-186">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "My Settings")</span></span>
2. <span data-ttu-id="618ee-187">Z nabídky uživatele, vyberte **Moje nastavení** tooopen vaše **Moje nastavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="618ee-187">From your user menu, select **My Settings** tooopen your **My Settings** page.</span></span>
3. <span data-ttu-id="618ee-188">V levém podokně hello, klikněte na **osobní** tooexpand hello osobní části a pak klikněte na **resetovat Moje zabezpečení tokenu**:</span><span class="sxs-lookup"><span data-stu-id="618ee-188">In hello left pane, click **Personal** tooexpand hello Personal section, and then click **Reset My Security Token**:</span></span>
   
   <span data-ttu-id="618ee-189">![Nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="618ee-189">![My Settings](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "My Settings")</span></span>
4. <span data-ttu-id="618ee-190">Na hello **resetovat Moje zabezpečení tokenu** klikněte na tlačítko **resetovat tokenu zabezpečení** toorequest e-mailu, který obsahuje váš token zabezpečení Salesforce.com.</span><span class="sxs-lookup"><span data-stu-id="618ee-190">On hello **Reset My Security Token** page, click **Reset Security Token** toorequest an email that contains your Salesforce.com security token.</span></span>
   
   <span data-ttu-id="618ee-191">![Nový Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "nový Token")</span><span class="sxs-lookup"><span data-stu-id="618ee-191">![New Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "New Token")</span></span>
5. <span data-ttu-id="618ee-192">Zkontrolujte Doručená pošta e-mailu z Salesforce.com s "**Salesforce.com zabezpečení potvrzení**" jako předmět.</span><span class="sxs-lookup"><span data-stu-id="618ee-192">Check your email inbox for an email from Salesforce.com with “**salesforce.com.com security confirmation**” as subject.</span></span>
6. <span data-ttu-id="618ee-193">Zkontrolujte tato e-mailu a zkopírujte hello zabezpečení hodnota tokenu.</span><span class="sxs-lookup"><span data-stu-id="618ee-193">Review this email and copy hello security token value.</span></span>
7. <span data-ttu-id="618ee-194">V portálu Azure classic, na hello hello **salesforce izolovaného prostoru** stránky integrace aplikací, klikněte na tlačítko **konfiguraci zřizování uživatelů** tooopen hello **konfiguraci zřizování uživatelů**dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="618ee-194">In hello Azure classic portal, on hello **salesforce Sandbox** application integration page, click **Configure user provisioning** tooopen hello **Configure User Provisioning** dialog.</span></span>
   
   <span data-ttu-id="618ee-195">![Konfiguraci zřizování uživatelů](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "konfiguraci zřizování uživatelů")</span><span class="sxs-lookup"><span data-stu-id="618ee-195">![Configure user provisioning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Configure user provisioning")</span></span>
8. <span data-ttu-id="618ee-196">Na hello **zadejte vaše zřizování automatické uživatelů izolovaného prostoru Salesforce pověření tooenable** zadejte hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="618ee-196">On hello **Enter your Salesforce Sandbox credentials tooenable automatic user provisioning** page, provide hello following configuration settings:</span></span>
   
   <span data-ttu-id="618ee-197">![Izolovaný prostor Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "izolovaného prostoru Salesforce")</span><span class="sxs-lookup"><span data-stu-id="618ee-197">![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")</span></span>   
 1. <span data-ttu-id="618ee-198">V hello **uživatelské jméno správce izolovaného prostoru Salesforce** textovému poli, zadejte název, který má hello účtu Salesforce izolovaného **správce systému** profil v Salesforce.com přiřazen.</span><span class="sxs-lookup"><span data-stu-id="618ee-198">In hello **Salesforce Sandbox Admin User Name** textbox, type a Salesforce sandbox account name that has hello **System Administrator** profile in Salesforce.com assigned.</span></span>
 2. <span data-ttu-id="618ee-199">V hello **heslo správce izolovaného prostoru Salesforce** textovému poli, zadejte hello heslo pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="618ee-199">In hello **Salesforce Sandbox Admin Password** textbox, type hello password for this account.</span></span>
 3. <span data-ttu-id="618ee-200">V hello **tokenu zabezpečení uživatele** textovému poli, vložte hello hodnota tokenu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="618ee-200">In hello **User Security Token** textbox, paste hello security token value.</span></span>
 4. <span data-ttu-id="618ee-201">Klikněte na tlačítko **ověřením** tooverify konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="618ee-201">Click **Validate** tooverify your configuration.</span></span>
 5. <span data-ttu-id="618ee-202">Klikněte na tlačítko hello **Další** tlačítko tooopen hello **potvrzení** stránky.</span><span class="sxs-lookup"><span data-stu-id="618ee-202">Click hello **Next** button tooopen hello **Confirmation** page.</span></span>
9. <span data-ttu-id="618ee-203">Na hello **potvrzení** klikněte na tlačítko **Complete** toosave konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="618ee-203">On hello **Confirmation** page, click **Complete** toosave your configuration.</span></span>
   
## <a name="assigning-users"></a><span data-ttu-id="618ee-204">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="618ee-204">Assigning users</span></span>

<span data-ttu-id="618ee-205">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="618ee-205">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="618ee-206">**tooassign uživatelé tooSalesforce izolovaného prostoru, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="618ee-206">**tooassign users tooSalesforce Sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="618ee-207">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="618ee-207">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="618ee-208">Na hello ** izolovaného prostoru Salesforce ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="618ee-208">On hello **Salesforce Sandbox **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="618ee-209">![Přiřazení uživatelů](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="618ee-209">![Assign users](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Assign users")</span></span>
3. <span data-ttu-id="618ee-210">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="618ee-210">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="618ee-211">![Ano](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="618ee-211">![Yes](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="618ee-212">Teď by měla Počkejte 10 minut a ověřte, zda text hello účet byl synchronizované tooSalesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="618ee-212">You should now wait for 10 minutes and verify that hello account has been synchronized tooSalesforce Sandbox.</span></span>

<span data-ttu-id="618ee-213">Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="618ee-213">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="618ee-214">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="618ee-214">For more details about hello Access Panel, see [Introduction toohello Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

