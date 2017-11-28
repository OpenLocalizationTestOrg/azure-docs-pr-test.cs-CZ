---
title: 'Kurz: Azure Active Directory integrace s Intacct | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Intacct."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3500039615166c2f61fb408d85bb82dfaefba134
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a><span data-ttu-id="e8b93-103">Kurz: Azure Active Directory integrace s Intacct</span><span class="sxs-lookup"><span data-stu-id="e8b93-103">Tutorial: Azure Active Directory integration with Intacct</span></span>

<span data-ttu-id="e8b93-104">V tomto kurzu zjistíte, jak toointegrate Intacct s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e8b93-104">In this tutorial, you learn how toointegrate Intacct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e8b93-105">Integrace Intacct s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e8b93-105">Integrating Intacct with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e8b93-106">Můžete řídit ve službě Azure AD, který má přístup tooIntacct</span><span class="sxs-lookup"><span data-stu-id="e8b93-106">You can control in Azure AD who has access tooIntacct</span></span>
- <span data-ttu-id="e8b93-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooIntacct (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e8b93-107">You can enable your users tooautomatically get signed-on tooIntacct (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e8b93-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e8b93-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e8b93-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e8b93-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8b93-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e8b93-110">Prerequisites</span></span>

<span data-ttu-id="e8b93-111">Integrace služby Azure AD s Intacct tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e8b93-111">tooconfigure Azure AD integration with Intacct, you need hello following items:</span></span>

- <span data-ttu-id="e8b93-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e8b93-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e8b93-113">Intacct jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e8b93-113">An Intacct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e8b93-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e8b93-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e8b93-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e8b93-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e8b93-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e8b93-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e8b93-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e8b93-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e8b93-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e8b93-118">Scenario description</span></span>
<span data-ttu-id="e8b93-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e8b93-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e8b93-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e8b93-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e8b93-121">Přidání Intacct z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e8b93-121">Adding Intacct from hello gallery</span></span>
2. <span data-ttu-id="e8b93-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e8b93-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intacct-from-hello-gallery"></a><span data-ttu-id="e8b93-123">Přidání Intacct z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e8b93-123">Adding Intacct from hello gallery</span></span>
<span data-ttu-id="e8b93-124">tooconfigure hello integrace Intacct do Azure AD, je nutné tooadd Intacct hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e8b93-124">tooconfigure hello integration of Intacct into Azure AD, you need tooadd Intacct from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e8b93-125">**tooadd Intacct z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e8b93-125">**tooadd Intacct from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e8b93-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e8b93-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e8b93-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e8b93-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e8b93-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e8b93-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e8b93-133">Hello vyhledávacího pole zadejte **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-133">In hello search box, type **Intacct**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_search.png)

5. <span data-ttu-id="e8b93-135">Na panelu výsledků hello vyberte **Intacct**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e8b93-135">In hello results panel, select **Intacct**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e8b93-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e8b93-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e8b93-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Intacct podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e8b93-138">In this section, you configure and test Azure AD single sign-on with Intacct based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e8b93-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Intacct je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e8b93-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Intacct is tooa user in Azure AD.</span></span> <span data-ttu-id="e8b93-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Intacct musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="e8b93-140">In other words, a link relationship between an Azure AD user and hello related user in Intacct needs toobe established.</span></span>

<span data-ttu-id="e8b93-141">V Intacct, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="e8b93-141">In Intacct, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e8b93-142">tooconfigure a testu Azure AD jednotné přihlašování s Intacct, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="e8b93-142">tooconfigure and test Azure AD single sign-on with Intacct, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e8b93-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="e8b93-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e8b93-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e8b93-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e8b93-145">**[Vytváření testovacího uživatele Intacct](#creating-an-intacct-test-user)**  -toohave protějšek Britta Simon v Intacct, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="e8b93-145">**[Creating an Intacct test user](#creating-an-intacct-test-user)** - toohave a counterpart of Britta Simon in Intacct that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e8b93-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e8b93-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e8b93-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="e8b93-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e8b93-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e8b93-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e8b93-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Intacct.</span><span class="sxs-lookup"><span data-stu-id="e8b93-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Intacct application.</span></span>

<span data-ttu-id="e8b93-150">**tooconfigure Azure AD jednotné přihlašování s Intacct, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e8b93-150">**tooconfigure Azure AD single sign-on with Intacct, perform hello following steps:**</span></span>

1. <span data-ttu-id="e8b93-151">V portálu Azure, na hello hello **Intacct** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-151">In hello Azure portal, on hello **Intacct** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e8b93-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e8b93-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_samlbase.png)

3. <span data-ttu-id="e8b93-155">Na hello **Intacct domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e8b93-155">On hello **Intacct Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_url.png)

    <span data-ttu-id="e8b93-157">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="e8b93-157">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > <span data-ttu-id="e8b93-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="e8b93-158">This value is not real.</span></span> <span data-ttu-id="e8b93-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e8b93-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="e8b93-160">Obraťte se na [tým podpory Intacct](https://us.intacct.com/support) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e8b93-160">Contact [Intacct support team](https://us.intacct.com/support) tooget this value.</span></span>

4. <span data-ttu-id="e8b93-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e8b93-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_certificate.png) 

5. <span data-ttu-id="e8b93-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e8b93-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e8b93-165">Na hello **Intacct konfigurace** klikněte na tlačítko **konfigurace Intacct** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e8b93-165">On hello **Intacct Configuration** section, click **Configure Intacct** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e8b93-166">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e8b93-166">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_configure.png) 

7. <span data-ttu-id="e8b93-168">V okně prohlížeče jiný web přihlaste jako správce tooyour Intacct společnosti lokality.</span><span class="sxs-lookup"><span data-stu-id="e8b93-168">In a different web browser window, sign in tooyour Intacct company site as an administrator.</span></span>

8. <span data-ttu-id="e8b93-169">Klikněte na tlačítko hello **společnosti** a pak klikněte **informace společnosti**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-169">Click hello **Company** tab, and then click **Company Info**.</span></span>

    <span data-ttu-id="e8b93-170">![Společnosti](./media/active-directory-saas-intacct-tutorial/ic790037.png "společnosti")</span><span class="sxs-lookup"><span data-stu-id="e8b93-170">![Company](./media/active-directory-saas-intacct-tutorial/ic790037.png "Company")</span></span>

9. <span data-ttu-id="e8b93-171">Klikněte na tlačítko hello **zabezpečení** a pak klikněte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-171">Click hello **Security** tab, and then click **Edit**.</span></span>

    <span data-ttu-id="e8b93-172">![Zabezpečení](./media/active-directory-saas-intacct-tutorial/ic790038.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="e8b93-172">![Security](./media/active-directory-saas-intacct-tutorial/ic790038.png "Security")</span></span>

10. <span data-ttu-id="e8b93-173">V hello **jednotné přihlašování (SSO)** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e8b93-173">In hello **Single sign on (SSO)** section, perform hello following steps:</span></span>

    <span data-ttu-id="e8b93-174">![Jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/ic790039.png "jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e8b93-174">![Single sign on](./media/active-directory-saas-intacct-tutorial/ic790039.png "single sign on")</span></span>

    <span data-ttu-id="e8b93-175">a.</span><span class="sxs-lookup"><span data-stu-id="e8b93-175">a.</span></span> <span data-ttu-id="e8b93-176">Vyberte **povolení jednotného přihlašování na**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-176">Select **Enable single sign on**.</span></span>

    <span data-ttu-id="e8b93-177">b.</span><span class="sxs-lookup"><span data-stu-id="e8b93-177">b.</span></span> <span data-ttu-id="e8b93-178">Jako **typ zprostředkovatele Identity**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-178">As **Identity provider type**, select **SAML 2.0**.</span></span>

    <span data-ttu-id="e8b93-179">c.</span><span class="sxs-lookup"><span data-stu-id="e8b93-179">c.</span></span> <span data-ttu-id="e8b93-180">V **URL vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e8b93-180">In **Issuer URL** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="e8b93-181">d.</span><span class="sxs-lookup"><span data-stu-id="e8b93-181">d.</span></span> <span data-ttu-id="e8b93-182">V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e8b93-182">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="e8b93-183">e.</span><span class="sxs-lookup"><span data-stu-id="e8b93-183">e.</span></span> <span data-ttu-id="e8b93-184">Otevřete váš **kódování base-64** kódování certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát** pole.</span><span class="sxs-lookup"><span data-stu-id="e8b93-184">Open your **base-64** encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Certificate** box.</span></span>
   
    <span data-ttu-id="e8b93-185">f.</span><span class="sxs-lookup"><span data-stu-id="e8b93-185">f.</span></span> <span data-ttu-id="e8b93-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="e8b93-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="e8b93-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e8b93-188">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="e8b93-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e8b93-189">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e8b93-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e8b93-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e8b93-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="e8b93-191">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="e8b93-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e8b93-193">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e8b93-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e8b93-194">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e8b93-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e8b93-196">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e8b93-198">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="e8b93-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e8b93-200">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e8b93-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e8b93-202">a.</span><span class="sxs-lookup"><span data-stu-id="e8b93-202">a.</span></span> <span data-ttu-id="e8b93-203">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e8b93-204">b.</span><span class="sxs-lookup"><span data-stu-id="e8b93-204">b.</span></span> <span data-ttu-id="e8b93-205">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e8b93-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e8b93-206">c.</span><span class="sxs-lookup"><span data-stu-id="e8b93-206">c.</span></span> <span data-ttu-id="e8b93-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e8b93-208">d.</span><span class="sxs-lookup"><span data-stu-id="e8b93-208">d.</span></span> <span data-ttu-id="e8b93-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-209">Click **Create**.</span></span>
 
### <a name="creating-an-intacct-test-user"></a><span data-ttu-id="e8b93-210">Vytváření testovacího uživatele Intacct</span><span class="sxs-lookup"><span data-stu-id="e8b93-210">Creating an Intacct test user</span></span>

<span data-ttu-id="e8b93-211">tooset přidávání uživatelů Azure AD, se můžete přihlásit tooIntacct, se musí být zřízená do Intacct.</span><span class="sxs-lookup"><span data-stu-id="e8b93-211">tooset up Azure AD users so they can sign in tooIntacct, they must be provisioned into Intacct.</span></span> <span data-ttu-id="e8b93-212">Pro Intacct zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="e8b93-212">For Intacct, provisioning is a manual task.</span></span>

<span data-ttu-id="e8b93-213">**tooprovision uživatelské účty, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e8b93-213">**tooprovision user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="e8b93-214">Přihlaste se tooyour **Intacct** klienta.</span><span class="sxs-lookup"><span data-stu-id="e8b93-214">Sign in tooyour **Intacct** tenant.</span></span>

2. <span data-ttu-id="e8b93-215">Klikněte na tlačítko hello **společnosti** a pak klikněte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-215">Click hello **Company** tab, and then click **Users**.</span></span>

    <span data-ttu-id="e8b93-216">![Uživatelé](./media/active-directory-saas-intacct-tutorial/ic790041.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="e8b93-216">![Users](./media/active-directory-saas-intacct-tutorial/ic790041.png "Users")</span></span>
3. <span data-ttu-id="e8b93-217">Klikněte na tlačítko hello **přidat** kartě.</span><span class="sxs-lookup"><span data-stu-id="e8b93-217">Click hello **Add** tab.</span></span>

    <span data-ttu-id="e8b93-218">![Přidat](./media/active-directory-saas-intacct-tutorial/ic790042.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="e8b93-218">![Add](./media/active-directory-saas-intacct-tutorial/ic790042.png "Add")</span></span>
4. <span data-ttu-id="e8b93-219">V hello **informace o uživateli** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e8b93-219">In hello **User Information** section, perform hello following steps:</span></span>

    <span data-ttu-id="e8b93-220">![Informace o uživateli](./media/active-directory-saas-intacct-tutorial/ic790043.png "informace o uživateli")</span><span class="sxs-lookup"><span data-stu-id="e8b93-220">![User Information](./media/active-directory-saas-intacct-tutorial/ic790043.png "User Information")</span></span>

    <span data-ttu-id="e8b93-221">a.</span><span class="sxs-lookup"><span data-stu-id="e8b93-221">a.</span></span> <span data-ttu-id="e8b93-222">Zadejte hello **ID uživatele**, hello **příjmení**, **křestní jméno**, hello **e-mailová adresa**, hello **název**, a hello **Phone** účtu Azure AD, které chcete do hello tooprovision **informace o uživateli** části.</span><span class="sxs-lookup"><span data-stu-id="e8b93-222">Enter hello **User ID**, hello **Last name**, **First name**, hello **Email address**, hello **Title**, and hello **Phone** of an Azure AD account that you want tooprovision into hello **User Information** section.</span></span>

    <span data-ttu-id="e8b93-223">b.</span><span class="sxs-lookup"><span data-stu-id="e8b93-223">b.</span></span> <span data-ttu-id="e8b93-224">Vyberte hello **oprávnění správce** účtu Azure AD, které chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="e8b93-224">Select hello **Admin privileges** of an Azure AD account that you want tooprovision.</span></span>
   
    <span data-ttu-id="e8b93-225">c.</span><span class="sxs-lookup"><span data-stu-id="e8b93-225">c.</span></span> <span data-ttu-id="e8b93-226">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-226">Click **Save**.</span></span> <span data-ttu-id="e8b93-227">Držitel účtu Azure AD Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="e8b93-227">hello Azure AD account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

>[!NOTE]
><span data-ttu-id="e8b93-228">tooprovision Azure AD uživatelské účty, můžete použít jiné nástroje pro tvorbu Intacct uživatelského účtu nebo rozhraní API, které jsou poskytovány Intacct.</span><span class="sxs-lookup"><span data-stu-id="e8b93-228">tooprovision Azure AD user accounts, you can use other Intacct user account creation tools or APIs that are provided by Intacct.</span></span>
        
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e8b93-229">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e8b93-229">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e8b93-230">V této části povolíte tak, že udělíte přístup tooIntacct toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e8b93-230">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIntacct.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e8b93-232">**tooassign Britta Simon tooIntacct, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e8b93-232">**tooassign Britta Simon tooIntacct, perform hello following steps:**</span></span>

1. <span data-ttu-id="e8b93-233">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-233">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e8b93-235">V seznamu aplikace hello vyberte **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-235">In hello applications list, select **Intacct**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_app.png) 

3. <span data-ttu-id="e8b93-237">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e8b93-237">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e8b93-239">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e8b93-239">Click **Add** button.</span></span> <span data-ttu-id="e8b93-240">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e8b93-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e8b93-242">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="e8b93-242">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e8b93-243">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e8b93-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e8b93-244">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e8b93-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e8b93-245">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e8b93-245">Testing single sign-on</span></span>

<span data-ttu-id="e8b93-246">V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e8b93-246">In this section, you test your Azure AD single sign-on configuration by using hello Access Panel.</span></span>

<span data-ttu-id="e8b93-247">Když kliknete na dlaždici Intacct hello v hello přístupového panelu, by měl být automaticky přihlášeni tooyour Intacct aplikace.</span><span class="sxs-lookup"><span data-stu-id="e8b93-247">When you click hello Intacct tile in hello Access Panel, you should be automatically signed in tooyour Intacct application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8b93-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e8b93-248">Additional resources</span></span>

* [<span data-ttu-id="e8b93-249">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e8b93-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e8b93-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e8b93-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

