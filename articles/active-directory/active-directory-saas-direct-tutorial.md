---
title: "Kurz: Azure Active Directory integrace s přímým | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Direct."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c2cd1f0-d14c-42f0-94a8-9b800008b285
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ac663070b39e55eade2c43814b63a9d0374c7316
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-direct"></a><span data-ttu-id="89ee7-103">Kurz: Azure Active Directory integrace s protokolem Direct</span><span class="sxs-lookup"><span data-stu-id="89ee7-103">Tutorial: Azure Active Directory integration with Direct</span></span>

<span data-ttu-id="89ee7-104">V tomto kurzu zjistíte, jak toointegrate přímo v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="89ee7-104">In this tutorial, you learn how toointegrate Direct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="89ee7-105">Integrace přímo s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="89ee7-105">Integrating Direct with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="89ee7-106">Můžete řídit ve službě Azure AD, který má přístup tooDirect</span><span class="sxs-lookup"><span data-stu-id="89ee7-106">You can control in Azure AD who has access tooDirect</span></span>
- <span data-ttu-id="89ee7-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDirect (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="89ee7-107">You can enable your users tooautomatically get signed-on tooDirect (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="89ee7-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="89ee7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="89ee7-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="89ee7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89ee7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="89ee7-110">Prerequisites</span></span>

<span data-ttu-id="89ee7-111">tooconfigure integrace Azure AD s přímým, je nutné hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="89ee7-111">tooconfigure Azure AD integration with Direct, you need hello following items:</span></span>

- <span data-ttu-id="89ee7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="89ee7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="89ee7-113">Přímé jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="89ee7-113">A Direct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="89ee7-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="89ee7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="89ee7-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="89ee7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="89ee7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="89ee7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="89ee7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="89ee7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="89ee7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="89ee7-118">Scenario description</span></span>
<span data-ttu-id="89ee7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="89ee7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="89ee7-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="89ee7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="89ee7-121">Přidání přímo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="89ee7-121">Adding Direct from hello gallery</span></span>
2. <span data-ttu-id="89ee7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="89ee7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-direct-from-hello-gallery"></a><span data-ttu-id="89ee7-123">Přidání přímo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="89ee7-123">Adding Direct from hello gallery</span></span>
<span data-ttu-id="89ee7-124">tooconfigure hello integrace Direct do Azure AD, je nutné tooadd přímo z hello Galerie tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="89ee7-124">tooconfigure hello integration of Direct into Azure AD, you need tooadd Direct from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="89ee7-125">**tooadd přímo z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="89ee7-125">**tooadd Direct from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="89ee7-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="89ee7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="89ee7-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="89ee7-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="89ee7-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="89ee7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="89ee7-133">Hello vyhledávacího pole zadejte **přímé**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-133">In hello search box, type **Direct**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_search.png)

5. <span data-ttu-id="89ee7-135">Na panelu výsledků hello vyberte **přímé**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="89ee7-135">In hello results panel, select **Direct**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="89ee7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="89ee7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="89ee7-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Direct podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="89ee7-138">In this section, you configure and test Azure AD single sign-on with Direct based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="89ee7-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v přímém je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="89ee7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Direct is tooa user in Azure AD.</span></span> <span data-ttu-id="89ee7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v toobe přímé musí navázat.</span><span class="sxs-lookup"><span data-stu-id="89ee7-140">In other words, a link relationship between an Azure AD user and hello related user in Direct needs toobe established.</span></span>

<span data-ttu-id="89ee7-141">V přímo, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="89ee7-141">In Direct, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="89ee7-142">tooconfigure a testování Azure AD jednotné přihlašování s přímým, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="89ee7-142">tooconfigure and test Azure AD single sign-on with Direct, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="89ee7-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="89ee7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="89ee7-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="89ee7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="89ee7-145">**[Vytváření uživatele – přímý test](#creating-a-direct-test-user)**  -toohave protějšek Britta Simon v Direct, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="89ee7-145">**[Creating a Direct test user](#creating-a-direct-test-user)** - toohave a counterpart of Britta Simon in Direct that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="89ee7-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="89ee7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="89ee7-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="89ee7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="89ee7-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="89ee7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="89ee7-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Direct.</span><span class="sxs-lookup"><span data-stu-id="89ee7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Direct application.</span></span>

<span data-ttu-id="89ee7-150">**tooconfigure Azure AD jednotné přihlašování s přímým, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="89ee7-150">**tooconfigure Azure AD single sign-on with Direct, perform hello following steps:**</span></span>

1. <span data-ttu-id="89ee7-151">V portálu Azure, na hello hello **přímé** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-151">In hello Azure portal, on hello **Direct** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="89ee7-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="89ee7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_samlbase.png)

3. <span data-ttu-id="89ee7-155">Na hello **přímé domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="89ee7-155">On hello **Direct Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_url.png)

    <span data-ttu-id="89ee7-157">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://direct4b.com/`</span><span class="sxs-lookup"><span data-stu-id="89ee7-157">In hello **Identifier** textbox, type hello URL: `https://direct4b.com/`</span></span>

4. <span data-ttu-id="89ee7-158">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="89ee7-158">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_url1.png)

     <span data-ttu-id="89ee7-160">V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://direct4b.com/sso`</span><span class="sxs-lookup"><span data-stu-id="89ee7-160">In hello **Sign-on URL** textbox, type hello URL: `https://direct4b.com/sso`</span></span> 
    
5. <span data-ttu-id="89ee7-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="89ee7-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_certificate.png) 

6. <span data-ttu-id="89ee7-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="89ee7-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="89ee7-165">tooconfigure jednotného přihlašování na **přímé** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým přímé podpory](https://direct4b.com/ja/support.html#inquiry).</span><span class="sxs-lookup"><span data-stu-id="89ee7-165">tooconfigure single sign-on on **Direct** side, you need toosend hello downloaded **Metadata XML** too[Direct support team](https://direct4b.com/ja/support.html#inquiry).</span></span> 

> [!TIP]
> <span data-ttu-id="89ee7-166">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="89ee7-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="89ee7-167">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="89ee7-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="89ee7-168">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="89ee7-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="89ee7-169">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="89ee7-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="89ee7-170">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="89ee7-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="89ee7-172">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="89ee7-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="89ee7-173">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="89ee7-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="89ee7-175">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-175">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="89ee7-177">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="89ee7-177">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="89ee7-179">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="89ee7-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="89ee7-181">a.</span><span class="sxs-lookup"><span data-stu-id="89ee7-181">a.</span></span> <span data-ttu-id="89ee7-182">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-182">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="89ee7-183">b.</span><span class="sxs-lookup"><span data-stu-id="89ee7-183">b.</span></span> <span data-ttu-id="89ee7-184">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="89ee7-184">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="89ee7-185">c.</span><span class="sxs-lookup"><span data-stu-id="89ee7-185">c.</span></span> <span data-ttu-id="89ee7-186">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="89ee7-187">d.</span><span class="sxs-lookup"><span data-stu-id="89ee7-187">d.</span></span> <span data-ttu-id="89ee7-188">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-188">Click **Create**.</span></span>
 
### <a name="creating-a-direct-test-user"></a><span data-ttu-id="89ee7-189">Vytváření uživatele – přímý test</span><span class="sxs-lookup"><span data-stu-id="89ee7-189">Creating a Direct test user</span></span>

<span data-ttu-id="89ee7-190">V této části vytvoříte uživatele volal Britta Simon v přímo.</span><span class="sxs-lookup"><span data-stu-id="89ee7-190">In this section, you create a user called Britta Simon in Direct.</span></span> <span data-ttu-id="89ee7-191">Práce s [tým přímé podpory](https://direct4b.com/ja/support.html#inquiry) pro přidání uživatelů hello hello přímou platformu.</span><span class="sxs-lookup"><span data-stu-id="89ee7-191">Work with [Direct support team](https://direct4b.com/ja/support.html#inquiry) to add hello users in hello Direct platform.</span></span> <span data-ttu-id="89ee7-192">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="89ee7-192">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="89ee7-193">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="89ee7-193">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="89ee7-194">V této části povolíte tak, že udělíte přístup tooDirect toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="89ee7-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDirect.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="89ee7-196">**tooassign Britta Simon tooDirect, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="89ee7-196">**tooassign Britta Simon tooDirect, perform hello following steps:**</span></span>

1. <span data-ttu-id="89ee7-197">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="89ee7-199">V seznamu aplikace hello vyberte **přímé**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-199">In hello applications list, select **Direct**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_app.png) 

3. <span data-ttu-id="89ee7-201">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="89ee7-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="89ee7-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="89ee7-203">Click **Add** button.</span></span> <span data-ttu-id="89ee7-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="89ee7-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="89ee7-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="89ee7-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="89ee7-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="89ee7-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="89ee7-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="89ee7-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="89ee7-209">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="89ee7-209">Testing single sign-on</span></span>

<span data-ttu-id="89ee7-210">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="89ee7-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

1. <span data-ttu-id="89ee7-211">Pokud chcete tootest v **IDP iniciované režimu**:</span><span class="sxs-lookup"><span data-stu-id="89ee7-211">If you wish tootest in **IDP Initiated Mode**:</span></span>

    <span data-ttu-id="89ee7-212">Když kliknete na tlačítko hello **přímé** dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour **přímé** aplikace.</span><span class="sxs-lookup"><span data-stu-id="89ee7-212">When you click hello **Direct** tile in hello Access Panel, you should get automatically signed-on tooyour **Direct** application.</span></span>

2. <span data-ttu-id="89ee7-213">Pokud chcete tootest v **SP iniciované režimu**:</span><span class="sxs-lookup"><span data-stu-id="89ee7-213">If you wish tootest in **SP Initiated Mode**:</span></span>
    
    <span data-ttu-id="89ee7-214">a.</span><span class="sxs-lookup"><span data-stu-id="89ee7-214">a.</span></span> <span data-ttu-id="89ee7-215">Klikněte na hello **přímé** dlaždici v hello Panel přístupu a bude přesměrované toohello aplikace stránku přihlášení.</span><span class="sxs-lookup"><span data-stu-id="89ee7-215">Click on hello **Direct** tile in hello Access Panel and you will be redirected toohello application sign-on page.</span></span>

    <span data-ttu-id="89ee7-216">b.</span><span class="sxs-lookup"><span data-stu-id="89ee7-216">b.</span></span> <span data-ttu-id="89ee7-217">Zadejte vaše `subdomain` hello textbox zobrazí a stiskněte '次へ (Další) a jste měli získat tooyour automaticky přihlášeného **přímé** aplikace.</span><span class="sxs-lookup"><span data-stu-id="89ee7-217">Input your `subdomain` in hello textbox displayed and press '次へ (Next)' and you should get automatically signed-on tooyour **Direct** application .</span></span>
    
<span data-ttu-id="89ee7-218">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="89ee7-218">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="89ee7-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="89ee7-219">Additional resources</span></span>

* [<span data-ttu-id="89ee7-220">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="89ee7-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="89ee7-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="89ee7-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-direct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-direct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-direct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-direct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-direct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-direct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-direct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-direct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-direct-tutorial/tutorial_general_203.png

