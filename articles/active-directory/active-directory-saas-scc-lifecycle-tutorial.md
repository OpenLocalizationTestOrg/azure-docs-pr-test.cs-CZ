---
title: "Kurz: Azure Active Directory integrace s životního cyklu SCC | Microsoft Docs"
description: "Zjistěte, jak toouse životního cyklu SCC s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
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
ms.openlocfilehash: c10c313c5fc157ed70d2ccecfb930a8a765f8444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a><span data-ttu-id="142b4-103">Kurz: Azure Active Directory integrace s SCC životního cyklu</span><span class="sxs-lookup"><span data-stu-id="142b4-103">Tutorial: Azure Active Directory integration with SCC LifeCycle</span></span>
<span data-ttu-id="142b4-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="142b4-104">hello objective of this tutorial is tooshow hello integration of Azure and SCC LifeCycle.</span></span>  

<span data-ttu-id="142b4-105">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="142b4-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="142b4-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="142b4-106">A valid Azure subscription</span></span>
* <span data-ttu-id="142b4-107">Životního cyklu SCC jednotné přihlašování (SSO) povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="142b4-107">A SCC LifeCycle single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="142b4-108">Po dokončení tohoto kurzu, hello uživatele Azure AD, které jste přiřadili tooSCC životního cyklu bude možné toosingle přihlášení do aplikace hello ve vaší lokalitě společnosti životního cyklu SCC (služba Zprostředkovatel iniciované přihlašování) nebo pomocí hello [Úvod toohello přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="142b4-108">After completing this tutorial, hello Azure AD users you have assigned tooSCC LifeCycle will be able toosingle sign into hello application at your SCC LifeCycle company site (service provider initiated sign on), or using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="142b4-109">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="142b4-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="142b4-110">Povolení integrace hello aplikace pro životní cyklus SCC</span><span class="sxs-lookup"><span data-stu-id="142b4-110">Enabling hello application integration for SCC LifeCycle</span></span>
2. <span data-ttu-id="142b4-111">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="142b4-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="142b4-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="142b4-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="142b4-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="142b4-113">Assigning users</span></span>

<span data-ttu-id="142b4-114">![Scénář](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="142b4-114">![Scenario](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-scc-lifecycle"></a><span data-ttu-id="142b4-115">Povolit integraci hello aplikace pro životní cyklus SCC</span><span class="sxs-lookup"><span data-stu-id="142b4-115">Enable hello application integration for SCC LifeCycle</span></span>
<span data-ttu-id="142b4-116">Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="142b4-116">hello objective of this section is toooutline how tooenable hello application integration for SCC LifeCycle.</span></span>

<span data-ttu-id="142b4-117">**Integrace aplikace hello tooenable pro SCC životního cyklu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="142b4-117">**tooenable hello application integration for SCC LifeCycle, perform hello following steps:**</span></span>

1. <span data-ttu-id="142b4-118">V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="142b4-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="142b4-119">![Služby Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="142b4-119">![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="142b4-120">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="142b4-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="142b4-121">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="142b4-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="142b4-122">![Aplikace](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="142b4-122">![Applications](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="142b4-123">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="142b4-123">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="142b4-124">![Přidat aplikaci](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="142b4-124">![Add application](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="142b4-125">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="142b4-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="142b4-126">![Přidání aplikace z gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="142b4-126">![Add an application from gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="142b4-127">V hello **vyhledávacího pole**, typ **životního cyklu SCC**.</span><span class="sxs-lookup"><span data-stu-id="142b4-127">In hello **search box**, type **SCC LifeCycle**.</span></span>
   
    <span data-ttu-id="142b4-128">![Galerie aplikací](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="142b4-128">![Application Gallery](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Application Gallery")</span></span>
7. <span data-ttu-id="142b4-129">V podokně výsledků hello, vyberte **životního cyklu SCC**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="142b4-129">In hello results pane, select **SCC LifeCycle**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="142b4-130">![Životní cyklus SCC](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC životního cyklu")</span><span class="sxs-lookup"><span data-stu-id="142b4-130">![SCC LifeCycle](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="142b4-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="142b4-131">Configure single sign-on</span></span>

<span data-ttu-id="142b4-132">Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooSCC životního cyklu ke svému účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="142b4-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooSCC LifeCycle with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="142b4-133">**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="142b4-133">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="142b4-134">V portálu Azure classic, na hello hello **životního cyklu SCC** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello ** nakonfigurovat jednotné přihlašování ** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="142b4-134">In hello Azure classic portal, on hello **SCC LifeCycle** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="142b4-135">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="142b4-135">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="142b4-136">Na hello **jak jste by například uživatelé toosign na tooSCC životního cyklu** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="142b4-136">On hello **How would you like users toosign on tooSCC LifeCycle** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="142b4-137">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="142b4-137">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="142b4-138">Na hello **konfigurace adresy URL aplikace** stránku hello **přihlašovací adresa URL** textovému poli, zadat adresu URL hello používá vaši uživatelé toosign na tooyour SCC životního cyklu aplikace pomocí hello následující vzor "*https:// BS1.SCC.com/lc7/Welcome/Customer/PICTtest.aspx*"a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="142b4-138">On hello **Configure App URL** page, in hello **Sign On URL** textbox, type hello URL used by your users toosign on tooyour SCC LifeCycle application using hello following pattern "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*", and then click **Next**.</span></span>
   
    <span data-ttu-id="142b4-139">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="142b4-139">![Configure App URL](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Configure App URL")</span></span>
4. <span data-ttu-id="142b4-140">Na hello **nakonfigurovat jednotné přihlašování v cyklu SCC** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="142b4-140">On hello **Configure single sign-on at SCC LifeCycle** page, click **Download metadata**, and then save metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="142b4-141">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="142b4-141">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="142b4-142">Předat dál této Metadata souboru tooSCC tým životní cyklus podpory.</span><span class="sxs-lookup"><span data-stu-id="142b4-142">Forward that Metadata file tooSCC LifeCycle Support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="142b4-143">Jednotné přihlašování má toobe ve hello tým podpory životního cyklu SCC povolené.</span><span class="sxs-lookup"><span data-stu-id="142b4-143">Single sign-on has toobe enabled by hello SCC LifeCycle support team.</span></span>
   > 
   > 

6. <span data-ttu-id="142b4-144">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="142b4-144">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="142b4-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="142b4-145">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="142b4-146">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="142b4-146">Configure user provisioning</span></span>

<span data-ttu-id="142b4-147">V pořadí tooenable Azure AD Uživatelé toolog do SCC životního cyklu musí být zřízená do SCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="142b4-147">In order tooenable Azure AD users toolog into SCC LifeCycle, they must be provisioned into SCC LifeCycle.</span></span> <span data-ttu-id="142b4-148">Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooSCC životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="142b4-148">There is no action item for you tooconfigure user provisioning tooSCC LifeCycle.</span></span>

<span data-ttu-id="142b4-149">Při pokusech přiřazený uživatel toolog do SCC životního cyklu SCC životní cyklus účtu se automaticky vytvoří v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="142b4-149">When an assigned user tries toolog into SCC LifeCycle, an SCC LifeCycle account is automatically created if necessary.</span></span>

>[!NOTE]
><span data-ttu-id="142b4-150">Můžete použít jakékoli jiné SCC životní cyklus uživatelského účtu vytvoření nástroje nebo rozhraní API poskytované životního cyklu SCC tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="142b4-150">You can use any other SCC LifeCycle user account creation tools or APIs provided by SCC LifeCycle tooprovision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="142b4-151">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="142b4-151">Assign users</span></span>
<span data-ttu-id="142b4-152">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="142b4-152">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="142b4-153">**tooassign uživatelé tooSCC životního cyklu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="142b4-153">**tooassign users tooSCC LifeCycle, perform hello following steps:**</span></span>

1. <span data-ttu-id="142b4-154">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="142b4-154">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="142b4-155">Na hello ** životního cyklu SCC ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="142b4-155">On hello **SCC LifeCycle **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="142b4-156">![Přiřazení uživatelů](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="142b4-156">![Assign Users](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Assign Users")</span></span>
3. <span data-ttu-id="142b4-157">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="142b4-157">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
    <span data-ttu-id="142b4-158">![Ano](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="142b4-158">![Yes](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="142b4-159">Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="142b4-159">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="142b4-160">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="142b4-160">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

