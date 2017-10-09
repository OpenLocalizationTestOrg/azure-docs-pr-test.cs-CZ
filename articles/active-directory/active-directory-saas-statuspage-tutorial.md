---
title: "Kurz: Azure Active Directory integrace s stránka se stavem | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a stránka se stavem."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 7c6717017984241e9e459273ead4b5e062311120
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a><span data-ttu-id="17d85-103">Kurz: Azure Active Directory integrace s stránka se stavem</span><span class="sxs-lookup"><span data-stu-id="17d85-103">Tutorial: Azure Active Directory integration with StatusPage</span></span>

<span data-ttu-id="17d85-104">V tomto kurzu zjistíte, jak toointegrate stránka se stavem v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="17d85-104">In this tutorial, you learn how toointegrate StatusPage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="17d85-105">Stránka se stavem integraci s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="17d85-105">Integrating StatusPage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="17d85-106">Můžete řídit ve službě Azure AD, který má přístup tooStatusPage</span><span class="sxs-lookup"><span data-stu-id="17d85-106">You can control in Azure AD who has access tooStatusPage</span></span>
- <span data-ttu-id="17d85-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooStatusPage (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="17d85-107">You can enable your users tooautomatically get signed-on tooStatusPage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="17d85-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="17d85-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="17d85-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="17d85-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17d85-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17d85-110">Prerequisites</span></span>

<span data-ttu-id="17d85-111">tooconfigure integrace Azure AD s stránka se stavem, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="17d85-111">tooconfigure Azure AD integration with StatusPage, you need hello following items:</span></span>

- <span data-ttu-id="17d85-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="17d85-112">An Azure AD subscription</span></span>
- <span data-ttu-id="17d85-113">Stránka se stavem jednoho přihlášení povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="17d85-113">A StatusPage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="17d85-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="17d85-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="17d85-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="17d85-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="17d85-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="17d85-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="17d85-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="17d85-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="17d85-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="17d85-118">Scenario description</span></span>
<span data-ttu-id="17d85-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="17d85-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="17d85-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="17d85-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="17d85-121">Přidání stránka se stavem z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="17d85-121">Adding StatusPage from hello gallery</span></span>
2. <span data-ttu-id="17d85-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="17d85-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-statuspage-from-hello-gallery"></a><span data-ttu-id="17d85-123">Přidání stránka se stavem z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="17d85-123">Adding StatusPage from hello gallery</span></span>
<span data-ttu-id="17d85-124">tooconfigure hello integrace stránka se stavem do Azure AD, je nutné tooadd stránka se stavem hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="17d85-124">tooconfigure hello integration of StatusPage into Azure AD, you need tooadd StatusPage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="17d85-125">**tooadd stránka se stavem z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="17d85-125">**tooadd StatusPage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="17d85-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="17d85-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="17d85-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="17d85-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="17d85-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="17d85-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="17d85-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="17d85-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="17d85-133">Hello vyhledávacího pole zadejte **stránka se stavem**.</span><span class="sxs-lookup"><span data-stu-id="17d85-133">In hello search box, type **StatusPage**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_search.png)

5. <span data-ttu-id="17d85-135">Na panelu výsledků hello vyberte **stránka se stavem**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="17d85-135">In hello results panel, select **StatusPage**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="17d85-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="17d85-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="17d85-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s stránka se stavem podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="17d85-138">In this section, you configure and test Azure AD single sign-on with StatusPage based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="17d85-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v stránka se stavem je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="17d85-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in StatusPage is tooa user in Azure AD.</span></span> <span data-ttu-id="17d85-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v stránka se stavem musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="17d85-140">In other words, a link relationship between an Azure AD user and hello related user in StatusPage needs toobe established.</span></span>

<span data-ttu-id="17d85-141">V stránka se stavem, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="17d85-141">In StatusPage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="17d85-142">tooconfigure a testu Azure AD jednotné přihlašování s stránka se stavem, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="17d85-142">tooconfigure and test Azure AD single sign-on with StatusPage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="17d85-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="17d85-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="17d85-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="17d85-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="17d85-145">**[Vytvoření zkušebního uživatele stránka se stavem](#creating-a-statuspage-test-user)**  -toohave protějšek Britta Simon v stránka se stavem, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="17d85-145">**[Creating a StatusPage test user](#creating-a-statuspage-test-user)** - toohave a counterpart of Britta Simon in StatusPage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="17d85-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17d85-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="17d85-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="17d85-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="17d85-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="17d85-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="17d85-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci stránka se stavem.</span><span class="sxs-lookup"><span data-stu-id="17d85-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your StatusPage application.</span></span>

<span data-ttu-id="17d85-150">**tooconfigure Azure AD jednotné přihlašování s stránka se stavem, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="17d85-150">**tooconfigure Azure AD single sign-on with StatusPage, perform hello following steps:**</span></span>

1. <span data-ttu-id="17d85-151">V portálu Azure, na hello hello **stránka se stavem** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="17d85-151">In hello Azure portal, on hello **StatusPage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="17d85-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17d85-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_samlbase.png)

3. <span data-ttu-id="17d85-155">Na hello **stránka se stavem domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="17d85-155">On hello **StatusPage Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_url.png)

    <span data-ttu-id="17d85-157">a.</span><span class="sxs-lookup"><span data-stu-id="17d85-157">a.</span></span> <span data-ttu-id="17d85-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="17d85-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    <span data-ttu-id="17d85-159">b.</span><span class="sxs-lookup"><span data-stu-id="17d85-159">b.</span></span> <span data-ttu-id="17d85-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="17d85-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span> 
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

    > [!NOTE]
    > <span data-ttu-id="17d85-161">Obraťte se na tým podpory hello stránka se stavem v [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)toorequest metadata nezbytné tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17d85-161">Contact hello StatusPage support team at [SupportTeam@statuspage.io](mailto:SupportTeam@statuspage.io)toorequest metadata necessary tooconfigure single sign-on.</span></span> 
    >
    ><span data-ttu-id="17d85-162">a.</span><span class="sxs-lookup"><span data-stu-id="17d85-162">a.</span></span> <span data-ttu-id="17d85-163">Z metadat hello zkopírujte hello vystavitele hodnotu a pak ji vložit do hello **identifikátor** textové pole.</span><span class="sxs-lookup"><span data-stu-id="17d85-163">From hello metadata, copy hello Issuer value, and then paste it into hello **Identifier** textbox.</span></span>
    >
    ><span data-ttu-id="17d85-164">b.</span><span class="sxs-lookup"><span data-stu-id="17d85-164">b.</span></span> <span data-ttu-id="17d85-165">Z metadat hello hello adresa URL odpovědi zkopírujte a vložte jej do hello **adresa URL odpovědi** textové pole.</span><span class="sxs-lookup"><span data-stu-id="17d85-165">From hello metadata, copy hello Reply URL, and then paste it into hello **Reply URL** textbox.</span></span>

4. <span data-ttu-id="17d85-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="17d85-166">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_certificate.png) 

5. <span data-ttu-id="17d85-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="17d85-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="17d85-170">Na hello **stránka se stavem konfigurace** klikněte na tlačítko **konfigurace stránka se stavem** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="17d85-170">On hello **StatusPage Configuration** section, click **Configure StatusPage** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="17d85-171">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="17d85-171">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_configure.png) 

7. <span data-ttu-id="17d85-173">V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti tooyour stránka se stavem.</span><span class="sxs-lookup"><span data-stu-id="17d85-173">In another browser window, sign on tooyour StatusPage company site as an administrator.</span></span>

8. <span data-ttu-id="17d85-174">Na hlavním panelu nástrojů hello klikněte na **spravovat účet**.</span><span class="sxs-lookup"><span data-stu-id="17d85-174">In hello main toolbar, click **Manage Account**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png) 

10. <span data-ttu-id="17d85-176">Klikněte na tlačítko hello **jednotné přihlašování** kartě.</span><span class="sxs-lookup"><span data-stu-id="17d85-176">Click hello **Single Sign-on** tab.</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png) 

11. <span data-ttu-id="17d85-178">Na stránce instalace jednotné přihlašování hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="17d85-178">On hello SSO Setup page, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_09.png) 
 
    <span data-ttu-id="17d85-181">a.</span><span class="sxs-lookup"><span data-stu-id="17d85-181">a.</span></span> <span data-ttu-id="17d85-182">V hello **jednotné přihlašování cílová adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="17d85-182">In hello **SSO Target URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="17d85-183">b.</span><span class="sxs-lookup"><span data-stu-id="17d85-183">b.</span></span> <span data-ttu-id="17d85-184">Otevřete stažený certifikát v poznámkovém bloku hello kopírování obsahu a pak ji vložit do hello **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="17d85-184">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **Certificate** textbox.</span></span> 

    <span data-ttu-id="17d85-185">c.</span><span class="sxs-lookup"><span data-stu-id="17d85-185">c.</span></span> <span data-ttu-id="17d85-186">Klikněte na tlačítko **uložení konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="17d85-186">Click **SAVE CONFIGURATION**.</span></span>

> [!TIP]
> <span data-ttu-id="17d85-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="17d85-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="17d85-188">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="17d85-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="17d85-189">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="17d85-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="17d85-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="17d85-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="17d85-191">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="17d85-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="17d85-193">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="17d85-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="17d85-194">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="17d85-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="17d85-196">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="17d85-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="17d85-198">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="17d85-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="17d85-200">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="17d85-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="17d85-202">a.</span><span class="sxs-lookup"><span data-stu-id="17d85-202">a.</span></span> <span data-ttu-id="17d85-203">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="17d85-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="17d85-204">b.</span><span class="sxs-lookup"><span data-stu-id="17d85-204">b.</span></span> <span data-ttu-id="17d85-205">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="17d85-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="17d85-206">c.</span><span class="sxs-lookup"><span data-stu-id="17d85-206">c.</span></span> <span data-ttu-id="17d85-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="17d85-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="17d85-208">d.</span><span class="sxs-lookup"><span data-stu-id="17d85-208">d.</span></span> <span data-ttu-id="17d85-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="17d85-209">Click **Create**.</span></span>
 
### <a name="creating-a-statuspage-test-user"></a><span data-ttu-id="17d85-210">Vytvoření zkušebního uživatele stránka se stavem</span><span class="sxs-lookup"><span data-stu-id="17d85-210">Creating a StatusPage test user</span></span>

<span data-ttu-id="17d85-211">Hello cílem této části je toocreate uživatel volal Britta Simon v stránka se stavem.</span><span class="sxs-lookup"><span data-stu-id="17d85-211">hello objective of this section is toocreate a user called Britta Simon in StatusPage.</span></span>

<span data-ttu-id="17d85-212">Stránka se stavem podporuje zřizování za běhu.</span><span class="sxs-lookup"><span data-stu-id="17d85-212">StatusPage supports just-in-time provisioning.</span></span> <span data-ttu-id="17d85-213">Již je v povolíte [konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="17d85-213">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="17d85-214">**toocreate uživatel volal Britta Simon v stránka se stavem, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="17d85-214">**toocreate a user called Britta Simon in StatusPage, perform hello following steps:**</span></span>

1. <span data-ttu-id="17d85-215">Web společnosti stránka se stavem tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="17d85-215">Sign-on tooyour StatusPage company site as an administrator.</span></span>

2. <span data-ttu-id="17d85-216">V nabídce hello hello nahoře, klikněte na tlačítko **spravovat účet**.</span><span class="sxs-lookup"><span data-stu-id="17d85-216">In hello menu on hello top, click **Manage Account**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png)

3. <span data-ttu-id="17d85-218">Klikněte na tlačítko hello **členové týmu** kartě.</span><span class="sxs-lookup"><span data-stu-id="17d85-218">Click hello **Team Members** tab.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png) 

4. <span data-ttu-id="17d85-220">Klikněte na tlačítko **člen týmu přidat**.</span><span class="sxs-lookup"><span data-stu-id="17d85-220">Click **ADD TEAM MEMBER**.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png) 

5. <span data-ttu-id="17d85-222">Typ hello **e-mailovou adresu**, **křestní jméno**, a **Sur název** platného uživatele, kterou chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="17d85-222">Type hello **Email Address**, **First Name**, and **Sur Name** of a valid user you want tooprovision into hello related textboxes.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png) 

6. <span data-ttu-id="17d85-224">Jako **Role**, zvolte **správce klienta**.</span><span class="sxs-lookup"><span data-stu-id="17d85-224">As **Role**, choose **Client Administrator**.</span></span>

7. <span data-ttu-id="17d85-225">Klikněte na tlačítko **vytvořit účet**.</span><span class="sxs-lookup"><span data-stu-id="17d85-225">Click **CREATE ACCOUNT**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="17d85-226">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="17d85-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="17d85-227">V této části povolíte tak, že udělíte přístup tooStatusPage toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17d85-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooStatusPage.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="17d85-229">**tooassign Britta Simon tooStatusPage, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="17d85-229">**tooassign Britta Simon tooStatusPage, perform hello following steps:**</span></span>

1. <span data-ttu-id="17d85-230">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="17d85-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="17d85-232">V seznamu aplikace hello vyberte **stránka se stavem**.</span><span class="sxs-lookup"><span data-stu-id="17d85-232">In hello applications list, select **StatusPage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_app.png) 

3. <span data-ttu-id="17d85-234">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="17d85-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="17d85-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="17d85-236">Click **Add** button.</span></span> <span data-ttu-id="17d85-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17d85-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="17d85-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="17d85-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="17d85-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17d85-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="17d85-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17d85-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="17d85-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="17d85-242">Testing single sign-on</span></span>

<span data-ttu-id="17d85-243">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="17d85-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="17d85-244">Když kliknete na dlaždici hello stránka se stavem v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour stránka se stavem aplikace.</span><span class="sxs-lookup"><span data-stu-id="17d85-244">When you click hello StatusPage tile in hello Access Panel, you should get automatically signed-on tooyour StatusPage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="17d85-245">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="17d85-245">Additional resources</span></span>

* [<span data-ttu-id="17d85-246">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="17d85-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="17d85-247">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="17d85-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png

