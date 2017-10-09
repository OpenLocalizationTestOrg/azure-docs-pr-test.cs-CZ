---
title: 'Kurz: Azure Active Directory integrace s 15Five | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a 15Five."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2fb301c2-7d7a-4046-8ee1-7dc9e7684806
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 9e531615c16331ce000e285d13d9adce13735a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-15five"></a><span data-ttu-id="82fa4-103">Kurz: Azure Active Directory integrace s 15Five</span><span class="sxs-lookup"><span data-stu-id="82fa4-103">Tutorial: Azure Active Directory integration with 15Five</span></span>

<span data-ttu-id="82fa4-104">V tomto kurzu zjistíte, jak toointegrate 15Five s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="82fa4-104">In this tutorial, you learn how toointegrate 15Five with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="82fa4-105">Integrace 15Five s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="82fa4-105">Integrating 15Five with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="82fa4-106">Můžete řídit ve službě Azure AD, který má přístup too15Five</span><span class="sxs-lookup"><span data-stu-id="82fa4-106">You can control in Azure AD who has access too15Five</span></span>
- <span data-ttu-id="82fa4-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného too15Five (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="82fa4-107">You can enable your users tooautomatically get signed-on too15Five (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="82fa4-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="82fa4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="82fa4-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="82fa4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82fa4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="82fa4-110">Prerequisites</span></span>

<span data-ttu-id="82fa4-111">Integrace služby Azure AD s 15Five tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="82fa4-111">tooconfigure Azure AD integration with 15Five, you need hello following items:</span></span>

- <span data-ttu-id="82fa4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="82fa4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="82fa4-113">15Five jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="82fa4-113">A 15Five single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="82fa4-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="82fa4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="82fa4-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="82fa4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="82fa4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="82fa4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="82fa4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="82fa4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="82fa4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="82fa4-118">Scenario description</span></span>
<span data-ttu-id="82fa4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="82fa4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="82fa4-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="82fa4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="82fa4-121">Přidání 15Five z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="82fa4-121">Adding 15Five from hello gallery</span></span>
2. <span data-ttu-id="82fa4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="82fa4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-15five-from-hello-gallery"></a><span data-ttu-id="82fa4-123">Přidání 15Five z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="82fa4-123">Adding 15Five from hello gallery</span></span>
<span data-ttu-id="82fa4-124">tooconfigure hello integrace 15Five do Azure AD, je nutné tooadd 15Five hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="82fa4-124">tooconfigure hello integration of 15Five into Azure AD, you need tooadd 15Five from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="82fa4-125">**tooadd 15Five z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="82fa4-125">**tooadd 15Five from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="82fa4-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="82fa4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="82fa4-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="82fa4-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="82fa4-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="82fa4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="82fa4-133">Hello vyhledávacího pole zadejte **15Five**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-133">In hello search box, type **15Five**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/tutorial_15five_search.png)

5. <span data-ttu-id="82fa4-135">Na panelu výsledků hello vyberte **15Five**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="82fa4-135">In hello results panel, select **15Five**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/tutorial_15five_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="82fa4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="82fa4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="82fa4-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 15Five podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="82fa4-138">In this section, you configure and test Azure AD single sign-on with 15Five based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="82fa4-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v 15Five je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82fa4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 15Five is tooa user in Azure AD.</span></span> <span data-ttu-id="82fa4-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v 15Five musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="82fa4-140">In other words, a link relationship between an Azure AD user and hello related user in 15Five needs toobe established.</span></span>

<span data-ttu-id="82fa4-141">V 15Five, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="82fa4-141">In 15Five, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="82fa4-142">tooconfigure a testu Azure AD jednotné přihlašování s 15Five, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="82fa4-142">tooconfigure and test Azure AD single sign-on with 15Five, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="82fa4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="82fa4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="82fa4-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="82fa4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="82fa4-145">**[Vytvoření zkušebního uživatele 15Five](#creating-a-15five-test-user)**  -toohave protějšek Britta Simon v 15Five, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="82fa4-145">**[Creating a 15Five test user](#creating-a-15five-test-user)** - toohave a counterpart of Britta Simon in 15Five that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="82fa4-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="82fa4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="82fa4-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="82fa4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="82fa4-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="82fa4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="82fa4-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci 15Five.</span><span class="sxs-lookup"><span data-stu-id="82fa4-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 15Five application.</span></span>

<span data-ttu-id="82fa4-150">**tooconfigure Azure AD jednotné přihlašování s 15Five, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="82fa4-150">**tooconfigure Azure AD single sign-on with 15Five, perform hello following steps:**</span></span>

1. <span data-ttu-id="82fa4-151">V portálu Azure, na hello hello **15Five** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-151">In hello Azure portal, on hello **15Five** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="82fa4-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="82fa4-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_15five_samlbase.png)

3. <span data-ttu-id="82fa4-155">Na hello **15Five domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="82fa4-155">On hello **15Five Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_15five_url.png)

    <span data-ttu-id="82fa4-157">a.</span><span class="sxs-lookup"><span data-stu-id="82fa4-157">a.</span></span> <span data-ttu-id="82fa4-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.15five.com`</span><span class="sxs-lookup"><span data-stu-id="82fa4-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.15five.com`</span></span>

    <span data-ttu-id="82fa4-159">b.</span><span class="sxs-lookup"><span data-stu-id="82fa4-159">b.</span></span> <span data-ttu-id="82fa4-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.15five.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="82fa4-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.15five.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="82fa4-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="82fa4-161">These values are not real.</span></span> <span data-ttu-id="82fa4-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="82fa4-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="82fa4-163">Obraťte se na [tým podpory klienta 15Five](https://www.15five.com/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="82fa4-163">Contact [15Five Client support team](https://www.15five.com/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="82fa4-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="82fa4-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_15five_certificate.png) 

5. <span data-ttu-id="82fa4-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="82fa4-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="82fa4-168">tooconfigure jednotného přihlašování na **15Five** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory 15Five](https://www.15five.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="82fa4-168">tooconfigure single sign-on on **15Five** side, you need toosend hello downloaded **Metadata XML** too[15Five support team](https://www.15five.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="82fa4-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="82fa4-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="82fa4-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="82fa4-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="82fa4-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="82fa4-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="82fa4-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="82fa4-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="82fa4-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="82fa4-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="82fa4-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="82fa4-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="82fa4-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="82fa4-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="82fa4-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="82fa4-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="82fa4-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="82fa4-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="82fa4-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="82fa4-184">a.</span><span class="sxs-lookup"><span data-stu-id="82fa4-184">a.</span></span> <span data-ttu-id="82fa4-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="82fa4-186">b.</span><span class="sxs-lookup"><span data-stu-id="82fa4-186">b.</span></span> <span data-ttu-id="82fa4-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="82fa4-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="82fa4-188">c.</span><span class="sxs-lookup"><span data-stu-id="82fa4-188">c.</span></span> <span data-ttu-id="82fa4-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="82fa4-190">d.</span><span class="sxs-lookup"><span data-stu-id="82fa4-190">d.</span></span> <span data-ttu-id="82fa4-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-191">Click **Create**.</span></span>
 
### <a name="creating-a-15five-test-user"></a><span data-ttu-id="82fa4-192">Vytvoření zkušebního uživatele 15Five</span><span class="sxs-lookup"><span data-stu-id="82fa4-192">Creating a 15Five test user</span></span>

<span data-ttu-id="82fa4-193">Uživatelé toolog tooenable Azure AD v too15Five, se musí být zřízená do 15Five.</span><span class="sxs-lookup"><span data-stu-id="82fa4-193">tooenable Azure AD users toolog in too15Five, they must be provisioned into 15Five.</span></span> <span data-ttu-id="82fa4-194">Při zřizování 15Five, je ruční úlohy.</span><span class="sxs-lookup"><span data-stu-id="82fa4-194">When 15Five, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="82fa4-195">tooconfigure zřizování uživatelů, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="82fa4-195">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="82fa4-196">Přihlaste se tooyour **15Five** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="82fa4-196">Log in tooyour **15Five** company site as administrator.</span></span>

2. <span data-ttu-id="82fa4-197">Přejděte příliš**společnosti spravovat**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-197">Go too**Manage Company**.</span></span>
   
    <span data-ttu-id="82fa4-198">![Spravovat společnosti](./media/active-directory-saas-15five-tutorial/IC784675.png "spravovat společnosti")</span><span class="sxs-lookup"><span data-stu-id="82fa4-198">![Manage Company](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")</span></span>

3. <span data-ttu-id="82fa4-199">Přejděte příliš**osoby \> přidat osoby**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-199">Go too**People \> Add People**.</span></span>
   
    <span data-ttu-id="82fa4-200">![Lidé](./media/active-directory-saas-15five-tutorial/IC784676.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="82fa4-200">![People](./media/active-directory-saas-15five-tutorial/IC784676.png "People")</span></span>

4. <span data-ttu-id="82fa4-201">V části přidat nové osobě hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="82fa4-201">In hello Add New Person section, perform hello following steps:</span></span>
   
    <span data-ttu-id="82fa4-202">![Přidat nové osobě](./media/active-directory-saas-15five-tutorial/IC784677.png "přidat nové osobě")</span><span class="sxs-lookup"><span data-stu-id="82fa4-202">![Add New Person](./media/active-directory-saas-15five-tutorial/IC784677.png "Add New Person")</span></span>
   
    <span data-ttu-id="82fa4-203">a.</span><span class="sxs-lookup"><span data-stu-id="82fa4-203">a.</span></span> <span data-ttu-id="82fa4-204">Typ hello **křestní jméno**, **příjmení**, **název**, **e-mailová adresa** platný účet služby Azure Active Directory, kterou chcete tooprovision do Hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="82fa4-204">Type hello **First Name**, **Last Name**, **Title**, **Email address** of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="82fa4-205">b.</span><span class="sxs-lookup"><span data-stu-id="82fa4-205">b.</span></span> <span data-ttu-id="82fa4-206">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="82fa4-206">Click **Done**.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="82fa4-207">Hello účet Azure AD, že držitel obdrží e-mail, včetně účet odkaz tooconfirm hello dříve, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="82fa4-207">hello Azure AD account holder receives an email including a link tooconfirm hello account before it becomes active.</span></span>
   
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="82fa4-208">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="82fa4-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="82fa4-209">V této části povolíte tak, že udělíte přístup too15Five toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="82fa4-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too15Five.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="82fa4-211">**tooassign Britta Simon too15Five, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="82fa4-211">**tooassign Britta Simon too15Five, perform hello following steps:**</span></span>

1. <span data-ttu-id="82fa4-212">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="82fa4-214">V seznamu aplikace hello vyberte **15Five**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-214">In hello applications list, select **15Five**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_15five_app.png) 

3. <span data-ttu-id="82fa4-216">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="82fa4-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="82fa4-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="82fa4-218">Click **Add** button.</span></span> <span data-ttu-id="82fa4-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82fa4-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="82fa4-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="82fa4-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="82fa4-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82fa4-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="82fa4-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82fa4-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="82fa4-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="82fa4-224">Testing single sign-on</span></span>

<span data-ttu-id="82fa4-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="82fa4-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="82fa4-226">Když kliknete na dlaždici 15Five hello v hello přístupového panelu, měli byste obdržet přihlašovací stránku 15Five aplikace.</span><span class="sxs-lookup"><span data-stu-id="82fa4-226">When you click hello 15Five tile in hello Access Panel, you should get login page of 15Five application.</span></span>
<span data-ttu-id="82fa4-227">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="82fa4-227">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="82fa4-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="82fa4-228">Additional resources</span></span>

* [<span data-ttu-id="82fa4-229">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="82fa4-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="82fa4-230">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="82fa4-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-15five-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-15five-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-15five-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-15five-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-15five-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-15five-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-15five-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-15five-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-15five-tutorial/tutorial_general_203.png

