---
title: "Kurz: Azure Active Directory integrace s Wizergos produkční Software | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi softwarem Wizergos produktivitu a Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: cdd186c38c426dde404ed8bb84700faf9c8c1a46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a><span data-ttu-id="7b214-103">Kurz: Azure Active Directory integrace s Wizergos produktivitu softwaru</span><span class="sxs-lookup"><span data-stu-id="7b214-103">Tutorial: Azure Active Directory integration with Wizergos Productivity Software</span></span>
<span data-ttu-id="7b214-104">cílem Hello tohoto kurzu je tooshow můžete jak toointegrate Wizergos produktivitu Software s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7b214-104">hello objective of this tutorial is tooshow you how toointegrate Wizergos Productivity Software  with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7b214-105">Integrace Wizergos produkční Software s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7b214-105">Integrating Wizergos Productivity Software with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="7b214-106">Můžete řídit ve službě Azure AD, který má přístup tooWizergos produktivitu softwaru</span><span class="sxs-lookup"><span data-stu-id="7b214-106">You can control in Azure AD who has access tooWizergos Productivity Software</span></span>
* <span data-ttu-id="7b214-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWizergos produktivitu softwaru jednotné přihlašování (SSO) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7b214-107">You can enable your users tooautomatically get signed-on tooWizergos Productivity Software single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="7b214-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="7b214-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="7b214-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7b214-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b214-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7b214-110">Prerequisites</span></span>
<span data-ttu-id="7b214-111">tooconfigure integrace Azure AD s Wizergos produktivitu softwaru, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7b214-111">tooconfigure Azure AD integration with Wizergos Productivity Software, you need hello following items:</span></span>

* <span data-ttu-id="7b214-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7b214-112">An Azure AD subscription</span></span>
* <span data-ttu-id="7b214-113">Povolené předplatné produktivity Wizergos, jednotného přihlašování k softwaru</span><span class="sxs-lookup"><span data-stu-id="7b214-113">A Wizergos Productivity Software SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="7b214-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7b214-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="7b214-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7b214-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="7b214-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="7b214-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="7b214-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7b214-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7b214-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7b214-118">Scenario description</span></span>
<span data-ttu-id="7b214-119">cílem Hello tohoto kurzu je tooenable jste tootest Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7b214-119">hello objective of this tutorial is tooenable you tootest Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="7b214-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7b214-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7b214-121">Přidání softwaru produktivitu Wizergos z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7b214-121">Adding Wizergos Productivity Software from hello gallery</span></span>
2. <span data-ttu-id="7b214-122">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="7b214-122">Configuring and testing Azure AD SSO</span></span>

## <a name="adding-wizergos-productivity-software-from-hello-gallery"></a><span data-ttu-id="7b214-123">Přidání softwaru produktivitu Wizergos z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7b214-123">Adding Wizergos Productivity Software from hello gallery</span></span>
<span data-ttu-id="7b214-124">tooconfigure hello integrace Wizergos produktivitu softwaru do služby Azure AD, je nutné tooadd Wizergos produktivitu softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7b214-124">tooconfigure hello integration of Wizergos Productivity Software into Azure AD, you need tooadd Wizergos Productivity Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7b214-125">**tooadd Wizergos produktivitu softwaru z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7b214-125">**tooadd Wizergos Productivity Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b214-126">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7b214-126">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="7b214-128">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="7b214-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="7b214-129">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="7b214-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Aplikace][2]
4. <span data-ttu-id="7b214-131">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="7b214-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Aplikace][3]
5. <span data-ttu-id="7b214-133">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="7b214-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Aplikace][4]
6. <span data-ttu-id="7b214-135">Hello vyhledávacího pole zadejte **Wizergos produktivitu softwaru**.</span><span class="sxs-lookup"><span data-stu-id="7b214-135">In hello search box, type **Wizergos Productivity Software**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. <span data-ttu-id="7b214-137">Na panelu výsledků hello vyberte **Wizergos produktivitu softwaru**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b214-137">In hello results panel, select **Wizergos Productivity Software**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Výběr aplikace hello v galerii hello](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a><span data-ttu-id="7b214-139">Konfigurace a otestování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="7b214-139">Configure and test Azure AD SSO</span></span>
<span data-ttu-id="7b214-140">Hello cílem této části je tooshow vám jak tooconfigure a testování Azure AD přihlášení SSO se Wizergos produktivitu softwaru na základě testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7b214-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD SSO with Wizergos Productivity Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7b214-141">Pro jednotné přihlašování toowork Azure AD musí tooknow, co uživatel hello protějškem v softwaru produktivitu Wizergos tooan uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7b214-141">For SSO toowork, Azure AD needs tooknow what hello counterpart user in Wizergos Productivity Software tooan user in Azure AD is.</span></span> <span data-ttu-id="7b214-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v softwaru produktivitu Wizergos musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="7b214-142">In other words, a link relationship between an Azure AD user and hello related user in Wizergos Productivity Software needs toobe established.</span></span>

<span data-ttu-id="7b214-143">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Wizergos produktivitu softwaru.</span><span class="sxs-lookup"><span data-stu-id="7b214-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Wizergos Productivity Software.</span></span>

<span data-ttu-id="7b214-144">tooconfigure a testu Azure AD jednotné přihlašování s Softwareder BynWizergos produktivitu, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="7b214-144">tooconfigure and test Azure AD single sign-on with BynWizergos Productivity Softwareder, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7b214-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7b214-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7b214-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7b214-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7b214-147">**[Vytvoření zkušebního uživatele softwaru produktivitu Wizergos](#creating-a-wizergos-productivity-software-test-user)**  -toohave protějšek Britta Simon v Wizergos produktivitu softwaru, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7b214-147">**[Creating a Wizergos Productivity Software test user](#creating-a-wizergos-productivity-software-test-user)** - toohave a counterpart of Britta Simon in Wizergos Productivity Software that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="7b214-148">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7b214-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7b214-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="7b214-149">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="7b214-150">Konfigurace Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7b214-150">Configuring Azure AD SSO</span></span>
<span data-ttu-id="7b214-151">V této části můžete povolit Azure AD jednotné přihlašování v portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Wizergos produktivitu softwaru.</span><span class="sxs-lookup"><span data-stu-id="7b214-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your Wizergos Productivity Software application.</span></span>

<span data-ttu-id="7b214-152">**tooconfigure Azure AD jednotné přihlašování s Wizergos produktivitu softwarem, provést hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7b214-152">**tooconfigure Azure AD single sign-on with Wizergos Productivity Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b214-153">Na portálu classic hello na hello **Wizergos produktivitu softwaru** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7b214-153">In hello classic portal, on hello **Wizergos Productivity Software** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][6] 
2. <span data-ttu-id="7b214-155">Na hello **jak jste by například uživatelé toosign na tooWizergos produktivitu softwaru** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**:</span><span class="sxs-lookup"><span data-stu-id="7b214-155">On hello **How would you like users toosign on tooWizergos Productivity Software** page, select **Azure AD Single Sign-On**, and then click **Next**:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. <span data-ttu-id="7b214-157">Na hello **nakonfigurovat nastavení aplikace** dialogové okno stránky, klikněte na tlačítko **Další**:</span><span class="sxs-lookup"><span data-stu-id="7b214-157">On hello **Configure App Settings** dialog page, click **Next**:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. <span data-ttu-id="7b214-159">Na hello **nakonfigurovat jednotné přihlašování v softwaru produktivitu Wizergos** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor hello ve vašem počítači:</span><span class="sxs-lookup"><span data-stu-id="7b214-159">On hello **Configure single sign-on at Wizergos Productivity Software** page, click **Download certificate**, and then save hello file on your computer:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. <span data-ttu-id="7b214-161">V okně prohlížeče jiný web, Wizergos produkční Software klienta tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="7b214-161">In a different web browser window, sign-on tooyour Wizergos Productivity Software tenant as an administrator.</span></span>
6. <span data-ttu-id="7b214-162">Hello hamburger nabídce vyberte **správce**.</span><span class="sxs-lookup"><span data-stu-id="7b214-162">From hello hamburger menu, select **Admin**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. <span data-ttu-id="7b214-164">Správce stránce v nabídce vlevo vyberte **ověřování** a klikněte na **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="7b214-164">In Admin page on left hand menu select **AUTHENTICATION** and click on **Azure AD**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. <span data-ttu-id="7b214-166">Proveďte následující kroky hello **ověřování** části.</span><span class="sxs-lookup"><span data-stu-id="7b214-166">Perform hello following steps on **AUTHENTICATION** section.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. <span data-ttu-id="7b214-168">Klikněte na tlačítko **NAHRÁT** hello tooupload tlačítko Stáhnout certifikát z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7b214-168">Click **UPLOAD** button tooupload hello downloaded certificate from Azure AD.</span></span> 
  2. <span data-ttu-id="7b214-169">V hello **URL vystavitele** textbox put hello hodnotu **URL vystavitele** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7b214-169">In hello **Issuer URL** textbox put hello value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
  3. <span data-ttu-id="7b214-170">V hello **adresy jednotného přihlašování** textbox put hello hodnotu **jeden přihlašování adresa URL služby** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7b214-170">In hello **Single Sign-On URL** textbox put hello value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
  4. <span data-ttu-id="7b214-171">V hello **jednu adresu URL Sign-Out** textbox put hello hodnotu **jednu adresu URL služby Sign-out** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7b214-171">In hello **Single Sign-Out URL** textbox put hello value of **Single Sign-out Service URL** from Azure AD application configuration wizard.</span></span>
  5. <span data-ttu-id="7b214-172">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7b214-172">Click **Save** button.</span></span>
9. <span data-ttu-id="7b214-173">Hello portálu classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7b214-173">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][10]
10. <span data-ttu-id="7b214-175">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="7b214-175">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
    
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="7b214-177">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7b214-177">Create an Azure AD test user</span></span>
<span data-ttu-id="7b214-178">Hello cílem této části je toocreate testovacího uživatele na portálu classic hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7b214-178">hello objective of this section is toocreate a test user in hello classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="7b214-180">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7b214-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b214-181">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7b214-181">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="7b214-183">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="7b214-183">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="7b214-184">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7b214-184">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="7b214-186">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="7b214-186">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="7b214-188">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7b214-188">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="7b214-190">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="7b214-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="7b214-191">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7b214-191">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="7b214-192">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7b214-192">Click **Next**.</span></span>
6. <span data-ttu-id="7b214-193">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7b214-193">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="7b214-195">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="7b214-195">In hello **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="7b214-196">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="7b214-196">In hello **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="7b214-197">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="7b214-197">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="7b214-198">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="7b214-198">In hello **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="7b214-199">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7b214-199">Click **Next**.</span></span>
7. <span data-ttu-id="7b214-200">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7b214-200">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="7b214-202">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7b214-202">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. <span data-ttu-id="7b214-204">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="7b214-204">Write down hello value of hello **New Password**.</span></span>
  2. <span data-ttu-id="7b214-205">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="7b214-205">Click **Complete**.</span></span>   

### <a name="create-a-wizergos-productivity-software-test-user"></a><span data-ttu-id="7b214-206">Vytvoření zkušebního uživatele Wizergos produktivitu softwaru</span><span class="sxs-lookup"><span data-stu-id="7b214-206">Create a Wizergos Productivity Software test user</span></span>
<span data-ttu-id="7b214-207">V této části vytvoříte volal Britta Simon v softwaru Wizergos produktivitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="7b214-207">In this section, you create a user called Britta Simon in Wizergos Productivity Software.</span></span> <span data-ttu-id="7b214-208">Spojte se s tým podpory Wizergos produkční Software prostřednictvím [ support@wizergos.com ](emailTo:support@wizergos.com) tooadd hello uživatelé v platformě Wizergos produktivitu softwaru hello.</span><span class="sxs-lookup"><span data-stu-id="7b214-208">Please work with Wizergos Productivity Software support team via [support@wizergos.com](emailTo:support@wizergos.com) tooadd hello users in hello Wizergos Productivity Software platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="7b214-209">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7b214-209">Assign hello Azure AD test user</span></span>
<span data-ttu-id="7b214-210">Hello cílem této části je tooenabling toouse Britta Simon jednotného přihlašování k Azure tak, že udělíte tooWizergos svůj přístup k produkční Software.</span><span class="sxs-lookup"><span data-stu-id="7b214-210">hello objective of this section is tooenabling Britta Simon toouse Azure SSO by granting her access tooWizergos Productivity Software.</span></span>

  ![Přiřadit uživatele][200]

<span data-ttu-id="7b214-212">**tooassign Britta Simon tooWizergos Software produktivitu, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7b214-212">**tooassign Britta Simon tooWizergos Productivity Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b214-213">Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="7b214-213">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Přiřadit uživatele][201]
2. <span data-ttu-id="7b214-215">V seznamu aplikace hello vyberte **Wizergos produktivitu softwaru**.</span><span class="sxs-lookup"><span data-stu-id="7b214-215">In hello applications list, select **Wizergos Productivity Software**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. <span data-ttu-id="7b214-217">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7b214-217">In hello menu on hello top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][203]
4. <span data-ttu-id="7b214-219">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="7b214-219">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="7b214-220">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="7b214-220">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="7b214-222">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7b214-222">Test single sign-on</span></span>
<span data-ttu-id="7b214-223">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="7b214-223">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="7b214-224">Když kliknete na dlaždici Wizergos produktivitu softwaru hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Wizergos produktivitu softwarová aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b214-224">When you click hello Wizergos Productivity Software tile in hello Access Panel, you should get automatically signed-on tooyour Wizergos Productivity Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b214-225">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7b214-225">Additional resources</span></span>
* [<span data-ttu-id="7b214-226">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7b214-226">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7b214-227">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7b214-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
