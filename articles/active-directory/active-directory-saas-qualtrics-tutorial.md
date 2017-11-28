---
title: 'Kurz: Azure Active Directory integrace s Qualtrics | Microsoft Docs'
description: "Zjistěte, jak toouse Qualtrics s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
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
ms.openlocfilehash: 941642e74b90bb13a5ce37ce6665cfa32cd86016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a><span data-ttu-id="ab361-103">Kurz: Azure Active Directory integrace s Qualtrics</span><span class="sxs-lookup"><span data-stu-id="ab361-103">Tutorial: Azure Active Directory integration with Qualtrics</span></span>
<span data-ttu-id="ab361-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="ab361-104">hello objective of this tutorial is tooshow hello integration of Azure and Qualtrics.</span></span>  

<span data-ttu-id="ab361-105">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ab361-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="ab361-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="ab361-106">A valid Azure subscription</span></span>
* <span data-ttu-id="ab361-107">Qualtrics jednotné přihlašování (SSO) povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ab361-107">A Qualtrics single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="ab361-108">Po dokončení tohoto kurzu, budou uživatelé hello Azure AD jste přiřadili tooQualtrics možné toosingle přihlášení do aplikace hello na váš web společnosti Qualtrics (služba Zprostředkovatel iniciované přihlašování) nebo pomocí hello [toohello Úvod Přístup k panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ab361-108">After completing this tutorial, hello Azure AD users you have assigned tooQualtrics will be able toosingle sign into hello application at your Qualtrics company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="ab361-109">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="ab361-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="ab361-110">Povolení integrace aplikace hello pro Qualtrics</span><span class="sxs-lookup"><span data-stu-id="ab361-110">Enabling hello application integration for Qualtrics</span></span>
2. <span data-ttu-id="ab361-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="ab361-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="ab361-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="ab361-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="ab361-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="ab361-113">Assigning users</span></span>

<span data-ttu-id="ab361-114">![Scénář](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="ab361-114">![Scenario](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-qualtrics"></a><span data-ttu-id="ab361-115">Povolení integrace aplikace hello pro Qualtrics</span><span class="sxs-lookup"><span data-stu-id="ab361-115">Enabling hello application integration for Qualtrics</span></span>
<span data-ttu-id="ab361-116">Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="ab361-116">hello objective of this section is toooutline how tooenable hello application integration for Qualtrics.</span></span>

<span data-ttu-id="ab361-117">**Integrace aplikace hello tooenable pro Qualtrics, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ab361-117">**tooenable hello application integration for Qualtrics, perform hello following steps:**</span></span>

1. <span data-ttu-id="ab361-118">V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ab361-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="ab361-119">![Služby Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="ab361-119">![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="ab361-120">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="ab361-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="ab361-121">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ab361-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="ab361-122">![Aplikace](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="ab361-122">![Applications](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="ab361-123">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="ab361-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="ab361-124">![Přidat aplikaci](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="ab361-124">![Add application](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="ab361-125">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="ab361-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="ab361-126">![Přidání aplikace z gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="ab361-126">![Add an application from gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="ab361-127">V hello **vyhledávacího pole**, typ **Qualtrics**.</span><span class="sxs-lookup"><span data-stu-id="ab361-127">In hello **search box**, type **Qualtrics**.</span></span>
   
   <span data-ttu-id="ab361-128">![Galerie aplikací](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="ab361-128">![Application Gallery](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Application Gallery")</span></span>
7. <span data-ttu-id="ab361-129">V podokně výsledků hello, vyberte **Qualtrics**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ab361-129">In hello results pane, select **Qualtrics**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="ab361-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span><span class="sxs-lookup"><span data-stu-id="ab361-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="ab361-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ab361-131">Configure single sign-on</span></span>

<span data-ttu-id="ab361-132">Hello cílem této části je toooutline jak tooQualtrics tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="ab361-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooQualtrics with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="ab361-133">**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ab361-133">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="ab361-134">V portálu Azure classic, na hello hello **Qualtrics** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ab361-134">In hello Azure classic portal, on hello **Qualtrics** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="ab361-135">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ab361-135">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="ab361-136">Na hello **jak jste by například uživatelé toosign na tooQualtrics** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ab361-136">On hello **How would you like users toosign on tooQualtrics** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="ab361-137">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ab361-137">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="ab361-138">Na hello **konfigurace adresy URL aplikace** stránku hello **Qualtrics přihlašovací adresa URL** textovému poli, zadejte adresu URL (například: "*https://ssotest2ut1.qualtrics.com*") a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ab361-138">On hello **Configure App URL** page, in hello **Qualtrics Sign On URL** textbox, type your URL (e.g.: “*https://ssotest2ut1.qualtrics.com*"), and then click **Next**.</span></span>
   
   <span data-ttu-id="ab361-139">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="ab361-139">![Configure App URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Configure App URL")</span></span>
4. <span data-ttu-id="ab361-140">Na hello **nakonfigurovat jednotné přihlašování v Qualtrics** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ab361-140">On hello **Configure single sign-on at Qualtrics** page, click **Download metadata**, and then save hello metadata file on your computer.</span></span>
   
   <span data-ttu-id="ab361-141">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ab361-141">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="ab361-142">Odešlete hello metadata souboru toohello Qualtrics tým podpory.</span><span class="sxs-lookup"><span data-stu-id="ab361-142">Send hello metadata file toohello Qualtrics support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="ab361-143">Konfigurace jednotného přihlašování k Hello má toobe provádí hello Qualtrics tým podpory.</span><span class="sxs-lookup"><span data-stu-id="ab361-143">hello SSO configuration has toobe performed by hello Qualtrics support team.</span></span> <span data-ttu-id="ab361-144">Zobrazí se oznámení a také konfigurace hello se dokončila.</span><span class="sxs-lookup"><span data-stu-id="ab361-144">You will get a notification as soon as hello configuration has been completed.</span></span>
   > 
   > 
6. <span data-ttu-id="ab361-145">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ab361-145">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="ab361-146">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="ab361-146">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="ab361-147">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="ab361-147">Configure user provisioning</span></span>

<span data-ttu-id="ab361-148">Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooQualtrics.</span><span class="sxs-lookup"><span data-stu-id="ab361-148">There is no action item for you tooconfigure user provisioning tooQualtrics.</span></span> <span data-ttu-id="ab361-149">Když se uživatel s přiřazenou pokusí toolog do Qualtrics pomocí hello přístupového panelu, Qualtrics ověří, zda existuje hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ab361-149">When an assigned user tries toolog into Qualtrics using hello access panel, Qualtrics checks whether hello user exists.</span></span>  

<span data-ttu-id="ab361-150">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Qualtrics.</span><span class="sxs-lookup"><span data-stu-id="ab361-150">If there is no user account available yet, it is automatically created by Qualtrics.</span></span>

## <a name="assign-users"></a><span data-ttu-id="ab361-151">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="ab361-151">Assign users</span></span>
<span data-ttu-id="ab361-152">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="ab361-152">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="ab361-153">**tooassign tooQualtrics uživatele, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ab361-153">**tooassign users tooQualtrics, perform hello following steps:**</span></span>

1. <span data-ttu-id="ab361-154">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="ab361-154">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="ab361-155">Na hello **Qualtrics** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="ab361-155">On hello **Qualtrics** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="ab361-156">![Přiřazení uživatelů](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="ab361-156">![Assign Users](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Assign Users")</span></span>
3. <span data-ttu-id="ab361-157">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="ab361-157">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="ab361-158">![Ano](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="ab361-158">![Yes](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="ab361-159">Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ab361-159">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="ab361-160">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ab361-160">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

