---
title: 'Kurz: Azure Active Directory integrace s Skilljar | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Skilljar."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c572f556-98a3-48e6-8e4c-e634b7a2ba70
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: f0f433d82a0b5510ec568ab610863bcade047697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skilljar"></a><span data-ttu-id="e5973-103">Kurz: Azure Active Directory integrace s Skilljar</span><span class="sxs-lookup"><span data-stu-id="e5973-103">Tutorial: Azure Active Directory integration with Skilljar</span></span>

<span data-ttu-id="e5973-104">V tomto kurzu zjistíte, jak toointegrate Skilljar s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e5973-104">In this tutorial, you learn how toointegrate Skilljar with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e5973-105">Integrace Skilljar s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e5973-105">Integrating Skilljar with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e5973-106">Můžete řídit ve službě Azure AD, který má přístup tooSkilljar</span><span class="sxs-lookup"><span data-stu-id="e5973-106">You can control in Azure AD who has access tooSkilljar</span></span>
- <span data-ttu-id="e5973-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSkilljar (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5973-107">You can enable your users tooautomatically get signed-on tooSkilljar (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e5973-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e5973-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e5973-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e5973-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5973-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e5973-110">Prerequisites</span></span>

<span data-ttu-id="e5973-111">Integrace služby Azure AD s Skilljar tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e5973-111">tooconfigure Azure AD integration with Skilljar, you need hello following items:</span></span>

- <span data-ttu-id="e5973-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5973-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e5973-113">Skilljar jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e5973-113">A Skilljar single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e5973-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e5973-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e5973-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e5973-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e5973-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e5973-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e5973-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e5973-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e5973-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e5973-118">Scenario description</span></span>
<span data-ttu-id="e5973-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e5973-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e5973-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e5973-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e5973-121">Přidání Skilljar z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e5973-121">Adding Skilljar from hello gallery</span></span>
2. <span data-ttu-id="e5973-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e5973-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skilljar-from-hello-gallery"></a><span data-ttu-id="e5973-123">Přidání Skilljar z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e5973-123">Adding Skilljar from hello gallery</span></span>
<span data-ttu-id="e5973-124">tooconfigure hello integrace Skilljar do Azure AD, je nutné tooadd Skilljar hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e5973-124">tooconfigure hello integration of Skilljar into Azure AD, you need tooadd Skilljar from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e5973-125">**tooadd Skilljar z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e5973-125">**tooadd Skilljar from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5973-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e5973-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e5973-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e5973-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e5973-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e5973-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e5973-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e5973-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e5973-133">Hello vyhledávacího pole zadejte **Skilljar**.</span><span class="sxs-lookup"><span data-stu-id="e5973-133">In hello search box, type **Skilljar**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_search.png)

5. <span data-ttu-id="e5973-135">Na panelu výsledků hello vyberte **Skilljar**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5973-135">In hello results panel, select **Skilljar**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e5973-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e5973-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e5973-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Skilljar podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e5973-138">In this section, you configure and test Azure AD single sign-on with Skilljar based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e5973-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Skilljar je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e5973-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Skilljar is tooa user in Azure AD.</span></span> <span data-ttu-id="e5973-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Skilljar musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="e5973-140">In other words, a link relationship between an Azure AD user and hello related user in Skilljar needs toobe established.</span></span>

<span data-ttu-id="e5973-141">V Skilljar, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="e5973-141">In Skilljar, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e5973-142">tooconfigure a testu Azure AD jednotné přihlašování s Skilljar, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="e5973-142">tooconfigure and test Azure AD single sign-on with Skilljar, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e5973-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="e5973-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e5973-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e5973-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e5973-145">**[Vytvoření zkušebního uživatele Skilljar](#creating-a-skilljar-test-user)**  -toohave protějšek Britta Simon v Skilljar, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="e5973-145">**[Creating a Skilljar test user](#creating-a-skilljar-test-user)** - toohave a counterpart of Britta Simon in Skilljar that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e5973-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e5973-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e5973-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="e5973-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e5973-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e5973-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e5973-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Skilljar.</span><span class="sxs-lookup"><span data-stu-id="e5973-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Skilljar application.</span></span>

<span data-ttu-id="e5973-150">**tooconfigure Azure AD jednotné přihlašování s Skilljar, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e5973-150">**tooconfigure Azure AD single sign-on with Skilljar, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5973-151">V portálu Azure, na hello hello **Skilljar** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e5973-151">In hello Azure portal, on hello **Skilljar** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e5973-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e5973-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_samlbase.png)

3. <span data-ttu-id="e5973-155">Na hello **Skilljar domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e5973-155">On hello **Skilljar Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_url.png)

    <span data-ttu-id="e5973-157">a.</span><span class="sxs-lookup"><span data-stu-id="e5973-157">a.</span></span> <span data-ttu-id="e5973-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.skilljar.com/`</span><span class="sxs-lookup"><span data-stu-id="e5973-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.skilljar.com/`</span></span>

    <span data-ttu-id="e5973-159">b.</span><span class="sxs-lookup"><span data-stu-id="e5973-159">b.</span></span> <span data-ttu-id="e5973-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.skilljar.com/`</span><span class="sxs-lookup"><span data-stu-id="e5973-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.skilljar.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e5973-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e5973-161">These values are not real.</span></span> <span data-ttu-id="e5973-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e5973-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e5973-163">Obraťte se na [tým podpory Skilljar klienta](http://support.skilljar.com/hc/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e5973-163">Contact [Skilljar Client support team](http://support.skilljar.com/hc/) tooget these values.</span></span> 
 
4. <span data-ttu-id="e5973-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e5973-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_certificate.png) 

5. <span data-ttu-id="e5973-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e5973-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skilljar-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e5973-168">tooconfigure jednotného přihlašování na **Skilljar** straně, je nutné stáhnout hello toosend **soubor XML s metadaty**, a **název identifikátor formátu hodnota - urn: oasis: názvy: tc: SAML:1.1:nameid-formát: emailAddress** příliš[tým podpory Skilljar](http://support.skilljar.com/hc/).</span><span class="sxs-lookup"><span data-stu-id="e5973-168">tooconfigure single sign-on on **Skilljar** side, you need toosend hello downloaded **Metadata XML**, and **Name Identifier Format Value - urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress** too[Skilljar support team](http://support.skilljar.com/hc/).</span></span> <span data-ttu-id="e5973-169">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="e5973-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e5973-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="e5973-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e5973-171">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="e5973-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e5973-172">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e5973-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e5973-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5973-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="e5973-174">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="e5973-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e5973-176">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e5973-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5973-177">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e5973-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e5973-179">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e5973-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e5973-181">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="e5973-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e5973-183">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5973-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e5973-185">a.</span><span class="sxs-lookup"><span data-stu-id="e5973-185">a.</span></span> <span data-ttu-id="e5973-186">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e5973-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e5973-187">b.</span><span class="sxs-lookup"><span data-stu-id="e5973-187">b.</span></span> <span data-ttu-id="e5973-188">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e5973-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e5973-189">c.</span><span class="sxs-lookup"><span data-stu-id="e5973-189">c.</span></span> <span data-ttu-id="e5973-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e5973-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e5973-191">d.</span><span class="sxs-lookup"><span data-stu-id="e5973-191">d.</span></span> <span data-ttu-id="e5973-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e5973-192">Click **Create**.</span></span>
 
### <a name="creating-a-skilljar-test-user"></a><span data-ttu-id="e5973-193">Vytvoření zkušebního uživatele Skilljar</span><span class="sxs-lookup"><span data-stu-id="e5973-193">Creating a Skilljar test user</span></span>

<span data-ttu-id="e5973-194">Hello cílem této části je toocreate volal Britta Simon v Skilljar uživatele.</span><span class="sxs-lookup"><span data-stu-id="e5973-194">hello objective of this section is toocreate a user called Britta Simon in Skilljar.</span></span> <span data-ttu-id="e5973-195">Skilljar podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="e5973-195">Skilljar supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="e5973-196">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="e5973-196">There is no action item for you in this section.</span></span> <span data-ttu-id="e5973-197">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess Skilljar.</span><span class="sxs-lookup"><span data-stu-id="e5973-197">A new user is created during an attempt tooaccess Skilljar if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="e5973-198">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory Skilljar](http://support.skilljar.com/hc/).</span><span class="sxs-lookup"><span data-stu-id="e5973-198">If you need toocreate a user manually, you need toocontact hello [Skilljar support team](http://support.skilljar.com/hc/).</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e5973-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e5973-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e5973-200">V této části povolíte tak, že udělíte přístup tooSkilljar toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e5973-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSkilljar.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e5973-202">**tooassign Britta Simon tooSkilljar, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e5973-202">**tooassign Britta Simon tooSkilljar, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5973-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e5973-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e5973-205">V seznamu aplikace hello vyberte **Skilljar**.</span><span class="sxs-lookup"><span data-stu-id="e5973-205">In hello applications list, select **Skilljar**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_app.png) 

3. <span data-ttu-id="e5973-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e5973-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e5973-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e5973-209">Click **Add** button.</span></span> <span data-ttu-id="e5973-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e5973-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e5973-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="e5973-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e5973-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e5973-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e5973-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e5973-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e5973-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e5973-215">Testing single sign-on</span></span>

<span data-ttu-id="e5973-216">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="e5973-216">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="e5973-217">Když kliknete na dlaždici Skilljar hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Skilljar aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5973-217">When you click hello Skilljar tile in hello Access Panel, you should get automatically signed-on tooyour Skilljar application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5973-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e5973-218">Additional resources</span></span>

* [<span data-ttu-id="e5973-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e5973-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e5973-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e5973-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_203.png

