---
title: "Kurz: Azure Active Directory integrace s TOPdesk - zabezpečené | Microsoft Docs"
description: "Další informace o použití TOPdesk - zabezpečit pomocí služby Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!."
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
ms.openlocfilehash: 28f0542dbe87bb34c83a7852db7c3a9fef055ce9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a><span data-ttu-id="b76ea-103">Kurz: Azure Active Directory integrace s TOPdesk - zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b76ea-103">Tutorial: Azure Active Directory integration with TOPdesk - Secure</span></span>
<span data-ttu-id="b76ea-104">Cílem tohoto kurzu je zobrazit integraci Azure a TOPdesk - zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="b76ea-104">The objective of this tutorial is to show the integration of Azure and TOPdesk - Secure.</span></span>  
<span data-ttu-id="b76ea-105">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="b76ea-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="b76ea-106">A valid Azure subscription</span></span>
* <span data-ttu-id="b76ea-107">A TOPdesk - zabezpečené jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b76ea-107">A TOPdesk - Secure single sign-on enabled subscription</span></span>

<span data-ttu-id="b76ea-108">Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace v TOPdesk - zabezpečené podnikové lokality (služba Zprostředkovatel iniciované přihlašování) nebo pomocí uživatele Azure AD, který jste přiřadili k TOPdesk - zabezpečené [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b76ea-108">After completing this tutorial, the Azure AD users you have assigned to TOPdesk - Secure will be able to single sign into the application at your TOPdesk - Secure company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="b76ea-109">Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="b76ea-110">Povolení integrace aplikace pro TOPdesk - zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b76ea-110">Enabling the application integration for TOPdesk - Secure</span></span>
2. <span data-ttu-id="b76ea-111">Konfigurace jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b76ea-111">Configuring single sign-on</span></span>
3. <span data-ttu-id="b76ea-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="b76ea-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="b76ea-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="b76ea-113">Assigning users</span></span>

<span data-ttu-id="b76ea-114">![Scénář](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="b76ea-114">![Scenario](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-topdesk---secure"></a><span data-ttu-id="b76ea-115">Povolení integrace aplikace pro TOPdesk - zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b76ea-115">Enabling the application integration for TOPdesk - Secure</span></span>
<span data-ttu-id="b76ea-116">Cílem této části se popisují postup povolení integrace aplikace pro TOPdesk - zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="b76ea-116">The objective of this section is to outline how to enable the application integration for TOPdesk - Secure.</span></span>

### <a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a><span data-ttu-id="b76ea-117">Chcete-li povolit integraci aplikace pro TOPdesk – zabezpečení, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-117">To enable the application integration for TOPdesk - Secure, perform the following steps:</span></span>
1. <span data-ttu-id="b76ea-118">V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="b76ea-119">![Služby Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="b76ea-119">![Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")</span></span>

2. <span data-ttu-id="b76ea-120">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="b76ea-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="b76ea-121">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="b76ea-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="b76ea-122">![Aplikace](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="b76ea-122">![Applications](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Applications")</span></span>

4. <span data-ttu-id="b76ea-123">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="b76ea-123">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="b76ea-124">![Přidat aplikaci](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="b76ea-124">![Add application](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Add application")</span></span>

5. <span data-ttu-id="b76ea-125">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="b76ea-126">![Přidání aplikace z gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="b76ea-126">![Add an application from gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Add an application from gallerry")</span></span>

6. <span data-ttu-id="b76ea-127">V **vyhledávacího pole**, typ **TOPdesk - zabezpečené**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-127">In the **search box**, type **TOPdesk - Secure**.</span></span>
   
    <span data-ttu-id="b76ea-128">![Galerie aplikací](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="b76ea-128">![Application Gallery](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Application Gallery")</span></span>

7. <span data-ttu-id="b76ea-129">V podokně výsledků vyberte **TOPdesk - zabezpečené**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="b76ea-129">In the results pane, select **TOPdesk - Secure**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="b76ea-130">![TOPdesk - zabezpečené](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="b76ea-130">![TOPdesk - Secure](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - Secure")</span></span>

## <a name="configuring-single-sign-on"></a><span data-ttu-id="b76ea-131">Konfigurace jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b76ea-131">Configuring single sign-on</span></span>
<span data-ttu-id="b76ea-132">Cílem této části se popisují, jak uživatelům povolit ověřování na TOPdesk - zabezpečit pomocí svého účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="b76ea-132">The objective of this section is to outline how to enable users to authenticate to TOPdesk - Secure with their account in Azure AD using federation based on the SAML protocol.</span></span>  
<span data-ttu-id="b76ea-133">Konfigurace jednotného přihlašování pro TOPdesk – zabezpečení vyžaduje, abyste nahrát soubor logo ikonu.</span><span class="sxs-lookup"><span data-stu-id="b76ea-133">Configuring single sign-on for TOPdesk - Secure requires you to upload a logo icon file.</span></span> <span data-ttu-id="b76ea-134">Chcete-li získat soubor ikony, kontaktujte tým podpory TOPdesk.</span><span class="sxs-lookup"><span data-stu-id="b76ea-134">To get the icon file, contact the TOPdesk support team.</span></span>

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a><span data-ttu-id="b76ea-135">Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-135">To configure single sign-on, perform the following steps:</span></span>
1. <span data-ttu-id="b76ea-136">Přihlaste se k vaší **TOPdesk - zabezpečené** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b76ea-136">Sign on to your **TOPdesk - Secure** company site as an administrator.</span></span>
2. <span data-ttu-id="b76ea-137">V **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-137">In the **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="b76ea-138">![Nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="b76ea-138">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

3. <span data-ttu-id="b76ea-139">Klikněte na tlačítko **nastavení přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-139">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="b76ea-140">![Nastavení přihlášení](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "nastavení přihlášení")</span><span class="sxs-lookup"><span data-stu-id="b76ea-140">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

4. <span data-ttu-id="b76ea-141">Rozbalte **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-141">Expand the **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="b76ea-142">![Obecné](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "obecné")</span><span class="sxs-lookup"><span data-stu-id="b76ea-142">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

5. <span data-ttu-id="b76ea-143">V **zabezpečeného** části **SAML přihlášení** konfigurace části, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-143">In the **Secure** section of the **SAML login** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="b76ea-144">![Technické nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "technické nastavení")</span><span class="sxs-lookup"><span data-stu-id="b76ea-144">![Technical Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technical Settings")</span></span>
   
    <span data-ttu-id="b76ea-145">a.</span><span class="sxs-lookup"><span data-stu-id="b76ea-145">a.</span></span> <span data-ttu-id="b76ea-146">Klikněte na tlačítko **Stáhnout** ke stažení souboru metadat veřejné a poté je uložit místně v počítači.</span><span class="sxs-lookup"><span data-stu-id="b76ea-146">Click **Download** to download the public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="b76ea-147">b.</span><span class="sxs-lookup"><span data-stu-id="b76ea-147">b.</span></span> <span data-ttu-id="b76ea-148">Otevřete soubor metadat a vyhledejte **AssertionConsumerService** uzlu.</span><span class="sxs-lookup"><span data-stu-id="b76ea-148">Open the metadata file, and then locate the **AssertionConsumerService** node.</span></span>
    
    <span data-ttu-id="b76ea-149">![Kontrolní výraz příjemce služby](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion příjemce služby")</span><span class="sxs-lookup"><span data-stu-id="b76ea-149">![Assertion Consumer Service](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion Consumer Service")</span></span>
   
    <span data-ttu-id="b76ea-150">c.</span><span class="sxs-lookup"><span data-stu-id="b76ea-150">c.</span></span> <span data-ttu-id="b76ea-151">Kopírování **AssertionConsumerService** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b76ea-151">Copy the **AssertionConsumerService** value.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="b76ea-152">Budete potřebovat hodnotu v **konfigurace adresy URL aplikace** později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b76ea-152">You will need the value in the **Configure App URL** section later in this tutorial.</span></span>
    > 
    > 

6. <span data-ttu-id="b76ea-153">V okně prohlížeče jiný web, přihlaste se k vaší **portál Azure classic** jako správce.</span><span class="sxs-lookup"><span data-stu-id="b76ea-153">In a different web browser window, log into your **Azure classic portal** as an administrator.</span></span>

7. <span data-ttu-id="b76ea-154">Na **TOPdesk - zabezpečené** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete ** nakonfigurovat jednotné přihlašování ** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b76ea-154">On the **TOPdesk - Secure** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="b76ea-155">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="b76ea-155">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Configure Single Sign-On")</span></span>

8. <span data-ttu-id="b76ea-156">Na **jak chcete uživatelům se přihlásit TOPdesk - zabezpečené** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-156">On the **How would you like users to sign on to TOPdesk - Secure** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="b76ea-157">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="b76ea-157">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Configure Single Sign-On")</span></span>

9. <span data-ttu-id="b76ea-158">Na **konfigurace adresy URL aplikace** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-158">On the **Configure App URL** page, perform the following steps:</span></span>
   
    <span data-ttu-id="b76ea-159">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="b76ea-159">![Configure App URL](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Configure App URL")</span></span>
   
    <span data-ttu-id="b76ea-160">a.</span><span class="sxs-lookup"><span data-stu-id="b76ea-160">a.</span></span> <span data-ttu-id="b76ea-161">V **TOPdesk - zabezpečené přihlašovací adresa URL** textové pole, zadejte adresu URL používají vaši uživatelé pro přihlášení do vaší TOPdesk - zabezpečené aplikace (například: "*https://qssolutions.topdesk.net*").</span><span class="sxs-lookup"><span data-stu-id="b76ea-161">In the **TOPdesk - Secure Sign On URL** textbox, type the URL used by your users to sign into your TOPdesk - Secure application (e.g.: "*https://qssolutions.topdesk.net*").</span></span>
   
    <span data-ttu-id="b76ea-162">b.</span><span class="sxs-lookup"><span data-stu-id="b76ea-162">b.</span></span> <span data-ttu-id="b76ea-163">V **TOPdesk – veřejná adresa URL odpovědi** textovému poli, Vložit **TOPdesk - zabezpečené URL AssertionConsumerService** (například: "*https://qssolutions.topdesk.net/tas/public/login/saml*")</span><span class="sxs-lookup"><span data-stu-id="b76ea-163">In the **TOPdesk – Public Reply URL** textbox, paste the **TOPdesk - Secure AssertionConsumerService URL** (e.g.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")</span></span>
   
    <span data-ttu-id="b76ea-164">c.</span><span class="sxs-lookup"><span data-stu-id="b76ea-164">c.</span></span> <span data-ttu-id="b76ea-165">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-165">Click **Next**.</span></span>

10. <span data-ttu-id="b76ea-166">Na **nakonfigurovat jednotné přihlašování v TOPdesk - zabezpečené** klikněte na stránce pro stažení souboru metadat **stáhnout metadata**a potom uložte soubor místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b76ea-166">On the **Configure single sign-on at TOPdesk - Secure** page, to download your metadata file, click **Download metadata**, and then save the file locally on your computer.</span></span>
    
    <span data-ttu-id="b76ea-167">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="b76ea-167">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Configure Single Sign-On")</span></span>

11. <span data-ttu-id="b76ea-168">Pokud chcete vytvořit soubor s certifikátem, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-168">To create a certificate file, perform the following steps:</span></span>
    
    <span data-ttu-id="b76ea-169">![Certifikát](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "certifikátu")</span><span class="sxs-lookup"><span data-stu-id="b76ea-169">![Certificate](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Certificate")</span></span>
    
    <span data-ttu-id="b76ea-170">a.</span><span class="sxs-lookup"><span data-stu-id="b76ea-170">a.</span></span> <span data-ttu-id="b76ea-171">Otevřete soubor stažený metadat.</span><span class="sxs-lookup"><span data-stu-id="b76ea-171">Open the downloaded metadata file.</span></span>
    <span data-ttu-id="b76ea-172">b.</span><span class="sxs-lookup"><span data-stu-id="b76ea-172">b.</span></span> <span data-ttu-id="b76ea-173">Rozbalte **RoleDescriptor** uzlu, který má **xsi: type** z **dodáni: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-173">Expand the **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    <span data-ttu-id="b76ea-174">c.</span><span class="sxs-lookup"><span data-stu-id="b76ea-174">c.</span></span> <span data-ttu-id="b76ea-175">Zkopírujte hodnotu **certifikátu x 509** uzlu.</span><span class="sxs-lookup"><span data-stu-id="b76ea-175">Copy the value of the **X509Certificate** node.</span></span>
    <span data-ttu-id="b76ea-176">d.</span><span class="sxs-lookup"><span data-stu-id="b76ea-176">d.</span></span> <span data-ttu-id="b76ea-177">Uložit zkopírovaný **certifikátu x 509** hodnota místně na vašem počítači v souboru.</span><span class="sxs-lookup"><span data-stu-id="b76ea-177">Save the copied **X509Certificate** value locally on your computer in a file.</span></span>

12. <span data-ttu-id="b76ea-178">Na vaše TOPdesk – zabezpečení společnosti lokality v **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-178">On your TOPdesk - Secure company site, in the **TOPdesk** menu, click **Settings**.</span></span>
    
    <span data-ttu-id="b76ea-179">![Nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="b76ea-179">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

13. <span data-ttu-id="b76ea-180">Klikněte na tlačítko **nastavení přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-180">Click **Login Settings**.</span></span>
    
    <span data-ttu-id="b76ea-181">![Nastavení přihlášení](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "nastavení přihlášení")</span><span class="sxs-lookup"><span data-stu-id="b76ea-181">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

14. <span data-ttu-id="b76ea-182">Rozbalte **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-182">Expand the **Login Settings** menu, and then click **General**.</span></span>
    
    <span data-ttu-id="b76ea-183">![Obecné](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "obecné")</span><span class="sxs-lookup"><span data-stu-id="b76ea-183">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

15. <span data-ttu-id="b76ea-184">V **veřejné** klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-184">In the **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="b76ea-185">![Přidat](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="b76ea-185">![Add](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Add")</span></span>

16. <span data-ttu-id="b76ea-186">Na **pomocníka konfigurace SAML** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-186">On the **SAML configuration assistant** dialog page, perform the following steps:</span></span>
    
    <span data-ttu-id="b76ea-187">![Pomocník pro konfigurace SAML](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "pomocníka konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="b76ea-187">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="b76ea-188">a.</span><span class="sxs-lookup"><span data-stu-id="b76ea-188">a.</span></span> <span data-ttu-id="b76ea-189">K odeslání souboru stažené metadata, v části **federačních metadat**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-189">To upload your downloaded metadata file, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="b76ea-190">b.</span><span class="sxs-lookup"><span data-stu-id="b76ea-190">b.</span></span> <span data-ttu-id="b76ea-191">Nahrát soubor certifikátu, v části **certifikát (RSA)**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-191">To upload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="b76ea-192">c.</span><span class="sxs-lookup"><span data-stu-id="b76ea-192">c.</span></span> <span data-ttu-id="b76ea-193">Nahrát soubor logo jste získali od týmu podpory TOPdesk, v části **Logo ikonu**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-193">To upload the logo file you got from the TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="b76ea-194">d.</span><span class="sxs-lookup"><span data-stu-id="b76ea-194">d.</span></span> <span data-ttu-id="b76ea-195">V **atribut uživatelského jména** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-195">In the **User name attribute** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="b76ea-196">e.</span><span class="sxs-lookup"><span data-stu-id="b76ea-196">e.</span></span> <span data-ttu-id="b76ea-197">V **zobrazovaný název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b76ea-197">In the **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="b76ea-198">f.</span><span class="sxs-lookup"><span data-stu-id="b76ea-198">f.</span></span> <span data-ttu-id="b76ea-199">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-199">Click **Save**.</span></span>

17. <span data-ttu-id="b76ea-200">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b76ea-200">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="b76ea-201">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="b76ea-201">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Configure Single Sign-On")</span></span>

## <a name="configuring-user-provisioning"></a><span data-ttu-id="b76ea-202">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="b76ea-202">Configuring user provisioning</span></span>
<span data-ttu-id="b76ea-203">Chcete-li povolit uživatelům Azure AD přihlášení do TOPdesk - bezpečný, se musí být zřízená do TOPdesk - zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="b76ea-203">In order to enable Azure AD users to log into TOPdesk - Secure, they must be provisioned into TOPdesk - Secure.</span></span>  
<span data-ttu-id="b76ea-204">V případě TOPdesk - zabezpečený a zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b76ea-204">In the case of TOPdesk - Secure, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="b76ea-205">Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-205">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="b76ea-206">Přihlaste se k vaší **TOPdesk - zabezpečené** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b76ea-206">Sign on to your **TOPdesk - Secure** company site as administrator.</span></span>
2. <span data-ttu-id="b76ea-207">V nabídce v horní části, klikněte na tlačítko **TOPdesk \> nový \> soubory podpory \> operátor**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-207">In the menu on the top, click **TOPdesk \> New \> Support Files \> Operator**.</span></span>
   
    <span data-ttu-id="b76ea-208">![Operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "– operátor")</span><span class="sxs-lookup"><span data-stu-id="b76ea-208">![Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")</span></span>

3. <span data-ttu-id="b76ea-209">Na **operátor New** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-209">On the **New Operator** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="b76ea-210">![Operátor new](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New – operátor")</span><span class="sxs-lookup"><span data-stu-id="b76ea-210">![New Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New Operator")</span></span>
   
    <span data-ttu-id="b76ea-211">a.</span><span class="sxs-lookup"><span data-stu-id="b76ea-211">a.</span></span> <span data-ttu-id="b76ea-212">Klikněte na kartu Obecné.</span><span class="sxs-lookup"><span data-stu-id="b76ea-212">Click the General tab.</span></span>
   
    <span data-ttu-id="b76ea-213">b.</span><span class="sxs-lookup"><span data-stu-id="b76ea-213">b.</span></span> <span data-ttu-id="b76ea-214">V **Přezdívka** textbox z **Obecné** zadejte příjmení chcete zřídit platný účet služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b76ea-214">In the **Surname** textbox of the **General** section, type the last name of a valid Azure Active Directory account you want to provision.</span></span>
   
    <span data-ttu-id="b76ea-215">c.</span><span class="sxs-lookup"><span data-stu-id="b76ea-215">c.</span></span> <span data-ttu-id="b76ea-216">Vyberte **lokality** pro účet v **umístění** části.</span><span class="sxs-lookup"><span data-stu-id="b76ea-216">Select a **Site** for the account in the **Location** section.</span></span>
   
    <span data-ttu-id="b76ea-217">d.</span><span class="sxs-lookup"><span data-stu-id="b76ea-217">d.</span></span> <span data-ttu-id="b76ea-218">V **přihlašovací jméno** textbox z **TOPdesk přihlášení** zadejte přihlašovací jméno uživatele.</span><span class="sxs-lookup"><span data-stu-id="b76ea-218">In the **Login Name** textbox of the **TOPdesk Login** section, type a login name for your user.</span></span>
   
    <span data-ttu-id="b76ea-219">e.</span><span class="sxs-lookup"><span data-stu-id="b76ea-219">e.</span></span> <span data-ttu-id="b76ea-220">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-220">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="b76ea-221">Můžete použít všechny ostatní TOPdesk – rozhraní API poskytované TOPdesk - zabezpečené zřídit AAD uživatelské účty nebo nástroje pro tvorbu účet zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="b76ea-221">You can use any other TOPdesk - Secure user account creation tools or APIs provided by TOPdesk - Secure to provision AAD user accounts.</span></span>
> 
> 

## <a name="assigning-users"></a><span data-ttu-id="b76ea-222">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="b76ea-222">Assigning users</span></span>
<span data-ttu-id="b76ea-223">Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="b76ea-223">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

### <a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a><span data-ttu-id="b76ea-224">Přiřazení uživatelů k TOPdesk - zabezpečit, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ea-224">To assign users to TOPdesk - Secure, perform the following steps:</span></span>
1. <span data-ttu-id="b76ea-225">Na portálu Azure classic vytvořte zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="b76ea-225">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="b76ea-226">Na ** TOPdesk - zabezpečené ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="b76ea-226">On the **TOPdesk - Secure **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="b76ea-227">![Přiřazení uživatelů](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b76ea-227">![Assign Users](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Assign Users")</span></span>

3. <span data-ttu-id="b76ea-228">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.</span><span class="sxs-lookup"><span data-stu-id="b76ea-228">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
    <span data-ttu-id="b76ea-229">![Ano](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="b76ea-229">![Yes](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="b76ea-230">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="b76ea-230">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="b76ea-231">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b76ea-231">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

