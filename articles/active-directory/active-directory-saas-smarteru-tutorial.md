---
title: 'Kurz: Azure Active Directory integrace s SmarterU | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SmarterU."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a3b81b557c47e31f09e61bcf75dd23f370e642e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a><span data-ttu-id="51847-103">Kurz: Azure Active Directory integrace s SmarterU</span><span class="sxs-lookup"><span data-stu-id="51847-103">Tutorial: Azure Active Directory integration with SmarterU</span></span>

<span data-ttu-id="51847-104">V tomto kurzu zjistíte, jak toointegrate SmarterU s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="51847-104">In this tutorial, you learn how toointegrate SmarterU with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="51847-105">Integrace SmarterU s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="51847-105">Integrating SmarterU with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="51847-106">Můžete řídit ve službě Azure AD, který má přístup tooSmarterU</span><span class="sxs-lookup"><span data-stu-id="51847-106">You can control in Azure AD who has access tooSmarterU</span></span>
- <span data-ttu-id="51847-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSmarterU (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="51847-107">You can enable your users tooautomatically get signed-on tooSmarterU (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="51847-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="51847-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="51847-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="51847-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="51847-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="51847-110">Prerequisites</span></span>

<span data-ttu-id="51847-111">Integrace služby Azure AD s SmarterU tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="51847-111">tooconfigure Azure AD integration with SmarterU, you need hello following items:</span></span>

- <span data-ttu-id="51847-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="51847-112">An Azure AD subscription</span></span>
- <span data-ttu-id="51847-113">SmarterU jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="51847-113">A SmarterU single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="51847-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="51847-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="51847-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="51847-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="51847-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="51847-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="51847-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="51847-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="51847-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="51847-118">Scenario description</span></span>
<span data-ttu-id="51847-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="51847-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="51847-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="51847-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="51847-121">Přidání SmarterU z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="51847-121">Adding SmarterU from hello gallery</span></span>
2. <span data-ttu-id="51847-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="51847-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-smarteru-from-hello-gallery"></a><span data-ttu-id="51847-123">Přidání SmarterU z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="51847-123">Adding SmarterU from hello gallery</span></span>
<span data-ttu-id="51847-124">tooconfigure hello integrace SmarterU do Azure AD, je nutné tooadd SmarterU hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="51847-124">tooconfigure hello integration of SmarterU into Azure AD, you need tooadd SmarterU from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="51847-125">**tooadd SmarterU z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="51847-125">**tooadd SmarterU from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="51847-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="51847-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="51847-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="51847-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="51847-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="51847-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="51847-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="51847-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="51847-133">Hello vyhledávacího pole zadejte **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="51847-133">In hello search box, type **SmarterU**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_search.png)

5. <span data-ttu-id="51847-135">Na panelu výsledků hello vyberte **SmarterU**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="51847-135">In hello results panel, select **SmarterU**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="51847-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="51847-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="51847-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SmarterU podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="51847-138">In this section, you configure and test Azure AD single sign-on with SmarterU based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="51847-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v SmarterU je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51847-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SmarterU is tooa user in Azure AD.</span></span> <span data-ttu-id="51847-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SmarterU musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="51847-140">In other words, a link relationship between an Azure AD user and hello related user in SmarterU needs toobe established.</span></span>

<span data-ttu-id="51847-141">V SmarterU, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="51847-141">In SmarterU, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="51847-142">tooconfigure a testu Azure AD jednotné přihlašování s SmarterU, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="51847-142">tooconfigure and test Azure AD single sign-on with SmarterU, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="51847-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="51847-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="51847-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="51847-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="51847-145">**[Vytvoření zkušebního uživatele SmarterU](#creating-a-smarteru-test-user)**  -toohave protějšek Britta Simon v SmarterU, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="51847-145">**[Creating a SmarterU test user](#creating-a-smarteru-test-user)** - toohave a counterpart of Britta Simon in SmarterU that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="51847-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51847-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="51847-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="51847-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="51847-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="51847-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="51847-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SmarterU.</span><span class="sxs-lookup"><span data-stu-id="51847-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SmarterU application.</span></span>

<span data-ttu-id="51847-150">**tooconfigure Azure AD jednotné přihlašování s SmarterU, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="51847-150">**tooconfigure Azure AD single sign-on with SmarterU, perform hello following steps:**</span></span>

1. <span data-ttu-id="51847-151">V portálu Azure, na hello hello **SmarterU** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="51847-151">In hello Azure portal, on hello **SmarterU** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="51847-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51847-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_samlbase.png)

3. <span data-ttu-id="51847-155">Na hello **SmarterU domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="51847-155">On hello **SmarterU Domain and URLs** section, perform hello following steps:</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_url.png)

    <span data-ttu-id="51847-157">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://www.smarteru.com/`</span><span class="sxs-lookup"><span data-stu-id="51847-157">In hello **Identifier** textbox, type hello URL: `https://www.smarteru.com/`</span></span>

4. <span data-ttu-id="51847-158">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="51847-158">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_certificate.png) 

5. <span data-ttu-id="51847-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="51847-160">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="51847-162">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti SmarterU tooyour.</span><span class="sxs-lookup"><span data-stu-id="51847-162">In a different web browser window, log in tooyour SmarterU company site as an administrator.</span></span>

7. <span data-ttu-id="51847-163">V panelu nástrojů hello hello nahoře, klikněte na **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="51847-163">In hello toolbar on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="51847-164">![Nastavení účtu](./media/active-directory-saas-smarteru-tutorial/IC777326.png "nastavení účtu")</span><span class="sxs-lookup"><span data-stu-id="51847-164">![Account Settings](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Account Settings")</span></span>

8. <span data-ttu-id="51847-165">Na stránce konfigurace účtu hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="51847-165">On hello account configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="51847-166">![Externí ověřování](./media/active-directory-saas-smarteru-tutorial/IC777327.png "externí ověřování")</span><span class="sxs-lookup"><span data-stu-id="51847-166">![External Authorization](./media/active-directory-saas-smarteru-tutorial/IC777327.png "External Authorization")</span></span> 
 
      <span data-ttu-id="51847-167">a.</span><span class="sxs-lookup"><span data-stu-id="51847-167">a.</span></span> <span data-ttu-id="51847-168">Vyberte **povolit externí autorizace**.</span><span class="sxs-lookup"><span data-stu-id="51847-168">Select **Enable External Authorization**.</span></span>
  
      <span data-ttu-id="51847-169">b.</span><span class="sxs-lookup"><span data-stu-id="51847-169">b.</span></span> <span data-ttu-id="51847-170">V hello **hlavní přihlášení řízení** části, vyberte hello **SmarterU** kartě.</span><span class="sxs-lookup"><span data-stu-id="51847-170">In hello **Master Login Control** section, select hello **SmarterU** tab.</span></span>
  
      <span data-ttu-id="51847-171">c.</span><span class="sxs-lookup"><span data-stu-id="51847-171">c.</span></span> <span data-ttu-id="51847-172">V hello **výchozí přihlašovací jméno** části, vyberte hello **SmarterU** kartě.</span><span class="sxs-lookup"><span data-stu-id="51847-172">In hello **User Default Login** section, select hello **SmarterU** tab.</span></span>
  
      <span data-ttu-id="51847-173">d.</span><span class="sxs-lookup"><span data-stu-id="51847-173">d.</span></span> <span data-ttu-id="51847-174">Vyberte **povolit Okta**.</span><span class="sxs-lookup"><span data-stu-id="51847-174">Select **Enable Okta**.</span></span>
  
      <span data-ttu-id="51847-175">e.</span><span class="sxs-lookup"><span data-stu-id="51847-175">e.</span></span> <span data-ttu-id="51847-176">Zkopírujte obsah hello hello stažené metadata souboru a pak ji vložit do hello **Okta Metadata** textové pole.</span><span class="sxs-lookup"><span data-stu-id="51847-176">Copy hello content of hello downloaded metadata file, and then paste it into hello **Okta Metadata** textbox.</span></span>
  
      <span data-ttu-id="51847-177">f.</span><span class="sxs-lookup"><span data-stu-id="51847-177">f.</span></span> <span data-ttu-id="51847-178">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="51847-178">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="51847-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="51847-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="51847-180">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="51847-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="51847-181">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="51847-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="51847-182">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="51847-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="51847-183">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="51847-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="51847-185">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="51847-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="51847-186">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="51847-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="51847-188">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="51847-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="51847-190">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="51847-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="51847-192">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="51847-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="51847-194">a.</span><span class="sxs-lookup"><span data-stu-id="51847-194">a.</span></span> <span data-ttu-id="51847-195">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="51847-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="51847-196">b.</span><span class="sxs-lookup"><span data-stu-id="51847-196">b.</span></span> <span data-ttu-id="51847-197">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="51847-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="51847-198">c.</span><span class="sxs-lookup"><span data-stu-id="51847-198">c.</span></span> <span data-ttu-id="51847-199">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="51847-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="51847-200">d.</span><span class="sxs-lookup"><span data-stu-id="51847-200">d.</span></span> <span data-ttu-id="51847-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="51847-201">Click **Create**.</span></span>
 
### <a name="creating-a-smarteru-test-user"></a><span data-ttu-id="51847-202">Vytvoření zkušebního uživatele SmarterU</span><span class="sxs-lookup"><span data-stu-id="51847-202">Creating a SmarterU test user</span></span>

<span data-ttu-id="51847-203">Uživatelé toolog tooenable Azure AD v tooSmarterU, se musí být zřízená do SmarterU.</span><span class="sxs-lookup"><span data-stu-id="51847-203">tooenable Azure AD users toolog in tooSmarterU, they must be provisioned into SmarterU.</span></span>

<span data-ttu-id="51847-204">Při zřizování SmarterU, je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="51847-204">When SmarterU, provisioning is a manual task.</span></span>

<span data-ttu-id="51847-205">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="51847-205">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="51847-206">Přihlaste se tooyour **SmarterU** klienta.</span><span class="sxs-lookup"><span data-stu-id="51847-206">Log in tooyour **SmarterU** tenant.</span></span>

2. <span data-ttu-id="51847-207">Přejděte příliš**uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="51847-207">Go too**Users**.</span></span>

3. <span data-ttu-id="51847-208">V části hello uživatele proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="51847-208">In hello user section, perform hello following steps:</span></span>
   
    <span data-ttu-id="51847-209">![Nový uživatel](./media/active-directory-saas-smarteru-tutorial/IC777329.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="51847-209">![New User](./media/active-directory-saas-smarteru-tutorial/IC777329.png "New User")</span></span>  

    <span data-ttu-id="51847-210">a.</span><span class="sxs-lookup"><span data-stu-id="51847-210">a.</span></span> <span data-ttu-id="51847-211">Klikněte na tlačítko **+ uživatele**.</span><span class="sxs-lookup"><span data-stu-id="51847-211">Click **+User**.</span></span>
    
    <span data-ttu-id="51847-212">b.</span><span class="sxs-lookup"><span data-stu-id="51847-212">b.</span></span> <span data-ttu-id="51847-213">Typ hello související hodnoty atributu hello Azure AD uživatelského účtu do textových polí následující hello: **primární e-mailovou**, **ID zaměstnance**, **heslo**,  **Ověřte heslo**, **křestní jméno**, **Přezdívka**.</span><span class="sxs-lookup"><span data-stu-id="51847-213">Type hello related attribute values of hello Azure AD user account into hello following textboxes: **Primary Email**, **Employee ID**, **Password**, **Verify Password**, **Given Name**, **Surname**.</span></span>
    
    <span data-ttu-id="51847-214">c.</span><span class="sxs-lookup"><span data-stu-id="51847-214">c.</span></span> <span data-ttu-id="51847-215">Klikněte na tlačítko **Active**.</span><span class="sxs-lookup"><span data-stu-id="51847-215">Click **Active**.</span></span> 
    
    <span data-ttu-id="51847-216">d.</span><span class="sxs-lookup"><span data-stu-id="51847-216">d.</span></span> <span data-ttu-id="51847-217">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="51847-217">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="51847-218">Můžete použít všechny ostatní SmarterU uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované SmarterU tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="51847-218">You can use any other SmarterU user account creation tools or APIs provided by SmarterU tooprovision AAD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="51847-219">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="51847-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="51847-220">V této části povolíte tak, že udělíte přístup tooSmarterU toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51847-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSmarterU.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="51847-222">**tooassign Britta Simon tooSmarterU, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="51847-222">**tooassign Britta Simon tooSmarterU, perform hello following steps:**</span></span>

1. <span data-ttu-id="51847-223">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="51847-223">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="51847-225">V seznamu aplikace hello vyberte **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="51847-225">In hello applications list, select **SmarterU**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_app.png) 

3. <span data-ttu-id="51847-227">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="51847-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="51847-229">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="51847-229">Click **Add** button.</span></span> <span data-ttu-id="51847-230">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51847-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="51847-232">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="51847-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="51847-233">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51847-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="51847-234">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51847-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="51847-235">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="51847-235">Testing single sign-on</span></span>

<span data-ttu-id="51847-236">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="51847-236">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="51847-237">Když kliknete na dlaždici SmarterU hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SmarterU aplikace.</span><span class="sxs-lookup"><span data-stu-id="51847-237">When you click hello SmarterU tile in hello Access Panel, you should get automatically signed-on tooyour SmarterU application.</span></span>
<span data-ttu-id="51847-238">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="51847-238">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 


## <a name="additional-resources"></a><span data-ttu-id="51847-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="51847-239">Additional resources</span></span>

* [<span data-ttu-id="51847-240">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="51847-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="51847-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="51847-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_203.png

