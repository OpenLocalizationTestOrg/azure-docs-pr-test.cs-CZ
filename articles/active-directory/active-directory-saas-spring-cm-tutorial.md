---
title: 'Kurz: Azure Active Directory integrace s SpringCM | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SpringCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 12c8ebe765e2c6e61115256e9343d90ec132e1f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a><span data-ttu-id="ddcbc-103">Kurz: Azure Active Directory integrace s SpringCM</span><span class="sxs-lookup"><span data-stu-id="ddcbc-103">Tutorial: Azure Active Directory integration with SpringCM</span></span>

<span data-ttu-id="ddcbc-104">V tomto kurzu zjistíte, jak toointegrate SpringCM s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ddcbc-104">In this tutorial, you learn how toointegrate SpringCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ddcbc-105">Integrace SpringCM s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ddcbc-105">Integrating SpringCM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ddcbc-106">Můžete řídit ve službě Azure AD, který má přístup tooSpringCM</span><span class="sxs-lookup"><span data-stu-id="ddcbc-106">You can control in Azure AD who has access tooSpringCM</span></span>
- <span data-ttu-id="ddcbc-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSpringCM (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ddcbc-107">You can enable your users tooautomatically get signed-on tooSpringCM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ddcbc-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ddcbc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ddcbc-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ddcbc-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ddcbc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ddcbc-110">Prerequisites</span></span>

<span data-ttu-id="ddcbc-111">Integrace služby Azure AD s SpringCM tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ddcbc-111">tooconfigure Azure AD integration with SpringCM, you need hello following items:</span></span>

- <span data-ttu-id="ddcbc-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ddcbc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ddcbc-113">SpringCM jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ddcbc-113">A SpringCM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ddcbc-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ddcbc-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ddcbc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ddcbc-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ddcbc-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ddcbc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ddcbc-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ddcbc-118">Scenario description</span></span>
<span data-ttu-id="ddcbc-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ddcbc-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ddcbc-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ddcbc-121">Přidání SpringCM z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ddcbc-121">Adding SpringCM from hello gallery</span></span>
2. <span data-ttu-id="ddcbc-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ddcbc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-springcm-from-hello-gallery"></a><span data-ttu-id="ddcbc-123">Přidání SpringCM z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ddcbc-123">Adding SpringCM from hello gallery</span></span>
<span data-ttu-id="ddcbc-124">tooconfigure hello integrace SpringCM do Azure AD, je nutné tooadd SpringCM hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-124">tooconfigure hello integration of SpringCM into Azure AD, you need tooadd SpringCM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ddcbc-125">**tooadd SpringCM z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ddcbc-125">**tooadd SpringCM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ddcbc-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ddcbc-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ddcbc-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ddcbc-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ddcbc-133">Hello vyhledávacího pole zadejte **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-133">In hello search box, type **SpringCM**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_search.png)

5. <span data-ttu-id="ddcbc-135">Na panelu výsledků hello vyberte **SpringCM**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-135">In hello results panel, select **SpringCM**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ddcbc-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ddcbc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ddcbc-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SpringCM podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="ddcbc-138">In this section, you configure and test Azure AD single sign-on with SpringCM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ddcbc-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v SpringCM je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SpringCM is tooa user in Azure AD.</span></span> <span data-ttu-id="ddcbc-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SpringCM musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-140">In other words, a link relationship between an Azure AD user and hello related user in SpringCM needs toobe established.</span></span>

<span data-ttu-id="ddcbc-141">V SpringCM, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-141">In SpringCM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ddcbc-142">tooconfigure a testu Azure AD jednotné přihlašování s SpringCM, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="ddcbc-142">tooconfigure and test Azure AD single sign-on with SpringCM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ddcbc-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ddcbc-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ddcbc-145">**[Vytvoření zkušebního uživatele SpringCM](#creating-a-springcm-test-user)**  -toohave protějšek Britta Simon v SpringCM, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-145">**[Creating a SpringCM test user](#creating-a-springcm-test-user)** - toohave a counterpart of Britta Simon in SpringCM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ddcbc-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ddcbc-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ddcbc-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ddcbc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ddcbc-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SpringCM.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SpringCM application.</span></span>

<span data-ttu-id="ddcbc-150">**tooconfigure Azure AD jednotné přihlašování s SpringCM, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ddcbc-150">**tooconfigure Azure AD single sign-on with SpringCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="ddcbc-151">V portálu Azure, na hello hello **SpringCM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-151">In hello Azure portal, on hello **SpringCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ddcbc-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_samlbase.png)

3. <span data-ttu-id="ddcbc-155">Na hello **SpringCM domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ddcbc-155">On hello **SpringCM Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_url.png)

    <span data-ttu-id="ddcbc-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="ddcbc-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ddcbc-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-158">This value is not real.</span></span> <span data-ttu-id="ddcbc-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="ddcbc-160">Obraťte se na [tým podpory SpringCM klienta](https://knowledge.springcm.com/support) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-160">Contact [SpringCM Client support team](https://knowledge.springcm.com/support) tooget this value.</span></span> 
 
4. <span data-ttu-id="ddcbc-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-161">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_certificate.png) 

5. <span data-ttu-id="ddcbc-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ddcbc-165">Na hello **SpringCM konfigurace** klikněte na tlačítko **konfigurace SpringCM** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-165">On hello **SpringCM Configuration** section, click **Configure SpringCM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ddcbc-166">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ddcbc-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_configure.png)   

7. <span data-ttu-id="ddcbc-168">V okně prohlížeče jiných webových přihlásit tooyour **SpringCM** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-168">In a different web browser window, sign on tooyour **SpringCM** company site as administrator.</span></span>

8. <span data-ttu-id="ddcbc-169">V nabídce hello hello nahoře, klikněte na tlačítko **přejít na**, klikněte na tlačítko **Předvolby**a pak na hello **předvolby účtu** klikněte na tlačítko **jednotné přihlašování SAML**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-169">In hello menu on hello top, click **GO TO**, click **Preferences**, and then, in hello **Account Preferences** section, click **SAML SSO**.</span></span>
   
    <span data-ttu-id="ddcbc-170">![JEDNOTNÉ PŘIHLAŠOVÁNÍ SAML](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "JEDNOTNÉ PŘIHLAŠOVÁNÍ SAML")</span><span class="sxs-lookup"><span data-stu-id="ddcbc-170">![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")</span></span>

9. <span data-ttu-id="ddcbc-171">V oddílu konfigurace zprostředkovatele Identity hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ddcbc-171">In hello Identity Provider Configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="ddcbc-172">![Konfigurace zprostředkovatele identity](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "konfigurace zprostředkovatele Identity")</span><span class="sxs-lookup"><span data-stu-id="ddcbc-172">![Identity Provider Configuration](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "Identity Provider Configuration")</span></span>
    
    <span data-ttu-id="ddcbc-173">a.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-173">a.</span></span> <span data-ttu-id="ddcbc-174">tooupload stažený certifikát Azure Active Directory, klikněte na tlačítko **vybrat certifikát vystavitele** nebo **změnu vystavitele certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-174">tooupload your downloaded Azure Active Directory certificate, click **Select Issuer Certificate** or **Change Issuer Certificate**.</span></span>
    
    <span data-ttu-id="ddcbc-175">b.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-175">b.</span></span> <span data-ttu-id="ddcbc-176">Vložení **SAML Entity ID** hodnotu, kterou jste zkopírovali z portálu Azure do hello **vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-176">Paste **SAML Entity ID** value, which you have copied from Azure portal into hello **Issuer** textbox.</span></span>
    
    <span data-ttu-id="ddcbc-177">c.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-177">c.</span></span> <span data-ttu-id="ddcbc-178">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **koncový bod iniciované poskytovatele služeb (SP)** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-178">Paste **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **Service Provider (SP) Initiated Endpoint** textbox.</span></span>
            
    <span data-ttu-id="ddcbc-179">d.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-179">d.</span></span> <span data-ttu-id="ddcbc-180">Vyberte **povoleno SAML** jako **povolit**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-180">Select **SAML Enabled** as **Enable**.</span></span>

    <span data-ttu-id="ddcbc-181">e.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-181">e.</span></span> <span data-ttu-id="ddcbc-182">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-182">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="ddcbc-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ddcbc-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ddcbc-184">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ddcbc-185">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ddcbc-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ddcbc-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ddcbc-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="ddcbc-187">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ddcbc-189">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ddcbc-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ddcbc-190">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ddcbc-192">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ddcbc-194">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ddcbc-196">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ddcbc-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ddcbc-198">a.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-198">a.</span></span> <span data-ttu-id="ddcbc-199">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ddcbc-200">b.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-200">b.</span></span> <span data-ttu-id="ddcbc-201">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ddcbc-202">c.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-202">c.</span></span> <span data-ttu-id="ddcbc-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ddcbc-204">d.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-204">d.</span></span> <span data-ttu-id="ddcbc-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-205">Click **Create**.</span></span>
 
### <a name="creating-a-springcm-test-user"></a><span data-ttu-id="ddcbc-206">Vytvoření zkušebního uživatele SpringCM</span><span class="sxs-lookup"><span data-stu-id="ddcbc-206">Creating a SpringCM test user</span></span>

<span data-ttu-id="ddcbc-207">toolog uživatelé Azure Active Directory tooenable v tooSpringCM, se musí být zřízená do SpringCM.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-207">tooenable Azure Active Directory users toolog in tooSpringCM, they must be provisioned into SpringCM.</span></span> <span data-ttu-id="ddcbc-208">V případě hello SpringCM zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-208">In hello case of SpringCM, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="ddcbc-209">Další informace najdete v tématu [vytvoření a úprava uživatele SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span><span class="sxs-lookup"><span data-stu-id="ddcbc-209">For more information, see [Create and Edit a SpringCM User](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span></span> 

<span data-ttu-id="ddcbc-210">**tooprovision tooSpringCM účet uživatele, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ddcbc-210">**tooprovision a user account tooSpringCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="ddcbc-211">Přihlaste se tooyour **SpringCM** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-211">Log in tooyour **SpringCM** company site as administrator.</span></span>

2. <span data-ttu-id="ddcbc-212">Klikněte na tlačítko **GOTO**a potom klikněte na **adresáře**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-212">Click **GOTO**, and then click **ADDRESS BOOK**.</span></span>
   
    <span data-ttu-id="ddcbc-213">![Vytvoření uživatele](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="ddcbc-213">![Create User](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "Create User")</span></span>

3. <span data-ttu-id="ddcbc-214">Klikněte na tlačítko **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-214">Click **Create User**.</span></span>

4. <span data-ttu-id="ddcbc-215">Vyberte **Role uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-215">Select a **User Role**.</span></span>

5. <span data-ttu-id="ddcbc-216">Vyberte **odeslání e-mailu aktivace**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-216">Select **Send Activation Email**.</span></span>

6. <span data-ttu-id="ddcbc-217">Typ hello křestní jméno, příjmení a e-mailovou adresu platný uživatelský účet služby Azure Active Directory, že který má tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-217">Type hello first name, last name, and email address of a valid Azure Active Directory user account you want tooprovision into hello related textboxes.</span></span>

7. <span data-ttu-id="ddcbc-218">Přidat uživatele tooa hello **skupiny zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-218">Add hello user tooa **Security group**.</span></span>

8. <span data-ttu-id="ddcbc-219">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-219">Click **Save**.</span></span>

  >[!NOTE]
  ><span data-ttu-id="ddcbc-220">Můžete použít všechny ostatní SpringCM uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované SpringCM tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-220">You can use any other SpringCM user account creation tools or APIs provided by SpringCM tooprovision AAD user accounts.</span></span>  
  > 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ddcbc-221">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ddcbc-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ddcbc-222">V této části povolíte tak, že udělíte přístup tooSpringCM toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSpringCM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ddcbc-224">**tooassign Britta Simon tooSpringCM, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ddcbc-224">**tooassign Britta Simon tooSpringCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="ddcbc-225">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ddcbc-227">V seznamu aplikace hello vyberte **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-227">In hello applications list, select **SpringCM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_app.png) 

3. <span data-ttu-id="ddcbc-229">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ddcbc-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-231">Click **Add** button.</span></span> <span data-ttu-id="ddcbc-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ddcbc-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ddcbc-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ddcbc-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ddcbc-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ddcbc-237">Testing single sign-on</span></span>

<span data-ttu-id="ddcbc-238">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="ddcbc-239">Když kliknete na dlaždici SpringCM hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SpringCM aplikace.</span><span class="sxs-lookup"><span data-stu-id="ddcbc-239">When you click hello SpringCM tile in hello Access Panel, you should get automatically signed-on tooyour SpringCM application.</span></span>

<span data-ttu-id="ddcbc-240">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ddcbc-240">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ddcbc-241">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ddcbc-241">Additional resources</span></span>

* [<span data-ttu-id="ddcbc-242">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ddcbc-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ddcbc-243">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ddcbc-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_203.png

