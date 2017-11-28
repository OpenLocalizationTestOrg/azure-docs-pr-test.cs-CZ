---
title: 'Kurz: Azure Active Directory integrace s Showpad | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Showpad."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 48b6bee0-dbc5-4863-964d-75b25e517741
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2c8c306b4b94c368a93f92123d3abe9fe35167db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-showpad"></a><span data-ttu-id="12640-103">Kurz: Azure Active Directory integrace s Showpad</span><span class="sxs-lookup"><span data-stu-id="12640-103">Tutorial: Azure Active Directory integration with Showpad</span></span>

<span data-ttu-id="12640-104">V tomto kurzu zjistíte, jak toointegrate Showpad s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="12640-104">In this tutorial, you learn how toointegrate Showpad with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="12640-105">Integrace Showpad s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="12640-105">Integrating Showpad with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="12640-106">Můžete řídit ve službě Azure AD, který má přístup tooShowpad</span><span class="sxs-lookup"><span data-stu-id="12640-106">You can control in Azure AD who has access tooShowpad</span></span>
- <span data-ttu-id="12640-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooShowpad (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="12640-107">You can enable your users tooautomatically get signed-on tooShowpad (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="12640-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="12640-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="12640-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="12640-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12640-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="12640-110">Prerequisites</span></span>

<span data-ttu-id="12640-111">Integrace služby Azure AD s Showpad tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="12640-111">tooconfigure Azure AD integration with Showpad, you need hello following items:</span></span>

- <span data-ttu-id="12640-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="12640-112">An Azure AD subscription</span></span>
- <span data-ttu-id="12640-113">Showpad jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="12640-113">A Showpad single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="12640-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="12640-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="12640-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="12640-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="12640-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="12640-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="12640-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="12640-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="12640-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="12640-118">Scenario description</span></span>
<span data-ttu-id="12640-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="12640-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="12640-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="12640-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="12640-121">Přidání Showpad z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="12640-121">Adding Showpad from hello gallery</span></span>
2. <span data-ttu-id="12640-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="12640-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-showpad-from-hello-gallery"></a><span data-ttu-id="12640-123">Přidání Showpad z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="12640-123">Adding Showpad from hello gallery</span></span>

<span data-ttu-id="12640-124">tooconfigure hello integrace Showpad do Azure AD, je nutné tooadd Showpad hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="12640-124">tooconfigure hello integration of Showpad into Azure AD, you need tooadd Showpad from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="12640-125">**tooadd Showpad z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="12640-125">**tooadd Showpad from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="12640-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="12640-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="12640-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="12640-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="12640-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="12640-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="12640-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="12640-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="12640-133">Hello vyhledávacího pole zadejte **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="12640-133">In hello search box, type **Showpad**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_search.png)

5. <span data-ttu-id="12640-135">Na panelu výsledků hello vyberte **Showpad**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="12640-135">In hello results panel, select **Showpad**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="12640-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="12640-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="12640-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Showpad podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="12640-138">In this section, you configure and test Azure AD single sign-on with Showpad based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="12640-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Showpad je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="12640-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Showpad is tooa user in Azure AD.</span></span> <span data-ttu-id="12640-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Showpad musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="12640-140">In other words, a link relationship between an Azure AD user and hello related user in Showpad needs toobe established.</span></span>

<span data-ttu-id="12640-141">V Showpad, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="12640-141">In Showpad, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="12640-142">tooconfigure a testu Azure AD jednotné přihlašování s Showpad, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="12640-142">tooconfigure and test Azure AD single sign-on with Showpad, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="12640-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="12640-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="12640-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="12640-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="12640-145">**[Vytvoření zkušebního uživatele Showpad](#creating-a-showpad-test-user)**  -toohave protějšek Britta Simon v Showpad, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="12640-145">**[Creating a Showpad test user](#creating-a-showpad-test-user)** - toohave a counterpart of Britta Simon in Showpad that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="12640-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="12640-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="12640-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="12640-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="12640-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="12640-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="12640-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Showpad.</span><span class="sxs-lookup"><span data-stu-id="12640-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Showpad application.</span></span>

<span data-ttu-id="12640-150">**tooconfigure Azure AD jednotné přihlašování s Showpad, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="12640-150">**tooconfigure Azure AD single sign-on with Showpad, perform hello following steps:**</span></span>

1. <span data-ttu-id="12640-151">V portálu Azure, na hello hello **Showpad** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="12640-151">In hello Azure portal, on hello **Showpad** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="12640-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="12640-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_samlbase.png)

3. <span data-ttu-id="12640-155">Na hello **Showpad domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="12640-155">On hello **Showpad Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_url.png)

    <span data-ttu-id="12640-157">a.</span><span class="sxs-lookup"><span data-stu-id="12640-157">a.</span></span> <span data-ttu-id="12640-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<comapany-name>.showpad.biz/login`</span><span class="sxs-lookup"><span data-stu-id="12640-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<comapany-name>.showpad.biz/login`</span></span>

    <span data-ttu-id="12640-159">b.</span><span class="sxs-lookup"><span data-stu-id="12640-159">b.</span></span> <span data-ttu-id="12640-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company-name>.showpad.biz`</span><span class="sxs-lookup"><span data-stu-id="12640-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company-name>.showpad.biz`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="12640-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="12640-161">These values are not real.</span></span> <span data-ttu-id="12640-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="12640-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="12640-163">Obraťte se na [tým podpory Showpad](https://help.showpad.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="12640-163">Contact [Showpad support team](https://help.showpad.com) tooget these values.</span></span> 
 


4. <span data-ttu-id="12640-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="12640-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_certificate.png) 

5. <span data-ttu-id="12640-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="12640-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="12640-168">Klient Showpad tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="12640-168">Sign-on tooyour Showpad tenant as an administrator.</span></span>

7. <span data-ttu-id="12640-169">V nabídce hello hello nahoře, klikněte na tlačítko hello **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="12640-169">In hello menu on hello top, click hello **Settings**.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

8. <span data-ttu-id="12640-171">Přejděte příliš"**jednotné přihlašování**"a klikněte na tlačítko"**povolit**."</span><span class="sxs-lookup"><span data-stu-id="12640-171">Navigate too"**Single Sign-On**" and click "**Enable**."</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

9. <span data-ttu-id="12640-173">Na hello **přidat službu SAML 2.0** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="12640-173">On hello **Add a SAML 2.0 Service** dialog, perform hello following steps:</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 
   
    <span data-ttu-id="12640-175">a.</span><span class="sxs-lookup"><span data-stu-id="12640-175">a.</span></span> <span data-ttu-id="12640-176">V hello **název** textovému poli, název typu hello identifikátor zprostředkovatele (například: název vaší společnosti).</span><span class="sxs-lookup"><span data-stu-id="12640-176">In hello **Name** textbox, type hello name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="12640-177">b.</span><span class="sxs-lookup"><span data-stu-id="12640-177">b.</span></span> <span data-ttu-id="12640-178">Jako **Metadata zdroje**, vyberte **XML**.</span><span class="sxs-lookup"><span data-stu-id="12640-178">As **Metadata Source**, select **XML**.</span></span>
   
    <span data-ttu-id="12640-179">c.</span><span class="sxs-lookup"><span data-stu-id="12640-179">c.</span></span> <span data-ttu-id="12640-180">Hello obsah souboru XML metadata, která jste si stáhli z portálu Azure hello, zkopírujte a vložte jej do hello **soubor XML s metadaty** textové pole.</span><span class="sxs-lookup"><span data-stu-id="12640-180">Copy hello content of metadata XML file, which you have downloaded from hello Azure portal, and then paste it into hello **Metadata XML** textbox.</span></span>
   
    <span data-ttu-id="12640-181">d.</span><span class="sxs-lookup"><span data-stu-id="12640-181">d.</span></span> <span data-ttu-id="12640-182">Vyberte **automatického zřizování účtů pro nové uživatele při přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="12640-182">Select **Auto-provision accounts for new users when they log in**.</span></span>
   
    <span data-ttu-id="12640-183">e.</span><span class="sxs-lookup"><span data-stu-id="12640-183">e.</span></span> <span data-ttu-id="12640-184">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="12640-184">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="12640-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="12640-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="12640-186">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="12640-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="12640-187">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="12640-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="12640-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="12640-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="12640-189">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="12640-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="12640-191">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="12640-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="12640-192">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="12640-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="12640-194">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="12640-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="12640-196">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="12640-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="12640-198">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="12640-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="12640-200">a.</span><span class="sxs-lookup"><span data-stu-id="12640-200">a.</span></span> <span data-ttu-id="12640-201">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="12640-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="12640-202">b.</span><span class="sxs-lookup"><span data-stu-id="12640-202">b.</span></span> <span data-ttu-id="12640-203">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="12640-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="12640-204">c.</span><span class="sxs-lookup"><span data-stu-id="12640-204">c.</span></span> <span data-ttu-id="12640-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="12640-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="12640-206">d.</span><span class="sxs-lookup"><span data-stu-id="12640-206">d.</span></span> <span data-ttu-id="12640-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="12640-207">Click **Create**.</span></span>
 
### <a name="creating-a-showpad-test-user"></a><span data-ttu-id="12640-208">Vytvoření zkušebního uživatele Showpad</span><span class="sxs-lookup"><span data-stu-id="12640-208">Creating a Showpad test user</span></span>

<span data-ttu-id="12640-209">Hello cílem této části je toocreate volal Britta Simon v Showpad uživatele.</span><span class="sxs-lookup"><span data-stu-id="12640-209">hello objective of this section is toocreate a user called Britta Simon in Showpad.</span></span> 

<span data-ttu-id="12640-210">Showpad podporuje zřizování za běhu.</span><span class="sxs-lookup"><span data-stu-id="12640-210">Showpad supports just-in-time provisioning.</span></span> <span data-ttu-id="12640-211">Povolení zřizování v  **[konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**.</span><span class="sxs-lookup"><span data-stu-id="12640-211">You have enabled provisioning in **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**.</span></span> 

<span data-ttu-id="12640-212">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="12640-212">There is no action item for you in this section.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="12640-213">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="12640-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="12640-214">V této části povolíte tak, že udělíte přístup tooShowpad toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="12640-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooShowpad.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="12640-216">**tooassign Britta Simon tooShowpad, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="12640-216">**tooassign Britta Simon tooShowpad, perform hello following steps:**</span></span>

1. <span data-ttu-id="12640-217">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="12640-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="12640-219">V seznamu aplikace hello vyberte **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="12640-219">In hello applications list, select **Showpad**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_app.png) 

3. <span data-ttu-id="12640-221">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="12640-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="12640-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="12640-223">Click **Add** button.</span></span> <span data-ttu-id="12640-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="12640-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="12640-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="12640-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="12640-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="12640-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="12640-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="12640-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="12640-229">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="12640-229">Testing single sign-on</span></span>

<span data-ttu-id="12640-230">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="12640-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="12640-231">Když kliknete na dlaždici Showpad hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooShowpad aplikace.</span><span class="sxs-lookup"><span data-stu-id="12640-231">When you click hello Showpad tile in hello Access Panel, you should get automatically signed-on tooShowpad application.</span></span>
<span data-ttu-id="12640-232">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="12640-232">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12640-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="12640-233">Additional resources</span></span>

* [<span data-ttu-id="12640-234">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="12640-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="12640-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="12640-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png

