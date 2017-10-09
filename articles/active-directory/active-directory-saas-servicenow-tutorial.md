---
title: 'Kurz: Azure Active Directory integrace s ServiceNow | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ServiceNow a ServiceNow Express."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: df6a07dd1aa437198fbdb9d0a04ea14f3a320249
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a><span data-ttu-id="9f531-103">Kurz: Azure Active Directory integrace s ServiceNow</span><span class="sxs-lookup"><span data-stu-id="9f531-103">Tutorial: Azure Active Directory integration with ServiceNow</span></span>
<span data-ttu-id="9f531-104">V tomto kurzu zjistíte, jak toointegrate ServiceNow a ServiceNow Express s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9f531-104">In this tutorial, you learn how toointegrate ServiceNow and ServiceNow Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9f531-105">Integrace s Azure AD ServiceNow a ServiceNow Express poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9f531-105">Integrating ServiceNow and ServiceNow Express with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="9f531-106">Můžete řídit v Azure AD, který má přístup tooServiceNow a ServiceNow Express</span><span class="sxs-lookup"><span data-stu-id="9f531-106">You can control in Azure AD who has access tooServiceNow and ServiceNow Express</span></span>
* <span data-ttu-id="9f531-107">Můžete povolit uživatelům tooautomatically get přihlášeného tooServiceNow a ServiceNow Express (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f531-107">You can enable your users tooautomatically get signed-on tooServiceNow and ServiceNow Express (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="9f531-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="9f531-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="9f531-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9f531-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f531-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f531-110">Prerequisites</span></span>
<span data-ttu-id="9f531-111">tooconfigure integrace Azure AD s ServiceNow a ServiceNow Express, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9f531-111">tooconfigure Azure AD integration with ServiceNow and ServiceNow Express, you need hello following items:</span></span>

* <span data-ttu-id="9f531-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f531-112">An Azure AD subscription</span></span>
* <span data-ttu-id="9f531-113">Pro ServiceNow, instanci nebo klienta ServiceNow Calgary verze nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="9f531-113">For ServiceNow, an instance or tenant of ServiceNow, Calgary version or higher</span></span>
* <span data-ttu-id="9f531-114">Pro instance ServiceNow Express, Helsinkách verze ServiceNow Express nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="9f531-114">For ServiceNow Express, an instance of ServiceNow Express, Helsinki version or higher</span></span>
* <span data-ttu-id="9f531-115">Hello ServiceNow klienta musí mít hello [více jeden znak na modul Plugin poskytovatele](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) povolena.</span><span class="sxs-lookup"><span data-stu-id="9f531-115">hello ServiceNow tenant must have hello [Multiple Provider Single Sign On Plugin](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) enabled.</span></span> <span data-ttu-id="9f531-116">To lze provést [odesílá se žádost o službu](https://hi.service-now.com).</span><span class="sxs-lookup"><span data-stu-id="9f531-116">This can be done by [submitting a service request](https://hi.service-now.com).</span></span> 

> [!NOTE]
> <span data-ttu-id="9f531-117">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f531-117">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="9f531-118">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9f531-118">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="9f531-119">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="9f531-119">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="9f531-120">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9f531-120">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9f531-121">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9f531-121">Scenario description</span></span>
<span data-ttu-id="9f531-122">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f531-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9f531-123">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9f531-123">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9f531-124">Přidání ServiceNow z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9f531-124">Adding ServiceNow from hello gallery</span></span>
2. <span data-ttu-id="9f531-125">Konfigurace a testování Azure AD jednotného přihlašování pro ServiceNow a ServiceNow Express</span><span class="sxs-lookup"><span data-stu-id="9f531-125">Configuring and testing Azure AD single sign-on for ServiceNow or ServiceNow Express</span></span>

## <a name="adding-servicenow-from-hello-gallery"></a><span data-ttu-id="9f531-126">Přidání ServiceNow z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9f531-126">Adding ServiceNow from hello gallery</span></span>
<span data-ttu-id="9f531-127">tooconfigure hello integrace ServiceNow a ServiceNow Express do Azure AD, je nutné tooadd ServiceNow hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9f531-127">tooconfigure hello integration of ServiceNow or ServiceNow Express into Azure AD, you need tooadd ServiceNow from hello gallery tooyour list of managed SaaS apps.</span></span> 

<span data-ttu-id="9f531-128">**tooadd ServiceNow z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f531-128">**tooadd ServiceNow from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f531-129">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9f531-129">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="9f531-131">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="9f531-131">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="9f531-132">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="9f531-132">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Aplikace][2]
4. <span data-ttu-id="9f531-134">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="9f531-134">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Aplikace][3]
5. <span data-ttu-id="9f531-136">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="9f531-136">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Aplikace][4]
6. <span data-ttu-id="9f531-138">Hello vyhledávacího pole zadejte **ServiceNow**.</span><span class="sxs-lookup"><span data-stu-id="9f531-138">In hello search box, type **ServiceNow**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. <span data-ttu-id="9f531-140">V podokně výsledků hello, vyberte **ServiceNow**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f531-140">In hello results pane, select **ServiceNow**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9f531-142">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f531-142">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9f531-143">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s ServiceNow a ServiceNow Express podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9f531-143">In this section, you configure and test Azure AD single sign-on with ServiceNow or ServiceNow Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9f531-144">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ServiceNow je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f531-144">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ServiceNow is tooa user in Azure AD.</span></span> <span data-ttu-id="9f531-145">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ServiceNow musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9f531-145">In other words, a link relationship between an Azure AD user and hello related user in ServiceNow needs toobe established.</span></span>
<span data-ttu-id="9f531-146">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-146">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ServiceNow.</span></span> <span data-ttu-id="9f531-147">tooconfigure a testu Azure AD jednotné přihlašování s ServiceNow, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9f531-147">tooconfigure and test Azure AD single sign-on with ServiceNow, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9f531-148">**[Konfigurace Azure AD jednotné přihlašování pro ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9f531-148">**[Configuring Azure AD Single Sign-On for ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9f531-149">**[Konfigurace Azure AD jednotné přihlašování pro expresní ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9f531-149">**[Configuring Azure AD Single Sign-On for ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - tooenable your users toouse this feature.</span></span>
3. <span data-ttu-id="9f531-150">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9f531-150">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="9f531-151">**[Vytvoření zkušebního uživatele ServiceNow](#creating-a-servicenow-test-user)**  -toohave protějšek Britta Simon v ServiceNow, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f531-151">**[Creating a ServiceNow test user](#creating-a-servicenow-test-user)** - toohave a counterpart of Britta Simon in ServiceNow that is linked toohello Azure AD representation of her.</span></span>
5. <span data-ttu-id="9f531-152">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9f531-152">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="9f531-153">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9f531-153">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

> [!NOTE]
> <span data-ttu-id="9f531-154">Pokud chcete, aby tooconfigure ServiceNow vynechte krok 2.</span><span class="sxs-lookup"><span data-stu-id="9f531-154">If you want tooconfigure ServiceNow omit step 2.</span></span> <span data-ttu-id="9f531-155">Podobně pokud chcete, aby tooconfigure ServiceNow Express vynechte krok 1.</span><span class="sxs-lookup"><span data-stu-id="9f531-155">Likewise, if you want tooconfigure ServiceNow Express omit step 1.</span></span>
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a><span data-ttu-id="9f531-156">Konfigurace Azure AD jednotné přihlášení pro ServiceNow</span><span class="sxs-lookup"><span data-stu-id="9f531-156">Configuring Azure AD Single Sign-On for ServiceNow</span></span>
1. <span data-ttu-id="9f531-157">Na portálu classic hello Azure AD na hello **ServiceNow** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno .</span><span class="sxs-lookup"><span data-stu-id="9f531-157">In hello Azure AD classic portal, on hello **ServiceNow** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="9f531-158">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-158">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="9f531-159">Na hello **jak jste by například uživatelé toosign na tooServiceNow** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f531-159">On hello **How would you like users toosign on tooServiceNow** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="9f531-160">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749324.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-160">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")</span></span>

3. <span data-ttu-id="9f531-161">Na hello **nakonfigurovat nastavení aplikace** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f531-161">On hello **Configure App Settings** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="9f531-162">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="9f531-162">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")</span></span>
   
    <span data-ttu-id="9f531-163">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-163">a.</span></span> <span data-ttu-id="9f531-164">v hello **ServiceNow přihlašovací adresa URL** textové pole, zadejte adresu URL používá vaše aplikace ServiceNow uživatelé tooyour toosign na následující vzor hello: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="9f531-164">in hello **ServiceNow Sign On URL** textbox, type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="9f531-165">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-165">b.</span></span> <span data-ttu-id="9f531-166">V hello **identifikátor** textové pole, zadejte adresu URL používá vaše aplikace ServiceNow uživatelé tooyour toosign na následující vzor hello: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="9f531-166">In hello **Identifier** textbox,type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="9f531-167">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-167">c.</span></span> <span data-ttu-id="9f531-168">Klikněte na **Další**</span><span class="sxs-lookup"><span data-stu-id="9f531-168">Click **Next**</span></span>

4. <span data-ttu-id="9f531-169">toohave Azure AD automaticky nakonfigurovat ServiceNow pro ověřování na základě SAML, zadejte název instance ServiceNow, uživatelské jméno správce a heslo správce v hello **automaticky nakonfigurovat jednotné přihlašování** formuláři a klikněte na tlačítko  *Konfigurace*.</span><span class="sxs-lookup"><span data-stu-id="9f531-169">toohave Azure AD automatically configure ServiceNow for SAML-based authentication, enter your ServiceNow instance name, admin username, and admin password in hello **Auto configure single sign-on** form and click *Configure*.</span></span> <span data-ttu-id="9f531-170">Všimněte si, že hello správce uživatelské jméno musí mít hello **security_admin** přiřazenou v ServiceNow pro tento toowork roli.</span><span class="sxs-lookup"><span data-stu-id="9f531-170">Note that hello admin username provided must have hello **security_admin** role assigned in ServiceNow for this toowork.</span></span> <span data-ttu-id="9f531-171">Jinak toomanually nakonfigurovat ServiceNow toouse Azure AD jako poskytovatele identity SAML, klikněte na **ručně nakonfigurovat hello aplikaci pro jednotné přihlašování**, pak klikněte na tlačítko **Další** a dokončení hello Následující kroky.</span><span class="sxs-lookup"><span data-stu-id="9f531-171">Otherwise, toomanually configure ServiceNow toouse Azure AD as a SAML identity provider, click **Manually configure hello application for single sign-on**, then click **Next** and complete hello following steps.</span></span>
   
    <span data-ttu-id="9f531-172">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="9f531-172">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")</span></span>

5. <span data-ttu-id="9f531-173">Na hello **nakonfigurovat jednotné přihlašování v ServiceNow** klikněte na tlačítko **stažení certifikátu**, uložte soubor certifikátu hello místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9f531-173">On hello **Configure single sign-on at ServiceNow** page, click **Download certificate**, save hello certificate file locally on your computer.</span></span>
   
    <span data-ttu-id="9f531-174">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749325.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-174">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")</span></span>

6. <span data-ttu-id="9f531-175">Přihlášení tooyour ServiceNow aplikace jako správce.</span><span class="sxs-lookup"><span data-stu-id="9f531-175">Sign-on tooyour ServiceNow application as an administrator.</span></span>

7. <span data-ttu-id="9f531-176">Aktivovat hello *integrace - více jeden přihlašování instalační program zprostředkovatele* modulu plug-in podle následující hello další kroky:</span><span class="sxs-lookup"><span data-stu-id="9f531-176">Activate hello *Integration - Multiple Provider Single Sign-On Installer* plugin by following hello next steps:</span></span>
   
    <span data-ttu-id="9f531-177">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-177">a.</span></span> <span data-ttu-id="9f531-178">V navigačním podokně hello na levé straně hello přejděte příliš**definice systému** části a pak klikněte na **modulů plug-in**.</span><span class="sxs-lookup"><span data-stu-id="9f531-178">In hello navigation pane on hello left side, go too**System Definition** section and then click **Plugins**.</span></span>
   
    <span data-ttu-id="9f531-179">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "aktivovat modulu plug-in")</span><span class="sxs-lookup"><span data-stu-id="9f531-179">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Activate plugin")</span></span>
   
    <span data-ttu-id="9f531-180">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-180">b.</span></span> <span data-ttu-id="9f531-181">Vyhledejte *integrace - více jeden přihlašování instalační program zprostředkovatele*.</span><span class="sxs-lookup"><span data-stu-id="9f531-181">Search for *Integration - Multiple Provider Single Sign-On Installer*.</span></span>
   
    <span data-ttu-id="9f531-182">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "aktivovat modulu plug-in")</span><span class="sxs-lookup"><span data-stu-id="9f531-182">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Activate plugin")</span></span>
   
    <span data-ttu-id="9f531-183">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-183">c.</span></span> <span data-ttu-id="9f531-184">Vyberte modul plug-in hello.</span><span class="sxs-lookup"><span data-stu-id="9f531-184">Select hello plugin.</span></span> <span data-ttu-id="9f531-185">Rigth klikněte a vyberte **aktivovat nebo upgradovat**.</span><span class="sxs-lookup"><span data-stu-id="9f531-185">Rigth click and select **Activate/Upgrade**.</span></span>
   
    <span data-ttu-id="9f531-186">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-186">d.</span></span> <span data-ttu-id="9f531-187">Klikněte na tlačítko hello **aktivovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f531-187">Click hello **Activate** button.</span></span>

8. <span data-ttu-id="9f531-188">V navigačním podokně hello na levé straně hello, klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="9f531-188">In hello navigation pane on hello left side, click **Properties**.</span></span>  
   
    <span data-ttu-id="9f531-189">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="9f531-189">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Configure app URL")</span></span>

9. <span data-ttu-id="9f531-190">Na hello **více jednotného přihlašování k vlastnosti zprostředkovatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f531-190">On hello **Multiple Provider SSO Properties** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="9f531-191">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="9f531-191">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Configure app URL")</span></span>
   
    <span data-ttu-id="9f531-192">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-192">a.</span></span> <span data-ttu-id="9f531-193">Jako **povolit zprostředkovatel vícenásobného jednotného přihlašování k**, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="9f531-193">As **Enable multiple provider SSO**, select **Yes**.</span></span>
   
    <span data-ttu-id="9f531-194">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-194">b.</span></span> <span data-ttu-id="9f531-195">Jako **povolit protokolování ladění získali hello zprostředkovatel vícenásobného jednotného přihlašování k integraci**, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="9f531-195">As **Enable debug logging got hello multiple provider SSO integration**, select **Yes**.</span></span>
   
    <span data-ttu-id="9f531-196">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-196">c.</span></span> <span data-ttu-id="9f531-197">V **hello pole hello uživatele tabulky, který...**  textovému poli, typ **uživatelské_jméno**.</span><span class="sxs-lookup"><span data-stu-id="9f531-197">In **hello field on hello user table that...** textbox, type **user_name**.</span></span>
   
    <span data-ttu-id="9f531-198">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-198">d.</span></span> <span data-ttu-id="9f531-199">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9f531-199">Click **Save**.</span></span>

10. <span data-ttu-id="9f531-200">V navigačním podokně hello na levé straně hello, klikněte na tlačítko **x509 certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="9f531-200">In hello navigation pane on hello left side, click **x509 Certificates**.</span></span>
    
     <span data-ttu-id="9f531-201">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-201">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Configure single sign-on")</span></span>

11. <span data-ttu-id="9f531-202">Na hello **certifikáty X.509** dialogové okno, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="9f531-202">On hello **X.509 Certificates** dialog, click **New**.</span></span>
    
     <span data-ttu-id="9f531-203">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-203">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Configure single sign-on")</span></span>

12. <span data-ttu-id="9f531-204">Na hello **certifikáty X.509** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f531-204">On hello **X.509 Certificates** dialog, perform hello following steps:</span></span>
    
     <span data-ttu-id="9f531-205">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-205">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")</span></span>
    
     <span data-ttu-id="9f531-206">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-206">a.</span></span> <span data-ttu-id="9f531-207">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="9f531-207">Click **New**.</span></span>
    
     <span data-ttu-id="9f531-208">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-208">b.</span></span> <span data-ttu-id="9f531-209">V hello **název** textovému poli, zadejte název pro svou konfiguraci (například: **TestSAML2.0**).</span><span class="sxs-lookup"><span data-stu-id="9f531-209">In hello **Name** textbox, type a name for your configuration (e.g.: **TestSAML2.0**).</span></span>
    
     <span data-ttu-id="9f531-210">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-210">c.</span></span> <span data-ttu-id="9f531-211">Vyberte **Active**.</span><span class="sxs-lookup"><span data-stu-id="9f531-211">Select **Active**.</span></span>
    
     <span data-ttu-id="9f531-212">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-212">d.</span></span> <span data-ttu-id="9f531-213">Jako **formátu**, vyberte **PEM**.</span><span class="sxs-lookup"><span data-stu-id="9f531-213">As **Format**, select **PEM**.</span></span>
    
     <span data-ttu-id="9f531-214">e.</span><span class="sxs-lookup"><span data-stu-id="9f531-214">e.</span></span> <span data-ttu-id="9f531-215">Jako **typ**, vyberte **důvěřovat certifikátu úložiště**.</span><span class="sxs-lookup"><span data-stu-id="9f531-215">As **Type**, select **Trust Store Cert**.</span></span>
    
     <span data-ttu-id="9f531-216">f.</span><span class="sxs-lookup"><span data-stu-id="9f531-216">f.</span></span> <span data-ttu-id="9f531-217">Otevření certifikátu kódováním Base64 v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **PEM certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9f531-217">Open your Base64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **PEM Certificate** textbox.</span></span>
    
     <span data-ttu-id="9f531-218">g.</span><span class="sxs-lookup"><span data-stu-id="9f531-218">g.</span></span> <span data-ttu-id="9f531-219">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="9f531-219">Click **Update**.</span></span>

13. <span data-ttu-id="9f531-220">V navigačním podokně hello na levé straně hello, klikněte na tlačítko **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="9f531-220">In hello navigation pane on hello left side, click **Identity Providers**.</span></span>
    
     <span data-ttu-id="9f531-221">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-221">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Configure single sign-on")</span></span>

14. <span data-ttu-id="9f531-222">Na hello **zprostředkovatelů Identity** dialogové okno, klikněte na tlačítko **nový**:</span><span class="sxs-lookup"><span data-stu-id="9f531-222">On hello **Identity Providers** dialog, click **New**:</span></span>
    
     <span data-ttu-id="9f531-223">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-223">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Configure single sign-on")</span></span>

15. <span data-ttu-id="9f531-224">Na hello **zprostředkovatelů Identity** dialogové okno, klikněte na tlačítko **aktualizaci1 typu SAML2?**:</span><span class="sxs-lookup"><span data-stu-id="9f531-224">On hello **Identity Providers** dialog, click **SAML2 Update1?**:</span></span>
    
     <span data-ttu-id="9f531-225">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-225">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Configure single sign-on")</span></span>

16. <span data-ttu-id="9f531-226">V dialogovém okně Vlastnosti typu SAML2 aktualizaci1 hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f531-226">On hello SAML2 Update1 Properties dialog, perform hello following steps:</span></span>
    
     <span data-ttu-id="9f531-227">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-227">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Configure single sign-on")</span></span>

    <span data-ttu-id="9f531-228">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-228">a.</span></span> <span data-ttu-id="9f531-229">v hello **název** textovému poli, zadejte název pro svou konfiguraci (například: **SAML 2.0**).</span><span class="sxs-lookup"><span data-stu-id="9f531-229">in hello **Name** textbox, type a name for your configuration (e.g.: **SAML 2.0**).</span></span>

    <span data-ttu-id="9f531-230">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-230">b.</span></span> <span data-ttu-id="9f531-231">V hello **pole uživatelského** textovému poli, typ **e-mailu** nebo **uživatelské_jméno**, v závislosti na tom, která pole se používá toouniquely identifikace uživatelů ve vašem nasazení ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-231">In hello **User Field** textbox, type **email** or **user_name**, depending on which field is used toouniquely identify users in your ServiceNow deployment.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="9f531-232">Můžete buď ID uživatele hello Azure AD (hlavní název uživatele) tooemit configue Azure AD nebo hello e-mailovou adresu jako jedinečný identifikátor v tokenu SAML hello hello podle budete toohello **ServiceNow > atributy > jednotné přihlašování** část Hello klasický portál Azure a mapování hello požadovaného pole toohello **nameidentifier** atribut.</span><span class="sxs-lookup"><span data-stu-id="9f531-232">You can configue Azure AD tooemit either hello Azure AD user ID (user principal name) or hello email address as hello unique identifier in hello SAML token by going toohello **ServiceNow > Attributes > Single Sign-On** section of hello Azure classic portal and mapping hello desired field toohello **nameidentifier** attribute.</span></span> <span data-ttu-id="9f531-233">Hodnota Hello uložené pro vybraný atribut hello ve službě Azure AD (například hlavní název uživatele) musí odpovídat hello hodnotou uloženou v ServiceNow hello zadaná pole (například uživatelské_jméno)</span><span class="sxs-lookup"><span data-stu-id="9f531-233">hello value stored for hello selected attribute in Azure AD (e.g. user principal name) must match hello value stored in ServiceNow for hello entered field (e.g. user_name)</span></span>

    <span data-ttu-id="9f531-234">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-234">c.</span></span> <span data-ttu-id="9f531-235">Na portálu classic hello Azure AD, zkopírujte hello **ID zprostředkovatele Identity** hodnotu a pak ji vložit do hello **adresa URL poskytovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9f531-235">In hello Azure AD classic portal, copy hello **Identity Provider ID** value, and then paste it into hello **Identity Provider URL** textbox.</span></span>

    <span data-ttu-id="9f531-236">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-236">d.</span></span> <span data-ttu-id="9f531-237">Na portálu classic hello Azure AD, zkopírujte hello **adresa URL žádosti o ověření** hodnotu a pak ji vložit do hello **AuthnRequest zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9f531-237">In hello Azure AD classic portal, copy hello **Authentication Request URL** value, and then paste it into hello **Identity Provider's AuthnRequest** textbox.</span></span>

    <span data-ttu-id="9f531-238">e.</span><span class="sxs-lookup"><span data-stu-id="9f531-238">e.</span></span> <span data-ttu-id="9f531-239">Na portálu classic hello Azure AD, zkopírujte hello **jednu adresu URL služby Sign-Out** hodnotu a pak ji vložit do hello **SingleLogoutRequest zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9f531-239">In hello Azure AD classic portal, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **Identity Provider's SingleLogoutRequest** textbox.</span></span>

    <span data-ttu-id="9f531-240">f.</span><span class="sxs-lookup"><span data-stu-id="9f531-240">f.</span></span> <span data-ttu-id="9f531-241">V hello **domovské stránky ServiceNow** textovému poli, typ hello URL domovské stránce instance ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-241">In hello **ServiceNow Homepage** textbox, type hello URL of your ServiceNow instance homepage.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9f531-242">domovskou stránku instance ServiceNow Hello je tvořen vaše **URL klienta ServieNow** a **/navpage.do** (např:`https://fabrikam.service-now.com/navpage.do`).</span><span class="sxs-lookup"><span data-stu-id="9f531-242">hello ServiceNow instance homepage is a concatenation of your **ServieNow tenant URL** and **/navpage.do** (e.g.:`https://fabrikam.service-now.com/navpage.do`).</span></span>

    <span data-ttu-id="9f531-243">g.</span><span class="sxs-lookup"><span data-stu-id="9f531-243">g.</span></span> <span data-ttu-id="9f531-244">V hello **Entity ID / vystavitele** textovému poli, zadat adresu URL hello klienta služby ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-244">In hello **Entity ID / Issuer** textbox, type hello URL of your ServiceNow tenant.</span></span>

    <span data-ttu-id="9f531-245">h.</span><span class="sxs-lookup"><span data-stu-id="9f531-245">h.</span></span> <span data-ttu-id="9f531-246">V hello **cílovou skupinu URL** textovému poli, zadat adresu URL hello klienta služby ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-246">In hello **Audience URL** textbox, type hello URL of your ServiceNow tenant.</span></span> 

    <span data-ttu-id="9f531-247">i.</span><span class="sxs-lookup"><span data-stu-id="9f531-247">i.</span></span> <span data-ttu-id="9f531-248">V hello **protokol vazby pro hello na IDP SingleLogoutRequest** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:bindings:HTTP-přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="9f531-248">In hello **Protocol Binding for hello IDP's SingleLogoutRequest** textbox, type **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.</span></span>

    <span data-ttu-id="9f531-249">j.</span><span class="sxs-lookup"><span data-stu-id="9f531-249">j.</span></span> <span data-ttu-id="9f531-250">Hello NameID zásad textové pole, zadejte **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: neurčené**.</span><span class="sxs-lookup"><span data-stu-id="9f531-250">In hello NameID Policy textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.</span></span>

    <span data-ttu-id="9f531-251">kB.</span><span class="sxs-lookup"><span data-stu-id="9f531-251">k.</span></span> <span data-ttu-id="9f531-252">Zrušte výběr **vytvořit AuthnContextClass**.</span><span class="sxs-lookup"><span data-stu-id="9f531-252">Deselect **Create an AuthnContextClass**.</span></span>

    <span data-ttu-id="9f531-253">l.</span><span class="sxs-lookup"><span data-stu-id="9f531-253">l.</span></span> <span data-ttu-id="9f531-254">V hello **AuthnContextClassRef metoda**, typ `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.</span><span class="sxs-lookup"><span data-stu-id="9f531-254">In hello **AuthnContextClassRef Method**, type `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.</span></span> <span data-ttu-id="9f531-255">To je potřeba jenom Pokud jste cloudu pouze organizace.</span><span class="sxs-lookup"><span data-stu-id="9f531-255">This is only needed if you are cloud only organization.</span></span> <span data-ttu-id="9f531-256">Pokud používáte místní služby AD FS nebo vícefaktorového ověřování pro ověřování pak byste neměli konfigurovat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9f531-256">If you are using on-premises ADFS or MFA for authentication then you should not configure this value.</span></span> 

    <span data-ttu-id="9f531-257">m.</span><span class="sxs-lookup"><span data-stu-id="9f531-257">m.</span></span> <span data-ttu-id="9f531-258">V **hodin zkreslit** textovému poli, typ **60**.</span><span class="sxs-lookup"><span data-stu-id="9f531-258">In **Clock Skew** textbox, type **60**.</span></span>

    <span data-ttu-id="9f531-259">n.</span><span class="sxs-lookup"><span data-stu-id="9f531-259">n.</span></span> <span data-ttu-id="9f531-260">Jako **jednoho přihlášení na skriptu**, vyberte **MultiSSO_SAML2_Update1**.</span><span class="sxs-lookup"><span data-stu-id="9f531-260">As **Single Sign On Script**, select **MultiSSO_SAML2_Update1**.</span></span>

    <span data-ttu-id="9f531-261">o.</span><span class="sxs-lookup"><span data-stu-id="9f531-261">o.</span></span> <span data-ttu-id="9f531-262">Jako **x509 certifikát**vyberte hello certifikátů, které jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="9f531-262">As **x509 Certificate**, select hello certificate you have created in hello previous step.</span></span>

    <span data-ttu-id="9f531-263">p.</span><span class="sxs-lookup"><span data-stu-id="9f531-263">p.</span></span> <span data-ttu-id="9f531-264">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="9f531-264">Click **Submit**.</span></span> 

1. <span data-ttu-id="9f531-265">Na portálu classic hello Azure AD, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f531-265">On hello Azure AD classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    <span data-ttu-id="9f531-266">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-266">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="9f531-267">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="9f531-267">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    <span data-ttu-id="9f531-268">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-268">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")</span></span>

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a><span data-ttu-id="9f531-269">Konfigurace Azure AD jednotné přihlášení pro ServiceNow Express</span><span class="sxs-lookup"><span data-stu-id="9f531-269">Configuring Azure AD Single Sign-On for ServiceNow Express</span></span>
1. <span data-ttu-id="9f531-270">Na portálu classic hello Azure AD na hello **ServiceNow** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno .</span><span class="sxs-lookup"><span data-stu-id="9f531-270">In hello Azure AD classic portal, on hello **ServiceNow** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="9f531-271">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-271">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="9f531-272">Na hello **jak jste by například uživatelé toosign na tooServiceNow** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f531-272">On hello **How would you like users toosign on tooServiceNow** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="9f531-273">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749324.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-273">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")</span></span>

3. <span data-ttu-id="9f531-274">Na hello **nakonfigurovat nastavení aplikace** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f531-274">On hello **Configure App Settings** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="9f531-275">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="9f531-275">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")</span></span>
   
    <span data-ttu-id="9f531-276">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-276">a.</span></span> <span data-ttu-id="9f531-277">v hello **ServiceNow přihlašovací adresa URL** textové pole, zadejte adresu URL používá vaše aplikace ServiceNow uživatelé tooyour toosign na následující vzor hello: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="9f531-277">in hello **ServiceNow Sign On URL** textbox, type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="9f531-278">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-278">b.</span></span> <span data-ttu-id="9f531-279">V hello **URL vystavitele** textové pole, zadejte adresu URL používá vaše aplikace ServiceNow uživatelé tooyour toosign na následující hello vzor `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="9f531-279">In hello **Issuer URL** textbox,type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="9f531-280">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-280">c.</span></span> <span data-ttu-id="9f531-281">Klikněte na **Další**</span><span class="sxs-lookup"><span data-stu-id="9f531-281">Click **Next**</span></span>

4. <span data-ttu-id="9f531-282">Klikněte na tlačítko **ručně nakonfigurovat hello aplikaci pro jednotné přihlašování**, pak klikněte na tlačítko **Další** a dokončení hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="9f531-282">Click **Manually configure hello application for single sign-on**, then click **Next** and complete hello following steps.</span></span>
   
    <span data-ttu-id="9f531-283">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="9f531-283">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")</span></span>

5. <span data-ttu-id="9f531-284">Na hello **nakonfigurovat jednotné přihlašování v ServiceNow** klikněte na tlačítko **stažení certifikátu**, uložit soubor certifikátu hello místně na vašem počítači a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f531-284">On hello **Configure single sign-on at ServiceNow** page, click **Download certificate**, save hello certificate file locally on your computer, and then click **Next**.</span></span>
   
    <span data-ttu-id="9f531-285">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749325.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-285">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")</span></span>

6. <span data-ttu-id="9f531-286">Přihlášení tooyour aplikace ServiceNow Express jako správce.</span><span class="sxs-lookup"><span data-stu-id="9f531-286">Sign-on tooyour ServiceNow Express application as an administrator.</span></span>

7. <span data-ttu-id="9f531-287">V navigačním podokně hello na levé straně hello, klikněte na tlačítko **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9f531-287">In hello navigation pane on hello left side, click **Single Sign-On**.</span></span>  
   
    <span data-ttu-id="9f531-288">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="9f531-288">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Configure app URL")</span></span>

8. <span data-ttu-id="9f531-289">Na hello **jednotné přihlašování** dialogové okno, klikněte na ikonu konfigurace hello na hello horní, pravé a nastavte hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="9f531-289">On hello **Single Sign-On** dialog, click hello configuration icon on hello upper right and set hello following properties:</span></span>
   
    <span data-ttu-id="9f531-290">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="9f531-290">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Configure app URL")</span></span>
   
    <span data-ttu-id="9f531-291">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-291">a.</span></span> <span data-ttu-id="9f531-292">Přepnutí **povolit zprostředkovatel vícenásobného jednotného přihlašování k** toohello vpravo.</span><span class="sxs-lookup"><span data-stu-id="9f531-292">Toggle **Enable multiple provider SSO** toohello right.</span></span>
   
    <span data-ttu-id="9f531-293">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-293">b.</span></span> <span data-ttu-id="9f531-294">Přepnutí **ladění povolení protokolování pro hello zprostředkovatel vícenásobného jednotného přihlašování k integraci** toohello vpravo.</span><span class="sxs-lookup"><span data-stu-id="9f531-294">Toggle **Enable debug logging for hello multiple provider SSO integration** toohello right.</span></span>
   
    <span data-ttu-id="9f531-295">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-295">c.</span></span> <span data-ttu-id="9f531-296">V **hello pole hello uživatele tabulky, který...**  textovému poli, typ **uživatelské_jméno**.</span><span class="sxs-lookup"><span data-stu-id="9f531-296">In **hello field on hello user table that...** textbox, type **user_name**.</span></span>
9. <span data-ttu-id="9f531-297">Na hello **jednotné přihlašování** dialogové okno, klikněte na tlačítko **přidat nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="9f531-297">On hello **Single Sign-On** dialog, click **Add New Certificate**.</span></span>
   
    <span data-ttu-id="9f531-298">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-298">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Configure single sign-on")</span></span>
10. <span data-ttu-id="9f531-299">Na hello **certifikáty X.509** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f531-299">On hello **X.509 Certificates** dialog, perform hello following steps:</span></span>
    
    <span data-ttu-id="9f531-300">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-300">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")</span></span>
    
    <span data-ttu-id="9f531-301">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-301">a.</span></span> <span data-ttu-id="9f531-302">V hello **název** textovému poli, zadejte název pro svou konfiguraci (například: **TestSAML2.0**).</span><span class="sxs-lookup"><span data-stu-id="9f531-302">In hello **Name** textbox, type a name for your configuration (e.g.: **TestSAML2.0**).</span></span>
    
    <span data-ttu-id="9f531-303">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-303">b.</span></span> <span data-ttu-id="9f531-304">Vyberte **Active**.</span><span class="sxs-lookup"><span data-stu-id="9f531-304">Select **Active**.</span></span>
    
    <span data-ttu-id="9f531-305">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-305">c.</span></span> <span data-ttu-id="9f531-306">Jako **formátu**, vyberte **PEM**.</span><span class="sxs-lookup"><span data-stu-id="9f531-306">As **Format**, select **PEM**.</span></span>
    
    <span data-ttu-id="9f531-307">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-307">d.</span></span> <span data-ttu-id="9f531-308">Jako **typ**, vyberte **důvěřovat certifikátu úložiště**.</span><span class="sxs-lookup"><span data-stu-id="9f531-308">As **Type**, select **Trust Store Cert**.</span></span>
    
    <span data-ttu-id="9f531-309">e.</span><span class="sxs-lookup"><span data-stu-id="9f531-309">e.</span></span> <span data-ttu-id="9f531-310">Vytvořte soubor kódováním Base64 z stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="9f531-310">Create a Base64 encoded file from your downloaded certificate.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="9f531-311">Další podrobnosti najdete v tématu [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="9f531-311">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>
    > 
    > 
    
    <span data-ttu-id="9f531-312">f.</span><span class="sxs-lookup"><span data-stu-id="9f531-312">f.</span></span> <span data-ttu-id="9f531-313">Otevření certifikátu kódováním Base64 v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **PEM certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9f531-313">Open your Base64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **PEM Certificate** textbox.</span></span>
    
    <span data-ttu-id="9f531-314">g.</span><span class="sxs-lookup"><span data-stu-id="9f531-314">g.</span></span> <span data-ttu-id="9f531-315">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="9f531-315">Click **Update**.</span></span>
11. <span data-ttu-id="9f531-316">Na hello **jednotné přihlašování** dialogové okno, klikněte na tlačítko **přidat nové rozšíření IdP**.</span><span class="sxs-lookup"><span data-stu-id="9f531-316">On hello **Single Sign-On** dialog, click **Add New IdP**.</span></span>
    
    <span data-ttu-id="9f531-317">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-317">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Configure single sign-on")</span></span>
12. <span data-ttu-id="9f531-318">Na hello **přidat nového poskytovatele Identity** dialogové okno, v části **konfigurace zprostředkovatele Identity**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f531-318">On hello **Add New Identity Provider** dialog, under **Configure Identity Provider**, perform hello following steps:</span></span>
    
    <span data-ttu-id="9f531-319">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-319">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Configure single sign-on")</span></span>

    <span data-ttu-id="9f531-320">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-320">a.</span></span> <span data-ttu-id="9f531-321">v hello **název** textovému poli, zadejte název pro svou konfiguraci (například: **SAML 2.0**).</span><span class="sxs-lookup"><span data-stu-id="9f531-321">In hello **Name** textbox, type a name for your configuration (e.g.: **SAML 2.0**).</span></span>

    <span data-ttu-id="9f531-322">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-322">b.</span></span> <span data-ttu-id="9f531-323">Na portálu classic hello Azure AD, zkopírujte hello **ID zprostředkovatele Identity** hodnotu a pak ji vložit do hello **adresa URL poskytovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9f531-323">In hello Azure AD classic portal, copy hello **Identity Provider ID** value, and then paste it into hello **Identity Provider URL** textbox.</span></span>

    <span data-ttu-id="9f531-324">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-324">c.</span></span> <span data-ttu-id="9f531-325">Na portálu classic hello Azure AD, zkopírujte hello **adresa URL žádosti o ověření** hodnotu a pak ji vložit do hello **AuthnRequest zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9f531-325">In hello Azure AD classic portal, copy hello **Authentication Request URL** value, and then paste it into hello **Identity Provider's AuthnRequest** textbox.</span></span>

    <span data-ttu-id="9f531-326">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-326">d.</span></span> <span data-ttu-id="9f531-327">Na portálu classic hello Azure AD, zkopírujte hello **jednu adresu URL služby Sign-Out** hodnotu a pak ji vložit do hello **SingleLogoutRequest zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9f531-327">In hello Azure AD classic portal, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **Identity Provider's SingleLogoutRequest** textbox.</span></span>

    <span data-ttu-id="9f531-328">e.</span><span class="sxs-lookup"><span data-stu-id="9f531-328">e.</span></span> <span data-ttu-id="9f531-329">Jako **certifikát zprostředkovatele Identity**vyberte hello certifikátů, které jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="9f531-329">As **Identity Provider Certificate**, select hello certificate you have created in hello previous step.</span></span>


1. <span data-ttu-id="9f531-330">Klikněte na tlačítko **Upřesnit nastavení**a v části **další vlastnosti zprostředkovatele Identity**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f531-330">Click **Advanced Settings**, and under **Additional Identity Provider Properties**, perform hello following steps:</span></span>
   
    <span data-ttu-id="9f531-331">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-331">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="9f531-332">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-332">a.</span></span> <span data-ttu-id="9f531-333">V hello **protokol vazby pro hello na IDP SingleLogoutRequest** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:bindings:HTTP-přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="9f531-333">In hello **Protocol Binding for hello IDP's SingleLogoutRequest** textbox, type **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.</span></span>
   
    <span data-ttu-id="9f531-334">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-334">b.</span></span> <span data-ttu-id="9f531-335">V hello **NameID zásad** textovému poli, typ **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: neurčené**.</span><span class="sxs-lookup"><span data-stu-id="9f531-335">In hello **NameID Policy** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.</span></span>    
   
    <span data-ttu-id="9f531-336">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-336">c.</span></span> <span data-ttu-id="9f531-337">V hello **AuthnContextClassRef metoda**, typ **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.</span><span class="sxs-lookup"><span data-stu-id="9f531-337">In hello **AuthnContextClassRef Method**, type **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.</span></span>
   
    <span data-ttu-id="9f531-338">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-338">d.</span></span> <span data-ttu-id="9f531-339">Zrušte výběr **vytvořit AuthnContextClass**.</span><span class="sxs-lookup"><span data-stu-id="9f531-339">Deselect **Create an AuthnContextClass**.</span></span>

2. <span data-ttu-id="9f531-340">V části **další vlastnosti zprostředkovatele služby**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f531-340">Under **Additional Service Provider Properties**, perform hello following steps:</span></span>
   
    <span data-ttu-id="9f531-341">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-341">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="9f531-342">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-342">a.</span></span> <span data-ttu-id="9f531-343">V hello **domovské stránky ServiceNow** textovému poli, typ hello URL domovské stránce instance ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-343">In hello **ServiceNow Homepage** textbox, type hello URL of your ServiceNow instance homepage.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="9f531-344">domovskou stránku instance ServiceNow Hello je tvořen vaše **URL klienta ServieNow** a **/navpage.do** (např: `https://fabrikam.service-now.com/navpage.do`).</span><span class="sxs-lookup"><span data-stu-id="9f531-344">hello ServiceNow instance homepage is a concatenation of your **ServieNow tenant URL** and **/navpage.do** (e.g.: `https://fabrikam.service-now.com/navpage.do`).</span></span>
    > 
    > 
   
    <span data-ttu-id="9f531-345">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-345">b.</span></span> <span data-ttu-id="9f531-346">V hello **Entity ID / vystavitele** textovému poli, zadat adresu URL hello klienta služby ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-346">In hello **Entity ID / Issuer** textbox, type hello URL of your ServiceNow tenant.</span></span>
   
    <span data-ttu-id="9f531-347">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-347">c.</span></span> <span data-ttu-id="9f531-348">V hello **identifikátor URI cílové skupiny** textovému poli, zadat adresu URL hello klienta služby ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-348">In hello **Audience URI** textbox, type hello URL of your ServiceNow tenant.</span></span> 
   
    <span data-ttu-id="9f531-349">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-349">d.</span></span> <span data-ttu-id="9f531-350">V **hodin zkreslit** textovému poli, typ **60**.</span><span class="sxs-lookup"><span data-stu-id="9f531-350">In **Clock Skew** textbox, type **60**.</span></span>
   
    <span data-ttu-id="9f531-351">e.</span><span class="sxs-lookup"><span data-stu-id="9f531-351">e.</span></span> <span data-ttu-id="9f531-352">V hello **pole uživatelského** textovému poli, typ **e-mailu** nebo **uživatelské_jméno**, v závislosti na tom, která pole se používá toouniquely identifikace uživatelů ve vašem nasazení ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-352">In hello **User Field** textbox, type **email** or **user_name**, depending on which field is used toouniquely identify users in your ServiceNow deployment.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="9f531-353">Můžete buď ID uživatele hello Azure AD (hlavní název uživatele) tooemit configue Azure AD nebo hello e-mailovou adresu jako jedinečný identifikátor v tokenu SAML hello hello podle budete toohello **ServiceNow > atributy > jednotné přihlašování** část Hello klasický portál Azure a mapování hello požadovaného pole toohello **nameidentifier** atribut.</span><span class="sxs-lookup"><span data-stu-id="9f531-353">You can configue Azure AD tooemit either hello Azure AD user ID (user principal name) or hello email address as hello unique identifier in hello SAML token by going toohello **ServiceNow > Attributes > Single Sign-On** section of hello Azure classic portal and mapping hello desired field toohello **nameidentifier** attribute.</span></span> <span data-ttu-id="9f531-354">Hodnota Hello uložené pro vybraný atribut hello ve službě Azure AD (například hlavní název uživatele) musí odpovídat hello hodnotou uloženou v ServiceNow hello zadaná pole (například uživatelské_jméno)</span><span class="sxs-lookup"><span data-stu-id="9f531-354">hello value stored for hello selected attribute in Azure AD (e.g. user principal name) must match hello value stored in ServiceNow for hello entered field (e.g. user_name)</span></span>
    > 
    > 
   
    <span data-ttu-id="9f531-355">f.</span><span class="sxs-lookup"><span data-stu-id="9f531-355">f.</span></span> <span data-ttu-id="9f531-356">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9f531-356">Click **Save**.</span></span> 

3. <span data-ttu-id="9f531-357">Na portálu classic hello Azure AD, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f531-357">On hello Azure AD classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    <span data-ttu-id="9f531-358">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-358">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")</span></span>

4. <span data-ttu-id="9f531-359">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="9f531-359">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    <span data-ttu-id="9f531-360">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9f531-360">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")</span></span>

## <a name="configuring-user-provisioning"></a><span data-ttu-id="9f531-361">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="9f531-361">Configuring user provisioning</span></span>
<span data-ttu-id="9f531-362">Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele tooServiceNow účty.</span><span class="sxs-lookup"><span data-stu-id="9f531-362">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooServiceNow.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="9f531-363">tooconfigure zřizování uživatelů, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f531-363">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="9f531-364">V hello Azure classic portálu pro správu, na hello **ServiceNow** stránky integrace aplikací, klikněte na tlačítko **konfiguraci zřizování uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="9f531-364">In hello Azure Management classic portal, on hello **ServiceNow** application integration page, click **Configure user provisioning**.</span></span> 
   
    <span data-ttu-id="9f531-365">![Zřizování uživatelů](./media/active-directory-saas-servicenow-tutorial/IC769498.png "zřizování uživatelů")</span><span class="sxs-lookup"><span data-stu-id="9f531-365">![User provisioning](./media/active-directory-saas-servicenow-tutorial/IC769498.png "User provisioning")</span></span>

2. <span data-ttu-id="9f531-366">Na hello **zadejte vaše zřizování automatické uživatelů ServiceNow pověření tooenable** zadejte hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="9f531-366">On hello **Enter your ServiceNow credentials tooenable automatic user provisioning** page, provide hello following configuration settings:</span></span>
   
     <span data-ttu-id="9f531-367">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-367">a.</span></span> <span data-ttu-id="9f531-368">V hello **název Instance ServiceNow** textovému poli, název instance ServiceNow hello typu.</span><span class="sxs-lookup"><span data-stu-id="9f531-368">In hello **ServiceNow Instance Name** textbox, type hello ServiceNow instance name.</span></span>
   
     <span data-ttu-id="9f531-369">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-369">b.</span></span> <span data-ttu-id="9f531-370">V hello **uživatelské jméno správce ServiceNow** textovému poli, název typu hello hello ServiceNow účet správce.</span><span class="sxs-lookup"><span data-stu-id="9f531-370">In hello **ServiceNow Admin User Name** textbox, type hello name of hello ServiceNow admin account.</span></span>
   
     <span data-ttu-id="9f531-371">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-371">c.</span></span> <span data-ttu-id="9f531-372">V hello **heslo správce ServiceNow** textovému poli, zadejte hello heslo pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="9f531-372">In hello **ServiceNow Admin Password** textbox, type hello password for this account.</span></span>
   
     <span data-ttu-id="9f531-373">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-373">d.</span></span> <span data-ttu-id="9f531-374">Klikněte na tlačítko **ověření** tooverify konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="9f531-374">Click **validate** tooverify your configuration.</span></span>
   
     <span data-ttu-id="9f531-375">e.</span><span class="sxs-lookup"><span data-stu-id="9f531-375">e.</span></span> <span data-ttu-id="9f531-376">Klikněte na tlačítko hello **Další** tlačítko tooopen hello **další kroky** stránky.</span><span class="sxs-lookup"><span data-stu-id="9f531-376">Click hello **Next** button tooopen hello **Next steps** page.</span></span>
   
     <span data-ttu-id="9f531-377">f.</span><span class="sxs-lookup"><span data-stu-id="9f531-377">f.</span></span> <span data-ttu-id="9f531-378">Pokud chcete tooprovision aplikace toothis všechny uživatele, vyberte možnost "**automaticky zřizovat všechny uživatelské účty v aplikaci toothis directory hello**".</span><span class="sxs-lookup"><span data-stu-id="9f531-378">If you want tooprovision all users toothis application, select “**Automatically provision all user accounts in hello directory toothis application**”.</span></span> 
   
    <span data-ttu-id="9f531-379">![Další kroky](./media/active-directory-saas-servicenow-tutorial/IC698804.png "další kroky")</span><span class="sxs-lookup"><span data-stu-id="9f531-379">![Next Steps](./media/active-directory-saas-servicenow-tutorial/IC698804.png "Next Steps")</span></span>
   
     <span data-ttu-id="9f531-380">g.</span><span class="sxs-lookup"><span data-stu-id="9f531-380">g.</span></span> <span data-ttu-id="9f531-381">Na hello **další kroky** klikněte na tlačítko **Complete** toosave konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="9f531-381">On hello **Next steps** page, click **Complete** toosave your configuration.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9f531-382">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f531-382">Creating an Azure AD test user</span></span>
<span data-ttu-id="9f531-383">V této části vytvoříte na portálu classic hello názvem Britta Simon testovacího uživatele.</span><span class="sxs-lookup"><span data-stu-id="9f531-383">In this section, you create a test user in hello classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="9f531-385">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f531-385">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f531-386">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9f531-386">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="9f531-388">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="9f531-388">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="9f531-389">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9f531-389">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9f531-391">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9f531-391">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="9f531-393">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f531-393">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="9f531-395">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-395">a.</span></span> <span data-ttu-id="9f531-396">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="9f531-396">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="9f531-397">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-397">b.</span></span> <span data-ttu-id="9f531-398">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9f531-398">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="9f531-399">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-399">c.</span></span> <span data-ttu-id="9f531-400">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f531-400">Click **Next**.</span></span>

6. <span data-ttu-id="9f531-401">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f531-401">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   <span data-ttu-id="9f531-403">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-403">a.</span></span> <span data-ttu-id="9f531-404">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="9f531-404">In hello **First Name** textbox, type **Britta**.</span></span>  
   
   <span data-ttu-id="9f531-405">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-405">b.</span></span> <span data-ttu-id="9f531-406">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="9f531-406">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
   <span data-ttu-id="9f531-407">c.</span><span class="sxs-lookup"><span data-stu-id="9f531-407">c.</span></span> <span data-ttu-id="9f531-408">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9f531-408">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="9f531-409">d.</span><span class="sxs-lookup"><span data-stu-id="9f531-409">d.</span></span> <span data-ttu-id="9f531-410">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9f531-410">In hello **Role** list, select **User**.</span></span>
   
   <span data-ttu-id="9f531-411">e.</span><span class="sxs-lookup"><span data-stu-id="9f531-411">e.</span></span> <span data-ttu-id="9f531-412">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f531-412">Click **Next**.</span></span>

7. <span data-ttu-id="9f531-413">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9f531-413">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="9f531-415">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f531-415">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="9f531-417">a.</span><span class="sxs-lookup"><span data-stu-id="9f531-417">a.</span></span> <span data-ttu-id="9f531-418">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="9f531-418">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="9f531-419">b.</span><span class="sxs-lookup"><span data-stu-id="9f531-419">b.</span></span> <span data-ttu-id="9f531-420">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="9f531-420">Click **Complete**.</span></span>   

### <a name="creating-a-servicenow-test-user"></a><span data-ttu-id="9f531-421">Vytvoření zkušebního uživatele ServiceNow</span><span class="sxs-lookup"><span data-stu-id="9f531-421">Creating a ServiceNow test user</span></span>
<span data-ttu-id="9f531-422">V této části vytvoříte uživatele volal Britta Simon v ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-422">In this section, you create a user called Britta Simon in ServiceNow.</span></span> <span data-ttu-id="9f531-423">V této části vytvoříte uživatele volal Britta Simon v ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-423">In this section, you create a user called Britta Simon in ServiceNow.</span></span> <span data-ttu-id="9f531-424">Pokud nevíte jak tooadd uživatele ve vaší ServiceNow nebo ServiceNow Express účet, kontaktujte tým podpory ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="9f531-424">If you don't know how tooadd a user in your ServiceNow or ServiceNow Express account, contact ServiceNow support team.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9f531-425">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9f531-425">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="9f531-426">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooServiceNow svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="9f531-426">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooServiceNow.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9f531-428">**tooassign Britta Simon tooServiceNow, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f531-428">**tooassign Britta Simon tooServiceNow, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f531-429">Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="9f531-429">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9f531-431">V seznamu aplikace hello vyberte **ServiceNow**.</span><span class="sxs-lookup"><span data-stu-id="9f531-431">In hello applications list, select **ServiceNow**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. <span data-ttu-id="9f531-433">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9f531-433">In hello menu on hello top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][203] 

4. <span data-ttu-id="9f531-435">V seznamu hello všechny uživatele, vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9f531-435">In hello All Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="9f531-436">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="9f531-436">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="9f531-438">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f531-438">Testing single sign-on</span></span>
<span data-ttu-id="9f531-439">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9f531-439">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9f531-440">Když kliknete na dlaždici ServiceNow hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ServiceNow aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f531-440">When you click hello ServiceNow tile in hello Access Panel, you should get automatically signed-on tooyour ServiceNow application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f531-441">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9f531-441">Additional resources</span></span>
* [<span data-ttu-id="9f531-442">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f531-442">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9f531-443">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9f531-443">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
