---
title: 'Kurz: Azure Active Directory integrace s xMatters OnDemand | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a xMatters OnDemand."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 7cc8f9f0d8cefc8a60b9514027437ced50c66242
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a><span data-ttu-id="58ba6-103">Kurz: Azure Active Directory integrace s xMatters OnDemand</span><span class="sxs-lookup"><span data-stu-id="58ba6-103">Tutorial: Azure Active Directory integration with xMatters OnDemand</span></span>

<span data-ttu-id="58ba6-104">V tomto kurzu zjistíte, jak toointegrate xMatters OnDemand službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="58ba6-104">In this tutorial, you learn how toointegrate xMatters OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="58ba6-105">Integrace xMatters OnDemand s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="58ba6-105">Integrating xMatters OnDemand with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="58ba6-106">Můžete řídit ve službě Azure AD, který má přístup tooxMatters OnDemand</span><span class="sxs-lookup"><span data-stu-id="58ba6-106">You can control in Azure AD who has access tooxMatters OnDemand</span></span>
- <span data-ttu-id="58ba6-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooxMatters OnDemand (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="58ba6-107">You can enable your users tooautomatically get signed-on tooxMatters OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="58ba6-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="58ba6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="58ba6-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="58ba6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58ba6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="58ba6-110">Prerequisites</span></span>

<span data-ttu-id="58ba6-111">tooconfigure integrace Azure AD s xMatters OnDemand, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="58ba6-111">tooconfigure Azure AD integration with xMatters OnDemand, you need hello following items:</span></span>

- <span data-ttu-id="58ba6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="58ba6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="58ba6-113">Předplatné povolené xMatters OnDemand jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="58ba6-113">A xMatters OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="58ba6-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="58ba6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="58ba6-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="58ba6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="58ba6-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="58ba6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="58ba6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="58ba6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="58ba6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="58ba6-118">Scenario description</span></span>
<span data-ttu-id="58ba6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="58ba6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="58ba6-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="58ba6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="58ba6-121">Přidání xMatters OnDemand z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="58ba6-121">Adding xMatters OnDemand from hello gallery</span></span>
2. <span data-ttu-id="58ba6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="58ba6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-xmatters-ondemand-from-hello-gallery"></a><span data-ttu-id="58ba6-123">Přidání xMatters OnDemand z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="58ba6-123">Adding xMatters OnDemand from hello gallery</span></span>
<span data-ttu-id="58ba6-124">tooconfigure hello integrace xMatters OnDemand do Azure AD, je nutné tooadd xMatters OnDemand hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="58ba6-124">tooconfigure hello integration of xMatters OnDemand into Azure AD, you need tooadd xMatters OnDemand from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="58ba6-125">**tooadd xMatters OnDemand z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="58ba6-125">**tooadd xMatters OnDemand from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="58ba6-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="58ba6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="58ba6-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="58ba6-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="58ba6-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="58ba6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="58ba6-133">Hello vyhledávacího pole zadejte **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-133">In hello search box, type **xMatters OnDemand**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. <span data-ttu-id="58ba6-135">Na panelu výsledků hello vyberte **xMatters OnDemand**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ba6-135">In hello results panel, select **xMatters OnDemand**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="58ba6-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="58ba6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="58ba6-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s xMatters OnDemand založené na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="58ba6-138">In this section, you configure and test Azure AD single sign-on with xMatters OnDemand based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="58ba6-139">Pro toowork jeden přihlašování, musí Azure AD tooknow jaké hello příslušného uživatele v xMatters OnDemand je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="58ba6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in xMatters OnDemand is tooa user in Azure AD.</span></span> <span data-ttu-id="58ba6-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v xMatters OnDemand musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="58ba6-140">In other words, a link relationship between an Azure AD user and hello related user in xMatters OnDemand needs toobe established.</span></span>

<span data-ttu-id="58ba6-141">V xMatters OnDemand přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="58ba6-141">In xMatters OnDemand, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="58ba6-142">tooconfigure a testu Azure AD jednotné přihlašování s xMatters OnDemand, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="58ba6-142">tooconfigure and test Azure AD single sign-on with xMatters OnDemand, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="58ba6-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="58ba6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="58ba6-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="58ba6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="58ba6-145">**[Vytvoření zkušebního uživatele OnDemand xMatters](#creating-a-xmatters-ondemand-test-user)**  -toohave protějšek Britta Simon v xMatters OnDemand, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="58ba6-145">**[Creating a xMatters OnDemand test user](#creating-a-xmatters-ondemand-test-user)** - toohave a counterpart of Britta Simon in xMatters OnDemand that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="58ba6-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="58ba6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="58ba6-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="58ba6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="58ba6-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="58ba6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="58ba6-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší xMatters OnDemand aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ba6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your xMatters OnDemand application.</span></span>

<span data-ttu-id="58ba6-150">**tooconfigure Azure AD jednotné přihlašování s xMatters OnDemand, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="58ba6-150">**tooconfigure Azure AD single sign-on with xMatters OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="58ba6-151">V portálu Azure, na hello hello **xMatters OnDemand** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-151">In hello Azure portal, on hello **xMatters OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="58ba6-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="58ba6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. <span data-ttu-id="58ba6-155">Na hello **xMatters OnDemand domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="58ba6-155">On hello **xMatters OnDemand Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    <span data-ttu-id="58ba6-157">a.</span><span class="sxs-lookup"><span data-stu-id="58ba6-157">a.</span></span> <span data-ttu-id="58ba6-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="58ba6-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>   
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    <span data-ttu-id="58ba6-159">b.</span><span class="sxs-lookup"><span data-stu-id="58ba6-159">b.</span></span> <span data-ttu-id="58ba6-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="58ba6-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > <span data-ttu-id="58ba6-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="58ba6-161">These values are not real.</span></span> <span data-ttu-id="58ba6-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="58ba6-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="58ba6-163">Obraťte se na [tým podpory xMatters OnDemand](https://www.xmatters.com/company/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="58ba6-163">Contact [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/) tooget these values.</span></span>

4. <span data-ttu-id="58ba6-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte hello soubor certifikátu místně jako **c:\\XMatters OnDemand.cer**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file locally as **c:\\XMatters OnDemand.cer**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="58ba6-166">Je třeba tooforward hello certifikát toohello [tým podpory xMatters OnDemand](https://www.xmatters.com/company/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="58ba6-166">You need tooforward hello certificate toohello [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/).</span></span> <span data-ttu-id="58ba6-167">certifikát Hello musí toobe odeslaný tým podpory xMatters hello předtím, než můžete dokončit hello jeden přihlašování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="58ba6-167">hello certificate needs toobe uploaded by hello xMatters support team before you can finalize hello single sign-on configuration.</span></span> 

5. <span data-ttu-id="58ba6-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="58ba6-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="58ba6-170">Na hello **xMatters OnDemand konfigurace** klikněte na tlačítko **konfigurace xMatters OnDemand** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="58ba6-170">On hello **xMatters OnDemand Configuration** section, click **Configure xMatters OnDemand** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="58ba6-171">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="58ba6-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. <span data-ttu-id="58ba6-173">V okně prohlížeče jiný web Přihlaste se tooyour XMatters OnDemand společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="58ba6-173">In a different web browser window, log in tooyour XMatters OnDemand company site as an administrator.</span></span>

8. <span data-ttu-id="58ba6-174">V panelu nástrojů hello hello nahoře, klikněte na **správce**a potom klikněte na **podrobnosti o společnosti** hello navigačním panelu na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="58ba6-174">In hello toolbar on hello top, click **Admin**, and then click **Company Details** in hello navigation bar on hello left side.</span></span>
   
    <span data-ttu-id="58ba6-175">![Správce](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "správce")</span><span class="sxs-lookup"><span data-stu-id="58ba6-175">![Admin](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")</span></span>

9. <span data-ttu-id="58ba6-176">Na hello **konfigurace SAML** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="58ba6-176">On hello **SAML Configuration** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="58ba6-177">![Konfigurace SAML](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="58ba6-177">![SAML configuration](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML configuration")</span></span>
   
    <span data-ttu-id="58ba6-178">a.</span><span class="sxs-lookup"><span data-stu-id="58ba6-178">a.</span></span> <span data-ttu-id="58ba6-179">Vyberte **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-179">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="58ba6-180">b.</span><span class="sxs-lookup"><span data-stu-id="58ba6-180">b.</span></span> <span data-ttu-id="58ba6-181">Vložení **SAML Entity ID**, který jste zkopírovali z hello portálu Azure do hello **ID zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="58ba6-181">Paste **SAML Entity ID**, which you have copied from hello Azure portal into hello **Identity Provider ID** textbox.</span></span>
   
    <span data-ttu-id="58ba6-182">c.</span><span class="sxs-lookup"><span data-stu-id="58ba6-182">c.</span></span> <span data-ttu-id="58ba6-183">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **jeden přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="58ba6-183">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Single Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="58ba6-184">d.</span><span class="sxs-lookup"><span data-stu-id="58ba6-184">d.</span></span> <span data-ttu-id="58ba6-185">Vložení **Sign-Out URL**, který jste zkopírovali z hello portálu Azure do hello **jednu adresu URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="58ba6-185">Paste **Sign-Out URL**, which you have copied from hello Azure portal into hello **Single Logout URL** textbox.</span></span>
   
    <span data-ttu-id="58ba6-186">e.</span><span class="sxs-lookup"><span data-stu-id="58ba6-186">e.</span></span> <span data-ttu-id="58ba6-187">Na stránce Podrobnosti o společnosti hello hello horní, klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-187">On hello Company Details page, at hello top, click **Save Changes**.</span></span>
    
    <span data-ttu-id="58ba6-188">![Podrobnosti o společnosti](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "společnosti podrobnosti")</span><span class="sxs-lookup"><span data-stu-id="58ba6-188">![Company details](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Company details")</span></span>

> [!TIP]
> <span data-ttu-id="58ba6-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="58ba6-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="58ba6-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="58ba6-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="58ba6-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="58ba6-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="58ba6-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="58ba6-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="58ba6-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="58ba6-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="58ba6-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="58ba6-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="58ba6-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="58ba6-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="58ba6-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="58ba6-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="58ba6-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="58ba6-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="58ba6-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="58ba6-204">a.</span><span class="sxs-lookup"><span data-stu-id="58ba6-204">a.</span></span> <span data-ttu-id="58ba6-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="58ba6-206">b.</span><span class="sxs-lookup"><span data-stu-id="58ba6-206">b.</span></span> <span data-ttu-id="58ba6-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="58ba6-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="58ba6-208">c.</span><span class="sxs-lookup"><span data-stu-id="58ba6-208">c.</span></span> <span data-ttu-id="58ba6-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="58ba6-210">d.</span><span class="sxs-lookup"><span data-stu-id="58ba6-210">d.</span></span> <span data-ttu-id="58ba6-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-211">Click **Create**.</span></span>
 
### <a name="creating-a-xmatters-ondemand-test-user"></a><span data-ttu-id="58ba6-212">Vytvoření zkušebního uživatele OnDemand xMatters</span><span class="sxs-lookup"><span data-stu-id="58ba6-212">Creating a xMatters OnDemand test user</span></span>

<span data-ttu-id="58ba6-213">V pořadí tooenable Azure AD Uživatelé toolog v tooXMatters OnDemand musí být zřízená do XMatters OnDemand.</span><span class="sxs-lookup"><span data-stu-id="58ba6-213">In order tooenable Azure AD users toolog in tooXMatters OnDemand, they must be provisioned into XMatters OnDemand.</span></span> <span data-ttu-id="58ba6-214">V případě hello XMatters OnDemand zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="58ba6-214">In hello case of XMatters OnDemand, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a><span data-ttu-id="58ba6-215">tooprovision uživatelské účty, provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="58ba6-215">tooprovision a user accounts, perform hello following steps:</span></span>
1. <span data-ttu-id="58ba6-216">Přihlaste se tooyour **XMatters OnDemand** klienta.</span><span class="sxs-lookup"><span data-stu-id="58ba6-216">Log in tooyour **XMatters OnDemand** tenant.</span></span>

2.  <span data-ttu-id="58ba6-217">Klikněte na tlačítko **uživatelé** kartu a pak klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-217">Click **Users** tab. and then click **Add User**.</span></span>
  
    <span data-ttu-id="58ba6-218">![Uživatelé](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="58ba6-218">![Users](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Users")</span></span>

3. <span data-ttu-id="58ba6-219">V hello **přidat uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="58ba6-219">In hello **Add a User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="58ba6-220">![Přidat uživatele](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="58ba6-220">![Add a User](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Add a User")</span></span>

    <span data-ttu-id="58ba6-221">a.</span><span class="sxs-lookup"><span data-stu-id="58ba6-221">a.</span></span> <span data-ttu-id="58ba6-222">Vyberte **Active**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-222">Select **Active**.</span></span>

    <span data-ttu-id="58ba6-223">b.</span><span class="sxs-lookup"><span data-stu-id="58ba6-223">b.</span></span> <span data-ttu-id="58ba6-224">V hello **ID uživatele** textovému poli, typ hello id uživatele jako Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="58ba6-224">In hello **User ID** textbox, type hello user id of user like Brittasimon@contoso.com.</span></span>
   
    <span data-ttu-id="58ba6-225">c.</span><span class="sxs-lookup"><span data-stu-id="58ba6-225">c.</span></span> <span data-ttu-id="58ba6-226">V hello **křestní jméno** textovému poli, typ křestní jméno uživatele hello jako Britta.</span><span class="sxs-lookup"><span data-stu-id="58ba6-226">In hello **First Name** textbox, type first name of hello user like Britta.</span></span>

    <span data-ttu-id="58ba6-227">d.</span><span class="sxs-lookup"><span data-stu-id="58ba6-227">d.</span></span> <span data-ttu-id="58ba6-228">V hello **příjmení** textovému poli, zadejte příjmení uživatele hello jako Simon aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ba6-228">In hello **Last Name** textbox, type last name of hello user like Simon.</span></span>
    
    <span data-ttu-id="58ba6-229">e.</span><span class="sxs-lookup"><span data-stu-id="58ba6-229">e.</span></span> <span data-ttu-id="58ba6-230">V hello **lokality** textovému poli, zadejte platnou lokalitou hello platný Azure AD účet chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="58ba6-230">In hello **Site** textbox, Enter hello valid site of a valid Azure AD account you want tooprovision.</span></span>
    
    <span data-ttu-id="58ba6-231">f.</span><span class="sxs-lookup"><span data-stu-id="58ba6-231">f.</span></span> <span data-ttu-id="58ba6-232">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-232">Click **Save**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="58ba6-233">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="58ba6-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="58ba6-234">V této části povolíte tak, že udělíte přístup tooxMatters OnDemand Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="58ba6-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooxMatters OnDemand.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="58ba6-236">**tooassign tooxMatters Britta Simon OnDemand, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="58ba6-236">**tooassign Britta Simon tooxMatters OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="58ba6-237">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="58ba6-239">V seznamu aplikace hello vyberte **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-239">In hello applications list, select **xMatters OnDemand**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. <span data-ttu-id="58ba6-241">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="58ba6-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="58ba6-243">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="58ba6-243">Click **Add** button.</span></span> <span data-ttu-id="58ba6-244">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="58ba6-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="58ba6-246">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="58ba6-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="58ba6-247">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="58ba6-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="58ba6-248">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="58ba6-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="58ba6-249">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="58ba6-249">Testing single sign-on</span></span>

<span data-ttu-id="58ba6-250">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="58ba6-250">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="58ba6-251">Po kliknutí na tlačítko hello xMatters OnDemand dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour xMatters OnDemand aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ba6-251">When you click hello xMatters OnDemand tile in hello Access Panel, you should get automatically signed-on tooyour xMatters OnDemand application.</span></span>
<span data-ttu-id="58ba6-252">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="58ba6-252">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58ba6-253">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="58ba6-253">Additional resources</span></span>

* [<span data-ttu-id="58ba6-254">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="58ba6-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="58ba6-255">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="58ba6-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_203.png

