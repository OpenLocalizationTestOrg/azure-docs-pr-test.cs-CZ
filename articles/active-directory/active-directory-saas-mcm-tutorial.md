---
title: 'Kurz: Azure Active Directory integrace s MCM | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a MCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7f00799d-e3e9-4ba9-ae4a-fbca843ac5db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 6fbb3c641725bed1e7c73ee78129efb86979839e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mcm"></a><span data-ttu-id="8dc79-103">Kurz: Azure Active Directory integrace s MCM</span><span class="sxs-lookup"><span data-stu-id="8dc79-103">Tutorial: Azure Active Directory integration with MCM</span></span>

<span data-ttu-id="8dc79-104">V tomto kurzu zjistíte, jak toointegrate MCM s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8dc79-104">In this tutorial, you learn how toointegrate MCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8dc79-105">Integrace MCM s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8dc79-105">Integrating MCM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8dc79-106">Můžete řídit ve službě Azure AD, který má přístup tooMCM</span><span class="sxs-lookup"><span data-stu-id="8dc79-106">You can control in Azure AD who has access tooMCM</span></span>
- <span data-ttu-id="8dc79-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMCM (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8dc79-107">You can enable your users tooautomatically get signed-on tooMCM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8dc79-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8dc79-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8dc79-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8dc79-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8dc79-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8dc79-110">Prerequisites</span></span>

<span data-ttu-id="8dc79-111">Integrace služby Azure AD s MCM tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8dc79-111">tooconfigure Azure AD integration with MCM, you need hello following items:</span></span>

- <span data-ttu-id="8dc79-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8dc79-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8dc79-113">MCM jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8dc79-113">A MCM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8dc79-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8dc79-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8dc79-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8dc79-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8dc79-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8dc79-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8dc79-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8dc79-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8dc79-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8dc79-118">Scenario description</span></span>
<span data-ttu-id="8dc79-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8dc79-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8dc79-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8dc79-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8dc79-121">Přidání MCM z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8dc79-121">Adding MCM from hello gallery</span></span>
2. <span data-ttu-id="8dc79-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8dc79-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mcm-from-hello-gallery"></a><span data-ttu-id="8dc79-123">Přidání MCM z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8dc79-123">Adding MCM from hello gallery</span></span>
<span data-ttu-id="8dc79-124">tooconfigure hello integrace MCM do Azure AD, je nutné tooadd MCM hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8dc79-124">tooconfigure hello integration of MCM into Azure AD, you need tooadd MCM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8dc79-125">**tooadd MCM z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8dc79-125">**tooadd MCM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8dc79-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8dc79-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8dc79-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8dc79-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8dc79-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8dc79-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8dc79-133">Hello vyhledávacího pole zadejte **MCM**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-133">In hello search box, type **MCM**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_search.png)

5. <span data-ttu-id="8dc79-135">Na panelu výsledků hello vyberte **MCM**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8dc79-135">In hello results panel, select **MCM**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8dc79-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8dc79-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8dc79-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s MCM podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="8dc79-138">In this section, you configure and test Azure AD single sign-on with MCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8dc79-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v MCM je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8dc79-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in MCM is tooa user in Azure AD.</span></span> <span data-ttu-id="8dc79-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v MCM musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8dc79-140">In other words, a link relationship between an Azure AD user and hello related user in MCM needs toobe established.</span></span>

<span data-ttu-id="8dc79-141">V MCM, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="8dc79-141">In MCM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8dc79-142">tooconfigure a testu Azure AD jednotné přihlašování s MCM, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="8dc79-142">tooconfigure and test Azure AD single sign-on with MCM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8dc79-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8dc79-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8dc79-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8dc79-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8dc79-145">**[Vytváření MCM testovací uživatel](#creating-a-mcm-test-user)**  -toohave protějšek Britta Simon v MCM, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8dc79-145">**[Creating a MCM test user](#creating-a-mcm-test-user)** - toohave a counterpart of Britta Simon in MCM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8dc79-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8dc79-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8dc79-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8dc79-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8dc79-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8dc79-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8dc79-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci MCM.</span><span class="sxs-lookup"><span data-stu-id="8dc79-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your MCM application.</span></span>

<span data-ttu-id="8dc79-150">**tooconfigure Azure AD jednotné přihlašování s MCM, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8dc79-150">**tooconfigure Azure AD single sign-on with MCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="8dc79-151">V portálu Azure, na hello hello **MCM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-151">In hello Azure portal, on hello **MCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8dc79-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8dc79-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_samlbase.png)

3. <span data-ttu-id="8dc79-155">Na hello **MCM domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8dc79-155">On hello **MCM Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_url.png)

    <span data-ttu-id="8dc79-157">a.</span><span class="sxs-lookup"><span data-stu-id="8dc79-157">a.</span></span> <span data-ttu-id="8dc79-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://myaba.co.uk/client-access/<companyname>/saml.php`</span><span class="sxs-lookup"><span data-stu-id="8dc79-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://myaba.co.uk/client-access/<companyname>/saml.php`</span></span>

    <span data-ttu-id="8dc79-159">b.</span><span class="sxs-lookup"><span data-stu-id="8dc79-159">b.</span></span> <span data-ttu-id="8dc79-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://myaba.co.uk/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="8dc79-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://myaba.co.uk/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8dc79-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="8dc79-161">These values are not real.</span></span> <span data-ttu-id="8dc79-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8dc79-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8dc79-163">Obraťte se na [tým podpory MCM klienta](http://mcmtechnology.com/support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8dc79-163">Contact [MCM Client support team](http://mcmtechnology.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="8dc79-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8dc79-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_certificate.png) 

5. <span data-ttu-id="8dc79-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8dc79-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="8dc79-168">tooconfigure jednotného přihlašování na **MCM** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory MCM](http://mcmtechnology.com/support/).</span><span class="sxs-lookup"><span data-stu-id="8dc79-168">tooconfigure single sign-on on **MCM** side, you need toosend hello downloaded **Metadata XML** too[MCM support team](http://mcmtechnology.com/support/).</span></span> <span data-ttu-id="8dc79-169">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="8dc79-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="8dc79-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="8dc79-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8dc79-171">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="8dc79-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8dc79-172">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8dc79-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8dc79-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8dc79-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="8dc79-174">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="8dc79-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8dc79-176">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8dc79-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8dc79-177">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8dc79-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8dc79-179">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8dc79-181">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="8dc79-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8dc79-183">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8dc79-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8dc79-185">a.</span><span class="sxs-lookup"><span data-stu-id="8dc79-185">a.</span></span> <span data-ttu-id="8dc79-186">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8dc79-187">b.</span><span class="sxs-lookup"><span data-stu-id="8dc79-187">b.</span></span> <span data-ttu-id="8dc79-188">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8dc79-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8dc79-189">c.</span><span class="sxs-lookup"><span data-stu-id="8dc79-189">c.</span></span> <span data-ttu-id="8dc79-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8dc79-191">d.</span><span class="sxs-lookup"><span data-stu-id="8dc79-191">d.</span></span> <span data-ttu-id="8dc79-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-192">Click **Create**.</span></span>
 
### <a name="creating-a-mcm-test-user"></a><span data-ttu-id="8dc79-193">Vytvoření zkušebního uživatele MCM</span><span class="sxs-lookup"><span data-stu-id="8dc79-193">Creating a MCM test user</span></span>

<span data-ttu-id="8dc79-194">V této části vytvoříte volal Britta Simon v MCM uživatele.</span><span class="sxs-lookup"><span data-stu-id="8dc79-194">In this section, you create a user called Britta Simon in MCM.</span></span> <span data-ttu-id="8dc79-195">Práce s [tým podpory MCM](http://mcmtechnology.com/support/) tooadd hello uživatelé v platformě MCM hello.</span><span class="sxs-lookup"><span data-stu-id="8dc79-195">Work with [MCM support team](http://mcmtechnology.com/support/) tooadd hello users in hello MCM platform.</span></span>

> [!NOTE]
> <span data-ttu-id="8dc79-196">Můžete použít všechny ostatní MCM uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované MCM tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="8dc79-196">You can use any other MCM user account creation tools or APIs provided by MCM tooprovision AAD user accounts.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8dc79-197">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8dc79-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8dc79-198">V této části povolíte tak, že udělíte přístup tooMCM toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8dc79-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMCM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8dc79-200">**tooassign Britta Simon tooMCM, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8dc79-200">**tooassign Britta Simon tooMCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="8dc79-201">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8dc79-203">V seznamu aplikace hello vyberte **MCM**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-203">In hello applications list, select **MCM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_app.png) 

3. <span data-ttu-id="8dc79-205">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8dc79-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8dc79-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8dc79-207">Click **Add** button.</span></span> <span data-ttu-id="8dc79-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8dc79-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8dc79-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8dc79-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8dc79-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8dc79-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8dc79-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8dc79-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8dc79-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8dc79-213">Testing single sign-on</span></span>

<span data-ttu-id="8dc79-214">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8dc79-214">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8dc79-215">Po kliknutí na tlačítko hello MCM dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour MCM aplikace.</span><span class="sxs-lookup"><span data-stu-id="8dc79-215">When you click hello MCM tile in hello Access Panel, you should get automatically signed-on tooyour MCM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8dc79-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8dc79-216">Additional resources</span></span>

* [<span data-ttu-id="8dc79-217">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8dc79-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8dc79-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8dc79-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_203.png

