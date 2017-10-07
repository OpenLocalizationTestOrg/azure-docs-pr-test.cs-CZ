---
title: 'Kurz: Azure Active Directory integrace s RealtimeBoard | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a RealtimeBoard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a37fc1c0-4bae-4173-989b-00de53a0076f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: jeedes
ms.openlocfilehash: 76644c9ba643d61a903295dea4d417716a47774a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-realtimeboard"></a><span data-ttu-id="d1b49-103">Kurz: Azure Active Directory integrace s RealtimeBoard</span><span class="sxs-lookup"><span data-stu-id="d1b49-103">Tutorial: Azure Active Directory integration with RealtimeBoard</span></span>

<span data-ttu-id="d1b49-104">V tomto kurzu zjistíte, jak toointegrate RealtimeBoard s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d1b49-104">In this tutorial, you learn how toointegrate RealtimeBoard with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d1b49-105">Integrace RealtimeBoard s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d1b49-105">Integrating RealtimeBoard with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d1b49-106">Můžete ovládat ve službě Azure AD, který má přístup tooRealtimeBoard.</span><span class="sxs-lookup"><span data-stu-id="d1b49-106">You can control in Azure AD who has access tooRealtimeBoard.</span></span>
- <span data-ttu-id="d1b49-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooRealtimeBoard (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d1b49-107">You can enable your users tooautomatically get signed-on tooRealtimeBoard (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d1b49-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d1b49-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="d1b49-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d1b49-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1b49-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d1b49-110">Prerequisites</span></span>

<span data-ttu-id="d1b49-111">Integrace služby Azure AD s RealtimeBoard tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="d1b49-111">tooconfigure Azure AD integration with RealtimeBoard, you need hello following items:</span></span>

- <span data-ttu-id="d1b49-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d1b49-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d1b49-113">RealtimeBoard jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d1b49-113">A RealtimeBoard single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d1b49-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d1b49-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d1b49-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d1b49-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d1b49-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d1b49-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d1b49-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d1b49-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d1b49-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d1b49-118">Scenario description</span></span>
<span data-ttu-id="d1b49-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d1b49-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d1b49-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d1b49-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d1b49-121">Přidání RealtimeBoard z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d1b49-121">Adding RealtimeBoard from hello gallery</span></span>
2. <span data-ttu-id="d1b49-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d1b49-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-realtimeboard-from-hello-gallery"></a><span data-ttu-id="d1b49-123">Přidání RealtimeBoard z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d1b49-123">Adding RealtimeBoard from hello gallery</span></span>
<span data-ttu-id="d1b49-124">tooconfigure hello integrace RealtimeBoard do Azure AD, je nutné tooadd RealtimeBoard hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d1b49-124">tooconfigure hello integration of RealtimeBoard into Azure AD, you need tooadd RealtimeBoard from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d1b49-125">**tooadd RealtimeBoard z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d1b49-125">**tooadd RealtimeBoard from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1b49-126">V hello ** [portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d1b49-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="d1b49-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d1b49-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d1b49-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d1b49-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="d1b49-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d1b49-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="d1b49-133">Hello vyhledávacího pole zadejte **RealtimeBoard**, vyberte **RealtimeBoard** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1b49-133">In hello search box, type **RealtimeBoard**, select **RealtimeBoard** from result panel then click **Add** button tooadd hello application.</span></span>

    ![RealtimeBoard v seznamu výsledků hello](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d1b49-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d1b49-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d1b49-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s RealtimeBoard podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d1b49-136">In this section, you configure and test Azure AD single sign-on with RealtimeBoard based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d1b49-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v RealtimeBoard je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d1b49-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RealtimeBoard is tooa user in Azure AD.</span></span> <span data-ttu-id="d1b49-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v RealtimeBoard musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="d1b49-138">In other words, a link relationship between an Azure AD user and hello related user in RealtimeBoard needs toobe established.</span></span>

<span data-ttu-id="d1b49-139">V RealtimeBoard, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="d1b49-139">In RealtimeBoard, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d1b49-140">tooconfigure a testu Azure AD jednotné přihlašování s RealtimeBoard, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="d1b49-140">tooconfigure and test Azure AD single sign-on with RealtimeBoard, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d1b49-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on) ** -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="d1b49-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d1b49-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user) ** -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d1b49-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d1b49-143">**[Vytvoření zkušebního uživatele RealtimeBoard](#create-a-realtimeboard-test-user) ** -toohave protějšek Britta Simon v RealtimeBoard, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="d1b49-143">**[Create a RealtimeBoard test user](#create-a-realtimeboard-test-user)** - toohave a counterpart of Britta Simon in RealtimeBoard that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d1b49-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d1b49-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d1b49-145">**[Test jednotného přihlašování](#test-single-sign-on) ** -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="d1b49-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d1b49-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d1b49-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d1b49-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci RealtimeBoard.</span><span class="sxs-lookup"><span data-stu-id="d1b49-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RealtimeBoard application.</span></span>

<span data-ttu-id="d1b49-148">**tooconfigure Azure AD jednotné přihlašování s RealtimeBoard, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d1b49-148">**tooconfigure Azure AD single sign-on with RealtimeBoard, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1b49-149">V portálu Azure, na hello hello **RealtimeBoard** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d1b49-149">In hello Azure portal, on hello **RealtimeBoard** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="d1b49-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d1b49-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_samlbase.png)

3. <span data-ttu-id="d1b49-153">Na hello **RealtimeBoard domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="d1b49-153">On hello **RealtimeBoard Domain and URLs** section, if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![RealtimeBoard domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_url.png)

    <span data-ttu-id="d1b49-155">V hello **identifikátor** textovému poli, zadejte adresu URL jako:`https://realtimeboard.com/`</span><span class="sxs-lookup"><span data-stu-id="d1b49-155">In hello **Identifier** textbox, type a URL as: `https://realtimeboard.com/`</span></span>

4. <span data-ttu-id="d1b49-156">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="d1b49-156">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_url2.png)

    <span data-ttu-id="d1b49-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://realtimeboard.com/sso/saml`</span><span class="sxs-lookup"><span data-stu-id="d1b49-158">In hello **Sign-on URL** textbox, type a URL as: `https://realtimeboard.com/sso/saml`</span></span>

5. <span data-ttu-id="d1b49-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d1b49-159">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_certificate.png) 

6. <span data-ttu-id="d1b49-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d1b49-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="d1b49-163">tooconfigure jednotného přihlašování na **RealtimeBoard** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory RealtimeBoard](mailto:support@realtimeboard.com).</span><span class="sxs-lookup"><span data-stu-id="d1b49-163">tooconfigure single sign-on on **RealtimeBoard** side, you need toosend hello downloaded **Metadata XML** too[RealtimeBoard support team](mailto:support@realtimeboard.com).</span></span> <span data-ttu-id="d1b49-164">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="d1b49-164">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d1b49-165">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="d1b49-165">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d1b49-166">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello ** Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="d1b49-166">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d1b49-167">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d1b49-167">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d1b49-168">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d1b49-168">Create an Azure AD test user</span></span>

<span data-ttu-id="d1b49-169">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="d1b49-169">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="d1b49-171">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d1b49-171">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1b49-172">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d1b49-172">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d1b49-174">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d1b49-174">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d1b49-176">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d1b49-176">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d1b49-178">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d1b49-178">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d1b49-180">a.</span><span class="sxs-lookup"><span data-stu-id="d1b49-180">a.</span></span> <span data-ttu-id="d1b49-181">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d1b49-181">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d1b49-182">b.</span><span class="sxs-lookup"><span data-stu-id="d1b49-182">b.</span></span> <span data-ttu-id="d1b49-183">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d1b49-183">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="d1b49-184">c.</span><span class="sxs-lookup"><span data-stu-id="d1b49-184">c.</span></span> <span data-ttu-id="d1b49-185">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="d1b49-185">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="d1b49-186">d.</span><span class="sxs-lookup"><span data-stu-id="d1b49-186">d.</span></span> <span data-ttu-id="d1b49-187">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d1b49-187">Click **Create**.</span></span>
 
### <a name="create-a-realtimeboard-test-user"></a><span data-ttu-id="d1b49-188">Vytvoření zkušebního uživatele RealtimeBoard</span><span class="sxs-lookup"><span data-stu-id="d1b49-188">Create a RealtimeBoard test user</span></span>

<span data-ttu-id="d1b49-189">Hello cílem této části je toocreate volal Britta Simon v RealtimeBoard uživatele.</span><span class="sxs-lookup"><span data-stu-id="d1b49-189">hello objective of this section is toocreate a user called Britta Simon in RealtimeBoard.</span></span> <span data-ttu-id="d1b49-190">RealtimeBoard podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="d1b49-190">RealtimeBoard supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="d1b49-191">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="d1b49-191">There is no action item for you in this section.</span></span> <span data-ttu-id="d1b49-192">Pokud uživatel v RealtimeBoard ještě neexistuje, je vytvořen nový pokusíte-li tooaccess RealtimeBoard.</span><span class="sxs-lookup"><span data-stu-id="d1b49-192">If a user doesn't already exist in RealtimeBoard, a new one is created when you attempt tooaccess RealtimeBoard.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="d1b49-193">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="d1b49-193">Assign hello Azure AD test user</span></span>

<span data-ttu-id="d1b49-194">V této části povolíte tak, že udělíte přístup tooRealtimeBoard toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d1b49-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRealtimeBoard.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="d1b49-196">**tooassign Britta Simon tooRealtimeBoard, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d1b49-196">**tooassign Britta Simon tooRealtimeBoard, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1b49-197">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d1b49-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d1b49-199">V seznamu aplikace hello vyberte **RealtimeBoard**.</span><span class="sxs-lookup"><span data-stu-id="d1b49-199">In hello applications list, select **RealtimeBoard**.</span></span>

    ![v seznamu aplikace hello Hello RealtimeBoard odkaz](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_app.png)  

3. <span data-ttu-id="d1b49-201">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d1b49-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="d1b49-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d1b49-203">Click **Add** button.</span></span> <span data-ttu-id="d1b49-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d1b49-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="d1b49-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="d1b49-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d1b49-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d1b49-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d1b49-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d1b49-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d1b49-209">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d1b49-209">Test single sign-on</span></span>

<span data-ttu-id="d1b49-210">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d1b49-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d1b49-211">Když kliknete na dlaždici RealtimeBoard hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour RealtimeBoard aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1b49-211">When you click hello RealtimeBoard tile in hello Access Panel, you should get automatically signed-on tooyour RealtimeBoard application.</span></span>
<span data-ttu-id="d1b49-212">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d1b49-212">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d1b49-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d1b49-213">Additional resources</span></span>

* [<span data-ttu-id="d1b49-214">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d1b49-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d1b49-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d1b49-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_203.png

