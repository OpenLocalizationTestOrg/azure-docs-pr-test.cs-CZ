---
title: "Kurz: Azure Active Directory integrace s rozpoznávat | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a rozpoznávat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: f33fc3959f72f875b8c5c4f0abd4e9b6737ca615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a><span data-ttu-id="b5832-103">Kurz: Azure Active Directory integrace s rozpoznávat</span><span class="sxs-lookup"><span data-stu-id="b5832-103">Tutorial: Azure Active Directory integration with Recognize</span></span>

<span data-ttu-id="b5832-104">V tomto kurzu zjistíte, jak toointegrate rozpoznat službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b5832-104">In this tutorial, you learn how toointegrate Recognize with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b5832-105">Integrace rozpoznávat s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b5832-105">Integrating Recognize with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b5832-106">Můžete řídit ve službě Azure AD, který má přístup tooRecognize</span><span class="sxs-lookup"><span data-stu-id="b5832-106">You can control in Azure AD who has access tooRecognize</span></span>
- <span data-ttu-id="b5832-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooRecognize (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5832-107">You can enable your users tooautomatically get signed-on tooRecognize (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b5832-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b5832-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b5832-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b5832-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5832-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5832-110">Prerequisites</span></span>

<span data-ttu-id="b5832-111">integrace tooconfigure Azure AD s rozpoznávat, je nutné hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b5832-111">tooconfigure Azure AD integration with Recognize, you need hello following items:</span></span>

- <span data-ttu-id="b5832-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5832-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b5832-113">Rozpoznávat jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b5832-113">A Recognize single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b5832-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b5832-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b5832-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b5832-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b5832-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b5832-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b5832-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b5832-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b5832-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b5832-118">Scenario description</span></span>
<span data-ttu-id="b5832-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b5832-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b5832-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b5832-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b5832-121">Přidání rozpoznávat z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b5832-121">Adding Recognize from hello gallery</span></span>
2. <span data-ttu-id="b5832-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b5832-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-recognize-from-hello-gallery"></a><span data-ttu-id="b5832-123">Přidání rozpoznávat z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b5832-123">Adding Recognize from hello gallery</span></span>
<span data-ttu-id="b5832-124">tooconfigure hello integrace rozpoznávat do Azure AD, je nutné tooadd rozpoznávat hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b5832-124">tooconfigure hello integration of Recognize into Azure AD, you need tooadd Recognize from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b5832-125">**tooadd rozpoznávat z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b5832-125">**tooadd Recognize from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5832-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b5832-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b5832-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b5832-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b5832-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b5832-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b5832-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5832-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b5832-133">Hello vyhledávacího pole zadejte **rozpoznávat**.</span><span class="sxs-lookup"><span data-stu-id="b5832-133">In hello search box, type **Recognize**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_search.png)

5. <span data-ttu-id="b5832-135">Na panelu výsledků hello vyberte **rozpoznávat**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5832-135">In hello results panel, select **Recognize**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b5832-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b5832-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b5832-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s rozpoznávat podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b5832-138">In this section, you configure and test Azure AD single sign-on with Recognize based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b5832-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v rozpoznávat je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b5832-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Recognize is tooa user in Azure AD.</span></span> <span data-ttu-id="b5832-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v rozpoznávat musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b5832-140">In other words, a link relationship between an Azure AD user and hello related user in Recognize needs toobe established.</span></span>

<span data-ttu-id="b5832-141">V rozpoznávat, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b5832-141">In Recognize, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b5832-142">tooconfigure a testování Azure AD jednotné přihlašování s rozpoznávat, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b5832-142">tooconfigure and test Azure AD single sign-on with Recognize, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b5832-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b5832-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b5832-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b5832-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b5832-145">**[Vytvoření zkušebního uživatele rozpoznávat](#creating-a-recognize-test-user)**  -toohave protějšek Britta Simon v rozpoznávat, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b5832-145">**[Creating a Recognize test user](#creating-a-recognize-test-user)** - toohave a counterpart of Britta Simon in Recognize that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b5832-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b5832-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b5832-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b5832-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b5832-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b5832-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b5832-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci rozpoznávat.</span><span class="sxs-lookup"><span data-stu-id="b5832-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Recognize application.</span></span>

<span data-ttu-id="b5832-150">**tooconfigure Azure AD jednotné přihlašování s rozpoznávat, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b5832-150">**tooconfigure Azure AD single sign-on with Recognize, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5832-151">V portálu Azure, na hello hello **rozpoznávat** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b5832-151">In hello Azure portal, on hello **Recognize** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b5832-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b5832-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_samlbase.png)

3. <span data-ttu-id="b5832-155">Na hello **rozpoznat domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b5832-155">On hello **Recognize Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_url.png)

    <span data-ttu-id="b5832-157">a.</span><span class="sxs-lookup"><span data-stu-id="b5832-157">a.</span></span> <span data-ttu-id="b5832-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://recognizeapp.com/<your-domain>/saml/sso`</span><span class="sxs-lookup"><span data-stu-id="b5832-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://recognizeapp.com/<your-domain>/saml/sso`</span></span>

    <span data-ttu-id="b5832-159">b.</span><span class="sxs-lookup"><span data-stu-id="b5832-159">b.</span></span> <span data-ttu-id="b5832-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://recognizeapp.com/<your-domain>`</span><span class="sxs-lookup"><span data-stu-id="b5832-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://recognizeapp.com/<your-domain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b5832-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b5832-161">These values are not real.</span></span> <span data-ttu-id="b5832-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b5832-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b5832-163">Obraťte se na [tým podpory rozpoznat klienta](mailto:support@recognizeapp.com) získat přihlašovací adresa URL a můžete získat hodnotu identifikátoru otevřením hello adresa URL služby zprostředkovatele metadat z hello nastavení jednotného přihlašování k oddílu, který se vysvětluje dále v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="b5832-163">Contact [Recognize Client support team](mailto:support@recognizeapp.com) to get Sign-On URL and you can get Identifier value by opening hello Service Provider Metadata URL from hello SSO Settings section that is explained later in hello tutorial.</span></span> <span data-ttu-id="b5832-164">.</span><span class="sxs-lookup"><span data-stu-id="b5832-164">.</span></span> 
 
4. <span data-ttu-id="b5832-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b5832-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_certificate.png) 

5. <span data-ttu-id="b5832-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5832-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b5832-169">Na hello **rozpoznat konfigurace** klikněte na tlačítko **konfigurace rozpoznat** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b5832-169">On hello **Recognize Configuration** section, click **Configure Recognize** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b5832-170">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b5832-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_configure.png) 

7. <span data-ttu-id="b5832-172">V okně prohlížeče jiný web, klienta rozpoznávat tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="b5832-172">In a different web browser window, sign-on tooyour Recognize tenant as an administrator.</span></span>

8. <span data-ttu-id="b5832-173">V horním pravém rohu hello, klikněte na tlačítko **nabídky**.</span><span class="sxs-lookup"><span data-stu-id="b5832-173">On hello upper right corner, click **Menu**.</span></span> <span data-ttu-id="b5832-174">Přejděte příliš**správce společnosti**.</span><span class="sxs-lookup"><span data-stu-id="b5832-174">Go too**Company Admin**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

9. <span data-ttu-id="b5832-176">V levém navigačním podokně hello, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b5832-176">On hello left navigation pane, click **Settings**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

10. <span data-ttu-id="b5832-178">Proveďte následující kroky hello **nastavení jednotného přihlašování k** části.</span><span class="sxs-lookup"><span data-stu-id="b5832-178">Perform hello following steps on **SSO Settings** section.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
    
    <span data-ttu-id="b5832-180">a.</span><span class="sxs-lookup"><span data-stu-id="b5832-180">a.</span></span> <span data-ttu-id="b5832-181">Jako **povolit jednotné přihlašování**, vyberte **ON**.</span><span class="sxs-lookup"><span data-stu-id="b5832-181">As **Enable SSO**, select **ON**.</span></span>

    <span data-ttu-id="b5832-182">b.</span><span class="sxs-lookup"><span data-stu-id="b5832-182">b.</span></span> <span data-ttu-id="b5832-183">V hello **IDP Entity ID** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b5832-183">In hello **IDP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="b5832-184">c.</span><span class="sxs-lookup"><span data-stu-id="b5832-184">c.</span></span> <span data-ttu-id="b5832-185">V hello **jednotné přihlašování cílová adresa url** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b5832-185">In hello **Sso target url** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="b5832-186">d.</span><span class="sxs-lookup"><span data-stu-id="b5832-186">d.</span></span> <span data-ttu-id="b5832-187">V hello **Slo cílová adresa url** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b5832-187">In hello **Slo target url** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span> 
    
    <span data-ttu-id="b5832-188">e.</span><span class="sxs-lookup"><span data-stu-id="b5832-188">e.</span></span> <span data-ttu-id="b5832-189">Otevřete váš stažené **certifikátu (Base64)** souborů v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b5832-189">Open your downloaded **Certificate (Base64)** file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Certificate** textbox.</span></span>
    
    <span data-ttu-id="b5832-190">f.</span><span class="sxs-lookup"><span data-stu-id="b5832-190">f.</span></span> <span data-ttu-id="b5832-191">Klikněte na tlačítko hello **uložit nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5832-191">Click hello **Save settings** button.</span></span> 

11. <span data-ttu-id="b5832-192">Vedle položky hello **nastavení jednotného přihlašování k** část, zkopírujte adresu URL hello pod **adresu url služby zprostředkovatele metadat**.</span><span class="sxs-lookup"><span data-stu-id="b5832-192">Beside hello **SSO Settings** section, copy hello URL under **Service Provider Metadata url**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

12. <span data-ttu-id="b5832-194">Otevřete hello **adresu URL metadat odkaz** v dokumentu metadat hello toodownload prázdné prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b5832-194">Open hello **Metadata URL link** under a blank browser toodownload hello metadata document.</span></span> <span data-ttu-id="b5832-195">Poté zkopírujte hello EntityDescriptor value(entityID) ze souboru hello a vložte jej do **identifikátor** textového pole v **rozpoznat domény a adresy URL části** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b5832-195">Then copy hello EntityDescriptor value(entityID) from hello file and paste it in **Identifier** textbox in **Recognize Domain and URLs section** on Azure portal.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> <span data-ttu-id="b5832-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b5832-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b5832-198">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b5832-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b5832-199">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b5832-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b5832-200">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5832-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="b5832-201">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b5832-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b5832-203">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b5832-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5832-204">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b5832-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b5832-206">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b5832-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b5832-208">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b5832-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b5832-210">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b5832-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b5832-212">a.</span><span class="sxs-lookup"><span data-stu-id="b5832-212">a.</span></span> <span data-ttu-id="b5832-213">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b5832-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b5832-214">b.</span><span class="sxs-lookup"><span data-stu-id="b5832-214">b.</span></span> <span data-ttu-id="b5832-215">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b5832-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b5832-216">c.</span><span class="sxs-lookup"><span data-stu-id="b5832-216">c.</span></span> <span data-ttu-id="b5832-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b5832-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b5832-218">d.</span><span class="sxs-lookup"><span data-stu-id="b5832-218">d.</span></span> <span data-ttu-id="b5832-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b5832-219">Click **Create**.</span></span>
 
### <a name="creating-a-recognize-test-user"></a><span data-ttu-id="b5832-220">Vytvoření zkušebního uživatele rozpoznávat</span><span class="sxs-lookup"><span data-stu-id="b5832-220">Creating a Recognize test user</span></span>

<span data-ttu-id="b5832-221">V pořadí tooenable Azure AD Uživatelé toolog do rozpoznávat musí být zřízená do rozpoznávat.</span><span class="sxs-lookup"><span data-stu-id="b5832-221">In order tooenable Azure AD users toolog into Recognize, they must be provisioned into Recognize.</span></span> <span data-ttu-id="b5832-222">V případě hello rozpoznávat zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b5832-222">In hello case of Recognize, provisioning is a manual task.</span></span>

<span data-ttu-id="b5832-223">Tato aplikace nepodporuje SCIM zřizování, ale má synchronizaci alternativní uživatele, která zřizuje uživatele.</span><span class="sxs-lookup"><span data-stu-id="b5832-223">This app doesn't support SCIM provisioning but has an alternate user sync that provisions users.</span></span> 

<span data-ttu-id="b5832-224">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b5832-224">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5832-225">Přihlaste se k serveru vaší společnosti rozpoznávat jako správce.</span><span class="sxs-lookup"><span data-stu-id="b5832-225">Log into your Recognize company site as an administrator.</span></span>

2. <span data-ttu-id="b5832-226">V horním pravém rohu hello, klikněte na tlačítko **nabídky**.</span><span class="sxs-lookup"><span data-stu-id="b5832-226">On hello upper right corner, click **Menu**.</span></span> <span data-ttu-id="b5832-227">Přejděte příliš**správce společnosti**.</span><span class="sxs-lookup"><span data-stu-id="b5832-227">Go too**Company Admin**.</span></span>

3. <span data-ttu-id="b5832-228">V levém navigačním podokně hello, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b5832-228">On hello left navigation pane, click **Settings**.</span></span>

4. <span data-ttu-id="b5832-229">Proveďte následující kroky hello **synchronizace uživatele** části.</span><span class="sxs-lookup"><span data-stu-id="b5832-229">Perform hello following steps on **User Sync** section.</span></span>
   
   <span data-ttu-id="b5832-230">![Nový uživatel](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="b5832-230">![New User](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "New User")</span></span>
   
   <span data-ttu-id="b5832-231">a.</span><span class="sxs-lookup"><span data-stu-id="b5832-231">a.</span></span> <span data-ttu-id="b5832-232">Jako **povolená synchronizace**, vyberte **ON**.</span><span class="sxs-lookup"><span data-stu-id="b5832-232">As **Sync Enabled**, select **ON**.</span></span>
   
   <span data-ttu-id="b5832-233">b.</span><span class="sxs-lookup"><span data-stu-id="b5832-233">b.</span></span> <span data-ttu-id="b5832-234">Jako **zprostředkovatele synchronizace zvolte**, vyberte **Microsoft nebo Office 365**.</span><span class="sxs-lookup"><span data-stu-id="b5832-234">As **Choose sync provider**, select **Microsoft / Office 365**.</span></span>
   
   <span data-ttu-id="b5832-235">c.</span><span class="sxs-lookup"><span data-stu-id="b5832-235">c.</span></span> <span data-ttu-id="b5832-236">Klikněte na tlačítko **spustíte synchronizaci uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="b5832-236">Click **Run User Sync**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b5832-237">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b5832-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b5832-238">V této části povolíte tak, že udělíte přístup tooRecognize toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b5832-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRecognize.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b5832-240">**tooassign Britta Simon tooRecognize, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b5832-240">**tooassign Britta Simon tooRecognize, perform hello following steps:**</span></span>

1. <span data-ttu-id="b5832-241">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b5832-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b5832-243">V seznamu aplikace hello vyberte **rozpoznávat**.</span><span class="sxs-lookup"><span data-stu-id="b5832-243">In hello applications list, select **Recognize**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_app.png) 

3. <span data-ttu-id="b5832-245">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b5832-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b5832-247">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5832-247">Click **Add** button.</span></span> <span data-ttu-id="b5832-248">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b5832-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b5832-250">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b5832-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b5832-251">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b5832-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b5832-252">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b5832-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b5832-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b5832-253">Testing single sign-on</span></span>

<span data-ttu-id="b5832-254">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b5832-254">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b5832-255">Když kliknete na dlaždici rozpoznávat hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour rozpoznávat aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5832-255">When you click hello Recognize tile in hello Access Panel, you should get automatically signed-on tooyour Recognize application.</span></span> <span data-ttu-id="b5832-256">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b5832-256">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5832-257">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b5832-257">Additional resources</span></span>

* [<span data-ttu-id="b5832-258">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b5832-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b5832-259">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b5832-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png

