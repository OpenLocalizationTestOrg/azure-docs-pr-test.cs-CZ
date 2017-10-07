---
title: 'Kurz: Azure Active Directory integrace s Nomadic | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Nomadic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 13d02b1c-d98a-40b1-824f-afa45a2deb6a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 8c1d3e350ce03c373cf475b2786ec299a7ce5f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nomadic"></a><span data-ttu-id="6f67a-103">Kurz: Azure Active Directory integrace s Nomadic</span><span class="sxs-lookup"><span data-stu-id="6f67a-103">Tutorial: Azure Active Directory integration with Nomadic</span></span>

<span data-ttu-id="6f67a-104">V tomto kurzu zjistíte, jak toointegrate Nomadic s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6f67a-104">In this tutorial, you learn how toointegrate Nomadic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6f67a-105">Integrace Nomadic s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6f67a-105">Integrating Nomadic with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6f67a-106">Můžete ovládat ve službě Azure AD, který má přístup tooNomadic.</span><span class="sxs-lookup"><span data-stu-id="6f67a-106">You can control in Azure AD who has access tooNomadic.</span></span>
- <span data-ttu-id="6f67a-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooNomadic (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6f67a-107">You can enable your users tooautomatically get signed-on tooNomadic (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="6f67a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6f67a-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="6f67a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6f67a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6f67a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6f67a-110">Prerequisites</span></span>

<span data-ttu-id="6f67a-111">Integrace služby Azure AD s Nomadic tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6f67a-111">tooconfigure Azure AD integration with Nomadic, you need hello following items:</span></span>

- <span data-ttu-id="6f67a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6f67a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6f67a-113">Nomadic jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6f67a-113">A Nomadic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6f67a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6f67a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6f67a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6f67a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6f67a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6f67a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6f67a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6f67a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6f67a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6f67a-118">Scenario description</span></span>
<span data-ttu-id="6f67a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6f67a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6f67a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6f67a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6f67a-121">Přidat Nomadic z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6f67a-121">Add Nomadic from hello gallery</span></span>
2. <span data-ttu-id="6f67a-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6f67a-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-nomadic-from-hello-gallery"></a><span data-ttu-id="6f67a-123">Přidat Nomadic z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6f67a-123">Add Nomadic from hello gallery</span></span>
<span data-ttu-id="6f67a-124">tooconfigure hello integrace Nomadic do Azure AD, je nutné tooadd Nomadic hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6f67a-124">tooconfigure hello integration of Nomadic into Azure AD, you need tooadd Nomadic from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6f67a-125">**tooadd Nomadic z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6f67a-125">**tooadd Nomadic from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6f67a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6f67a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="6f67a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6f67a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="6f67a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f67a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="6f67a-133">Hello vyhledávacího pole zadejte **Nomadic**, vyberte **Nomadic** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f67a-133">In hello search box, type **Nomadic**, select **Nomadic** from result panel then click **Add** button tooadd hello application.</span></span>

    ![V seznamu výsledků hello nomadic](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6f67a-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6f67a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="6f67a-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Nomadic podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6f67a-136">In this section, you configure and test Azure AD single sign-on with Nomadic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6f67a-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Nomadic je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6f67a-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Nomadic is tooa user in Azure AD.</span></span> <span data-ttu-id="6f67a-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Nomadic musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6f67a-138">In other words, a link relationship between an Azure AD user and hello related user in Nomadic needs toobe established.</span></span>

<span data-ttu-id="6f67a-139">V Nomadic, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6f67a-139">In Nomadic, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6f67a-140">tooconfigure a testu Azure AD jednotné přihlašování s Nomadic, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6f67a-140">tooconfigure and test Azure AD single sign-on with Nomadic, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6f67a-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6f67a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6f67a-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6f67a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6f67a-143">**[Vytvoření Nomadic zkušebního uživatele](#create-a-nomadic-test-user)**  -toohave protějšek Britta Simon v Nomadic, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6f67a-143">**[Create a Nomadic test user](#create-a-nomadic-test-user)** - toohave a counterpart of Britta Simon in Nomadic that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6f67a-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6f67a-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6f67a-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6f67a-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6f67a-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6f67a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6f67a-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Nomadic.</span><span class="sxs-lookup"><span data-stu-id="6f67a-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Nomadic application.</span></span>

<span data-ttu-id="6f67a-148">**tooconfigure Azure AD jednotné přihlašování s Nomadic, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6f67a-148">**tooconfigure Azure AD single sign-on with Nomadic, perform hello following steps:**</span></span>

1. <span data-ttu-id="6f67a-149">V portálu Azure, na hello hello **Nomadic** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-149">In hello Azure portal, on hello **Nomadic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="6f67a-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6f67a-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_samlbase.png)

3. <span data-ttu-id="6f67a-153">Na hello **Nomadic domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6f67a-153">On hello **Nomadic Domain and URLs** section, perform hello following steps:</span></span>

    ![Nomadic domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_url.png)

    <span data-ttu-id="6f67a-155">a.</span><span class="sxs-lookup"><span data-stu-id="6f67a-155">a.</span></span> <span data-ttu-id="6f67a-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.nomadic.fm/signin`</span><span class="sxs-lookup"><span data-stu-id="6f67a-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.nomadic.fm/signin`</span></span>

    <span data-ttu-id="6f67a-157">b.</span><span class="sxs-lookup"><span data-stu-id="6f67a-157">b.</span></span> <span data-ttu-id="6f67a-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzor: `https://<company name>.nomadic.fm/auth/saml2/sp`,`https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span><span class="sxs-lookup"><span data-stu-id="6f67a-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.nomadic.fm/auth/saml2/sp`, `https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6f67a-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6f67a-159">These values are not real.</span></span> <span data-ttu-id="6f67a-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6f67a-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6f67a-161">Obraťte se na [tým podpory Nomadic klienta](mailto:help@nomadic.fm) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6f67a-161">Contact [Nomadic Client support team](mailto:help@nomadic.fm) tooget these values.</span></span> 
 


4. <span data-ttu-id="6f67a-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6f67a-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_certificate.png) 

5. <span data-ttu-id="6f67a-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f67a-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-nomadic-tutorial/tutorial_general_400.png)

6.  <span data-ttu-id="6f67a-166">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [tým podpory Nomadic](mailto:help@nomadic.fm) a poskytnout hello Stáhnout **metadata**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-166">tooget SSO configured for your application, contact [Nomadic support team](mailto:help@nomadic.fm) and provide them with hello downloaded **metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="6f67a-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6f67a-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6f67a-168">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6f67a-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6f67a-169">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6f67a-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6f67a-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6f67a-170">Create an Azure AD test user</span></span>

<span data-ttu-id="6f67a-171">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6f67a-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="6f67a-173">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6f67a-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6f67a-174">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f67a-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-nomadic-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="6f67a-176">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-nomadic-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="6f67a-178">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f67a-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-nomadic-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="6f67a-180">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6f67a-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-nomadic-tutorial/create_aaduser_04.png)

    <span data-ttu-id="6f67a-182">a.</span><span class="sxs-lookup"><span data-stu-id="6f67a-182">a.</span></span> <span data-ttu-id="6f67a-183">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6f67a-184">b.</span><span class="sxs-lookup"><span data-stu-id="6f67a-184">b.</span></span> <span data-ttu-id="6f67a-185">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6f67a-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="6f67a-186">c.</span><span class="sxs-lookup"><span data-stu-id="6f67a-186">c.</span></span> <span data-ttu-id="6f67a-187">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="6f67a-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="6f67a-188">d.</span><span class="sxs-lookup"><span data-stu-id="6f67a-188">d.</span></span> <span data-ttu-id="6f67a-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-189">Click **Create**.</span></span>
 
### <a name="create-a-nomadic-test-user"></a><span data-ttu-id="6f67a-190">Vytvoření Nomadic zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="6f67a-190">Create a Nomadic test user</span></span>

<span data-ttu-id="6f67a-191">V této části vytvoříte volal Britta Simon v Nomadic uživatele.</span><span class="sxs-lookup"><span data-stu-id="6f67a-191">In this section, you create a user called Britta Simon in Nomadic.</span></span> <span data-ttu-id="6f67a-192">Spojte se s [tým podpory Nomadic](mailto:help@nomadic.fm) tooadd hello uživatelé v platformě Nomadic hello.</span><span class="sxs-lookup"><span data-stu-id="6f67a-192">Please work with [Nomadic support team](mailto:help@nomadic.fm) tooadd hello users in hello Nomadic platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="6f67a-193">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6f67a-193">Assign hello Azure AD test user</span></span>

<span data-ttu-id="6f67a-194">V této části povolíte tak, že udělíte přístup tooNomadic toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6f67a-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNomadic.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="6f67a-196">**tooassign Britta Simon tooNomadic, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6f67a-196">**tooassign Britta Simon tooNomadic, perform hello following steps:**</span></span>

1. <span data-ttu-id="6f67a-197">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6f67a-199">V seznamu aplikace hello vyberte **Nomadic**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-199">In hello applications list, select **Nomadic**.</span></span>

    ![Hello Nomadic odkaz v seznamu aplikace hello](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_app.png)  

3. <span data-ttu-id="6f67a-201">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6f67a-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="6f67a-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f67a-203">Click **Add** button.</span></span> <span data-ttu-id="6f67a-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f67a-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="6f67a-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6f67a-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6f67a-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f67a-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6f67a-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f67a-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6f67a-209">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6f67a-209">Test single sign-on</span></span>

<span data-ttu-id="6f67a-210">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6f67a-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6f67a-211">Po kliknutí na tlačítko hello Nomadic dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Nomadic aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f67a-211">When you click hello Nomadic tile in hello Access Panel, you should get automatically signed-on tooyour Nomadic application.</span></span>
<span data-ttu-id="6f67a-212">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6f67a-212">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6f67a-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6f67a-213">Additional resources</span></span>

* [<span data-ttu-id="6f67a-214">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f67a-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6f67a-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6f67a-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_203.png

