---
title: 'Kurz: Azure Active Directory integrace s PolicyStat | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a PolicyStat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 868053cd0d37359fd9b4aeb93dba42cbbaa09845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a><span data-ttu-id="cb535-103">Kurz: Azure Active Directory integrace s PolicyStat</span><span class="sxs-lookup"><span data-stu-id="cb535-103">Tutorial: Azure Active Directory integration with PolicyStat</span></span>

<span data-ttu-id="cb535-104">V tomto kurzu zjistíte, jak toointegrate PolicyStat s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cb535-104">In this tutorial, you learn how toointegrate PolicyStat with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cb535-105">Integrace PolicyStat s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cb535-105">Integrating PolicyStat with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cb535-106">Můžete řídit ve službě Azure AD, který má přístup tooPolicyStat</span><span class="sxs-lookup"><span data-stu-id="cb535-106">You can control in Azure AD who has access tooPolicyStat</span></span>
- <span data-ttu-id="cb535-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPolicyStat (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cb535-107">You can enable your users tooautomatically get signed-on tooPolicyStat (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cb535-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cb535-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cb535-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cb535-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb535-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cb535-110">Prerequisites</span></span>

<span data-ttu-id="cb535-111">Integrace služby Azure AD s PolicyStat tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="cb535-111">tooconfigure Azure AD integration with PolicyStat, you need hello following items:</span></span>

- <span data-ttu-id="cb535-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cb535-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cb535-113">PolicyStat jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cb535-113">A PolicyStat single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cb535-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cb535-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cb535-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cb535-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cb535-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="cb535-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cb535-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cb535-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cb535-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cb535-118">Scenario description</span></span>
<span data-ttu-id="cb535-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cb535-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cb535-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cb535-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cb535-121">Přidání PolicyStat z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cb535-121">Adding PolicyStat from hello gallery</span></span>
2. <span data-ttu-id="cb535-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cb535-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-policystat-from-hello-gallery"></a><span data-ttu-id="cb535-123">Přidání PolicyStat z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cb535-123">Adding PolicyStat from hello gallery</span></span>
<span data-ttu-id="cb535-124">tooconfigure hello integrace PolicyStat do Azure AD, je nutné tooadd PolicyStat hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cb535-124">tooconfigure hello integration of PolicyStat into Azure AD, you need tooadd PolicyStat from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cb535-125">**tooadd PolicyStat z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cb535-125">**tooadd PolicyStat from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cb535-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cb535-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cb535-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cb535-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cb535-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cb535-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cb535-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cb535-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cb535-133">Hello vyhledávacího pole zadejte **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="cb535-133">In hello search box, type **PolicyStat**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. <span data-ttu-id="cb535-135">Na panelu výsledků hello vyberte **PolicyStat**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb535-135">In hello results panel, select **PolicyStat**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cb535-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cb535-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cb535-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PolicyStat podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="cb535-138">In this section, you configure and test Azure AD single sign-on with PolicyStat based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cb535-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v PolicyStat je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cb535-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PolicyStat is tooa user in Azure AD.</span></span> <span data-ttu-id="cb535-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v PolicyStat musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="cb535-140">In other words, a link relationship between an Azure AD user and hello related user in PolicyStat needs toobe established.</span></span>

<span data-ttu-id="cb535-141">V PolicyStat, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="cb535-141">In PolicyStat, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cb535-142">tooconfigure a testu Azure AD jednotné přihlašování s PolicyStat, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="cb535-142">tooconfigure and test Azure AD single sign-on with PolicyStat, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cb535-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="cb535-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cb535-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cb535-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cb535-145">**[Vytvoření zkušebního uživatele PolicyStat](#creating-a-policystat-test-user)**  -toohave protějšek Britta Simon v PolicyStat, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="cb535-145">**[Creating a PolicyStat test user](#creating-a-policystat-test-user)** - toohave a counterpart of Britta Simon in PolicyStat that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cb535-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cb535-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cb535-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="cb535-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cb535-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cb535-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cb535-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="cb535-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PolicyStat application.</span></span>

<span data-ttu-id="cb535-150">**tooconfigure Azure AD jednotné přihlašování s PolicyStat, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cb535-150">**tooconfigure Azure AD single sign-on with PolicyStat, perform hello following steps:**</span></span>

1. <span data-ttu-id="cb535-151">V portálu Azure, na hello hello **PolicyStat** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cb535-151">In hello Azure portal, on hello **PolicyStat** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cb535-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cb535-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. <span data-ttu-id="cb535-155">Na hello **PolicyStat domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cb535-155">On hello **PolicyStat Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    <span data-ttu-id="cb535-157">a.</span><span class="sxs-lookup"><span data-stu-id="cb535-157">a.</span></span> <span data-ttu-id="cb535-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.policystat.com`</span><span class="sxs-lookup"><span data-stu-id="cb535-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.policystat.com`</span></span>

    <span data-ttu-id="cb535-159">b.</span><span class="sxs-lookup"><span data-stu-id="cb535-159">b.</span></span> <span data-ttu-id="cb535-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.policystat.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="cb535-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.policystat.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cb535-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="cb535-161">These values are not real.</span></span> <span data-ttu-id="cb535-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="cb535-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cb535-163">Obraťte se na [tým podpory PolicyStat klienta](http://www.policystat.com/support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cb535-163">Contact [PolicyStat Client support team](http://www.policystat.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="cb535-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cb535-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. <span data-ttu-id="cb535-166">Hello cílem této části je toooutline jak tooPolicyStat tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="cb535-166">hello objective of this section is toooutline how tooenable users tooauthenticate tooPolicyStat with their account in Azure AD using federation based on hello SAML protocol.</span></span>

    <span data-ttu-id="cb535-167">Hello PolicyStat aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour **atributy tokenu SAML** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cb535-167">hello PolicyStat application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.</span></span>  

     <span data-ttu-id="cb535-168">Hello následující snímek obrazovky ukazuje příklad tohoto objektu.</span><span class="sxs-lookup"><span data-stu-id="cb535-168">hello following screenshot shows an example of this.</span></span>

     <span data-ttu-id="cb535-169">![Atributy](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="cb535-169">![Attributes](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "Attributes")</span></span>

6. <span data-ttu-id="cb535-170">mapování atributů tooadd hello vyžaduje, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cb535-170">tooadd hello required attribute mappings, perform hello following steps:</span></span>

    | <span data-ttu-id="cb535-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="cb535-171">Attribute Name</span></span>    |   <span data-ttu-id="cb535-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="cb535-172">Attribute Value</span></span> |
    |------------------- | -------------------- |
    | <span data-ttu-id="cb535-173">UID</span><span class="sxs-lookup"><span data-stu-id="cb535-173">uid</span></span> | <span data-ttu-id="cb535-174">ExtractMailPrefix([mail])</span><span class="sxs-lookup"><span data-stu-id="cb535-174">ExtractMailPrefix([mail])</span></span> |
    
    <span data-ttu-id="cb535-175">a.</span><span class="sxs-lookup"><span data-stu-id="cb535-175">a.</span></span> <span data-ttu-id="cb535-176">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cb535-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    <span data-ttu-id="cb535-179">b.</span><span class="sxs-lookup"><span data-stu-id="cb535-179">b.</span></span> <span data-ttu-id="cb535-180">V hello **název atributu** textovému poli, typ **uid**.</span><span class="sxs-lookup"><span data-stu-id="cb535-180">In hello **Attribute Name** textbox, type **uid**.</span></span>

    <span data-ttu-id="cb535-181">c.</span><span class="sxs-lookup"><span data-stu-id="cb535-181">c.</span></span> <span data-ttu-id="cb535-182">V hello **hodnota atributu** textovému poli, vyberte **ExtractMailPrefix()**.</span><span class="sxs-lookup"><span data-stu-id="cb535-182">In hello **Attribute Value** textbox, select **ExtractMailPrefix()**.</span></span>    
   
    <span data-ttu-id="cb535-183">d.</span><span class="sxs-lookup"><span data-stu-id="cb535-183">d.</span></span> <span data-ttu-id="cb535-184">Z hello **e-mailu** seznamu, vyberte **User.mail**.</span><span class="sxs-lookup"><span data-stu-id="cb535-184">From hello **Mail** list, select **User.mail**.</span></span>
    
    <span data-ttu-id="cb535-185">e.</span><span class="sxs-lookup"><span data-stu-id="cb535-185">e.</span></span> <span data-ttu-id="cb535-186">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="cb535-186">Click **Ok**</span></span>

7. <span data-ttu-id="cb535-187">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cb535-187">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="cb535-189">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="cb535-189">In a different web browser window, log into your PolicyStat company site as an administrator.</span></span>

9. <span data-ttu-id="cb535-190">Klikněte na tlačítko hello **správce** a pak klikněte **konfigurace přihlášení** v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="cb535-190">Click hello **Admin** tab, and then click **Single Sign-On Configuration** in left navigation pane.</span></span>
   
    <span data-ttu-id="cb535-191">![Správce nabídek](./media/active-directory-saas-policystat-tutorial/ic808633.png "správce nabídek")</span><span class="sxs-lookup"><span data-stu-id="cb535-191">![Administrator Menu](./media/active-directory-saas-policystat-tutorial/ic808633.png "Administrator Menu")</span></span>

10. <span data-ttu-id="cb535-192">V hello **instalace** vyberte **povolit jeden integrace přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cb535-192">In hello **Setup** section, select **Enable Single Sign-on Integration**.</span></span>
   
    <span data-ttu-id="cb535-193">![Jednotné přihlašování konfigurace](./media/active-directory-saas-policystat-tutorial/ic808634.png "jednotné přihlašování v konfiguraci")</span><span class="sxs-lookup"><span data-stu-id="cb535-193">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808634.png "Single Sign-On Configuration")</span></span>

11. <span data-ttu-id="cb535-194">Klikněte na tlačítko **konfigurace atributů**a pak na hello **konfigurace atributů** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cb535-194">Click **Configure Attributes**, and then, in hello **Configure Attributes** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="cb535-195">![Jednotné přihlašování konfigurace](./media/active-directory-saas-policystat-tutorial/ic808635.png "jednotné přihlašování v konfiguraci")</span><span class="sxs-lookup"><span data-stu-id="cb535-195">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808635.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="cb535-196">a.</span><span class="sxs-lookup"><span data-stu-id="cb535-196">a.</span></span> <span data-ttu-id="cb535-197">V hello **uživatelské jméno atribut** textovému poli, typ **uid**.</span><span class="sxs-lookup"><span data-stu-id="cb535-197">In hello **Username Attribute** textbox, type **uid**.</span></span>

    <span data-ttu-id="cb535-198">b.</span><span class="sxs-lookup"><span data-stu-id="cb535-198">b.</span></span> <span data-ttu-id="cb535-199">V hello **křestní jméno atribut** textovému poli, typ **firstname** uživatele **Britta**.</span><span class="sxs-lookup"><span data-stu-id="cb535-199">In hello **First Name Attribute** textbox, type **firstname** of user **Britta**.</span></span>

    <span data-ttu-id="cb535-200">c.</span><span class="sxs-lookup"><span data-stu-id="cb535-200">c.</span></span> <span data-ttu-id="cb535-201">V hello **poslední atribut Name** textovému poli, typ **lastname** uživatele **Simon**.</span><span class="sxs-lookup"><span data-stu-id="cb535-201">In hello **Last Name Attribute** textbox, type **lastname** of user **Simon**.</span></span>

    <span data-ttu-id="cb535-202">d.</span><span class="sxs-lookup"><span data-stu-id="cb535-202">d.</span></span> <span data-ttu-id="cb535-203">V hello **atribut e-mailu** textovému poli, typ **emailaddress** uživatele  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="cb535-203">In hello **Email Attribute** textbox, type **emailaddress** of user **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="cb535-204">e.</span><span class="sxs-lookup"><span data-stu-id="cb535-204">e.</span></span> <span data-ttu-id="cb535-205">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="cb535-205">Click **Save Changes**.</span></span>

12. <span data-ttu-id="cb535-206">Klikněte na tlačítko **si Metadata IDP**a pak na hello **si Metadata IDP** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cb535-206">Click **Your IDP Metadata**, and then, in hello **Your IDP Metadata** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="cb535-207">![Jednotné přihlašování konfigurace](./media/active-directory-saas-policystat-tutorial/ic808636.png "jednotné přihlašování v konfiguraci")</span><span class="sxs-lookup"><span data-stu-id="cb535-207">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808636.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="cb535-208">a.</span><span class="sxs-lookup"><span data-stu-id="cb535-208">a.</span></span> <span data-ttu-id="cb535-209">Otevřete soubor stažený metadat, hello kopírování obsahu a pak ji vložit do hello **vaše metadat zprostředkovatelů Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="cb535-209">Open your downloaded metadata file, copy hello content, and  then paste it into hello **Your Identity Provider Metadata** textbox.</span></span>

    <span data-ttu-id="cb535-210">b.</span><span class="sxs-lookup"><span data-stu-id="cb535-210">b.</span></span> <span data-ttu-id="cb535-211">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="cb535-211">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="cb535-212">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="cb535-212">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cb535-213">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="cb535-213">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cb535-214">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cb535-214">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cb535-215">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cb535-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="cb535-216">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="cb535-216">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cb535-218">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cb535-218">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cb535-219">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cb535-219">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cb535-221">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cb535-221">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cb535-223">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="cb535-223">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cb535-225">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cb535-225">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cb535-227">a.</span><span class="sxs-lookup"><span data-stu-id="cb535-227">a.</span></span> <span data-ttu-id="cb535-228">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cb535-228">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cb535-229">b.</span><span class="sxs-lookup"><span data-stu-id="cb535-229">b.</span></span> <span data-ttu-id="cb535-230">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cb535-230">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cb535-231">c.</span><span class="sxs-lookup"><span data-stu-id="cb535-231">c.</span></span> <span data-ttu-id="cb535-232">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cb535-232">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cb535-233">d.</span><span class="sxs-lookup"><span data-stu-id="cb535-233">d.</span></span> <span data-ttu-id="cb535-234">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cb535-234">Click **Create**.</span></span>
 
### <a name="creating-a-policystat-test-user"></a><span data-ttu-id="cb535-235">Vytvoření zkušebního uživatele PolicyStat</span><span class="sxs-lookup"><span data-stu-id="cb535-235">Creating a PolicyStat test user</span></span>

<span data-ttu-id="cb535-236">V pořadí tooenable Azure AD Uživatelé toolog do PolicyStat musí být zřízená do PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="cb535-236">In order tooenable Azure AD users toolog into PolicyStat, they must be provisioned into PolicyStat.</span></span>  

<span data-ttu-id="cb535-237">PolicyStat podporuje jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="cb535-237">PolicyStat supports just in time user provisioning.</span></span> <span data-ttu-id="cb535-238">To znamená, tooadd hello uživatelů není nutné ručně tooPolicyStat.</span><span class="sxs-lookup"><span data-stu-id="cb535-238">This means, you do not need tooadd hello users manually tooPolicyStat.</span></span> <span data-ttu-id="cb535-239">Hello uživatelé budou se přidají automaticky na jejich první přihlášení pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cb535-239">hello users will get added automatically on their first login through SSO.</span></span>

>[!NOTE]
><span data-ttu-id="cb535-240">Můžete použít všechny ostatní PolicyStat uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované PolicyStat tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cb535-240">You can use any other PolicyStat user account creation tools or APIs provided by PolicyStat tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cb535-241">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="cb535-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cb535-242">V této části povolíte tak, že udělíte přístup tooPolicyStat toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cb535-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPolicyStat.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cb535-244">**tooassign Britta Simon tooPolicyStat, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cb535-244">**tooassign Britta Simon tooPolicyStat, perform hello following steps:**</span></span>

1. <span data-ttu-id="cb535-245">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cb535-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cb535-247">V seznamu aplikace hello vyberte **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="cb535-247">In hello applications list, select **PolicyStat**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. <span data-ttu-id="cb535-249">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cb535-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cb535-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cb535-251">Click **Add** button.</span></span> <span data-ttu-id="cb535-252">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cb535-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cb535-254">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="cb535-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cb535-255">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cb535-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cb535-256">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cb535-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cb535-257">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cb535-257">Testing single sign-on</span></span>

<span data-ttu-id="cb535-258">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cb535-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cb535-259">Když kliknete na dlaždici PolicyStat hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour PolicyStat aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb535-259">When you click hello PolicyStat tile in hello Access Panel, you should get automatically signed-on tooyour PolicyStat application.</span></span>
<span data-ttu-id="cb535-260">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cb535-260">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cb535-261">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cb535-261">Additional resources</span></span>

* [<span data-ttu-id="cb535-262">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cb535-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cb535-263">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cb535-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

