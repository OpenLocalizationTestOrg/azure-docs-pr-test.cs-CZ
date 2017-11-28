---
title: 'Kurz: Azure Active Directory integraci s Workday | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a během pracovního dne."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: aaa41e372e45f464c4540a70fc79c98dbc06d6b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a><span data-ttu-id="9897e-103">Kurz: Azure Active Directory integraci s Workday</span><span class="sxs-lookup"><span data-stu-id="9897e-103">Tutorial: Azure Active Directory integration with Workday</span></span>

<span data-ttu-id="9897e-104">V tomto kurzu zjistíte, jak toointegrate Workday s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9897e-104">In this tutorial, you learn how toointegrate Workday with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9897e-105">Integrace Workday s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9897e-105">Integrating Workday with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9897e-106">Můžete řídit ve službě Azure AD, který má přístup tooWorkday</span><span class="sxs-lookup"><span data-stu-id="9897e-106">You can control in Azure AD who has access tooWorkday</span></span>
- <span data-ttu-id="9897e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWorkday (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9897e-107">You can enable your users tooautomatically get signed-on tooWorkday (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9897e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9897e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9897e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9897e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9897e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9897e-110">Prerequisites</span></span>

<span data-ttu-id="9897e-111">tooconfigure Azure AD integraci s Workday, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9897e-111">tooconfigure Azure AD integration with Workday, you need hello following items:</span></span>

- <span data-ttu-id="9897e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9897e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9897e-113">Workday jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9897e-113">A Workday single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9897e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9897e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9897e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9897e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9897e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9897e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9897e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9897e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9897e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9897e-118">Scenario description</span></span>
<span data-ttu-id="9897e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9897e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9897e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9897e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9897e-121">Přidání Workday z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9897e-121">Adding Workday from hello gallery</span></span>
2. <span data-ttu-id="9897e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9897e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workday-from-hello-gallery"></a><span data-ttu-id="9897e-123">Přidání Workday z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9897e-123">Adding Workday from hello gallery</span></span>
<span data-ttu-id="9897e-124">tooconfigure hello integrace Workday do Azure AD, musíte tooadd Workday hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9897e-124">tooconfigure hello integration of Workday into Azure AD, you need tooadd Workday from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9897e-125">**tooadd Workday z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9897e-125">**tooadd Workday from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9897e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9897e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9897e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9897e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9897e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9897e-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9897e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9897e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9897e-133">Hello vyhledávacího pole zadejte **Workday**.</span><span class="sxs-lookup"><span data-stu-id="9897e-133">In hello search box, type **Workday**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. <span data-ttu-id="9897e-135">Na panelu výsledků hello vyberte **Workday**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9897e-135">In hello results panel, select **Workday**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9897e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9897e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9897e-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workday podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9897e-138">In this section, you configure and test Azure AD single sign-on with Workday based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9897e-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Workday je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9897e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workday is tooa user in Azure AD.</span></span> <span data-ttu-id="9897e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello ve Workday musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9897e-140">In other words, a link relationship between an Azure AD user and hello related user in Workday needs toobe established.</span></span>

<span data-ttu-id="9897e-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** ve Workday.</span><span class="sxs-lookup"><span data-stu-id="9897e-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Workday.</span></span>

<span data-ttu-id="9897e-142">tooconfigure a testu Azure AD jednotné přihlašování s Workday, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9897e-142">tooconfigure and test Azure AD single sign-on with Workday, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9897e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9897e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9897e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9897e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9897e-145">**[Vytvoření zkušebního uživatele Workday](#creating-a-workday-test-user)**  -toohave protějšek Britta Simon ve Workday, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="9897e-145">**[Creating a Workday test user](#creating-a-workday-test-user)** - toohave a counterpart of Britta Simon in Workday that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9897e-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9897e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9897e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9897e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9897e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9897e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9897e-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci během pracovního dne.</span><span class="sxs-lookup"><span data-stu-id="9897e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workday application.</span></span>

<span data-ttu-id="9897e-150">**tooconfigure Azure AD jednotné přihlašování s Workday, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9897e-150">**tooconfigure Azure AD single sign-on with Workday, perform hello following steps:**</span></span>

1. <span data-ttu-id="9897e-151">V portálu Azure, na hello hello **Workday** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9897e-151">In hello Azure portal, on hello **Workday** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9897e-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9897e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. <span data-ttu-id="9897e-155">Na hello **Workday domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9897e-155">On hello **Workday Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    <span data-ttu-id="9897e-157">a.</span><span class="sxs-lookup"><span data-stu-id="9897e-157">a.</span></span> <span data-ttu-id="9897e-158">V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`https://impl.workday.com/<tenant>/login-saml2.htmld`</span><span class="sxs-lookup"><span data-stu-id="9897e-158">In hello **Sign-on URL** textbox, type hello value as: `https://impl.workday.com/<tenant>/login-saml2.htmld`</span></span>

    <span data-ttu-id="9897e-159">b.</span><span class="sxs-lookup"><span data-stu-id="9897e-159">b.</span></span> <span data-ttu-id="9897e-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://impl.workday.com/<tenant>/login-saml.htmld`</span><span class="sxs-lookup"><span data-stu-id="9897e-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://impl.workday.com/<tenant>/login-saml.htmld`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9897e-161">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="9897e-161">These values are not hello real.</span></span> <span data-ttu-id="9897e-162">Tyto hodnoty aktualizujte hello skutečná adresa URL přihlašování a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9897e-162">Update these values with hello actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="9897e-163">Vaše adresa URL odpovědi musí mít například subdoména: www, wd2, wd3, wd3 impl, wd5, wd5 impl).</span><span class="sxs-lookup"><span data-stu-id="9897e-163">Your reply URL must have a subdomain for example: www, wd2, wd3, wd3-impl, wd5, wd5-impl).</span></span> <span data-ttu-id="9897e-164">Pomocí něco podobného jako "*http://www.myworkday.com*" funguje, ale "*http://myworkday.com*" neexistuje.</span><span class="sxs-lookup"><span data-stu-id="9897e-164">Using something like "*http://www.myworkday.com*" works but "*http://myworkday.com*" does not.</span></span> <span data-ttu-id="9897e-165">Obraťte se na [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9897e-165">Contact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) tooget these values.</span></span> 
 

4. <span data-ttu-id="9897e-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9897e-166">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. <span data-ttu-id="9897e-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9897e-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9897e-170">Na hello **Workday konfigurace** klikněte na tlačítko **konfigurace Workday** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9897e-170">On hello **Workday Configuration** section, click **Configure Workday** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9897e-171">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9897e-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="9897e-172">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="9897e-172">![Configure Single Sign-On](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span></span>
7. <span data-ttu-id="9897e-173">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour Workday.</span><span class="sxs-lookup"><span data-stu-id="9897e-173">In a different web browser window, log in tooyour Workday company site as an administrator.</span></span>

8. <span data-ttu-id="9897e-174">Přejděte příliš**nabídky \> Workbench**.</span><span class="sxs-lookup"><span data-stu-id="9897e-174">Go too**Menu \> Workbench**.</span></span>
   
    <span data-ttu-id="9897e-175">![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")</span><span class="sxs-lookup"><span data-stu-id="9897e-175">![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")</span></span>

9. <span data-ttu-id="9897e-176">Přejděte příliš**správy účtů**.</span><span class="sxs-lookup"><span data-stu-id="9897e-176">Go too**Account Administration**.</span></span>
   
    <span data-ttu-id="9897e-177">![Účet správy](./media/active-directory-saas-workday-tutorial/IC782924.png "účet správy")</span><span class="sxs-lookup"><span data-stu-id="9897e-177">![Account Administration](./media/active-directory-saas-workday-tutorial/IC782924.png "Account Administration")</span></span>

10. <span data-ttu-id="9897e-178">Přejděte příliš**upravit nastavení klienta – zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="9897e-178">Go too**Edit Tenant Setup – Security**.</span></span>
   
    <span data-ttu-id="9897e-179">![Upravit zabezpečení klienta](./media/active-directory-saas-workday-tutorial/IC782925.png "upravit klienta zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="9897e-179">![Edit Tenant Security](./media/active-directory-saas-workday-tutorial/IC782925.png "Edit Tenant Security")</span></span>

11. <span data-ttu-id="9897e-180">V hello **adresy URL pro přesměrování** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9897e-180">In hello **Redirection URLs** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="9897e-181">![Adresy URL pro přesměrování](./media/active-directory-saas-workday-tutorial/IC7829581.png "adresy URL pro přesměrování")</span><span class="sxs-lookup"><span data-stu-id="9897e-181">![Redirection URLs](./media/active-directory-saas-workday-tutorial/IC7829581.png "Redirection URLs")</span></span>
   
    <span data-ttu-id="9897e-182">a.</span><span class="sxs-lookup"><span data-stu-id="9897e-182">a.</span></span> <span data-ttu-id="9897e-183">Klikněte na tlačítko **přidejte řádek**.</span><span class="sxs-lookup"><span data-stu-id="9897e-183">Click **Add Row**.</span></span>
   
    <span data-ttu-id="9897e-184">b.</span><span class="sxs-lookup"><span data-stu-id="9897e-184">b.</span></span> <span data-ttu-id="9897e-185">V hello **adresy URL přesměrování při přihlášení** textové pole a hello **adresu URL pro přesměrování mobilní** textovému poli, typ hello **přihlašovací adresa URL** jste zadali na hello **Workday domény a Adresy URL** části hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9897e-185">In hello **Login Redirect URL** textbox and hello **Mobile Redirect URL** textbox, type hello **Sign-on URL** you have entered on hello **Workday Domain and URLs** section of hello Azure portal.</span></span>
   
    <span data-ttu-id="9897e-186">c.</span><span class="sxs-lookup"><span data-stu-id="9897e-186">c.</span></span> <span data-ttu-id="9897e-187">V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **Sign-Out URL**a pak ji vložit do hello **adresy URL přesměrování odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9897e-187">In hello Azure portal, on hello **Configure sign-on** window, copy hello **Sign-Out URL**, and then paste it into hello **Logout Redirect URL** textbox.</span></span>
   
    <span data-ttu-id="9897e-188">d.</span><span class="sxs-lookup"><span data-stu-id="9897e-188">d.</span></span>  <span data-ttu-id="9897e-189">V **prostředí** textovému poli, název typu hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="9897e-189">In **Environment** textbox, type hello environment name.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="9897e-190">Hello hodnotu atributu hello prostředí je vázaný toohello hodnotu adresy URL hello klienta:</span><span class="sxs-lookup"><span data-stu-id="9897e-190">hello value of hello Environment attribute is tied toohello value of hello tenant URL:</span></span>  
    ><span data-ttu-id="9897e-191">-Pokud název domény hello URL klienta Workday hello začíná impl například: *https://impl.workday.com/\<klienta\>/login-saml2.htmld*), hello **prostředí** musí být nastaven tooImplementation.</span><span class="sxs-lookup"><span data-stu-id="9897e-191">-If hello domain name of hello Workday tenant URL starts with impl for example: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), hello **Environment** attribute must be set tooImplementation.</span></span>  
    ><span data-ttu-id="9897e-192">– Pokud je název domény hello začíná něco jiného, je nutné toocontact [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) tooget hello odpovídající **prostředí** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9897e-192">-If hello domain name starts with something else, you need toocontact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) tooget hello matching **Environment** value.</span></span>

12. <span data-ttu-id="9897e-193">V hello **SAML instalace** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9897e-193">In hello **SAML Setup** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="9897e-194">![Instalační program SAML](./media/active-directory-saas-workday-tutorial/IC782926.png "instalace SAML")</span><span class="sxs-lookup"><span data-stu-id="9897e-194">![SAML Setup](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")</span></span>
   
    <span data-ttu-id="9897e-195">a.</span><span class="sxs-lookup"><span data-stu-id="9897e-195">a.</span></span>  <span data-ttu-id="9897e-196">Vyberte **povolit ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="9897e-196">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="9897e-197">b.</span><span class="sxs-lookup"><span data-stu-id="9897e-197">b.</span></span>  <span data-ttu-id="9897e-198">Klikněte na tlačítko **přidejte řádek**.</span><span class="sxs-lookup"><span data-stu-id="9897e-198">Click **Add Row**.</span></span>

13. <span data-ttu-id="9897e-199">V části zprostředkovatelů Identity SAML hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9897e-199">In hello SAML Identity Providers section, perform hello following steps:</span></span>
   
    <span data-ttu-id="9897e-200">![Zprostředkovatelé Identity SAML](./media/active-directory-saas-workday-tutorial/IC7829271.png "zprostředkovatelů Identity SAML")</span><span class="sxs-lookup"><span data-stu-id="9897e-200">![SAML Identity Providers](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identity Providers")</span></span>
   
    <span data-ttu-id="9897e-201">a.</span><span class="sxs-lookup"><span data-stu-id="9897e-201">a.</span></span> <span data-ttu-id="9897e-202">Do textového pole Název zprostředkovatele Identity hello, zadejte název zprostředkovatele (například: *SPInitiatedSSO*).</span><span class="sxs-lookup"><span data-stu-id="9897e-202">In hello Identity Provider Name textbox, type a provider name (for example: *SPInitiatedSSO*).</span></span>
   
    <span data-ttu-id="9897e-203">b.</span><span class="sxs-lookup"><span data-stu-id="9897e-203">b.</span></span> <span data-ttu-id="9897e-204">V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **SAML Entity ID** hodnotu a pak ji vložit do hello **vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9897e-204">In hello Azure portal, on hello **Configure sign-on** window, copy hello **SAML Entity ID** value, and then paste it into hello **Issuer** textbox.</span></span>
   
    <span data-ttu-id="9897e-205">c.</span><span class="sxs-lookup"><span data-stu-id="9897e-205">c.</span></span> <span data-ttu-id="9897e-206">Vyberte **povolit Workday Initiated odhlášení**.</span><span class="sxs-lookup"><span data-stu-id="9897e-206">Select **Enable Workday Initiated Logout**.</span></span>
   
    <span data-ttu-id="9897e-207">d.</span><span class="sxs-lookup"><span data-stu-id="9897e-207">d.</span></span> <span data-ttu-id="9897e-208">V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **Sign-Out URL** hodnotu a pak ji vložit do hello **adresy URL žádosti o odhlášení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9897e-208">In hello Azure portal, on hello **Configure sign-on** window, copy hello **Sign-Out URL** value, and then paste it into hello **Logout Request URL** textbox.</span></span>

    <span data-ttu-id="9897e-209">e.</span><span class="sxs-lookup"><span data-stu-id="9897e-209">e.</span></span> <span data-ttu-id="9897e-210">Klikněte na tlačítko **certifikátu veřejného klíče zprostředkovatele Identity**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9897e-210">Click **Identity Provider Public Key Certificate**, and then click **Create**.</span></span> 

    <span data-ttu-id="9897e-211">![Vytvoření](./media/active-directory-saas-workday-tutorial/IC782928.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="9897e-211">![Create](./media/active-directory-saas-workday-tutorial/IC782928.png "Create")</span></span>

    <span data-ttu-id="9897e-212">f.</span><span class="sxs-lookup"><span data-stu-id="9897e-212">f.</span></span> <span data-ttu-id="9897e-213">Klikněte na tlačítko **vytvořit x509 veřejný klíč**.</span><span class="sxs-lookup"><span data-stu-id="9897e-213">Click **Create x509 Public Key**.</span></span> 

    <span data-ttu-id="9897e-214">![Vytvoření](./media/active-directory-saas-workday-tutorial/IC782929.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="9897e-214">![Create](./media/active-directory-saas-workday-tutorial/IC782929.png "Create")</span></span>


14. <span data-ttu-id="9897e-215">V hello **veřejný klíč zobrazení x509** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9897e-215">In hello **View x509 Public Key** section, perform hello following steps:</span></span> 
   
    <span data-ttu-id="9897e-216">![Veřejný klíč zobrazení x509](./media/active-directory-saas-workday-tutorial/IC782930.png "zobrazení x509 veřejný klíč")</span><span class="sxs-lookup"><span data-stu-id="9897e-216">![View x509 Public Key](./media/active-directory-saas-workday-tutorial/IC782930.png "View x509 Public Key")</span></span> 
   
    <span data-ttu-id="9897e-217">a.</span><span class="sxs-lookup"><span data-stu-id="9897e-217">a.</span></span> <span data-ttu-id="9897e-218">V hello **název** textovému poli, zadejte název pro svůj certifikát (například: *PPE\_SP*).</span><span class="sxs-lookup"><span data-stu-id="9897e-218">In hello **Name** textbox, type a name for your certificate (for example: *PPE\_SP*).</span></span>
   
    <span data-ttu-id="9897e-219">b.</span><span class="sxs-lookup"><span data-stu-id="9897e-219">b.</span></span> <span data-ttu-id="9897e-220">V hello **platný z** textovému poli, typ hello platnost od hodnota atributu vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="9897e-220">In hello **Valid From** textbox, type hello valid from attribute value of your certificate.</span></span>
   
    <span data-ttu-id="9897e-221">c.</span><span class="sxs-lookup"><span data-stu-id="9897e-221">c.</span></span>  <span data-ttu-id="9897e-222">V hello **platný k** textovému poli, hodnota platná tooattribute hello typu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="9897e-222">In hello **Valid To** textbox, type hello valid tooattribute value of your certificate.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="9897e-223">Můžete získat platný hello od data a hello platný toodate z hello stáhnout certifikát poklepáním.</span><span class="sxs-lookup"><span data-stu-id="9897e-223">You can get hello valid from date and hello valid toodate from hello downloaded certificate by double-clicking it.</span></span>  <span data-ttu-id="9897e-224">Hello data jsou uvedeny v části hello **podrobnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="9897e-224">hello dates are listed under hello **Details** tab.</span></span>
    > 
    >
   
    <span data-ttu-id="9897e-225">d.</span><span class="sxs-lookup"><span data-stu-id="9897e-225">d.</span></span>  <span data-ttu-id="9897e-226">V programu Poznámkový blok a potom hello kopírování obsahu je otevření kódovaného certifikátu kódování base-64.</span><span class="sxs-lookup"><span data-stu-id="9897e-226">Open your base-64 encoded certificate in notepad, and then copy hello content of it.</span></span>
   
    <span data-ttu-id="9897e-227">e.</span><span class="sxs-lookup"><span data-stu-id="9897e-227">e.</span></span>  <span data-ttu-id="9897e-228">V hello **certifikát** textovému poli, hello vložit obsah schránky.</span><span class="sxs-lookup"><span data-stu-id="9897e-228">In hello **Certificate** textbox, paste hello content of your clipboard.</span></span>
   
    <span data-ttu-id="9897e-229">f.</span><span class="sxs-lookup"><span data-stu-id="9897e-229">f.</span></span>  <span data-ttu-id="9897e-230">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9897e-230">Click **OK**.</span></span>

15. <span data-ttu-id="9897e-231">Proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9897e-231">Perform hello following steps:</span></span> 
   
    <span data-ttu-id="9897e-232">![Konfigurace jednotného přihlašování k](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "Konfigurace jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9897e-232">![SSO configuration](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO configuration")</span></span>
   
    <span data-ttu-id="9897e-233">a.</span><span class="sxs-lookup"><span data-stu-id="9897e-233">a.</span></span>  <span data-ttu-id="9897e-234">Povolit hello **x509 pár privátního klíče**.</span><span class="sxs-lookup"><span data-stu-id="9897e-234">Enable hello **x509 Private Key Pair**.</span></span>
   
    <span data-ttu-id="9897e-235">b.</span><span class="sxs-lookup"><span data-stu-id="9897e-235">b.</span></span>  <span data-ttu-id="9897e-236">V hello **ID zprostředkovatele služby** textovému poli, typ **http://www.workday.com**.</span><span class="sxs-lookup"><span data-stu-id="9897e-236">In hello **Service Provider ID** textbox, type **http://www.workday.com**.</span></span>
   
    <span data-ttu-id="9897e-237">c.</span><span class="sxs-lookup"><span data-stu-id="9897e-237">c.</span></span>  <span data-ttu-id="9897e-238">Vyberte **povolit SP spustil ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="9897e-238">Select **Enable SP Initiated SAML Authentication**.</span></span>
   
    <span data-ttu-id="9897e-239">d.</span><span class="sxs-lookup"><span data-stu-id="9897e-239">d.</span></span>  <span data-ttu-id="9897e-240">V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **SAML jeden přihlašování adresa URL služby** hodnotu a pak ji vložit do hello **IdP adresa URL služby jednotného přihlašování k** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9897e-240">In hello Azure portal, on hello **Configure sign-on** window, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **IdP SSO Service URL** textbox.</span></span>
   
    <span data-ttu-id="9897e-241">e.</span><span class="sxs-lookup"><span data-stu-id="9897e-241">e.</span></span> <span data-ttu-id="9897e-242">Vyberte **není Deflate žádosti o ověření spouštěná SP**.</span><span class="sxs-lookup"><span data-stu-id="9897e-242">Select **Do Not Deflate SP-initiated Authentication Request**.</span></span>
   
    <span data-ttu-id="9897e-243">f.</span><span class="sxs-lookup"><span data-stu-id="9897e-243">f.</span></span> <span data-ttu-id="9897e-244">Jako **metoda žádosti o podpis ověřování**, vyberte **SHA256**.</span><span class="sxs-lookup"><span data-stu-id="9897e-244">As **Authentication Request Signature Method**, select **SHA256**.</span></span> 
   
    <span data-ttu-id="9897e-245">![Metoda ověřování žádost o podpis](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "metodu ověřování žádost o podpis")</span><span class="sxs-lookup"><span data-stu-id="9897e-245">![Authentication Request Signature Method](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "Authentication Request Signature Method")</span></span> 
   
    <span data-ttu-id="9897e-246">g.</span><span class="sxs-lookup"><span data-stu-id="9897e-246">g.</span></span> <span data-ttu-id="9897e-247">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9897e-247">Click **OK**.</span></span> 
   
    <span data-ttu-id="9897e-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span><span class="sxs-lookup"><span data-stu-id="9897e-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span></span>

> [!TIP]
> <span data-ttu-id="9897e-249">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="9897e-249">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9897e-250">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="9897e-250">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9897e-251">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9897e-251">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9897e-252">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9897e-252">Creating an Azure AD test user</span></span>
<span data-ttu-id="9897e-253">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="9897e-253">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9897e-255">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9897e-255">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9897e-256">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9897e-256">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9897e-258">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9897e-258">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9897e-260">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="9897e-260">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9897e-262">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9897e-262">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9897e-264">a.</span><span class="sxs-lookup"><span data-stu-id="9897e-264">a.</span></span> <span data-ttu-id="9897e-265">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9897e-265">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9897e-266">b.</span><span class="sxs-lookup"><span data-stu-id="9897e-266">b.</span></span> <span data-ttu-id="9897e-267">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9897e-267">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9897e-268">c.</span><span class="sxs-lookup"><span data-stu-id="9897e-268">c.</span></span> <span data-ttu-id="9897e-269">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9897e-269">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9897e-270">d.</span><span class="sxs-lookup"><span data-stu-id="9897e-270">d.</span></span> <span data-ttu-id="9897e-271">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9897e-271">Click **Create**.</span></span>
 
### <a name="creating-a-workday-test-user"></a><span data-ttu-id="9897e-272">Vytvoření zkušebního uživatele Workday</span><span class="sxs-lookup"><span data-stu-id="9897e-272">Creating a Workday test user</span></span>

<span data-ttu-id="9897e-273">tooget testovacího uživatele zřízené do Workday, budete potřebovat toocontact hello [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html).</span><span class="sxs-lookup"><span data-stu-id="9897e-273">tooget a test user provisioned into Workday, you need toocontact hello [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html).</span></span>
<span data-ttu-id="9897e-274">Hello [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) pro vás vytvoří hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="9897e-274">hello [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) creates hello user for you.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9897e-275">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9897e-275">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9897e-276">V této části povolíte tak, že udělíte přístup tooWorkday toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9897e-276">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkday.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9897e-278">**tooassign Britta Simon tooWorkday, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9897e-278">**tooassign Britta Simon tooWorkday, perform hello following steps:**</span></span>

1. <span data-ttu-id="9897e-279">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9897e-279">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9897e-281">V seznamu aplikace hello vyberte **Workday**.</span><span class="sxs-lookup"><span data-stu-id="9897e-281">In hello applications list, select **Workday**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. <span data-ttu-id="9897e-283">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9897e-283">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9897e-285">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9897e-285">Click **Add** button.</span></span> <span data-ttu-id="9897e-286">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9897e-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9897e-288">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9897e-288">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9897e-289">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9897e-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9897e-290">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9897e-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9897e-291">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9897e-291">Testing single sign-on</span></span>

<span data-ttu-id="9897e-292">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9897e-292">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="9897e-293">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9897e-293">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9897e-294">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9897e-294">Additional resources</span></span>

* [<span data-ttu-id="9897e-295">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9897e-295">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9897e-296">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9897e-296">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="9897e-297">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="9897e-297">Configure User Provisioning</span></span>](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

