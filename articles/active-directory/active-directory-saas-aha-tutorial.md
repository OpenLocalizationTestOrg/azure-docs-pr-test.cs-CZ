---
title: 'Kurz: Azure Active Directory integrace s Aha! | Dokumentace Microsoftu'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Aha!."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: d844db3c0a035560e6fb275017215171743fba56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a><span data-ttu-id="e424e-104">Kurz: Azure Active Directory integrace s Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-104">Tutorial: Azure Active Directory integration with Aha!</span></span>

<span data-ttu-id="e424e-105">V tomto kurzu zjistíte, jak toointegrate Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-105">In this tutorial, you learn how toointegrate Aha!</span></span> <span data-ttu-id="e424e-106">s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e424e-106">with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e424e-107">Integrace Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-107">Integrating Aha!</span></span> <span data-ttu-id="e424e-108">s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e424e-108">with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e424e-109">Můžete řídit ve službě Azure AD, který má přístup tooAha!</span><span class="sxs-lookup"><span data-stu-id="e424e-109">You can control in Azure AD who has access tooAha!</span></span>
- <span data-ttu-id="e424e-110">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAha!</span><span class="sxs-lookup"><span data-stu-id="e424e-110">You can enable your users tooautomatically get signed-on tooAha!</span></span> <span data-ttu-id="e424e-111">(Jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e424e-111">(Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e424e-112">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e424e-112">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e424e-113">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e424e-113">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e424e-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e424e-114">Prerequisites</span></span>

<span data-ttu-id="e424e-115">tooconfigure integrace Azure AD s Aha!, je nutné hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e424e-115">tooconfigure Azure AD integration with Aha!, you need hello following items:</span></span>

- <span data-ttu-id="e424e-116">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e424e-116">An Azure AD subscription</span></span>
- <span data-ttu-id="e424e-117">Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-117">An Aha!</span></span> <span data-ttu-id="e424e-118">jednotné přihlašování v předplatném povolené</span><span class="sxs-lookup"><span data-stu-id="e424e-118">single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e424e-119">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e424e-119">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e424e-120">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e424e-120">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e424e-121">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e424e-121">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e424e-122">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e424e-122">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e424e-123">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e424e-123">Scenario description</span></span>
<span data-ttu-id="e424e-124">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e424e-124">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e424e-125">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e424e-125">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e424e-126">Přidání Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-126">Adding Aha!</span></span> <span data-ttu-id="e424e-127">z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e424e-127">from hello gallery</span></span>
2. <span data-ttu-id="e424e-128">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e424e-128">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-aha-from-hello-gallery"></a><span data-ttu-id="e424e-129">Přidání Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-129">Adding Aha!</span></span> <span data-ttu-id="e424e-130">z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e424e-130">from hello gallery</span></span>
<span data-ttu-id="e424e-131">integrace hello tooconfigure Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-131">tooconfigure hello integration of Aha!</span></span> <span data-ttu-id="e424e-132">do služby Azure AD je nutné tooadd Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-132">into Azure AD, you need tooadd Aha!</span></span> <span data-ttu-id="e424e-133">hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e424e-133">from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e424e-134">**tooadd Aha! z Galerie hello proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e424e-134">**tooadd Aha! from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e424e-135">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e424e-135">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e424e-137">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e424e-137">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e424e-138">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e424e-138">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e424e-140">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e424e-140">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e424e-142">Hello vyhledávacího pole zadejte **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="e424e-142">In hello search box, type **Aha!**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/tutorial_aha_search.png)

5. <span data-ttu-id="e424e-144">Na panelu výsledků hello vyberte **Aha!**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e424e-144">In hello results panel, select **Aha!**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e424e-146">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e424e-146">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e424e-147">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-147">In this section, you configure and test Azure AD single sign-on with Aha!</span></span> <span data-ttu-id="e424e-148">na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="e424e-148">based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e424e-149">Azure AD pro toowork jeden přihlašování, musí tooknow hello příslušného uživatele v Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-149">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Aha!</span></span> <span data-ttu-id="e424e-150">je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e424e-150">is tooa user in Azure AD.</span></span> <span data-ttu-id="e424e-151">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-151">In other words, a link relationship between an Azure AD user and hello related user in Aha!</span></span> <span data-ttu-id="e424e-152">musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="e424e-152">needs toobe established.</span></span>

<span data-ttu-id="e424e-153">V Aha!, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="e424e-153">In Aha!, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e424e-154">tooconfigure a testování Azure AD jednotné přihlašování s Aha!, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="e424e-154">tooconfigure and test Azure AD single sign-on with Aha!, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e424e-155">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="e424e-155">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e424e-156">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e424e-156">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e424e-157">**[Vytvoření Aha! testovací uživatel](#creating-an-aha-test-user)**  -toohave protějšek Britta Simon v Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-157">**[Creating an Aha! test user](#creating-an-aha-test-user)** - toohave a counterpart of Britta Simon in Aha!</span></span> <span data-ttu-id="e424e-158">To je propojené toohello reprezentace uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e424e-158">that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e424e-159">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e424e-159">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e424e-160">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="e424e-160">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e424e-161">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e424e-161">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e424e-162">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-162">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Aha!</span></span> <span data-ttu-id="e424e-163">Aplikace.</span><span class="sxs-lookup"><span data-stu-id="e424e-163">application.</span></span>

<span data-ttu-id="e424e-164">**tooconfigure Azure AD jednotné přihlašování s Aha!, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e424e-164">**tooconfigure Azure AD single sign-on with Aha!, perform hello following steps:**</span></span>

1. <span data-ttu-id="e424e-165">V portálu Azure, na hello hello **Aha!**</span><span class="sxs-lookup"><span data-stu-id="e424e-165">In hello Azure portal, on hello **Aha!**</span></span> <span data-ttu-id="e424e-166">Stránka integrace aplikace, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e424e-166">application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e424e-168">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e424e-168">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_aha_samlbase.png)

3. <span data-ttu-id="e424e-170">Na hello **Aha! Domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e424e-170">On hello **Aha! Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_aha_url.png)

    <span data-ttu-id="e424e-172">a.</span><span class="sxs-lookup"><span data-stu-id="e424e-172">a.</span></span> <span data-ttu-id="e424e-173">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.aha.io/session/new`</span><span class="sxs-lookup"><span data-stu-id="e424e-173">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.aha.io/session/new`</span></span>

    <span data-ttu-id="e424e-174">b.</span><span class="sxs-lookup"><span data-stu-id="e424e-174">b.</span></span> <span data-ttu-id="e424e-175">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.aha.io`</span><span class="sxs-lookup"><span data-stu-id="e424e-175">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.aha.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e424e-176">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e424e-176">These values are not real.</span></span> <span data-ttu-id="e424e-177">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e424e-177">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e424e-178">Obraťte se na [Aha! Tým podpory klienta](https://www.aha.io/company/contact) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e424e-178">Contact [Aha! Client support team](https://www.aha.io/company/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="e424e-179">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e424e-179">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_aha_certificate.png) 

5. <span data-ttu-id="e424e-181">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e424e-181">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e424e-183">V okně prohlížeče jiný web Přihlaste se tooyour Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-183">In a different web browser window, log in tooyour Aha!</span></span> <span data-ttu-id="e424e-184">web společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="e424e-184">company site as an administrator.</span></span>

7. <span data-ttu-id="e424e-185">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="e424e-185">In hello menu on hello top, click **Settings**.</span></span>

    <span data-ttu-id="e424e-186">![Nastavení](./media/active-directory-saas-aha-tutorial/IC798950.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="e424e-186">![Settings](./media/active-directory-saas-aha-tutorial/IC798950.png "Settings")</span></span>

8. <span data-ttu-id="e424e-187">Klikněte na tlačítko **účet**.</span><span class="sxs-lookup"><span data-stu-id="e424e-187">Click **Account**.</span></span>
   
    <span data-ttu-id="e424e-188">![Profil](./media/active-directory-saas-aha-tutorial/IC798951.png "profilu")</span><span class="sxs-lookup"><span data-stu-id="e424e-188">![Profile](./media/active-directory-saas-aha-tutorial/IC798951.png "Profile")</span></span>

9. <span data-ttu-id="e424e-189">Klikněte na tlačítko **zabezpečení a jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e424e-189">Click **Security and single sign-on**.</span></span>
   
    <span data-ttu-id="e424e-190">![Zabezpečení a jednotné přihlašování](./media/active-directory-saas-aha-tutorial/IC798952.png "zabezpečení a jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e424e-190">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798952.png "Security and single sign-on")</span></span>

10. <span data-ttu-id="e424e-191">V **jednotné přihlašování** části jako **zprostředkovatele Identity**, vyberte **SAML2.0**.</span><span class="sxs-lookup"><span data-stu-id="e424e-191">In **Single Sign-On** section, as **Identity Provider**, select **SAML2.0**.</span></span>
   
    <span data-ttu-id="e424e-192">![Zabezpečení a jednotné přihlašování](./media/active-directory-saas-aha-tutorial/IC798953.png "zabezpečení a jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e424e-192">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798953.png "Security and single sign-on")</span></span>

11. <span data-ttu-id="e424e-193">Na hello **jednotné přihlašování** konfigurace proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e424e-193">On hello **Single Sign-On** configuration page, perform hello following steps:</span></span>
    
    <span data-ttu-id="e424e-194">![Jednotné přihlašování](./media/active-directory-saas-aha-tutorial/IC798954.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e424e-194">![Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798954.png "Single Sign-On")</span></span>
    
       <span data-ttu-id="e424e-195">a.</span><span class="sxs-lookup"><span data-stu-id="e424e-195">a.</span></span> <span data-ttu-id="e424e-196">V hello **název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e424e-196">In hello **Name** textbox, type a name for your configuration.</span></span>

       <span data-ttu-id="e424e-197">b.</span><span class="sxs-lookup"><span data-stu-id="e424e-197">b.</span></span> <span data-ttu-id="e424e-198">Pro **nakonfigurovat**, vyberte **soubor metadat**.</span><span class="sxs-lookup"><span data-stu-id="e424e-198">For **Configure using**, select **Metadata File**.</span></span>
   
       <span data-ttu-id="e424e-199">c.</span><span class="sxs-lookup"><span data-stu-id="e424e-199">c.</span></span> <span data-ttu-id="e424e-200">tooupload váš soubor stažený metadata, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="e424e-200">tooupload your downloaded metadata file, click **Browse**.</span></span>
   
       <span data-ttu-id="e424e-201">d.</span><span class="sxs-lookup"><span data-stu-id="e424e-201">d.</span></span> <span data-ttu-id="e424e-202">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="e424e-202">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="e424e-203">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="e424e-203">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e424e-204">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="e424e-204">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e424e-205">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e424e-205">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e424e-206">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e424e-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="e424e-207">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="e424e-207">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e424e-209">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e424e-209">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e424e-210">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e424e-210">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e424e-212">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e424e-212">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e424e-214">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="e424e-214">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e424e-216">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e424e-216">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e424e-218">a.</span><span class="sxs-lookup"><span data-stu-id="e424e-218">a.</span></span> <span data-ttu-id="e424e-219">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e424e-219">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e424e-220">b.</span><span class="sxs-lookup"><span data-stu-id="e424e-220">b.</span></span> <span data-ttu-id="e424e-221">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e424e-221">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e424e-222">c.</span><span class="sxs-lookup"><span data-stu-id="e424e-222">c.</span></span> <span data-ttu-id="e424e-223">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e424e-223">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e424e-224">d.</span><span class="sxs-lookup"><span data-stu-id="e424e-224">d.</span></span> <span data-ttu-id="e424e-225">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e424e-225">Click **Create**.</span></span>
 
### <a name="creating-an-aha-test-user"></a><span data-ttu-id="e424e-226">Vytvoření Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-226">Creating an Aha!</span></span> <span data-ttu-id="e424e-227">testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="e424e-227">test user</span></span>

<span data-ttu-id="e424e-228">Uživatelé toolog tooenable Azure AD v tooAha!, musí být zřízená do Aha!.</span><span class="sxs-lookup"><span data-stu-id="e424e-228">tooenable Azure AD users toolog in tooAha!, they must be provisioned into Aha!.</span></span>  

<span data-ttu-id="e424e-229">V případě hello Aha!, zřizování je automatizaci úloh.</span><span class="sxs-lookup"><span data-stu-id="e424e-229">In hello case of Aha!, provisioning is an automated task.</span></span> <span data-ttu-id="e424e-230">Neexistuje žádná položka akce za vás.</span><span class="sxs-lookup"><span data-stu-id="e424e-230">There is no action item for you.</span></span>

<span data-ttu-id="e424e-231">Uživatelé jsou automaticky vytvářeny v případě potřeby během hello první jeden přihlašování pokus.</span><span class="sxs-lookup"><span data-stu-id="e424e-231">Users are automatically created if necessary during hello first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="e424e-232">Můžete použít jiné Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-232">You can use any other Aha!</span></span> <span data-ttu-id="e424e-233">Nástroje pro tvorbu účet uživatele nebo rozhraní API poskytovaných Aha!</span><span class="sxs-lookup"><span data-stu-id="e424e-233">user account creation tools or APIs provided by Aha!</span></span> <span data-ttu-id="e424e-234">tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="e424e-234">tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e424e-235">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e424e-235">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e424e-236">V této části povolíte tak, že udělíte přístup tooAha toouse Britta Simon Azure jednotné přihlašování!.</span><span class="sxs-lookup"><span data-stu-id="e424e-236">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAha!.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e424e-238">**tooassign tooAha Britta Simon!, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e424e-238">**tooassign Britta Simon tooAha!, perform hello following steps:**</span></span>

1. <span data-ttu-id="e424e-239">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e424e-239">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e424e-241">V seznamu aplikace hello vyberte **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="e424e-241">In hello applications list, select **Aha!**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_aha_app.png) 

3. <span data-ttu-id="e424e-243">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e424e-243">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e424e-245">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e424e-245">Click **Add** button.</span></span> <span data-ttu-id="e424e-246">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e424e-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e424e-248">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="e424e-248">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e424e-249">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e424e-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e424e-250">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e424e-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e424e-251">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e424e-251">Testing single sign-on</span></span>

<span data-ttu-id="e424e-252">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e424e-252">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="e424e-253">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e424e-253">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e424e-254">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e424e-254">Additional resources</span></span>

* [<span data-ttu-id="e424e-255">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e424e-255">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e424e-256">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e424e-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aha-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aha-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aha-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aha-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aha-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aha-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aha-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aha-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aha-tutorial/tutorial_general_203.png

