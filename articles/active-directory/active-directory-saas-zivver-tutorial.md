---
title: 'Kurz: Azure Active Directory integrace s ZIVVER | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ZIVVER."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 64cb7ea0-df6c-4963-84d8-6f435980e2de
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 0928abcc4dcbd97b892298f4919c8d44e1a058a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zivver"></a><span data-ttu-id="5fc03-103">Kurz: Azure Active Directory integrace s ZIVVER</span><span class="sxs-lookup"><span data-stu-id="5fc03-103">Tutorial: Azure Active Directory integration with ZIVVER</span></span>

<span data-ttu-id="5fc03-104">V tomto kurzu zjistíte, jak toointegrate ZIVVER s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5fc03-104">In this tutorial, you learn how toointegrate ZIVVER with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5fc03-105">Integrace ZIVVER s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5fc03-105">Integrating ZIVVER with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5fc03-106">Můžete ovládat ve službě Azure AD, který má přístup tooZIVVER.</span><span class="sxs-lookup"><span data-stu-id="5fc03-106">You can control in Azure AD who has access tooZIVVER.</span></span>
- <span data-ttu-id="5fc03-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooZIVVER (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5fc03-107">You can enable your users tooautomatically get signed-on tooZIVVER (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="5fc03-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5fc03-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="5fc03-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5fc03-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5fc03-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5fc03-110">Prerequisites</span></span>

<span data-ttu-id="5fc03-111">Integrace služby Azure AD s ZIVVER tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5fc03-111">tooconfigure Azure AD integration with ZIVVER, you need hello following items:</span></span>

- <span data-ttu-id="5fc03-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5fc03-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5fc03-113">ZIVVER jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5fc03-113">A ZIVVER single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5fc03-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5fc03-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5fc03-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5fc03-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5fc03-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5fc03-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5fc03-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5fc03-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5fc03-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5fc03-118">Scenario description</span></span>
<span data-ttu-id="5fc03-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5fc03-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5fc03-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5fc03-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5fc03-121">Přidání ZIVVER z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5fc03-121">Adding ZIVVER from hello gallery</span></span>
2. <span data-ttu-id="5fc03-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5fc03-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zivver-from-hello-gallery"></a><span data-ttu-id="5fc03-123">Přidání ZIVVER z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5fc03-123">Adding ZIVVER from hello gallery</span></span>
<span data-ttu-id="5fc03-124">tooconfigure hello integrace ZIVVER do Azure AD, je nutné tooadd ZIVVER hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5fc03-124">tooconfigure hello integration of ZIVVER into Azure AD, you need tooadd ZIVVER from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5fc03-125">**tooadd ZIVVER z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5fc03-125">**tooadd ZIVVER from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5fc03-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5fc03-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="5fc03-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5fc03-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5fc03-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5fc03-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="5fc03-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5fc03-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="5fc03-133">Hello vyhledávacího pole zadejte **ZIVVER**, vyberte **ZIVVER** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5fc03-133">In hello search box, type **ZIVVER**, select **ZIVVER** from result panel then click **Add** button tooadd hello application.</span></span>

    ![ZIVVER v seznamu výsledků hello](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5fc03-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5fc03-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="5fc03-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ZIVVER podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5fc03-136">In this section, you configure and test Azure AD single sign-on with ZIVVER based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5fc03-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ZIVVER je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5fc03-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ZIVVER is tooa user in Azure AD.</span></span> <span data-ttu-id="5fc03-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ZIVVER musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5fc03-138">In other words, a link relationship between an Azure AD user and hello related user in ZIVVER needs toobe established.</span></span>

<span data-ttu-id="5fc03-139">V ZIVVER, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5fc03-139">In ZIVVER, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5fc03-140">tooconfigure a testu Azure AD jednotné přihlašování s ZIVVER, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5fc03-140">tooconfigure and test Azure AD single sign-on with ZIVVER, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5fc03-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5fc03-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5fc03-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5fc03-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5fc03-143">**[Vytvoření zkušebního uživatele ZIVVER](#create-a-zivver-test-user)**  -toohave protějšek Britta Simon v ZIVVER, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5fc03-143">**[Create a ZIVVER test user](#create-a-zivver-test-user)** - toohave a counterpart of Britta Simon in ZIVVER that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5fc03-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5fc03-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5fc03-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5fc03-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="5fc03-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5fc03-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="5fc03-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ZIVVER.</span><span class="sxs-lookup"><span data-stu-id="5fc03-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ZIVVER application.</span></span>

<span data-ttu-id="5fc03-148">**tooconfigure Azure AD jednotné přihlašování s ZIVVER, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5fc03-148">**tooconfigure Azure AD single sign-on with ZIVVER, perform hello following steps:**</span></span>

1. <span data-ttu-id="5fc03-149">V portálu Azure, na hello hello **ZIVVER** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5fc03-149">In hello Azure portal, on hello **ZIVVER** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="5fc03-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5fc03-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_samlbase.png)

3. <span data-ttu-id="5fc03-153">Na hello **ZIVVER domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5fc03-153">On hello **ZIVVER Domain and URLs** section, perform hello following steps:</span></span>

    ![ZIVVER domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_url.png)

    <span data-ttu-id="5fc03-155">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://app.zivver.com/SAML/Zivver`</span><span class="sxs-lookup"><span data-stu-id="5fc03-155">In hello **Identifier** textbox, type hello URL: `https://app.zivver.com/SAML/Zivver`</span></span>

4. <span data-ttu-id="5fc03-156">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5fc03-156">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_certificate.png) 

5. <span data-ttu-id="5fc03-158">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5fc03-158">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-zivver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5fc03-160">tooconfigure jednotného přihlašování na **ZIVVER** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory ZIVVER](https://support.zivver.com).</span><span class="sxs-lookup"><span data-stu-id="5fc03-160">tooconfigure single sign-on on **ZIVVER** side, you need toosend hello downloaded **Metadata XML** too[ZIVVER support team](https://support.zivver.com).</span></span> <span data-ttu-id="5fc03-161">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="5fc03-161">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="5fc03-162">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5fc03-162">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5fc03-163">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5fc03-163">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5fc03-164">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5fc03-164">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5fc03-165">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5fc03-165">Create an Azure AD test user</span></span>

<span data-ttu-id="5fc03-166">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5fc03-166">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="5fc03-168">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5fc03-168">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5fc03-169">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5fc03-169">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-zivver-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="5fc03-171">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5fc03-171">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-zivver-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="5fc03-173">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5fc03-173">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-zivver-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="5fc03-175">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5fc03-175">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-zivver-tutorial/create_aaduser_04.png)

    <span data-ttu-id="5fc03-177">a.</span><span class="sxs-lookup"><span data-stu-id="5fc03-177">a.</span></span> <span data-ttu-id="5fc03-178">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5fc03-178">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5fc03-179">b.</span><span class="sxs-lookup"><span data-stu-id="5fc03-179">b.</span></span> <span data-ttu-id="5fc03-180">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5fc03-180">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="5fc03-181">c.</span><span class="sxs-lookup"><span data-stu-id="5fc03-181">c.</span></span> <span data-ttu-id="5fc03-182">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="5fc03-182">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="5fc03-183">d.</span><span class="sxs-lookup"><span data-stu-id="5fc03-183">d.</span></span> <span data-ttu-id="5fc03-184">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5fc03-184">Click **Create**.</span></span>
  
### <a name="create-a-zivver-test-user"></a><span data-ttu-id="5fc03-185">Vytvoření zkušebního uživatele ZIVVER</span><span class="sxs-lookup"><span data-stu-id="5fc03-185">Create a ZIVVER test user</span></span>

<span data-ttu-id="5fc03-186">V této části vytvoříte volal Britta Simon v ZIVVER uživatele.</span><span class="sxs-lookup"><span data-stu-id="5fc03-186">In this section, you create a user called Britta Simon in ZIVVER.</span></span> <span data-ttu-id="5fc03-187">Práce s [tým podpory ZIVVER](https://support.zivver.com) tooadd hello uživatelé v platformě ZIVVER hello.</span><span class="sxs-lookup"><span data-stu-id="5fc03-187">Work with [ZIVVER support team](https://support.zivver.com) tooadd hello users in hello ZIVVER platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="5fc03-188">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5fc03-188">Assign hello Azure AD test user</span></span>

<span data-ttu-id="5fc03-189">V této části povolíte tak, že udělíte přístup tooZIVVER toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5fc03-189">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZIVVER.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="5fc03-191">**tooassign Britta Simon tooZIVVER, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5fc03-191">**tooassign Britta Simon tooZIVVER, perform hello following steps:**</span></span>

1. <span data-ttu-id="5fc03-192">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5fc03-192">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5fc03-194">V seznamu aplikace hello vyberte **ZIVVER**.</span><span class="sxs-lookup"><span data-stu-id="5fc03-194">In hello applications list, select **ZIVVER**.</span></span>

    ![v seznamu aplikace hello Hello ZIVVER odkaz](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_app.png)  

3. <span data-ttu-id="5fc03-196">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5fc03-196">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="5fc03-198">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5fc03-198">Click **Add** button.</span></span> <span data-ttu-id="5fc03-199">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5fc03-199">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="5fc03-201">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5fc03-201">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5fc03-202">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5fc03-202">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5fc03-203">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5fc03-203">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5fc03-204">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5fc03-204">Test single sign-on</span></span>

<span data-ttu-id="5fc03-205">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5fc03-205">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5fc03-206">Po kliknutí na tlačítko hello ZIVVER dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ZIVVER aplikace.</span><span class="sxs-lookup"><span data-stu-id="5fc03-206">When you click hello ZIVVER tile in hello Access Panel, you should get automatically signed-on tooyour ZIVVER application.</span></span>
<span data-ttu-id="5fc03-207">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5fc03-207">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5fc03-208">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5fc03-208">Additional resources</span></span>

* [<span data-ttu-id="5fc03-209">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5fc03-209">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5fc03-210">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5fc03-210">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_203.png

