---
title: 'Kurz: Azure Active Directory integrace s Halosys | Microsoft Docs'
description: "Zjistěte, jak toouse Halosys s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18043ed1b6f7ab45c59cfd36252bef1621618e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a><span data-ttu-id="83ac6-103">Kurz: Azure Active Directory integrace s Halosys</span><span class="sxs-lookup"><span data-stu-id="83ac6-103">Tutorial: Azure Active Directory integration with Halosys</span></span>

<span data-ttu-id="83ac6-104">V tomto kurzu zjistíte, jak toointegrate Halosys s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="83ac6-104">In this tutorial, you learn how toointegrate Halosys with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="83ac6-105">Integrace Halosys s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="83ac6-105">Integrating Halosys with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="83ac6-106">Můžete řídit ve službě Azure AD, který má přístup tooHalosys</span><span class="sxs-lookup"><span data-stu-id="83ac6-106">You can control in Azure AD who has access tooHalosys</span></span>
- <span data-ttu-id="83ac6-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHalosys (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="83ac6-107">You can enable your users tooautomatically get signed-on tooHalosys (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="83ac6-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="83ac6-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="83ac6-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="83ac6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83ac6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83ac6-110">Prerequisites</span></span>

<span data-ttu-id="83ac6-111">Integrace služby Azure AD s Halosys tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="83ac6-111">tooconfigure Azure AD integration with Halosys, you need hello following items:</span></span>

- <span data-ttu-id="83ac6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="83ac6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="83ac6-113">Halosys jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="83ac6-113">A Halosys single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="83ac6-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="83ac6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="83ac6-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="83ac6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="83ac6-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="83ac6-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="83ac6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="83ac6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="83ac6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="83ac6-118">Scenario description</span></span>
<span data-ttu-id="83ac6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="83ac6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="83ac6-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="83ac6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="83ac6-121">Přidání Halosys z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="83ac6-121">Adding Halosys from hello gallery</span></span>
2. <span data-ttu-id="83ac6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="83ac6-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-halosys-from-hello-gallery"></a><span data-ttu-id="83ac6-123">Přidání Halosys z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="83ac6-123">Adding Halosys from hello gallery</span></span>
<span data-ttu-id="83ac6-124">tooconfigure hello integrace Halosys do Azure AD, je nutné tooadd Halosys hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="83ac6-124">tooconfigure hello integration of Halosys into Azure AD, you need tooadd Halosys from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="83ac6-125">**tooadd Halosys z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="83ac6-125">**tooadd Halosys from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="83ac6-126">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]
2. <span data-ttu-id="83ac6-128">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="83ac6-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="83ac6-129">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="83ac6-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Aplikace][2]

4. <span data-ttu-id="83ac6-131">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="83ac6-131">Click **Add** at hello bottom of hello page.</span></span>

    ![Aplikace][3]

5. <span data-ttu-id="83ac6-133">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>

    ![Aplikace][4]

6. <span data-ttu-id="83ac6-135">Hello vyhledávacího pole zadejte **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-135">In hello search box, type **Halosys**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. <span data-ttu-id="83ac6-137">V podokně výsledků hello, vyberte **Halosys**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="83ac6-137">In hello results pane, select **Halosys**, and then click **Complete** tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="83ac6-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="83ac6-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="83ac6-140">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Halosys podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="83ac6-140">In this section, you configure and test Azure AD single sign-on with Halosys based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="83ac6-141">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Halosys je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83ac6-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Halosys is tooa user in Azure AD.</span></span> <span data-ttu-id="83ac6-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Halosys musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="83ac6-142">In other words, a link relationship between an Azure AD user and hello related user in Halosys needs toobe established.</span></span>

<span data-ttu-id="83ac6-143">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Halosys.</span><span class="sxs-lookup"><span data-stu-id="83ac6-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Halosys.</span></span>

<span data-ttu-id="83ac6-144">tooconfigure a testu Azure AD jednotné přihlašování s Halosys, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="83ac6-144">tooconfigure and test Azure AD single sign-on with Halosys, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="83ac6-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="83ac6-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="83ac6-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="83ac6-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="83ac6-147">**[Vytvoření zkušebního uživatele Halosys](#creating-a-halosys-test-user)**  -toohave protějšek Britta Simon v Halosys, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83ac6-147">**[Creating a Halosys test user](#creating-a-halosys-test-user)** - toohave a counterpart of Britta Simon in Halosys that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="83ac6-148">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="83ac6-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="83ac6-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="83ac6-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="83ac6-150">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="83ac6-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="83ac6-151">V této části můžete povolit Azure AD jednotné přihlašování v portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Halosys.</span><span class="sxs-lookup"><span data-stu-id="83ac6-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your Halosys application.</span></span>


<span data-ttu-id="83ac6-152">**tooconfigure Azure AD jednotné přihlašování s Halosys, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="83ac6-152">**tooconfigure Azure AD single sign-on with Halosys, perform hello following steps:**</span></span>

1. <span data-ttu-id="83ac6-153">Na portálu classic hello na hello **Halosys** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83ac6-153">In hello classic portal, on hello **Halosys** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
     
    ![Konfigurovat jednotné přihlašování][6] 

2. <span data-ttu-id="83ac6-155">Na hello **jak jste by například uživatelé toosign na tooHalosys** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-155">On hello **How would you like users toosign on tooHalosys** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. <span data-ttu-id="83ac6-157">Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="83ac6-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    <span data-ttu-id="83ac6-159">a.</span><span class="sxs-lookup"><span data-stu-id="83ac6-159">a.</span></span> <span data-ttu-id="83ac6-160">V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše aplikace Halosys tooyour toosign na uživatele pomocí hello následující vzor: `https://<company-name>.Halosys.com/client-api/api`.</span><span class="sxs-lookup"><span data-stu-id="83ac6-160">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour Halosys application using hello following pattern: `https://<company-name>.Halosys.com/client-api/api`.</span></span>

    <span data-ttu-id="83ac6-161">b.In hello **identifikátoru adresy URL** textovému poli, zadejte adresu URL hello v hello následující vzor: `https://<company-name>.Halosys.com`.</span><span class="sxs-lookup"><span data-stu-id="83ac6-161">b.In hello **Identifier URL** textbox, type hello URL in hello following pattern: `https://<company-name>.Halosys.com`.</span></span> 
         
4. <span data-ttu-id="83ac6-162">Na hello **nakonfigurovat jednotné přihlašování v Halosys** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello ve vašem počítači:</span><span class="sxs-lookup"><span data-stu-id="83ac6-162">On hello **Configure single sign-on at Halosys** page, click **Download metadata**, and then save hello file on your computer:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. <span data-ttu-id="83ac6-164">tooget jednotné přihlašování, které jsou nakonfigurované pro aplikace, obraťte se na tým podpory Halosys a poskytnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="83ac6-164">tooget SSO configured for your application, contact Halosys support team and provide them with hello following:</span></span>

    <span data-ttu-id="83ac6-165">• hello Stáhnout **soubor metadat**</span><span class="sxs-lookup"><span data-stu-id="83ac6-165">• hello downloaded **metadata file**</span></span>
    
    <span data-ttu-id="83ac6-166">• hello **URL jednotné přihlašování SAML**</span><span class="sxs-lookup"><span data-stu-id="83ac6-166">• hello **SAML SSO URL**</span></span>
    

6. <span data-ttu-id="83ac6-167">Hello portálu classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-167">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Azure AD jednotné přihlášení][10]

7. <span data-ttu-id="83ac6-169">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-169">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="83ac6-171">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="83ac6-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="83ac6-172">V této části vytvoříte na portálu classic hello názvem Britta Simon testovacího uživatele.</span><span class="sxs-lookup"><span data-stu-id="83ac6-172">In this section, you create a test user in hello classic portal called Britta Simon.</span></span>


![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="83ac6-174">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="83ac6-174">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="83ac6-175">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-175">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="83ac6-177">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="83ac6-177">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="83ac6-178">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-178">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="83ac6-180">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-180">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="83ac6-182">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky hello: ![vytváření testovací uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span><span class="sxs-lookup"><span data-stu-id="83ac6-182">On hello **Tell us about this user** dialog page, perform hello following steps:  ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span></span> 

    <span data-ttu-id="83ac6-183">a.</span><span class="sxs-lookup"><span data-stu-id="83ac6-183">a.</span></span> <span data-ttu-id="83ac6-184">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="83ac6-184">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="83ac6-185">b.</span><span class="sxs-lookup"><span data-stu-id="83ac6-185">b.</span></span> <span data-ttu-id="83ac6-186">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-186">In hello User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="83ac6-187">c.</span><span class="sxs-lookup"><span data-stu-id="83ac6-187">c.</span></span> <span data-ttu-id="83ac6-188">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-188">Click **Next**.</span></span>

6.  <span data-ttu-id="83ac6-189">Na hello **profil uživatele** dialogové okno proveďte následující kroky hello: ![vytváření testovací uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span><span class="sxs-lookup"><span data-stu-id="83ac6-189">On hello **User Profile** dialog page, perform hello following steps: ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span></span> 

    <span data-ttu-id="83ac6-190">a.</span><span class="sxs-lookup"><span data-stu-id="83ac6-190">a.</span></span> <span data-ttu-id="83ac6-191">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-191">In hello **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="83ac6-192">b.</span><span class="sxs-lookup"><span data-stu-id="83ac6-192">b.</span></span> <span data-ttu-id="83ac6-193">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-193">In hello **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="83ac6-194">c.</span><span class="sxs-lookup"><span data-stu-id="83ac6-194">c.</span></span> <span data-ttu-id="83ac6-195">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-195">In hello **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="83ac6-196">d.</span><span class="sxs-lookup"><span data-stu-id="83ac6-196">d.</span></span> <span data-ttu-id="83ac6-197">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-197">In hello **Role** list, select **User**.</span></span>

    <span data-ttu-id="83ac6-198">e.</span><span class="sxs-lookup"><span data-stu-id="83ac6-198">e.</span></span> <span data-ttu-id="83ac6-199">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-199">Click **Next**.</span></span>

7. <span data-ttu-id="83ac6-200">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-200">On hello **Get temporary password** dialog page, click **create**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="83ac6-202">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="83ac6-202">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="83ac6-204">a.</span><span class="sxs-lookup"><span data-stu-id="83ac6-204">a.</span></span> <span data-ttu-id="83ac6-205">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-205">Write down hello value of hello **New Password**.</span></span>

    <span data-ttu-id="83ac6-206">b.</span><span class="sxs-lookup"><span data-stu-id="83ac6-206">b.</span></span> <span data-ttu-id="83ac6-207">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-207">Click **Complete**.</span></span>   



### <a name="creating-a-halosys-test-user"></a><span data-ttu-id="83ac6-208">Vytvoření zkušebního uživatele Halosys</span><span class="sxs-lookup"><span data-stu-id="83ac6-208">Creating a Halosys test user</span></span>

<span data-ttu-id="83ac6-209">V této části vytvoříte volal Britta Simon v Halosys uživatele.</span><span class="sxs-lookup"><span data-stu-id="83ac6-209">In this section, you create a user called Britta Simon in Halosys.</span></span> <span data-ttu-id="83ac6-210">Spojte se s Halosys podporu team tooadd hello uživatelé v platformě Halosys hello.</span><span class="sxs-lookup"><span data-stu-id="83ac6-210">Please work with Halosys support team tooadd hello users in hello Halosys platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="83ac6-211">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="83ac6-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="83ac6-212">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooHalosys svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="83ac6-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooHalosys.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="83ac6-214">**tooassign Britta Simon tooHalosys, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="83ac6-214">**tooassign Britta Simon tooHalosys, perform hello following steps:**</span></span>

1. <span data-ttu-id="83ac6-215">Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="83ac6-215">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="83ac6-217">V seznamu aplikace hello vyberte **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-217">In hello applications list, select **Halosys**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. <span data-ttu-id="83ac6-219">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-219">In hello menu on hello top, click **Users**.</span></span>

    ![Přiřadit uživatele][203]

4. <span data-ttu-id="83ac6-221">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-221">In hello Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="83ac6-222">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="83ac6-222">In hello toolbar on hello bottom, click **Assign**.</span></span>

    ![Přiřadit uživatele][205]


### <a name="testing-single-sign-on"></a><span data-ttu-id="83ac6-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="83ac6-224">Testing single sign-on</span></span>

<span data-ttu-id="83ac6-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="83ac6-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="83ac6-226">Po kliknutí na tlačítko hello Halosys dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Halosys aplikace.</span><span class="sxs-lookup"><span data-stu-id="83ac6-226">When you click hello Halosys tile in hello Access Panel, you should get automatically signed-on tooyour Halosys application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="83ac6-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="83ac6-227">Additional resources</span></span>

* [<span data-ttu-id="83ac6-228">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="83ac6-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="83ac6-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="83ac6-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
