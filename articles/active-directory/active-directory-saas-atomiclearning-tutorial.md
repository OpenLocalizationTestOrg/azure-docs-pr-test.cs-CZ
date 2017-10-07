---
title: 'Kurz: Azure Active Directory integrace s Atomic Learning | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Atomic učení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 495f54a6-e6c4-41b0-aafa-a6283d33efc8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 12d7c380dbe47899eb35486c5e2a6936e8fb8b3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atomic-learning"></a><span data-ttu-id="c5543-103">Kurz: Azure Active Directory integrace s Atomic učení</span><span class="sxs-lookup"><span data-stu-id="c5543-103">Tutorial: Azure Active Directory integration with Atomic Learning</span></span>

<span data-ttu-id="c5543-104">V tomto kurzu zjistíte, jak toointegrate Atomic Learning s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c5543-104">In this tutorial, you learn how toointegrate Atomic Learning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c5543-105">Integrace Atomic Learning s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c5543-105">Integrating Atomic Learning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c5543-106">Můžete řídit ve službě Azure AD, který má přístup tooAtomic učení</span><span class="sxs-lookup"><span data-stu-id="c5543-106">You can control in Azure AD who has access tooAtomic Learning</span></span>
- <span data-ttu-id="c5543-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAtomic Learning (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c5543-107">You can enable your users tooautomatically get signed-on tooAtomic Learning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c5543-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c5543-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c5543-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c5543-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5543-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c5543-110">Prerequisites</span></span>

<span data-ttu-id="c5543-111">Integrace služby Azure AD s Atomic Learning tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c5543-111">tooconfigure Azure AD integration with Atomic Learning, you need hello following items:</span></span>

- <span data-ttu-id="c5543-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c5543-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c5543-113">Atomic Learning jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c5543-113">An Atomic Learning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c5543-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c5543-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c5543-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c5543-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c5543-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c5543-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c5543-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c5543-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c5543-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c5543-118">Scenario description</span></span>
<span data-ttu-id="c5543-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c5543-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c5543-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c5543-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c5543-121">Přidání Atomic Learning z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c5543-121">Adding Atomic Learning from hello gallery</span></span>
2. <span data-ttu-id="c5543-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c5543-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-atomic-learning-from-hello-gallery"></a><span data-ttu-id="c5543-123">Přidání Atomic Learning z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c5543-123">Adding Atomic Learning from hello gallery</span></span>
<span data-ttu-id="c5543-124">tooconfigure hello integrace Atomic Learning do Azure AD, je nutné tooadd Atomic Learning hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c5543-124">tooconfigure hello integration of Atomic Learning into Azure AD, you need tooadd Atomic Learning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c5543-125">**tooadd Atomic Learning z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c5543-125">**tooadd Atomic Learning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c5543-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c5543-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c5543-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c5543-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c5543-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c5543-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c5543-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c5543-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c5543-133">Hello vyhledávacího pole zadejte **Atomic Learning**.</span><span class="sxs-lookup"><span data-stu-id="c5543-133">In hello search box, type **Atomic Learning**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_search.png)

5. <span data-ttu-id="c5543-135">Na panelu výsledků hello vyberte **Atomic Learning**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5543-135">In hello results panel, select **Atomic Learning**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c5543-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c5543-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c5543-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Atomic Learning podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="c5543-138">In this section, you configure and test Azure AD single sign-on with Atomic Learning based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c5543-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Atomic Learning je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c5543-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Atomic Learning is tooa user in Azure AD.</span></span> <span data-ttu-id="c5543-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Atomic Learning musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c5543-140">In other words, a link relationship between an Azure AD user and hello related user in Atomic Learning needs toobe established.</span></span>

<span data-ttu-id="c5543-141">V Atomic Learning přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c5543-141">In Atomic Learning, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c5543-142">tooconfigure a testu Azure AD jednotné přihlašování s Atomic Learning, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c5543-142">tooconfigure and test Azure AD single sign-on with Atomic Learning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c5543-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c5543-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c5543-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c5543-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c5543-145">**[Vytváření testovacího uživatele Atomic Learning](#creating-an-atomic-learning-test-user)**  -toohave protějšek Britta Simon v Atomic Learning, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c5543-145">**[Creating an Atomic Learning test user](#creating-an-atomic-learning-test-user)** - toohave a counterpart of Britta Simon in Atomic Learning that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c5543-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c5543-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c5543-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c5543-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c5543-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c5543-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c5543-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Atomic učení.</span><span class="sxs-lookup"><span data-stu-id="c5543-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Atomic Learning application.</span></span>

<span data-ttu-id="c5543-150">**tooconfigure Azure AD jednotné přihlašování s Atomic Learning, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c5543-150">**tooconfigure Azure AD single sign-on with Atomic Learning, perform hello following steps:**</span></span>

1. <span data-ttu-id="c5543-151">V portálu Azure, na hello hello **Atomic Learning** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c5543-151">In hello Azure portal, on hello **Atomic Learning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c5543-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c5543-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_samlbase.png)

3. <span data-ttu-id="c5543-155">Na hello **Atomic Learning domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c5543-155">On hello **Atomic Learning Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_url.png)

     <span data-ttu-id="c5543-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://secure2.atomiclearning.com/sso/shibboleth/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="c5543-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://secure2.atomiclearning.com/sso/shibboleth/<companyname>`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="c5543-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="c5543-158">This value is not real.</span></span> <span data-ttu-id="c5543-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c5543-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="c5543-160">Obraťte se na [tým podpory Atomic klientů Learning](mailto:cs@atomiclearning.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c5543-160">Contact [Atomic Learning Client support team](mailto:cs@atomiclearning.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="c5543-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c5543-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_certificate.png) 

5. <span data-ttu-id="c5543-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c5543-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c5543-165">tooconfigure jednotného přihlašování na **Atomic Learning** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Atomic Learning](mailto:cs@atomiclearning.com).</span><span class="sxs-lookup"><span data-stu-id="c5543-165">tooconfigure single sign-on on **Atomic Learning** side, you need toosend hello downloaded **Metadata XML** too[Atomic Learning support team](mailto:cs@atomiclearning.com).</span></span> <span data-ttu-id="c5543-166">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="c5543-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="c5543-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c5543-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c5543-168">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c5543-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c5543-169">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c5543-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c5543-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c5543-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="c5543-171">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c5543-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c5543-173">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c5543-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c5543-174">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c5543-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c5543-176">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c5543-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c5543-178">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c5543-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c5543-180">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c5543-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c5543-182">a.</span><span class="sxs-lookup"><span data-stu-id="c5543-182">a.</span></span> <span data-ttu-id="c5543-183">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c5543-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c5543-184">b.</span><span class="sxs-lookup"><span data-stu-id="c5543-184">b.</span></span> <span data-ttu-id="c5543-185">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c5543-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c5543-186">c.</span><span class="sxs-lookup"><span data-stu-id="c5543-186">c.</span></span> <span data-ttu-id="c5543-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c5543-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c5543-188">d.</span><span class="sxs-lookup"><span data-stu-id="c5543-188">d.</span></span> <span data-ttu-id="c5543-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c5543-189">Click **Create**.</span></span>
 
### <a name="creating-an-atomic-learning-test-user"></a><span data-ttu-id="c5543-190">Vytváření testovacího uživatele Atomic učení</span><span class="sxs-lookup"><span data-stu-id="c5543-190">Creating an Atomic Learning test user</span></span>

<span data-ttu-id="c5543-191">V této části vytvoříte uživatele volal Britta Simon v Atomic učení.</span><span class="sxs-lookup"><span data-stu-id="c5543-191">In this section, you create a user called Britta Simon in Atomic Learning.</span></span> <span data-ttu-id="c5543-192">Atomic Learning podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="c5543-192">Atomic Learning supports just-in-time provisioning, which is by default enabled.</span></span> 

<span data-ttu-id="c5543-193">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="c5543-193">There is no action item for you in this section.</span></span> <span data-ttu-id="c5543-194">Pokud neexistuje, ještě pomocí hello e-mailovou adresu pro uživatele hello, vytvoří se nový uživatel během pokusu o tooaccess Atomic Learning.</span><span class="sxs-lookup"><span data-stu-id="c5543-194">A new user is created during an attempt tooaccess Atomic Learning if it doesn't exist yet using hello email address for hello user.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c5543-195">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c5543-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c5543-196">V této části můžete povolit Britta Simon toouse Azure jednotné přihlašování a udělení přístupu tooAtomic učení.</span><span class="sxs-lookup"><span data-stu-id="c5543-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAtomic Learning.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c5543-198">**tooassign tooAtomic Britta Simon učení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c5543-198">**tooassign Britta Simon tooAtomic Learning, perform hello following steps:**</span></span>

1. <span data-ttu-id="c5543-199">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c5543-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c5543-201">V seznamu aplikace hello vyberte **Atomic Learning**.</span><span class="sxs-lookup"><span data-stu-id="c5543-201">In hello applications list, select **Atomic Learning**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_app.png) 

3. <span data-ttu-id="c5543-203">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c5543-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c5543-205">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c5543-205">Click **Add** button.</span></span> <span data-ttu-id="c5543-206">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c5543-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c5543-208">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c5543-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c5543-209">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c5543-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c5543-210">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c5543-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c5543-211">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c5543-211">Testing single sign-on</span></span>

<span data-ttu-id="c5543-212">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c5543-212">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c5543-213">Když kliknete na dlaždici Atomic Learning hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Atomic Learning aplikací.</span><span class="sxs-lookup"><span data-stu-id="c5543-213">When you click hello Atomic Learning tile in hello Access Panel, you should get automatically signed-on tooyour Atomic Learning application.</span></span>
<span data-ttu-id="c5543-214">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c5543-214">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c5543-215">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c5543-215">Additional resources</span></span>

* [<span data-ttu-id="c5543-216">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c5543-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c5543-217">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c5543-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_203.png

