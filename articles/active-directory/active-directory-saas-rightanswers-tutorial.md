---
title: 'Kurz: Azure Active Directory integrace s RightAnswers | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a RightAnswers."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7f09e25a-a716-41e1-8ca3-fd00e3d1b8cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 745e7ed5a13291afeed8f48a595e95b27d4b0e58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightanswers"></a><span data-ttu-id="b51b5-103">Kurz: Azure Active Directory integrace s RightAnswers</span><span class="sxs-lookup"><span data-stu-id="b51b5-103">Tutorial: Azure Active Directory integration with RightAnswers</span></span>

<span data-ttu-id="b51b5-104">V tomto kurzu zjistíte, jak toointegrate RightAnswers s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b51b5-104">In this tutorial, you learn how toointegrate RightAnswers with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b51b5-105">Integrace RightAnswers s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b51b5-105">Integrating RightAnswers with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b51b5-106">Můžete řídit ve službě Azure AD, který má přístup tooRightAnswers</span><span class="sxs-lookup"><span data-stu-id="b51b5-106">You can control in Azure AD who has access tooRightAnswers</span></span>
- <span data-ttu-id="b51b5-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooRightAnswers (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b51b5-107">You can enable your users tooautomatically get signed-on tooRightAnswers (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b51b5-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b51b5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b51b5-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b51b5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b51b5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b51b5-110">Prerequisites</span></span>

<span data-ttu-id="b51b5-111">Integrace služby Azure AD s RightAnswers tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b51b5-111">tooconfigure Azure AD integration with RightAnswers, you need hello following items:</span></span>

- <span data-ttu-id="b51b5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b51b5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b51b5-113">RightAnswers jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b51b5-113">A RightAnswers single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b51b5-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b51b5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b51b5-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b51b5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b51b5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b51b5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b51b5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b51b5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b51b5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b51b5-118">Scenario description</span></span>
<span data-ttu-id="b51b5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b51b5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b51b5-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b51b5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b51b5-121">Přidání RightAnswers z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b51b5-121">Adding RightAnswers from hello gallery</span></span>
2. <span data-ttu-id="b51b5-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b51b5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rightanswers-from-hello-gallery"></a><span data-ttu-id="b51b5-123">Přidání RightAnswers z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b51b5-123">Adding RightAnswers from hello gallery</span></span>
<span data-ttu-id="b51b5-124">tooconfigure hello integrace RightAnswers do Azure AD, je nutné tooadd RightAnswers hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b51b5-124">tooconfigure hello integration of RightAnswers into Azure AD, you need tooadd RightAnswers from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b51b5-125">**tooadd RightAnswers z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b51b5-125">**tooadd RightAnswers from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b51b5-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b51b5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b51b5-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b51b5-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b51b5-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b51b5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b51b5-133">Hello vyhledávacího pole zadejte **RightAnswers**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-133">In hello search box, type **RightAnswers**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_search.png)

5. <span data-ttu-id="b51b5-135">Na panelu výsledků hello vyberte **RightAnswers**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b51b5-135">In hello results panel, select **RightAnswers**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b51b5-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b51b5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b51b5-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s RightAnswers podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b51b5-138">In this section, you configure and test Azure AD single sign-on with RightAnswers based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b51b5-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v RightAnswers je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b51b5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RightAnswers is tooa user in Azure AD.</span></span> <span data-ttu-id="b51b5-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v RightAnswers musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b51b5-140">In other words, a link relationship between an Azure AD user and hello related user in RightAnswers needs toobe established.</span></span>

<span data-ttu-id="b51b5-141">V RightAnswers, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b51b5-141">In RightAnswers, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b51b5-142">tooconfigure a testu Azure AD jednotné přihlašování s RightAnswers, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b51b5-142">tooconfigure and test Azure AD single sign-on with RightAnswers, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b51b5-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b51b5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b51b5-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b51b5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b51b5-145">**[Vytvoření zkušebního uživatele RightAnswers](#creating-a-rightanswers-test-user)**  -toohave protějšek Britta Simon v RightAnswers, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b51b5-145">**[Creating a RightAnswers test user](#creating-a-rightanswers-test-user)** - toohave a counterpart of Britta Simon in RightAnswers that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b51b5-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b51b5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b51b5-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b51b5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b51b5-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b51b5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b51b5-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci RightAnswers.</span><span class="sxs-lookup"><span data-stu-id="b51b5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RightAnswers application.</span></span>

<span data-ttu-id="b51b5-150">**tooconfigure Azure AD jednotné přihlašování s RightAnswers, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b51b5-150">**tooconfigure Azure AD single sign-on with RightAnswers, perform hello following steps:**</span></span>

1. <span data-ttu-id="b51b5-151">V portálu Azure, na hello hello **RightAnswers** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-151">In hello Azure portal, on hello **RightAnswers** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b51b5-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b51b5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_samlbase.png)

3. <span data-ttu-id="b51b5-155">Na hello **RightAnswers domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b51b5-155">On hello **RightAnswers Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_url.png)

    <span data-ttu-id="b51b5-157">a.</span><span class="sxs-lookup"><span data-stu-id="b51b5-157">a.</span></span> <span data-ttu-id="b51b5-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.rightanswers.com/portal/ss/`</span><span class="sxs-lookup"><span data-stu-id="b51b5-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.rightanswers.com/portal/ss/`</span></span>

    <span data-ttu-id="b51b5-159">b.</span><span class="sxs-lookup"><span data-stu-id="b51b5-159">b.</span></span> <span data-ttu-id="b51b5-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.rightanswers.com:<identifier>/portal`</span><span class="sxs-lookup"><span data-stu-id="b51b5-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.rightanswers.com:<identifier>/portal`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b51b5-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b51b5-161">These values are not real.</span></span> <span data-ttu-id="b51b5-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b51b5-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b51b5-163">Obraťte se na [tým podpory RightAnswers klienta](https://www.rightanswers.com/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b51b5-163">Contact [RightAnswers Client support team](https://www.rightanswers.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="b51b5-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b51b5-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_certificate.png) 

5. <span data-ttu-id="b51b5-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b51b5-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightanswers-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b51b5-168">tooconfigure jednotného přihlašování na **RightAnswers** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory RightAnswers](https://www.rightanswers.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="b51b5-168">tooconfigure single sign-on on **RightAnswers** side, you need toosend hello downloaded **Metadata XML** too[RightAnswers support team](https://www.rightanswers.com/contact-us/).</span></span>

    >[!NOTE]
    ><span data-ttu-id="b51b5-169">Váš tým podpory RightAnswers má toodo hello skutečné jednotné přihlašování konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b51b5-169">Your RightAnswers support team has toodo hello actual SSO configuration.</span></span>
    ><span data-ttu-id="b51b5-170">Zobrazí se oznámení, když bylo povoleno jednotné přihlašování pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="b51b5-170">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="b51b5-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b51b5-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b51b5-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b51b5-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b51b5-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b51b5-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b51b5-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b51b5-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="b51b5-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b51b5-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b51b5-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b51b5-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b51b5-178">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b51b5-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b51b5-180">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b51b5-182">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b51b5-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b51b5-184">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b51b5-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b51b5-186">a.</span><span class="sxs-lookup"><span data-stu-id="b51b5-186">a.</span></span> <span data-ttu-id="b51b5-187">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b51b5-188">b.</span><span class="sxs-lookup"><span data-stu-id="b51b5-188">b.</span></span> <span data-ttu-id="b51b5-189">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b51b5-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b51b5-190">c.</span><span class="sxs-lookup"><span data-stu-id="b51b5-190">c.</span></span> <span data-ttu-id="b51b5-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b51b5-192">d.</span><span class="sxs-lookup"><span data-stu-id="b51b5-192">d.</span></span> <span data-ttu-id="b51b5-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-193">Click **Create**.</span></span>
 
### <a name="creating-a-rightanswers-test-user"></a><span data-ttu-id="b51b5-194">Vytvoření zkušebního uživatele RightAnswers</span><span class="sxs-lookup"><span data-stu-id="b51b5-194">Creating a RightAnswers test user</span></span>

<span data-ttu-id="b51b5-195">Uživatelé toolog tooenable Azure AD v tooRightAnswers, se musí být zřízená do RightAnswers.</span><span class="sxs-lookup"><span data-stu-id="b51b5-195">tooenable Azure AD users toolog in tooRightAnswers, they must be provisioned into RightAnswers.</span></span> <span data-ttu-id="b51b5-196">Když RightAnswers, zřizování je automatizovaných úloh, takže neexistuje žádná položka akce pro vás.</span><span class="sxs-lookup"><span data-stu-id="b51b5-196">When RightAnswers, provisioning is an automated task so there is no action item for you.</span></span>

<span data-ttu-id="b51b5-197">Uživatelé jsou automaticky vytvářeny v případě potřeby během hello první jeden přihlašování pokus.</span><span class="sxs-lookup"><span data-stu-id="b51b5-197">Users are automatically created if necessary during hello first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="b51b5-198">Můžete použít všechny ostatní RightAnswers uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované RightAnswers tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b51b5-198">You can use any other RightAnswers user account creation tools or APIs provided by RightAnswers tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b51b5-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b51b5-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b51b5-200">V této části povolíte tak, že udělíte přístup tooRightAnswers toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b51b5-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRightAnswers.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b51b5-202">**tooassign Britta Simon tooRightAnswers, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b51b5-202">**tooassign Britta Simon tooRightAnswers, perform hello following steps:**</span></span>

1. <span data-ttu-id="b51b5-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b51b5-205">V seznamu aplikace hello vyberte **RightAnswers**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-205">In hello applications list, select **RightAnswers**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_app.png) 

3. <span data-ttu-id="b51b5-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b51b5-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b51b5-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b51b5-209">Click **Add** button.</span></span> <span data-ttu-id="b51b5-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b51b5-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b51b5-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b51b5-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b51b5-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b51b5-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b51b5-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b51b5-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b51b5-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b51b5-215">Testing single sign-on</span></span>

<span data-ttu-id="b51b5-216">Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b51b5-216">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="b51b5-217">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b51b5-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>
## <a name="additional-resources"></a><span data-ttu-id="b51b5-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b51b5-218">Additional resources</span></span>

* [<span data-ttu-id="b51b5-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b51b5-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b51b5-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b51b5-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_203.png

