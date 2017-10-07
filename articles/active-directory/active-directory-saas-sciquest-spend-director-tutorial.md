---
title: "Kurz: Azure Active Directory integrace s SciQuest tráví ředitel | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SciQuest tráví ředitel."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 47c46f1297054fd96b86c1d8c66e1a55ec151497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a><span data-ttu-id="ce005-103">Kurz: Azure Active Directory integrace s SciQuest tráví ředitel</span><span class="sxs-lookup"><span data-stu-id="ce005-103">Tutorial: Azure Active Directory integration with SciQuest Spend Director</span></span>
<span data-ttu-id="ce005-104">cílem Hello tohoto kurzu je tooshow můžete jak toointegrate SciQuest tráví nacházející se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ce005-104">hello objective of this tutorial is tooshow you how toointegrate SciQuest Spend Director with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="ce005-105">Integrace SciQuest tráví ředitel s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ce005-105">Integrating SciQuest Spend Director with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="ce005-106">Můžete řídit ve službě Azure AD, který má přístup tooSciQuest výdaji ředitel</span><span class="sxs-lookup"><span data-stu-id="ce005-106">You can control in Azure AD who has access tooSciQuest Spend Director</span></span> 
* <span data-ttu-id="ce005-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSciQuest výdaji ředitel (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce005-107">You can enable your users tooautomatically get signed-on tooSciQuest Spend Director (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="ce005-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="ce005-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="ce005-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ce005-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ce005-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ce005-110">Prerequisites</span></span>
<span data-ttu-id="ce005-111">tooconfigure integrace Azure AD s SciQuest tráví ředitel, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ce005-111">tooconfigure Azure AD integration with SciQuest Spend Director, you need hello following items:</span></span>

* <span data-ttu-id="ce005-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce005-112">An Azure AD subscription</span></span>
* <span data-ttu-id="ce005-113">SciQuest tráví ředitel jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ce005-113">A SciQuest Spend Director single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ce005-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce005-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="ce005-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ce005-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="ce005-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="ce005-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="ce005-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ce005-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="ce005-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ce005-118">Scenario Description</span></span>
<span data-ttu-id="ce005-119">cílem Hello tohoto kurzu je tooenable tootest Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce005-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="ce005-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ce005-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ce005-121">Přidání SciQuest tráví ředitel z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ce005-121">Adding SciQuest Spend Director from hello gallery</span></span> 
2. <span data-ttu-id="ce005-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce005-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sciquest-spend-director-from-hello-gallery"></a><span data-ttu-id="ce005-123">Přidání SciQuest tráví ředitel z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ce005-123">Adding SciQuest Spend Director from hello gallery</span></span>
<span data-ttu-id="ce005-124">tooconfigure hello integrace SciQuest tráví ředitel do Azure AD, je nutné tooadd SciQuest tráví ředitel hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ce005-124">tooconfigure hello integration of SciQuest Spend Director into Azure AD, you need tooadd SciQuest Spend Director from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ce005-125">**tooadd SciQuest tráví ředitel z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ce005-125">**tooadd SciQuest Spend Director from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ce005-126">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ce005-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="ce005-128">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="ce005-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="ce005-129">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ce005-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Aplikace][2]

4. <span data-ttu-id="ce005-131">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="ce005-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Aplikace][3]

5. <span data-ttu-id="ce005-133">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="ce005-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Aplikace][4]

6. <span data-ttu-id="ce005-135">Hello vyhledávacího pole zadejte **sciQuest tráví ředitel**.</span><span class="sxs-lookup"><span data-stu-id="ce005-135">In hello search box, type **sciQuest spend director**.</span></span>
   
    ![Aplikace][5]

7. <span data-ttu-id="ce005-137">V podokně výsledků hello, vyberte **SciQuest tráví ředitel**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce005-137">In hello results pane, select **SciQuest Spend Director**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Aplikace][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ce005-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce005-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ce005-140">Hello cílem této části je tooshow můžete jak tooconfigure a testování Azure AD jednotné přihlašování s SciQuest tráví ředitel podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ce005-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with SciQuest Spend Director based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ce005-141">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel hello protějškem v SciQuest tráví ředitel tooan uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ce005-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SciQuest Spend Director tooan user in Azure AD is.</span></span> <span data-ttu-id="ce005-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SciQuest tráví ředitel musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ce005-142">In other words, a link relationship between an Azure AD user and hello related user in SciQuest Spend Director needs toobe established.</span></span>  
<span data-ttu-id="ce005-143">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="ce005-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SciQuest Spend Director.</span></span>

<span data-ttu-id="ce005-144">tooconfigure a testu Azure AD jednotné přihlašování s SciQuest tráví ředitel, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="ce005-144">tooconfigure and test Azure AD single sign-on with SciQuest Spend Director, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ce005-145">**[Konfigurace Azure AD jeden jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ce005-145">**[Configuring Azure AD Single Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ce005-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ce005-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ce005-147">**[Vytvoření zkušebního uživatele ředitel tráví SciQuest](#creating-a-halogen-software-test-user)**  -toohave protějšek Britta Simon v tráví ředitel SciQuest, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ce005-147">**[Creating a SciQuest Spend Director test user](#creating-a-halogen-software-test-user)** - toohave a counterpart of Britta Simon in SciQuest Spend Director that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="ce005-148">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ce005-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ce005-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ce005-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-single-sign-on"></a><span data-ttu-id="ce005-150">Konfigurace Azure AD jednoho jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce005-150">Configuring Azure AD Single Single Sign-On</span></span>
<span data-ttu-id="ce005-151">Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure classic a tooconfigure jednotné přihlašování v aplikaci SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="ce005-151">hello objective of this section is tooenable Azure AD single sign-on in hello Azure classic portal and tooconfigure single sign-on in your SciQuest Spend Director application.</span></span>

<span data-ttu-id="ce005-152">**tooconfigure Azure AD jednotné přihlašování s SciQuest tráví ředitel, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce005-152">**tooconfigure Azure AD single sign-on with SciQuest Spend Director, perform hello following steps:**</span></span>

1. <span data-ttu-id="ce005-153">V portálu Azure classic, na hello hello **SciQuest tráví ředitel** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce005-153">In hello Azure classic portal, on hello **SciQuest Spend Director** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][8]

2. <span data-ttu-id="ce005-155">Na hello **jak jste by například uživatelé toosign na tooSciQuest výdaji ředitel** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ce005-155">On hello **How would you like users toosign on tooSciQuest Spend Director** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][9]

3. <span data-ttu-id="ce005-157">Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce005-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span> 
   
    ![Konfigurovat nastavení aplikace][10]
   
     <span data-ttu-id="ce005-159">a.</span><span class="sxs-lookup"><span data-stu-id="ce005-159">a.</span></span> <span data-ttu-id="ce005-160">V hello **přihlašovací adresa URL** textové pole, zadejte adresu URL používá vaši uživatelé toosign na tooyour SciQuest tráví ředitel aplikace pomocí hello následující vzor: *https://.* SciQuest.com/.**</span><span class="sxs-lookup"><span data-stu-id="ce005-160">In hello **Sign On URL** textbox, type your URL used by your users toosign on tooyour SciQuest Spend Director application using hello following pattern: *https://.*sciquest.com/.**</span></span>
   
     <span data-ttu-id="ce005-161">b.</span><span class="sxs-lookup"><span data-stu-id="ce005-161">b.</span></span> <span data-ttu-id="ce005-162">V hello **adresa URL odpovědi** textové pole, typ hello stejnou hodnotou, kterou jste zadali do hello **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ce005-162">In hello **Reply URL** textbox, type hello same value you have typed into hello **Sign On URL** textbox.</span></span> 
   
     <span data-ttu-id="ce005-163">c.</span><span class="sxs-lookup"><span data-stu-id="ce005-163">c.</span></span> <span data-ttu-id="ce005-164">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ce005-164">Click **Next**.</span></span>

4. <span data-ttu-id="ce005-165">Na hello **nakonfigurovat jednotné přihlašování v ředitel tráví SciQuest** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat hello místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ce005-165">On hello **Configure single sign-on at SciQuest Spend Director** page, click **Download metadata**, and then save hello metadata file locally on your computer.</span></span>
   
    ![Co je služba Azure AD Connect][11]

5. <span data-ttu-id="ce005-167">Obraťte se na podporu tooenable SciQuest tuto metodu ověřování pomocí metadat hello výše stáhli.</span><span class="sxs-lookup"><span data-stu-id="ce005-167">Contact SciQuest support tooenable this authentication method using hello above downloaded metadata.</span></span>

6. <span data-ttu-id="ce005-168">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce005-168">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span> 
   
    ![Co je služba Azure AD Connect][15]

7. <span data-ttu-id="ce005-170">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="ce005-170">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ce005-171">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce005-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="ce005-172">Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="ce005-172">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="ce005-173">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ce005-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ce005-174">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ce005-174">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Co je služba Azure AD Connect][100] 

2. <span data-ttu-id="ce005-176">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="ce005-176">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="ce005-177">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ce005-177">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Co je služba Azure AD Connect][101] 

4. <span data-ttu-id="ce005-179">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ce005-179">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Co je služba Azure AD Connect][102] 

5. <span data-ttu-id="ce005-181">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce005-181">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Co je služba Azure AD Connect][103] 
   
    <span data-ttu-id="ce005-183">a.</span><span class="sxs-lookup"><span data-stu-id="ce005-183">a.</span></span> <span data-ttu-id="ce005-184">Jako **typ uživatele**, vyberte **nového uživatele ve vaší organizaci**.</span><span class="sxs-lookup"><span data-stu-id="ce005-184">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="ce005-185">b.</span><span class="sxs-lookup"><span data-stu-id="ce005-185">b.</span></span> <span data-ttu-id="ce005-186">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ce005-186">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="ce005-187">c.</span><span class="sxs-lookup"><span data-stu-id="ce005-187">c.</span></span> <span data-ttu-id="ce005-188">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ce005-188">Click **Next**.</span></span>

6. <span data-ttu-id="ce005-189">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce005-189">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Co je služba Azure AD Connect][104] 
   
    <span data-ttu-id="ce005-191">a.</span><span class="sxs-lookup"><span data-stu-id="ce005-191">a.</span></span> <span data-ttu-id="ce005-192">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="ce005-192">In hello **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="ce005-193">b.</span><span class="sxs-lookup"><span data-stu-id="ce005-193">b.</span></span> <span data-ttu-id="ce005-194">V hello **příjmení** txtbox, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="ce005-194">In hello **Last Name** txtbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="ce005-195">c.</span><span class="sxs-lookup"><span data-stu-id="ce005-195">c.</span></span> <span data-ttu-id="ce005-196">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ce005-196">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="ce005-197">d.</span><span class="sxs-lookup"><span data-stu-id="ce005-197">d.</span></span> <span data-ttu-id="ce005-198">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ce005-198">In hello **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="ce005-199">e.</span><span class="sxs-lookup"><span data-stu-id="ce005-199">e.</span></span> <span data-ttu-id="ce005-200">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ce005-200">Click **Next**.</span></span>

7. <span data-ttu-id="ce005-201">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ce005-201">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Co je služba Azure AD Connect][105]  

8. <span data-ttu-id="ce005-203">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce005-203">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Co je služba Azure AD Connect][106]   
   
    <span data-ttu-id="ce005-205">a.</span><span class="sxs-lookup"><span data-stu-id="ce005-205">a.</span></span> <span data-ttu-id="ce005-206">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="ce005-206">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="ce005-207">b.</span><span class="sxs-lookup"><span data-stu-id="ce005-207">b.</span></span> <span data-ttu-id="ce005-208">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="ce005-208">Click **Complete**.</span></span>   

### <a name="creating-a-sciquest-spend-director-test-user"></a><span data-ttu-id="ce005-209">Vytvoření zkušebního uživatele SciQuest tráví ředitel</span><span class="sxs-lookup"><span data-stu-id="ce005-209">Creating a SciQuest Spend Director test user</span></span>
<span data-ttu-id="ce005-210">Hello cílem této části je toocreate volal Britta Simon v SciQuest tráví ředitel uživatele.</span><span class="sxs-lookup"><span data-stu-id="ce005-210">hello objective of this section is toocreate a user called Britta Simon in SciQuest Spend Director.</span></span>

<span data-ttu-id="ce005-211">Potřebujete toocontact váš tým podpory SciQuest tráví ředitele a jim poskytnout hello podrobnosti o váš účet tooget test, vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ce005-211">You need toocontact your SciQuest Spend Director support team and provide them with hello details about your test account tooget it created.</span></span>

<span data-ttu-id="ce005-212">Alternativně můžete využít i za běhu zřizování, jeden přihlašování funkce, která podporuje SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="ce005-212">Alternatively, you can also leverage just-in-time provisioning, a single sign-on feature that is supported by SciQuest Spend Director.</span></span>  
<span data-ttu-id="ce005-213">Pokud za běhu zřizování je povolená, uživatelé jsou automaticky vytváří SciQuest tráví ředitel během jednoho pokusu o přihlášení Pokud ještě neexistují.</span><span class="sxs-lookup"><span data-stu-id="ce005-213">If just-in-time provisioning is enabled, users are automatically created by SciQuest Spend Director during a single sign-on attempt if they don't exist.</span></span> <span data-ttu-id="ce005-214">Tato funkce eliminuje nutnost hello toomanually vytváření protějšku přihlašování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ce005-214">This feature eliminates hello need toomanually create single sign-on counterpart users.</span></span>

<span data-ttu-id="ce005-215">zřizování tooget za běhu povolena, je nutné toocontact jste váš tým podpory SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="ce005-215">tooget just-in-time provisioning enabled, you need toocontact your your SciQuest Spend Director support team.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ce005-216">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ce005-216">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="ce005-217">Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování, poskytněte svůj přístup tooSciQuest výdaji ředitel.</span><span class="sxs-lookup"><span data-stu-id="ce005-217">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooSciQuest Spend Director.</span></span>

![Co je služba Azure AD Connect][200]

<span data-ttu-id="ce005-219">**tooassign Britta Simon tooSciQuest výdaji ředitel, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce005-219">**tooassign Britta Simon tooSciQuest Spend Director, perform hello following steps:**</span></span>

1. <span data-ttu-id="ce005-220">Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ce005-220">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Co je služba Azure AD Connect][201]

2. <span data-ttu-id="ce005-222">V seznamu aplikace hello vyberte **SciQuest tráví ředitel**.</span><span class="sxs-lookup"><span data-stu-id="ce005-222">In hello applications list, select **SciQuest Spend Director**.</span></span>
   
    ![Co je služba Azure AD Connect][202]

3. <span data-ttu-id="ce005-224">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ce005-224">In hello menu on hello top, click **Users**.</span></span>
   
    ![Co je služba Azure AD Connect][203]

4. <span data-ttu-id="ce005-226">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ce005-226">In hello Users list, select **Britta Simon**.</span></span>
   
    ![Co je služba Azure AD Connect][204]

5. <span data-ttu-id="ce005-228">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="ce005-228">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Co je služba Azure AD Connect][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="ce005-230">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce005-230">Testing Single Sign-On</span></span>
<span data-ttu-id="ce005-231">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ce005-231">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="ce005-232">Když kliknete na dlaždici SciQuest tráví ředitel hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SciQuest tráví ředitel aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce005-232">When you click hello SciQuest Spend Director tile in hello Access Panel, you should get automatically signed-on tooyour SciQuest Spend Director application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ce005-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ce005-233">Additional Resources</span></span>
* [<span data-ttu-id="ce005-234">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ce005-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ce005-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ce005-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

