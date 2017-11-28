---
title: 'Kurz: Azure Active Directory integrace s Splunk Enterprise a Splunk cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Splunk Enterprise a Splunk cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: 9bb6817cb31dce684cd9cc1c567fa3efc8906ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a><span data-ttu-id="28159-103">Kurz: Azure Active Directory integrace s Splunk Enterprise a Splunk cloudu</span><span class="sxs-lookup"><span data-stu-id="28159-103">Tutorial: Azure Active Directory integration with Splunk Enterprise and Splunk Cloud</span></span>

<span data-ttu-id="28159-104">V tomto kurzu zjistíte, jak toointegrate Splunk Enterprise a Splunk cloudu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="28159-104">In this tutorial, you learn how toointegrate Splunk Enterprise and Splunk Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="28159-105">Integrace Splunk Enterprise a Splunk cloudu s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="28159-105">Integrating Splunk Enterprise and Splunk Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="28159-106">Můžete ovládat ve službě Azure AD, který má přístup tooSplunk Enterprise a Splunk cloudu</span><span class="sxs-lookup"><span data-stu-id="28159-106">You can control in Azure AD who has access tooSplunk Enterprise and Splunk Cloud</span></span>
- <span data-ttu-id="28159-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSplunk Enterprise a Splunk cloudu jednotné přihlašování (SSO) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="28159-107">You can enable your users tooautomatically get signed-on tooSplunk Enterprise and Splunk Cloud single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="28159-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="28159-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="28159-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="28159-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28159-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="28159-110">Prerequisites</span></span>

<span data-ttu-id="28159-111">tooconfigure integrace Azure AD s Splunk Enterprise a Splunk cloudu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="28159-111">tooconfigure Azure AD integration with Splunk Enterprise and Splunk Cloud, you need hello following items:</span></span>

- <span data-ttu-id="28159-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="28159-112">An Azure AD subscription</span></span>
- <span data-ttu-id="28159-113">Splunk Enterprise nebo jednotného přihlašování k cloudu Splunk povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="28159-113">A Splunk Enterprise or Splunk Cloud SSO enabled subscription</span></span>


>[!NOTE]
><span data-ttu-id="28159-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="28159-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="28159-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="28159-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="28159-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="28159-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="28159-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="28159-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="28159-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="28159-118">Scenario description</span></span>
<span data-ttu-id="28159-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="28159-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="28159-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="28159-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="28159-121">Přidání Splunk Enterprise a Splunk cloudu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="28159-121">Adding Splunk Enterprise and Splunk Cloud from hello gallery</span></span>
2. <span data-ttu-id="28159-122">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="28159-122">Configuring and testing Azure AD SSO</span></span>


## <a name="add-splunk-enterprise-and-splunk-cloud-from-hello-gallery"></a><span data-ttu-id="28159-123">Přidat Splunk Enterprise a Splunk cloudu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="28159-123">Add Splunk Enterprise and Splunk Cloud from hello gallery</span></span>
<span data-ttu-id="28159-124">tooconfigure hello integrace Splunk Enterprise a Splunk cloudu do Azure AD, budete muset tooadd Splunk Enterprise a Splunk cloudu hello Galerie tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="28159-124">tooconfigure hello integration of Splunk Enterprise and Splunk Cloud into Azure AD, you need tooadd Splunk Enterprise and Splunk Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="28159-125">**tooadd Splunk Enterprise a Splunk cloudu z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="28159-125">**tooadd Splunk Enterprise and Splunk Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="28159-126">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="28159-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]

2. <span data-ttu-id="28159-128">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="28159-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="28159-129">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="28159-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Aplikace][2]

4. <span data-ttu-id="28159-131">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="28159-131">Click **Add** at hello bottom of hello page.</span></span>

    ![Aplikace][3]

5. <span data-ttu-id="28159-133">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="28159-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>

    ![Aplikace][4]

6. <span data-ttu-id="28159-135">Hello vyhledávacího pole zadejte **Splunk Enterprise nebo Splunk Cloud**.</span><span class="sxs-lookup"><span data-stu-id="28159-135">In hello search box, type **Splunk Enterprise or Splunk Cloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. <span data-ttu-id="28159-137">V podokně výsledků hello, vyberte **Splunk Enterprise a cloudu Splunk**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="28159-137">In hello results pane, select **Splunk Enterprise and Splunk Cloud**, and then click **Complete** tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="28159-139">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="28159-139">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="28159-140">V této části můžete nakonfigurovat a otestovat Azure AD jednotného přihlašování k Splunk organizace a Splunk cloudu podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="28159-140">In this section, you configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="28159-141">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Splunk Enterprise a Splunk cloudu je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="28159-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Splunk Enterprise and Splunk Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="28159-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Splunk Enterprise a Splunk cloudu musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="28159-142">In other words, a link relationship between an Azure AD user and hello related user in Splunk Enterprise and Splunk Cloud needs toobe established.</span></span>

<span data-ttu-id="28159-143">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** Splunk Enterprise a Splunk cloudu.</span><span class="sxs-lookup"><span data-stu-id="28159-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Splunk Enterprise and Splunk Cloud.</span></span>

<span data-ttu-id="28159-144">tooconfigure a testu Azure AD jednotné přihlašování s Splunk Enterprise a Splunk cloudu, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="28159-144">tooconfigure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="28159-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="28159-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="28159-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="28159-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="28159-147">**[Vytvoření zkušebního uživatele Splunk Enterprise a cloudu Splunk](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  -toohave protějšku Britta Simon ve Splunk podniku a Splunk cloudu, který je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="28159-147">**[Creating a Splunk Enterprise and Splunk Cloud test user](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)** - toohave a counterpart of Britta Simon in Splunk Enterprise and Splunk Cloud that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="28159-148">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="28159-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="28159-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="28159-149">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="28159-150">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="28159-150">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="28159-151">V této části můžete povolit Azure AD jednotného přihlašování k portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Splunk Enterprise a Splunk cloudu.</span><span class="sxs-lookup"><span data-stu-id="28159-151">In this section, you enable Azure AD SSO in hello classic portal and configure SSO in your Splunk Enterprise and Splunk Cloud application.</span></span>


<span data-ttu-id="28159-152">**tooconfigure Azure AD jednotné přihlašování s Splunk Enterprise a Splunk cloudu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="28159-152">**tooconfigure Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="28159-153">Na portálu classic hello na hello **Splunk Enterprise a cloudu Splunk** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="28159-153">In hello classic portal, on hello **Splunk Enterprise and Splunk Cloud** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
     
    ![Konfigurovat jednotné přihlašování][6] 

2. <span data-ttu-id="28159-155">Na hello **jakým způsobem uživatelé toosign na tooSplunk Enterprise a Splunk Cloud** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="28159-155">On hello **How would you like users toosign on tooSplunk Enterprise and Splunk Cloud** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. <span data-ttu-id="28159-157">Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="28159-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. <span data-ttu-id="28159-159">V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše uživatele toosign na tooyour Splunk Enterprise a Splunk cloudových aplikací pomocí následujících vzor hello:`https://<splunkserverUrl>/en-US/app/launcher/home`</span><span class="sxs-lookup"><span data-stu-id="28159-159">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour Splunk Enterprise and Splunk Cloud application using hello following pattern: `https://<splunkserverUrl>/en-US/app/launcher/home`</span></span>
  2. <span data-ttu-id="28159-160">V hello **identifikátor** textovému poli, zadat adresu URL hello Splunk serveru.</span><span class="sxs-lookup"><span data-stu-id="28159-160">In hello **Identifier** textbox, type hello URL of your Splunk Server.</span></span>
  3. <span data-ttu-id="28159-161">V hello **adresa URL odpovědi** textovému poli, zadat adresu URL hello s hello následující vzoru:`https://<splunkserver>/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="28159-161">In hello **Reply URL** textbox, type hello URL with hello following pattern: `https://<splunkserver>/saml/acs`</span></span>
  4. <span data-ttu-id="28159-162">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="28159-162">Click **Next**.</span></span>
 
4. <span data-ttu-id="28159-163">Na hello **nakonfigurovat jednotné přihlašování v Splunk Enterprise a Splunk cloudu** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="28159-163">On hello **Configure single sign-on at Splunk Enterprise and Splunk Cloud** page, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. <span data-ttu-id="28159-165">Klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="28159-165">Click **Download metadata**, and then save hello file on your computer.</span></span>
  2. <span data-ttu-id="28159-166">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="28159-166">Click **Next**.</span></span>

5. <span data-ttu-id="28159-167">tooget jednotné přihlašování, které jsou nakonfigurované pro aplikace, obraťte se na Splunk Enterprise a tým podpory Splunk cloudu a poskytnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="28159-167">tooget SSO configured for your application, contact Splunk Enterprise and Splunk Cloud support team and provide them with hello following:</span></span>

    * <span data-ttu-id="28159-168">Hello Stáhnout **federaton metadat**</span><span class="sxs-lookup"><span data-stu-id="28159-168">hello downloaded **federaton metadata**</span></span>
6. <span data-ttu-id="28159-169">Hello portálu classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="28159-169">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Azure AD jednotné přihlášení][10]

7. <span data-ttu-id="28159-171">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="28159-171">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="28159-173">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="28159-173">Create an Azure AD test user</span></span>
<span data-ttu-id="28159-174">V této části vytvoříte na portálu classic hello názvem Britta Simon testovacího uživatele.</span><span class="sxs-lookup"><span data-stu-id="28159-174">In this section, you create a test user in hello classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="28159-176">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="28159-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="28159-177">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="28159-177">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="28159-179">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="28159-179">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="28159-180">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="28159-180">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="28159-182">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="28159-182">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="28159-184">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="28159-184">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="28159-186">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="28159-186">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="28159-187">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="28159-187">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="28159-188">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="28159-188">Click **Next**.</span></span>

6.  <span data-ttu-id="28159-189">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="28159-189">On hello **User Profile** dialog page, perform hello following steps:</span></span>
  
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. <span data-ttu-id="28159-191">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="28159-191">In hello **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="28159-192">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="28159-192">In hello **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="28159-193">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="28159-193">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="28159-194">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="28159-194">In hello **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="28159-195">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="28159-195">Click **Next**.</span></span>

7. <span data-ttu-id="28159-196">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="28159-196">On hello **Get temporary password** dialog page, click **create**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="28159-198">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="28159-198">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. <span data-ttu-id="28159-200">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="28159-200">Write down hello value of hello **New Password**.</span></span>
  2. <span data-ttu-id="28159-201">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="28159-201">Click **Complete**.</span></span>   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a><span data-ttu-id="28159-202">Vytvoření Splunk Enterprise a Splunk cloudu testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="28159-202">Create a Splunk Enterprise and Splunk Cloud test user</span></span>

<span data-ttu-id="28159-203">V této části vytvoříte uživatele volat Britta Simon ve Splunk podniku a Splunk cloudu.</span><span class="sxs-lookup"><span data-stu-id="28159-203">In this section, you create a user called Britta Simon in Splunk Enterprise and Splunk Cloud.</span></span> <span data-ttu-id="28159-204">Spojte se s Splunk Enterprise a Splunk cloudu podporu team tooadd hello uživatele v hello Splunk Enterprise a Splunk Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="28159-204">Please work with Splunk Enterprise and Splunk Cloud support team tooadd hello users in hello Splunk Enterprise and Splunk Cloud platform.</span></span>


### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="28159-205">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="28159-205">Assign hello Azure AD test user</span></span>

<span data-ttu-id="28159-206">V této části povolíte Britta Simon toouse SSOy Azure udělení svůj přístup tooSplunk Enterprise a Splunk cloudu.</span><span class="sxs-lookup"><span data-stu-id="28159-206">In this section, you enable Britta Simon toouse Azure SSOy granting her access tooSplunk Enterprise and Splunk Cloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="28159-208">**tooassign Britta Simon tooSplunk Enterprise a Splunk cloudu, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="28159-208">**tooassign Britta Simon tooSplunk Enterprise and Splunk Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="28159-209">Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="28159-209">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="28159-211">V seznamu aplikace hello vyberte **Splunk Enterprise a Splunk cloudu**.</span><span class="sxs-lookup"><span data-stu-id="28159-211">In hello applications list, select **Splunk Enterprise and Splunk Cloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. <span data-ttu-id="28159-213">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="28159-213">In hello menu on hello top, click **Users**.</span></span>

    ![Přiřadit uživatele][203]

4. <span data-ttu-id="28159-215">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="28159-215">In hello Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="28159-216">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="28159-216">In hello toolbar on hello bottom, click **Assign**.</span></span>

    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="28159-218">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="28159-218">Test single sign-on</span></span>

<span data-ttu-id="28159-219">V této části je otestovat vaše Azure AD SSOonfiguration pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="28159-219">In this section, you test your Azure AD SSOonfiguration using hello Access Panel.</span></span>

<span data-ttu-id="28159-220">Po kliknutí na tlačítko hello Splunk Enterprise Splunk Cloud dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Splunk Enterprise a Splunk cloudové aplikace.</span><span class="sxs-lookup"><span data-stu-id="28159-220">When you click hello Splunk Enterprise and Splunk Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Splunk Enterprise and Splunk Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="28159-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="28159-221">Additional resources</span></span>

* [<span data-ttu-id="28159-222">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="28159-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="28159-223">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="28159-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
