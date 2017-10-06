---
title: 'Kurz: Azure Active Directory integrace s DocuSign | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: e4ef40b8f5af20d811d8d806d2bd7e2039c55052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a><span data-ttu-id="9dbe1-103">Kurz: Azure Active Directory integrace s DocuSign</span><span class="sxs-lookup"><span data-stu-id="9dbe1-103">Tutorial: Azure Active Directory integration with DocuSign</span></span>

<span data-ttu-id="9dbe1-104">V tomto kurzu zjistíte, jak toointegrate DocuSign s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9dbe1-104">In this tutorial, you learn how toointegrate DocuSign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9dbe1-105">Integrace DocuSign s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9dbe1-105">Integrating DocuSign with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9dbe1-106">Můžete řídit ve službě Azure AD, který má přístup tooDocuSign</span><span class="sxs-lookup"><span data-stu-id="9dbe1-106">You can control in Azure AD who has access tooDocuSign</span></span>
- <span data-ttu-id="9dbe1-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDocuSign (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9dbe1-107">You can enable your users tooautomatically get signed-on tooDocuSign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9dbe1-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9dbe1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9dbe1-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9dbe1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9dbe1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9dbe1-110">Prerequisites</span></span>

<span data-ttu-id="9dbe1-111">Integrace služby Azure AD s DocuSign tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9dbe1-111">tooconfigure Azure AD integration with DocuSign, you need hello following items:</span></span>

- <span data-ttu-id="9dbe1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9dbe1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9dbe1-113">DocuSign jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9dbe1-113">A DocuSign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9dbe1-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9dbe1-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9dbe1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9dbe1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9dbe1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9dbe1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9dbe1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9dbe1-118">Scenario description</span></span>
<span data-ttu-id="9dbe1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9dbe1-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9dbe1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9dbe1-121">Přidání DocuSign z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9dbe1-121">Adding DocuSign from hello gallery</span></span>
2. <span data-ttu-id="9dbe1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9dbe1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-docusign-from-hello-gallery"></a><span data-ttu-id="9dbe1-123">Přidání DocuSign z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9dbe1-123">Adding DocuSign from hello gallery</span></span>
<span data-ttu-id="9dbe1-124">tooconfigure hello integrace DocuSign do Azure AD, je nutné tooadd DocuSign hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-124">tooconfigure hello integration of DocuSign into Azure AD, you need tooadd DocuSign from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9dbe1-125">**tooadd DocuSign z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9dbe1-125">**tooadd DocuSign from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9dbe1-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9dbe1-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9dbe1-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9dbe1-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9dbe1-133">Hello vyhledávacího pole zadejte **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-133">In hello search box, type **DocuSign**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_search.png)

5. <span data-ttu-id="9dbe1-135">Na panelu výsledků hello vyberte **DocuSign**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-135">In hello results panel, select **DocuSign**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9dbe1-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9dbe1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9dbe1-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s DocuSign podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9dbe1-138">In this section, you configure and test Azure AD single sign-on with DocuSign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9dbe1-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v DocuSign je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in DocuSign is tooa user in Azure AD.</span></span> <span data-ttu-id="9dbe1-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v DocuSign musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-140">In other words, a link relationship between an Azure AD user and hello related user in DocuSign needs toobe established.</span></span>

<span data-ttu-id="9dbe1-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v DocuSign.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in DocuSign.</span></span>

<span data-ttu-id="9dbe1-142">tooconfigure a testu Azure AD jednotné přihlašování s DocuSign, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9dbe1-142">tooconfigure and test Azure AD single sign-on with DocuSign, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9dbe1-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9dbe1-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9dbe1-145">**[Vytvoření zkušebního uživatele DocuSign](#creating-a-docusign-test-user)**  -toohave protějšek Britta Simon v DocuSign, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-145">**[Creating a DocuSign test user](#creating-a-docusign-test-user)** - toohave a counterpart of Britta Simon in DocuSign that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9dbe1-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9dbe1-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9dbe1-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9dbe1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9dbe1-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci DocuSign.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your DocuSign application.</span></span>

<span data-ttu-id="9dbe1-150">**tooconfigure Azure AD jednotné přihlašování s DocuSign, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9dbe1-150">**tooconfigure Azure AD single sign-on with DocuSign, perform hello following steps:**</span></span>

1. <span data-ttu-id="9dbe1-151">V portálu Azure, na hello hello **DocuSign** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-151">In hello Azure portal, on hello **DocuSign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9dbe1-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_samlbase.png)

3. <span data-ttu-id="9dbe1-155">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base 64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-155">On hello **SAML Signing Certificate** section, click **Certificate(Base 64)** and then save certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_certificate.png) 

4. <span data-ttu-id="9dbe1-157">Na hello **DocuSign konfigurace** části portálu Azure, klikněte na tlačítko **konfigurace DocuSign** tooopen přihlašování konfigurace okna.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-157">On hello **DocuSign Configuration** section of Azure portal, Click **Configure DocuSign** tooopen Configure sign-on window.</span></span> <span data-ttu-id="9dbe1-158">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9dbe1-158">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_configure.png)

5. <span data-ttu-id="9dbe1-160">V okně prohlížeče jiný web, přihlášení tooyour **portál pro správu DocuSign** jako správce.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-160">In a different web browser window, login tooyour **DocuSign admin portal** as an administrator.</span></span>

6. <span data-ttu-id="9dbe1-161">V navigační nabídce hello na levé straně hello, klikněte na **domény**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-161">In hello navigation menu on hello left, click **Domains**.</span></span>
   
    ![Konfigurace jednotného přihlašování][51]

7. <span data-ttu-id="9dbe1-163">V pravém podokně hello, klikněte na tlačítko **deklarace identity domény**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-163">On hello right pane, click **Claim Domain**.</span></span>
   
    ![Konfigurace jednotného přihlašování][52]

8. <span data-ttu-id="9dbe1-165">Na hello **deklarace identity domény** dialogové okno, ve hello **název domény** textovému poli, zadejte doménu vaší společnosti a pak klikněte na tlačítko **deklarace identity**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-165">On hello **Claim a domain** dialog, in hello **Domain Name** textbox, type your company domain, and then click **Claim**.</span></span> <span data-ttu-id="9dbe1-166">Ujistěte se, že ověření hello domény a stav hello je aktivní.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-166">Make sure that you verify hello domain and hello status is active.</span></span>
   
    ![Konfigurace jednotného přihlašování][53]

9. <span data-ttu-id="9dbe1-168">V nabídce na levé straně hello, klikněte na tlačítko **poskytovatelů identit**</span><span class="sxs-lookup"><span data-stu-id="9dbe1-168">In menu on hello left side, click **Identity Providers**</span></span>  
   
    ![Konfigurace jednotného přihlašování][54]
10. <span data-ttu-id="9dbe1-170">V pravém podokně hello, klikněte na **přidat zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-170">In hello right pane, click **Add Identity Provider**.</span></span> 
   
    ![Konfigurace jednotného přihlašování][55]

11. <span data-ttu-id="9dbe1-172">Na hello **nastavení zprostředkovatele Identity** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9dbe1-172">On hello **Identity Provider Settings** page, perform hello following steps:</span></span>
   
    ![Konfigurace jednotného přihlašování][56]

    <span data-ttu-id="9dbe1-174">a.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-174">a.</span></span> <span data-ttu-id="9dbe1-175">V hello **název** textovému poli, zadejte jedinečný název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-175">In hello **Name** textbox, type a unique name for your configuration.</span></span> <span data-ttu-id="9dbe1-176">Nepoužívejte mezery.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-176">Do not use spaces.</span></span>

    <span data-ttu-id="9dbe1-177">b.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-177">b.</span></span> <span data-ttu-id="9dbe1-178">Vložení **SAML Entity ID** do hello **vystavitele zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-178">Paste **SAML Entity ID** into hello **Identity Provider Issuer** textbox.</span></span>

    <span data-ttu-id="9dbe1-179">c.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-179">c.</span></span> <span data-ttu-id="9dbe1-180">Vložení **SAML jeden přihlašování adresa URL služby** do hello **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-180">Paste **SAML Single Sign-On Service URL** into hello **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="9dbe1-181">d.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-181">d.</span></span> <span data-ttu-id="9dbe1-182">Vložení **Sign-Out URL** do hello **adresa URL odhlašovací zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-182">Paste **Sign-Out URL** into hello **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="9dbe1-183">e.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-183">e.</span></span> <span data-ttu-id="9dbe1-184">Vyberte **přihlásit ověřovacího požadavku**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-184">Select **Sign AuthN Request**.</span></span>

    <span data-ttu-id="9dbe1-185">f.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-185">f.</span></span> <span data-ttu-id="9dbe1-186">Jako **odeslání ověřovacího požadavku pomocí**, vyberte **POST**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-186">As **Send AuthN request by**, select **POST**.</span></span>

    <span data-ttu-id="9dbe1-187">g.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-187">g.</span></span> <span data-ttu-id="9dbe1-188">Jako **odeslán požadavek na odhlášení podle**, vyberte **získat**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-188">As **Send logout request by**, select **GET**.</span></span>

12. <span data-ttu-id="9dbe1-189">V hello **vlastní atribut mapování** zvolte pole hello chcete toomap s Azure AD deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-189">In hello **Custom Attribute Mapping** section, choose hello field you want toomap with Azure AD Claim.</span></span> <span data-ttu-id="9dbe1-190">V tomto příkladu hello **emailaddress** deklarace identity je namapována na hodnotu hello **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-190">In this example, hello **emailaddress** claim is mapped with hello value of **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span> <span data-ttu-id="9dbe1-191">Je hello výchozí název deklarace identity z Azure AD pro deklarace identity e-mailu.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-191">It is hello default claim name from Azure AD for email claim.</span></span> 
   
    > [!NOTE]
    > <span data-ttu-id="9dbe1-192">Použití hello odpovídající **uživatelský identifikátor** toomap hello uživatele z mapování uživatelů tooDocuSign Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-192">Use hello appropriate **User identifier** toomap hello user from Azure AD tooDocuSign user mapping.</span></span> <span data-ttu-id="9dbe1-193">Vyberte hello správné pole a zadejte odpovídající hodnotu hello na základě nastavení vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-193">Select hello proper Field and enter hello appropriate value based on your organization settings.</span></span>
          
    ![Konfigurace jednotného přihlašování][57]

13. <span data-ttu-id="9dbe1-195">V hello **certifikát zprostředkovatele Identity** klikněte na tlačítko **přidat certifikát**a pak nahrajte certifikát hello jste si stáhli z portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-195">In hello **Identity Provider Certificate** section, click **Add Certificate**, and then upload hello certificate you have downloaded from Azure AD portal.</span></span>   
   
    ![Konfigurace jednotného přihlašování][58]

14. <span data-ttu-id="9dbe1-197">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-197">Click **Save**.</span></span>

15. <span data-ttu-id="9dbe1-198">V hello **zprostředkovatelů Identity** klikněte na tlačítko **akce**a potom klikněte na **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-198">In hello **Identity Providers** section, click **Actions**, and then click **Endpoints**.</span></span>   
   
    ![Konfigurace jednotného přihlašování][59]
 
16. <span data-ttu-id="9dbe1-200">V hello **zobrazit SAML 2.0 koncové body** části na **portál pro správu DocuSign**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9dbe1-200">In hello **View SAML 2.0 Endpoints** section on **DocuSign admin portal**, perform hello following steps:</span></span>
   
    ![Konfigurace jednotného přihlašování][60]
   
    <span data-ttu-id="9dbe1-202">a.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-202">a.</span></span> <span data-ttu-id="9dbe1-203">Kopírování hello **adresa URL vystavitele pro zprostředkovatele služby**a vložte do hello **identifikátor** textové pole na **DocuSign domény a adresy URL** části hello Azure portálu následující hello vzor: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-203">Copy hello **Service Provider Issuer URL**, and then paste into hello **Identifier** textbox on **DocuSign Domain and URLs** section of hello Azure portal following hello pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span></span>
   
    <span data-ttu-id="9dbe1-204">b.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-204">b.</span></span> <span data-ttu-id="9dbe1-205">Kopírování hello **adresa URL služby zprostředkovatele přihlášení**a vložte do hello **přihlašovací adresa URL** textové pole na **DocuSign domény a adresy URL** části hello Azure portálu následující hello vzor: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-205">Copy hello **Service Provider Login URL**, and then paste into hello **Sign On URL** textbox on **DocuSign Domain and URLs** section of hello Azure portal following hello pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_url.png)
      
    <span data-ttu-id="9dbe1-207">c.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-207">c.</span></span>  <span data-ttu-id="9dbe1-208">Klikněte na tlačítko **zavřít**</span><span class="sxs-lookup"><span data-stu-id="9dbe1-208">Click **Close**</span></span>
    
17. <span data-ttu-id="9dbe1-209">Na portálu Azure hello, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-209">On hello Azure portal, click **Save**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="9dbe1-211">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="9dbe1-211">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9dbe1-212">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-212">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9dbe1-213">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9dbe1-213">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9dbe1-214">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9dbe1-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="9dbe1-215">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-215">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9dbe1-217">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9dbe1-217">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9dbe1-218">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-218">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9dbe1-220">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-220">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9dbe1-222">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-222">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9dbe1-224">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9dbe1-224">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9dbe1-226">a.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-226">a.</span></span> <span data-ttu-id="9dbe1-227">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-227">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9dbe1-228">b.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-228">b.</span></span> <span data-ttu-id="9dbe1-229">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-229">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9dbe1-230">c.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-230">c.</span></span> <span data-ttu-id="9dbe1-231">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-231">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9dbe1-232">d.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-232">d.</span></span> <span data-ttu-id="9dbe1-233">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-233">Click **Create**.</span></span>
 
### <a name="creating-a-docusign-test-user"></a><span data-ttu-id="9dbe1-234">Vytvoření zkušebního uživatele DocuSign</span><span class="sxs-lookup"><span data-stu-id="9dbe1-234">Creating a DocuSign test user</span></span>

<span data-ttu-id="9dbe1-235">Aplikace podporuje **těsně v zřizování uživatelů čas** a po ověření uživatele v aplikaci hello automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-235">Application supports **Just in time user provisioning** and after authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9dbe1-236">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9dbe1-236">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9dbe1-237">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooDocuSign svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-237">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooDocuSign.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9dbe1-239">**tooassign Britta Simon tooDocuSign, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9dbe1-239">**tooassign Britta Simon tooDocuSign, perform hello following steps:**</span></span>

1. <span data-ttu-id="9dbe1-240">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-240">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9dbe1-242">V seznamu aplikace hello vyberte **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-242">In hello applications list, select **DocuSign**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_app.png) 

3. <span data-ttu-id="9dbe1-244">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-244">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9dbe1-246">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-246">Click **Add** button.</span></span> <span data-ttu-id="9dbe1-247">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9dbe1-249">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-249">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9dbe1-250">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9dbe1-251">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9dbe1-252">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9dbe1-252">Testing single sign-on</span></span>

<span data-ttu-id="9dbe1-253">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-253">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9dbe1-254">Když kliknete na dlaždici DocuSign hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour DocuSign aplikace.</span><span class="sxs-lookup"><span data-stu-id="9dbe1-254">When you click hello DocuSign tile in hello Access Panel, you should get automatically signed-on tooyour DocuSign application.</span></span>
<span data-ttu-id="9dbe1-255">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9dbe1-255">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9dbe1-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9dbe1-256">Additional resources</span></span>

* [<span data-ttu-id="9dbe1-257">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9dbe1-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9dbe1-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9dbe1-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="9dbe1-259">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="9dbe1-259">Configure User Provisioning</span></span>](active-directory-saas-docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_203.png

