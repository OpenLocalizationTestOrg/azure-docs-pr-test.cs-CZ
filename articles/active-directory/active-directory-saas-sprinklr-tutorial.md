---
title: 'Kurz: Azure Active Directory integrace s Sprinklr | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Sprinklr."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 14b467c72d4a453ed7ad248eadcdade710f105af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a><span data-ttu-id="38b3e-103">Kurz: Azure Active Directory integrace s Sprinklr</span><span class="sxs-lookup"><span data-stu-id="38b3e-103">Tutorial: Azure Active Directory integration with Sprinklr</span></span>

<span data-ttu-id="38b3e-104">V tomto kurzu zjistíte, jak toointegrate Sprinklr s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="38b3e-104">In this tutorial, you learn how toointegrate Sprinklr with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="38b3e-105">Integrace Sprinklr s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="38b3e-105">Integrating Sprinklr with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="38b3e-106">Můžete řídit ve službě Azure AD, který má přístup tooSprinklr</span><span class="sxs-lookup"><span data-stu-id="38b3e-106">You can control in Azure AD who has access tooSprinklr</span></span>
- <span data-ttu-id="38b3e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSprinklr (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="38b3e-107">You can enable your users tooautomatically get signed-on tooSprinklr (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="38b3e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="38b3e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="38b3e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="38b3e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="38b3e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="38b3e-110">Prerequisites</span></span>

<span data-ttu-id="38b3e-111">Integrace služby Azure AD s Sprinklr tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="38b3e-111">tooconfigure Azure AD integration with Sprinklr, you need hello following items:</span></span>

- <span data-ttu-id="38b3e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="38b3e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="38b3e-113">Sprinklr jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="38b3e-113">A Sprinklr single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="38b3e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="38b3e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="38b3e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="38b3e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="38b3e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="38b3e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="38b3e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="38b3e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="38b3e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="38b3e-118">Scenario description</span></span>
<span data-ttu-id="38b3e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="38b3e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="38b3e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="38b3e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="38b3e-121">Přidání Sprinklr z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="38b3e-121">Adding Sprinklr from hello gallery</span></span>
2. <span data-ttu-id="38b3e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="38b3e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sprinklr-from-hello-gallery"></a><span data-ttu-id="38b3e-123">Přidání Sprinklr z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="38b3e-123">Adding Sprinklr from hello gallery</span></span>
<span data-ttu-id="38b3e-124">tooconfigure hello integrace Sprinklr do Azure AD, je nutné tooadd Sprinklr hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="38b3e-124">tooconfigure hello integration of Sprinklr into Azure AD, you need tooadd Sprinklr from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="38b3e-125">**tooadd Sprinklr z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="38b3e-125">**tooadd Sprinklr from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="38b3e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="38b3e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="38b3e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="38b3e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="38b3e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="38b3e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="38b3e-133">Hello vyhledávacího pole zadejte **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-133">In hello search box, type **Sprinklr**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. <span data-ttu-id="38b3e-135">Na panelu výsledků hello vyberte **Sprinklr**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="38b3e-135">In hello results panel, select **Sprinklr**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="38b3e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="38b3e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="38b3e-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sprinklr podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="38b3e-138">In this section, you configure and test Azure AD single sign-on with Sprinklr based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="38b3e-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Sprinklr je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38b3e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Sprinklr is tooa user in Azure AD.</span></span> <span data-ttu-id="38b3e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Sprinklr musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="38b3e-140">In other words, a link relationship between an Azure AD user and hello related user in Sprinklr needs toobe established.</span></span>

<span data-ttu-id="38b3e-141">V Sprinklr, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="38b3e-141">In Sprinklr, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="38b3e-142">tooconfigure a testu Azure AD jednotné přihlašování s Sprinklr, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="38b3e-142">tooconfigure and test Azure AD single sign-on with Sprinklr, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="38b3e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="38b3e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="38b3e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="38b3e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="38b3e-145">**[Vytvoření zkušebního uživatele Sprinklr](#creating-a-sprinklr-test-user)**  -toohave protějšek Britta Simon v Sprinklr, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="38b3e-145">**[Creating a Sprinklr test user](#creating-a-sprinklr-test-user)** - toohave a counterpart of Britta Simon in Sprinklr that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="38b3e-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="38b3e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="38b3e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="38b3e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="38b3e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="38b3e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="38b3e-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Sprinklr.</span><span class="sxs-lookup"><span data-stu-id="38b3e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Sprinklr application.</span></span>

<span data-ttu-id="38b3e-150">**tooconfigure Azure AD jednotné přihlašování s Sprinklr, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="38b3e-150">**tooconfigure Azure AD single sign-on with Sprinklr, perform hello following steps:**</span></span>

1. <span data-ttu-id="38b3e-151">V portálu Azure, na hello hello **Sprinklr** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-151">In hello Azure portal, on hello **Sprinklr** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="38b3e-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="38b3e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. <span data-ttu-id="38b3e-155">Na hello **Sprinklr domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="38b3e-155">On hello **Sprinklr Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    <span data-ttu-id="38b3e-157">a.</span><span class="sxs-lookup"><span data-stu-id="38b3e-157">a.</span></span> <span data-ttu-id="38b3e-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="38b3e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    <span data-ttu-id="38b3e-159">b.</span><span class="sxs-lookup"><span data-stu-id="38b3e-159">b.</span></span> <span data-ttu-id="38b3e-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="38b3e-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="38b3e-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="38b3e-161">These values are not real.</span></span> <span data-ttu-id="38b3e-162">Aktualizujte hodnotu hello s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="38b3e-162">Update hello value with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="38b3e-163">Obraťte se na [tým podpory Sprinklr klienta](https://www.sprinklr.com/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="38b3e-163">Contact [Sprinklr Client support team](https://www.sprinklr.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="38b3e-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="38b3e-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. <span data-ttu-id="38b3e-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="38b3e-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="38b3e-168">Na hello **Sprinklr konfigurace** klikněte na tlačítko **konfigurace Sprinklr** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="38b3e-168">On hello **Sprinklr Configuration** section, click **Configure Sprinklr** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="38b3e-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="38b3e-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

7. <span data-ttu-id="38b3e-170">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti Sprinklr tooyour.</span><span class="sxs-lookup"><span data-stu-id="38b3e-170">In a different web browser window, log in tooyour Sprinklr company site as an administrator.</span></span>

8. <span data-ttu-id="38b3e-171">Přejděte příliš**správy \> nastavení**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-171">Go too**Administration \> Settings**.</span></span>
   
    <span data-ttu-id="38b3e-172">![Správa](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "správy")</span><span class="sxs-lookup"><span data-stu-id="38b3e-172">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

9. <span data-ttu-id="38b3e-173">Přejděte příliš**spravovat partnera \> jednotné přihlašování** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="38b3e-173">Go too**Manage Partner \> Single Sign** on from hello left pane.</span></span>
   
    <span data-ttu-id="38b3e-174">![Správa partnera](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "spravovat partnera")</span><span class="sxs-lookup"><span data-stu-id="38b3e-174">![Manage Partner](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Manage Partner")</span></span>

10. <span data-ttu-id="38b3e-175">Klikněte na tlačítko **+ jednotné přihlašování, doplňky**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-175">Click **+Add Single Sign Ons**.</span></span>
   
    <span data-ttu-id="38b3e-176">![Jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="38b3e-176">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "Single Sign-Ons")</span></span>

11. <span data-ttu-id="38b3e-177">Na hello **jednotného přihlašování** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="38b3e-177">On hello **Single Sign on** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="38b3e-178">![Jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="38b3e-178">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "Single Sign-Ons")</span></span>

    <span data-ttu-id="38b3e-179">a.</span><span class="sxs-lookup"><span data-stu-id="38b3e-179">a.</span></span> <span data-ttu-id="38b3e-180">V hello **název** textovému poli, zadejte název pro svou konfiguraci (například: *WAADSSOTest*).</span><span class="sxs-lookup"><span data-stu-id="38b3e-180">In hello **Name** textbox, type a name for your configuration (for example: *WAADSSOTest*).</span></span>

    <span data-ttu-id="38b3e-181">b.</span><span class="sxs-lookup"><span data-stu-id="38b3e-181">b.</span></span> <span data-ttu-id="38b3e-182">Vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-182">Select **Enabled**.</span></span>

    <span data-ttu-id="38b3e-183">c.</span><span class="sxs-lookup"><span data-stu-id="38b3e-183">c.</span></span> <span data-ttu-id="38b3e-184">Vyberte **používat nový certifikát jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-184">Select **Use new SSO Certificate**.</span></span>
             
    <span data-ttu-id="38b3e-185">e.</span><span class="sxs-lookup"><span data-stu-id="38b3e-185">e.</span></span> <span data-ttu-id="38b3e-186">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="38b3e-186">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="38b3e-187">f.</span><span class="sxs-lookup"><span data-stu-id="38b3e-187">f.</span></span> <span data-ttu-id="38b3e-188">Vložení hello **SAML Entity ID** hodnotu, která jste zkopírovali z portálu Azure do hello **Entity Id** textové pole.</span><span class="sxs-lookup"><span data-stu-id="38b3e-188">Paste hello **SAML Entity ID** value which you have copied from Azure Portal into hello **Entity Id** textbox.</span></span>

    <span data-ttu-id="38b3e-189">g.</span><span class="sxs-lookup"><span data-stu-id="38b3e-189">g.</span></span> <span data-ttu-id="38b3e-190">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu, která jste zkopírovali z portálu Azure do hello **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="38b3e-190">Paste hello **SAML Single Sign-On Service URL** value which you have copied from Azure Portal into hello **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="38b3e-191">h.</span><span class="sxs-lookup"><span data-stu-id="38b3e-191">h.</span></span> <span data-ttu-id="38b3e-192">Vložení hello **Sign-Out URL** hodnotu, která jste zkopírovali z portálu Azure do hello **adresa URL odhlašovací zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="38b3e-192">Paste hello **Sign-Out URL** value which you have copied from Azure Portal into hello **Identity Provider Logout URL** textbox.</span></span>
     
    <span data-ttu-id="38b3e-193">i.</span><span class="sxs-lookup"><span data-stu-id="38b3e-193">i.</span></span> <span data-ttu-id="38b3e-194">Jako **typ ID uživatele SAML**, vyberte **kontrolní výraz obsahuje uživatele "uživatelské jméno s sprinklr.com**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-194">As **SAML User ID Type**, select **Assertion contains User”s sprinklr.com username**.</span></span>

    <span data-ttu-id="38b3e-195">j.</span><span class="sxs-lookup"><span data-stu-id="38b3e-195">j.</span></span> <span data-ttu-id="38b3e-196">Jako **umístění ID uživatele SAML**, vyberte **ID uživatele je v elementu identifikátor jména hello hello subjektu příkaz**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-196">As **SAML User ID Location**, select **User ID is in hello Name Identifier element of hello Subject statement**.</span></span>

    <span data-ttu-id="38b3e-197">kB.</span><span class="sxs-lookup"><span data-stu-id="38b3e-197">k.</span></span> <span data-ttu-id="38b3e-198">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-198">Click **Save**.</span></span>
       
    <span data-ttu-id="38b3e-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="38b3e-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span></span>

> [!TIP]
> <span data-ttu-id="38b3e-200">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="38b3e-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="38b3e-201">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="38b3e-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="38b3e-202">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="38b3e-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="38b3e-203">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="38b3e-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="38b3e-204">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="38b3e-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="38b3e-206">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="38b3e-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="38b3e-207">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="38b3e-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="38b3e-209">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="38b3e-211">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="38b3e-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="38b3e-213">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="38b3e-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="38b3e-215">a.</span><span class="sxs-lookup"><span data-stu-id="38b3e-215">a.</span></span> <span data-ttu-id="38b3e-216">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="38b3e-217">b.</span><span class="sxs-lookup"><span data-stu-id="38b3e-217">b.</span></span> <span data-ttu-id="38b3e-218">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="38b3e-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="38b3e-219">c.</span><span class="sxs-lookup"><span data-stu-id="38b3e-219">c.</span></span> <span data-ttu-id="38b3e-220">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="38b3e-221">d.</span><span class="sxs-lookup"><span data-stu-id="38b3e-221">d.</span></span> <span data-ttu-id="38b3e-222">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-222">Click **Create**.</span></span>
 
### <a name="creating-a-sprinklr-test-user"></a><span data-ttu-id="38b3e-223">Vytvoření zkušebního uživatele Sprinklr</span><span class="sxs-lookup"><span data-stu-id="38b3e-223">Creating a Sprinklr test user</span></span>

1. <span data-ttu-id="38b3e-224">Přihlaste se tooyour Sprinklr společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="38b3e-224">Log in tooyour Sprinklr company site as an administrator.</span></span>

2. <span data-ttu-id="38b3e-225">Přejděte příliš**správy \> nastavení**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-225">Go too**Administration \> Settings**.</span></span>
   
    <span data-ttu-id="38b3e-226">![Správa](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "správy")</span><span class="sxs-lookup"><span data-stu-id="38b3e-226">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

3. <span data-ttu-id="38b3e-227">Přejděte příliš**spravovat klienta \> uživatelé** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="38b3e-227">Go too**Manage Client \> Users** from hello left pane.</span></span>
   
    <span data-ttu-id="38b3e-228">![Nastavení](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="38b3e-228">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "Settings")</span></span>

4. <span data-ttu-id="38b3e-229">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-229">Click **Add User**.</span></span>
   
    <span data-ttu-id="38b3e-230">![Nastavení](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="38b3e-230">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "Settings")</span></span>

5. <span data-ttu-id="38b3e-231">Na hello **upravit uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="38b3e-231">On hello **Edit user** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="38b3e-232">![Úprava uživatele](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "úpravy uživatele")</span><span class="sxs-lookup"><span data-stu-id="38b3e-232">![Edit user](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Edit user")</span></span> 

    <span data-ttu-id="38b3e-233">a.</span><span class="sxs-lookup"><span data-stu-id="38b3e-233">a.</span></span> <span data-ttu-id="38b3e-234">V hello **e-mailu**, **křestní jméno** a **příjmení** textových polí, informace o typu hello chcete tooprovision účtu uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38b3e-234">In hello **Email**, **First Name** and **Last Name** textboxes, type hello information of an Azure AD user account you want tooprovision.</span></span>

    <span data-ttu-id="38b3e-235">b.</span><span class="sxs-lookup"><span data-stu-id="38b3e-235">b.</span></span> <span data-ttu-id="38b3e-236">Vyberte **heslo zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-236">Select **Password Disabled**.</span></span>

    <span data-ttu-id="38b3e-237">c.</span><span class="sxs-lookup"><span data-stu-id="38b3e-237">c.</span></span> <span data-ttu-id="38b3e-238">Vyberte **jazyk**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-238">Select **Language**.</span></span>

    <span data-ttu-id="38b3e-239">d.</span><span class="sxs-lookup"><span data-stu-id="38b3e-239">d.</span></span> <span data-ttu-id="38b3e-240">Vyberte **typ uživatele**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-240">Select **User Type**.</span></span>

    <span data-ttu-id="38b3e-241">e.</span><span class="sxs-lookup"><span data-stu-id="38b3e-241">e.</span></span> <span data-ttu-id="38b3e-242">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-242">Click **Update**.</span></span>
   
     >[!IMPORTANT]
     ><span data-ttu-id="38b3e-243">**Heslo zakázáno** musí být vybraný tooenable toolog uživatele v pomocí zprostředkovatele Identity.</span><span class="sxs-lookup"><span data-stu-id="38b3e-243">**Password Disabled** must be selected tooenable a user toolog in via an Identity provider.</span></span> 
     
6. <span data-ttu-id="38b3e-244">Přejděte příliš**Role**a poté proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="38b3e-244">Go too**Role**, and then perform hello following steps:</span></span>
   
    <span data-ttu-id="38b3e-245">![Partner role](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner role")</span><span class="sxs-lookup"><span data-stu-id="38b3e-245">![Partner Roles](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner Roles")</span></span>

    <span data-ttu-id="38b3e-246">a.</span><span class="sxs-lookup"><span data-stu-id="38b3e-246">a.</span></span> <span data-ttu-id="38b3e-247">Z hello **globální** seznamu, vyberte **všechny\_oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-247">From hello **Global** list, select **ALL\_Permissions**.</span></span>  

    <span data-ttu-id="38b3e-248">b.</span><span class="sxs-lookup"><span data-stu-id="38b3e-248">b.</span></span> <span data-ttu-id="38b3e-249">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-249">Click **Update**.</span></span>

>[!NOTE]
><span data-ttu-id="38b3e-250">Můžete použít všechny ostatní Sprinklr uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Sprinklr tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38b3e-250">You can use any other Sprinklr user account creation tools or APIs provided by Sprinklr tooprovision Azure AD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="38b3e-251">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="38b3e-251">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="38b3e-252">V této části povolíte tak, že udělíte přístup tooSprinklr toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="38b3e-252">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSprinklr.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="38b3e-254">**tooassign Britta Simon tooSprinklr, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="38b3e-254">**tooassign Britta Simon tooSprinklr, perform hello following steps:**</span></span>

1. <span data-ttu-id="38b3e-255">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-255">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="38b3e-257">V seznamu aplikace hello vyberte **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-257">In hello applications list, select **Sprinklr**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. <span data-ttu-id="38b3e-259">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="38b3e-259">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="38b3e-261">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="38b3e-261">Click **Add** button.</span></span> <span data-ttu-id="38b3e-262">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="38b3e-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="38b3e-264">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="38b3e-264">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="38b3e-265">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="38b3e-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="38b3e-266">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="38b3e-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="38b3e-267">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="38b3e-267">Testing single sign-on</span></span>

<span data-ttu-id="38b3e-268">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="38b3e-268">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="38b3e-269">Po kliknutí na tlačítko hello Sprinklr dlaždice v hello přístupového panelu, můžete získat aplikaci Sprinklr automaticky přihlášeného tooyour Další informace o hello přístupového panelu, najdete v části [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="38b3e-269">When you click hello Sprinklr tile in hello Access Panel, you should get automatically signed-on tooyour Sprinklr application For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="38b3e-270">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="38b3e-270">Additional resources</span></span>

* [<span data-ttu-id="38b3e-271">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="38b3e-271">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="38b3e-272">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="38b3e-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

