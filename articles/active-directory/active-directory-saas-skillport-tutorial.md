---
title: 'Kurz: Azure Active Directory integrace s Skillport | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Skillport."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4df349b2-a73f-4b88-a077-ec0fbfc26527
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: ba504c3cae5f92767eb90d8453887904690fe0c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skillport"></a><span data-ttu-id="2e99d-103">Kurz: Azure Active Directory integrace s Skillport</span><span class="sxs-lookup"><span data-stu-id="2e99d-103">Tutorial: Azure Active Directory integration with Skillport</span></span>

<span data-ttu-id="2e99d-104">V tomto kurzu zjistíte, jak toointegrate Skillport s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2e99d-104">In this tutorial, you learn how toointegrate Skillport with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2e99d-105">Integrace Skillport s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2e99d-105">Integrating Skillport with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2e99d-106">Můžete řídit ve službě Azure AD, který má přístup tooSkillport</span><span class="sxs-lookup"><span data-stu-id="2e99d-106">You can control in Azure AD who has access tooSkillport</span></span>
- <span data-ttu-id="2e99d-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSkillport (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2e99d-107">You can enable your users tooautomatically get signed-on tooSkillport (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2e99d-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2e99d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2e99d-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2e99d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e99d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2e99d-110">Prerequisites</span></span>

<span data-ttu-id="2e99d-111">Integrace služby Azure AD s Skillport tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2e99d-111">tooconfigure Azure AD integration with Skillport, you need hello following items:</span></span>

- <span data-ttu-id="2e99d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2e99d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2e99d-113">Skillport jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2e99d-113">A Skillport single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2e99d-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2e99d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2e99d-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2e99d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2e99d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2e99d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2e99d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2e99d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2e99d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2e99d-118">Scenario description</span></span>
<span data-ttu-id="2e99d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2e99d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2e99d-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2e99d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2e99d-121">Přidání Skillport z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2e99d-121">Adding Skillport from hello gallery</span></span>
2. <span data-ttu-id="2e99d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2e99d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skillport-from-hello-gallery"></a><span data-ttu-id="2e99d-123">Přidání Skillport z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2e99d-123">Adding Skillport from hello gallery</span></span>
<span data-ttu-id="2e99d-124">tooconfigure hello integrace Skillport do Azure AD, je nutné tooadd Skillport hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2e99d-124">tooconfigure hello integration of Skillport into Azure AD, you need tooadd Skillport from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2e99d-125">**tooadd Skillport z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2e99d-125">**tooadd Skillport from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2e99d-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2e99d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2e99d-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2e99d-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2e99d-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2e99d-131">Click **New Application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2e99d-133">Hello vyhledávacího pole zadejte **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-133">In hello search box, type **Skillport**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_search.png)

5. <span data-ttu-id="2e99d-135">Na panelu výsledků hello vyberte **Skillport**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e99d-135">In hello results panel, select **Skillport**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2e99d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2e99d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2e99d-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Skillport podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2e99d-138">In this section, you configure and test Azure AD single sign-on with Skillport based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2e99d-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Skillport je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2e99d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Skillport is tooa user in Azure AD.</span></span> <span data-ttu-id="2e99d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Skillport musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2e99d-140">In other words, a link relationship between an Azure AD user and hello related user in Skillport needs toobe established.</span></span>

<span data-ttu-id="2e99d-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Skillport.</span><span class="sxs-lookup"><span data-stu-id="2e99d-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Skillport.</span></span>

<span data-ttu-id="2e99d-142">tooconfigure a testu Azure AD jednotné přihlašování s Skillport, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2e99d-142">tooconfigure and test Azure AD single sign-on with Skillport, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2e99d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2e99d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2e99d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2e99d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2e99d-145">**[Vytvoření zkušebního uživatele Skillport](#creating-a-skillport-test-user)**  -toohave protějšek Britta Simon v Skillport, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e99d-145">**[Creating a Skillport test user](#creating-a-skillport-test-user)** - toohave a counterpart of Britta Simon in Skillport that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2e99d-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2e99d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2e99d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2e99d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2e99d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2e99d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2e99d-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Skillport.</span><span class="sxs-lookup"><span data-stu-id="2e99d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Skillport application.</span></span>

<span data-ttu-id="2e99d-150">**tooconfigure Azure AD jednotné přihlašování s Skillport, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2e99d-150">**tooconfigure Azure AD single sign-on with Skillport, perform hello following steps:**</span></span>

1. <span data-ttu-id="2e99d-151">V portálu Azure, na hello hello **Skillport** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-151">In hello Azure  portal, on hello **Skillport** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2e99d-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2e99d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_samlbase.png)

3. <span data-ttu-id="2e99d-155">Na hello **Skillport domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2e99d-155">On hello **Skillport Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_url.png)

    <span data-ttu-id="2e99d-157">a.</span><span class="sxs-lookup"><span data-stu-id="2e99d-157">a.</span></span> <span data-ttu-id="2e99d-158">V hello **přihlašovací adresa URL** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="2e99d-158">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span>
      
      <span data-ttu-id="2e99d-159">Evropa Datacenter:`https://<subdomain>.skillport.eu`</span><span class="sxs-lookup"><span data-stu-id="2e99d-159">EU Datacenter: `https://<subdomain>.skillport.eu`</span></span>
   
      <span data-ttu-id="2e99d-160">USA – Datacenter:`https://<subdomain>.skillport.com`</span><span class="sxs-lookup"><span data-stu-id="2e99d-160">US Datacenter: `https://<subdomain>.skillport.com`</span></span>
   
    <span data-ttu-id="2e99d-161">b.</span><span class="sxs-lookup"><span data-stu-id="2e99d-161">b.</span></span> <span data-ttu-id="2e99d-162">V hello **adresa URL odpovědi** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="2e99d-162">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span>
    
      <span data-ttu-id="2e99d-163">Evropa Datacenter:`https://<subdomain>.skillport.eu/adfs/ls/`</span><span class="sxs-lookup"><span data-stu-id="2e99d-163">EU Datacenter: `https://<subdomain>.skillport.eu/adfs/ls/`</span></span>
    
      <span data-ttu-id="2e99d-164">USA – Datacenter:`https://<subdomain>.skillport.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="2e99d-164">US Datacenter: `https://<subdomain>.skillport.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2e99d-165">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="2e99d-165">These values are not hello real.</span></span> <span data-ttu-id="2e99d-166">Tyto hodnoty aktualizujte hello skutečná adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="2e99d-166">Update these values with hello actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="2e99d-167">Obraťte se na [tým podpory Skillport klienta](https://www.skillsoft.com/contact.asp) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2e99d-167">Contact [Skillport Client support team](https://www.skillsoft.com/contact.asp) tooget these values.</span></span>
 
4. <span data-ttu-id="2e99d-168">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2e99d-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_certificate.png) 

5. <span data-ttu-id="2e99d-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2e99d-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2e99d-172">tooconfigure jednotného přihlašování na **Skillport** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Skillport](https://www.skillsoft.com/contact.asp).</span><span class="sxs-lookup"><span data-stu-id="2e99d-172">tooconfigure single sign-on on **Skillport** side, you need toosend hello downloaded **Metadata XML** too[Skillport support team](https://www.skillsoft.com/contact.asp).</span></span> <span data-ttu-id="2e99d-173">Budou se ho nastavit toohave hello jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="2e99d-173">They will set it up toohave hello SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2e99d-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2e99d-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="2e99d-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2e99d-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2e99d-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2e99d-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2e99d-178">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2e99d-178">In hello **Azure  portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2e99d-180">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2e99d-180">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2e99d-182">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2e99d-182">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2e99d-184">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2e99d-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2e99d-186">a.</span><span class="sxs-lookup"><span data-stu-id="2e99d-186">a.</span></span> <span data-ttu-id="2e99d-187">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2e99d-188">b.</span><span class="sxs-lookup"><span data-stu-id="2e99d-188">b.</span></span> <span data-ttu-id="2e99d-189">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2e99d-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2e99d-190">c.</span><span class="sxs-lookup"><span data-stu-id="2e99d-190">c.</span></span> <span data-ttu-id="2e99d-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2e99d-192">d.</span><span class="sxs-lookup"><span data-stu-id="2e99d-192">d.</span></span> <span data-ttu-id="2e99d-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-193">Click **Create**.</span></span>
 
### <a name="creating-a-skillport-test-user"></a><span data-ttu-id="2e99d-194">Vytvoření zkušebního uživatele Skillport</span><span class="sxs-lookup"><span data-stu-id="2e99d-194">Creating a Skillport test user</span></span>

<span data-ttu-id="2e99d-195">Pořadí toocreate Skillport testovacího uživatele, je nutné toocontact [tým podpory Skillport](https://www.skillsoft.com/contact.asp) mají více obchodní scénáře podle toohello požadavek koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e99d-195">In order toocreate Skillport test user, you need toocontact [Skillport support team](https://www.skillsoft.com/contact.asp) as they have multiple business scenarios according toohello requirement of end user.</span></span> <span data-ttu-id="2e99d-196">Nakonfiguruje ho po diskuse s uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="2e99d-196">They will configure it after discussion with hello users.</span></span>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2e99d-197">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2e99d-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2e99d-198">V této části povolíte tak, že udělíte přístup tooSkillport toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2e99d-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSkillport.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2e99d-200">**tooassign Britta Simon tooSkillport, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2e99d-200">**tooassign Britta Simon tooSkillport, perform hello following steps:**</span></span>

1. <span data-ttu-id="2e99d-201">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2e99d-203">V seznamu aplikace hello vyberte **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-203">In hello applications list, select **Skillport**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_app.png) 

3. <span data-ttu-id="2e99d-205">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2e99d-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2e99d-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2e99d-207">Click **Add** button.</span></span> <span data-ttu-id="2e99d-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2e99d-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2e99d-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2e99d-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2e99d-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2e99d-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2e99d-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2e99d-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2e99d-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2e99d-213">Testing single sign-on</span></span>

<span data-ttu-id="2e99d-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2e99d-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2e99d-215">Když kliknete na dlaždici Skillport hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Skillport aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e99d-215">When you click hello Skillport tile in hello Access Panel, you should get automatically signed-on tooyour Skillport application.</span></span>
<span data-ttu-id="2e99d-216">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="2e99d-216">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2e99d-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2e99d-217">Additional resources</span></span>

* [<span data-ttu-id="2e99d-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2e99d-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2e99d-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2e99d-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_203.png

