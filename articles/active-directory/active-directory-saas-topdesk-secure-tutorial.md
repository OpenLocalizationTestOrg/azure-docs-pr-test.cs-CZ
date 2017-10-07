---
title: "Kurz: Azure Active Directory integrace s TOPdesk - zabezpečené | Microsoft Docs"
description: "Zjistěte, jak toouse TOPdesk - zabezpečit pomocí služby Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 10fe420d1691c2845b89c779486ffd6fcd736432
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a><span data-ttu-id="ad2fe-103">Kurz: Azure Active Directory integrace s TOPdesk - zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ad2fe-103">Tutorial: Azure Active Directory integration with TOPdesk - Secure</span></span>
<span data-ttu-id="ad2fe-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a TOPdesk – zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-104">hello objective of this tutorial is tooshow hello integration of Azure and TOPdesk - Secure.</span></span>  
<span data-ttu-id="ad2fe-105">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="ad2fe-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="ad2fe-106">A valid Azure subscription</span></span>
* <span data-ttu-id="ad2fe-107">A TOPdesk - zabezpečené jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ad2fe-107">A TOPdesk - Secure single sign-on enabled subscription</span></span>

<span data-ttu-id="ad2fe-108">Po dokončení tohoto kurzu, hello Azure AD uživatelům přiřadili jste tooTOPdesk - zabezpečené bude být schopný toosingle přihlášení do aplikace hello na vaše TOPdesk - zabezpečené podnikové lokality (služba Zprostředkovatel iniciované přihlašování), nebo pomocí hello [Úvod toohello přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ad2fe-108">After completing this tutorial, hello Azure AD users you have assigned tooTOPdesk - Secure will be able toosingle sign into hello application at your TOPdesk - Secure company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="ad2fe-109">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="ad2fe-110">Povolení integrace aplikace hello pro TOPdesk - zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ad2fe-110">Enabling hello application integration for TOPdesk - Secure</span></span>
2. <span data-ttu-id="ad2fe-111">Konfigurace jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ad2fe-111">Configuring single sign-on</span></span>
3. <span data-ttu-id="ad2fe-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="ad2fe-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="ad2fe-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="ad2fe-113">Assigning users</span></span>

<span data-ttu-id="ad2fe-114">![Scénář](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-114">![Scenario](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-topdesk---secure"></a><span data-ttu-id="ad2fe-115">Povolení integrace aplikace hello pro TOPdesk - zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ad2fe-115">Enabling hello application integration for TOPdesk - Secure</span></span>
<span data-ttu-id="ad2fe-116">Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro TOPdesk - zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-116">hello objective of this section is toooutline how tooenable hello application integration for TOPdesk - Secure.</span></span>

### <a name="tooenable-hello-application-integration-for-topdesk---secure-perform-hello-following-steps"></a><span data-ttu-id="ad2fe-117">Integrace aplikace hello tooenable pro TOPdesk - bezpečný, proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-117">tooenable hello application integration for TOPdesk - Secure, perform hello following steps:</span></span>
1. <span data-ttu-id="ad2fe-118">V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="ad2fe-119">![Služby Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-119">![Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")</span></span>

2. <span data-ttu-id="ad2fe-120">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="ad2fe-121">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="ad2fe-122">![Aplikace](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-122">![Applications](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Applications")</span></span>

4. <span data-ttu-id="ad2fe-123">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-123">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="ad2fe-124">![Přidat aplikaci](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-124">![Add application](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Add application")</span></span>

5. <span data-ttu-id="ad2fe-125">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="ad2fe-126">![Přidání aplikace z gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-126">![Add an application from gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Add an application from gallerry")</span></span>

6. <span data-ttu-id="ad2fe-127">V hello **vyhledávacího pole**, typ **TOPdesk - zabezpečené**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-127">In hello **search box**, type **TOPdesk - Secure**.</span></span>
   
    <span data-ttu-id="ad2fe-128">![Galerie aplikací](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-128">![Application Gallery](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Application Gallery")</span></span>

7. <span data-ttu-id="ad2fe-129">V podokně výsledků hello, vyberte **TOPdesk - zabezpečené**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-129">In hello results pane, select **TOPdesk - Secure**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="ad2fe-130">![TOPdesk - zabezpečené](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-130">![TOPdesk - Secure](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - Secure")</span></span>

## <a name="configuring-single-sign-on"></a><span data-ttu-id="ad2fe-131">Konfigurace jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ad2fe-131">Configuring single sign-on</span></span>
<span data-ttu-id="ad2fe-132">Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooTOPdesk - zabezpečit pomocí svého účtu ve službě Azure AD využívající federaci podle hello protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooTOPdesk - Secure with their account in Azure AD using federation based on hello SAML protocol.</span></span>  
<span data-ttu-id="ad2fe-133">Konfigurace jednotného přihlašování pro TOPdesk – zabezpečení vyžaduje, abyste tooupload soubor logo ikonu.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-133">Configuring single sign-on for TOPdesk - Secure requires you tooupload a logo icon file.</span></span> <span data-ttu-id="ad2fe-134">tooget hello soubor ikony, tým podpory TOPdesk kontaktní hello.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-134">tooget hello icon file, contact hello TOPdesk support team.</span></span>

### <a name="tooconfigure-single-sign-on-perform-hello-following-steps"></a><span data-ttu-id="ad2fe-135">tooconfigure jednotné přihlašování, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-135">tooconfigure single sign-on, perform hello following steps:</span></span>
1. <span data-ttu-id="ad2fe-136">Přihlaste se tooyour **TOPdesk - zabezpečené** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-136">Sign on tooyour **TOPdesk - Secure** company site as an administrator.</span></span>
2. <span data-ttu-id="ad2fe-137">V hello **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-137">In hello **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="ad2fe-138">![Nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-138">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

3. <span data-ttu-id="ad2fe-139">Klikněte na tlačítko **nastavení přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-139">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="ad2fe-140">![Nastavení přihlášení](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "nastavení přihlášení")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-140">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

4. <span data-ttu-id="ad2fe-141">Rozbalte hello **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-141">Expand hello **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="ad2fe-142">![Obecné](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "obecné")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-142">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

5. <span data-ttu-id="ad2fe-143">V hello **zabezpečeného** části hello **SAML přihlášení** konfigurace části, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-143">In hello **Secure** section of hello **SAML login** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="ad2fe-144">![Technické nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "technické nastavení")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-144">![Technical Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technical Settings")</span></span>
   
    <span data-ttu-id="ad2fe-145">a.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-145">a.</span></span> <span data-ttu-id="ad2fe-146">Klikněte na tlačítko **Stáhnout** toodownload hello veřejné metadata souboru a poté je uložit místně v počítači.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-146">Click **Download** toodownload hello public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="ad2fe-147">b.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-147">b.</span></span> <span data-ttu-id="ad2fe-148">Otevřete soubor metadat hello a vyhledejte hello **AssertionConsumerService** uzlu.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-148">Open hello metadata file, and then locate hello **AssertionConsumerService** node.</span></span>
    
    <span data-ttu-id="ad2fe-149">![Kontrolní výraz příjemce služby](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion příjemce služby")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-149">![Assertion Consumer Service](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion Consumer Service")</span></span>
   
    <span data-ttu-id="ad2fe-150">c.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-150">c.</span></span> <span data-ttu-id="ad2fe-151">Kopírování hello **AssertionConsumerService** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-151">Copy hello **AssertionConsumerService** value.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="ad2fe-152">Bude nutné hello hodnota v hello **konfigurace adresy URL aplikace** později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-152">You will need hello value in hello **Configure App URL** section later in this tutorial.</span></span>
    > 
    > 

6. <span data-ttu-id="ad2fe-153">V okně prohlížeče jiný web, přihlaste se k vaší **portál Azure classic** jako správce.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-153">In a different web browser window, log into your **Azure classic portal** as an administrator.</span></span>

7. <span data-ttu-id="ad2fe-154">Na hello **TOPdesk - zabezpečené** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello ** nakonfigurovat jednotné přihlašování ** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-154">On hello **TOPdesk - Secure** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="ad2fe-155">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-155">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Configure Single Sign-On")</span></span>

8. <span data-ttu-id="ad2fe-156">Na hello **jak by vám jako uživatelé toosign na tooTOPdesk - Secure** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-156">On hello **How would you like users toosign on tooTOPdesk - Secure** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="ad2fe-157">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-157">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Configure Single Sign-On")</span></span>

9. <span data-ttu-id="ad2fe-158">Na hello **konfigurace adresy URL aplikace** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-158">On hello **Configure App URL** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="ad2fe-159">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-159">![Configure App URL](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Configure App URL")</span></span>
   
    <span data-ttu-id="ad2fe-160">a.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-160">a.</span></span> <span data-ttu-id="ad2fe-161">V hello **TOPdesk - zabezpečené přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše toosign uživatelů do vašeho TOPdesk - zabezpečené aplikace (například: "*https://qssolutions.topdesk.net*").</span><span class="sxs-lookup"><span data-stu-id="ad2fe-161">In hello **TOPdesk - Secure Sign On URL** textbox, type hello URL used by your users toosign into your TOPdesk - Secure application (e.g.: "*https://qssolutions.topdesk.net*").</span></span>
   
    <span data-ttu-id="ad2fe-162">b.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-162">b.</span></span> <span data-ttu-id="ad2fe-163">V hello **TOPdesk – veřejná adresa URL odpovědi** textovému poli, vložte hello **TOPdesk - zabezpečené URL AssertionConsumerService** (například: "*https://qssolutions.topdesk.net/tas/public/login/saml*")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-163">In hello **TOPdesk – Public Reply URL** textbox, paste hello **TOPdesk - Secure AssertionConsumerService URL** (e.g.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")</span></span>
   
    <span data-ttu-id="ad2fe-164">c.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-164">c.</span></span> <span data-ttu-id="ad2fe-165">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-165">Click **Next**.</span></span>

10. <span data-ttu-id="ad2fe-166">Na hello **nakonfigurovat jednotné přihlašování v TOPdesk - zabezpečené** stránky, toodownload souboru metadat klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-166">On hello **Configure single sign-on at TOPdesk - Secure** page, toodownload your metadata file, click **Download metadata**, and then save hello file locally on your computer.</span></span>
    
    <span data-ttu-id="ad2fe-167">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-167">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Configure Single Sign-On")</span></span>

11. <span data-ttu-id="ad2fe-168">toocreate soubor certifikátu, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-168">toocreate a certificate file, perform hello following steps:</span></span>
    
    <span data-ttu-id="ad2fe-169">![Certifikát](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "certifikátu")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-169">![Certificate](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Certificate")</span></span>
    
    <span data-ttu-id="ad2fe-170">a.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-170">a.</span></span> <span data-ttu-id="ad2fe-171">Otevřete hello soubor stažený metadat.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-171">Open hello downloaded metadata file.</span></span>
    <span data-ttu-id="ad2fe-172">b.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-172">b.</span></span> <span data-ttu-id="ad2fe-173">Rozbalte hello **RoleDescriptor** uzlu, který má **xsi: type** z **dodáni: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-173">Expand hello **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    <span data-ttu-id="ad2fe-174">c.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-174">c.</span></span> <span data-ttu-id="ad2fe-175">Zkopírujte hodnotu hello hello **certifikátu x 509** uzlu.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-175">Copy hello value of hello **X509Certificate** node.</span></span>
    <span data-ttu-id="ad2fe-176">d.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-176">d.</span></span> <span data-ttu-id="ad2fe-177">Uložit hello zkopírovat **certifikátu x 509** hodnota místně na vašem počítači v souboru.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-177">Save hello copied **X509Certificate** value locally on your computer in a file.</span></span>

12. <span data-ttu-id="ad2fe-178">Na vaše TOPdesk – zabezpečení společnosti lokality v hello **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-178">On your TOPdesk - Secure company site, in hello **TOPdesk** menu, click **Settings**.</span></span>
    
    <span data-ttu-id="ad2fe-179">![Nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-179">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

13. <span data-ttu-id="ad2fe-180">Klikněte na tlačítko **nastavení přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-180">Click **Login Settings**.</span></span>
    
    <span data-ttu-id="ad2fe-181">![Nastavení přihlášení](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "nastavení přihlášení")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-181">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

14. <span data-ttu-id="ad2fe-182">Rozbalte hello **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-182">Expand hello **Login Settings** menu, and then click **General**.</span></span>
    
    <span data-ttu-id="ad2fe-183">![Obecné](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "obecné")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-183">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

15. <span data-ttu-id="ad2fe-184">V hello **veřejné** klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-184">In hello **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="ad2fe-185">![Přidat](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-185">![Add](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Add")</span></span>

16. <span data-ttu-id="ad2fe-186">Na hello **pomocníka konfigurace SAML** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-186">On hello **SAML configuration assistant** dialog page, perform hello following steps:</span></span>
    
    <span data-ttu-id="ad2fe-187">![Pomocník pro konfigurace SAML](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "pomocníka konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-187">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="ad2fe-188">a.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-188">a.</span></span> <span data-ttu-id="ad2fe-189">tooupload stažené metadata souboru v části **federačních metadat**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-189">tooupload your downloaded metadata file, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="ad2fe-190">b.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-190">b.</span></span> <span data-ttu-id="ad2fe-191">tooupload svůj certifikát v části souboru **certifikát (RSA)**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-191">tooupload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="ad2fe-192">c.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-192">c.</span></span> <span data-ttu-id="ad2fe-193">Soubor loga hello tooupload jste získali od týmu podpory TOPdesk hello, v části **Logo ikonu**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-193">tooupload hello logo file you got from hello TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="ad2fe-194">d.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-194">d.</span></span> <span data-ttu-id="ad2fe-195">V hello **atribut uživatelského jména** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-195">In hello **User name attribute** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="ad2fe-196">e.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-196">e.</span></span> <span data-ttu-id="ad2fe-197">V hello **zobrazovaný název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-197">In hello **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="ad2fe-198">f.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-198">f.</span></span> <span data-ttu-id="ad2fe-199">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-199">Click **Save**.</span></span>

17. <span data-ttu-id="ad2fe-200">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-200">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="ad2fe-201">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-201">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Configure Single Sign-On")</span></span>

## <a name="configuring-user-provisioning"></a><span data-ttu-id="ad2fe-202">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="ad2fe-202">Configuring user provisioning</span></span>
<span data-ttu-id="ad2fe-203">V pořadí tooenable Azure AD Uživatelé toolog do TOPdesk - bezpečný, se musí být zřízená do TOPdesk - zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-203">In order tooenable Azure AD users toolog into TOPdesk - Secure, they must be provisioned into TOPdesk - Secure.</span></span>  
<span data-ttu-id="ad2fe-204">V případě hello TOPdesk - zabezpečený a zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-204">In hello case of TOPdesk - Secure, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="ad2fe-205">tooconfigure zřizování uživatelů, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-205">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="ad2fe-206">Přihlaste se tooyour **TOPdesk - zabezpečené** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-206">Sign on tooyour **TOPdesk - Secure** company site as administrator.</span></span>
2. <span data-ttu-id="ad2fe-207">V nabídce hello hello nahoře, klikněte na tlačítko **TOPdesk \> nový \> soubory podpory \> operátor**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-207">In hello menu on hello top, click **TOPdesk \> New \> Support Files \> Operator**.</span></span>
   
    <span data-ttu-id="ad2fe-208">![Operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "– operátor")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-208">![Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")</span></span>

3. <span data-ttu-id="ad2fe-209">Na hello **operátor New** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-209">On hello **New Operator** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="ad2fe-210">![Operátor new](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New – operátor")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-210">![New Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New Operator")</span></span>
   
    <span data-ttu-id="ad2fe-211">a.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-211">a.</span></span> <span data-ttu-id="ad2fe-212">Klikněte na kartu Obecné hello.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-212">Click hello General tab.</span></span>
   
    <span data-ttu-id="ad2fe-213">b.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-213">b.</span></span> <span data-ttu-id="ad2fe-214">V hello **Přezdívka** textbox hello **Obecné** části, typ hello příjmení chcete tooprovision platný účet služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-214">In hello **Surname** textbox of hello **General** section, type hello last name of a valid Azure Active Directory account you want tooprovision.</span></span>
   
    <span data-ttu-id="ad2fe-215">c.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-215">c.</span></span> <span data-ttu-id="ad2fe-216">Vyberte **lokality** pro účet hello v hello **umístění** části.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-216">Select a **Site** for hello account in hello **Location** section.</span></span>
   
    <span data-ttu-id="ad2fe-217">d.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-217">d.</span></span> <span data-ttu-id="ad2fe-218">V hello **přihlašovací jméno** textbox hello **TOPdesk přihlášení** zadejte přihlašovací jméno uživatele.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-218">In hello **Login Name** textbox of hello **TOPdesk Login** section, type a login name for your user.</span></span>
   
    <span data-ttu-id="ad2fe-219">e.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-219">e.</span></span> <span data-ttu-id="ad2fe-220">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-220">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="ad2fe-221">Můžete použít všechny ostatní TOPdesk - zadáním hodnoty zabezpečené uživatelský účet, nástroje pro tvorbu nebo rozhraní API poskytovaných TOPdesk - Secure tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-221">You can use any other TOPdesk - Secure user account creation tools or APIs provided by TOPdesk - Secure tooprovision AAD user accounts.</span></span>
> 
> 

## <a name="assigning-users"></a><span data-ttu-id="ad2fe-222">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="ad2fe-222">Assigning users</span></span>
<span data-ttu-id="ad2fe-223">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-223">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

### <a name="tooassign-users-tootopdesk---secure-perform-hello-following-steps"></a><span data-ttu-id="ad2fe-224">Uživatelé tooTOPdesk tooassign - zabezpečit, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ad2fe-224">tooassign users tooTOPdesk - Secure, perform hello following steps:</span></span>
1. <span data-ttu-id="ad2fe-225">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-225">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="ad2fe-226">Na hello ** TOPdesk - zabezpečené ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-226">On hello **TOPdesk - Secure **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="ad2fe-227">![Přiřazení uživatelů](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-227">![Assign Users](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Assign Users")</span></span>

3. <span data-ttu-id="ad2fe-228">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-228">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
    <span data-ttu-id="ad2fe-229">![Ano](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="ad2fe-229">![Yes](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="ad2fe-230">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ad2fe-230">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="ad2fe-231">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ad2fe-231">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

