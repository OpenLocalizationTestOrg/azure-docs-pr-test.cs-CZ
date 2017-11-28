---
title: 'Kurz: Azure Active Directory integrace s Githubu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Githubu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 688779de4e6627e49c0e3e8a7576f2f8c7a81ab1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a><span data-ttu-id="c8499-103">Kurz: Azure Active Directory integrace s Githubu</span><span class="sxs-lookup"><span data-stu-id="c8499-103">Tutorial: Azure Active Directory integration with GitHub</span></span>

<span data-ttu-id="c8499-104">V tomto kurzu zjistíte, jak toointegrate Githubu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c8499-104">In this tutorial, you learn how toointegrate GitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c8499-105">Integrace Githubu s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c8499-105">Integrating GitHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c8499-106">Můžete řídit ve službě Azure AD, který má přístup tooGitHub</span><span class="sxs-lookup"><span data-stu-id="c8499-106">You can control in Azure AD who has access tooGitHub</span></span>
- <span data-ttu-id="c8499-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooGitHub (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8499-107">You can enable your users tooautomatically get signed-on tooGitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c8499-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="c8499-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="c8499-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c8499-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8499-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c8499-110">Prerequisites</span></span>

<span data-ttu-id="c8499-111">tooconfigure integrace Azure AD s Githubu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c8499-111">tooconfigure Azure AD integration with GitHub, you need hello following items:</span></span>

- <span data-ttu-id="c8499-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8499-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c8499-113">Githubu jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c8499-113">A GitHub single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="c8499-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8499-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="c8499-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c8499-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c8499-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="c8499-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="c8499-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8499-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="c8499-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c8499-118">Scenario description</span></span>
<span data-ttu-id="c8499-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8499-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c8499-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c8499-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c8499-121">Přidání Githubu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c8499-121">Adding GitHub from hello gallery</span></span>
2. <span data-ttu-id="c8499-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c8499-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-github-from-hello-gallery"></a><span data-ttu-id="c8499-123">Přidání Githubu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c8499-123">Adding GitHub from hello gallery</span></span>
<span data-ttu-id="c8499-124">tooconfigure hello integrace Githubu do Azure AD, je nutné tooadd Githubu hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c8499-124">tooconfigure hello integration of GitHub into Azure AD, you need tooadd GitHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c8499-125">**tooadd Githubu z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c8499-125">**tooadd GitHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8499-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c8499-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c8499-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c8499-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c8499-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c8499-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c8499-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c8499-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c8499-133">Hello vyhledávacího pole zadejte **webu GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="c8499-133">In hello search box, type **GitHub.com**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. <span data-ttu-id="c8499-135">Na panelu výsledků hello vyberte **Githubu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8499-135">In hello results panel, select **GitHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c8499-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c8499-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c8499-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Githubu podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c8499-138">In this section, you configure and test Azure AD single sign-on with GitHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c8499-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Githubu je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c8499-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in GitHub is tooa user in Azure AD.</span></span> <span data-ttu-id="c8499-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Githubu musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c8499-140">In other words, a link relationship between an Azure AD user and hello related user in GitHub needs toobe established.</span></span>

<span data-ttu-id="c8499-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Githubu.</span><span class="sxs-lookup"><span data-stu-id="c8499-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in GitHub.</span></span>

<span data-ttu-id="c8499-142">tooconfigure a testu Azure AD jednotné přihlašování s Githubu, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c8499-142">tooconfigure and test Azure AD single sign-on with GitHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c8499-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c8499-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c8499-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8499-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c8499-145">**[Vytvoření zkušebního uživatele Githubu](#creating-a-GitHub-test-user)**  -toohave protějšek Britta Simon v Githubu, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c8499-145">**[Creating a GitHub test user](#creating-a-GitHub-test-user)** - toohave a counterpart of Britta Simon in GitHub that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="c8499-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c8499-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c8499-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c8499-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c8499-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c8499-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c8499-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Githubu.</span><span class="sxs-lookup"><span data-stu-id="c8499-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your GitHub application.</span></span>

<span data-ttu-id="c8499-150">**tooconfigure Azure AD jednotné přihlašování s Githubu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c8499-150">**tooconfigure Azure AD single sign-on with GitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8499-151">V hello Azure Management portal na hello **Githubu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c8499-151">In hello Azure Management portal, on hello **GitHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c8499-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="c8499-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. <span data-ttu-id="c8499-155">Na hello **Githubu domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c8499-155">On hello **GitHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    <span data-ttu-id="c8499-157">a.</span><span class="sxs-lookup"><span data-stu-id="c8499-157">a.</span></span> <span data-ttu-id="c8499-158">V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`https://github.com/orgs/<entity-id>/sso`</span><span class="sxs-lookup"><span data-stu-id="c8499-158">In hello **Sign-on URL** textbox, type hello value as: `https://github.com/orgs/<entity-id>/sso`</span></span>

    <span data-ttu-id="c8499-159">b.</span><span class="sxs-lookup"><span data-stu-id="c8499-159">b.</span></span> <span data-ttu-id="c8499-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://github.com/orgs/<entity-id>`</span><span class="sxs-lookup"><span data-stu-id="c8499-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://github.com/orgs/<entity-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c8499-161">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c8499-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="c8499-162">Máte tooupdate tyto hodnoty s hello skutečné Sing na adresu URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c8499-162">You have tooupdate these values with hello actual Sing-on URL and Identifier.</span></span> <span data-ttu-id="c8499-163">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c8499-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="c8499-164">Přejděte tooGitHub správce části tooretrieve tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c8499-164">Go tooGitHub Admin section tooretrieve these values.</span></span> 

4. <span data-ttu-id="c8499-165">Na hello **uživatelské atributy** vyberte **uživatelský identifikátor** jako user.mail.</span><span class="sxs-lookup"><span data-stu-id="c8499-165">On hello **User Attributes** section, select **User Identifier** as user.mail.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. <span data-ttu-id="c8499-167">Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="c8499-167">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. <span data-ttu-id="c8499-169">Na hello **vytvořit nový certifikát** dialogové okno, klikněte na ikonu hello kalendáře a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="c8499-169">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="c8499-170">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c8499-170">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. <span data-ttu-id="c8499-172">Na hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c8499-172">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. <span data-ttu-id="c8499-174">V místní nabídce hello **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8499-174">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="c8499-176">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c8499-176">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. <span data-ttu-id="c8499-178">Na hello **Githubu konfigurace** klikněte na tlačítko **konfigurace Githubu** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c8499-178">On hello **GitHub Configuration** section, click **Configure GitHub** tooopen **Configure sign-on** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. <span data-ttu-id="c8499-181">V okně prohlížeče jiný web Přihlaste se jako správce k organizace webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="c8499-181">In a different web browser window, log into your GitHub organization site as an administrator.</span></span>

12. <span data-ttu-id="c8499-182">Přejděte příliš**nastavení** a klikněte na tlačítko **zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="c8499-182">Navigate too**Settings** and click **Security**</span></span>

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. <span data-ttu-id="c8499-184">Zkontrolujte hello **ověřování povolit SAML** pole odhalil hello jednotné přihlašování konfigurační pole.</span><span class="sxs-lookup"><span data-stu-id="c8499-184">Check hello **Enable SAML authentication** box, revealing hello Single Sign-on configuration fields.</span></span> <span data-ttu-id="c8499-185">Pak použijte hello jednoho přihlášení adresy URL hodnotu tooupdate hello jeden přihlašování adresu URL v konfiguraci služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c8499-185">Then, use hello single sign-on URL value tooupdate hello Single sign-on URL on Azure AD configuration.</span></span>

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. <span data-ttu-id="c8499-187">Nakonfigurujte hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="c8499-187">Configure hello following fields:</span></span>

    <span data-ttu-id="c8499-188">a.</span><span class="sxs-lookup"><span data-stu-id="c8499-188">a.</span></span> <span data-ttu-id="c8499-189">**Přihlaste na adrese URL**: Zadejte **SAML jednotné přihlašování adresa URL služby** z hello **konfigurace Githubu** části na Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8499-189">**Sign on URL**: Enter **SAML Single sign-on Service URL** from hello **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="c8499-190">b.</span><span class="sxs-lookup"><span data-stu-id="c8499-190">b.</span></span> <span data-ttu-id="c8499-191">**Vystavitel**: Zadejte **SAML Entity ID** z hello **konfigurace Githubu** části na Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8499-191">**Issuer**: Enter **SAML Entity ID** from hello **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="c8499-192">c.</span><span class="sxs-lookup"><span data-stu-id="c8499-192">c.</span></span> <span data-ttu-id="c8499-193">**Veřejný certifikát**: Otevřete hello stáhnout certifikát z Azure AD v programu Poznámkový blok a zkopírujte hello obsah, včetně "Začít certifikát" a "END CERTIFICATE"</span><span class="sxs-lookup"><span data-stu-id="c8499-193">**Public Certificate**: Open hello downloaded certificate from Azure AD in a notepad and copy hello content including "BEGIN CERTIFICATE" and "END CERTIFICATE"</span></span>

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. <span data-ttu-id="c8499-195">Klikněte na **Konfigurace testu SAML** tooconfirm, žádná chyb při ověřování nebo chyby během jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c8499-195">Click on **Test SAML configuration** tooconfirm that no validation failures or errors during SSO.</span></span>

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. <span data-ttu-id="c8499-197">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="c8499-197">Click **Save**</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c8499-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8499-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="c8499-199">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8499-199">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c8499-201">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c8499-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8499-202">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c8499-202">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c8499-204">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c8499-204">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c8499-206">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8499-206">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c8499-208">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c8499-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c8499-210">a.</span><span class="sxs-lookup"><span data-stu-id="c8499-210">a.</span></span> <span data-ttu-id="c8499-211">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c8499-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c8499-212">b.</span><span class="sxs-lookup"><span data-stu-id="c8499-212">b.</span></span> <span data-ttu-id="c8499-213">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c8499-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c8499-214">c.</span><span class="sxs-lookup"><span data-stu-id="c8499-214">c.</span></span> <span data-ttu-id="c8499-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c8499-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c8499-216">d.</span><span class="sxs-lookup"><span data-stu-id="c8499-216">d.</span></span> <span data-ttu-id="c8499-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c8499-217">Click **Create**.</span></span> 


### <a name="creating-a-github-test-user"></a><span data-ttu-id="c8499-218">Vytvoření zkušebního uživatele Githubu</span><span class="sxs-lookup"><span data-stu-id="c8499-218">Creating a GitHub test user</span></span>

<span data-ttu-id="c8499-219">V pořadí tooenable Azure AD Uživatelé toolog do Githubu musí být zřízená na Githubu.</span><span class="sxs-lookup"><span data-stu-id="c8499-219">In order tooenable Azure AD users toolog into GitHub, they must be provisioned into GitHub.</span></span>  
<span data-ttu-id="c8499-220">V případě hello z Githubu zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="c8499-220">In hello case of GitHub, provisioning is a manual task.</span></span>

<span data-ttu-id="c8499-221">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c8499-221">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8499-222">Přihlaste se tooyour Githubu společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="c8499-222">Log in tooyour GitHub company site as an administrator.</span></span>

2. <span data-ttu-id="c8499-223">Klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="c8499-223">Click **People**.</span></span>

    <span data-ttu-id="c8499-224">![Lidé](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="c8499-224">![People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "People")</span></span>

3. <span data-ttu-id="c8499-225">Klikněte na tlačítko **pozvání člen**.</span><span class="sxs-lookup"><span data-stu-id="c8499-225">Click **Invite member**.</span></span>

    <span data-ttu-id="c8499-226">![Uživatele pozvat](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="c8499-226">![Invite Users](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "Invite Users")</span></span>

4. <span data-ttu-id="c8499-227">Na hello **pozvání člen** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c8499-227">On hello **Invite member** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="c8499-228">a.</span><span class="sxs-lookup"><span data-stu-id="c8499-228">a.</span></span> <span data-ttu-id="c8499-229">V hello **e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8499-229">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="c8499-230">![Pozvěte lidi, kteří](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="c8499-230">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "Invite People")</span></span>
    
    <span data-ttu-id="c8499-231">b.</span><span class="sxs-lookup"><span data-stu-id="c8499-231">b.</span></span> <span data-ttu-id="c8499-232">Klikněte na tlačítko **odeslat pozvánky**.</span><span class="sxs-lookup"><span data-stu-id="c8499-232">Click **Send Invitation**.</span></span>

    <span data-ttu-id="c8499-233">![Pozvěte lidi, kteří](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="c8499-233">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "Invite People")</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8499-234">Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="c8499-234">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c8499-235">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c8499-235">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c8499-236">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooGitHub svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="c8499-236">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooGitHub.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c8499-238">**tooassign Britta Simon tooGitHub, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c8499-238">**tooassign Britta Simon tooGitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8499-239">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c8499-239">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c8499-241">V seznamu aplikace hello vyberte **webu GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="c8499-241">In hello applications list, select **GitHub.com**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. <span data-ttu-id="c8499-243">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c8499-243">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c8499-245">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c8499-245">Click **Add** button.</span></span> <span data-ttu-id="c8499-246">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8499-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c8499-248">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c8499-248">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c8499-249">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8499-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c8499-250">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8499-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="c8499-251">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c8499-251">Testing single sign-on</span></span>

<span data-ttu-id="c8499-252">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c8499-252">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c8499-253">Když kliknete na dlaždici Githubu hello v hello přístupového panelu, měli byste obdržet přihlášeného tooyour Githubu aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8499-253">When you click hello GitHub tile in hello Access Panel, you should get signed-on tooyour GitHub application.</span></span> <span data-ttu-id="c8499-254">Pomocí svého osobního účtu, budete mít přihlášení jako účtu organizace, ale pak toolog potřeba.</span><span class="sxs-lookup"><span data-stu-id="c8499-254">You'll be logging in as an Organization account but then need toolog in with your personal account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="c8499-255">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c8499-255">Additional resources</span></span>

* [<span data-ttu-id="c8499-256">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c8499-256">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c8499-257">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c8499-257">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
