---
title: 'Kurz: Azure Active Directory integrace s EverBridge | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a EverBridge."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: a260298279407ed709bc2e685a104410f9836a74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a><span data-ttu-id="9c314-103">Kurz: Azure Active Directory integrace s EverBridge</span><span class="sxs-lookup"><span data-stu-id="9c314-103">Tutorial: Azure Active Directory integration with EverBridge</span></span>

<span data-ttu-id="9c314-104">V tomto kurzu zjistíte, jak toointegrate EverBridge s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9c314-104">In this tutorial, you learn how toointegrate EverBridge with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9c314-105">Integrace EverBridge s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9c314-105">Integrating EverBridge with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9c314-106">Můžete řídit ve službě Azure AD, který má přístup tooEverBridge</span><span class="sxs-lookup"><span data-stu-id="9c314-106">You can control in Azure AD who has access tooEverBridge</span></span>
- <span data-ttu-id="9c314-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooEverBridge (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c314-107">You can enable your users tooautomatically get signed-on tooEverBridge (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9c314-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9c314-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9c314-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9c314-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c314-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9c314-110">Prerequisites</span></span>

<span data-ttu-id="9c314-111">Integrace služby Azure AD s EverBridge tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9c314-111">tooconfigure Azure AD integration with EverBridge, you need hello following items:</span></span>

- <span data-ttu-id="9c314-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c314-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9c314-113">EverBridge jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9c314-113">An EverBridge single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9c314-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c314-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9c314-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9c314-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9c314-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9c314-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9c314-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9c314-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9c314-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9c314-118">Scenario description</span></span>
<span data-ttu-id="9c314-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c314-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9c314-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9c314-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9c314-121">Přidání EverBridge z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9c314-121">Adding EverBridge from hello gallery</span></span>
2. <span data-ttu-id="9c314-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c314-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-everbridge-from-hello-gallery"></a><span data-ttu-id="9c314-123">Přidání EverBridge z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9c314-123">Adding EverBridge from hello gallery</span></span>
<span data-ttu-id="9c314-124">tooconfigure hello integrace EverBridge do Azure AD, je nutné tooadd EverBridge hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9c314-124">tooconfigure hello integration of EverBridge into Azure AD, you need tooadd EverBridge from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9c314-125">**tooadd EverBridge z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9c314-125">**tooadd EverBridge from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9c314-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9c314-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9c314-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9c314-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9c314-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9c314-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9c314-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c314-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9c314-133">Hello vyhledávacího pole zadejte **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="9c314-133">In hello search box, type **EverBridge**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_search.png)

5. <span data-ttu-id="9c314-135">Na panelu výsledků hello vyberte **EverBridge**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c314-135">In hello results panel, select **EverBridge**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9c314-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c314-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9c314-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s EverBridge podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9c314-138">In this section, you configure and test Azure AD single sign-on with EverBridge based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9c314-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v EverBridge je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c314-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in EverBridge is tooa user in Azure AD.</span></span> <span data-ttu-id="9c314-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v EverBridge musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9c314-140">In other words, a link relationship between an Azure AD user and hello related user in EverBridge needs toobe established.</span></span>

<span data-ttu-id="9c314-141">V EverBridge, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="9c314-141">In EverBridge, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9c314-142">tooconfigure a testu Azure AD jednotné přihlašování s EverBridge, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9c314-142">tooconfigure and test Azure AD single sign-on with EverBridge, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9c314-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9c314-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9c314-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c314-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9c314-145">**[Vytváření testovacího uživatele EverBridge](#creating-an-everbridge-test-user)**  -toohave protějšek Britta Simon v EverBridge, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c314-145">**[Creating an EverBridge test user](#creating-an-everbridge-test-user)** - toohave a counterpart of Britta Simon in EverBridge that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9c314-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9c314-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9c314-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9c314-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9c314-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c314-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9c314-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci EverBridge.</span><span class="sxs-lookup"><span data-stu-id="9c314-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your EverBridge application.</span></span>

<span data-ttu-id="9c314-150">**tooconfigure Azure AD jednotné přihlašování s EverBridge, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9c314-150">**tooconfigure Azure AD single sign-on with EverBridge, perform hello following steps:**</span></span>

1. <span data-ttu-id="9c314-151">V portálu Azure, na hello hello **EverBridge** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9c314-151">In hello Azure portal, on hello **EverBridge** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9c314-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9c314-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_samlbase.png)

3. <span data-ttu-id="9c314-155">Na hello **EverBridge domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9c314-155">On hello **EverBridge Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_url.png)

    <span data-ttu-id="9c314-157">a.</span><span class="sxs-lookup"><span data-stu-id="9c314-157">a.</span></span> <span data-ttu-id="9c314-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://sso.everbridge.net/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="9c314-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://sso.everbridge.net/<companyname>`</span></span>

    <span data-ttu-id="9c314-159">b.</span><span class="sxs-lookup"><span data-stu-id="9c314-159">b.</span></span> <span data-ttu-id="9c314-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span><span class="sxs-lookup"><span data-stu-id="9c314-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9c314-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9c314-161">These values are not real.</span></span> <span data-ttu-id="9c314-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9c314-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="9c314-163">Obraťte se na [tým podpory EverBridge](mailto:support@everbridge.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9c314-163">Contact [EverBridge support team](mailto:support@everbridge.com) tooget these values.</span></span>
 
4. <span data-ttu-id="9c314-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9c314-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_certificate.png) 

5. <span data-ttu-id="9c314-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c314-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9c314-168">Na hello **EverBridge konfigurace** klikněte na tlačítko **konfigurace EverBridge** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9c314-168">On hello **EverBridge Configuration** section, click **Configure EverBridge** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9c314-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9c314-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_configure.png) 

6. <span data-ttu-id="9c314-171">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte klienta Everbridge toosign na tooyour jako správce.</span><span class="sxs-lookup"><span data-stu-id="9c314-171">tooget SSO configured for your application, you need toosign-on tooyour Everbridge tenant as an administrator.</span></span>

7. <span data-ttu-id="9c314-172">V nabídce hello hello nahoře, klikněte na tlačítko hello **nastavení** a vyberte **jednotné přihlašování** pod **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="9c314-172">In hello menu on hello top, click hello **Settings** tab and select **Single Sign-On** under **Security**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)
   
    <span data-ttu-id="9c314-174">a.</span><span class="sxs-lookup"><span data-stu-id="9c314-174">a.</span></span> <span data-ttu-id="9c314-175">V hello **název** textovému poli, název typu hello identifikátor zprostředkovatele (například: název vaší společnosti).</span><span class="sxs-lookup"><span data-stu-id="9c314-175">In hello **Name** textbox, type hello name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="9c314-176">b.</span><span class="sxs-lookup"><span data-stu-id="9c314-176">b.</span></span> <span data-ttu-id="9c314-177">V hello **název rozhraní API** textovému poli, název typu hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9c314-177">In hello **API Name** textbox, type hello name of API.</span></span>
   
    <span data-ttu-id="9c314-178">c.</span><span class="sxs-lookup"><span data-stu-id="9c314-178">c.</span></span> <span data-ttu-id="9c314-179">Klikněte na tlačítko **zvolit soubor** tlačítko tooupload hello metadata souboru, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9c314-179">Click **Choose File** button tooupload hello metadata file which you downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="9c314-180">d.</span><span class="sxs-lookup"><span data-stu-id="9c314-180">d.</span></span> <span data-ttu-id="9c314-181">V hello umístění Identity SAML, vyberte **identita je v elementu NameIdentifier hello hello subjektu příkaz**.</span><span class="sxs-lookup"><span data-stu-id="9c314-181">In hello SAML Identity Location, select **Identity is in hello NameIdentifier element of hello Subject statement**.</span></span>
   
    <span data-ttu-id="9c314-182">e.</span><span class="sxs-lookup"><span data-stu-id="9c314-182">e.</span></span> <span data-ttu-id="9c314-183">V hello **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, hodnota hello vložte adresu URL pro jednotné přihlašování SAML z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c314-183">In hello **Identity Provider Login URL** textbox, paste hello value of SAML SSO URL from Azure AD.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)
   
    <span data-ttu-id="9c314-185">f.</span><span class="sxs-lookup"><span data-stu-id="9c314-185">f.</span></span> <span data-ttu-id="9c314-186">V hello služby iniciované žádosti vazby poskytovatele, vyberte **přesměrování protokolu HTTP**.</span><span class="sxs-lookup"><span data-stu-id="9c314-186">In hello Service Provider Initiated Request Binding, select **HTTP Redirect**.</span></span>

    <span data-ttu-id="9c314-187">g.</span><span class="sxs-lookup"><span data-stu-id="9c314-187">g.</span></span> <span data-ttu-id="9c314-188">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="9c314-188">Click **Save**</span></span>

> [!TIP]
> <span data-ttu-id="9c314-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="9c314-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9c314-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="9c314-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9c314-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9c314-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9c314-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c314-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="9c314-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="9c314-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9c314-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9c314-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9c314-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9c314-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9c314-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c314-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9c314-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="9c314-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9c314-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c314-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9c314-204">a.</span><span class="sxs-lookup"><span data-stu-id="9c314-204">a.</span></span> <span data-ttu-id="9c314-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9c314-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9c314-206">b.</span><span class="sxs-lookup"><span data-stu-id="9c314-206">b.</span></span> <span data-ttu-id="9c314-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9c314-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9c314-208">c.</span><span class="sxs-lookup"><span data-stu-id="9c314-208">c.</span></span> <span data-ttu-id="9c314-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9c314-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9c314-210">d.</span><span class="sxs-lookup"><span data-stu-id="9c314-210">d.</span></span> <span data-ttu-id="9c314-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9c314-211">Click **Create**.</span></span>
 
### <a name="creating-an-everbridge-test-user"></a><span data-ttu-id="9c314-212">Vytváření testovacího uživatele EverBridge</span><span class="sxs-lookup"><span data-stu-id="9c314-212">Creating an EverBridge test user</span></span>

<span data-ttu-id="9c314-213">V této části vytvoříte volal Britta Simon v Everbridge uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c314-213">In this section, you create a user called Britta Simon in Everbridge.</span></span> <span data-ttu-id="9c314-214">Práce s [tým podpory EverBridge](mailto:support@everbridge.com) tooadd hello uživatelé v platformě Everbridge hello.</span><span class="sxs-lookup"><span data-stu-id="9c314-214">Work with [EverBridge support team](mailto:support@everbridge.com) tooadd hello users in hello Everbridge platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9c314-215">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9c314-215">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9c314-216">V této části povolíte tak, že udělíte přístup tooEverBridge toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9c314-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEverBridge.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9c314-218">**tooassign Britta Simon tooEverBridge, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9c314-218">**tooassign Britta Simon tooEverBridge, perform hello following steps:**</span></span>

1. <span data-ttu-id="9c314-219">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9c314-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9c314-221">V seznamu aplikace hello vyberte **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="9c314-221">In hello applications list, select **EverBridge**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_app.png) 

3. <span data-ttu-id="9c314-223">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9c314-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9c314-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c314-225">Click **Add** button.</span></span> <span data-ttu-id="9c314-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c314-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9c314-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9c314-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9c314-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c314-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9c314-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c314-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9c314-231">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c314-231">Testing single sign-on</span></span>

<span data-ttu-id="9c314-232">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9c314-232">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9c314-233">Když kliknete na dlaždici Everbridge hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Everbridge aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c314-233">When you click hello Everbridge tile in hello Access Panel, you should get automatically signed-on tooyour Everbridge application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c314-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9c314-234">Additional resources</span></span>

* [<span data-ttu-id="9c314-235">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c314-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9c314-236">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9c314-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png

