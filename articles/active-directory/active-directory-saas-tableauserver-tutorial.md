---
title: 'Kurz: Azure Active Directory integrace se serverem Tableau | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Tableau serveru."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: feb2087bd6ae6ddcb920901e6719688fc95ae287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a><span data-ttu-id="810b9-103">Kurz: Azure Active Directory integrace se serverem Tableau</span><span class="sxs-lookup"><span data-stu-id="810b9-103">Tutorial: Azure Active Directory integration with Tableau Server</span></span>

<span data-ttu-id="810b9-104">V tomto kurzu zjistíte, jak toointegrate Tableau serveru se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="810b9-104">In this tutorial, you learn how toointegrate Tableau Server with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="810b9-105">Integrace serveru Tableau s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="810b9-105">Integrating Tableau Server with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="810b9-106">Můžete řídit ve službě Azure AD, který má přístup tooTableau serveru</span><span class="sxs-lookup"><span data-stu-id="810b9-106">You can control in Azure AD who has access tooTableau Server</span></span>
- <span data-ttu-id="810b9-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTableau serveru (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="810b9-107">You can enable your users tooautomatically get signed-on tooTableau Server (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="810b9-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="810b9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="810b9-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="810b9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="810b9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="810b9-110">Prerequisites</span></span>

<span data-ttu-id="810b9-111">tooconfigure integrace Azure AD s Tableau serveru, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="810b9-111">tooconfigure Azure AD integration with Tableau Server, you need hello following items:</span></span>

- <span data-ttu-id="810b9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="810b9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="810b9-113">Serveru Tableau jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="810b9-113">A Tableau Server single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="810b9-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="810b9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="810b9-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="810b9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="810b9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="810b9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="810b9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="810b9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="810b9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="810b9-118">Scenario description</span></span>
<span data-ttu-id="810b9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="810b9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="810b9-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="810b9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="810b9-121">Přidání serveru Tableau z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="810b9-121">Adding Tableau Server from hello gallery</span></span>
2. <span data-ttu-id="810b9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="810b9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-server-from-hello-gallery"></a><span data-ttu-id="810b9-123">Přidání serveru Tableau z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="810b9-123">Adding Tableau Server from hello gallery</span></span>
<span data-ttu-id="810b9-124">tooconfigure hello integrace Tableau serveru do Azure AD, je nutné tooadd Tableau Server hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="810b9-124">tooconfigure hello integration of Tableau Server into Azure AD, you need tooadd Tableau Server from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="810b9-125">**tooadd Tableau serveru z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="810b9-125">**tooadd Tableau Server from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="810b9-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="810b9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="810b9-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="810b9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="810b9-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="810b9-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="810b9-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="810b9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="810b9-133">Hello vyhledávacího pole zadejte **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="810b9-133">In hello search box, type **Tableau Server**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. <span data-ttu-id="810b9-135">Na panelu výsledků hello vyberte **Tableau Server**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="810b9-135">In hello results panel, select **Tableau Server**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="810b9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="810b9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="810b9-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tableau serveru podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="810b9-138">In this section, you configure and test Azure AD single sign-on with Tableau Server based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="810b9-139">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello Tableau serveru je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="810b9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tableau Server is tooa user in Azure AD.</span></span> <span data-ttu-id="810b9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Tableau serveru musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="810b9-140">In other words, a link relationship between an Azure AD user and hello related user in Tableau Server needs toobe established.</span></span>

<span data-ttu-id="810b9-141">V serveru Tableau přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="810b9-141">In Tableau Server, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="810b9-142">tooconfigure a testu Azure AD jednotné přihlašování s Tableau serveru, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="810b9-142">tooconfigure and test Azure AD single sign-on with Tableau Server, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="810b9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="810b9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="810b9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="810b9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="810b9-145">**[Vytvoření zkušebního uživatele serveru Tableau](#creating-a-tableau-server-test-user)**  -toohave protějšek Britta Simon Tableau serveru, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="810b9-145">**[Creating a Tableau Server test user](#creating-a-tableau-server-test-user)** - toohave a counterpart of Britta Simon in Tableau Server that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="810b9-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="810b9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="810b9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="810b9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="810b9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="810b9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="810b9-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Tableau serveru.</span><span class="sxs-lookup"><span data-stu-id="810b9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tableau Server application.</span></span>

<span data-ttu-id="810b9-150">**tooconfigure Azure AD jednotné přihlašování se serverem Tableau, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="810b9-150">**tooconfigure Azure AD single sign-on with Tableau Server, perform hello following steps:**</span></span>

1. <span data-ttu-id="810b9-151">V portálu Azure, na hello hello **Tableau Server** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="810b9-151">In hello Azure portal, on hello **Tableau Server** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="810b9-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="810b9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. <span data-ttu-id="810b9-155">Na hello **Tableau Server domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="810b9-155">On hello **Tableau Server Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    <span data-ttu-id="810b9-157">a.</span><span class="sxs-lookup"><span data-stu-id="810b9-157">a.</span></span> <span data-ttu-id="810b9-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="810b9-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://azure.<domain name>.link`</span></span>
    
    <span data-ttu-id="810b9-159">b.</span><span class="sxs-lookup"><span data-stu-id="810b9-159">b.</span></span> <span data-ttu-id="810b9-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="810b9-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://azure.<domain name>.link`</span></span>

    <span data-ttu-id="810b9-161">c.</span><span class="sxs-lookup"><span data-stu-id="810b9-161">c.</span></span> <span data-ttu-id="810b9-162">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://azure.<domain name>.link/wg/saml/SSO/index.html`</span><span class="sxs-lookup"><span data-stu-id="810b9-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://azure.<domain name>.link/wg/saml/SSO/index.html`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="810b9-163">Hello předchozí hodnoty nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="810b9-163">hello preceding values are not real values.</span></span> <span data-ttu-id="810b9-164">Později aktualizujte hello hodnoty s hello skutečná adresa URL a identifikátor ze stránky konfigurace Tableau Server hello.</span><span class="sxs-lookup"><span data-stu-id="810b9-164">Later, you update hello values with hello actual URL and identifier from hello Tableau Server configuration page.</span></span> 

4. <span data-ttu-id="810b9-165">Aplikace serveru tableau očekává, že hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="810b9-165">Tableau Server application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="810b9-166">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="810b9-166">Configure hello following claims for this application.</span></span> <span data-ttu-id="810b9-167">Můžete spravovat hello hodnoty těchto atributů z hello **"Atributy uživatele"** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="810b9-167">You can manage hello values of these attributes from hello **"User Attributes"** section on application integration page.</span></span> <span data-ttu-id="810b9-168">Hello následující snímek obrazovky ukazuje příklad pro hello stejné.</span><span class="sxs-lookup"><span data-stu-id="810b9-168">hello following screenshot shows an example for hello same.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. <span data-ttu-id="810b9-170">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="810b9-170">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="810b9-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="810b9-171">Attribute Name</span></span> | <span data-ttu-id="810b9-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="810b9-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="810b9-173">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="810b9-173">username</span></span> | <span data-ttu-id="810b9-174">*User.DisplayName*</span><span class="sxs-lookup"><span data-stu-id="810b9-174">*user.displayname*</span></span> |

    <span data-ttu-id="810b9-175">a.</span><span class="sxs-lookup"><span data-stu-id="810b9-175">a.</span></span> <span data-ttu-id="810b9-176">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="810b9-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    <span data-ttu-id="810b9-179">b.</span><span class="sxs-lookup"><span data-stu-id="810b9-179">b.</span></span> <span data-ttu-id="810b9-180">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="810b9-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="810b9-181">c.</span><span class="sxs-lookup"><span data-stu-id="810b9-181">c.</span></span> <span data-ttu-id="810b9-182">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="810b9-182">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="810b9-183">d.</span><span class="sxs-lookup"><span data-stu-id="810b9-183">d.</span></span> <span data-ttu-id="810b9-184">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="810b9-184">Click **Ok**</span></span>


6. <span data-ttu-id="810b9-185">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="810b9-185">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. <span data-ttu-id="810b9-187">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="810b9-187">Click **Save** button.</span></span>

    <span data-ttu-id="810b9-188">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="810b9-188">![Configure Single Sign-On](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span></span>
8. <span data-ttu-id="810b9-189">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte klienta toosign na tooyour Tableau serveru jako správce.</span><span class="sxs-lookup"><span data-stu-id="810b9-189">tooget SSO configured for your application, you need toosign-on tooyour Tableau Server tenant as an administrator.</span></span>
   
   <span data-ttu-id="810b9-190">a.</span><span class="sxs-lookup"><span data-stu-id="810b9-190">a.</span></span> <span data-ttu-id="810b9-191">V konfiguraci serveru Tableau hello, klikněte na tlačítko hello **SAML** kartě.</span><span class="sxs-lookup"><span data-stu-id="810b9-191">In hello Tableau Server configuration, click hello **SAML** tab.</span></span>
  
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   <span data-ttu-id="810b9-193">b.</span><span class="sxs-lookup"><span data-stu-id="810b9-193">b.</span></span> <span data-ttu-id="810b9-194">Zaškrtněte políčko hello z **pomocí SAML pro jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="810b9-194">Select hello checkbox of **Use SAML for single sign-on**.</span></span>
   
   <span data-ttu-id="810b9-195">c.</span><span class="sxs-lookup"><span data-stu-id="810b9-195">c.</span></span> <span data-ttu-id="810b9-196">Tableau Server návratovou adresu URL – adresy URL, která uživatelé Tableau serveru budou například http://tableau_server hello.</span><span class="sxs-lookup"><span data-stu-id="810b9-196">Tableau Server return URL—hello URL that Tableau Server users will be accessing, such as http://tableau_server.</span></span> <span data-ttu-id="810b9-197">Se nedoporučuje používat http://localhost.</span><span class="sxs-lookup"><span data-stu-id="810b9-197">Using http://localhost is not recommended.</span></span> <span data-ttu-id="810b9-198">Použijete adresu URL s koncové lomítko (například http://tableau_server/) není podporována.</span><span class="sxs-lookup"><span data-stu-id="810b9-198">Using a URL with a trailing slash (for example, http://tableau_server/) is not supported.</span></span> <span data-ttu-id="810b9-199">Kopírování **Tableau Server návratovou adresu URL** a vložte ji tooAzure AD **přihlašovací adresa URL** textového pole v **Tableau Server domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="810b9-199">Copy **Tableau Server return URL** and paste it tooAzure AD **Sign On URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="810b9-200">d.</span><span class="sxs-lookup"><span data-stu-id="810b9-200">d.</span></span> <span data-ttu-id="810b9-201">SAML entity ID – hello entity ID jednoznačně identifikuje vaši toohello instalace serveru Tableau IdP.</span><span class="sxs-lookup"><span data-stu-id="810b9-201">SAML entity ID—hello entity ID uniquely identifies your Tableau Server installation toohello IdP.</span></span> <span data-ttu-id="810b9-202">Můžete zadat vaše adresa URL serveru Tableau znovu sem, pokud chcete, ale nemá toobe vaše adresa URL serveru Tableau.</span><span class="sxs-lookup"><span data-stu-id="810b9-202">You can enter your Tableau Server URL again here, if you like, but it does not have toobe your Tableau Server URL.</span></span> <span data-ttu-id="810b9-203">Kopírování **SAML entity ID** a vložte ji tooAzure AD **identifikátor** textového pole v **Tableau Server domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="810b9-203">Copy **SAML entity ID** and paste it tooAzure AD **Identifier** textbox in **Tableau Server Domain and URLs** section.</span></span>
     
   <span data-ttu-id="810b9-204">e.</span><span class="sxs-lookup"><span data-stu-id="810b9-204">e.</span></span> <span data-ttu-id="810b9-205">Klikněte na tlačítko hello **exportovat soubor metadat** a otevře ji v hello textový editor aplikace.</span><span class="sxs-lookup"><span data-stu-id="810b9-205">Click hello **Export Metadata File** and open it in hello text editor application.</span></span> <span data-ttu-id="810b9-206">Vyhledejte adresu URL služby příjemce Assertion pomocí Http Post a Index 0 a adresy URL hello kopírování.</span><span class="sxs-lookup"><span data-stu-id="810b9-206">Locate Assertion Consumer Service URL with Http Post and Index 0 and copy hello URL.</span></span> <span data-ttu-id="810b9-207">Nyní vložte jej tooAzure AD **adresa URL odpovědi** textového pole v **Tableau Server domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="810b9-207">Now paste it tooAzure AD **Reply URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="810b9-208">f.</span><span class="sxs-lookup"><span data-stu-id="810b9-208">f.</span></span> <span data-ttu-id="810b9-209">Vyhledejte soubor federačních metadat stáhli z portálu Azure a nahrajte ho v hello **soubor metadat SAML Idp**.</span><span class="sxs-lookup"><span data-stu-id="810b9-209">Locate your Federation Metadata file downloaded from Azure portal, and then upload it in hello **SAML Idp metadata file**.</span></span>
   
   <span data-ttu-id="810b9-210">g.</span><span class="sxs-lookup"><span data-stu-id="810b9-210">g.</span></span> <span data-ttu-id="810b9-211">Klikněte na tlačítko hello **OK** tlačítka na stránku konfigurace serveru Tableau hello.</span><span class="sxs-lookup"><span data-stu-id="810b9-211">Click hello **OK** button in hello Tableau Server Configuration page.</span></span>
   
    >[!NOTE] 
    ><span data-ttu-id="810b9-212">Zákazník mít tooupload některý z certifikátů v konfiguraci jednotné přihlašování SAML Tableau Server hello a budou získat ignorovat v hello toku jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="810b9-212">Customer have tooupload any certificate in hello Tableau Server SAML SSO configuration and it will get ignored in hello SSO flow.</span></span>
    ><span data-ttu-id="810b9-213">Pokud jste třeba pomoci konfigurace SAML na serveru Tableau pak naleznete v článku toothis [konfigurace SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span><span class="sxs-lookup"><span data-stu-id="810b9-213">If you need help configuring SAML on Tableau Server then please refer toothis article [Configure SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span></span>
    >
<CE>

> [!TIP]
> <span data-ttu-id="810b9-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="810b9-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="810b9-215">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="810b9-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="810b9-216">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="810b9-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="810b9-217">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="810b9-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="810b9-218">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="810b9-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="810b9-220">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="810b9-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="810b9-221">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="810b9-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="810b9-223">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="810b9-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="810b9-225">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="810b9-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="810b9-227">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="810b9-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="810b9-229">a.</span><span class="sxs-lookup"><span data-stu-id="810b9-229">a.</span></span> <span data-ttu-id="810b9-230">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="810b9-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="810b9-231">b.</span><span class="sxs-lookup"><span data-stu-id="810b9-231">b.</span></span> <span data-ttu-id="810b9-232">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="810b9-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="810b9-233">c.</span><span class="sxs-lookup"><span data-stu-id="810b9-233">c.</span></span> <span data-ttu-id="810b9-234">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="810b9-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="810b9-235">d.</span><span class="sxs-lookup"><span data-stu-id="810b9-235">d.</span></span> <span data-ttu-id="810b9-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="810b9-236">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-server-test-user"></a><span data-ttu-id="810b9-237">Vytvoření zkušebního uživatele Tableau serveru</span><span class="sxs-lookup"><span data-stu-id="810b9-237">Creating a Tableau Server test user</span></span>

<span data-ttu-id="810b9-238">Hello cílem této části je toocreate uživatele volat Britta Simon Tableau serveru.</span><span class="sxs-lookup"><span data-stu-id="810b9-238">hello objective of this section is toocreate a user called Britta Simon in Tableau Server.</span></span> <span data-ttu-id="810b9-239">Je nutné tooprovision všichni uživatelé hello hello Tableau serveru.</span><span class="sxs-lookup"><span data-stu-id="810b9-239">You need tooprovision all hello users in hello Tableau server.</span></span> 

<span data-ttu-id="810b9-240">Shodovat s tímto uživatelským jménem uživatele hello hello hodnotu, která jste nakonfigurovali ve vlastní atribut hello Azure AD **uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="810b9-240">That username of hello user should match hello value which you have configured in hello Azure AD custom attribute of **username**.</span></span> <span data-ttu-id="810b9-241">S hello správné mapování hello integrace by měla fungovat [konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="810b9-241">With hello correct mapping hello integration should work [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="810b9-242">Pokud potřebujete toocreate uživatel ručně, musíte toocontact hello Tableau serveru správce ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="810b9-242">If you need toocreate a user manually, you need toocontact hello Tableau Server administrator in your organization.</span></span>
> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="810b9-243">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="810b9-243">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="810b9-244">V této části povolíte tak, že udělíte přístup tooTableau Server toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="810b9-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTableau Server.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="810b9-246">**tooassign Britta Simon tooTableau serveru, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="810b9-246">**tooassign Britta Simon tooTableau Server, perform hello following steps:**</span></span>

1. <span data-ttu-id="810b9-247">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="810b9-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="810b9-249">V seznamu aplikace hello vyberte **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="810b9-249">In hello applications list, select **Tableau Server**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. <span data-ttu-id="810b9-251">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="810b9-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="810b9-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="810b9-253">Click **Add** button.</span></span> <span data-ttu-id="810b9-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="810b9-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="810b9-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="810b9-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="810b9-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="810b9-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="810b9-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="810b9-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="810b9-259">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="810b9-259">Testing single sign-on</span></span>

<span data-ttu-id="810b9-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="810b9-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="810b9-261">Když kliknete na dlaždici Tableau Server hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Tableau serverová aplikace.</span><span class="sxs-lookup"><span data-stu-id="810b9-261">When you click hello Tableau Server tile in hello Access Panel, you should get automatically signed-on tooyour Tableau Server application.</span></span>
<span data-ttu-id="810b9-262">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="810b9-262">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="810b9-263">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="810b9-263">Additional resources</span></span>

* [<span data-ttu-id="810b9-264">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="810b9-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="810b9-265">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="810b9-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

