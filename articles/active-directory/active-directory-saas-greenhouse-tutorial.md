---
title: "Kurz: Azure Active Directory integrace s skleníkových | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a skleníkových."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 78ec1766-4f79-4f16-9a66-d5584c4b6151
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 1a7cdd00c4f2b15a1afc89522d79af22f4c5d866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a><span data-ttu-id="e9673-103">Kurz: Azure Active Directory integrace s skleníkových</span><span class="sxs-lookup"><span data-stu-id="e9673-103">Tutorial: Azure Active Directory integration with Greenhouse</span></span>

<span data-ttu-id="e9673-104">V tomto kurzu zjistíte, jak toointegrate skleníkových službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e9673-104">In this tutorial, you learn how toointegrate Greenhouse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e9673-105">Integrace skleníkových s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e9673-105">Integrating Greenhouse with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e9673-106">Můžete ovládat ve službě Azure AD, který má přístup tooGreenhouse.</span><span class="sxs-lookup"><span data-stu-id="e9673-106">You can control in Azure AD who has access tooGreenhouse.</span></span>
- <span data-ttu-id="e9673-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooGreenhouse (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e9673-107">You can enable your users tooautomatically get signed-on tooGreenhouse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e9673-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e9673-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="e9673-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e9673-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9673-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e9673-110">Prerequisites</span></span>

<span data-ttu-id="e9673-111">Integrace služby Azure AD s skleníkových tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e9673-111">tooconfigure Azure AD integration with Greenhouse, you need hello following items:</span></span>

- <span data-ttu-id="e9673-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e9673-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e9673-113">Skleníkových jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e9673-113">A Greenhouse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e9673-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e9673-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e9673-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e9673-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e9673-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e9673-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e9673-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e9673-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e9673-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e9673-118">Scenario description</span></span>
<span data-ttu-id="e9673-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e9673-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e9673-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e9673-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e9673-121">Přidání skleníkových z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e9673-121">Adding Greenhouse from hello gallery</span></span>
2. <span data-ttu-id="e9673-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e9673-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-greenhouse-from-hello-gallery"></a><span data-ttu-id="e9673-123">Přidání skleníkových z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e9673-123">Adding Greenhouse from hello gallery</span></span>
<span data-ttu-id="e9673-124">tooconfigure hello integrace skleníkových do Azure AD, je nutné tooadd skleníkových hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e9673-124">tooconfigure hello integration of Greenhouse into Azure AD, you need tooadd Greenhouse from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e9673-125">**tooadd skleníkových z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e9673-125">**tooadd Greenhouse from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9673-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e9673-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="e9673-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e9673-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e9673-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e9673-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="e9673-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e9673-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="e9673-133">Hello vyhledávacího pole zadejte **skleníkových**, vyberte **skleníkových** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9673-133">In hello search box, type **Greenhouse**, select **Greenhouse** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Skleníkových v seznamu výsledků hello](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e9673-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e9673-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e9673-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s skleníkových podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e9673-136">In this section, you configure and test Azure AD single sign-on with Greenhouse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e9673-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v skleníkových je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e9673-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Greenhouse is tooa user in Azure AD.</span></span> <span data-ttu-id="e9673-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v skleníkových musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="e9673-138">In other words, a link relationship between an Azure AD user and hello related user in Greenhouse needs toobe established.</span></span>

<span data-ttu-id="e9673-139">V skleníkových, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="e9673-139">In Greenhouse, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e9673-140">tooconfigure a testu Azure AD jednotné přihlašování s skleníkových, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="e9673-140">tooconfigure and test Azure AD single sign-on with Greenhouse, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e9673-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="e9673-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e9673-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e9673-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e9673-143">**[Vytvoření zkušebního uživatele skleníkových](#create-a-greenhouse-test-user)**  -toohave protějšek Britta Simon v skleníkových, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="e9673-143">**[Create a Greenhouse test user](#create-a-greenhouse-test-user)** - toohave a counterpart of Britta Simon in Greenhouse that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e9673-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e9673-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e9673-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="e9673-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e9673-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e9673-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e9673-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci skleníkových.</span><span class="sxs-lookup"><span data-stu-id="e9673-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Greenhouse application.</span></span>

<span data-ttu-id="e9673-148">**tooconfigure Azure AD jednotné přihlašování s skleníkových, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e9673-148">**tooconfigure Azure AD single sign-on with Greenhouse, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9673-149">V portálu Azure, na hello hello **skleníkových** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e9673-149">In hello Azure portal, on hello **Greenhouse** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="e9673-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e9673-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_samlbase.png)

3. <span data-ttu-id="e9673-153">Na hello **skleníkových domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e9673-153">On hello **Greenhouse Domain and URLs** section, perform hello following steps:</span></span>

    ![Skleníkových domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_url.png)

    <span data-ttu-id="e9673-155">a.</span><span class="sxs-lookup"><span data-stu-id="e9673-155">a.</span></span> <span data-ttu-id="e9673-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="e9673-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.greenhouse.io`</span></span>

    <span data-ttu-id="e9673-157">b.</span><span class="sxs-lookup"><span data-stu-id="e9673-157">b.</span></span> <span data-ttu-id="e9673-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="e9673-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.greenhouse.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e9673-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e9673-159">These values are not real.</span></span> <span data-ttu-id="e9673-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e9673-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e9673-161">Obraťte se na [tým podpory skleníkových klienta](https://www.greenhouse.io/contact) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e9673-161">Contact [Greenhouse Client support team](https://www.greenhouse.io/contact) tooget these values.</span></span> 
 


4. <span data-ttu-id="e9673-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e9673-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_certificate.png) 

5. <span data-ttu-id="e9673-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e9673-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-greenhouse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e9673-166">tooconfigure jednotného přihlašování na **skleníkových** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory skleníkových](http://www.greenhouse.io/contact).</span><span class="sxs-lookup"><span data-stu-id="e9673-166">tooconfigure single sign-on on **Greenhouse** side, you need toosend hello downloaded **Metadata XML** too[Greenhouse support team](http://www.greenhouse.io/contact).</span></span>

> [!TIP]
> <span data-ttu-id="e9673-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="e9673-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e9673-168">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="e9673-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e9673-169">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e9673-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e9673-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e9673-170">Create an Azure AD test user</span></span>

<span data-ttu-id="e9673-171">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="e9673-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="e9673-173">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e9673-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9673-174">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e9673-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e9673-176">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e9673-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e9673-178">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e9673-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e9673-180">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e9673-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e9673-182">a.</span><span class="sxs-lookup"><span data-stu-id="e9673-182">a.</span></span> <span data-ttu-id="e9673-183">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e9673-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e9673-184">b.</span><span class="sxs-lookup"><span data-stu-id="e9673-184">b.</span></span> <span data-ttu-id="e9673-185">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e9673-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="e9673-186">c.</span><span class="sxs-lookup"><span data-stu-id="e9673-186">c.</span></span> <span data-ttu-id="e9673-187">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="e9673-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="e9673-188">d.</span><span class="sxs-lookup"><span data-stu-id="e9673-188">d.</span></span> <span data-ttu-id="e9673-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e9673-189">Click **Create**.</span></span>
 
### <a name="create-a-greenhouse-test-user"></a><span data-ttu-id="e9673-190">Vytvoření zkušebního uživatele skleníkových</span><span class="sxs-lookup"><span data-stu-id="e9673-190">Create a Greenhouse test user</span></span>

<span data-ttu-id="e9673-191">V pořadí tooenable Azure AD Uživatelé toolog do skleníkových musí být zřízená do skleníkových.</span><span class="sxs-lookup"><span data-stu-id="e9673-191">In order tooenable Azure AD users toolog into Greenhouse, they must be provisioned into Greenhouse.</span></span> <span data-ttu-id="e9673-192">V případě hello skleníkových zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="e9673-192">In hello case of Greenhouse, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="e9673-193">Můžete použít všechny ostatní skleníkových uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované skleníkových tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="e9673-193">You can use any other Greenhouse user account creation tools or APIs provided by Greenhouse tooprovision AAD user accounts.</span></span> 

<span data-ttu-id="e9673-194">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e9673-194">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9673-195">Přihlaste se tooyour **skleníkových** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="e9673-195">Log in tooyour **Greenhouse** company site as an administrator.</span></span>

2. <span data-ttu-id="e9673-196">V nabídce hello hello nahoře, klikněte na tlačítko **konfigurace**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e9673-196">In hello menu on hello top, click **Configure**, and then click **Users**.</span></span>
   
   <span data-ttu-id="e9673-197">![Uživatelé](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="e9673-197">![Users](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "Users")</span></span>

3. <span data-ttu-id="e9673-198">Klikněte na tlačítko **noví uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e9673-198">Click **New Users**.</span></span>
   
   <span data-ttu-id="e9673-199">![Nový uživatel](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="e9673-199">![New User](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "New User")</span></span>

4. <span data-ttu-id="e9673-200">V hello **přidat nové uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e9673-200">In hello **Add New User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="e9673-201">![Přidání nového uživatele](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "přidat nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="e9673-201">![Add New User](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "Add New User")</span></span>

   <span data-ttu-id="e9673-202">a.</span><span class="sxs-lookup"><span data-stu-id="e9673-202">a.</span></span> <span data-ttu-id="e9673-203">V hello **zadejte e-mailů uživatele** textovému poli, typ hello e-mailovou adresu chcete tooprovision platný účet služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e9673-203">In hello **Enter user emails** textbox, type hello email address of a valid Azure Active Directory account you want tooprovision.</span></span>

   <span data-ttu-id="e9673-204">b.</span><span class="sxs-lookup"><span data-stu-id="e9673-204">b.</span></span> <span data-ttu-id="e9673-205">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e9673-205">Click **Save**.</span></span>    
   
      >[!NOTE]
      ><span data-ttu-id="e9673-206">držiteli účtu Azure Active Directory Hello obdrží e-mailu, včetně účet odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="e9673-206">hello Azure Active Directory account holders will receive an email including a link tooconfirm hello account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e9673-207">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e9673-207">Assign hello Azure AD test user</span></span>

<span data-ttu-id="e9673-208">V této části povolíte tak, že udělíte přístup tooGreenhouse toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e9673-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGreenhouse.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="e9673-210">**tooassign Britta Simon tooGreenhouse, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e9673-210">**tooassign Britta Simon tooGreenhouse, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9673-211">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e9673-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e9673-213">V seznamu aplikace hello vyberte **skleníkových**.</span><span class="sxs-lookup"><span data-stu-id="e9673-213">In hello applications list, select **Greenhouse**.</span></span>

    ![v seznamu aplikace hello Hello skleníkových odkaz](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_app.png)  

3. <span data-ttu-id="e9673-215">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e9673-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="e9673-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e9673-217">Click **Add** button.</span></span> <span data-ttu-id="e9673-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e9673-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="e9673-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="e9673-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e9673-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e9673-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e9673-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e9673-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e9673-223">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e9673-223">Test single sign-on</span></span>

<span data-ttu-id="e9673-224">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e9673-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e9673-225">Když kliknete na dlaždici skleníkových hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour skleníkových aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9673-225">When you click hello Greenhouse tile in hello Access Panel, you should get automatically signed-on tooyour Greenhouse application.</span></span>
<span data-ttu-id="e9673-226">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e9673-226">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e9673-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e9673-227">Additional resources</span></span>

* [<span data-ttu-id="e9673-228">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e9673-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e9673-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e9673-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_203.png

