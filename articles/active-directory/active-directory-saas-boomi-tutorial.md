---
title: 'Kurz: Azure Active Directory integrace s Boomi | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Boomi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ce64a4561697d311a8c7b1b244315bb552c5cfb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a><span data-ttu-id="6af11-103">Kurz: Azure Active Directory integrace s Boomi</span><span class="sxs-lookup"><span data-stu-id="6af11-103">Tutorial: Azure Active Directory integration with Boomi</span></span>

<span data-ttu-id="6af11-104">V tomto kurzu zjistíte, jak toointegrate Boomi s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6af11-104">In this tutorial, you learn how toointegrate Boomi with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6af11-105">Integrace Boomi s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6af11-105">Integrating Boomi with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6af11-106">Můžete řídit ve službě Azure AD, který má přístup tooBoomi</span><span class="sxs-lookup"><span data-stu-id="6af11-106">You can control in Azure AD who has access tooBoomi</span></span>
- <span data-ttu-id="6af11-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBoomi (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6af11-107">You can enable your users tooautomatically get signed-on tooBoomi (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6af11-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6af11-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6af11-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6af11-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6af11-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6af11-110">Prerequisites</span></span>

<span data-ttu-id="6af11-111">Integrace služby Azure AD s Boomi tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6af11-111">tooconfigure Azure AD integration with Boomi, you need hello following items:</span></span>

- <span data-ttu-id="6af11-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6af11-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6af11-113">Boomi jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6af11-113">A Boomi single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6af11-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6af11-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6af11-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6af11-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6af11-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6af11-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6af11-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6af11-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6af11-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6af11-118">Scenario description</span></span>
<span data-ttu-id="6af11-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6af11-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6af11-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6af11-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6af11-121">Přidání Boomi z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6af11-121">Adding Boomi from hello gallery</span></span>
2. <span data-ttu-id="6af11-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6af11-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-boomi-from-hello-gallery"></a><span data-ttu-id="6af11-123">Přidání Boomi z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6af11-123">Adding Boomi from hello gallery</span></span>
<span data-ttu-id="6af11-124">tooconfigure hello integrace Boomi do Azure AD, je nutné tooadd Boomi hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6af11-124">tooconfigure hello integration of Boomi into Azure AD, you need tooadd Boomi from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6af11-125">**tooadd Boomi z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6af11-125">**tooadd Boomi from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6af11-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6af11-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6af11-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6af11-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6af11-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6af11-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6af11-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6af11-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6af11-133">Hello vyhledávacího pole zadejte **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="6af11-133">In hello search box, type **Boomi**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. <span data-ttu-id="6af11-135">Na panelu výsledků hello vyberte **Boomi**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6af11-135">In hello results panel, select **Boomi**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6af11-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6af11-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6af11-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Boomi podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6af11-138">In this section, you configure and test Azure AD single sign-on with Boomi based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6af11-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Boomi je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6af11-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Boomi is tooa user in Azure AD.</span></span> <span data-ttu-id="6af11-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Boomi musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6af11-140">In other words, a link relationship between an Azure AD user and hello related user in Boomi needs toobe established.</span></span>

<span data-ttu-id="6af11-141">V Boomi, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6af11-141">In Boomi, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6af11-142">tooconfigure a testu Azure AD jednotné přihlašování s Boomi, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6af11-142">tooconfigure and test Azure AD single sign-on with Boomi, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6af11-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6af11-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6af11-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6af11-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6af11-145">**[Vytvoření zkušebního uživatele Boomi](#creating-a-boomi-test-user)**  -toohave protějšek Britta Simon v Boomi, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6af11-145">**[Creating a Boomi test user](#creating-a-boomi-test-user)** - toohave a counterpart of Britta Simon in Boomi that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6af11-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6af11-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6af11-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6af11-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6af11-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6af11-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6af11-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Boomi.</span><span class="sxs-lookup"><span data-stu-id="6af11-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Boomi application.</span></span>

<span data-ttu-id="6af11-150">**tooconfigure Azure AD jednotné přihlašování s Boomi, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6af11-150">**tooconfigure Azure AD single sign-on with Boomi, perform hello following steps:**</span></span>

1. <span data-ttu-id="6af11-151">V portálu Azure, na hello hello **Boomi** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6af11-151">In hello Azure portal, on hello **Boomi** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6af11-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6af11-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. <span data-ttu-id="6af11-155">Na hello **Boomi domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6af11-155">On hello **Boomi Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    <span data-ttu-id="6af11-157">a.</span><span class="sxs-lookup"><span data-stu-id="6af11-157">a.</span></span> <span data-ttu-id="6af11-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="6af11-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    <span data-ttu-id="6af11-159">b.</span><span class="sxs-lookup"><span data-stu-id="6af11-159">b.</span></span> <span data-ttu-id="6af11-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="6af11-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6af11-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6af11-161">These values are not real.</span></span> <span data-ttu-id="6af11-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6af11-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="6af11-163">Obraťte se na [tým podpory Boomi](https://boomi.com/company/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6af11-163">Contact [Boomi support team](https://boomi.com/company/contact/) tooget these values.</span></span>

4. <span data-ttu-id="6af11-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6af11-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. <span data-ttu-id="6af11-166">Aplikace Boomi očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="6af11-166">Boomi application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="6af11-167">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6af11-167">Please configure hello following claims for this application.</span></span> <span data-ttu-id="6af11-168">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="6af11-168">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="6af11-169">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="6af11-169">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="6af11-171">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogu pro každý řádek v tabulce hello níže, proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6af11-171">In hello **User Attributes** section on hello **Single sign-on** dialog, for each row shown in hello table below, perform hello following steps:</span></span>

    | <span data-ttu-id="6af11-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="6af11-172">Attribute Name</span></span> | <span data-ttu-id="6af11-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="6af11-173">Attribute Value</span></span> |
    | -------------- | --------------- |
    | <span data-ttu-id="6af11-174">FEDERATION_ID</span><span class="sxs-lookup"><span data-stu-id="6af11-174">FEDERATION_ID</span></span> | <span data-ttu-id="6af11-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="6af11-175">user.mail</span></span> |
    
    <span data-ttu-id="6af11-176">a.</span><span class="sxs-lookup"><span data-stu-id="6af11-176">a.</span></span> <span data-ttu-id="6af11-177">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6af11-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="6af11-180">b.</span><span class="sxs-lookup"><span data-stu-id="6af11-180">b.</span></span> <span data-ttu-id="6af11-181">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6af11-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="6af11-182">c.</span><span class="sxs-lookup"><span data-stu-id="6af11-182">c.</span></span> <span data-ttu-id="6af11-183">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6af11-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="6af11-184">d.</span><span class="sxs-lookup"><span data-stu-id="6af11-184">d.</span></span> <span data-ttu-id="6af11-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6af11-185">Click **Ok**.</span></span>

6. <span data-ttu-id="6af11-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6af11-186">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="6af11-188">Na hello **Boomi konfigurace** klikněte na tlačítko **konfigurace Boomi** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6af11-188">On hello **Boomi Configuration** section, click **Configure Boomi** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6af11-189">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6af11-189">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. <span data-ttu-id="6af11-191">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Boomi.</span><span class="sxs-lookup"><span data-stu-id="6af11-191">In a different web browser window, log into your Boomi company site as an administrator.</span></span> 

9. <span data-ttu-id="6af11-192">Přejděte příliš**název společnosti** a přejděte příliš**nastavit**.</span><span class="sxs-lookup"><span data-stu-id="6af11-192">Navigate too**Company Name** and go too**Set up**.</span></span>

10. <span data-ttu-id="6af11-193">Klikněte na tlačítko hello **možnosti jednotného přihlašování k** kartě a proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="6af11-193">Click hello **SSO Options** tab and perform below steps.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    <span data-ttu-id="6af11-195">a.</span><span class="sxs-lookup"><span data-stu-id="6af11-195">a.</span></span> <span data-ttu-id="6af11-196">Zkontrolujte **povolit SAML jednotné přihlašování** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="6af11-196">Check **Enable SAML Single Sign-On** checkbox.</span></span>

    <span data-ttu-id="6af11-197">b.</span><span class="sxs-lookup"><span data-stu-id="6af11-197">b.</span></span> <span data-ttu-id="6af11-198">Klikněte na tlačítko **Import** tooupload hello stáhnout certifikát z Azure AD příliš**certifikát zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="6af11-198">Click **Import** tooupload hello downloaded certificate from Azure AD too**Identity Provider Certificate**.</span></span>
    
    <span data-ttu-id="6af11-199">c.</span><span class="sxs-lookup"><span data-stu-id="6af11-199">c.</span></span> <span data-ttu-id="6af11-200">V hello **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu hello z **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6af11-200">In hello **Identity Provider Login URL** textbox, put hello value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="6af11-201">d.</span><span class="sxs-lookup"><span data-stu-id="6af11-201">d.</span></span> <span data-ttu-id="6af11-202">Jako **Federation Id umístění**, vyberte **federace je v elementu FEDERATION_ID atribut** přepínač.</span><span class="sxs-lookup"><span data-stu-id="6af11-202">As **Federation Id Location**, select **Federation Id is in FEDERATION_ID Attribute element** radio button.</span></span> 

    <span data-ttu-id="6af11-203">e.</span><span class="sxs-lookup"><span data-stu-id="6af11-203">e.</span></span> <span data-ttu-id="6af11-204">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6af11-204">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="6af11-205">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6af11-205">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6af11-206">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6af11-206">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6af11-207">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6af11-207">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6af11-208">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6af11-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="6af11-209">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6af11-209">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6af11-211">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6af11-211">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6af11-212">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6af11-212">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6af11-214">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6af11-214">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6af11-216">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="6af11-216">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6af11-218">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6af11-218">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6af11-220">a.</span><span class="sxs-lookup"><span data-stu-id="6af11-220">a.</span></span> <span data-ttu-id="6af11-221">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6af11-221">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6af11-222">b.</span><span class="sxs-lookup"><span data-stu-id="6af11-222">b.</span></span> <span data-ttu-id="6af11-223">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6af11-223">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6af11-224">c.</span><span class="sxs-lookup"><span data-stu-id="6af11-224">c.</span></span> <span data-ttu-id="6af11-225">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6af11-225">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6af11-226">d.</span><span class="sxs-lookup"><span data-stu-id="6af11-226">d.</span></span> <span data-ttu-id="6af11-227">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6af11-227">Click **Create**.</span></span>
 
### <a name="creating-a-boomi-test-user"></a><span data-ttu-id="6af11-228">Vytvoření zkušebního uživatele Boomi</span><span class="sxs-lookup"><span data-stu-id="6af11-228">Creating a Boomi test user</span></span>

<span data-ttu-id="6af11-229">V pořadí tooenable Azure AD Uživatelé toolog v tooBoomi musí být zřízená do Boomi.</span><span class="sxs-lookup"><span data-stu-id="6af11-229">In order tooenable Azure AD users toolog in tooBoomi, they must be provisioned into Boomi.</span></span> <span data-ttu-id="6af11-230">V případě hello Boomi zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="6af11-230">In hello case of Boomi, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="6af11-231">tooprovision uživatelský účet, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6af11-231">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="6af11-232">Přihlaste se tooyour Boomi společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="6af11-232">Log in tooyour Boomi company site as an administrator.</span></span>

2. <span data-ttu-id="6af11-233">Po přihlášení přejděte příliš**Správa uživatelů** a přejděte příliš**uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6af11-233">After logging in, navigate too**User Management** and go too**Users**.</span></span>

    <span data-ttu-id="6af11-234">![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="6af11-234">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "Users")</span></span>

3. <span data-ttu-id="6af11-235">Klikněte na tlačítko  **+**  ikonu a hello **role uživatele přidat/zachování** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6af11-235">Click **+**  icon and hello **Add/Maintain User Roles** dialog opens.</span></span>

    <span data-ttu-id="6af11-236">![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="6af11-236">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "Users")</span></span>

    <span data-ttu-id="6af11-237">![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="6af11-237">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "Users")</span></span>

    <span data-ttu-id="6af11-238">a.</span><span class="sxs-lookup"><span data-stu-id="6af11-238">a.</span></span> <span data-ttu-id="6af11-239">V hello **e-mailová adresa uživatele** jako typ hello e-mailu uživatele k textovému poli, BrittaSimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="6af11-239">In hello **User e-mail address** textbox, type hello email of user like BrittaSimon@contoso.com.</span></span>
    
    <span data-ttu-id="6af11-240">b.</span><span class="sxs-lookup"><span data-stu-id="6af11-240">b.</span></span> <span data-ttu-id="6af11-241">V hello **křestní jméno** textovému poli, typ hello křestní jméno uživatele jako Britta.</span><span class="sxs-lookup"><span data-stu-id="6af11-241">In hello **First name** textbox, type hello First name of user like Britta.</span></span>

    <span data-ttu-id="6af11-242">c.</span><span class="sxs-lookup"><span data-stu-id="6af11-242">c.</span></span> <span data-ttu-id="6af11-243">V hello **příjmení** textovému poli, typ hello příjmení uživatele jako Simon.</span><span class="sxs-lookup"><span data-stu-id="6af11-243">In hello **Last name** textbox, type hello Last name of user like Simon.</span></span>
    
    <span data-ttu-id="6af11-244">d.</span><span class="sxs-lookup"><span data-stu-id="6af11-244">d.</span></span> <span data-ttu-id="6af11-245">Zadejte uživatele hello **federace**.</span><span class="sxs-lookup"><span data-stu-id="6af11-245">Enter hello user's **Federation ID**.</span></span> <span data-ttu-id="6af11-246">Každý uživatel musí mít ID federace, která jednoznačně identifikuje uživatele hello v rámci účtu hello.</span><span class="sxs-lookup"><span data-stu-id="6af11-246">Each user must have a Federation ID that uniquely identifies hello user within hello account.</span></span>
    
    <span data-ttu-id="6af11-247">e.</span><span class="sxs-lookup"><span data-stu-id="6af11-247">e.</span></span> <span data-ttu-id="6af11-248">Přiřadit hello **standardní uživatel** toohello uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="6af11-248">Assign hello **Standard User** role toohello user.</span></span> <span data-ttu-id="6af11-249">Vzhledem k tomu, který by mu udělit normální prostředí přístup, jakož i přístup přihlašování není přiřadit role správce hello.</span><span class="sxs-lookup"><span data-stu-id="6af11-249">Do not assign hello Administrator role because that would give him normal Atmosphere access as well as single sign-on access.</span></span>
    
    <span data-ttu-id="6af11-250">f.</span><span class="sxs-lookup"><span data-stu-id="6af11-250">f.</span></span> <span data-ttu-id="6af11-251">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6af11-251">Click **OK**.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="6af11-252">Hello uživatel neobdrží úvodní oznamovací e-mail obsahující heslo, které můžou být použité toolog v toohello AtomSphere účet, protože heslo je spravován pomocí zprostředkovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="6af11-252">hello user will not receive a welcome notification email containing a password that can be used toolog in toohello AtomSphere account because his password is managed through hello identity provider.</span></span> <span data-ttu-id="6af11-253">Může použít jakékoli jiné Boomi uživatele účtu vytvoření nástroje nebo rozhraní API poskytované Boomi tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="6af11-253">You may use any other Boomi user account creation tools or APIs provided by Boomi tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6af11-254">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6af11-254">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6af11-255">V této části povolíte tak, že udělíte přístup tooBoomi toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6af11-255">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBoomi.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6af11-257">**tooassign Britta Simon tooBoomi, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6af11-257">**tooassign Britta Simon tooBoomi, perform hello following steps:**</span></span>

1. <span data-ttu-id="6af11-258">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6af11-258">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6af11-260">V seznamu aplikace hello vyberte **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="6af11-260">In hello applications list, select **Boomi**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. <span data-ttu-id="6af11-262">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6af11-262">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6af11-264">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6af11-264">Click **Add** button.</span></span> <span data-ttu-id="6af11-265">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6af11-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6af11-267">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6af11-267">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6af11-268">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6af11-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6af11-269">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6af11-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6af11-270">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6af11-270">Testing single sign-on</span></span>

<span data-ttu-id="6af11-271">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6af11-271">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6af11-272">Když kliknete na dlaždici Boomi hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Boomi aplikace.</span><span class="sxs-lookup"><span data-stu-id="6af11-272">When you click hello Boomi tile in hello Access Panel, you should get automatically signed-on tooyour Boomi application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6af11-273">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6af11-273">Additional resources</span></span>

* [<span data-ttu-id="6af11-274">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6af11-274">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6af11-275">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6af11-275">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

