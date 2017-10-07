---
title: 'Kurz: Azure Active Directory integrace s Brightspace podle Desire2Learn | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Brightspace podle Desire2Learn."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e2d3065b-1f6c-4c45-af78-0d5da3266999
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 99d03dc50defcb291a651a5415e30baab39e1e77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a><span data-ttu-id="3b2e8-103">Kurz: Azure Active Directory integrace s Brightspace podle Desire2Learn</span><span class="sxs-lookup"><span data-stu-id="3b2e8-103">Tutorial: Azure Active Directory integration with Brightspace by Desire2Learn</span></span>

<span data-ttu-id="3b2e8-104">V tomto kurzu zjistíte, jak toointegrate Brightspace podle Desire2Learn službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3b2e8-104">In this tutorial, you learn how toointegrate Brightspace by Desire2Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3b2e8-105">Integrace Brightspace podle Desire2Learn s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3b2e8-105">Integrating Brightspace by Desire2Learn with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3b2e8-106">Můžete řídit ve službě Azure AD, který má přístup tooBrightspace podle Desire2Learn</span><span class="sxs-lookup"><span data-stu-id="3b2e8-106">You can control in Azure AD who has access tooBrightspace by Desire2Learn</span></span>
- <span data-ttu-id="3b2e8-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBrightspace podle Desire2Learn (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b2e8-107">You can enable your users tooautomatically get signed-on tooBrightspace by Desire2Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3b2e8-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3b2e8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3b2e8-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3b2e8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b2e8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3b2e8-110">Prerequisites</span></span>

<span data-ttu-id="3b2e8-111">Integrace služby Azure AD s Brightspace podle Desire2Learn tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3b2e8-111">tooconfigure Azure AD integration with Brightspace by Desire2Learn, you need hello following items:</span></span>

- <span data-ttu-id="3b2e8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b2e8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3b2e8-113">Předplatné povolené Brightspace podle Desire2Learn jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b2e8-113">A Brightspace by Desire2Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3b2e8-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3b2e8-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3b2e8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3b2e8-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3b2e8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3b2e8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3b2e8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3b2e8-118">Scenario description</span></span>
<span data-ttu-id="3b2e8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3b2e8-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3b2e8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3b2e8-121">Přidání Brightspace podle Desire2Learn z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3b2e8-121">Adding Brightspace by Desire2Learn from hello gallery</span></span>
2. <span data-ttu-id="3b2e8-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b2e8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-brightspace-by-desire2learn-from-hello-gallery"></a><span data-ttu-id="3b2e8-123">Přidání Brightspace podle Desire2Learn z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3b2e8-123">Adding Brightspace by Desire2Learn from hello gallery</span></span>
<span data-ttu-id="3b2e8-124">tooconfigure hello integrace Brightspace podle Desire2Learn do Azure AD, je nutné tooadd Brightspace podle Desire2Learn hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-124">tooconfigure hello integration of Brightspace by Desire2Learn into Azure AD, you need tooadd Brightspace by Desire2Learn from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3b2e8-125">**tooadd Brightspace podle Desire2Learn z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3b2e8-125">**tooadd Brightspace by Desire2Learn from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b2e8-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3b2e8-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3b2e8-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3b2e8-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3b2e8-133">Hello vyhledávacího pole zadejte **Brightspace podle Desire2Learn**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-133">In hello search box, type **Brightspace by Desire2Learn**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_search.png)

5. <span data-ttu-id="3b2e8-135">Na panelu výsledků hello vyberte **Brightspace podle Desire2Learn**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-135">In hello results panel, select **Brightspace by Desire2Learn**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3b2e8-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b2e8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3b2e8-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s Brightspace podle Desire2Learn podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3b2e8-138">In this section, you configure and test Azure AD single sign-on with Brightspace by Desire2Learn based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3b2e8-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Brightspace podle Desire2Learn je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Brightspace by Desire2Learn is tooa user in Azure AD.</span></span> <span data-ttu-id="3b2e8-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Brightspace podle Desire2Learn musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-140">In other words, a link relationship between an Azure AD user and hello related user in Brightspace by Desire2Learn needs toobe established.</span></span>

<span data-ttu-id="3b2e8-141">V Brightspace podle Desire2Learn přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-141">In Brightspace by Desire2Learn, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3b2e8-142">tooconfigure a testu Azure AD jednotné přihlašování s Brightspace podle Desire2Learn, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="3b2e8-142">tooconfigure and test Azure AD single sign-on with Brightspace by Desire2Learn, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3b2e8-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3b2e8-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3b2e8-145">**[Vytváření Brightspace uživatelem test Desire2Learn](#creating-a-brightspace-by-desire2learn-test-user)**  -toohave protějšek Britta Simon v Brightspace podle Desire2Learn, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-145">**[Creating a Brightspace by Desire2Learn test user](#creating-a-brightspace-by-desire2learn-test-user)** - toohave a counterpart of Britta Simon in Brightspace by Desire2Learn that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3b2e8-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3b2e8-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3b2e8-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b2e8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3b2e8-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaše Brightspace Desire2Learn aplikací.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Brightspace by Desire2Learn application.</span></span>

<span data-ttu-id="3b2e8-150">**tooconfigure Azure AD jednotné přihlašování s Brightspace podle Desire2Learn, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3b2e8-150">**tooconfigure Azure AD single sign-on with Brightspace by Desire2Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b2e8-151">V portálu Azure, na hello hello **Brightspace podle Desire2Learn** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-151">In hello Azure portal, on hello **Brightspace by Desire2Learn** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3b2e8-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_samlbase.png)

3. <span data-ttu-id="3b2e8-155">Na hello **Brightspace Desire2Learn domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="3b2e8-155">On hello **Brightspace by Desire2Learn Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_url.png)

    <span data-ttu-id="3b2e8-157">a.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-157">a.</span></span> <span data-ttu-id="3b2e8-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="3b2e8-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.tenants.brightspace.com/samlLogin`|
    | `https://<companyname>.desire2learn.com/shibboleth-sp`|

    <span data-ttu-id="3b2e8-159">b.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-159">b.</span></span> <span data-ttu-id="3b2e8-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span><span class="sxs-lookup"><span data-stu-id="3b2e8-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3b2e8-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-161">These values are not real.</span></span> <span data-ttu-id="3b2e8-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="3b2e8-163">Obraťte se na [Brightspace tým podpory Desire2Learn](https://www.d2l.com/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-163">Contact [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/) tooget these values.</span></span>
 


4. <span data-ttu-id="3b2e8-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_certificate.png) 

5. <span data-ttu-id="3b2e8-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3b2e8-168">tooconfigure jednotného přihlašování na **Brightspace podle Desire2Learn** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[Brightspace tým podpory Desire2Learn](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="3b2e8-168">tooconfigure single sign-on on **Brightspace by Desire2Learn** side, you need toosend hello downloaded **Metadata XML** too[Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="3b2e8-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="3b2e8-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3b2e8-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3b2e8-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3b2e8-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3b2e8-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b2e8-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="3b2e8-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3b2e8-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3b2e8-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b2e8-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3b2e8-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3b2e8-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3b2e8-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3b2e8-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3b2e8-184">a.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-184">a.</span></span> <span data-ttu-id="3b2e8-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3b2e8-186">b.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-186">b.</span></span> <span data-ttu-id="3b2e8-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3b2e8-188">c.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-188">c.</span></span> <span data-ttu-id="3b2e8-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3b2e8-190">d.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-190">d.</span></span> <span data-ttu-id="3b2e8-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-191">Click **Create**.</span></span>
 
### <a name="creating-a-brightspace-by-desire2learn-test-user"></a><span data-ttu-id="3b2e8-192">Vytváření Brightspace Desire2Learn testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="3b2e8-192">Creating a Brightspace by Desire2Learn test user</span></span>

<span data-ttu-id="3b2e8-193">V pořadí tooenable Azure AD Uživatelé toolog do Brightspace podle Desire2Learn se musí být zřízená do Brightspace podle Desire2Learn.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-193">In order tooenable Azure AD users toolog into Brightspace by Desire2Learn, they must be provisioned into Brightspace by Desire2Learn.</span></span>  

<span data-ttu-id="3b2e8-194">V případě hello Brightspace podle Desire2Learn, hello uživatelské účty musí toobe vytvořené vaše [Brightspace tým podpory Desire2Learn](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="3b2e8-194">In hello case of Brightspace by Desire2Learn, hello user accounts need toobe created by your [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

>[!NOTE]
><span data-ttu-id="3b2e8-195">Můžete použít jiné Brightspace Desire2Learn nástroje pro vytvoření účtu uživatele nebo rozhraní API poskytovaných podle Brightspace Desire2Learn tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-195">You can use any other Brightspace by Desire2Learn user account creation tools or APIs provided by Brightspace by Desire2Learn tooprovision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3b2e8-196">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="3b2e8-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3b2e8-197">V této části povolíte tak, že udělíte přístup tooBrightspace podle Desire2Learn toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBrightspace by Desire2Learn.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3b2e8-199">**tooassign Britta Simon tooBrightspace podle Desire2Learn, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3b2e8-199">**tooassign Britta Simon tooBrightspace by Desire2Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b2e8-200">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3b2e8-202">V seznamu aplikace hello vyberte **Brightspace podle Desire2Learn**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-202">In hello applications list, select **Brightspace by Desire2Learn**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_app.png) 

3. <span data-ttu-id="3b2e8-204">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3b2e8-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-206">Click **Add** button.</span></span> <span data-ttu-id="3b2e8-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3b2e8-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3b2e8-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3b2e8-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3b2e8-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b2e8-212">Testing single sign-on</span></span>

<span data-ttu-id="3b2e8-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3b2e8-214">Po kliknutí na tlačítko hello Brightspace podle Desire2Learn dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Brightspace Desire2Learn aplikací.</span><span class="sxs-lookup"><span data-stu-id="3b2e8-214">When you click hello Brightspace by Desire2Learn tile in hello Access Panel, you should get automatically signed-on tooyour Brightspace by Desire2Learn application.</span></span>
<span data-ttu-id="3b2e8-215">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3b2e8-215">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b2e8-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3b2e8-216">Additional resources</span></span>

* [<span data-ttu-id="3b2e8-217">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b2e8-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3b2e8-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3b2e8-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_203.png

