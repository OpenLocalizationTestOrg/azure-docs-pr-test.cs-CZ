---
title: 'Kurz: Azure Active Directory integrace s Salesforce | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1d848518ee30910e051cdc4746c599219f3b5a3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a><span data-ttu-id="65217-103">Kurz: Azure Active Directory integrace s Salesforce</span><span class="sxs-lookup"><span data-stu-id="65217-103">Tutorial: Azure Active Directory integration with Salesforce</span></span>

<span data-ttu-id="65217-104">V tomto kurzu zjistíte, jak toointegrate Salesforce službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="65217-104">In this tutorial, you learn how toointegrate Salesforce with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="65217-105">Integrace služby Salesforce s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="65217-105">Integrating Salesforce with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="65217-106">Můžete řídit ve službě Azure AD, který má přístup tooSalesforce</span><span class="sxs-lookup"><span data-stu-id="65217-106">You can control in Azure AD who has access tooSalesforce</span></span>
- <span data-ttu-id="65217-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSalesforce (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="65217-107">You can enable your users tooautomatically get signed-on tooSalesforce (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="65217-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="65217-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="65217-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="65217-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65217-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="65217-110">Prerequisites</span></span>

<span data-ttu-id="65217-111">tooconfigure integrace Azure AD pomocí služby Salesforce, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="65217-111">tooconfigure Azure AD integration with Salesforce, you need hello following items:</span></span>

- <span data-ttu-id="65217-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="65217-112">An Azure AD subscription</span></span>
- <span data-ttu-id="65217-113">Služby Salesforce jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="65217-113">A Salesforce single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="65217-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="65217-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="65217-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="65217-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="65217-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="65217-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="65217-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="65217-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="65217-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="65217-118">Scenario description</span></span>
<span data-ttu-id="65217-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="65217-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="65217-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="65217-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="65217-121">Přidání Salesforce z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="65217-121">Adding Salesforce from hello gallery</span></span>
2. <span data-ttu-id="65217-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="65217-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-from-hello-gallery"></a><span data-ttu-id="65217-123">Přidání Salesforce z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="65217-123">Adding Salesforce from hello gallery</span></span>
<span data-ttu-id="65217-124">tooconfigure hello integrace služby Salesforce do Azure AD, je nutné tooadd Salesforce hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="65217-124">tooconfigure hello integration of Salesforce into Azure AD, you need tooadd Salesforce from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="65217-125">**tooadd Salesforce z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="65217-125">**tooadd Salesforce from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="65217-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="65217-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="65217-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="65217-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="65217-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="65217-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="65217-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="65217-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="65217-133">Hello vyhledávacího pole zadejte **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="65217-133">In hello search box, type **Salesforce**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. <span data-ttu-id="65217-135">Na panelu výsledků hello vyberte **Salesforce**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="65217-135">In hello results panel, select **Salesforce**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="65217-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="65217-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="65217-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Salesforce podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="65217-138">In this section, you configure and test Azure AD single sign-on with Salesforce based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="65217-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Salesforce je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="65217-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Salesforce is tooa user in Azure AD.</span></span> <span data-ttu-id="65217-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Salesforce musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="65217-140">In other words, a link relationship between an Azure AD user and hello related user in Salesforce needs toobe established.</span></span>

<span data-ttu-id="65217-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="65217-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Salesforce.</span></span>

<span data-ttu-id="65217-142">tooconfigure a testování Azure AD jednotné přihlašování pomocí služby Salesforce, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="65217-142">tooconfigure and test Azure AD single sign-on with Salesforce, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="65217-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="65217-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="65217-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="65217-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="65217-145">**[Vytvoření zkušebního uživatele Salesforce](#creating-a-salesforce-test-user)**  -toohave protějšek Britta Simon v Salesforce, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="65217-145">**[Creating a Salesforce test user](#creating-a-salesforce-test-user)** - toohave a counterpart of Britta Simon in Salesforce that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="65217-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="65217-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="65217-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="65217-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="65217-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="65217-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="65217-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Salesforce.</span><span class="sxs-lookup"><span data-stu-id="65217-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Salesforce application.</span></span>

<span data-ttu-id="65217-150">**tooconfigure Azure AD jednotné přihlašování pomocí služby Salesforce, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="65217-150">**tooconfigure Azure AD single sign-on with Salesforce, perform hello following steps:**</span></span>

1. <span data-ttu-id="65217-151">V portálu Azure, na hello hello **Salesforce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="65217-151">In hello Azure portal, on hello **Salesforce** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="65217-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="65217-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. <span data-ttu-id="65217-155">Na hello **Salesforce domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="65217-155">On hello **Salesforce Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    <span data-ttu-id="65217-157">V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="65217-157">In hello **Sign-on URL** textbox, type hello value using hello following pattern:</span></span> 
   * <span data-ttu-id="65217-158">Účet organizace:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="65217-158">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
   * <span data-ttu-id="65217-159">Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="65217-159">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="65217-160">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="65217-160">These values are not hello real.</span></span> <span data-ttu-id="65217-161">Tyto hodnoty aktualizujte s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="65217-161">Update these values with hello actual Sign-on URL.</span></span> <span data-ttu-id="65217-162">Obraťte se na [tým podpory služby Salesforce klienta](https://help.salesforce.com/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="65217-162">Contact [Salesforce Client support team](https://help.salesforce.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="65217-163">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="65217-163">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. <span data-ttu-id="65217-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="65217-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="65217-167">Na hello **Salesforce konfigurace** klikněte na tlačítko **konfigurace Salesforce** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="65217-167">On hello **Salesforce Configuration** section, click **Configure Salesforce** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="65217-168">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="65217-168">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span> 

    <span data-ttu-id="65217-169">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="65217-169">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span></span>
7.  <span data-ttu-id="65217-170">V prohlížeči otevřete novou kartu a přihlaste se tooyour správce účtu Salesforce.</span><span class="sxs-lookup"><span data-stu-id="65217-170">Open a new tab in your browser and log in tooyour Salesforce administrator account.</span></span>

8.  <span data-ttu-id="65217-171">V části hello **správce** navigačním podokně klikněte na tlačítko **ovládacích prvků zabezpečení** tooexpand hello související části.</span><span class="sxs-lookup"><span data-stu-id="65217-171">Under hello **Administrator** navigation pane, click **Security Controls** tooexpand hello related section.</span></span> <span data-ttu-id="65217-172">Pak klikněte na tlačítko **nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="65217-172">Then click **Single Sign-On Settings**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  <span data-ttu-id="65217-174">Na hello **nastavení jednotného přihlašování** klikněte na tlačítko hello **upravit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="65217-174">On hello **Single Sign-On Settings** page, click hello **Edit** button.</span></span>
    <span data-ttu-id="65217-175">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span><span class="sxs-lookup"><span data-stu-id="65217-175">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span></span>

      > [!NOTE]
      > <span data-ttu-id="65217-176">Pokud jste nelze tooenable jednotné přihlašování v nastavení účtu služby Salesforce, může být nutné toocontact [tým podpory služby Salesforce klienta](https://help.salesforce.com/support).</span><span class="sxs-lookup"><span data-stu-id="65217-176">If you are unable tooenable Single Sign-On settings for your Salesforce account, you may need toocontact [Salesforce Client support team](https://help.salesforce.com/support).</span></span> 

10. <span data-ttu-id="65217-177">Vyberte **povoleno SAML**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="65217-177">Select **SAML Enabled**, and then click **Save**.</span></span>

      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. <span data-ttu-id="65217-179">tooconfigure SAML jeden přihlašování nastavení, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="65217-179">tooconfigure your SAML single sign-on settings, click **New**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. <span data-ttu-id="65217-181">Na hello **SAML jeden přihlašování nastavení upravit** vyberte hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="65217-181">On hello **SAML Single Sign-On Setting Edit** page, make hello following configurations:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    <span data-ttu-id="65217-183">a.</span><span class="sxs-lookup"><span data-stu-id="65217-183">a.</span></span> <span data-ttu-id="65217-184">Pro hello **název** pole, zadejte popisný název pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="65217-184">For hello **Name** field, type in a friendly name for this configuration.</span></span> <span data-ttu-id="65217-185">Poskytuje hodnotu pro **název** automaticky vyplnit hello **název rozhraní API** textové pole.</span><span class="sxs-lookup"><span data-stu-id="65217-185">Providing a value for **Name** automatically populate hello **API Name** textbox.</span></span>

    <span data-ttu-id="65217-186">b.</span><span class="sxs-lookup"><span data-stu-id="65217-186">b.</span></span> <span data-ttu-id="65217-187">Vložení **SMAL Entity ID** hodnotu do hello **vystavitele** pole v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="65217-187">Paste **SMAL Entity ID** value into hello **Issuer** field in Salesforce.</span></span>

    <span data-ttu-id="65217-188">c.</span><span class="sxs-lookup"><span data-stu-id="65217-188">c.</span></span> <span data-ttu-id="65217-189">V hello **textové pole Entity Id**, zadejte název domény vaší služby Salesforce pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="65217-189">In hello **Entity Id textbox**, type your Salesforce domain name using hello following pattern:</span></span>
      
      * <span data-ttu-id="65217-190">Účet organizace:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="65217-190">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
      * <span data-ttu-id="65217-191">Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="65217-191">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>
      
    <span data-ttu-id="65217-192">d.</span><span class="sxs-lookup"><span data-stu-id="65217-192">d.</span></span> <span data-ttu-id="65217-193">Klikněte na tlačítko **Procházet** nebo **zvolit soubor** tooopen hello **zvolit soubor tooUpload** dialogovém okně, vyberte svůj certifikát Salesforce a pak klikněte na tlačítko **otevřete**tooupload hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="65217-193">Click **Browse** or **Choose File** tooopen hello **Choose File tooUpload** dialog, select your Salesforce certificate, and then click **Open** tooupload hello certificate.</span></span>

    <span data-ttu-id="65217-194">e.</span><span class="sxs-lookup"><span data-stu-id="65217-194">e.</span></span> <span data-ttu-id="65217-195">Pro **typ Identity SAML**, vyberte **kontrolní výraz obsahuje uživatelské jméno uživatele salesforce.com**.</span><span class="sxs-lookup"><span data-stu-id="65217-195">For **SAML Identity Type**, select **Assertion contains User's salesforce.com username**.</span></span>

    <span data-ttu-id="65217-196">f.</span><span class="sxs-lookup"><span data-stu-id="65217-196">f.</span></span> <span data-ttu-id="65217-197">Pro **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier hello hello předmětu – příkaz**</span><span class="sxs-lookup"><span data-stu-id="65217-197">For **SAML Identity Location**, select **Identity is in hello NameIdentifier element of hello Subject statement**</span></span>

    <span data-ttu-id="65217-198">g.</span><span class="sxs-lookup"><span data-stu-id="65217-198">g.</span></span> <span data-ttu-id="65217-199">Vložení **jeden přihlašování adresa URL služby** do hello **adresu URL pro přihlášení zprostředkovatele Identity** pole v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="65217-199">Paste **Single Sign-On Service URL** into hello **Identity Provider Login URL** field in Salesforce.</span></span>
    
    <span data-ttu-id="65217-200">h.</span><span class="sxs-lookup"><span data-stu-id="65217-200">h.</span></span> <span data-ttu-id="65217-201">Pro **zprostředkovatele iniciované žádosti vazby služby**, vyberte **přesměrování protokolu HTTP**.</span><span class="sxs-lookup"><span data-stu-id="65217-201">For **Service Provider Initiated Request Binding**, select **HTTP Redirect**.</span></span>
    
    <span data-ttu-id="65217-202">i.</span><span class="sxs-lookup"><span data-stu-id="65217-202">i.</span></span> <span data-ttu-id="65217-203">Nakonec klikněte na **Uložit** tooapply SAML jeden přihlašování nastavení.</span><span class="sxs-lookup"><span data-stu-id="65217-203">Finally, click **Save** tooapply your SAML single sign-on settings.</span></span>

13. <span data-ttu-id="65217-204">V levém navigačním podokně hello v Salesforce, klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény**.</span><span class="sxs-lookup"><span data-stu-id="65217-204">On hello left navigation pane in Salesforce, click **Domain Management** tooexpand hello related section, and then click **My Domain**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. <span data-ttu-id="65217-206">Projděte dolů toohello **konfiguraci ověřování** části a klikněte na tlačítko hello **upravit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="65217-206">Scroll down toohello **Authentication Configuration** section, and click hello **Edit** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. <span data-ttu-id="65217-208">V hello **ověřovací služby** vyberte hello popisný název konfigurace jednotného přihlašování SAML a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="65217-208">In hello **Authentication Service** section, select hello friendly name of your SAML SSO configuration, and then click **Save**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > <span data-ttu-id="65217-210">Pokud je vybrána více než jedna služba ověřování, jsou uživatelé výzvami tooselect služby ověřování, která se jako toosign se při inicializaci tooyour přihlašování Salesforce prostředí.</span><span class="sxs-lookup"><span data-stu-id="65217-210">If more than one authentication service is selected, users are prompted tooselect which authentication service they like toosign in with while initiating single sign-on tooyour Salesforce environment.</span></span> <span data-ttu-id="65217-211">Pokud nechcete, aby bylo toohappen, pak byste měli **nechte nezaškrtnuté všechny ostatní služby ověřování**.</span><span class="sxs-lookup"><span data-stu-id="65217-211">If you don’t want it toohappen, then you should **leave all other authentication services unchecked**.</span></span>
<CE>    
> [!TIP]
> <span data-ttu-id="65217-212">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="65217-212">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="65217-213">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="65217-213">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="65217-214">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="65217-214">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="65217-215">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="65217-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="65217-216">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="65217-216">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="65217-218">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="65217-218">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="65217-219">V levém navigačním podokně hello v hello **portál Azure**, klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="65217-219">On hello left navigation pane in hello **Azure portal**, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="65217-221">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="65217-221">toodisplay hello list of users, Go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="65217-223">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="65217-223">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="65217-225">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="65217-225">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="65217-227">a.</span><span class="sxs-lookup"><span data-stu-id="65217-227">a.</span></span> <span data-ttu-id="65217-228">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="65217-228">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="65217-229">b.</span><span class="sxs-lookup"><span data-stu-id="65217-229">b.</span></span> <span data-ttu-id="65217-230">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="65217-230">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="65217-231">c.</span><span class="sxs-lookup"><span data-stu-id="65217-231">c.</span></span> <span data-ttu-id="65217-232">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="65217-232">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="65217-233">d.</span><span class="sxs-lookup"><span data-stu-id="65217-233">d.</span></span> <span data-ttu-id="65217-234">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="65217-234">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-test-user"></a><span data-ttu-id="65217-235">Vytvoření zkušebního uživatele Salesforce</span><span class="sxs-lookup"><span data-stu-id="65217-235">Creating a Salesforce test user</span></span>

<span data-ttu-id="65217-236">V této části se uživatel volá Britta Simon vytvoří v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="65217-236">In this section, a user called Britta Simon is created in Salesforce.</span></span> <span data-ttu-id="65217-237">Salesforce podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="65217-237">Salesforce supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="65217-238">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="65217-238">There is no action item for you in this section.</span></span> <span data-ttu-id="65217-239">Pokud uživatel v Salesforce ještě neexistuje, je vytvořen nový pokusíte-li tooaccess Salesforce.</span><span class="sxs-lookup"><span data-stu-id="65217-239">If a user doesn't already exist in Salesforce, a new one is created when you attempt tooaccess Salesforce.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="65217-240">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="65217-240">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="65217-241">V této části povolíte tak, že udělíte přístup tooSalesforce toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="65217-241">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSalesforce.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="65217-243">**tooassign Britta Simon tooSalesforce, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="65217-243">**tooassign Britta Simon tooSalesforce, perform hello following steps:**</span></span>

1. <span data-ttu-id="65217-244">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="65217-244">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="65217-246">V seznamu aplikace hello vyberte **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="65217-246">In hello applications list, select **Salesforce**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. <span data-ttu-id="65217-248">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="65217-248">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="65217-250">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="65217-250">Click **Add** button.</span></span> <span data-ttu-id="65217-251">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="65217-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="65217-253">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="65217-253">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="65217-254">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="65217-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="65217-255">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="65217-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="65217-256">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="65217-256">Testing single sign-on</span></span>

<span data-ttu-id="65217-257">tootest jeden přihlašování nastavení, otevřete hello přístupovému panelu na adrese [https://myapps.microsoft.com](https://myapps.microsoft.com/), pak se přihlaste do hello testovací účet a klikněte na tlačítko **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="65217-257">tootest your single sign-on settings, open hello Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), then sign into hello test account, and click **Salesforce**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="65217-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="65217-258">Additional resources</span></span>

* [<span data-ttu-id="65217-259">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="65217-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="65217-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="65217-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="65217-261">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="65217-261">Configure User Provisioning</span></span>](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

