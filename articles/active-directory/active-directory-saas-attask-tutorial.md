---
title: 'Kurz: Azure Active Directory integrace s @Task| Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 0840763622086a02a27cfafff3b741bc66cec498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a><span data-ttu-id="5551f-103">Kurz: Azure Active Directory integrace s@Task</span><span class="sxs-lookup"><span data-stu-id="5551f-103">Tutorial: Azure Active Directory integration with @Task</span></span>
<span data-ttu-id="5551f-104">cílem Hello tohoto kurzu je tooshow můžete jak toointegrate @Task s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5551f-104">hello objective of this tutorial is tooshow you how toointegrate @Task with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="5551f-105">Integrace @Task s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5551f-105">Integrating @Task with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="5551f-106">Můžete řídit ve službě Azure AD, který má přístuptoo@Task</span><span class="sxs-lookup"><span data-stu-id="5551f-106">You can control in Azure AD who has access too@Task</span></span>
* <span data-ttu-id="5551f-107">Můžete povolit uživatelům tooautomatically získat přihlášeného too@Task (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5551f-107">You can enable your users tooautomatically get signed-on too@Task (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="5551f-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="5551f-108">You can manage your accounts in one central location - hello Azure classic Portal</span></span>

<span data-ttu-id="5551f-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5551f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5551f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5551f-110">Prerequisites</span></span>
<span data-ttu-id="5551f-111">integrace tooconfigure Azure AD s @Task, budete potřebovat hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5551f-111">tooconfigure Azure AD integration with @Task, you need hello following items:</span></span>

* <span data-ttu-id="5551f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5551f-112">An Azure AD subscription</span></span>
* <span data-ttu-id="5551f-113">@Task Jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5551f-113">An @Task single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5551f-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5551f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="5551f-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5551f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="5551f-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="5551f-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="5551f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5551f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="5551f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5551f-118">Scenario Description</span></span>
<span data-ttu-id="5551f-119">cílem Hello tohoto kurzu je tooenable tootest Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5551f-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="5551f-120">scénář Hello uvedených v tomto kurzu se skládá ze tří hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5551f-120">hello scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="5551f-121">Přidání @Task z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5551f-121">Adding @Task from hello gallery</span></span> 
2. <span data-ttu-id="5551f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5551f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-task-from-hello-gallery"></a><span data-ttu-id="5551f-123">Přidání @Task z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5551f-123">Adding @Task from hello gallery</span></span>
<span data-ttu-id="5551f-124">integrace hello tooconfigure @Task do služby Azure AD, je nutné tooadd @Task hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5551f-124">tooconfigure hello integration of @Task into Azure AD, you need tooadd @Task from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5551f-125">**tooadd @Task z Galerie hello provést hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5551f-125">**tooadd @Task from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5551f-126">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5551f-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1] 
2. <span data-ttu-id="5551f-128">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="5551f-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="5551f-129">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5551f-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Aplikace][2] 
4. <span data-ttu-id="5551f-131">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="5551f-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Aplikace][3] 
5. <span data-ttu-id="5551f-133">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="5551f-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Aplikace][4] 
6. <span data-ttu-id="5551f-135">Hello vyhledávacího pole zadejte  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="5551f-135">In hello search box, type **@Task**.</span></span>
   
    ![Aplikace][5] 
7. <span data-ttu-id="5551f-137">V podokně výsledků hello, vyberte  **@Task** a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5551f-137">In hello results pane, select **@Task**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Aplikace][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5551f-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5551f-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5551f-140">Hello cílem této části je tooshow můžete jak tooconfigure a testování Azure AD jednotného přihlašování pomocí @Task podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5551f-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with @Task based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5551f-141">Pro toowork jeden přihlašování, musí Azure AD tooknow hello příslušného uživatele v @Task tooan uživatel ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5551f-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in @Task tooan user in Azure AD is.</span></span> <span data-ttu-id="5551f-142">Jinými slovy, odkaz vztah mezi uživatele Azure AD a související uživatelské hello v @Task musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5551f-142">In other words, a link relationship between an Azure AD user and hello related user in @Task needs toobe established.</span></span>   
<span data-ttu-id="5551f-143">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v @Task.</span><span class="sxs-lookup"><span data-stu-id="5551f-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in @Task.</span></span>

<span data-ttu-id="5551f-144">tooconfigure a testování Azure AD jednotné přihlašování s @Task, budete potřebovat následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5551f-144">tooconfigure and test Azure AD single sign-on with @Task, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5551f-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5551f-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5551f-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5551f-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5551f-147">**[Vytváření @Tasktest uživatele](#creating-a-halogen-software-test-user)**  -toohave a protějšku Britta Simon v @Taskthat je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5551f-147">**[Creating a @Tasktest user](#creating-a-halogen-software-test-user)** - toohave a counterpart of Britta Simon in @Taskthat is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="5551f-148">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5551f-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5551f-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5551f-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5551f-150">Konfigurace Azure AD jednotné přihlášení</span><span class="sxs-lookup"><span data-stu-id="5551f-150">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="5551f-151">Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure classic a tooconfigure jednotné přihlašování v vaší @Task aplikace.</span><span class="sxs-lookup"><span data-stu-id="5551f-151">hello objective of this section is tooenable Azure AD single sign-on in hello Azure classic portal and tooconfigure single sign-on in your @Task application.</span></span>

<span data-ttu-id="5551f-152">**tooconfigure Azure AD jednotné přihlašování s @Task, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5551f-152">**tooconfigure Azure AD single sign-on with @Task, perform hello following steps:**</span></span>

1. <span data-ttu-id="5551f-153">V portálu Azure classic, na hello hello  **@Task**  stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**  Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5551f-153">In hello Azure classic portal, on hello **@Task** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][6] 
2. <span data-ttu-id="5551f-155">Na hello **jak můžete jako toosign uživatelé by na too@Task**  vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5551f-155">On hello **How would you like users toosign on too@Task** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][7] 
3. <span data-ttu-id="5551f-157">Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5551f-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>
   
    ![Konfigurovat nastavení aplikace][8] 
   
     <span data-ttu-id="5551f-159">a.</span><span class="sxs-lookup"><span data-stu-id="5551f-159">a.</span></span> <span data-ttu-id="5551f-160">V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaši uživatelé toosign na tooyour @Task aplikace (např:*https://<Tenant name>.attask ondemand.com*).</span><span class="sxs-lookup"><span data-stu-id="5551f-160">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour @Task application (e.g.:*https://<Tenant name>.attask-ondemand.com*).</span></span>
   
     <span data-ttu-id="5551f-161">b.</span><span class="sxs-lookup"><span data-stu-id="5551f-161">b.</span></span> <span data-ttu-id="5551f-162">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5551f-162">Click **Next**.</span></span>
4. <span data-ttu-id="5551f-163">Na hello **nakonfigurovat jednotné přihlašování v @Task**  klikněte na tlačítko **stáhnout metadata**, uložit soubor metadat hello místně na vašem počítači a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5551f-163">On hello **Configure single sign-on at @Task** page, click **Download metadata**, save hello metadata file locally on your computer, and then click **Next**.</span></span>
   
    ![Co je služba Azure AD Connect][9] 
5. <span data-ttu-id="5551f-165">Přihlášení tooyour @Task společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="5551f-165">Sign-on tooyour @Task company site as administrator.</span></span>
6. <span data-ttu-id="5551f-166">Přejděte příliš**jedna přihlašovací na konfigurační**.</span><span class="sxs-lookup"><span data-stu-id="5551f-166">Go too**Single Sign On Configuration**.</span></span>
7. <span data-ttu-id="5551f-167">Na hello **jednotné přihlašování** dialogové okno, proveďte následující kroky hello</span><span class="sxs-lookup"><span data-stu-id="5551f-167">On hello **Single Sign-On** dialog, perform hello following steps</span></span>
   
    ![Konfigurovat jednotné přihlašování][23]
   
    <span data-ttu-id="5551f-169">a.</span><span class="sxs-lookup"><span data-stu-id="5551f-169">a.</span></span> <span data-ttu-id="5551f-170">Jako **typ**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="5551f-170">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="5551f-171">b.</span><span class="sxs-lookup"><span data-stu-id="5551f-171">b.</span></span> <span data-ttu-id="5551f-172">Vyberte **služby ID zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="5551f-172">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="5551f-173">c.</span><span class="sxs-lookup"><span data-stu-id="5551f-173">c.</span></span> <span data-ttu-id="5551f-174">Na portálu Azure classic hello, zkopírujte hello **vzdálené adresy URL pro přihlášení**a pak ji vložit do hello **adresu URL pro přihlášení portálu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5551f-174">On hello Azure classic portal, copy hello **Remote Login URL**, and then paste it into hello **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="5551f-175">d.</span><span class="sxs-lookup"><span data-stu-id="5551f-175">d.</span></span> <span data-ttu-id="5551f-176">Na portálu Azure classic hello, zkopírujte hello **jednu adresu URL služby Sign-Out**a pak ji vložit do hello **Sign-Out URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5551f-176">On hello Azure classic portal, copy hello **Single Sign-Out Service URL**, and then paste it into hello **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="5551f-177">e.</span><span class="sxs-lookup"><span data-stu-id="5551f-177">e.</span></span> <span data-ttu-id="5551f-178">Na portálu Azure classic hello, zkopírujte hello **heslo změnit adresu URL**a pak ji vložit do hello **heslo změnit adresu URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5551f-178">On hello Azure classic portal, copy hello **Change Password URL**, and then paste it into hello **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="5551f-179">f.</span><span class="sxs-lookup"><span data-stu-id="5551f-179">f.</span></span> <span data-ttu-id="5551f-180">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5551f-180">Click **Save**.</span></span>
8. <span data-ttu-id="5551f-181">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5551f-181">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Co je služba Azure AD Connect][10]
9. <span data-ttu-id="5551f-183">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="5551f-183">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Co je služba Azure AD Connect][11]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5551f-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5551f-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="5551f-186">Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="5551f-186">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>  

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="5551f-188">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5551f-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5551f-189">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5551f-189">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. <span data-ttu-id="5551f-191">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="5551f-191">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="5551f-192">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5551f-192">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. <span data-ttu-id="5551f-194">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="5551f-194">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. <span data-ttu-id="5551f-196">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5551f-196">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="5551f-198">a.</span><span class="sxs-lookup"><span data-stu-id="5551f-198">a.</span></span> <span data-ttu-id="5551f-199">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="5551f-199">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="5551f-200">b.</span><span class="sxs-lookup"><span data-stu-id="5551f-200">b.</span></span> <span data-ttu-id="5551f-201">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5551f-201">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="5551f-202">c.</span><span class="sxs-lookup"><span data-stu-id="5551f-202">c.</span></span> <span data-ttu-id="5551f-203">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5551f-203">Click **Next**.</span></span>
6. <span data-ttu-id="5551f-204">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5551f-204">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    <span data-ttu-id="5551f-206">a.</span><span class="sxs-lookup"><span data-stu-id="5551f-206">a.</span></span> <span data-ttu-id="5551f-207">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="5551f-207">In hello **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="5551f-208">b.</span><span class="sxs-lookup"><span data-stu-id="5551f-208">b.</span></span> <span data-ttu-id="5551f-209">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="5551f-209">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="5551f-210">c.</span><span class="sxs-lookup"><span data-stu-id="5551f-210">c.</span></span> <span data-ttu-id="5551f-211">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="5551f-211">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="5551f-212">d.</span><span class="sxs-lookup"><span data-stu-id="5551f-212">d.</span></span> <span data-ttu-id="5551f-213">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="5551f-213">In hello **Role** list, select **User**.</span></span>

    <span data-ttu-id="5551f-214">e.</span><span class="sxs-lookup"><span data-stu-id="5551f-214">e.</span></span> <span data-ttu-id="5551f-215">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5551f-215">Click **Next**.</span></span>

7. <span data-ttu-id="5551f-216">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5551f-216">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. <span data-ttu-id="5551f-218">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5551f-218">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="5551f-220">a.</span><span class="sxs-lookup"><span data-stu-id="5551f-220">a.</span></span> <span data-ttu-id="5551f-221">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="5551f-221">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="5551f-222">b.</span><span class="sxs-lookup"><span data-stu-id="5551f-222">b.</span></span> <span data-ttu-id="5551f-223">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="5551f-223">Click **Complete**.</span></span>   

### <a name="creating-an-task-test-user"></a><span data-ttu-id="5551f-224">Vytvoření @Task testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5551f-224">Creating an @Task test user</span></span>
<span data-ttu-id="5551f-225">Hello cílem této části je toocreate uživatel volal Britta Simon v @Task.</span><span class="sxs-lookup"><span data-stu-id="5551f-225">hello objective of this section is toocreate a user called Britta Simon in @Task.</span></span>

<span data-ttu-id="5551f-226">**toocreate uživatel volal Britta Simon v @Task, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5551f-226">**toocreate a user called Britta Simon in @Task, perform hello following steps:**</span></span>

1. <span data-ttu-id="5551f-227">Přihlaste se tooyour @Task společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="5551f-227">Sign on tooyour @Task company site as administrator.</span></span>
2. <span data-ttu-id="5551f-228">V nabídce hello hello nahoře, klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="5551f-228">In hello menu on hello top, click **People**.</span></span>
3. <span data-ttu-id="5551f-229">Klikněte na tlačítko **nové osobě**.</span><span class="sxs-lookup"><span data-stu-id="5551f-229">Click **New Person**.</span></span> 
4. <span data-ttu-id="5551f-230">V dialogovém okně hello nové osobě proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5551f-230">On hello New Person dialog, perform hello following steps:</span></span>
   
    ![Vytvoření @Task testovacího uživatele][21] 
   
    <span data-ttu-id="5551f-232">a.</span><span class="sxs-lookup"><span data-stu-id="5551f-232">a.</span></span> <span data-ttu-id="5551f-233">V hello **křestní jméno** textovému poli, zadejte "Britta".</span><span class="sxs-lookup"><span data-stu-id="5551f-233">In hello **First Name** textbox, type "Britta".</span></span>
   
    <span data-ttu-id="5551f-234">b.</span><span class="sxs-lookup"><span data-stu-id="5551f-234">b.</span></span> <span data-ttu-id="5551f-235">V hello **příjmení** textovému poli, zadejte "Simon".</span><span class="sxs-lookup"><span data-stu-id="5551f-235">In hello **Last Name** textbox, type "Simon".</span></span>
   
    <span data-ttu-id="5551f-236">c.</span><span class="sxs-lookup"><span data-stu-id="5551f-236">c.</span></span> <span data-ttu-id="5551f-237">V hello **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu Britta Simon v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5551f-237">In hello **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="5551f-238">d.</span><span class="sxs-lookup"><span data-stu-id="5551f-238">d.</span></span> <span data-ttu-id="5551f-239">Klikněte na tlačítko **přidat osobu**.</span><span class="sxs-lookup"><span data-stu-id="5551f-239">Click **Add Person**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5551f-240">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5551f-240">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="5551f-241">Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování tak, že udělíte přístup too@Task.</span><span class="sxs-lookup"><span data-stu-id="5551f-241">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access too@Task.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5551f-243">**tooassign Britta Simon too@Task, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5551f-243">**tooassign Britta Simon too@Task, perform hello following steps:**</span></span>

1. <span data-ttu-id="5551f-244">Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5551f-244">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Přiřadit uživatele][201] 
2. <span data-ttu-id="5551f-246">V seznamu aplikace hello vyberte  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="5551f-246">In hello applications list, select **@Task**.</span></span>
   
    ![Přiřadit uživatele][202] 
3. <span data-ttu-id="5551f-248">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5551f-248">In hello menu on hello top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][203] 
4. <span data-ttu-id="5551f-250">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="5551f-250">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="5551f-251">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="5551f-251">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="5551f-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5551f-253">Testing Single Sign-On</span></span>
<span data-ttu-id="5551f-254">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5551f-254">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="5551f-255">Když kliknete na tlačítko hello @Task dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour @Task aplikace.</span><span class="sxs-lookup"><span data-stu-id="5551f-255">When you click hello @Task tile in hello Access Panel, you should get automatically signed-on tooyour @Task application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5551f-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5551f-256">Additional Resources</span></span>
* [<span data-ttu-id="5551f-257">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5551f-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5551f-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5551f-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






