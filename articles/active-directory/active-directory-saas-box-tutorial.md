---
title: 'Kurz: Azure Active Directory integrace s pole | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a pole."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5f3517f8-30f2-4be7-9e47-43d702701797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: e13a7979761a0b30ecdaac242f1f57a7f8da54c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-box"></a><span data-ttu-id="ec5c1-103">Kurz: Azure Active Directory integrace s poli</span><span class="sxs-lookup"><span data-stu-id="ec5c1-103">Tutorial: Azure Active Directory integration with Box</span></span>

<span data-ttu-id="ec5c1-104">V tomto kurzu zjistíte, jak toointegrate pole s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ec5c1-104">In this tutorial, you learn how toointegrate Box with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ec5c1-105">Integrace pole s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ec5c1-105">Integrating Box with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ec5c1-106">Můžete řídit ve službě Azure AD, který má přístup tooBox</span><span class="sxs-lookup"><span data-stu-id="ec5c1-106">You can control in Azure AD who has access tooBox</span></span>
- <span data-ttu-id="ec5c1-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBox (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ec5c1-107">You can enable your users tooautomatically get signed-on tooBox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ec5c1-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ec5c1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ec5c1-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ec5c1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec5c1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ec5c1-110">Prerequisites</span></span>

<span data-ttu-id="ec5c1-111">tooconfigure integrace Azure AD s pole, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ec5c1-111">tooconfigure Azure AD integration with Box, you need hello following items:</span></span>

- <span data-ttu-id="ec5c1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ec5c1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ec5c1-113">Pole jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ec5c1-113">A Box single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ec5c1-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ec5c1-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ec5c1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ec5c1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ec5c1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ec5c1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ec5c1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ec5c1-118">Scenario description</span></span>
<span data-ttu-id="ec5c1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ec5c1-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ec5c1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ec5c1-121">Přidání pole z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ec5c1-121">Adding Box from hello gallery</span></span>
2. <span data-ttu-id="ec5c1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ec5c1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-box-from-hello-gallery"></a><span data-ttu-id="ec5c1-123">Přidání pole z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ec5c1-123">Adding Box from hello gallery</span></span>
<span data-ttu-id="ec5c1-124">tooconfigure hello integrace pole do Azure AD, je nutné tooadd pole ze seznamu tooyour Galerie hello spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-124">tooconfigure hello integration of Box into Azure AD, you need tooadd Box from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ec5c1-125">**tooadd pole z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ec5c1-125">**tooadd Box from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ec5c1-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ec5c1-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ec5c1-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ec5c1-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ec5c1-133">Hello vyhledávacího pole zadejte **pole**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-133">In hello search box, type **Box**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/tutorial_box_search.png)

5. <span data-ttu-id="ec5c1-135">Na panelu výsledků hello vyberte **pole**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-135">In hello results panel, select **Box**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/tutorial_box_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ec5c1-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ec5c1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ec5c1-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s pole podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="ec5c1-138">In this section, you configure and test Azure AD single sign-on with Box based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ec5c1-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v poli je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Box is tooa user in Azure AD.</span></span> <span data-ttu-id="ec5c1-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello pole musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-140">In other words, a link relationship between an Azure AD user and hello related user in Box needs toobe established.</span></span>

<span data-ttu-id="ec5c1-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Box.</span></span>

<span data-ttu-id="ec5c1-142">tooconfigure a testu Azure AD jednotné přihlašování s pole, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="ec5c1-142">tooconfigure and test Azure AD single sign-on with Box, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ec5c1-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ec5c1-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ec5c1-145">**[Vytvoření zkušebního uživatele pole](#creating-a-box-test-user)**  -toohave protějšek Britta Simon pole, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-145">**[Creating a Box test user](#creating-a-box-test-user)** - toohave a counterpart of Britta Simon in Box that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ec5c1-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ec5c1-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ec5c1-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ec5c1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ec5c1-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci pole.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Box application.</span></span>

<span data-ttu-id="ec5c1-150">**tooconfigure Azure AD jednotné přihlašování s poli, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ec5c1-150">**tooconfigure Azure AD single sign-on with Box, perform hello following steps:**</span></span>

1. <span data-ttu-id="ec5c1-151">V portálu Azure, na hello hello **pole** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-151">In hello Azure portal, on hello **Box** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ec5c1-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-153">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_box_samlbase.png)

3. <span data-ttu-id="ec5c1-155">Na hello **pole domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ec5c1-155">On hello **Box Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_box_url.png)

    <span data-ttu-id="ec5c1-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.box.com`</span><span class="sxs-lookup"><span data-stu-id="ec5c1-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.box.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ec5c1-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-158">This value is not real.</span></span> <span data-ttu-id="ec5c1-159">Aktualizujte hodnotu hello s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-159">Update hello value with hello actual Sign-on URL.</span></span> <span data-ttu-id="ec5c1-160">Obraťte se na [tým podpory pole klienta](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-160">Contact [Box Client support team](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) tooget this value.</span></span> 
 
4. <span data-ttu-id="ec5c1-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_box_certificate.png) 

5. <span data-ttu-id="ec5c1-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ec5c1-165">tooget jednotné přihlašování, které jsou nakonfigurované pro aplikace, obraťte se na [tým podpory pole klienta](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) a poskytnout hello stáhnout soubor XML.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-165">tooget SSO configured for your application, Contact [Box Client support team](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) and provide them with hello downloaded XML file.</span></span>

> [!TIP]
> <span data-ttu-id="ec5c1-166">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ec5c1-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ec5c1-167">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ec5c1-168">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ec5c1-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ec5c1-169">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ec5c1-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="ec5c1-170">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ec5c1-172">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ec5c1-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ec5c1-173">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ec5c1-175">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-175">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ec5c1-177">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-177">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ec5c1-179">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec5c1-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ec5c1-181">a.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-181">a.</span></span> <span data-ttu-id="ec5c1-182">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-182">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ec5c1-183">b.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-183">b.</span></span> <span data-ttu-id="ec5c1-184">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-184">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ec5c1-185">c.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-185">c.</span></span> <span data-ttu-id="ec5c1-186">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ec5c1-187">d.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-187">d.</span></span> <span data-ttu-id="ec5c1-188">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-188">Click **Create**.</span></span>
 
### <a name="creating-a-box-test-user"></a><span data-ttu-id="ec5c1-189">Vytvoření zkušebního uživatele pole</span><span class="sxs-lookup"><span data-stu-id="ec5c1-189">Creating a Box test user</span></span>

<span data-ttu-id="ec5c1-190">V této části se vytvoří uživatele volat Britta Simon pole.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-190">In this section, a user called Britta Simon is created in Box.</span></span> <span data-ttu-id="ec5c1-191">Pole podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-191">Box supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="ec5c1-192">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-192">There is no action item for you in this section.</span></span> <span data-ttu-id="ec5c1-193">Pokud uživatel ještě neexistuje v poli, je vytvořen nový pokusíte-li tooaccess pole.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-193">If a user doesn't already exist in Box, a new one is created when you attempt tooaccess Box.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ec5c1-194">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ec5c1-194">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ec5c1-195">V této části povolíte tak, že udělíte přístup tooBox toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-195">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBox.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ec5c1-197">**tooassign Britta Simon tooBox, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ec5c1-197">**tooassign Britta Simon tooBox, perform hello following steps:**</span></span>

1. <span data-ttu-id="ec5c1-198">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-198">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ec5c1-200">V seznamu aplikace hello vyberte **pole**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-200">In hello applications list, select **Box**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_box_app.png) 

3. <span data-ttu-id="ec5c1-202">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-202">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ec5c1-204">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-204">Click **Add** button.</span></span> <span data-ttu-id="ec5c1-205">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ec5c1-207">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-207">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ec5c1-208">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ec5c1-209">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ec5c1-210">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ec5c1-210">Testing single sign-on</span></span>

<span data-ttu-id="ec5c1-211">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-211">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ec5c1-212">Když kliknete na dlaždici hello pole v hello přístupového panelu, měli byste obdržet přihlašovací stránky tooget přihlášeného tooyour pole aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec5c1-212">When you click hello Box tile in hello Access Panel, you should get login page tooget signed-on tooyour Box application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ec5c1-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ec5c1-213">Additional resources</span></span>

* [<span data-ttu-id="ec5c1-214">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ec5c1-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ec5c1-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ec5c1-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="ec5c1-216">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="ec5c1-216">Configure User Provisioning</span></span>](active-directory-saas-box-userprovisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-box-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-box-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-box-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-box-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-box-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-box-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-box-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-box-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-box-tutorial/tutorial_general_203.png

