---
title: 'Kurz: Azure Active Directory integrace s Chromeriver | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Chromeriver."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 445c5600-e340-4724-a9cb-3cfaf5770b70
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: 7cf0e36fb6407bf3915e26717a2a997ac13110e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-chromeriver"></a><span data-ttu-id="ef2b9-103">Kurz: Azure Active Directory integrace s Chromeriver</span><span class="sxs-lookup"><span data-stu-id="ef2b9-103">Tutorial: Azure Active Directory integration with Chromeriver</span></span>

<span data-ttu-id="ef2b9-104">V tomto kurzu zjistíte, jak toointegrate Chromeriver s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ef2b9-104">In this tutorial, you learn how toointegrate Chromeriver with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ef2b9-105">Integrace Chromeriver s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-105">Integrating Chromeriver with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ef2b9-106">Můžete řídit ve službě Azure AD, který má přístup tooChromeriver</span><span class="sxs-lookup"><span data-stu-id="ef2b9-106">You can control in Azure AD who has access tooChromeriver</span></span>
- <span data-ttu-id="ef2b9-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooChromeriver (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ef2b9-107">You can enable your users tooautomatically get signed-on tooChromeriver (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ef2b9-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ef2b9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ef2b9-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ef2b9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef2b9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ef2b9-110">Prerequisites</span></span>

<span data-ttu-id="ef2b9-111">Integrace služby Azure AD s Chromeriver tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-111">tooconfigure Azure AD integration with Chromeriver, you need hello following items:</span></span>

- <span data-ttu-id="ef2b9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ef2b9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ef2b9-113">Chromeriver jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ef2b9-113">A Chromeriver single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ef2b9-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ef2b9-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ef2b9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ef2b9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ef2b9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ef2b9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ef2b9-118">Scenario description</span></span>
<span data-ttu-id="ef2b9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ef2b9-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ef2b9-121">Přidání Chromeriver z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ef2b9-121">Adding Chromeriver from hello gallery</span></span>
2. <span data-ttu-id="ef2b9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ef2b9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-chromeriver-from-hello-gallery"></a><span data-ttu-id="ef2b9-123">Přidání Chromeriver z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ef2b9-123">Adding Chromeriver from hello gallery</span></span>
<span data-ttu-id="ef2b9-124">tooconfigure hello integrace Chromeriver do Azure AD, je nutné tooadd Chromeriver hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-124">tooconfigure hello integration of Chromeriver into Azure AD, you need tooadd Chromeriver from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ef2b9-125">**tooadd Chromeriver z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ef2b9-125">**tooadd Chromeriver from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ef2b9-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ef2b9-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ef2b9-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ef2b9-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ef2b9-133">Hello vyhledávacího pole zadejte **Chromeriver**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-133">In hello search box, type **Chromeriver**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_search.png)

5. <span data-ttu-id="ef2b9-135">Na panelu výsledků hello vyberte **Chromeriver**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-135">In hello results panel, select **Chromeriver**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ef2b9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ef2b9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ef2b9-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Chromeriver podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="ef2b9-138">In this section, you configure and test Azure AD single sign-on with Chromeriver based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ef2b9-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Chromeriver je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Chromeriver is tooa user in Azure AD.</span></span> <span data-ttu-id="ef2b9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Chromeriver musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-140">In other words, a link relationship between an Azure AD user and hello related user in Chromeriver needs toobe established.</span></span>

<span data-ttu-id="ef2b9-141">V Chromeriver, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-141">In Chromeriver, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ef2b9-142">tooconfigure a testu Azure AD jednotné přihlašování s Chromeriver, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-142">tooconfigure and test Azure AD single sign-on with Chromeriver, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ef2b9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ef2b9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ef2b9-145">**[Vytvoření zkušebního uživatele Chromeriver](#creating-a-chromeriver-test-user)**  -toohave protějšek Britta Simon v Chromeriver, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-145">**[Creating a Chromeriver test user](#creating-a-chromeriver-test-user)** - toohave a counterpart of Britta Simon in Chromeriver that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ef2b9-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ef2b9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ef2b9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ef2b9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ef2b9-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Chromeriver.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Chromeriver application.</span></span>

<span data-ttu-id="ef2b9-150">**tooconfigure Azure AD jednotné přihlašování s Chromeriver, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ef2b9-150">**tooconfigure Azure AD single sign-on with Chromeriver, perform hello following steps:**</span></span>

1. <span data-ttu-id="ef2b9-151">V portálu Azure, na hello hello **Chromeriver** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-151">In hello Azure portal, on hello **Chromeriver** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ef2b9-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_samlbase.png)

3. <span data-ttu-id="ef2b9-155">Na hello **Chromeriver domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-155">On hello **Chromeriver Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_url.png)

    <span data-ttu-id="ef2b9-157">a.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-157">a.</span></span> <span data-ttu-id="ef2b9-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.chromeriver.com`</span><span class="sxs-lookup"><span data-stu-id="ef2b9-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.chromeriver.com`</span></span>

    <span data-ttu-id="ef2b9-159">b.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-159">b.</span></span> <span data-ttu-id="ef2b9-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.chromeriver.com/login/sso/saml/consume?customerId=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="ef2b9-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.chromeriver.com/login/sso/saml/consume?customerId=<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ef2b9-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-161">These values are not real.</span></span> <span data-ttu-id="ef2b9-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="ef2b9-163">Obraťte se na [tým podpory Chromeriver](https://www.chromeriver.com/services/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-163">Contact [Chromeriver support team](https://www.chromeriver.com/services/support) tooget these values.</span></span>
 


4. <span data-ttu-id="ef2b9-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_certificate.png) 

5. <span data-ttu-id="ef2b9-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-chromeriver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ef2b9-168">tooconfigure jednotného přihlašování na **Chromeriver** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Chromeriver](https://www.chromeriver.com/services/support).</span><span class="sxs-lookup"><span data-stu-id="ef2b9-168">tooconfigure single sign-on on **Chromeriver** side, you need toosend hello downloaded **Metadata XML** too[Chromeriver support team](https://www.chromeriver.com/services/support).</span></span> <span data-ttu-id="ef2b9-169">Zobrazí se oznámení, když bylo povoleno jednotné přihlašování pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-169">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="ef2b9-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ef2b9-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ef2b9-171">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ef2b9-172">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ef2b9-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ef2b9-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ef2b9-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="ef2b9-174">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ef2b9-176">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ef2b9-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ef2b9-177">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ef2b9-179">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ef2b9-181">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ef2b9-183">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ef2b9-185">a.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-185">a.</span></span> <span data-ttu-id="ef2b9-186">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ef2b9-187">b.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-187">b.</span></span> <span data-ttu-id="ef2b9-188">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ef2b9-189">c.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-189">c.</span></span> <span data-ttu-id="ef2b9-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ef2b9-191">d.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-191">d.</span></span> <span data-ttu-id="ef2b9-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-192">Click **Create**.</span></span>
 
### <a name="creating-a-chromeriver-test-user"></a><span data-ttu-id="ef2b9-193">Vytvoření zkušebního uživatele Chromeriver</span><span class="sxs-lookup"><span data-stu-id="ef2b9-193">Creating a Chromeriver test user</span></span>

<span data-ttu-id="ef2b9-194">Uživatelé toolog tooenable Azure AD v tooChromeriver, se musí být zřízená do Chromeriver.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-194">tooenable Azure AD users toolog in tooChromeriver, they must be provisioned into Chromeriver.</span></span>  

<span data-ttu-id="ef2b9-195">V případě hello Chromeriver, hello uživatelské účty musí toobe vytvořené vaše [tým podpory Chromeriver](https://www.chromeriver.com/services/support).</span><span class="sxs-lookup"><span data-stu-id="ef2b9-195">In hello case of Chromeriver, hello user accounts need toobe created by your [Chromeriver support team](https://www.chromeriver.com/services/support).</span></span>

>[!NOTE]
><span data-ttu-id="ef2b9-196">Můžete použít všechny ostatní Chromeriver uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Chromeriver tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-196">You can use any other Chromeriver user account creation tools or APIs provided by Chromeriver tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ef2b9-197">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ef2b9-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ef2b9-198">V této části povolíte tak, že udělíte přístup tooChromeriver toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooChromeriver.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ef2b9-200">**tooassign Britta Simon tooChromeriver, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ef2b9-200">**tooassign Britta Simon tooChromeriver, perform hello following steps:**</span></span>

1. <span data-ttu-id="ef2b9-201">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ef2b9-203">V seznamu aplikace hello vyberte **Chromeriver**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-203">In hello applications list, select **Chromeriver**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_app.png) 

3. <span data-ttu-id="ef2b9-205">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ef2b9-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-207">Click **Add** button.</span></span> <span data-ttu-id="ef2b9-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ef2b9-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ef2b9-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ef2b9-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ef2b9-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ef2b9-213">Testing single sign-on</span></span>

<span data-ttu-id="ef2b9-214">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-214">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ef2b9-215">Když kliknete na dlaždici Chromeriver hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Chromeriver aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-215">When you click hello Chromeriver tile in hello Access Panel, you should get automatically signed-on tooyour Chromeriver application.</span></span> <span data-ttu-id="ef2b9-216">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ef2b9-216">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef2b9-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ef2b9-217">Additional resources</span></span>

* [<span data-ttu-id="ef2b9-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ef2b9-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ef2b9-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ef2b9-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_203.png

