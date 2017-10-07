---
title: 'Kurz: Azure Active Directory integrace s FM:Systems | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FM:Systems."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f78c58c5-6e98-458b-8991-78624a245665
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 677ef74dac663a43835d65a4d4f4fd031a0078cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fmsystems"></a><span data-ttu-id="e99ff-103">Kurz: Azure Active Directory integrace s FM:Systems</span><span class="sxs-lookup"><span data-stu-id="e99ff-103">Tutorial: Azure Active Directory integration with FM:Systems</span></span>

<span data-ttu-id="e99ff-104">V tomto kurzu zjistíte, jak toointegrate FM:Systems s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e99ff-104">In this tutorial, you learn how toointegrate FM:Systems with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e99ff-105">Integrace FM:Systems s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e99ff-105">Integrating FM:Systems with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e99ff-106">Můžete řídit ve službě Azure AD, který má přístup tooFM:Systems</span><span class="sxs-lookup"><span data-stu-id="e99ff-106">You can control in Azure AD who has access tooFM:Systems</span></span>
- <span data-ttu-id="e99ff-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFM:Systems (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e99ff-107">You can enable your users tooautomatically get signed-on tooFM:Systems (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e99ff-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e99ff-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e99ff-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e99ff-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e99ff-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e99ff-110">Prerequisites</span></span>

<span data-ttu-id="e99ff-111">Integrace služby Azure AD s FM:Systems tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e99ff-111">tooconfigure Azure AD integration with FM:Systems, you need hello following items:</span></span>

- <span data-ttu-id="e99ff-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e99ff-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e99ff-113">FM:Systems jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e99ff-113">An FM:Systems single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e99ff-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e99ff-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e99ff-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e99ff-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e99ff-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e99ff-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e99ff-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e99ff-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e99ff-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e99ff-118">Scenario description</span></span>
<span data-ttu-id="e99ff-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e99ff-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e99ff-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e99ff-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e99ff-121">Přidání FM:Systems z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e99ff-121">Adding FM:Systems from hello gallery</span></span>
2. <span data-ttu-id="e99ff-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e99ff-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-fmsystems-from-hello-gallery"></a><span data-ttu-id="e99ff-123">Přidání FM:Systems z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e99ff-123">Adding FM:Systems from hello gallery</span></span>
<span data-ttu-id="e99ff-124">tooconfigure hello integrace FM:Systems do Azure AD, je nutné tooadd FM:Systems hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e99ff-124">tooconfigure hello integration of FM:Systems into Azure AD, you need tooadd FM:Systems from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e99ff-125">**tooadd FM:Systems z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e99ff-125">**tooadd FM:Systems from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e99ff-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e99ff-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e99ff-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e99ff-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e99ff-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e99ff-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e99ff-133">Hello vyhledávacího pole zadejte **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-133">In hello search box, type **FM:Systems**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_search.png)

5. <span data-ttu-id="e99ff-135">Na panelu výsledků hello vyberte **FM:Systems**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e99ff-135">In hello results panel, select **FM:Systems**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e99ff-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e99ff-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e99ff-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FM:Systems podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e99ff-138">In this section, you configure and test Azure AD single sign-on with FM:Systems based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e99ff-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v FM:Systems je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e99ff-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FM:Systems is tooa user in Azure AD.</span></span> <span data-ttu-id="e99ff-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v FM:Systems musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="e99ff-140">In other words, a link relationship between an Azure AD user and hello related user in FM:Systems needs toobe established.</span></span>

<span data-ttu-id="e99ff-141">V FM:Systems, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="e99ff-141">In FM:Systems, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e99ff-142">tooconfigure a testu Azure AD jednotné přihlašování s FM:Systems, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="e99ff-142">tooconfigure and test Azure AD single sign-on with FM:Systems, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e99ff-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="e99ff-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e99ff-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e99ff-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e99ff-145">**[Vytváření testovacího uživatele FM:Systems](#creating-an-fmsystems-test-user)**  -toohave protějšek Britta Simon v FM:Systems, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="e99ff-145">**[Creating an FM:Systems test user](#creating-an-fmsystems-test-user)** - toohave a counterpart of Britta Simon in FM:Systems that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e99ff-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e99ff-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e99ff-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="e99ff-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e99ff-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e99ff-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e99ff-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci FM:Systems.</span><span class="sxs-lookup"><span data-stu-id="e99ff-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your FM:Systems application.</span></span>

<span data-ttu-id="e99ff-150">**tooconfigure Azure AD jednotné přihlašování s FM:Systems, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e99ff-150">**tooconfigure Azure AD single sign-on with FM:Systems, perform hello following steps:**</span></span>

1. <span data-ttu-id="e99ff-151">V portálu Azure, na hello hello **FM:Systems** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-151">In hello Azure portal, on hello **FM:Systems** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e99ff-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e99ff-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_samlbase.png)

3. <span data-ttu-id="e99ff-155">Na hello **FM:Systems domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e99ff-155">On hello **FM:Systems Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_url.png)

    <span data-ttu-id="e99ff-157">V hello **adresa URL odpovědi** textovému poli, zadejte vaše FM:Systems **adresa URL odpovědi**, typ hello adresu URL pomocí hello následující vzoru:`https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span><span class="sxs-lookup"><span data-stu-id="e99ff-157">In hello **Reply URL** textbox, type your FM:Systems **Reply URL**, type hello URL using hello following pattern: `https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e99ff-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="e99ff-158">This value is not real.</span></span> <span data-ttu-id="e99ff-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e99ff-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="e99ff-160">Obraťte se na [tým podpory FM:Systems](https://fmsystems.com/ask-us/) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e99ff-160">Contact [FM:Systems support team](https://fmsystems.com/ask-us/) tooget this value.</span></span>
 
4. <span data-ttu-id="e99ff-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e99ff-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_certificate.png) 

5. <span data-ttu-id="e99ff-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e99ff-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e99ff-165">tooconfigure jednotného přihlašování na **FM:Systems** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory FM:Systems](https://fmsystems.com/ask-us/).</span><span class="sxs-lookup"><span data-stu-id="e99ff-165">tooconfigure single sign-on on **FM:Systems** side, you need toosend hello downloaded **Metadata XML** too[FM:Systems support team](https://fmsystems.com/ask-us/).</span></span> <span data-ttu-id="e99ff-166">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="e99ff-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span> <span data-ttu-id="e99ff-167">Zobrazí se oznámení, když bylo povoleno jednotné přihlašování pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="e99ff-167">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="e99ff-168">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="e99ff-168">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e99ff-169">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="e99ff-169">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e99ff-170">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e99ff-170">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e99ff-171">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e99ff-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="e99ff-172">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="e99ff-172">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e99ff-174">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e99ff-174">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e99ff-175">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e99ff-175">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e99ff-177">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-177">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e99ff-179">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="e99ff-179">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e99ff-181">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e99ff-181">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e99ff-183">a.</span><span class="sxs-lookup"><span data-stu-id="e99ff-183">a.</span></span> <span data-ttu-id="e99ff-184">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-184">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e99ff-185">b.</span><span class="sxs-lookup"><span data-stu-id="e99ff-185">b.</span></span> <span data-ttu-id="e99ff-186">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e99ff-186">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e99ff-187">c.</span><span class="sxs-lookup"><span data-stu-id="e99ff-187">c.</span></span> <span data-ttu-id="e99ff-188">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-188">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e99ff-189">d.</span><span class="sxs-lookup"><span data-stu-id="e99ff-189">d.</span></span> <span data-ttu-id="e99ff-190">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-190">Click **Create**.</span></span>
 
### <a name="creating-an-fmsystems-test-user"></a><span data-ttu-id="e99ff-191">Vytváření testovacího uživatele FM:Systems</span><span class="sxs-lookup"><span data-stu-id="e99ff-191">Creating an FM:Systems test user</span></span>

1. <span data-ttu-id="e99ff-192">V okně webového prohlížeče Přihlaste se jako správce k serveru vaší společnosti FM:Systems.</span><span class="sxs-lookup"><span data-stu-id="e99ff-192">In a web browser window, log into your FM:Systems company site as an administrator.</span></span>

2. <span data-ttu-id="e99ff-193">Přejděte příliš**systému správy \> spravovat zabezpečení \> uživatelé \> seznam uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-193">Go too**System Administration \> Manage Security \> Users \> User list**.</span></span>
   
    <span data-ttu-id="e99ff-194">![Správa systému](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "správu systému")</span><span class="sxs-lookup"><span data-stu-id="e99ff-194">![System Administration](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "System Administration")</span></span>

3. <span data-ttu-id="e99ff-195">Klikněte na tlačítko **vytvořit nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-195">Click **Create new user**.</span></span>
   
    <span data-ttu-id="e99ff-196">![Vytvoření nového uživatele](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "vytvořit nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="e99ff-196">![Create New User](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "Create New User")</span></span>

4. <span data-ttu-id="e99ff-197">V hello **vytvořit uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e99ff-197">In hello **Create User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="e99ff-198">![Vytvoření uživatele](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="e99ff-198">![Create User](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "Create User")</span></span>
   
    <span data-ttu-id="e99ff-199">a.</span><span class="sxs-lookup"><span data-stu-id="e99ff-199">a.</span></span> <span data-ttu-id="e99ff-200">Typ hello **uživatelské jméno**, hello **heslo**, **Potvrdit heslo**, **e-mailu** a hello **identifikační číslo zaměstnance**platný Azure souvisejícím s účtem služby Active Directory do hello chcete tooprovision textových polí.</span><span class="sxs-lookup"><span data-stu-id="e99ff-200">Type hello **UserName**, hello **Password**, **Confirm Password**, **E-mail** and hello **Employee ID** of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="e99ff-201">b.</span><span class="sxs-lookup"><span data-stu-id="e99ff-201">b.</span></span> <span data-ttu-id="e99ff-202">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-202">Click **Next**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e99ff-203">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e99ff-203">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e99ff-204">V této části povolíte tak, že udělíte přístup tooFM:Systems toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e99ff-204">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFM:Systems.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e99ff-206">**tooassign Britta Simon tooFM:Systems, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e99ff-206">**tooassign Britta Simon tooFM:Systems, perform hello following steps:**</span></span>

1. <span data-ttu-id="e99ff-207">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-207">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e99ff-209">V seznamu aplikace hello vyberte **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-209">In hello applications list, select **FM:Systems**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_app.png) 

3. <span data-ttu-id="e99ff-211">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e99ff-211">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e99ff-213">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e99ff-213">Click **Add** button.</span></span> <span data-ttu-id="e99ff-214">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e99ff-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e99ff-216">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="e99ff-216">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e99ff-217">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e99ff-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e99ff-218">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e99ff-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e99ff-219">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e99ff-219">Testing single sign-on</span></span>

<span data-ttu-id="e99ff-220">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e99ff-220">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e99ff-221">Když kliknete na dlaždici FM:Systems hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour FM:Systems aplikace.</span><span class="sxs-lookup"><span data-stu-id="e99ff-221">When you click hello FM:Systems tile in hello Access Panel, you should get automatically signed-on tooyour FM:Systems application.</span></span>
<span data-ttu-id="e99ff-222">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e99ff-222">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e99ff-223">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e99ff-223">Additional resources</span></span>

* [<span data-ttu-id="e99ff-224">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e99ff-224">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e99ff-225">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e99ff-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_203.png

