---
title: "Kurz: Azure Active Directory integrace s Adobe přihlašovací | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Adobe přihlášení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: b4b07907f30a0890003554a02a76d968400b43ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a><span data-ttu-id="df458-103">Kurz: Azure Active Directory integrace s Adobe přihlášení</span><span class="sxs-lookup"><span data-stu-id="df458-103">Tutorial: Azure Active Directory integration with Adobe Sign</span></span>

<span data-ttu-id="df458-104">V tomto kurzu zjistíte, jak toointegrate Adobe přihlaste s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="df458-104">In this tutorial, you learn how toointegrate Adobe Sign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="df458-105">Integrace Adobe přihlášení s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="df458-105">Integrating Adobe Sign with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="df458-106">Můžete řídit ve službě Azure AD, který má přístup tooAdobe přihlášení</span><span class="sxs-lookup"><span data-stu-id="df458-106">You can control in Azure AD who has access tooAdobe Sign</span></span>
- <span data-ttu-id="df458-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAdobe přihlašovací (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="df458-107">You can enable your users tooautomatically get signed-on tooAdobe Sign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="df458-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="df458-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="df458-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="df458-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df458-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="df458-110">Prerequisites</span></span>

<span data-ttu-id="df458-111">tooconfigure integrace Azure AD znakem Adobe, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="df458-111">tooconfigure Azure AD integration with Adobe Sign, you need hello following items:</span></span>

- <span data-ttu-id="df458-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="df458-112">An Azure AD subscription</span></span>
- <span data-ttu-id="df458-113">Přihlášení Adobe jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="df458-113">An Adobe Sign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="df458-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="df458-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="df458-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="df458-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="df458-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="df458-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="df458-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="df458-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="df458-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="df458-118">Scenario description</span></span>
<span data-ttu-id="df458-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="df458-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="df458-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="df458-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="df458-121">Přidání Adobe přihlášení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="df458-121">Adding Adobe Sign from hello gallery</span></span>
2. <span data-ttu-id="df458-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="df458-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-sign-from-hello-gallery"></a><span data-ttu-id="df458-123">Přidání Adobe přihlášení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="df458-123">Adding Adobe Sign from hello gallery</span></span>
<span data-ttu-id="df458-124">tooconfigure hello Integrace Adobe registrace do Azure AD, je nutné tooadd Adobe přihlašovací hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="df458-124">tooconfigure hello integration of Adobe Sign into Azure AD, you need tooadd Adobe Sign from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="df458-125">**tooadd Adobe přihlášení z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="df458-125">**tooadd Adobe Sign from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="df458-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="df458-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="df458-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="df458-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="df458-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="df458-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="df458-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="df458-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="df458-133">Hello vyhledávacího pole zadejte **Adobe přihlašovací**.</span><span class="sxs-lookup"><span data-stu-id="df458-133">In hello search box, type **Adobe Sign**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. <span data-ttu-id="df458-135">Na panelu výsledků hello vyberte **Adobe přihlašovací**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="df458-135">In hello results panel, select **Adobe Sign**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="df458-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="df458-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="df458-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování znakem Adobe podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="df458-138">In this section, you configure and test Azure AD single sign-on with Adobe Sign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="df458-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Adobe přihlášení je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="df458-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Adobe Sign is tooa user in Azure AD.</span></span> <span data-ttu-id="df458-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Adobe přihlašovací musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="df458-140">In other words, a link relationship between an Azure AD user and hello related user in Adobe Sign needs toobe established.</span></span>

<span data-ttu-id="df458-141">V Adobe přihlašovací přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="df458-141">In Adobe Sign, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="df458-142">tooconfigure a testu Azure AD jednotné přihlašování s Adobe přihlášení, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="df458-142">tooconfigure and test Azure AD single sign-on with Adobe Sign, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="df458-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="df458-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="df458-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df458-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="df458-145">**[Vytváření testovacího uživatele přihlášení Adobe](#creating-an-adobe-sign-test-user)**  -toohave protějšek Britta Simon v Adobe přihlášení, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="df458-145">**[Creating an Adobe Sign test user](#creating-an-adobe-sign-test-user)** - toohave a counterpart of Britta Simon in Adobe Sign that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="df458-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="df458-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="df458-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="df458-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="df458-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="df458-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="df458-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Adobe přihlášení.</span><span class="sxs-lookup"><span data-stu-id="df458-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Adobe Sign application.</span></span>

<span data-ttu-id="df458-150">**tooconfigure Azure AD jednotné přihlašování znakem Adobe, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="df458-150">**tooconfigure Azure AD single sign-on with Adobe Sign, perform hello following steps:**</span></span>

1. <span data-ttu-id="df458-151">V portálu Azure, na hello hello **Adobe přihlašovací** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="df458-151">In hello Azure portal, on hello **Adobe Sign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="df458-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="df458-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. <span data-ttu-id="df458-155">Na hello **Adobe přihlašovací domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="df458-155">On hello **Adobe Sign Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    <span data-ttu-id="df458-157">a.</span><span class="sxs-lookup"><span data-stu-id="df458-157">a.</span></span> <span data-ttu-id="df458-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.echosign.com/`</span><span class="sxs-lookup"><span data-stu-id="df458-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.echosign.com/`</span></span>

    <span data-ttu-id="df458-159">b.</span><span class="sxs-lookup"><span data-stu-id="df458-159">b.</span></span> <span data-ttu-id="df458-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.echosign.com`</span><span class="sxs-lookup"><span data-stu-id="df458-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.echosign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="df458-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="df458-161">These values are not real.</span></span> <span data-ttu-id="df458-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="df458-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="df458-163">Obraťte se na [tým podpory Adobe přihlašovací klienta](https://helpx.adobe.com/in/contact/support.html) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="df458-163">Contact [Adobe Sign Client support team](https://helpx.adobe.com/in/contact/support.html) tooget these values.</span></span> 
 
4. <span data-ttu-id="df458-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="df458-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. <span data-ttu-id="df458-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="df458-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="df458-168">Na hello **Adobe přihlašovací konfigurace** klikněte na tlačítko **nakonfigurovat přihlašovací Adobe** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="df458-168">On hello **Adobe Sign Configuration** section, click **Configure Adobe Sign** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="df458-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="df458-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 


7. <span data-ttu-id="df458-171">V okně prohlížeče jiný web Přihlaste se jako správce v tooyour Adobe přihlášení na webu společnosti.</span><span class="sxs-lookup"><span data-stu-id="df458-171">In a different web browser window, log in tooyour Adobe Sign company site as an administrator.</span></span>

8. <span data-ttu-id="df458-172">V nabídce hello hello nahoře, klikněte na tlačítko **účet**a klikněte v navigačním podokně hello na levé straně hello **SAML nastavení** pod **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="df458-172">In hello menu on hello top, click **Account**, and then, in hello navigation pane on hello left side, click **SAML Settings** under **Account Settings**.</span></span>
   
   <span data-ttu-id="df458-173">![Účet](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "účtu")</span><span class="sxs-lookup"><span data-stu-id="df458-173">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "Account")</span></span>

9. <span data-ttu-id="df458-174">V hello v oddílu nastavení SAML proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="df458-174">In hello SAML Settings section, perform hello following steps:</span></span>
   
   <span data-ttu-id="df458-175">![Nastavení SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="df458-175">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML Settings")</span></span>
   
   <span data-ttu-id="df458-176">a.</span><span class="sxs-lookup"><span data-stu-id="df458-176">a.</span></span> <span data-ttu-id="df458-177">Jako **SAML režimu**, vyberte **SAML povinné**.</span><span class="sxs-lookup"><span data-stu-id="df458-177">As **SAML Mode**, select **SAML Mandatory**.</span></span>
   
   <span data-ttu-id="df458-178">b.</span><span class="sxs-lookup"><span data-stu-id="df458-178">b.</span></span> <span data-ttu-id="df458-179">Vyberte **toolog povolit správci EchoSign účtu pomocí svých přihlašovacích údajů EchoSign**.</span><span class="sxs-lookup"><span data-stu-id="df458-179">Select **Allow EchoSign Account Administrators toolog in using their EchoSign Credentials**.</span></span>
   
   <span data-ttu-id="df458-180">c.</span><span class="sxs-lookup"><span data-stu-id="df458-180">c.</span></span> <span data-ttu-id="df458-181">Jako **vytvoření uživatele**, vyberte **automaticky přidat uživatele ověřeného pomocí SAML**.</span><span class="sxs-lookup"><span data-stu-id="df458-181">As **User Creation**, select **Automatically add users authenticated through SAML**.</span></span>

10. <span data-ttu-id="df458-182">Přesunout, provádění hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="df458-182">Move on, performing hello following steps:</span></span>

       <span data-ttu-id="df458-183">![Nastavení SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="df458-183">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML Settings")</span></span>

    <span data-ttu-id="df458-184">a.</span><span class="sxs-lookup"><span data-stu-id="df458-184">a.</span></span> <span data-ttu-id="df458-185">Vložení **SAML Entity ID**, který jste zkopírovali z portálu Azure do hello **IdP Entity ID** textové pole.</span><span class="sxs-lookup"><span data-stu-id="df458-185">Paste **SAML Entity ID**, which you have copied from Azure portal into hello **IdP Entity ID** textbox.</span></span>
    
    <span data-ttu-id="df458-186">b.</span><span class="sxs-lookup"><span data-stu-id="df458-186">b.</span></span> <span data-ttu-id="df458-187">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure do hello **IdP přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="df458-187">Paste **SAML Single Sign-On Service URL**, which you have copied from Azure portal into hello **IdP Login URL** textbox.</span></span>
   
    <span data-ttu-id="df458-188">c.</span><span class="sxs-lookup"><span data-stu-id="df458-188">c.</span></span> <span data-ttu-id="df458-189">Vložení **Sign-Out URL**, který jste zkopírovali z portálu Azure do hello **adresy URL odhlašovací IdP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="df458-189">Paste **Sign-Out URL**, which you have copied from Azure portal into hello **IdP Logout URL** textbox.</span></span>

    <span data-ttu-id="df458-190">d.</span><span class="sxs-lookup"><span data-stu-id="df458-190">d.</span></span> <span data-ttu-id="df458-191">Otevřete váš stažené **Certificate(Base64)** souborů v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **IdP certifikát** textbox</span><span class="sxs-lookup"><span data-stu-id="df458-191">Open your downloaded **Certificate(Base64)** file in notepad, copy hello content of it into your clipboard, and then paste it toohello **IdP Certificate** textbox</span></span>

    <span data-ttu-id="df458-192">e.</span><span class="sxs-lookup"><span data-stu-id="df458-192">e.</span></span> <span data-ttu-id="df458-193">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="df458-193">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="df458-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="df458-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="df458-195">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="df458-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="df458-196">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="df458-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="df458-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="df458-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="df458-198">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="df458-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="df458-200">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="df458-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="df458-201">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="df458-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="df458-203">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="df458-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="df458-205">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="df458-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="df458-207">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="df458-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="df458-209">a.</span><span class="sxs-lookup"><span data-stu-id="df458-209">a.</span></span> <span data-ttu-id="df458-210">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="df458-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="df458-211">b.</span><span class="sxs-lookup"><span data-stu-id="df458-211">b.</span></span> <span data-ttu-id="df458-212">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="df458-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="df458-213">c.</span><span class="sxs-lookup"><span data-stu-id="df458-213">c.</span></span> <span data-ttu-id="df458-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="df458-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="df458-215">d.</span><span class="sxs-lookup"><span data-stu-id="df458-215">d.</span></span> <span data-ttu-id="df458-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="df458-216">Click **Create**.</span></span>
 
### <a name="creating-an-adobe-sign-test-user"></a><span data-ttu-id="df458-217">Vytváření testovacího uživatele přihlášení Adobe</span><span class="sxs-lookup"><span data-stu-id="df458-217">Creating an Adobe Sign test user</span></span>

<span data-ttu-id="df458-218">Uživatelé toolog tooenable Azure AD v tooAdobe přihlášení, se musí být zřízená do Adobe přihlášení.</span><span class="sxs-lookup"><span data-stu-id="df458-218">tooenable Azure AD users toolog in tooAdobe Sign, they must be provisioned into Adobe Sign.</span></span> <span data-ttu-id="df458-219">V případě hello Adobe registrace zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="df458-219">In hello case of Adobe Sign, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="df458-220">Můžete použít jakékoli jiné Adobe přihlášení uživatele účtu vytvoření nástroje nebo rozhraní API poskytované tooprovision Adobe přihlašovací uživatelské účty AAD.</span><span class="sxs-lookup"><span data-stu-id="df458-220">You can use any other Adobe Sign user account creation tools or APIs provided by Adobe Sign tooprovision AAD user accounts.</span></span> 

<span data-ttu-id="df458-221">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="df458-221">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="df458-222">Přihlaste se tooyour **Adobe přihlašovací** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="df458-222">Log in tooyour **Adobe Sign** company site as administrator.</span></span>

2. <span data-ttu-id="df458-223">V nabídce hello hello nahoře, klikněte na tlačítko **účet**a klikněte v navigačním podokně hello na levé straně hello **uživatelé a skupiny**a potom klikněte na **vytvořte nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="df458-223">In hello menu on hello top, click **Account**, and then, in hello navigation pane on hello left side, click **Users & Groups**, and then, click **Create a new user**.</span></span>
   
   <span data-ttu-id="df458-224">![Účet](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "účtu")</span><span class="sxs-lookup"><span data-stu-id="df458-224">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "Account")</span></span>
   
3. <span data-ttu-id="df458-225">V hello **vytvořit nového uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="df458-225">In hello **Create New User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="df458-226">![Vytvoření uživatele](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="df458-226">![Create User](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "Create User")</span></span>
   
   <span data-ttu-id="df458-227">a.</span><span class="sxs-lookup"><span data-stu-id="df458-227">a.</span></span> <span data-ttu-id="df458-228">Typ hello **e-mailovou adresu**, **křestní jméno**, a **příjmení** z platný účet AAD chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="df458-228">Type hello **Email Address**, **First Name**, and **Last Name** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
   
   <span data-ttu-id="df458-229">b.</span><span class="sxs-lookup"><span data-stu-id="df458-229">b.</span></span> <span data-ttu-id="df458-230">Klikněte na tlačítko **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="df458-230">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="df458-231">Držitel účtu Azure Active Directory Hello obdrží e-mail, který obsahuje účet odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="df458-231">hello Azure Active Directory account holder receives an email that includes a link tooconfirm hello account before it becomes active.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="df458-232">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="df458-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="df458-233">V této části povolíte tak, že udělíte přístup tooAdobe přihlašovací Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="df458-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAdobe Sign.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="df458-235">**tooassign tooAdobe Britta Simon přihlášení, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="df458-235">**tooassign Britta Simon tooAdobe Sign, perform hello following steps:**</span></span>

1. <span data-ttu-id="df458-236">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="df458-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="df458-238">V seznamu aplikace hello vyberte **Adobe přihlašovací**.</span><span class="sxs-lookup"><span data-stu-id="df458-238">In hello applications list, select **Adobe Sign**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. <span data-ttu-id="df458-240">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="df458-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="df458-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="df458-242">Click **Add** button.</span></span> <span data-ttu-id="df458-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="df458-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="df458-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="df458-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="df458-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="df458-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="df458-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="df458-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="df458-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="df458-248">Testing single sign-on</span></span>

<span data-ttu-id="df458-249">Po kliknutí na tlačítko hello Adobe přihlašovací dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace Adobe přihlášení.</span><span class="sxs-lookup"><span data-stu-id="df458-249">When you click hello Adobe Sign tile in hello Access Panel, you should get automatically signed-on tooyour Adobe Sign application.</span></span>
<span data-ttu-id="df458-250">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="df458-250">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df458-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="df458-251">Additional resources</span></span>

* [<span data-ttu-id="df458-252">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="df458-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="df458-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="df458-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

