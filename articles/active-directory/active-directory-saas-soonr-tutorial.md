---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti Soonr | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Soonr síti na pracovišti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: f950b45d0beceab2fa17b7690c9de81ec6603089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a><span data-ttu-id="244f9-103">Kurz: Azure Active Directory integrace s Soonr síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="244f9-103">Tutorial: Azure Active Directory integration with Soonr Workplace</span></span>

<span data-ttu-id="244f9-104">cílem Hello tohoto kurzu je tooshow můžete jak toointegrate Soonr síti na pracovišti s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="244f9-104">hello objective of this tutorial is tooshow you how toointegrate Soonr Workplace with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="244f9-105">Integrace s Azure AD Soonr síti na pracovišti vám poskytne hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="244f9-105">Integrating Soonr Workplace with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="244f9-106">Můžete řídit ve službě Azure AD, který má tooSoonr přístup k síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="244f9-106">You can control in Azure AD who has access tooSoonr Workplace</span></span>
- <span data-ttu-id="244f9-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSoonr síti na pracovišti (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="244f9-107">You can enable your users tooautomatically get signed-on tooSoonr Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="244f9-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="244f9-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="244f9-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="244f9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="244f9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="244f9-110">Prerequisites</span></span>

<span data-ttu-id="244f9-111">tooconfigure integrace Azure AD s Soonr síti na pracovišti, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="244f9-111">tooconfigure Azure AD integration with Soonr Workplace, you need hello following items:</span></span>

- <span data-ttu-id="244f9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="244f9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="244f9-113">Síti na pracovišti Soonr jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="244f9-113">A Soonr Workplace single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="244f9-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="244f9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="244f9-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="244f9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="244f9-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="244f9-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="244f9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="244f9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="244f9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="244f9-118">Scenario description</span></span>
<span data-ttu-id="244f9-119">cílem Hello tohoto kurzu je tooenable tootest Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="244f9-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="244f9-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="244f9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="244f9-121">Přidání Soonr síti na pracovišti z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="244f9-121">Adding Soonr Workplace from hello gallery</span></span>
2. <span data-ttu-id="244f9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="244f9-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-soonr-workplace-from-hello-gallery"></a><span data-ttu-id="244f9-123">Přidání Soonr síti na pracovišti z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="244f9-123">Adding Soonr Workplace from hello gallery</span></span>
<span data-ttu-id="244f9-124">tooconfigure hello integrace síti na pracovišti Soonr do Azure AD, je nutné tooadd síti na pracovišti Soonr hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="244f9-124">tooconfigure hello integration of Soonr Workplace into Azure AD, you need tooadd Soonr Workplace from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="244f9-125">**tooadd Soonr síti na pracovišti z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="244f9-125">**tooadd Soonr Workplace from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="244f9-126">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="244f9-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="244f9-128">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="244f9-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="244f9-129">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="244f9-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Aplikace][2]

4. <span data-ttu-id="244f9-131">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="244f9-131">Click **Add** at hello bottom of hello page.</span></span>

    ![Aplikace][3]

5. <span data-ttu-id="244f9-133">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="244f9-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
 
    ![Aplikace][4]

6. <span data-ttu-id="244f9-135">Hello vyhledávacího pole zadejte **Soonr síti na pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="244f9-135">In hello search box, type **Soonr Workplace**.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. <span data-ttu-id="244f9-137">V podokně výsledků hello, vyberte **síti na pracovišti Soonr**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="244f9-137">In hello results pane, select **Soonr Workplace**, and then click **Complete** tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="244f9-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="244f9-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="244f9-140">Hello cílem této části je tooshow vám jak tooconfigure a testování Azure AD jednotné přihlašování se Soonr pracovišti na základě testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="244f9-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with Soonr Workplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="244f9-141">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel hello protějškem v síti na pracovišti Soonr tooan uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="244f9-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Soonr Workplace tooan user in Azure AD is.</span></span> <span data-ttu-id="244f9-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v síti na pracovišti Soonr musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="244f9-142">In other words, a link relationship between an Azure AD user and hello related user in Soonr Workplace needs toobe established.</span></span>  

<span data-ttu-id="244f9-143">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v síti na pracovišti Soonr.</span><span class="sxs-lookup"><span data-stu-id="244f9-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Soonr Workplace.</span></span>

<span data-ttu-id="244f9-144">tooconfigure a testu Azure AD jednotné přihlašování s Soonr síti na pracovišti, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="244f9-144">tooconfigure and test Azure AD single sign-on with Soonr Workplace, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="244f9-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="244f9-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="244f9-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="244f9-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="244f9-147">**[Vytvoření zkušebního uživatele síti na pracovišti Soonr](#creating-a-soonr-workplace-test-user)**  -toohave protějšek Britta Simon v síti na pracovišti Soonr, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="244f9-147">**[Creating a Soonr Workplace test user](#creating-a-soonr-workplace-test-user)** - toohave a counterpart of Britta Simon in Soonr Workplace that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="244f9-148">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="244f9-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="244f9-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="244f9-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="244f9-150">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="244f9-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="244f9-151">V této části můžete povolit Azure AD jednotné přihlašování v portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Soonr síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="244f9-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your Soonr Workplace application.</span></span>


<span data-ttu-id="244f9-152">**tooconfigure Azure AD jednotné přihlašování s Soonr síti na pracovišti, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="244f9-152">**tooconfigure Azure AD single sign-on with Soonr Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="244f9-153">V portálu Azure classic, na hello hello **síti na pracovišti Soonr** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**  Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="244f9-153">In hello Azure classic portal, on hello **Soonr Workplace** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>

    ![Konfigurovat jednotné přihlašování][6] 

2. <span data-ttu-id="244f9-155">Na hello **jak jste by jako toosign uživatelů v síti na pracovišti tooSoonr** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="244f9-155">On hello **How would you like users toosign on tooSoonr Workplace** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. <span data-ttu-id="244f9-157">Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky hello:.</span><span class="sxs-lookup"><span data-stu-id="244f9-157">On hello **Configure App Settings** dialog page, perform hello following steps:.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    <span data-ttu-id="244f9-159">a.</span><span class="sxs-lookup"><span data-stu-id="244f9-159">a.</span></span> <span data-ttu-id="244f9-160">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzor: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span><span class="sxs-lookup"><span data-stu-id="244f9-160">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span></span>

    <span data-ttu-id="244f9-161">b.</span><span class="sxs-lookup"><span data-stu-id="244f9-161">b.</span></span> <span data-ttu-id="244f9-162">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="244f9-162">Click **Next**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="244f9-163">Upozorňujeme, že se nejedná hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="244f9-163">Please note that this is not hello real value.</span></span> <span data-ttu-id="244f9-164">Máte tooupdate tuto hodnotu s hello skutečné přihlásit na adrese URL.</span><span class="sxs-lookup"><span data-stu-id="244f9-164">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="244f9-165">Obraťte se na tuto hodnotu tooget tým podpory Soonr síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="244f9-165">Contact Soonr Workplace support team tooget this value.</span></span>

4. <span data-ttu-id="244f9-166">Na hello **nakonfigurovat jednotné přihlašování v síti na pracovišti Soonr** klikněte na tlačítko **stáhnout metadata** a uložte soubor hello ve vašem počítači:</span><span class="sxs-lookup"><span data-stu-id="244f9-166">On hello **Configure single sign-on at Soonr Workplace** page, click **Download metadata** and then save hello file on your computer:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. <span data-ttu-id="244f9-168">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Soonr síti na pracovišti a poskytnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="244f9-168">tooget SSO configured for your application, contact your Soonr Workplace support team and provide them with hello following:</span></span> 

    <span data-ttu-id="244f9-169">• hello Stáhnout **Metadata** souboru</span><span class="sxs-lookup"><span data-stu-id="244f9-169">•  hello downloaded **Metadata** file</span></span>

    <span data-ttu-id="244f9-170">• hello **URL vystavitele**</span><span class="sxs-lookup"><span data-stu-id="244f9-170">•  hello **Issuer URL**</span></span>

    <span data-ttu-id="244f9-171">• hello **URL jednotné přihlašování SAML**</span><span class="sxs-lookup"><span data-stu-id="244f9-171">•  hello **SAML SSO URL**</span></span>

    <span data-ttu-id="244f9-172">• hello **jednu adresu URL služby Sign-Out**</span><span class="sxs-lookup"><span data-stu-id="244f9-172">•  hello **Single Sign-Out Service URL**</span></span>

    >[!NOTE]
    ><span data-ttu-id="244f9-173">Tato aplikace je nahrazena <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">síti na pracovišti Autotask</a> a najdete <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">to</a> kurz pro konfiguraci aplikace hello s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="244f9-173">This application is superseded by <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask Workplace</a> and you can refer <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">this</a> tutorial for configuring hello application with Azure AD.</span></span>
   
6. <span data-ttu-id="244f9-174">Hello portál Azure classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="244f9-174">In hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>

    ![Azure AD jednotné přihlášení][10]

7. <span data-ttu-id="244f9-176">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="244f9-176">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
  
    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="244f9-178">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="244f9-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="244f9-179">Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="244f9-179">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>  

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="244f9-181">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="244f9-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="244f9-182">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="244f9-182">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="244f9-184">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="244f9-184">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="244f9-185">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="244f9-185">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="244f9-187">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="244f9-187">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="244f9-189">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="244f9-189">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    <span data-ttu-id="244f9-191">a.</span><span class="sxs-lookup"><span data-stu-id="244f9-191">a.</span></span> <span data-ttu-id="244f9-192">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="244f9-192">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="244f9-193">b.</span><span class="sxs-lookup"><span data-stu-id="244f9-193">b.</span></span> <span data-ttu-id="244f9-194">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="244f9-194">In hello User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="244f9-195">c.</span><span class="sxs-lookup"><span data-stu-id="244f9-195">c.</span></span> <span data-ttu-id="244f9-196">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="244f9-196">Click **Next**.</span></span>

6.  <span data-ttu-id="244f9-197">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="244f9-197">On hello **User Profile** dialog page, perform hello following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    <span data-ttu-id="244f9-199">a.</span><span class="sxs-lookup"><span data-stu-id="244f9-199">a.</span></span> <span data-ttu-id="244f9-200">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="244f9-200">In hello **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="244f9-201">b.</span><span class="sxs-lookup"><span data-stu-id="244f9-201">b.</span></span> <span data-ttu-id="244f9-202">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="244f9-202">In hello **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="244f9-203">c.</span><span class="sxs-lookup"><span data-stu-id="244f9-203">c.</span></span> <span data-ttu-id="244f9-204">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="244f9-204">In hello **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="244f9-205">d.</span><span class="sxs-lookup"><span data-stu-id="244f9-205">d.</span></span> <span data-ttu-id="244f9-206">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="244f9-206">In hello **Role** list, select **User**.</span></span>

    <span data-ttu-id="244f9-207">e.</span><span class="sxs-lookup"><span data-stu-id="244f9-207">e.</span></span> <span data-ttu-id="244f9-208">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="244f9-208">Click **Next**.</span></span>

7. <span data-ttu-id="244f9-209">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="244f9-209">On hello **Get temporary password** dialog page, click **create**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="244f9-211">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="244f9-211">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="244f9-213">a.</span><span class="sxs-lookup"><span data-stu-id="244f9-213">a.</span></span> <span data-ttu-id="244f9-214">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="244f9-214">Write down hello value of hello **New Password**.</span></span>

    <span data-ttu-id="244f9-215">b.</span><span class="sxs-lookup"><span data-stu-id="244f9-215">b.</span></span> <span data-ttu-id="244f9-216">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="244f9-216">Click **Complete**.</span></span>   



### <a name="creating-a-soonr-workplace-test-user"></a><span data-ttu-id="244f9-217">Vytvoření zkušebního uživatele Soonr síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="244f9-217">Creating a Soonr Workplace test user</span></span>

<span data-ttu-id="244f9-218">Hello cílem této části je toocreate volal Britta Simon v síti na pracovišti Soonr uživatele.</span><span class="sxs-lookup"><span data-stu-id="244f9-218">hello objective of this section is toocreate a user called Britta Simon in Soonr Workplace.</span></span> <span data-ttu-id="244f9-219">Spojte se s toocreate tým podpory síti na pracovišti Soonr uživatele v platformě hello.</span><span class="sxs-lookup"><span data-stu-id="244f9-219">Please work with Soonr Workplace support team toocreate a user in hello platform.</span></span> <span data-ttu-id="244f9-220">Můžete zvýšit lístku podpory hello s Soonr z <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">zde</a>.</span><span class="sxs-lookup"><span data-stu-id="244f9-220">You can raise hello support ticket with Soonr from <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">here</a>.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="244f9-221">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="244f9-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="244f9-222">Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování, poskytněte tooSoonr svůj přístup k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="244f9-222">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooSoonr Workplace.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="244f9-224">**tooassign Britta Simon tooSoonr síti na pracovišti, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="244f9-224">**tooassign Britta Simon tooSoonr Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="244f9-225">Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="244f9-225">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="244f9-227">V seznamu aplikace hello vyberte **Soonr síti na pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="244f9-227">In hello applications list, select **Soonr Workplace**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. <span data-ttu-id="244f9-229">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="244f9-229">In hello menu on hello top, click **Users**.</span></span>

    ![Přiřadit uživatele][203] 

1. <span data-ttu-id="244f9-231">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="244f9-231">In hello Users list, select **Britta Simon**.</span></span>

2. <span data-ttu-id="244f9-232">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="244f9-232">In hello toolbar on hello bottom, click **Assign**.</span></span>

    ![Přiřadit uživatele][205]



### <a name="testing-single-sign-on"></a><span data-ttu-id="244f9-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="244f9-234">Testing single sign-on</span></span>

<span data-ttu-id="244f9-235">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="244f9-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="244f9-236">Po kliknutí na tlačítko hello síti na pracovišti Soonr dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace Soonr síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="244f9-236">When you click hello Soonr Workplace tile in hello Access Panel, you should get automatically signed-on tooyour Soonr Workplace application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="244f9-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="244f9-237">Additional resources</span></span>

* [<span data-ttu-id="244f9-238">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="244f9-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="244f9-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="244f9-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
