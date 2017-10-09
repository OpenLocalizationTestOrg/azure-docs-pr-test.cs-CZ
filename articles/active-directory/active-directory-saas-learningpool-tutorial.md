---
title: 'Kurz: Azure Active Directory integrace s Learningpool Act | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Learningpool akce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 51e8695f-31e1-4d09-8eb3-13241999d99f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: f343623f08bb60e143aaff07d93e4ef773232e07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learningpool-act"></a><span data-ttu-id="6a9a1-103">Kurz: Azure Active Directory integrace s Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="6a9a1-103">Tutorial: Azure Active Directory integration with Learningpool Act</span></span>

<span data-ttu-id="6a9a1-104">V tomto kurzu zjistíte, jak toointegrate Learningpool fungovat s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6a9a1-104">In this tutorial, you learn how toointegrate Learningpool Act with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6a9a1-105">Integrace s Azure AD Application Compatibility Toolkit Learningpool poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6a9a1-105">Integrating Learningpool Act with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6a9a1-106">Můžete řídit ve službě Azure AD, který má přístup tooLearningpool Act</span><span class="sxs-lookup"><span data-stu-id="6a9a1-106">You can control in Azure AD who has access tooLearningpool Act</span></span>
- <span data-ttu-id="6a9a1-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLearningpool Act (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a9a1-107">You can enable your users tooautomatically get signed-on tooLearningpool Act (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6a9a1-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6a9a1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6a9a1-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6a9a1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a9a1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6a9a1-110">Prerequisites</span></span>

<span data-ttu-id="6a9a1-111">tooconfigure integrace Azure AD s Learningpool Application Compatibility Toolkit, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6a9a1-111">tooconfigure Azure AD integration with Learningpool Act, you need hello following items:</span></span>

- <span data-ttu-id="6a9a1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a9a1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6a9a1-113">Learningpool Act jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6a9a1-113">A Learningpool Act single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6a9a1-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6a9a1-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6a9a1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6a9a1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6a9a1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6a9a1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6a9a1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6a9a1-118">Scenario description</span></span>
<span data-ttu-id="6a9a1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6a9a1-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6a9a1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6a9a1-121">Přidání Application Compatibility Toolkit Learningpool z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6a9a1-121">Adding Learningpool Act from hello gallery</span></span>
2. <span data-ttu-id="6a9a1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a9a1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learningpool-act-from-hello-gallery"></a><span data-ttu-id="6a9a1-123">Přidání Application Compatibility Toolkit Learningpool z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6a9a1-123">Adding Learningpool Act from hello gallery</span></span>
<span data-ttu-id="6a9a1-124">tooconfigure hello integrace Learningpool Act do Azure AD, je nutné tooadd Learningpool Act hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-124">tooconfigure hello integration of Learningpool Act into Azure AD, you need tooadd Learningpool Act from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6a9a1-125">**tooadd Learningpool Act z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a9a1-125">**tooadd Learningpool Act from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a9a1-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6a9a1-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6a9a1-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6a9a1-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6a9a1-133">Hello vyhledávacího pole zadejte **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-133">In hello search box, type **Learningpool Act**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_search.png)

5. <span data-ttu-id="6a9a1-135">Na panelu výsledků hello vyberte **Learningpool Act**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-135">In hello results panel, select **Learningpool Act**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6a9a1-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a9a1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6a9a1-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Learningpool Act podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6a9a1-138">In this section, you configure and test Azure AD single sign-on with Learningpool Act based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6a9a1-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Learningpool Act je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Learningpool Act is tooa user in Azure AD.</span></span> <span data-ttu-id="6a9a1-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Learningpool Act musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-140">In other words, a link relationship between an Azure AD user and hello related user in Learningpool Act needs toobe established.</span></span>

<span data-ttu-id="6a9a1-141">V Learningpool Act přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-141">In Learningpool Act, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6a9a1-142">tooconfigure a testu Azure AD jednotné přihlašování s Learningpool Application Compatibility Toolkit, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6a9a1-142">tooconfigure and test Azure AD single sign-on with Learningpool Act, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6a9a1-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6a9a1-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6a9a1-145">**[Vytvoření zkušebního uživatele Learningpool Act](#creating-a-learningpool-act-test-user)**  -toohave protějšek Britta Simon v Learningpool akce, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-145">**[Creating a Learningpool Act test user](#creating-a-learningpool-act-test-user)** - toohave a counterpart of Britta Simon in Learningpool Act that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6a9a1-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6a9a1-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6a9a1-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a9a1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6a9a1-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Learningpool akce.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Learningpool Act application.</span></span>

<span data-ttu-id="6a9a1-150">**tooconfigure Azure AD jednotné přihlašování s Learningpool Application Compatibility Toolkit, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a9a1-150">**tooconfigure Azure AD single sign-on with Learningpool Act, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a9a1-151">V portálu Azure, na hello hello **Learningpool Act** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-151">In hello Azure portal, on hello **Learningpool Act** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6a9a1-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_samlbase.png)

3. <span data-ttu-id="6a9a1-155">Na hello **Learningpool Act domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6a9a1-155">On hello **Learningpool Act Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_url.png)

    <span data-ttu-id="6a9a1-157">a.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-157">a.</span></span> <span data-ttu-id="6a9a1-158">V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span><span class="sxs-lookup"><span data-stu-id="6a9a1-158">In hello **Sign-on URL** textbox, type hello URL: `https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span></span>

    <span data-ttu-id="6a9a1-159">b.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-159">b.</span></span> <span data-ttu-id="6a9a1-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="6a9a1-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.Learningpool.com/shibboleth` |
    | `https://<subdomain>.preview.Learningpool.com/shibboleth` |

    > [!NOTE] 
    > <span data-ttu-id="6a9a1-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-161">These values are not real.</span></span> <span data-ttu-id="6a9a1-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6a9a1-163">Obraťte se na [tým podpory Learningpool Act klienta](https://www.Learningpool.com/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-163">Contact [Learningpool Act Client support team](https://www.Learningpool.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="6a9a1-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_certificate.png) 

5. <span data-ttu-id="6a9a1-166">Aplikace Learningpool Act očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-166">Learningpool Act application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="6a9a1-167">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-167">Please configure hello following claims for this application.</span></span> <span data-ttu-id="6a9a1-168">Můžete spravovat hello hodnoty těchto atributů z hello **"Atrribute"** karta aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-168">You can manage hello values of these attributes from hello **"Atrribute"** tab of hello application.</span></span> <span data-ttu-id="6a9a1-169">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-169">hello following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_attribute.png) 

6. <span data-ttu-id="6a9a1-171">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6a9a1-171">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="6a9a1-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="6a9a1-172">Attribute Name</span></span> | <span data-ttu-id="6a9a1-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="6a9a1-173">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="6a9a1-174">název urn: oid:1.2.840.113556.1.4.221</span><span class="sxs-lookup"><span data-stu-id="6a9a1-174">urn:oid:1.2.840.113556.1.4.221</span></span> | <span data-ttu-id="6a9a1-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="6a9a1-175">user.userprincipalname</span></span> |
    | <span data-ttu-id="6a9a1-176">název urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="6a9a1-176">urn:oid:2.5.4.42</span></span> | <span data-ttu-id="6a9a1-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="6a9a1-177">user.givenname</span></span> |
    | <span data-ttu-id="6a9a1-178">název urn: oid:0.9.2342.19200300.100.1.3</span><span class="sxs-lookup"><span data-stu-id="6a9a1-178">urn:oid:0.9.2342.19200300.100.1.3</span></span> | <span data-ttu-id="6a9a1-179">User.Mail</span><span class="sxs-lookup"><span data-stu-id="6a9a1-179">user.mail</span></span> |    
    | <span data-ttu-id="6a9a1-180">název urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="6a9a1-180">urn:oid:2.5.4.4</span></span> | <span data-ttu-id="6a9a1-181">User.Surname</span><span class="sxs-lookup"><span data-stu-id="6a9a1-181">user.surname</span></span> |
    
    <span data-ttu-id="6a9a1-182">a.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-182">a.</span></span> <span data-ttu-id="6a9a1-183">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-183">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="6a9a1-186">b.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-186">b.</span></span> <span data-ttu-id="6a9a1-187">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-187">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="6a9a1-188">c.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-188">c.</span></span> <span data-ttu-id="6a9a1-189">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-189">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="6a9a1-190">d.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-190">d.</span></span> <span data-ttu-id="6a9a1-191">Nechte hello **Namespace** prázdné.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-191">Leave hello **Namespace** blank.</span></span>
    
    <span data-ttu-id="6a9a1-192">e.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-192">e.</span></span> <span data-ttu-id="6a9a1-193">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-193">Click **Ok**.</span></span>

7. <span data-ttu-id="6a9a1-194">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-194">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="6a9a1-196">tooconfigure jednotného přihlašování na **Learningpool Act** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Learningpool Act](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="6a9a1-196">tooconfigure single sign-on on **Learningpool Act** side, you need toosend hello downloaded **Metadata XML** too[Learningpool Act support team](https://www.Learningpool.com/support).</span></span> <span data-ttu-id="6a9a1-197">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-197">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="6a9a1-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6a9a1-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6a9a1-199">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6a9a1-200">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6a9a1-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6a9a1-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a9a1-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="6a9a1-202">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6a9a1-204">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a9a1-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a9a1-205">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6a9a1-207">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6a9a1-209">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6a9a1-211">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6a9a1-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6a9a1-213">a.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-213">a.</span></span> <span data-ttu-id="6a9a1-214">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6a9a1-215">b.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-215">b.</span></span> <span data-ttu-id="6a9a1-216">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6a9a1-217">c.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-217">c.</span></span> <span data-ttu-id="6a9a1-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6a9a1-219">d.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-219">d.</span></span> <span data-ttu-id="6a9a1-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-220">Click **Create**.</span></span>
 
### <a name="creating-a-learningpool-act-test-user"></a><span data-ttu-id="6a9a1-221">Vytvoření zkušebního uživatele Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="6a9a1-221">Creating a Learningpool Act test user</span></span>

<span data-ttu-id="6a9a1-222">Uživatelé toolog tooenable Azure AD v tooLearningpool Application Compatibility Toolkit, se musí být zřízená do Learningpool akce.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-222">tooenable Azure AD users toolog in tooLearningpool Act, they must be provisioned into Learningpool Act.</span></span>

<span data-ttu-id="6a9a1-223">Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooLearningpool Application Compatibility Toolkit.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-223">There is no action item for you tooconfigure user provisioning tooLearningpool Act.</span></span>  
<span data-ttu-id="6a9a1-224">Uživatelé potřebují toobe vytvořené vaše [tým podpory Learningpool Act](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="6a9a1-224">Users need toobe created by your [Learningpool Act support team](https://www.Learningpool.com/support).</span></span>

>[!NOTE]
><span data-ttu-id="6a9a1-225">Můžete použít všechny ostatní Learningpool Act uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Learningpool Act tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-225">You can use any other Learningpool Act user account creation tools or APIs provided by Learningpool Act tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6a9a1-226">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6a9a1-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6a9a1-227">V této části povolíte tak, že udělíte přístup tooLearningpool Act Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLearningpool Act.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6a9a1-229">**tooassign tooLearningpool Britta Simon Application Compatibility Toolkit, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a9a1-229">**tooassign Britta Simon tooLearningpool Act, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a9a1-230">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6a9a1-232">V seznamu aplikace hello vyberte **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-232">In hello applications list, select **Learningpool Act**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_app.png) 

3. <span data-ttu-id="6a9a1-234">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6a9a1-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-236">Click **Add** button.</span></span> <span data-ttu-id="6a9a1-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6a9a1-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6a9a1-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6a9a1-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6a9a1-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a9a1-242">Testing single sign-on</span></span>

<span data-ttu-id="6a9a1-243">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6a9a1-244">Po kliknutí na tlačítko hello Learningpool Act dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Learningpool Act aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a9a1-244">When you click hello Learningpool Act tile in hello Access Panel, you should get automatically signed-on tooyour Learningpool Act application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a9a1-245">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6a9a1-245">Additional resources</span></span>

* [<span data-ttu-id="6a9a1-246">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6a9a1-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6a9a1-247">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6a9a1-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_203.png

