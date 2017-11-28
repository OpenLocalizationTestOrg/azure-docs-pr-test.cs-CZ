---
title: "Kurz: Azure Active Directory integrace s Kantega jednotné přihlašování pro soutoku | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Kantega jednotné přihlašování pro soutoku."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: b35eb8757e41e86228a3a9ee10869086cc801c9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a><span data-ttu-id="61596-103">Kurz: Azure Active Directory integrace s Kantega jednotné přihlašování pro soutoku</span><span class="sxs-lookup"><span data-stu-id="61596-103">Tutorial: Azure Active Directory integration with Kantega SSO for Confluence</span></span>

<span data-ttu-id="61596-104">V tomto kurzu zjistíte, jak toointegrate Kantega jednotné přihlašování pro soutoku s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="61596-104">In this tutorial, you learn how toointegrate Kantega SSO for Confluence with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="61596-105">Integrace Kantega jednotné přihlašování pro soutoku s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="61596-105">Integrating Kantega SSO for Confluence with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="61596-106">Můžete řídit ve službě Azure AD, který má přístup tooKantega jednotné přihlašování pro soutoku</span><span class="sxs-lookup"><span data-stu-id="61596-106">You can control in Azure AD who has access tooKantega SSO for Confluence</span></span>
- <span data-ttu-id="61596-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooKantega jednotné přihlašování pro soutoku (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="61596-107">You can enable your users tooautomatically get signed-on tooKantega SSO for Confluence (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="61596-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="61596-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="61596-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="61596-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61596-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="61596-110">Prerequisites</span></span>

<span data-ttu-id="61596-111">Integrace služby Azure AD pomocí jednotného přihlašování Kantega pro soutoku tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="61596-111">tooconfigure Azure AD integration with Kantega SSO for Confluence, you need hello following items:</span></span>

- <span data-ttu-id="61596-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="61596-112">An Azure AD subscription</span></span>
- <span data-ttu-id="61596-113">Předplatné povolené Kantega SSO pro soutoku jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="61596-113">A Kantega SSO for Confluence single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="61596-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="61596-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="61596-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="61596-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="61596-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="61596-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="61596-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="61596-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="61596-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="61596-118">Scenario description</span></span>
<span data-ttu-id="61596-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="61596-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="61596-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="61596-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="61596-121">Přidání Kantega jednotné přihlašování pro soutoku z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="61596-121">Adding Kantega SSO for Confluence from hello gallery</span></span>
2. <span data-ttu-id="61596-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="61596-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-confluence-from-hello-gallery"></a><span data-ttu-id="61596-123">Přidání Kantega jednotné přihlašování pro soutoku z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="61596-123">Adding Kantega SSO for Confluence from hello gallery</span></span>
<span data-ttu-id="61596-124">integrace hello tooconfigure Kantega přihlašování pro soutoku do služby Azure AD, potřebujete pro soutoku hello Galerie tooyour seznamu spravovaných aplikací SaaS tooadd Kantega jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="61596-124">tooconfigure hello integration of Kantega SSO for Confluence into Azure AD, you need tooadd Kantega SSO for Confluence from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="61596-125">**tooadd Kantega jednotné přihlašování pro soutoku z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="61596-125">**tooadd Kantega SSO for Confluence from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="61596-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="61596-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="61596-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="61596-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="61596-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="61596-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="61596-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="61596-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="61596-133">Hello vyhledávacího pole zadejte **Kantega jednotné přihlašování pro soutoku**.</span><span class="sxs-lookup"><span data-stu-id="61596-133">In hello search box, type **Kantega SSO for Confluence**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_search.png)

5. <span data-ttu-id="61596-135">Na panelu výsledků hello vyberte **Kantega jednotné přihlašování pro soutoku**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="61596-135">In hello results panel, select **Kantega SSO for Confluence**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="61596-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="61596-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="61596-138">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro soutoku podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="61596-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for Confluence based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="61596-139">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello Kantega SSO pro soutoku je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="61596-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kantega SSO for Confluence is tooa user in Azure AD.</span></span> <span data-ttu-id="61596-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Kantega SSO pro soutoku musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="61596-140">In other words, a link relationship between an Azure AD user and hello related user in Kantega SSO for Confluence needs toobe established.</span></span>

<span data-ttu-id="61596-141">V Kantega jednotné přihlašování pro soutoku, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="61596-141">In Kantega SSO for Confluence, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="61596-142">tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega soutoku, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="61596-142">tooconfigure and test Azure AD single sign-on with Kantega SSO for Confluence, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="61596-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="61596-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="61596-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="61596-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="61596-145">**[Vytváření Kantega SSO pro testovací uživatele soutoku](#creating-a-kantega-sso-for-confluence-test-user)**  -toohave protějšek Britta Simon Kantega SSO pro soutoku, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="61596-145">**[Creating a Kantega SSO for Confluence test user](#creating-a-kantega-sso-for-confluence-test-user)** - toohave a counterpart of Britta Simon in Kantega SSO for Confluence that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="61596-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="61596-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="61596-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="61596-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="61596-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="61596-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="61596-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší Kantega jednotné přihlašování pro aplikace soutoku.</span><span class="sxs-lookup"><span data-stu-id="61596-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kantega SSO for Confluence application.</span></span>

<span data-ttu-id="61596-150">**tooconfigure Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro soutoku, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="61596-150">**tooconfigure Azure AD single sign-on with Kantega SSO for Confluence, perform hello following steps:**</span></span>

1. <span data-ttu-id="61596-151">V portálu Azure, na hello hello **Kantega jednotné přihlašování pro soutoku** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="61596-151">In hello Azure portal, on hello **Kantega SSO for Confluence** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="61596-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="61596-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_samlbase.png)

3. <span data-ttu-id="61596-155">V **IDP** iniciované režimu na hello **Kantega SSO soutoku domény a adresy URL** části provést následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="61596-155">In **IDP** initiated mode, on hello **Kantega SSO for Confluence Domain and URLs** section perform hello following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url1.png)

    <span data-ttu-id="61596-157">a.</span><span class="sxs-lookup"><span data-stu-id="61596-157">a.</span></span> <span data-ttu-id="61596-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="61596-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="61596-159">b.</span><span class="sxs-lookup"><span data-stu-id="61596-159">b.</span></span> <span data-ttu-id="61596-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="61596-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="61596-161">V **SP** initiated režimu, zkontrolujte **zobrazit upřesňující nastavení adresy URL** a proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="61596-161">In **SP** initiated mode, check **Show advanced URL settings** and perform hello following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url2.png)

    <span data-ttu-id="61596-163">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="61596-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="61596-164">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="61596-164">These values are not real.</span></span> <span data-ttu-id="61596-165">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="61596-165">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="61596-166">Tyto hodnoty jsou přijímány během konfigurace hello soutoku modul plug-in, který je vysvětlen později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="61596-166">These values are received during hello configuration of Confluence plugin, which is explained later in hello tutorial.</span></span>

5. <span data-ttu-id="61596-167">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="61596-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_certificate.png) 

6. <span data-ttu-id="61596-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="61596-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="61596-171">V okně prohlížeče jiných webových přihlásit tooyour **portál pro správu soutoku** jako správce.</span><span class="sxs-lookup"><span data-stu-id="61596-171">In a different web browser window, log in tooyour **Confluence admin portal** as an administrator.</span></span>

8. <span data-ttu-id="61596-172">Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **doplňky**.</span><span class="sxs-lookup"><span data-stu-id="61596-172">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon1.png)

9. <span data-ttu-id="61596-174">V části **ATLASSIAN MARKETPLACE** , klikněte na **najít nové rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="61596-174">Under **ATLASSIAN MARKETPLACE** tab, click **Find new add-ons**.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon.png)

10. <span data-ttu-id="61596-176">Hledání **Kantega jednotné přihlašování pro protokol Kerberos SAML soutoku** a klikněte na tlačítko **nainstalovat** tooinstall tlačítko hello nové zásuvný modul SAML.</span><span class="sxs-lookup"><span data-stu-id="61596-176">Search **Kantega SSO for Confluence SAML Kerberos** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon2.png)

11. <span data-ttu-id="61596-178">spuštění instalace modulu plug-in Hello.</span><span class="sxs-lookup"><span data-stu-id="61596-178">hello plugin installation starts.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon3.png)

12. <span data-ttu-id="61596-180">Po dokončení instalace hello.</span><span class="sxs-lookup"><span data-stu-id="61596-180">Once hello installation is complete.</span></span> <span data-ttu-id="61596-181">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="61596-181">Click **Close**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon33.png)

13. <span data-ttu-id="61596-183">Klikněte na **Manage** (Spravovat).</span><span class="sxs-lookup"><span data-stu-id="61596-183">Click **Manage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon34.png)
    
14. <span data-ttu-id="61596-185">Klikněte na tlačítko **konfigurace** tooconfigure hello nového modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="61596-185">Click **Configure** tooconfigure hello new plugin.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon35.png)

15. <span data-ttu-id="61596-187">Tento nový modul plug-in můžete také najít v části **uživatelů a zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="61596-187">This new plugin can also be found under **USERS & SECURITY** tab.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon36.png)
    
16. <span data-ttu-id="61596-189">V hello **SAML** části.</span><span class="sxs-lookup"><span data-stu-id="61596-189">In hello **SAML** section.</span></span> <span data-ttu-id="61596-190">Vyberte **Azure Active Directory (Azure AD)** z hello **zprostředkovatele identity přidat** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="61596-190">Select **Azure Active Directory (Azure AD)** from hello **Add identity provider** dropdown.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon4.png)

17. <span data-ttu-id="61596-192">Vyberte úroveň předplatné jako **základní**.</span><span class="sxs-lookup"><span data-stu-id="61596-192">Select subscription level as **Basic**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon5.png)       

18. <span data-ttu-id="61596-194">Na hello **vlastností aplikace** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="61596-194">On hello **App properties** section, perform following steps:</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon6.png)

    <span data-ttu-id="61596-196">a.</span><span class="sxs-lookup"><span data-stu-id="61596-196">a.</span></span> <span data-ttu-id="61596-197">Kopírování hello **identifikátor ID URI aplikace** hodnotu a použít ho jako **identifikátor, adresa URL odpovědi a přihlašovací adresa URL** na hello **Kantega SSO soutoku domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="61596-197">Copy hello **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on hello **Kantega SSO for Confluence Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="61596-198">b.</span><span class="sxs-lookup"><span data-stu-id="61596-198">b.</span></span> <span data-ttu-id="61596-199">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61596-199">Click **Next**.</span></span>

19. <span data-ttu-id="61596-200">Na hello **import metadat** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="61596-200">On hello **Metadata import** section, perform following steps:</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon7.png)

    <span data-ttu-id="61596-202">a.</span><span class="sxs-lookup"><span data-stu-id="61596-202">a.</span></span> <span data-ttu-id="61596-203">Vyberte **soubor metadat v mém počítači**a metadata souboru k odeslání, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="61596-203">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="61596-204">b.</span><span class="sxs-lookup"><span data-stu-id="61596-204">b.</span></span> <span data-ttu-id="61596-205">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61596-205">Click **Next**.</span></span>

20. <span data-ttu-id="61596-206">Na hello **název a jednotného přihlašování k umístění** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="61596-206">On hello **Name and SSO location** section, perform following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon8.png)
    
    <span data-ttu-id="61596-208">a.</span><span class="sxs-lookup"><span data-stu-id="61596-208">a.</span></span> <span data-ttu-id="61596-209">Přidat název hello zprostředkovatele Identity v **název zprostředkovatele Identity** textbox (např. Azure AD).</span><span class="sxs-lookup"><span data-stu-id="61596-209">Add Name of hello Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="61596-210">b.</span><span class="sxs-lookup"><span data-stu-id="61596-210">b.</span></span> <span data-ttu-id="61596-211">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61596-211">Click **Next**.</span></span>

21. <span data-ttu-id="61596-212">Ověřte hello podpisového certifikátu a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61596-212">Verify hello Signing certificate and click **Next**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon9.png)

22. <span data-ttu-id="61596-214">Na hello **soutoku uživatelské účty** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="61596-214">On hello **Confluence user accounts** section, perform following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon10.png)

    <span data-ttu-id="61596-216">a.</span><span class="sxs-lookup"><span data-stu-id="61596-216">a.</span></span> <span data-ttu-id="61596-217">Vyberte **v případě potřeby vytvořte uživatele v adresáři interní na soutoku** a zadejte vhodný název hello hello skupiny pro uživatele (může být více ne.</span><span class="sxs-lookup"><span data-stu-id="61596-217">Select **Create users in Confluence's internal Directory if needed** and enter hello appropriate name of hello group for users (can be multiple no.</span></span> <span data-ttu-id="61596-218">skupin oddělené čárkou).</span><span class="sxs-lookup"><span data-stu-id="61596-218">of groups separated by comma).</span></span>

    <span data-ttu-id="61596-219">b.</span><span class="sxs-lookup"><span data-stu-id="61596-219">b.</span></span> <span data-ttu-id="61596-220">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61596-220">Click **Next**.</span></span>

23. <span data-ttu-id="61596-221">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="61596-221">Click **Finish**.</span></span>   

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon11.png)

24. <span data-ttu-id="61596-223">Na hello **známé domén pro Azure AD** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="61596-223">On hello **Known domains for Azure AD** section, perform following steps:</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon12.png)

    <span data-ttu-id="61596-225">a.</span><span class="sxs-lookup"><span data-stu-id="61596-225">a.</span></span> <span data-ttu-id="61596-226">Vyberte **známé domén** z hello levém panelu hello stránky.</span><span class="sxs-lookup"><span data-stu-id="61596-226">Select **Known domains** from hello left panel of hello page.</span></span>

    <span data-ttu-id="61596-227">b.</span><span class="sxs-lookup"><span data-stu-id="61596-227">b.</span></span> <span data-ttu-id="61596-228">Zadejte název domény v hello **známé domén** textové pole.</span><span class="sxs-lookup"><span data-stu-id="61596-228">Enter domain name in hello **Known domains** textbox.</span></span>

    <span data-ttu-id="61596-229">c.</span><span class="sxs-lookup"><span data-stu-id="61596-229">c.</span></span> <span data-ttu-id="61596-230">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="61596-230">Click **Save**.</span></span> 

> [!TIP]
> <span data-ttu-id="61596-231">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="61596-231">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="61596-232">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="61596-232">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="61596-233">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="61596-233">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="61596-234">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="61596-234">Creating an Azure AD test user</span></span>
<span data-ttu-id="61596-235">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="61596-235">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="61596-237">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="61596-237">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="61596-238">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="61596-238">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="61596-240">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="61596-240">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="61596-242">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="61596-242">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="61596-244">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="61596-244">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="61596-246">a.</span><span class="sxs-lookup"><span data-stu-id="61596-246">a.</span></span> <span data-ttu-id="61596-247">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="61596-247">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="61596-248">b.</span><span class="sxs-lookup"><span data-stu-id="61596-248">b.</span></span> <span data-ttu-id="61596-249">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="61596-249">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="61596-250">c.</span><span class="sxs-lookup"><span data-stu-id="61596-250">c.</span></span> <span data-ttu-id="61596-251">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="61596-251">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="61596-252">d.</span><span class="sxs-lookup"><span data-stu-id="61596-252">d.</span></span> <span data-ttu-id="61596-253">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="61596-253">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-confluence-test-user"></a><span data-ttu-id="61596-254">Vytváření Kantega SSO pro soutoku testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="61596-254">Creating a Kantega SSO for Confluence test user</span></span>

<span data-ttu-id="61596-255">Uživatelé toolog tooenable Azure AD v tooConfluence, se musí být zřízená do soutoku.</span><span class="sxs-lookup"><span data-stu-id="61596-255">tooenable Azure AD users toolog in tooConfluence, they must be provisioned into Confluence.</span></span> <span data-ttu-id="61596-256">V případě hello Kantega přihlašování pro soutoku zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="61596-256">In hello case of Kantega SSO for Confluence, provisioning is a manual task.</span></span>

<span data-ttu-id="61596-257">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="61596-257">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="61596-258">Tooyour Kantega jednotné přihlašování pro web společnosti soutoku se přihlaste jako správce.</span><span class="sxs-lookup"><span data-stu-id="61596-258">Log in tooyour Kantega SSO for Confluence company site as an administrator.</span></span>

2. <span data-ttu-id="61596-259">Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="61596-259">Hover on cog and click hello **User management**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforconfluence-tutorial/user1.png) 

3. <span data-ttu-id="61596-261">V části uživatelé klikněte na tlačítko **přidat uživatele** kartě. Na hello **"Přidat uživatele"** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="61596-261">Under Users section, click **Add Users** tab. On hello **“Add a User”** dialog page, perform hello following steps:</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforconfluence-tutorial/user2.png) 

    <span data-ttu-id="61596-263">a.</span><span class="sxs-lookup"><span data-stu-id="61596-263">a.</span></span> <span data-ttu-id="61596-264">V hello **uživatelské jméno** jako typ hello e-mailu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="61596-264">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="61596-265">b.</span><span class="sxs-lookup"><span data-stu-id="61596-265">b.</span></span> <span data-ttu-id="61596-266">V hello **úplný název** textovému poli, úplný název typu hello uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="61596-266">In hello **Full Name** textbox, type hello full name of user like Britta Simon.</span></span>

    <span data-ttu-id="61596-267">c.</span><span class="sxs-lookup"><span data-stu-id="61596-267">c.</span></span> <span data-ttu-id="61596-268">V hello **e-mailu** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="61596-268">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="61596-269">d.</span><span class="sxs-lookup"><span data-stu-id="61596-269">d.</span></span> <span data-ttu-id="61596-270">V hello **heslo** textovému poli, zadejte hello heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="61596-270">In hello **Password** textbox, type hello password for user.</span></span>

    <span data-ttu-id="61596-271">e.</span><span class="sxs-lookup"><span data-stu-id="61596-271">e.</span></span> <span data-ttu-id="61596-272">Klikněte na tlačítko **Potvrdit heslo** zadejte znovu heslo hello.</span><span class="sxs-lookup"><span data-stu-id="61596-272">Click **Confirm Password** reenter hello password.</span></span>
    
    <span data-ttu-id="61596-273">f.</span><span class="sxs-lookup"><span data-stu-id="61596-273">f.</span></span> <span data-ttu-id="61596-274">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="61596-274">Click **Add** button.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="61596-275">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="61596-275">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="61596-276">V této části povolíte tak, že udělíte přístup tooKantega jednotné přihlašování pro soutoku Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="61596-276">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKantega SSO for Confluence.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="61596-278">**tooassign tooKantega Britta Simon jednotného přihlašování pro soutoku, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="61596-278">**tooassign Britta Simon tooKantega SSO for Confluence, perform hello following steps:**</span></span>

1. <span data-ttu-id="61596-279">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="61596-279">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="61596-281">V seznamu aplikace hello vyberte **Kantega jednotné přihlašování pro soutoku**.</span><span class="sxs-lookup"><span data-stu-id="61596-281">In hello applications list, select **Kantega SSO for Confluence**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_app.png) 

3. <span data-ttu-id="61596-283">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="61596-283">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="61596-285">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="61596-285">Click **Add** button.</span></span> <span data-ttu-id="61596-286">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="61596-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="61596-288">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="61596-288">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="61596-289">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="61596-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="61596-290">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="61596-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="61596-291">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="61596-291">Testing single sign-on</span></span>

<span data-ttu-id="61596-292">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="61596-292">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="61596-293">Po kliknutí na tlačítko hello Kantega jednotné přihlašování pro dlaždici soutoku v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Kantega jednotné přihlašování pro aplikace soutoku.</span><span class="sxs-lookup"><span data-stu-id="61596-293">When you click hello Kantega SSO for Confluence tile in hello Access Panel, you should get automatically signed-on tooyour Kantega SSO for Confluence application.</span></span>
<span data-ttu-id="61596-294">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="61596-294">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="61596-295">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="61596-295">Additional resources</span></span>

* [<span data-ttu-id="61596-296">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="61596-296">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="61596-297">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="61596-297">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_203.png

