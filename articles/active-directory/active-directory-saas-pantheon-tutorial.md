---
title: 'Kurz: Azure Active Directory integrace s Pantheon | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Pantheon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 5c3e54aef1f64dbab77d40a8c098172d609bfec8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a><span data-ttu-id="67b2f-103">Kurz: Azure Active Directory integrace s Pantheon</span><span class="sxs-lookup"><span data-stu-id="67b2f-103">Tutorial: Azure Active Directory integration with Pantheon</span></span>

<span data-ttu-id="67b2f-104">V tomto kurzu zjistíte, jak toointegrate Pantheon s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="67b2f-104">In this tutorial, you learn how toointegrate Pantheon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="67b2f-105">Integrace Pantheon s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="67b2f-105">Integrating Pantheon with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="67b2f-106">Můžete řídit ve službě Azure AD, který má přístup tooPantheon</span><span class="sxs-lookup"><span data-stu-id="67b2f-106">You can control in Azure AD who has access tooPantheon</span></span>
- <span data-ttu-id="67b2f-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPantheon (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="67b2f-107">You can enable your users tooautomatically get signed-on tooPantheon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="67b2f-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="67b2f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="67b2f-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="67b2f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67b2f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="67b2f-110">Prerequisites</span></span>

<span data-ttu-id="67b2f-111">Integrace služby Azure AD s Pantheon tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="67b2f-111">tooconfigure Azure AD integration with Pantheon, you need hello following items:</span></span>

- <span data-ttu-id="67b2f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="67b2f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="67b2f-113">Pantheon jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="67b2f-113">A Pantheon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="67b2f-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="67b2f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="67b2f-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="67b2f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="67b2f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="67b2f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="67b2f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="67b2f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="67b2f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="67b2f-118">Scenario description</span></span>
<span data-ttu-id="67b2f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="67b2f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="67b2f-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="67b2f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="67b2f-121">Přidání Pantheon z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="67b2f-121">Adding Pantheon from hello gallery</span></span>
2. <span data-ttu-id="67b2f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="67b2f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pantheon-from-hello-gallery"></a><span data-ttu-id="67b2f-123">Přidání Pantheon z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="67b2f-123">Adding Pantheon from hello gallery</span></span>
<span data-ttu-id="67b2f-124">tooconfigure hello integrace Pantheon do Azure AD, je nutné tooadd Pantheon hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="67b2f-124">tooconfigure hello integration of Pantheon into Azure AD, you need tooadd Pantheon from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="67b2f-125">**tooadd Pantheon z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="67b2f-125">**tooadd Pantheon from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="67b2f-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="67b2f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="67b2f-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="67b2f-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="67b2f-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="67b2f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="67b2f-133">Hello vyhledávacího pole zadejte **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-133">In hello search box, type **Pantheon**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_search.png)

5. <span data-ttu-id="67b2f-135">Na panelu výsledků hello vyberte **Pantheon**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="67b2f-135">In hello results panel, select **Pantheon**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="67b2f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="67b2f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="67b2f-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pantheon podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="67b2f-138">In this section, you configure and test Azure AD single sign-on with Pantheon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="67b2f-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Pantheon je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="67b2f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pantheon is tooa user in Azure AD.</span></span> <span data-ttu-id="67b2f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Pantheon musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="67b2f-140">In other words, a link relationship between an Azure AD user and hello related user in Pantheon needs toobe established.</span></span>

<span data-ttu-id="67b2f-141">V Pantheon, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="67b2f-141">In Pantheon, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="67b2f-142">tooconfigure a testu Azure AD jednotné přihlašování s Pantheon, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="67b2f-142">tooconfigure and test Azure AD single sign-on with Pantheon, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="67b2f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="67b2f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="67b2f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="67b2f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="67b2f-145">**[Vytvoření zkušebního uživatele Pantheon](#creating-a-pantheon-test-user)**  -toohave protějšek Britta Simon v Pantheon, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="67b2f-145">**[Creating a Pantheon test user](#creating-a-pantheon-test-user)** - toohave a counterpart of Britta Simon in Pantheon that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="67b2f-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="67b2f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="67b2f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="67b2f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="67b2f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="67b2f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="67b2f-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Pantheon.</span><span class="sxs-lookup"><span data-stu-id="67b2f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Pantheon application.</span></span>

<span data-ttu-id="67b2f-150">**tooconfigure Azure AD jednotné přihlašování s Pantheon, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="67b2f-150">**tooconfigure Azure AD single sign-on with Pantheon, perform hello following steps:**</span></span>

1. <span data-ttu-id="67b2f-151">V portálu Azure, na hello hello **Pantheon** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-151">In hello Azure portal, on hello **Pantheon** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="67b2f-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="67b2f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_samlbase.png)

3. <span data-ttu-id="67b2f-155">Na hello **Pantheon domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="67b2f-155">On hello **Pantheon Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_url.png)

    <span data-ttu-id="67b2f-157">a.</span><span class="sxs-lookup"><span data-stu-id="67b2f-157">a.</span></span> <span data-ttu-id="67b2f-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`urn:auth0:pantheon:<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="67b2f-158">In hello **Identifier** textbox, type a URL using hello following pattern: `urn:auth0:pantheon:<orgname>-SSO`</span></span>

    <span data-ttu-id="67b2f-159">b.</span><span class="sxs-lookup"><span data-stu-id="67b2f-159">b.</span></span> <span data-ttu-id="67b2f-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="67b2f-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="67b2f-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="67b2f-161">These values are not real.</span></span> <span data-ttu-id="67b2f-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="67b2f-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="67b2f-163">Obraťte se na [tým podpory Pantheon](https://pantheon.io/docs/getting-support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="67b2f-163">Contact [Pantheon support team](https://pantheon.io/docs/getting-support/) tooget these values.</span></span>

4. <span data-ttu-id="67b2f-164">Aplikace pantheon očekává kontrolního výrazu SAML hello v konkrétním formátu, který vyžaduje jste tooset hello hodnota atributu UserIdentifier s hello uživatele e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="67b2f-164">Pantheon application expects hello SAML assertion in specific format, which requires you tooset hello UserIdentifier attribute value with hello user’s email address.</span></span> <span data-ttu-id="67b2f-165">Ve výchozím nastavení používá Azure AD hello UserPrincipalName UserIdentifier atributu.</span><span class="sxs-lookup"><span data-stu-id="67b2f-165">By default Azure AD uses hello UserPrincipalName for UserIdentifier attribute.</span></span> <span data-ttu-id="67b2f-166">Ale pro úspěšné integrace potřebovat tooadjust toomatch tuto hodnotu s e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="67b2f-166">But for successful integration you need tooadjust this value toomatch with user’s email address.</span></span> <span data-ttu-id="67b2f-167">integrace Hello budou fungovat jenom po provedení hello správné mapování.</span><span class="sxs-lookup"><span data-stu-id="67b2f-167">hello integration will only work after doing hello correct mapping.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_attribute.png)  


5. <span data-ttu-id="67b2f-169">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="67b2f-169">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_certificate.png)

6. <span data-ttu-id="67b2f-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="67b2f-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="67b2f-173">Na hello **Pantheon konfigurace** klikněte na tlačítko **konfigurace Pantheon** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="67b2f-173">On hello **Pantheon Configuration** section, click **Configure Pantheon** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="67b2f-174">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="67b2f-174">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_configure.png) 

8. <span data-ttu-id="67b2f-176">tooconfigure jednotného přihlašování na **Pantheon** straně, je nutné stáhnout hello toosend **certifikát** a **SAML jeden přihlašování adresa URL služby** příliš[Pantheon tým podpory](https://pantheon.io/docs/getting-support/).</span><span class="sxs-lookup"><span data-stu-id="67b2f-176">tooconfigure single sign-on on **Pantheon** side, you need toosend hello downloaded **Certificate** and **SAML Single Sign-On Service URL** too[Pantheon support team](https://pantheon.io/docs/getting-support/).</span></span>

     > [!Note]
     > <span data-ttu-id="67b2f-177">Musíte také informace o doménách e-mailu hello tooprovide a datum a čas při tooenable chcete toto připojení.</span><span class="sxs-lookup"><span data-stu-id="67b2f-177">You also need tooprovide hello Email Domain(s) information and Date Time when you want tooenable this connection.</span></span> <span data-ttu-id="67b2f-178">Můžete najít další podrobnosti o z [sem](https://pantheon.io/docs/sso-organizations/)</span><span class="sxs-lookup"><span data-stu-id="67b2f-178">You can find more details about it from [here](https://pantheon.io/docs/sso-organizations/)</span></span>

> [!TIP]
> <span data-ttu-id="67b2f-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="67b2f-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="67b2f-180">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="67b2f-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="67b2f-181">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="67b2f-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="67b2f-182">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="67b2f-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="67b2f-183">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="67b2f-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="67b2f-185">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="67b2f-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="67b2f-186">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="67b2f-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="67b2f-188">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="67b2f-190">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="67b2f-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="67b2f-192">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="67b2f-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="67b2f-194">a.</span><span class="sxs-lookup"><span data-stu-id="67b2f-194">a.</span></span> <span data-ttu-id="67b2f-195">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="67b2f-196">b.</span><span class="sxs-lookup"><span data-stu-id="67b2f-196">b.</span></span> <span data-ttu-id="67b2f-197">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="67b2f-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="67b2f-198">c.</span><span class="sxs-lookup"><span data-stu-id="67b2f-198">c.</span></span> <span data-ttu-id="67b2f-199">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="67b2f-200">d.</span><span class="sxs-lookup"><span data-stu-id="67b2f-200">d.</span></span> <span data-ttu-id="67b2f-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-201">Click **Create**.</span></span>
 
### <a name="creating-a-pantheon-test-user"></a><span data-ttu-id="67b2f-202">Vytvoření zkušebního uživatele Pantheon</span><span class="sxs-lookup"><span data-stu-id="67b2f-202">Creating a Pantheon test user</span></span>

<span data-ttu-id="67b2f-203">V této části vytvoříte volal Britta Simon v Pantheon uživatele.</span><span class="sxs-lookup"><span data-stu-id="67b2f-203">In this section, you create a user called Britta Simon in Pantheon.</span></span> <span data-ttu-id="67b2f-204">Postupujte podle níže kroky tooadd hello uživatele v Pantheon hello.</span><span class="sxs-lookup"><span data-stu-id="67b2f-204">Please follow hello below steps tooadd hello user in Pantheon.</span></span> 

>[!NOTE] 
><span data-ttu-id="67b2f-205">Pro jednotné přihlašování musí uživatel toowork toobe vytvořili první v Pantheon.</span><span class="sxs-lookup"><span data-stu-id="67b2f-205">For SSO toowork user needs toobe created first in Pantheon.</span></span>

1. <span data-ttu-id="67b2f-206">TooPantheon přihlášení pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="67b2f-206">Login tooPantheon with admin credentials.</span></span>

2. <span data-ttu-id="67b2f-207">Přejděte příliš**organizace** stránka řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="67b2f-207">Navigate too**Organization** dashboard page.</span></span>
 
3. <span data-ttu-id="67b2f-208">Klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-208">Click **People**.</span></span>

4. <span data-ttu-id="67b2f-209">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-209">Click **Add user**.</span></span>

5. <span data-ttu-id="67b2f-210">Zadejte e-mailovou adresu uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="67b2f-210">Enter hello user's email address.</span></span>

6. <span data-ttu-id="67b2f-211">Vyberte roli uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="67b2f-211">Choose hello user's role.</span></span>

7. <span data-ttu-id="67b2f-212">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-212">Click **Add user**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="67b2f-213">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="67b2f-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="67b2f-214">V této části povolíte tak, že udělíte přístup tooPantheon toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="67b2f-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPantheon.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="67b2f-216">**tooassign Britta Simon tooPantheon, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="67b2f-216">**tooassign Britta Simon tooPantheon, perform hello following steps:**</span></span>

1. <span data-ttu-id="67b2f-217">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="67b2f-219">V seznamu aplikace hello vyberte **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-219">In hello applications list, select **Pantheon**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_app.png) 

3. <span data-ttu-id="67b2f-221">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="67b2f-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="67b2f-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="67b2f-223">Click **Add** button.</span></span> <span data-ttu-id="67b2f-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="67b2f-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="67b2f-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="67b2f-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="67b2f-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="67b2f-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="67b2f-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="67b2f-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="67b2f-229">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="67b2f-229">Testing single sign-on</span></span>

<span data-ttu-id="67b2f-230">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="67b2f-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="67b2f-231">Když kliknete na dlaždici Pantheon hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Pantheon aplikace.</span><span class="sxs-lookup"><span data-stu-id="67b2f-231">When you click hello Pantheon tile in hello Access Panel, you should get automatically signed-on tooyour Pantheon application.</span></span>
<span data-ttu-id="67b2f-232">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="67b2f-232">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="67b2f-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="67b2f-233">Additional resources</span></span>

* [<span data-ttu-id="67b2f-234">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="67b2f-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="67b2f-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="67b2f-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_203.png

