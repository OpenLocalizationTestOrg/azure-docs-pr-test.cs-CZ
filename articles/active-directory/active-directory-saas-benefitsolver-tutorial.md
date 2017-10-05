---
title: 'Kurz: Azure Active Directory integrace s Benefitsolver | Microsoft Docs'
description: "Další informace o použití Benefitsolver s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: cf4529b1-3fb6-4475-82b7-2ceedcb70b3c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 8a13dd5ebd872f86247158379b28bc291a9c9d83
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a><span data-ttu-id="21ea7-103">Kurz: Azure Active Directory integrace s Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="21ea7-103">Tutorial: Azure Active Directory integration with Benefitsolver</span></span>
<span data-ttu-id="21ea7-104">Cílem tohoto kurzu je zobrazit integraci Azure a Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="21ea7-104">The objective of this tutorial is to show the integration of Azure and Benefitsolver.</span></span>  

<span data-ttu-id="21ea7-105">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="21ea7-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="21ea7-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="21ea7-106">A valid Azure subscription</span></span>
* <span data-ttu-id="21ea7-107">Benefitsolver jednotné přihlašování (SSO) povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="21ea7-107">A Benefitsolver single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="21ea7-108">Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace pomocí uživatele Azure AD, které jste přiřadili Benefitsolver [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="21ea7-108">After completing this tutorial, the Azure AD users you have assigned to Benefitsolver will be able to single sign into the application using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="21ea7-109">Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="21ea7-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="21ea7-110">Povolení integrace aplikace pro Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="21ea7-110">Enabling the application integration for Benefitsolver</span></span>
2. <span data-ttu-id="21ea7-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="21ea7-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="21ea7-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="21ea7-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="21ea7-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="21ea7-113">Assigning users</span></span>

<span data-ttu-id="21ea7-114">![Scénář](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="21ea7-114">![Scenario](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-benefitsolver"></a><span data-ttu-id="21ea7-115">Povolení integrace aplikace pro Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="21ea7-115">Enabling the application integration for Benefitsolver</span></span>
<span data-ttu-id="21ea7-116">Cílem této části se popisují postup povolení integrace aplikace pro Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="21ea7-116">The objective of this section is to outline how to enable the application integration for Benefitsolver.</span></span>

### <a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a><span data-ttu-id="21ea7-117">Pokud chcete povolit integraci aplikací pro Benefitsolver, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="21ea7-117">To enable the application integration for Benefitsolver, perform the following steps:</span></span>
1. <span data-ttu-id="21ea7-118">V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="21ea7-119">![Služby Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="21ea7-119">![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="21ea7-120">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="21ea7-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="21ea7-121">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="21ea7-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="21ea7-122">![Aplikace](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="21ea7-122">![Applications](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="21ea7-123">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="21ea7-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="21ea7-124">![Přidat aplikaci](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="21ea7-124">![Add application](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="21ea7-125">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="21ea7-126">![Přidání aplikace z gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="21ea7-126">![Add an application from gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="21ea7-127">V **vyhledávacího pole**, typ **Benefitsolver**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-127">In the **search box**, type **Benefitsolver**.</span></span>
   
   <span data-ttu-id="21ea7-128">![Galerie aplikací](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="21ea7-128">![Application Gallery](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Application Gallery")</span></span>
7. <span data-ttu-id="21ea7-129">V podokně výsledků vyberte **Benefitsolver**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="21ea7-129">In the results pane, select **Benefitsolver**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="21ea7-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span><span class="sxs-lookup"><span data-stu-id="21ea7-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="21ea7-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="21ea7-131">Configure single sign-on</span></span>

<span data-ttu-id="21ea7-132">Cílem této části se popisují, jak uživatelům povolit ověřování na Benefitsolver ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="21ea7-132">The objective of this section is to outline how to enable users to authenticate to Benefitsolver with their account in Azure AD using federation based on the SAML protocol.</span></span>  

<span data-ttu-id="21ea7-133">Aplikace Benefitsolver očekává SAML kontrolní výrazy ve specifickém formátu, který můžete přidat mapování vlastní atribut vyžaduje vaše **atributy tokenu saml** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="21ea7-133">Your Benefitsolver application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **saml token attributes** configuration.</span></span> 

<span data-ttu-id="21ea7-134">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="21ea7-134">The following screenshot shows an example for this.</span></span>

<span data-ttu-id="21ea7-135">![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="21ea7-135">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>

<span data-ttu-id="21ea7-136">**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="21ea7-136">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="21ea7-137">Na portálu Azure classic na **Benefitsolver** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="21ea7-137">In the Azure classic portal, on the **Benefitsolver** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="21ea7-138">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="21ea7-138">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="21ea7-139">Na **jak chcete uživatelům se přihlásit Benefitsolver** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-139">On the **How would you like users to sign on to Benefitsolver** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="21ea7-140">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="21ea7-140">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="21ea7-141">Na **nakonfigurovat nastavení aplikace** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="21ea7-141">On the **Configure App Settings** page, perform the following steps:</span></span>
   
   <span data-ttu-id="21ea7-142">![Konfigurovat nastavení aplikace](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "konfigurovat nastavení aplikace")</span><span class="sxs-lookup"><span data-stu-id="21ea7-142">![Configure App Settings](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Configure App Settings")</span></span>
   
   1. <span data-ttu-id="21ea7-143">V **přihlašovací adresa URL** textovému poli, typ **http://azure.benefitsolver.com**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-143">In the **Sign On URL** textbox, type **http://azure.benefitsolver.com**.</span></span>
   2. <span data-ttu-id="21ea7-144">V **adresa URL odpovědi** textovému poli, typ **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-144">In the **Reply URL** textbox, type **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span></span>  
   3. <span data-ttu-id="21ea7-145">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-145">Click **Next**.</span></span>
4. <span data-ttu-id="21ea7-146">Na **nakonfigurovat jednotné přihlašování v Benefitsolver** stránku a stáhnout metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="21ea7-146">On the **Configure single sign-on at Benefitsolver** page, to download your metadata, click **Download metadata**, and then save the metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="21ea7-147">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="21ea7-147">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="21ea7-148">Odešlete soubor stažený metadat pro váš tým podpory Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="21ea7-148">Send the downloaded metadata file to your Benefitsolver support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="21ea7-149">Váš tým podpory Benefitsolver musí provést konfiguraci skutečné jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="21ea7-149">Your Benefitsolver support team has to do the actual SSO configuration.</span></span> <span data-ttu-id="21ea7-150">Zobrazí se oznámení, když bylo povoleno jednotné přihlašování pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="21ea7-150">You will get a notification when SSO has been enabled for your subscription.</span></span>
   >

6. <span data-ttu-id="21ea7-151">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="21ea7-151">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="21ea7-152">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="21ea7-152">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="21ea7-153">V nabídce v horní části, klikněte na tlačítko **atributy** otevřete **atributy tokenu SAML** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="21ea7-153">In the menu on the top, click **Attributes** to open the **SAML Token Attributes** dialog.</span></span>
   
   <span data-ttu-id="21ea7-154">![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="21ea7-154">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attributes")</span></span>
8. <span data-ttu-id="21ea7-155">Pokud chcete přidat mapování požadovaný atribut, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="21ea7-155">To add the required attribute mappings, perform the following steps:</span></span>
   
   <span data-ttu-id="21ea7-156">![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="21ea7-156">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>
   
   | <span data-ttu-id="21ea7-157">Název atributu</span><span class="sxs-lookup"><span data-stu-id="21ea7-157">Attribute Name</span></span> | <span data-ttu-id="21ea7-158">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="21ea7-158">Attribute Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="21ea7-159">ClientID</span><span class="sxs-lookup"><span data-stu-id="21ea7-159">ClientID</span></span> |<span data-ttu-id="21ea7-160">Budete muset získat tuto hodnotu z váš tým podpory Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="21ea7-160">You need to get this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="21ea7-161">ClientKey</span><span class="sxs-lookup"><span data-stu-id="21ea7-161">ClientKey</span></span> |<span data-ttu-id="21ea7-162">Budete muset získat tuto hodnotu z váš tým podpory Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="21ea7-162">You need to get this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="21ea7-163">LogoutURL</span><span class="sxs-lookup"><span data-stu-id="21ea7-163">LogoutURL</span></span> |<span data-ttu-id="21ea7-164">Budete muset získat tuto hodnotu z váš tým podpory Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="21ea7-164">You need to get this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="21ea7-165">Číslo zaměstnance</span><span class="sxs-lookup"><span data-stu-id="21ea7-165">EmployeeID</span></span> |<span data-ttu-id="21ea7-166">Budete muset získat tuto hodnotu z váš tým podpory Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="21ea7-166">You need to get this value from your Benefitsolver support team.</span></span> |
   
   1. <span data-ttu-id="21ea7-167">Pro každý řádek dat v předchozí tabulce, klikněte na tlačítko **přidat atribut uživatele**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-167">For each data row in the table above, click **add user attribute**.</span></span>
   2. <span data-ttu-id="21ea7-168">V **název atributu** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="21ea7-168">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
   3. <span data-ttu-id="21ea7-169">V **hodnota atributu** textovému poli, vyberte hodnotu atributu zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="21ea7-169">In the **Attribute Value** textbox, select the attribute value shown for that row.</span></span>
   4. <span data-ttu-id="21ea7-170">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-170">Click **Complete**.</span></span>
9. <span data-ttu-id="21ea7-171">Klikněte na tlačítko **změny**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-171">Click **Apply Changes**.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="21ea7-172">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="21ea7-172">Configure user provisioning</span></span>
<span data-ttu-id="21ea7-173">Pokud chcete povolit uživatelům Azure AD přihlášení do Benefitsolver, musí být zřízená do Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="21ea7-173">In order to enable Azure AD users to log into Benefitsolver, they must be provisioned into Benefitsolver.</span></span>  

<span data-ttu-id="21ea7-174">V případě Benefitsolver zaměstnanec data jsou v aplikaci naplněno prostřednictvím soubor úplné zjišťování z vašeho systému HRIS (obvykle je každou noc).</span><span class="sxs-lookup"><span data-stu-id="21ea7-174">In the case of Benefitsolver, employee data is in your application populated through a Census file from your HRIS system (typically nightly).</span></span>  

>[!NOTE]
><span data-ttu-id="21ea7-175">Můžete použít všechny ostatní Benefitsolver uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Benefitsolver zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="21ea7-175">You can use any other Benefitsolver user account creation tools or APIs provided by Benefitsolver to provision AAD user accounts.</span></span> 
> 

## <a name="assigning-users"></a><span data-ttu-id="21ea7-176">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="21ea7-176">Assigning users</span></span>
<span data-ttu-id="21ea7-177">Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="21ea7-177">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

### <a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a><span data-ttu-id="21ea7-178">Přiřazení uživatelů k Benefitsolver, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="21ea7-178">To assign users to Benefitsolver, perform the following steps:</span></span>
1. <span data-ttu-id="21ea7-179">Na portálu Azure classic vytvořte zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="21ea7-179">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="21ea7-180">Na ** Benefitsolver ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="21ea7-180">On the **Benefitsolver **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="21ea7-181">![Přiřazení uživatelů](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="21ea7-181">![Assign Users](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Assign Users")</span></span>
3. <span data-ttu-id="21ea7-182">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.</span><span class="sxs-lookup"><span data-stu-id="21ea7-182">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="21ea7-183">![Ano](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="21ea7-183">![Yes](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="21ea7-184">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="21ea7-184">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="21ea7-185">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="21ea7-185">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

