---
title: 'Kurz: Azure Active Directory integrace s LinkedIn Learning | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LinkedIn Learning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: jeedes
ms.openlocfilehash: 14610a25132ed0ccf5892cad6ccc4e1ef03ff280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a><span data-ttu-id="2b966-103">Kurz: Azure Active Directory integrace s LinkedIn Learning</span><span class="sxs-lookup"><span data-stu-id="2b966-103">Tutorial: Azure Active Directory integration with LinkedIn Learning</span></span>

<span data-ttu-id="2b966-104">V tomto kurzu zjistíte, jak toointegrate LinkedIn Learning s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2b966-104">In this tutorial, you learn how toointegrate LinkedIn Learning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2b966-105">Integrace LinkedIn Learning s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2b966-105">Integrating LinkedIn Learning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2b966-106">Můžete řídit ve službě Azure AD, který má přístup tooLinkedIn učení</span><span class="sxs-lookup"><span data-stu-id="2b966-106">You can control in Azure AD who has access tooLinkedIn Learning</span></span>
- <span data-ttu-id="2b966-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLinkedIn Learning (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2b966-107">You can enable your users tooautomatically get signed-on tooLinkedIn Learning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2b966-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2b966-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2b966-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2b966-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b966-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2b966-110">Prerequisites</span></span>

<span data-ttu-id="2b966-111">tooconfigure integrace Azure AD s LinkedIn Learning, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2b966-111">tooconfigure Azure AD integration with LinkedIn Learning, you need hello following items:</span></span>

- <span data-ttu-id="2b966-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2b966-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2b966-113">LinkedIn Learning jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2b966-113">A LinkedIn Learning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2b966-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2b966-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2b966-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2b966-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2b966-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2b966-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2b966-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2b966-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2b966-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2b966-118">Scenario description</span></span>
<span data-ttu-id="2b966-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2b966-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2b966-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2b966-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2b966-121">Přidání LinkedIn Learning z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2b966-121">Adding LinkedIn Learning from hello gallery</span></span>
2. <span data-ttu-id="2b966-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2b966-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-learning-from-hello-gallery"></a><span data-ttu-id="2b966-123">Přidání LinkedIn Learning z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2b966-123">Adding LinkedIn Learning from hello gallery</span></span>
<span data-ttu-id="2b966-124">tooconfigure hello integrace LinkedIn Learning do Azure AD, je nutné tooadd LinkedIn Learning hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2b966-124">tooconfigure hello integration of LinkedIn Learning into Azure AD, you need tooadd LinkedIn Learning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2b966-125">**tooadd LinkedIn Learning z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2b966-125">**tooadd LinkedIn Learning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b966-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2b966-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2b966-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2b966-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2b966-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2b966-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2b966-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2b966-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2b966-133">Hello vyhledávacího pole zadejte **LinkedIn Learning**.</span><span class="sxs-lookup"><span data-stu-id="2b966-133">In hello search box, type **LinkedIn Learning**.</span></span> <span data-ttu-id="2b966-134">Na panelu výsledků klikněte na **LinkedIn Learning** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b966-134">From results panel, click **LinkedIn Learning** tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2b966-136">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2b966-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2b966-137">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LinkedIn Learning podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2b966-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Learning based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2b966-138">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v LinkedIn Learning je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2b966-138">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Learning is tooa user in Azure AD.</span></span> <span data-ttu-id="2b966-139">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LinkedIn Learning musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2b966-139">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Learning needs toobe established.</span></span>

<span data-ttu-id="2b966-140">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v LinkedIn Learning.</span><span class="sxs-lookup"><span data-stu-id="2b966-140">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Learning.</span></span>

<span data-ttu-id="2b966-141">tooconfigure a testu Azure AD jednotné přihlašování s LinkedIn Learning, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2b966-141">tooconfigure and test Azure AD single sign-on with LinkedIn Learning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2b966-142">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2b966-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2b966-143">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2b966-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2b966-144">**[Vytvoření zkušebního uživatele LinkedIn Learning](#creating-a-linkedin-learning-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2b966-144">**[Creating a LinkedIn Learning test user](#creating-a-linkedin-learning-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="2b966-145">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2b966-145">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2b966-146">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2b966-146">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2b966-147">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2b966-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2b966-148">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LinkedIn Learning.</span><span class="sxs-lookup"><span data-stu-id="2b966-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LinkedIn Learning application.</span></span>

<span data-ttu-id="2b966-149">**tooconfigure Azure AD jednotné přihlašování s LinkedIn Learning, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2b966-149">**tooconfigure Azure AD single sign-on with LinkedIn Learning, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b966-150">V portálu Azure, na hello hello **LinkedIn Learning** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2b966-150">In hello Azure portal, on hello **LinkedIn Learning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2b966-152">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2b966-152">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="2b966-154">V okně prohlížeče jiný web, klienta LinkedIn Learning tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="2b966-154">In a different web browser window, sign-on tooyour LinkedIn Learning tenant as an administrator.</span></span>

4. <span data-ttu-id="2b966-155">V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="2b966-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="2b966-156">Kromě toho **učení – výchozí** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="2b966-156">Also, select **Learning - Default** from hello dropdown list.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="2b966-158">Klikněte na tlačítko **nebo klepněte sem tooload a zkopírujte jednotlivých polí z formuláře hello** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**</span><span class="sxs-lookup"><span data-stu-id="2b966-158">Click **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="2b966-160">Na portálu Azure v části **LinkedIn Learning domény a adresy URL**, proveďte následující kroky, pokud chcete, aby tooconfigure jednotné přihlašování hello v **IdP iniciované** režimu</span><span class="sxs-lookup"><span data-stu-id="2b966-160">On Azure portal, under **LinkedIn Learning Domain and URLs**, perform hello following steps if you want tooconfigure SSO in **IdP Initiated** mode</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="2b966-162">a.</span><span class="sxs-lookup"><span data-stu-id="2b966-162">a.</span></span> <span data-ttu-id="2b966-163">V hello **identifikátor** textovému poli, zadejte hello **Entity ID** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="2b966-163">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="2b966-164">b.</span><span class="sxs-lookup"><span data-stu-id="2b966-164">b.</span></span> <span data-ttu-id="2b966-165">V hello **adresa URL odpovědi** textovému poli, zadejte hello **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="2b966-165">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="2b966-166">Pokud chcete, aby tooconfigure jednotné přihlašování v **iniciované SP**, klikněte na možnost zobrazit Advanced adresy URL v části Konfigurace hello a nakonfigurovat následující vzor hello hello přihlašovací adresa URL:</span><span class="sxs-lookup"><span data-stu-id="2b966-166">If you want tooconfigure SSO in **SP Initiated**, then click Show Advanced URL setting option in hello configuration section and configure hello sign-on URL with hello following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. <span data-ttu-id="2b966-168">Aplikace LinkedIn Learning očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="2b966-168">Your LinkedIn Learning application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="2b966-169">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="2b966-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="2b966-170">Výchozí hodnota Hello **uživatelský identifikátor** je **user.userprincipalname** ale LinkedIn Learning očekává tento toobe namapována na hello uživatele e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="2b966-170">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Learning expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="2b966-171">K tomu můžete použít **user.mail** atribut ze seznamu hello nebo použijte hodnotu hello odpovídajícího atributu na základě konfigurace vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="2b966-171">For that you can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. <span data-ttu-id="2b966-173">V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavte atributy hello.</span><span class="sxs-lookup"><span data-stu-id="2b966-173">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="2b966-174">Hello uživatel potřebuje tooadd čtyři deklarace identity s názvem **e-mailu**, **oddělení**, **firstname**, a **lastname** a hodnota hello je namapována na toobe **user.mail**, **user.department**, **user.givenname**, a **user.surname** v uvedeném pořadí</span><span class="sxs-lookup"><span data-stu-id="2b966-174">hello user needs tooadd four claims named **email**, **department**, **firstname**, and **lastname** and hello value is toobe mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="2b966-175">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2b966-175">Attribute Name</span></span> | <span data-ttu-id="2b966-176">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="2b966-176">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="2b966-177">E-mailu</span><span class="sxs-lookup"><span data-stu-id="2b966-177">email</span></span>| <span data-ttu-id="2b966-178">User.Mail</span><span class="sxs-lookup"><span data-stu-id="2b966-178">user.mail</span></span> |    
    | <span data-ttu-id="2b966-179">Oddělení</span><span class="sxs-lookup"><span data-stu-id="2b966-179">department</span></span>| <span data-ttu-id="2b966-180">User.Department</span><span class="sxs-lookup"><span data-stu-id="2b966-180">user.department</span></span> |
    | <span data-ttu-id="2b966-181">FirstName</span><span class="sxs-lookup"><span data-stu-id="2b966-181">firstname</span></span>| <span data-ttu-id="2b966-182">User.givenName</span><span class="sxs-lookup"><span data-stu-id="2b966-182">user.givenname</span></span> |
    | <span data-ttu-id="2b966-183">Příjmení</span><span class="sxs-lookup"><span data-stu-id="2b966-183">lastname</span></span>| <span data-ttu-id="2b966-184">User.Surname</span><span class="sxs-lookup"><span data-stu-id="2b966-184">user.surname</span></span> |
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    <span data-ttu-id="2b966-186">a.</span><span class="sxs-lookup"><span data-stu-id="2b966-186">a.</span></span> <span data-ttu-id="2b966-187">Klikněte na tlačítko **přidat atribut** tooopen hello atribut dialogu.</span><span class="sxs-lookup"><span data-stu-id="2b966-187">Click **Add Attribute** tooopen hello attribute dialog.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="2b966-190">b.</span><span class="sxs-lookup"><span data-stu-id="2b966-190">b.</span></span> <span data-ttu-id="2b966-191">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2b966-191">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="2b966-192">c.</span><span class="sxs-lookup"><span data-stu-id="2b966-192">c.</span></span> <span data-ttu-id="2b966-193">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2b966-193">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="2b966-194">d.</span><span class="sxs-lookup"><span data-stu-id="2b966-194">d.</span></span> <span data-ttu-id="2b966-195">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="2b966-195">Click **Ok**</span></span>

10. <span data-ttu-id="2b966-196">Proveďte následující kroky na hello hello **název** atribut -</span><span class="sxs-lookup"><span data-stu-id="2b966-196">Perform hello following steps on hello **name** attribute-</span></span>

    <span data-ttu-id="2b966-197">a.</span><span class="sxs-lookup"><span data-stu-id="2b966-197">a.</span></span> <span data-ttu-id="2b966-198">Klikněte na hello atribut tooopen hello **Upravit atribut** okno.</span><span class="sxs-lookup"><span data-stu-id="2b966-198">Click on hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    <span data-ttu-id="2b966-200">b.</span><span class="sxs-lookup"><span data-stu-id="2b966-200">b.</span></span> <span data-ttu-id="2b966-201">Odstranit hodnotu adresy URL hello z hello **obor názvů**.</span><span class="sxs-lookup"><span data-stu-id="2b966-201">Delete hello URL value from hello **namespace**.</span></span>
    
    <span data-ttu-id="2b966-202">c.</span><span class="sxs-lookup"><span data-stu-id="2b966-202">c.</span></span> <span data-ttu-id="2b966-203">Klikněte na tlačítko **Ok** toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="2b966-203">Click **Ok** toosave hello setting.</span></span>

11. <span data-ttu-id="2b966-204">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2b966-204">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. <span data-ttu-id="2b966-206">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2b966-206">Click **Save**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="2b966-208">Přejděte příliš**nastavení správce LinkedIn** části.</span><span class="sxs-lookup"><span data-stu-id="2b966-208">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="2b966-209">Nahrajte soubor hello XML, který jste si stáhli z portálu Azure hello kliknutím na možnost soubor nahrát XML hello.</span><span class="sxs-lookup"><span data-stu-id="2b966-209">Upload hello XML file you downloaded from hello Azure portal by clicking hello Upload XML file option.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="2b966-211">Klikněte na tlačítko **na** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2b966-211">Click **On** tooenable SSO.</span></span> <span data-ttu-id="2b966-212">Jednotné přihlašování stav se změní z **Nepřipojeno** příliš**připojeno**</span><span class="sxs-lookup"><span data-stu-id="2b966-212">SSO status changes from **Not Connected** too**Connected**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2b966-214">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2b966-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="2b966-215">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2b966-215">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2b966-217">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2b966-217">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b966-218">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2b966-218">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2b966-220">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2b966-220">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2b966-222">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="2b966-222">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2b966-224">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2b966-224">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2b966-226">a.</span><span class="sxs-lookup"><span data-stu-id="2b966-226">a.</span></span> <span data-ttu-id="2b966-227">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2b966-227">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2b966-228">b.</span><span class="sxs-lookup"><span data-stu-id="2b966-228">b.</span></span> <span data-ttu-id="2b966-229">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2b966-229">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2b966-230">c.</span><span class="sxs-lookup"><span data-stu-id="2b966-230">c.</span></span> <span data-ttu-id="2b966-231">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2b966-231">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2b966-232">d.</span><span class="sxs-lookup"><span data-stu-id="2b966-232">d.</span></span> <span data-ttu-id="2b966-233">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2b966-233">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-learning-test-user"></a><span data-ttu-id="2b966-234">Vytvoření zkušebního uživatele LinkedIn Learning</span><span class="sxs-lookup"><span data-stu-id="2b966-234">Creating a LinkedIn Learning test user</span></span>

<span data-ttu-id="2b966-235">Propojené Learning aplikace podporuje.</span><span class="sxs-lookup"><span data-stu-id="2b966-235">Linked Learning Application supports.</span></span> <span data-ttu-id="2b966-236">Jenom při zřizování uživatelů čas a po ověření uživatele se automaticky vytvoří v aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2b966-236">Just in time user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="2b966-237">Na stránce Nastavení správce hello na přepínači portálu flip hello LinkedIn Learning hello **automaticky přiřadit licence** tooactive tooenable pouze v době zřizování a to také přiřadit licence toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="2b966-237">On hello admin settings page on hello LinkedIn Learning portal flip hello switch **Automatically Assign licenses** tooactive tooenable Just in time provisioning and this will also assign a license toohello user.</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2b966-239">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2b966-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2b966-240">V této části můžete povolit Britta Simon toouse Azure jednotné přihlašování a udělení přístupu tooLinkedIn učení.</span><span class="sxs-lookup"><span data-stu-id="2b966-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLinkedIn Learning.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2b966-242">**tooassign tooLinkedIn Britta Simon učení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2b966-242">**tooassign Britta Simon tooLinkedIn Learning, perform hello following steps:**</span></span>

1. <span data-ttu-id="2b966-243">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2b966-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2b966-245">V seznamu aplikace hello vyberte **LinkedIn Learning**.</span><span class="sxs-lookup"><span data-stu-id="2b966-245">In hello applications list, select **LinkedIn Learning**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. <span data-ttu-id="2b966-247">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2b966-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2b966-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2b966-249">Click **Add** button.</span></span> <span data-ttu-id="2b966-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2b966-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2b966-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2b966-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2b966-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2b966-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2b966-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2b966-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2b966-255">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2b966-255">Testing single sign-on</span></span>

<span data-ttu-id="2b966-256">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2b966-256">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2b966-257">Když kliknete na dlaždici LinkedIn Learning hello v hello přístupového panelu, měli byste obdržet hello Azure přihlašovací stránku a na po úspěšném přihlášení, měli byste obdržet do své aplikace LinkedIn Learning.</span><span class="sxs-lookup"><span data-stu-id="2b966-257">When you click hello LinkedIn Learning tile in hello Access Panel, you should get hello Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Learning application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b966-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2b966-258">Additional resources</span></span>

* [<span data-ttu-id="2b966-259">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2b966-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2b966-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2b966-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_203.png