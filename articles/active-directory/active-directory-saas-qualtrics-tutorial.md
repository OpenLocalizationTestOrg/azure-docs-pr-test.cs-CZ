---
title: 'Kurz: Azure Active Directory integrace s Qualtrics | Microsoft Docs'
description: "Další informace o použití Qualtrics s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2fcde595a40dafda7549f5bccb582b57585b314e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a><span data-ttu-id="657a7-103">Kurz: Azure Active Directory integrace s Qualtrics</span><span class="sxs-lookup"><span data-stu-id="657a7-103">Tutorial: Azure Active Directory integration with Qualtrics</span></span>
<span data-ttu-id="657a7-104">Cílem tohoto kurzu je zobrazit integraci Azure a Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="657a7-104">The objective of this tutorial is to show the integration of Azure and Qualtrics.</span></span>  

<span data-ttu-id="657a7-105">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="657a7-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="657a7-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="657a7-106">A valid Azure subscription</span></span>
* <span data-ttu-id="657a7-107">Qualtrics jednotné přihlašování (SSO) povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="657a7-107">A Qualtrics single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="657a7-108">Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace ve vaší lokalitě společnosti Qualtrics (služba Zprostředkovatel iniciované přihlašování) nebo pomocí uživatele Azure AD, které jste přiřadili Qualtrics [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="657a7-108">After completing this tutorial, the Azure AD users you have assigned to Qualtrics will be able to single sign into the application at your Qualtrics company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="657a7-109">Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="657a7-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="657a7-110">Povolení integrace aplikace pro Qualtrics</span><span class="sxs-lookup"><span data-stu-id="657a7-110">Enabling the application integration for Qualtrics</span></span>
2. <span data-ttu-id="657a7-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="657a7-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="657a7-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="657a7-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="657a7-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="657a7-113">Assigning users</span></span>

<span data-ttu-id="657a7-114">![Scénář](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="657a7-114">![Scenario](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-qualtrics"></a><span data-ttu-id="657a7-115">Povolení integrace aplikace pro Qualtrics</span><span class="sxs-lookup"><span data-stu-id="657a7-115">Enabling the application integration for Qualtrics</span></span>
<span data-ttu-id="657a7-116">Cílem této části se popisují postup povolení integrace aplikace pro Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="657a7-116">The objective of this section is to outline how to enable the application integration for Qualtrics.</span></span>

<span data-ttu-id="657a7-117">**Pokud chcete povolit integraci aplikací pro Qualtrics, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="657a7-117">**To enable the application integration for Qualtrics, perform the following steps:**</span></span>

1. <span data-ttu-id="657a7-118">V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="657a7-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="657a7-119">![Služby Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="657a7-119">![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="657a7-120">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="657a7-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="657a7-121">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="657a7-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="657a7-122">![Aplikace](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="657a7-122">![Applications](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="657a7-123">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="657a7-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="657a7-124">![Přidat aplikaci](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="657a7-124">![Add application](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="657a7-125">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="657a7-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="657a7-126">![Přidání aplikace z gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="657a7-126">![Add an application from gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="657a7-127">V **vyhledávacího pole**, typ **Qualtrics**.</span><span class="sxs-lookup"><span data-stu-id="657a7-127">In the **search box**, type **Qualtrics**.</span></span>
   
   <span data-ttu-id="657a7-128">![Galerie aplikací](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="657a7-128">![Application Gallery](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Application Gallery")</span></span>
7. <span data-ttu-id="657a7-129">V podokně výsledků vyberte **Qualtrics**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="657a7-129">In the results pane, select **Qualtrics**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="657a7-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span><span class="sxs-lookup"><span data-stu-id="657a7-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="657a7-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="657a7-131">Configure single sign-on</span></span>

<span data-ttu-id="657a7-132">Cílem této části se popisují, jak uživatelům povolit ověřování na Qualtrics ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="657a7-132">The objective of this section is to outline how to enable users to authenticate to Qualtrics with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="657a7-133">**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="657a7-133">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="657a7-134">Na portálu Azure classic na **Qualtrics** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="657a7-134">In the Azure classic portal, on the **Qualtrics** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="657a7-135">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="657a7-135">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="657a7-136">Na **jak chcete uživatelům se přihlásit Qualtrics** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="657a7-136">On the **How would you like users to sign on to Qualtrics** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="657a7-137">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="657a7-137">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="657a7-138">Na **konfigurace adresy URL aplikace** stránky v **Qualtrics přihlašovací adresa URL** textovému poli, zadejte adresu URL (například: "*https://ssotest2ut1.qualtrics.com*") a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="657a7-138">On the **Configure App URL** page, in the **Qualtrics Sign On URL** textbox, type your URL (e.g.: “*https://ssotest2ut1.qualtrics.com*"), and then click **Next**.</span></span>
   
   <span data-ttu-id="657a7-139">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="657a7-139">![Configure App URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Configure App URL")</span></span>
4. <span data-ttu-id="657a7-140">Na **nakonfigurovat jednotné přihlašování v Qualtrics** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="657a7-140">On the **Configure single sign-on at Qualtrics** page, click **Download metadata**, and then save the metadata file on your computer.</span></span>
   
   <span data-ttu-id="657a7-141">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="657a7-141">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="657a7-142">Odešlete soubor metadat Qualtrics tým podpory.</span><span class="sxs-lookup"><span data-stu-id="657a7-142">Send the metadata file to the Qualtrics support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="657a7-143">Konfigurace jednotného přihlašování musí být provedena Qualtrics tým podpory.</span><span class="sxs-lookup"><span data-stu-id="657a7-143">The SSO configuration has to be performed by the Qualtrics support team.</span></span> <span data-ttu-id="657a7-144">Zobrazí se oznámení a také konfigurace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="657a7-144">You will get a notification as soon as the configuration has been completed.</span></span>
   > 
   > 
6. <span data-ttu-id="657a7-145">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="657a7-145">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="657a7-146">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="657a7-146">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="657a7-147">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="657a7-147">Configure user provisioning</span></span>

<span data-ttu-id="657a7-148">Neexistuje žádná položka akce můžete nakonfigurovat na Qualtrics zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="657a7-148">There is no action item for you to configure user provisioning to Qualtrics.</span></span> <span data-ttu-id="657a7-149">Když přiřazený uživatel se pokusí přihlásit pomocí přístupového panelu Qualtrics, Qualtrics ověří, zda uživatel existuje.</span><span class="sxs-lookup"><span data-stu-id="657a7-149">When an assigned user tries to log into Qualtrics using the access panel, Qualtrics checks whether the user exists.</span></span>  

<span data-ttu-id="657a7-150">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="657a7-150">If there is no user account available yet, it is automatically created by Qualtrics.</span></span>

## <a name="assign-users"></a><span data-ttu-id="657a7-151">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="657a7-151">Assign users</span></span>
<span data-ttu-id="657a7-152">Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="657a7-152">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="657a7-153">**Přiřazení uživatelů k Qualtrics, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="657a7-153">**To assign users to Qualtrics, perform the following steps:**</span></span>

1. <span data-ttu-id="657a7-154">Na portálu Azure classic vytvořte zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="657a7-154">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="657a7-155">Na **Qualtrics** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="657a7-155">On the **Qualtrics** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="657a7-156">![Přiřazení uživatelů](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="657a7-156">![Assign Users](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Assign Users")</span></span>
3. <span data-ttu-id="657a7-157">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.</span><span class="sxs-lookup"><span data-stu-id="657a7-157">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="657a7-158">![Ano](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="657a7-158">![Yes](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="657a7-159">Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="657a7-159">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="657a7-160">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="657a7-160">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

