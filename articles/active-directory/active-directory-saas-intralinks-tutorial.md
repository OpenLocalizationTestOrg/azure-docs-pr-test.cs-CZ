---
title: 'Kurz: Azure Active Directory integrace s Intralinks | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Intralinks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 6fa49c932d0c48d4b48e04fe91af9fc86a0c1cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a><span data-ttu-id="078e2-103">Kurz: Azure Active Directory integrace s Intralinks</span><span class="sxs-lookup"><span data-stu-id="078e2-103">Tutorial: Azure Active Directory integration with Intralinks</span></span>

<span data-ttu-id="078e2-104">V tomto kurzu zjistíte, jak toointegrate Intralinks s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="078e2-104">In this tutorial, you learn how toointegrate Intralinks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="078e2-105">Integrace Intralinks s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="078e2-105">Integrating Intralinks with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="078e2-106">Můžete řídit ve službě Azure AD, který má přístup tooIntralinks</span><span class="sxs-lookup"><span data-stu-id="078e2-106">You can control in Azure AD who has access tooIntralinks</span></span>
- <span data-ttu-id="078e2-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooIntralinks (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="078e2-107">You can enable your users tooautomatically get signed-on tooIntralinks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="078e2-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="078e2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="078e2-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="078e2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="078e2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="078e2-110">Prerequisites</span></span>

<span data-ttu-id="078e2-111">Integrace služby Azure AD s Intralinks tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="078e2-111">tooconfigure Azure AD integration with Intralinks, you need hello following items:</span></span>

- <span data-ttu-id="078e2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="078e2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="078e2-113">Intralinks jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="078e2-113">An Intralinks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="078e2-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="078e2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="078e2-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="078e2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="078e2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="078e2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="078e2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="078e2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="078e2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="078e2-118">Scenario description</span></span>
<span data-ttu-id="078e2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="078e2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="078e2-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="078e2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="078e2-121">Přidání Intralinks z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="078e2-121">Adding Intralinks from hello gallery</span></span>
2. <span data-ttu-id="078e2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="078e2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intralinks-from-hello-gallery"></a><span data-ttu-id="078e2-123">Přidání Intralinks z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="078e2-123">Adding Intralinks from hello gallery</span></span>
<span data-ttu-id="078e2-124">tooconfigure hello integrace Intralinks do Azure AD, je nutné tooadd Intralinks hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="078e2-124">tooconfigure hello integration of Intralinks into Azure AD, you need tooadd Intralinks from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="078e2-125">**tooadd Intralinks z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="078e2-125">**tooadd Intralinks from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="078e2-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="078e2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="078e2-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="078e2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="078e2-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="078e2-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="078e2-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="078e2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="078e2-133">Hello vyhledávacího pole zadejte **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="078e2-133">In hello search box, type **Intralinks**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="078e2-135">Na panelu výsledků hello vyberte **Intralinks**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="078e2-135">In hello results panel, select **Intralinks**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="078e2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="078e2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="078e2-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Intralinks podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="078e2-138">In this section, you configure and test Azure AD single sign-on with Intralinks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="078e2-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Intralinks je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="078e2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Intralinks is tooa user in Azure AD.</span></span> <span data-ttu-id="078e2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Intralinks musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="078e2-140">In other words, a link relationship between an Azure AD user and hello related user in Intralinks needs toobe established.</span></span>

<span data-ttu-id="078e2-141">V Intralinks, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="078e2-141">In Intralinks, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="078e2-142">tooconfigure a testu Azure AD jednotné přihlašování s Intralinks, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="078e2-142">tooconfigure and test Azure AD single sign-on with Intralinks, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="078e2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="078e2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="078e2-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="078e2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="078e2-145">**[Vytváření testovacího uživatele Intralinks](#creating-an-intralinks-test-user)**  -toohave protějšek Britta Simon v Intralinks, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="078e2-145">**[Creating an Intralinks test user](#creating-an-intralinks-test-user)** - toohave a counterpart of Britta Simon in Intralinks that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="078e2-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="078e2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="078e2-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="078e2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="078e2-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="078e2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="078e2-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Intralinks.</span><span class="sxs-lookup"><span data-stu-id="078e2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Intralinks application.</span></span>

<span data-ttu-id="078e2-150">**tooconfigure Azure AD jednotné přihlašování s Intralinks, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="078e2-150">**tooconfigure Azure AD single sign-on with Intralinks, perform hello following steps:**</span></span>

1. <span data-ttu-id="078e2-151">V portálu Azure, na hello hello **Intralinks** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="078e2-151">In hello Azure portal, on hello **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="078e2-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="078e2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. <span data-ttu-id="078e2-155">Na hello **Intralinks domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="078e2-155">On hello **Intralinks Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_url.png)

    <span data-ttu-id="078e2-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span><span class="sxs-lookup"><span data-stu-id="078e2-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:  `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="078e2-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="078e2-158">This value is not real.</span></span> <span data-ttu-id="078e2-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="078e2-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="078e2-160">Obraťte se na [tým podpory Intralinks klienta](https://www.intralinks.com/contact-1) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="078e2-160">Contact [Intralinks Client support team](https://www.intralinks.com/contact-1) tooget this value.</span></span> 
 
4. <span data-ttu-id="078e2-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="078e2-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. <span data-ttu-id="078e2-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="078e2-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="078e2-165">tooconfigure jednotného přihlašování na **Intralinks** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** [tým podpory Intralinks](https://www.intralinks.com/contact-1).</span><span class="sxs-lookup"><span data-stu-id="078e2-165">tooconfigure single sign-on on **Intralinks** side, you need toosend hello downloaded **Metadata XML** [Intralinks support team](https://www.intralinks.com/contact-1).</span></span> <span data-ttu-id="078e2-166">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="078e2-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="078e2-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="078e2-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="078e2-168">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="078e2-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="078e2-169">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="078e2-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="078e2-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="078e2-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="078e2-171">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="078e2-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="078e2-173">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="078e2-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="078e2-174">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="078e2-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="078e2-176">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="078e2-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="078e2-178">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="078e2-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="078e2-180">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="078e2-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="078e2-182">a.</span><span class="sxs-lookup"><span data-stu-id="078e2-182">a.</span></span> <span data-ttu-id="078e2-183">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="078e2-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="078e2-184">b.</span><span class="sxs-lookup"><span data-stu-id="078e2-184">b.</span></span> <span data-ttu-id="078e2-185">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="078e2-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="078e2-186">c.</span><span class="sxs-lookup"><span data-stu-id="078e2-186">c.</span></span> <span data-ttu-id="078e2-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="078e2-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="078e2-188">d.</span><span class="sxs-lookup"><span data-stu-id="078e2-188">d.</span></span> <span data-ttu-id="078e2-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="078e2-189">Click **Create**.</span></span>
 
### <a name="creating-an-intralinks-test-user"></a><span data-ttu-id="078e2-190">Vytváření testovacího uživatele Intralinks</span><span class="sxs-lookup"><span data-stu-id="078e2-190">Creating an Intralinks test user</span></span>

<span data-ttu-id="078e2-191">V této části vytvoříte volal Britta Simon v Intralinks uživatele.</span><span class="sxs-lookup"><span data-stu-id="078e2-191">In this section, you create a user called Britta Simon in Intralinks.</span></span> <span data-ttu-id="078e2-192">Spojte se s [tým podpory Intralinks](https://www.intralinks.com/contact-1) tooadd hello uživatelé v platformě Intralinks hello.</span><span class="sxs-lookup"><span data-stu-id="078e2-192">Please work with [Intralinks support team](https://www.intralinks.com/contact-1) tooadd hello users in hello Intralinks platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="078e2-193">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="078e2-193">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="078e2-194">V této části povolíte tak, že udělíte přístup tooIntralinks toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="078e2-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIntralinks.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="078e2-196">**tooassign Britta Simon tooIntralinks, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="078e2-196">**tooassign Britta Simon tooIntralinks, perform hello following steps:**</span></span>

1. <span data-ttu-id="078e2-197">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="078e2-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="078e2-199">V seznamu aplikace hello vyberte **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="078e2-199">In hello applications list, select **Intralinks**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_app.png) 

3. <span data-ttu-id="078e2-201">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="078e2-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="078e2-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="078e2-203">Click **Add** button.</span></span> <span data-ttu-id="078e2-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="078e2-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="078e2-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="078e2-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="078e2-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="078e2-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="078e2-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="078e2-208">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="add-intralinks-via-or-elite-application"></a><span data-ttu-id="078e2-209">Přidání Intralinks prostřednictvím nebo Elite aplikace</span><span class="sxs-lookup"><span data-stu-id="078e2-209">Add Intralinks VIA or Elite application</span></span>

<span data-ttu-id="078e2-210">Používá Intralinks hello stejnou platformu identity jednotného přihlašování pro všechny ostatní aplikace Intralinks s výjimkou pozornosti Nexus aplikace.</span><span class="sxs-lookup"><span data-stu-id="078e2-210">Intralinks uses hello same SSO identity platform for all other Intralinks applications excluding Deal Nexus application.</span></span> <span data-ttu-id="078e2-211">Takže pokud máte v plánu toouse žádnou jinou aplikaci Intralinks nejprve máte tooconfigure jednotné přihlašování pro jednu aplikaci primární Intralinks postupem hello popsané výše.</span><span class="sxs-lookup"><span data-stu-id="078e2-211">So if you plan toouse any other Intralinks application then first you have tooconfigure SSO for one Primary Intralinks application using hello procedure described above.</span></span>

<span data-ttu-id="078e2-212">Potom můžete provést hello níže postup tooadd jiná aplikace Intralinks ve vašem klientovi, který můžete využít této primární aplikace pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="078e2-212">After that you can follow hello below procedure tooadd another Intralinks application in your tenant which can leverage this primary application for SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="078e2-213">Tato funkce je k dispozici pouze zákazníkům skladová položka Premium tooAzure AD a není dostupné pro zákazníky volné nebo základní SKU.</span><span class="sxs-lookup"><span data-stu-id="078e2-213">This feature is available only tooAzure AD Premium SKU Customers and not available for Free or Basic SKU customers.</span></span>

1. <span data-ttu-id="078e2-214">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="078e2-214">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]


2. <span data-ttu-id="078e2-216">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="078e2-216">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="078e2-217">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="078e2-217">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="078e2-219">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="078e2-219">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="078e2-221">Hello vyhledávacího pole zadejte **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="078e2-221">In hello search box, type **Intralinks**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="078e2-223">Na **Intralinks přidat aplikaci** provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="078e2-223">On **Intralinks Add app** perform hello following steps:</span></span>

    ![Přidání Intralinks prostřednictvím nebo Elite aplikace](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addapp.png)

    <span data-ttu-id="078e2-225">a.</span><span class="sxs-lookup"><span data-stu-id="078e2-225">a.</span></span> <span data-ttu-id="078e2-226">V **název** textovému poli, například zadejte vhodný název aplikace hello **Intralinks Elite**.</span><span class="sxs-lookup"><span data-stu-id="078e2-226">In **Name** textbox, enter appropriate name of hello application e.g. **Intralinks Elite**.</span></span>

    <span data-ttu-id="078e2-227">b.</span><span class="sxs-lookup"><span data-stu-id="078e2-227">b.</span></span> <span data-ttu-id="078e2-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="078e2-228">Click **Add** button.</span></span>

6.  <span data-ttu-id="078e2-229">V portálu Azure, na hello hello **Intralinks** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="078e2-229">In hello Azure portal, on hello **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

7. <span data-ttu-id="078e2-231">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **propojené přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="078e2-231">On hello **Single sign-on** dialog, select **Mode** as   **Linked Sign-on**.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. <span data-ttu-id="078e2-233">Získat adresu URL k hello hello SP získaných jednotného přihlašování z [Intralinks team](https://www.intralinks.com/contact-1) pro hello jiná aplikace Intralinks a zadejte ho v **konfigurovat přihlašovací adresa URL** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="078e2-233">Get hello hello SP Initiated SSO URL from [Intralinks team](https://www.intralinks.com/contact-1) for hello other Intralinks application and enter it in **Configure Sign-on URL** as shown below.</span></span> 
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     <span data-ttu-id="078e2-235">V textovém poli hello přihlašovací adresa URL zadejte adresu URL hello používá vaše aplikace Intralinks tooyour toosign na uživatele pomocí hello následující vzor:</span><span class="sxs-lookup"><span data-stu-id="078e2-235">In hello Sign On URL textbox, type hello URL used by your users toosign-on tooyour Intralinks application using hello following pattern:</span></span>
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. <span data-ttu-id="078e2-236">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="078e2-236">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="078e2-238">Přiřazení toouser aplikace hello nebo skupin, jak je uvedeno v části hello  **[přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**.</span><span class="sxs-lookup"><span data-stu-id="078e2-238">Assign hello application toouser or groups as shown in hello section **[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)**.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="078e2-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="078e2-239">Testing single sign-on</span></span>

<span data-ttu-id="078e2-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="078e2-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="078e2-241">Po kliknutí na tlačítko hello Intralinks dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Intralinks aplikace.</span><span class="sxs-lookup"><span data-stu-id="078e2-241">When you click hello Intralinks tile in hello Access Panel, you should get automatically signed-on tooyour Intralinks application.</span></span>
<span data-ttu-id="078e2-242">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="078e2-242">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="078e2-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="078e2-243">Additional resources</span></span>

* [<span data-ttu-id="078e2-244">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="078e2-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="078e2-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="078e2-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png

