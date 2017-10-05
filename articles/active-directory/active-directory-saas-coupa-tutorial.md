---
title: 'Kurz: Azure Active Directory integrace s Coupa | Microsoft Docs'
description: "Další informace o použití Coupa s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: c952975919cfd948f1b9ea93ff2ac2641a53f923
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a><span data-ttu-id="e115a-103">Kurz: Azure Active Directory integrace s Coupa</span><span class="sxs-lookup"><span data-stu-id="e115a-103">Tutorial: Azure Active Directory integration with Coupa</span></span>
<span data-ttu-id="e115a-104">Cílem tohoto kurzu je zobrazit integraci Azure a Coupa.</span><span class="sxs-lookup"><span data-stu-id="e115a-104">The objective of this tutorial is to show the integration of Azure and Coupa.</span></span>  
<span data-ttu-id="e115a-105">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="e115a-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="e115a-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="e115a-106">A valid Azure subscription</span></span>
* <span data-ttu-id="e115a-107">Coupa jednotné přihlašování (SSO) povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e115a-107">A Coupa single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="e115a-108">Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace pomocí uživatele Azure AD, které jste přiřadili Coupa [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e115a-108">After completing this tutorial, the Azure AD users you have assigned to Coupa will be able to single sign into the application using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="e115a-109">Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e115a-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

* <span data-ttu-id="e115a-110">Povolení integrace aplikace pro Coupa</span><span class="sxs-lookup"><span data-stu-id="e115a-110">Enabling the application integration for Coupa</span></span>
* <span data-ttu-id="e115a-111">Konfigurace jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e115a-111">Configuring single sign-on</span></span>
* <span data-ttu-id="e115a-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="e115a-112">Configuring user provisioning</span></span>
* <span data-ttu-id="e115a-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="e115a-113">Assigning users</span></span>

<span data-ttu-id="e115a-114">![Scénář](./media/active-directory-saas-coupa-tutorial/IC791897.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="e115a-114">![Scenario](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-coupa"></a><span data-ttu-id="e115a-115">Povolit integraci aplikace pro Coupa</span><span class="sxs-lookup"><span data-stu-id="e115a-115">Enable the application integration for Coupa</span></span>
<span data-ttu-id="e115a-116">Cílem této části se popisují postup povolení integrace aplikace pro Coupa.</span><span class="sxs-lookup"><span data-stu-id="e115a-116">The objective of this section is to outline how to enable the application integration for Coupa.</span></span>

<span data-ttu-id="e115a-117">**Pokud chcete povolit integraci aplikací pro Coupa, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e115a-117">**To enable the application integration for Coupa, perform the following steps:**</span></span>

1. <span data-ttu-id="e115a-118">V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e115a-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="e115a-119">![Služby Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="e115a-119">![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="e115a-120">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="e115a-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="e115a-121">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="e115a-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="e115a-122">![Aplikace](./media/active-directory-saas-coupa-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="e115a-122">![Applications](./media/active-directory-saas-coupa-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="e115a-123">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="e115a-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="e115a-124">![Přidat aplikaci](./media/active-directory-saas-coupa-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="e115a-124">![Add application](./media/active-directory-saas-coupa-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="e115a-125">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="e115a-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="e115a-126">![Přidání aplikace z gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="e115a-126">![Add an application from gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="e115a-127">V **vyhledávacího pole**, typ **Coupa**.</span><span class="sxs-lookup"><span data-stu-id="e115a-127">In the **search box**, type **Coupa**.</span></span>
   
   <span data-ttu-id="e115a-128">![Galerie aplikací](./media/active-directory-saas-coupa-tutorial/IC791898.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="e115a-128">![Application Gallery](./media/active-directory-saas-coupa-tutorial/IC791898.png "Application Gallery")</span></span>
7. <span data-ttu-id="e115a-129">V podokně výsledků vyberte **Coupa**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="e115a-129">In the results pane, select **Coupa**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="e115a-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span><span class="sxs-lookup"><span data-stu-id="e115a-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="e115a-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e115a-131">Configure single sign-on</span></span>

<span data-ttu-id="e115a-132">Cílem této části se popisují, jak uživatelům povolit ověřování na Coupa ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="e115a-132">The objective of this section is to outline how to enable users to authenticate to Coupa with their account in Azure AD using federation based on the SAML protocol.</span></span>  

<span data-ttu-id="e115a-133">Konfigurace jednotného přihlašování pro Coupa vyžaduje, abyste načíst hodnotu kryptografický otisk z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e115a-133">Configuring single sign-on for Coupa requires you to retrieve a thumbprint value from a certificate.</span></span> <span data-ttu-id="e115a-134">Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak načíst hodnoty kryptografického otisku certifikátu](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="e115a-134">If you are not familiar with this procedure, see [How to retrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span>

<span data-ttu-id="e115a-135">**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e115a-135">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="e115a-136">Přihlásit se k serveru vaší společnosti Coupa jako správce.</span><span class="sxs-lookup"><span data-stu-id="e115a-136">Sign on to your Coupa company site as an administrator.</span></span>
2. <span data-ttu-id="e115a-137">Přejděte na **instalace \> řízení zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="e115a-137">Go to **Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="e115a-138">![Ovládací prvky zabezpečení](./media/active-directory-saas-coupa-tutorial/IC791900.png "kontrolních mechanismů pro zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="e115a-138">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
3. <span data-ttu-id="e115a-139">Chcete-li stáhnout soubor metadat Coupa do počítače, klikněte na tlačítko **stahování a import metadat SP**.</span><span class="sxs-lookup"><span data-stu-id="e115a-139">To download the Coupa metadata file to your computer, click **Download and import SP metadata**.</span></span>
   
   <span data-ttu-id="e115a-140">![Metadata Coupa SP](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadat")</span><span class="sxs-lookup"><span data-stu-id="e115a-140">![Coupa SP metadata](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")</span></span>
4. <span data-ttu-id="e115a-141">V okně jiný prohlížeč Přihlaste se k portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="e115a-141">In a different browser window, sign on to the Azure classic portal.</span></span>
5. <span data-ttu-id="e115a-142">Na **Coupa** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e115a-142">On the **Coupa** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="e115a-143">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791902.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e115a-143">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791902.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="e115a-144">Na **jak chcete uživatelům se přihlásit Coupa** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e115a-144">On the **How would you like users to sign on to Coupa** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="e115a-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791903.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e115a-145">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791903.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="e115a-146">Na **konfigurace adresy URL aplikace** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e115a-146">On the **Configure App URL** page, perform the following steps:</span></span>
   
   <span data-ttu-id="e115a-147">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-coupa-tutorial/IC791904.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="e115a-147">![Configure App URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "Configure App URL")</span></span>   
   1. <span data-ttu-id="e115a-148">V **přihlašovací adresa URL** textovému poli, používá k přihlášení do aplikace Coupa vaši uživatelé zadat adresu URL (například: "*http://company.Coupa.com*").</span><span class="sxs-lookup"><span data-stu-id="e115a-148">In the **Sign On URL** textbox, type URL used by your users to sign on to your Coupa application (e.g.: “*http://company.Coupa.com*”).</span></span>
   2. <span data-ttu-id="e115a-149">Otevřete váš stažený soubor metadat Coupa a zkopírujte **AssertionConsumerService index nebo adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="e115a-149">Open your downloaded Coupa metadata file, and then copy the **AssertionConsumerService index/URL**.</span></span>
   3. <span data-ttu-id="e115a-150">V **adresa URL odpovědi Coupa** textovému poli, Vložit **AssertionConsumerService index nebo adresa URL** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e115a-150">In the **Coupa Reply URL** textbox, paste the **AssertionConsumerService index/URL** value.</span></span>
   4. <span data-ttu-id="e115a-151">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e115a-151">Click **Next**.</span></span>
8. <span data-ttu-id="e115a-152">Na **nakonfigurovat jednotné přihlašování v Coupa** klikněte na stránce pro stažení souboru metadat **stáhnout metadata**a potom uložte soubor místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e115a-152">On the **Configure single sign-on at Coupa** page, to download your metadata file, click **Download metadata**, and then save the file locally on your computer.</span></span>
   
   <span data-ttu-id="e115a-153">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791905.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e115a-153">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791905.png "Configure Single Sign-On")</span></span>
9. <span data-ttu-id="e115a-154">Na webu společnosti Coupa, přejděte na **instalace \> řízení zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="e115a-154">On the Coupa company site, go to **Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="e115a-155">![Ovládací prvky zabezpečení](./media/active-directory-saas-coupa-tutorial/IC791900.png "kontrolních mechanismů pro zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="e115a-155">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
10. <span data-ttu-id="e115a-156">V **přihlásit pomocí přihlašovacích údajů Coupa** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e115a-156">In the **Log in using Coupa credentials** section, perform the following steps:</span></span>  

   <span data-ttu-id="e115a-157">![Přihlaste se pomocí přihlašovacích údajů Coupa](./media/active-directory-saas-coupa-tutorial/IC791906.png "přihlásit pomocí přihlašovacích údajů Coupa")</span><span class="sxs-lookup"><span data-stu-id="e115a-157">![Log in using Coupa credentials](./media/active-directory-saas-coupa-tutorial/IC791906.png "Log in using Coupa credentials")</span></span> 
   1. <span data-ttu-id="e115a-158">Vyberte **Přihlaste se pomocí SAML**.</span><span class="sxs-lookup"><span data-stu-id="e115a-158">Select **Log in using SAML**.</span></span>
   2. <span data-ttu-id="e115a-159">Klikněte na tlačítko **Procházet** k odeslání stažené Azure Active metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="e115a-159">Click **Browse** to upload your downloaded Azure Active metadata file.</span></span>
   3. <span data-ttu-id="e115a-160">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e115a-160">Click **Save**.</span></span>
11. <span data-ttu-id="e115a-161">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e115a-161">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
   <span data-ttu-id="e115a-162">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791907.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e115a-162">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791907.png "Configure Single Sign-On")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="e115a-163">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="e115a-163">Configure user provisioning</span></span>

<span data-ttu-id="e115a-164">Pokud chcete povolit uživatelům Azure AD přihlášení do Coupa, musí být zřízená do Coupa.</span><span class="sxs-lookup"><span data-stu-id="e115a-164">In order to enable Azure AD users to log into Coupa, they must be provisioned into Coupa.</span></span>  

* <span data-ttu-id="e115a-165">V případě Coupa zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="e115a-165">In the case of Coupa, provisioning is a manual task.</span></span>

<span data-ttu-id="e115a-166">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e115a-166">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="e115a-167">Přihlaste se k vaší **Coupa** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="e115a-167">Log in to your **Coupa** company site as administrator.</span></span>
2. <span data-ttu-id="e115a-168">V nabídce v horní části, klikněte na tlačítko **instalace**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e115a-168">In the menu on the top, click **Setup**, and then click **Users**.</span></span>
   
   <span data-ttu-id="e115a-169">![Uživatelé](./media/active-directory-saas-coupa-tutorial/IC791908.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="e115a-169">![Users](./media/active-directory-saas-coupa-tutorial/IC791908.png "Users")</span></span>
3. <span data-ttu-id="e115a-170">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e115a-170">Click **Create**.</span></span>
   
   <span data-ttu-id="e115a-171">![Vytvoření uživatelů](./media/active-directory-saas-coupa-tutorial/IC791909.png "vytvoření uživatelů")</span><span class="sxs-lookup"><span data-stu-id="e115a-171">![Create Users](./media/active-directory-saas-coupa-tutorial/IC791909.png "Create Users")</span></span>
4. <span data-ttu-id="e115a-172">V **vytvořit uživateli** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e115a-172">In the **User Create** section, perform the following steps:</span></span>
   
   <span data-ttu-id="e115a-173">![Podrobné informace o uživateli](./media/active-directory-saas-coupa-tutorial/IC791910.png "podrobné informace o uživateli")</span><span class="sxs-lookup"><span data-stu-id="e115a-173">![User Details](./media/active-directory-saas-coupa-tutorial/IC791910.png "User Details")</span></span>
   
   1. <span data-ttu-id="e115a-174">Typ **přihlášení**, **křestní jméno**, **příjmení**, **ID přihlášení**, **e-mailu** atributy platný účet služby Azure Active Directory, který má být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="e115a-174">Type the **Login**, **First name**, **Last Name**, **Single Sign-On ID**, **Email** attributes of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   2. <span data-ttu-id="e115a-175">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e115a-175">Click **Create**.</span></span>   
   >[!NOTE]
   ><span data-ttu-id="e115a-176">Držitel účtu Azure Active Directory dostane e-mail s odkazem pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="e115a-176">The Azure Active Directory account holder will get an email with a link to confirm the account before it becomes active.</span></span> 
   > 

>[!NOTE]
><span data-ttu-id="e115a-177">Můžete použít všechny ostatní Coupa uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Coupa zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="e115a-177">You can use any other Coupa user account creation tools or APIs provided by Coupa to provision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="e115a-178">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="e115a-178">Assign users</span></span>
<span data-ttu-id="e115a-179">Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="e115a-179">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="e115a-180">**Přiřazení uživatelů k Coupa, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e115a-180">**To assign users to Coupa, perform the following steps:**</span></span>

1. <span data-ttu-id="e115a-181">Na portálu Azure classic vytvořte zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="e115a-181">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="e115a-182">Na ** Coupa ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="e115a-182">On the **Coupa **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="e115a-183">![Přiřazení uživatelů](./media/active-directory-saas-coupa-tutorial/IC791911.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="e115a-183">![Assign Users](./media/active-directory-saas-coupa-tutorial/IC791911.png "Assign Users")</span></span>
3. <span data-ttu-id="e115a-184">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.</span><span class="sxs-lookup"><span data-stu-id="e115a-184">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="e115a-185">![Ano](./media/active-directory-saas-coupa-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="e115a-185">![Yes](./media/active-directory-saas-coupa-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="e115a-186">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="e115a-186">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="e115a-187">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e115a-187">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

