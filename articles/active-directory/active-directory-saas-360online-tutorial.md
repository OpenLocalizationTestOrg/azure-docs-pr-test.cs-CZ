---
title: 'Kurz: Azure Active Directory integrace s 360 Online | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a 360 Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cda8eba6-843f-4a09-8c55-0aaf6e593d75
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 413e4e2c41336f99e1999857c788c5dac15be4c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-360-online"></a><span data-ttu-id="8e75b-103">Kurz: Azure Active Directory integrace s 360 Online</span><span class="sxs-lookup"><span data-stu-id="8e75b-103">Tutorial: Azure Active Directory integration with 360 Online</span></span>

<span data-ttu-id="8e75b-104">V tomto kurzu zjistíte, jak toointegrate 360 Online službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8e75b-104">In this tutorial, you learn how toointegrate 360 Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8e75b-105">Integrace 360 Online s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8e75b-105">Integrating 360 Online with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8e75b-106">Můžete řídit ve službě Azure AD, který má přístup too360 Online</span><span class="sxs-lookup"><span data-stu-id="8e75b-106">You can control in Azure AD who has access too360 Online</span></span>
- <span data-ttu-id="8e75b-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného too360 Online (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e75b-107">You can enable your users tooautomatically get signed-on too360 Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8e75b-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8e75b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8e75b-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8e75b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e75b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8e75b-110">Prerequisites</span></span>

<span data-ttu-id="8e75b-111">tooconfigure integrace Azure AD s 360 Online, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8e75b-111">tooconfigure Azure AD integration with 360 Online, you need hello following items:</span></span>

- <span data-ttu-id="8e75b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e75b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8e75b-113">Předplatné povolené 360 Online jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8e75b-113">A 360 Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8e75b-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8e75b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8e75b-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8e75b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8e75b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8e75b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8e75b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8e75b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8e75b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8e75b-118">Scenario description</span></span>
<span data-ttu-id="8e75b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8e75b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8e75b-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8e75b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8e75b-121">Přidání 360 Online z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8e75b-121">Adding 360 Online from hello gallery</span></span>
2. <span data-ttu-id="8e75b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8e75b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-360-online-from-hello-gallery"></a><span data-ttu-id="8e75b-123">Přidání 360 Online z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8e75b-123">Adding 360 Online from hello gallery</span></span>
<span data-ttu-id="8e75b-124">tooconfigure hello integrace 360 Online do služby Azure AD, je nutné tooadd 360 Online hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8e75b-124">tooconfigure hello integration of 360 Online into Azure AD, you need tooadd 360 Online from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8e75b-125">**tooadd 360 Online z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8e75b-125">**tooadd 360 Online from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e75b-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8e75b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8e75b-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8e75b-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8e75b-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8e75b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8e75b-133">Hello vyhledávacího pole zadejte **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-133">In hello search box, type **360 Online**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/tutorial_360online_search.png)

5. <span data-ttu-id="8e75b-135">Na panelu výsledků hello vyberte **360 Online**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8e75b-135">In hello results panel, select **360 Online**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/tutorial_360online_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8e75b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8e75b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8e75b-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 360 Online na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="8e75b-138">In this section, you configure and test Azure AD single sign-on with 360 Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8e75b-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v 360 Online je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8e75b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 360 Online is tooa user in Azure AD.</span></span> <span data-ttu-id="8e75b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v 360 Online musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8e75b-140">In other words, a link relationship between an Azure AD user and hello related user in 360 Online needs toobe established.</span></span>

<span data-ttu-id="8e75b-141">V 360 Online přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="8e75b-141">In 360 Online, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8e75b-142">tooconfigure a testu Azure AD jednotné přihlašování s 360 Online, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="8e75b-142">tooconfigure and test Azure AD single sign-on with 360 Online, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8e75b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8e75b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8e75b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8e75b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8e75b-145">**[Vytváření 360 Online testovacího uživatele](#creating-a-360-online-test-user)**  -toohave protějšek Britta Simon v 360 Online, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8e75b-145">**[Creating a 360 Online test user](#creating-a-360-online-test-user)** - toohave a counterpart of Britta Simon in 360 Online that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8e75b-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8e75b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8e75b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8e75b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8e75b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8e75b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8e75b-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci 360 Online.</span><span class="sxs-lookup"><span data-stu-id="8e75b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 360 Online application.</span></span>

<span data-ttu-id="8e75b-150">**tooconfigure Azure AD jednotné přihlašování s 360 Online, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8e75b-150">**tooconfigure Azure AD single sign-on with 360 Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e75b-151">V portálu Azure, na hello hello **360 Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-151">In hello Azure portal, on hello **360 Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8e75b-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8e75b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_360online_samlbase.png)

3. <span data-ttu-id="8e75b-155">Na hello **360 Online domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8e75b-155">On hello **360 Online Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_360online_url.png)

    <span data-ttu-id="8e75b-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.public360online.com`</span><span class="sxs-lookup"><span data-stu-id="8e75b-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.public360online.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8e75b-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="8e75b-158">hello value is not real.</span></span> <span data-ttu-id="8e75b-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8e75b-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="8e75b-160">Obraťte se na [360 tým podpory Online klienta](mailto:360online@software-innovation.com) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8e75b-160">Contact [360 Online Client support team](mailto:360online@software-innovation.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="8e75b-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8e75b-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_360online_certificate.png) 

5. <span data-ttu-id="8e75b-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8e75b-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8e75b-165">tooconfigure jednotného přihlašování na **360 Online** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory 360 Online](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="8e75b-165">tooconfigure single sign-on on **360 Online** side, you need toosend hello downloaded **Metadata XML** too[360 Online support team](mailto:360online@software-innovation.com).</span></span> 

> [!TIP]
> <span data-ttu-id="8e75b-166">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="8e75b-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8e75b-167">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="8e75b-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8e75b-168">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8e75b-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8e75b-169">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e75b-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="8e75b-170">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="8e75b-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8e75b-172">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8e75b-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e75b-173">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8e75b-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8e75b-175">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-175">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8e75b-177">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="8e75b-177">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8e75b-179">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8e75b-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8e75b-181">a.</span><span class="sxs-lookup"><span data-stu-id="8e75b-181">a.</span></span> <span data-ttu-id="8e75b-182">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-182">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8e75b-183">b.</span><span class="sxs-lookup"><span data-stu-id="8e75b-183">b.</span></span> <span data-ttu-id="8e75b-184">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8e75b-184">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8e75b-185">c.</span><span class="sxs-lookup"><span data-stu-id="8e75b-185">c.</span></span> <span data-ttu-id="8e75b-186">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8e75b-187">d.</span><span class="sxs-lookup"><span data-stu-id="8e75b-187">d.</span></span> <span data-ttu-id="8e75b-188">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-188">Click **Create**.</span></span>
 
### <a name="creating-a-360-online-test-user"></a><span data-ttu-id="8e75b-189">Vytváření 360 Online zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="8e75b-189">Creating a 360 Online test user</span></span>

<span data-ttu-id="8e75b-190">V této části vytvoříte uživatele volal Britta Simon v 360 Online.</span><span class="sxs-lookup"><span data-stu-id="8e75b-190">In this section, you create a user called Britta Simon in 360 Online.</span></span> <span data-ttu-id="8e75b-191">je třeba toocontact [tým podpory 360 Online](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="8e75b-191">you need toocontact [360 Online support team](mailto:360online@software-innovation.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8e75b-192">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8e75b-192">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8e75b-193">V této části povolíte tak, že udělíte přístup too360 Online toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8e75b-193">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too360 Online.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8e75b-195">**tooassign Britta Simon too360 Online, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8e75b-195">**tooassign Britta Simon too360 Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e75b-196">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-196">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8e75b-198">V seznamu aplikace hello vyberte **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-198">In hello applications list, select **360 Online**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_360online_app.png) 

3. <span data-ttu-id="8e75b-200">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8e75b-200">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8e75b-202">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8e75b-202">Click **Add** button.</span></span> <span data-ttu-id="8e75b-203">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8e75b-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8e75b-205">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8e75b-205">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8e75b-206">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8e75b-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8e75b-207">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8e75b-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8e75b-208">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8e75b-208">Testing single sign-on</span></span>

<span data-ttu-id="8e75b-209">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8e75b-209">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8e75b-210">Po kliknutí na tlačítko hello 360 Online dlaždice v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného 360 Online aplikace.</span><span class="sxs-lookup"><span data-stu-id="8e75b-210">When you click hello 360 Online tile in hello Access Panel, you should get automatically signed-on tooyour 360 Online application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8e75b-211">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8e75b-211">Additional resources</span></span>

* [<span data-ttu-id="8e75b-212">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8e75b-212">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8e75b-213">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8e75b-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-360online-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png

