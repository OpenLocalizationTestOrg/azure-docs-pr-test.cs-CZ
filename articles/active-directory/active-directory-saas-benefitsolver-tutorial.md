---
title: 'Kurz: Azure Active Directory integrace s Benefitsolver | Microsoft Docs'
description: "Zjistěte, jak toouse Benefitsolver s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
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
ms.openlocfilehash: 5bb8511ef9be1e386956188a93e899d6ebe56ed5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a><span data-ttu-id="46798-103">Kurz: Azure Active Directory integrace s Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="46798-103">Tutorial: Azure Active Directory integration with Benefitsolver</span></span>
<span data-ttu-id="46798-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="46798-104">hello objective of this tutorial is tooshow hello integration of Azure and Benefitsolver.</span></span>  

<span data-ttu-id="46798-105">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="46798-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="46798-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="46798-106">A valid Azure subscription</span></span>
* <span data-ttu-id="46798-107">Benefitsolver jednotné přihlašování (SSO) povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="46798-107">A Benefitsolver single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="46798-108">Po dokončení tohoto kurzu, budou uživatelé hello Azure AD jste přiřadili tooBenefitsolver možné toosingle přihlášení do aplikace hello pomocí hello [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="46798-108">After completing this tutorial, hello Azure AD users you have assigned tooBenefitsolver will be able toosingle sign into hello application using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="46798-109">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="46798-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="46798-110">Povolení integrace aplikace hello pro Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="46798-110">Enabling hello application integration for Benefitsolver</span></span>
2. <span data-ttu-id="46798-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="46798-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="46798-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="46798-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="46798-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="46798-113">Assigning users</span></span>

<span data-ttu-id="46798-114">![Scénář](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="46798-114">![Scenario](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-benefitsolver"></a><span data-ttu-id="46798-115">Povolení integrace aplikace hello pro Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="46798-115">Enabling hello application integration for Benefitsolver</span></span>
<span data-ttu-id="46798-116">Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="46798-116">hello objective of this section is toooutline how tooenable hello application integration for Benefitsolver.</span></span>

### <a name="tooenable-hello-application-integration-for-benefitsolver-perform-hello-following-steps"></a><span data-ttu-id="46798-117">Integrace aplikace hello tooenable pro Benefitsolver, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="46798-117">tooenable hello application integration for Benefitsolver, perform hello following steps:</span></span>
1. <span data-ttu-id="46798-118">V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="46798-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="46798-119">![Služby Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="46798-119">![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="46798-120">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="46798-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="46798-121">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="46798-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="46798-122">![Aplikace](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="46798-122">![Applications](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="46798-123">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="46798-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="46798-124">![Přidat aplikaci](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="46798-124">![Add application](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="46798-125">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="46798-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="46798-126">![Přidání aplikace z gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="46798-126">![Add an application from gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="46798-127">V hello **vyhledávacího pole**, typ **Benefitsolver**.</span><span class="sxs-lookup"><span data-stu-id="46798-127">In hello **search box**, type **Benefitsolver**.</span></span>
   
   <span data-ttu-id="46798-128">![Galerie aplikací](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="46798-128">![Application Gallery](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Application Gallery")</span></span>
7. <span data-ttu-id="46798-129">V podokně výsledků hello, vyberte **Benefitsolver**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="46798-129">In hello results pane, select **Benefitsolver**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="46798-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span><span class="sxs-lookup"><span data-stu-id="46798-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="46798-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="46798-131">Configure single sign-on</span></span>

<span data-ttu-id="46798-132">Hello cílem této části je toooutline jak tooBenefitsolver tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="46798-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooBenefitsolver with their account in Azure AD using federation based on hello SAML protocol.</span></span>  

<span data-ttu-id="46798-133">Aplikace Benefitsolver očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour **atributy tokenu saml** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="46798-133">Your Benefitsolver application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **saml token attributes** configuration.</span></span> 

<span data-ttu-id="46798-134">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="46798-134">hello following screenshot shows an example for this.</span></span>

<span data-ttu-id="46798-135">![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="46798-135">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>

<span data-ttu-id="46798-136">**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="46798-136">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="46798-137">V portálu Azure classic, na hello hello **Benefitsolver** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="46798-137">In hello Azure classic portal, on hello **Benefitsolver** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="46798-138">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="46798-138">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="46798-139">Na hello **jak jste by například uživatelé toosign na tooBenefitsolver** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="46798-139">On hello **How would you like users toosign on tooBenefitsolver** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="46798-140">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="46798-140">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="46798-141">Na hello **nakonfigurovat nastavení aplikace** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="46798-141">On hello **Configure App Settings** page, perform hello following steps:</span></span>
   
   <span data-ttu-id="46798-142">![Konfigurovat nastavení aplikace](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "konfigurovat nastavení aplikace")</span><span class="sxs-lookup"><span data-stu-id="46798-142">![Configure App Settings](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Configure App Settings")</span></span>
   
   1. <span data-ttu-id="46798-143">V hello **přihlašovací adresa URL** textovému poli, typ **http://azure.benefitsolver.com**.</span><span class="sxs-lookup"><span data-stu-id="46798-143">In hello **Sign On URL** textbox, type **http://azure.benefitsolver.com**.</span></span>
   2. <span data-ttu-id="46798-144">V hello **adresa URL odpovědi** textovému poli, typ **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span><span class="sxs-lookup"><span data-stu-id="46798-144">In hello **Reply URL** textbox, type **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span></span>  
   3. <span data-ttu-id="46798-145">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="46798-145">Click **Next**.</span></span>
4. <span data-ttu-id="46798-146">Na hello **nakonfigurovat jednotné přihlašování v Benefitsolver** stránky, toodownload metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat hello místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="46798-146">On hello **Configure single sign-on at Benefitsolver** page, toodownload your metadata, click **Download metadata**, and then save hello metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="46798-147">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="46798-147">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="46798-148">Odešlete hello stáhnout metadata souboru tooyour Benefitsolver tým podpory.</span><span class="sxs-lookup"><span data-stu-id="46798-148">Send hello downloaded metadata file tooyour Benefitsolver support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="46798-149">Váš tým podpory Benefitsolver má toodo hello skutečné jednotné přihlašování konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="46798-149">Your Benefitsolver support team has toodo hello actual SSO configuration.</span></span> <span data-ttu-id="46798-150">Zobrazí se oznámení, když bylo povoleno jednotné přihlašování pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="46798-150">You will get a notification when SSO has been enabled for your subscription.</span></span>
   >

6. <span data-ttu-id="46798-151">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="46798-151">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="46798-152">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="46798-152">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="46798-153">V nabídce hello hello nahoře, klikněte na tlačítko **atributy** tooopen hello **atributy tokenu SAML** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="46798-153">In hello menu on hello top, click **Attributes** tooopen hello **SAML Token Attributes** dialog.</span></span>
   
   <span data-ttu-id="46798-154">![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="46798-154">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attributes")</span></span>
8. <span data-ttu-id="46798-155">mapování atributů tooadd hello vyžaduje, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="46798-155">tooadd hello required attribute mappings, perform hello following steps:</span></span>
   
   <span data-ttu-id="46798-156">![Atributy](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="46798-156">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>
   
   | <span data-ttu-id="46798-157">Název atributu</span><span class="sxs-lookup"><span data-stu-id="46798-157">Attribute Name</span></span> | <span data-ttu-id="46798-158">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="46798-158">Attribute Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="46798-159">ClientID</span><span class="sxs-lookup"><span data-stu-id="46798-159">ClientID</span></span> |<span data-ttu-id="46798-160">Tooget musíte z váš tým podpory Benefitsolver tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="46798-160">You need tooget this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="46798-161">ClientKey</span><span class="sxs-lookup"><span data-stu-id="46798-161">ClientKey</span></span> |<span data-ttu-id="46798-162">Tooget musíte z váš tým podpory Benefitsolver tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="46798-162">You need tooget this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="46798-163">LogoutURL</span><span class="sxs-lookup"><span data-stu-id="46798-163">LogoutURL</span></span> |<span data-ttu-id="46798-164">Tooget musíte z váš tým podpory Benefitsolver tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="46798-164">You need tooget this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="46798-165">Číslo zaměstnance</span><span class="sxs-lookup"><span data-stu-id="46798-165">EmployeeID</span></span> |<span data-ttu-id="46798-166">Tooget musíte z váš tým podpory Benefitsolver tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="46798-166">You need tooget this value from your Benefitsolver support team.</span></span> |
   
   1. <span data-ttu-id="46798-167">Pro každý řádek dat v tabulce hello výše, klikněte na tlačítko **přidat atribut uživatele**.</span><span class="sxs-lookup"><span data-stu-id="46798-167">For each data row in hello table above, click **add user attribute**.</span></span>
   2. <span data-ttu-id="46798-168">V hello **název atributu** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="46798-168">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
   3. <span data-ttu-id="46798-169">V hello **hodnota atributu** textovému poli, hodnota atributu vyberte hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="46798-169">In hello **Attribute Value** textbox, select hello attribute value shown for that row.</span></span>
   4. <span data-ttu-id="46798-170">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="46798-170">Click **Complete**.</span></span>
9. <span data-ttu-id="46798-171">Klikněte na tlačítko **změny**.</span><span class="sxs-lookup"><span data-stu-id="46798-171">Click **Apply Changes**.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="46798-172">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="46798-172">Configure user provisioning</span></span>
<span data-ttu-id="46798-173">V pořadí tooenable Azure AD Uživatelé toolog do Benefitsolver musí být zřízená do Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="46798-173">In order tooenable Azure AD users toolog into Benefitsolver, they must be provisioned into Benefitsolver.</span></span>  

<span data-ttu-id="46798-174">V případě hello Benefitsolver zaměstnanec data jsou v aplikaci naplněno prostřednictvím soubor úplné zjišťování z vašeho systému HRIS (obvykle je každou noc).</span><span class="sxs-lookup"><span data-stu-id="46798-174">In hello case of Benefitsolver, employee data is in your application populated through a Census file from your HRIS system (typically nightly).</span></span>  

>[!NOTE]
><span data-ttu-id="46798-175">Můžete použít všechny ostatní Benefitsolver uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Benefitsolver tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="46798-175">You can use any other Benefitsolver user account creation tools or APIs provided by Benefitsolver tooprovision AAD user accounts.</span></span> 
> 

## <a name="assigning-users"></a><span data-ttu-id="46798-176">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="46798-176">Assigning users</span></span>
<span data-ttu-id="46798-177">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="46798-177">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

### <a name="tooassign-users-toobenefitsolver-perform-hello-following-steps"></a><span data-ttu-id="46798-178">tooassign tooBenefitsolver uživatele, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="46798-178">tooassign users tooBenefitsolver, perform hello following steps:</span></span>
1. <span data-ttu-id="46798-179">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="46798-179">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="46798-180">Na hello ** Benefitsolver ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="46798-180">On hello **Benefitsolver **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="46798-181">![Přiřazení uživatelů](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="46798-181">![Assign Users](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Assign Users")</span></span>
3. <span data-ttu-id="46798-182">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="46798-182">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="46798-183">![Ano](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="46798-183">![Yes](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="46798-184">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="46798-184">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="46798-185">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="46798-185">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

