---
title: "Kurz: Azure Active Directory integrace s životního cyklu SCC | Microsoft Docs"
description: "Další informace o použití životního cyklu SCC službou Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 9a30bcca720ff135d0180d73f46e78403e9bca43
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a><span data-ttu-id="293d8-103">Kurz: Azure Active Directory integrace s SCC životního cyklu</span><span class="sxs-lookup"><span data-stu-id="293d8-103">Tutorial: Azure Active Directory integration with SCC LifeCycle</span></span>
<span data-ttu-id="293d8-104">Cílem tohoto kurzu je zobrazit integraci Azure a SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="293d8-104">The objective of this tutorial is to show the integration of Azure and SCC LifeCycle.</span></span>  

<span data-ttu-id="293d8-105">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="293d8-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="293d8-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="293d8-106">A valid Azure subscription</span></span>
* <span data-ttu-id="293d8-107">Životního cyklu SCC jednotné přihlašování (SSO) povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="293d8-107">A SCC LifeCycle single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="293d8-108">Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace ve vaší lokalitě společnosti životního cyklu SCC (služba Zprostředkovatel iniciované přihlašování) nebo pomocí uživatele Azure AD, které jste přiřadili SCC životního cyklu [Úvod do přístup Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="293d8-108">After completing this tutorial, the Azure AD users you have assigned to SCC LifeCycle will be able to single sign into the application at your SCC LifeCycle company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="293d8-109">Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="293d8-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="293d8-110">Povolení integrace aplikace pro životní cyklus SCC</span><span class="sxs-lookup"><span data-stu-id="293d8-110">Enabling the application integration for SCC LifeCycle</span></span>
2. <span data-ttu-id="293d8-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="293d8-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="293d8-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="293d8-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="293d8-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="293d8-113">Assigning users</span></span>

<span data-ttu-id="293d8-114">![Scénář](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="293d8-114">![Scenario](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-scc-lifecycle"></a><span data-ttu-id="293d8-115">Povolit integraci aplikace pro životní cyklus SCC</span><span class="sxs-lookup"><span data-stu-id="293d8-115">Enable the application integration for SCC LifeCycle</span></span>
<span data-ttu-id="293d8-116">Cílem této části se popisují postup povolení integrace aplikace pro životní cyklus SCC.</span><span class="sxs-lookup"><span data-stu-id="293d8-116">The objective of this section is to outline how to enable the application integration for SCC LifeCycle.</span></span>

<span data-ttu-id="293d8-117">**Pokud chcete povolit integraci aplikace pro životní cyklus SCC, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="293d8-117">**To enable the application integration for SCC LifeCycle, perform the following steps:**</span></span>

1. <span data-ttu-id="293d8-118">V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="293d8-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="293d8-119">![Služby Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="293d8-119">![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="293d8-120">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="293d8-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="293d8-121">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="293d8-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="293d8-122">![Aplikace](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="293d8-122">![Applications](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="293d8-123">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="293d8-123">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="293d8-124">![Přidat aplikaci](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="293d8-124">![Add application](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="293d8-125">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="293d8-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="293d8-126">![Přidání aplikace z gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="293d8-126">![Add an application from gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="293d8-127">V **vyhledávacího pole**, typ **životního cyklu SCC**.</span><span class="sxs-lookup"><span data-stu-id="293d8-127">In the **search box**, type **SCC LifeCycle**.</span></span>
   
    <span data-ttu-id="293d8-128">![Galerie aplikací](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="293d8-128">![Application Gallery](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Application Gallery")</span></span>
7. <span data-ttu-id="293d8-129">V podokně výsledků vyberte **životního cyklu SCC**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="293d8-129">In the results pane, select **SCC LifeCycle**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="293d8-130">![Životní cyklus SCC](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC životního cyklu")</span><span class="sxs-lookup"><span data-stu-id="293d8-130">![SCC LifeCycle](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="293d8-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="293d8-131">Configure single sign-on</span></span>

<span data-ttu-id="293d8-132">Cílem této části se popisují, jak uživatelům povolit ověřování na životní cyklus SCC ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="293d8-132">The objective of this section is to outline how to enable users to authenticate to SCC LifeCycle with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="293d8-133">**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="293d8-133">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="293d8-134">Na portálu Azure classic na **životního cyklu SCC** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete ** nakonfigurovat jednotné přihlašování ** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="293d8-134">In the Azure classic portal, on the **SCC LifeCycle** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="293d8-135">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="293d8-135">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="293d8-136">Na **jak chcete uživatelům se přihlásit životní cyklus SCC** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="293d8-136">On the **How would you like users to sign on to SCC LifeCycle** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="293d8-137">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="293d8-137">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="293d8-138">Na **konfigurace adresy URL aplikace** stránky v **přihlašovací adresa URL** textové pole, zadejte adresu URL používají vaši uživatelé k přihlášení do aplikace životního cyklu SCC pomocí následujícího vzorce "*https://bs1.scc.com/ lc7/Welcome/Customer/PICTtest.aspx*"a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="293d8-138">On the **Configure App URL** page, in the **Sign On URL** textbox, type the URL used by your users to sign on to your SCC LifeCycle application using the following pattern "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*", and then click **Next**.</span></span>
   
    <span data-ttu-id="293d8-139">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="293d8-139">![Configure App URL](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Configure App URL")</span></span>
4. <span data-ttu-id="293d8-140">Na **nakonfigurovat jednotné přihlašování v cyklu SCC** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="293d8-140">On the **Configure single sign-on at SCC LifeCycle** page, click **Download metadata**, and then save metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="293d8-141">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="293d8-141">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="293d8-142">Předat dál tento soubor metadat týmu SCC životní cyklus podpory.</span><span class="sxs-lookup"><span data-stu-id="293d8-142">Forward that Metadata file to SCC LifeCycle Support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="293d8-143">Jednotné přihlašování musí být povoleno tým podpory SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="293d8-143">Single sign-on has to be enabled by the SCC LifeCycle support team.</span></span>
   > 
   > 

6. <span data-ttu-id="293d8-144">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="293d8-144">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="293d8-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="293d8-145">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="293d8-146">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="293d8-146">Configure user provisioning</span></span>

<span data-ttu-id="293d8-147">Pokud chcete povolit uživatelům Azure AD přihlášení do SCC životního cyklu, musí být zřízená do SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="293d8-147">In order to enable Azure AD users to log into SCC LifeCycle, they must be provisioned into SCC LifeCycle.</span></span> <span data-ttu-id="293d8-148">Neexistuje žádná položka akce pro konfiguraci zřizování uživatelů k SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="293d8-148">There is no action item for you to configure user provisioning to SCC LifeCycle.</span></span>

<span data-ttu-id="293d8-149">Když přiřazený uživatel se pokusí přihlásit SCC životního cyklu, se automaticky vytvoří účet SCC životního cyklu, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="293d8-149">When an assigned user tries to log into SCC LifeCycle, an SCC LifeCycle account is automatically created if necessary.</span></span>

>[!NOTE]
><span data-ttu-id="293d8-150">Můžete použít jakékoli jiné SCC životní cyklus uživatelského účtu vytvoření nástroje nebo rozhraní API poskytované životního cyklu SCC zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="293d8-150">You can use any other SCC LifeCycle user account creation tools or APIs provided by SCC LifeCycle to provision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="293d8-151">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="293d8-151">Assign users</span></span>
<span data-ttu-id="293d8-152">Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="293d8-152">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="293d8-153">**Přiřazení uživatelů k SCC životního cyklu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="293d8-153">**To assign users to SCC LifeCycle, perform the following steps:**</span></span>

1. <span data-ttu-id="293d8-154">Na portálu Azure classic vytvořte zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="293d8-154">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="293d8-155">Na ** životního cyklu SCC ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="293d8-155">On the **SCC LifeCycle **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="293d8-156">![Přiřazení uživatelů](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="293d8-156">![Assign Users](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Assign Users")</span></span>
3. <span data-ttu-id="293d8-157">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.</span><span class="sxs-lookup"><span data-stu-id="293d8-157">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
    <span data-ttu-id="293d8-158">![Ano](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="293d8-158">![Yes](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="293d8-159">Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="293d8-159">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="293d8-160">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="293d8-160">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

