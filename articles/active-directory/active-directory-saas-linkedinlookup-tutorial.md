---
title: "Kurz: Azure Active Directory integrace s LinkedIn vyhledávání | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a vyhledávání LinkedIn."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2757a39-1ead-4a3e-91e4-270be3055683
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: d79c34baa676391699e4b49806f16422fcfe73e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a><span data-ttu-id="4fca0-103">Kurz: Azure Active Directory integrace s LinkedIn vyhledávání</span><span class="sxs-lookup"><span data-stu-id="4fca0-103">Tutorial: Azure Active Directory integration with LinkedIn Lookup</span></span>

<span data-ttu-id="4fca0-104">V tomto kurzu zjistíte, jak toointegrate LinkedIn vyhledávání v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4fca0-104">In this tutorial, you learn how toointegrate LinkedIn Lookup with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4fca0-105">Integrace vyhledávání LinkedIn s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="4fca0-105">Integrating LinkedIn Lookup with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4fca0-106">Můžete řídit ve službě Azure AD, který má přístup tooLinkedIn vyhledávání</span><span class="sxs-lookup"><span data-stu-id="4fca0-106">You can control in Azure AD who has access tooLinkedIn Lookup</span></span>
- <span data-ttu-id="4fca0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLinkedIn vyhledávání (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="4fca0-107">You can enable your users tooautomatically get signed-on tooLinkedIn Lookup (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4fca0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4fca0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4fca0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4fca0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4fca0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4fca0-110">Prerequisites</span></span>

<span data-ttu-id="4fca0-111">tooconfigure integrace Azure AD s LinkedIn vyhledávání, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="4fca0-111">tooconfigure Azure AD integration with LinkedIn Lookup, you need hello following items:</span></span>

- <span data-ttu-id="4fca0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="4fca0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4fca0-113">Vyhledávání LinkedIn jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="4fca0-113">An LinkedIn Lookup single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4fca0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4fca0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4fca0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="4fca0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4fca0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="4fca0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4fca0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4fca0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4fca0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="4fca0-118">Scenario description</span></span>
<span data-ttu-id="4fca0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="4fca0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4fca0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="4fca0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4fca0-121">Přidání LinkedIn vyhledávání z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4fca0-121">Adding LinkedIn Lookup from hello gallery</span></span>
2. <span data-ttu-id="4fca0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4fca0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-lookup-from-hello-gallery"></a><span data-ttu-id="4fca0-123">Přidání LinkedIn vyhledávání z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4fca0-123">Adding LinkedIn Lookup from hello gallery</span></span>
<span data-ttu-id="4fca0-124">tooconfigure hello integrace vyhledávání LinkedIn do Azure AD, je nutné tooadd LinkedIn vyhledávání hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="4fca0-124">tooconfigure hello integration of LinkedIn Lookup into Azure AD, you need tooadd LinkedIn Lookup from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4fca0-125">**tooadd LinkedIn vyhledávání z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4fca0-125">**tooadd LinkedIn Lookup from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4fca0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4fca0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4fca0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4fca0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="4fca0-131">Klikněte na tlačítko **novou aplikaci** tlačítka v horní části hello hello dialogové okno tooadd nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fca0-131">Click **New application** button on hello top of hello dialog tooadd new application.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="4fca0-133">Hello vyhledávacího pole zadejte **LinkedIn vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-133">In hello search box, type **LinkedIn Lookup**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. <span data-ttu-id="4fca0-135">Na panelu výsledků hello vyberte **LinkedIn vyhledávání**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fca0-135">In hello results panel, select **LinkedIn Lookup**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4fca0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4fca0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4fca0-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s LinkedIn vyhledávání podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="4fca0-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Lookup based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4fca0-139">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello LinkedIn vyhledávání je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4fca0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Lookup is tooa user in Azure AD.</span></span> <span data-ttu-id="4fca0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello vyhledávání LinkedIn musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="4fca0-140">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Lookup needs toobe established.</span></span>

<span data-ttu-id="4fca0-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** LinkedIn vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fca0-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Lookup.</span></span>

<span data-ttu-id="4fca0-142">tooconfigure a testu Azure AD jednotné přihlašování s LinkedIn vyhledávání, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="4fca0-142">tooconfigure and test Azure AD single sign-on with LinkedIn Lookup, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4fca0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="4fca0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4fca0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4fca0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4fca0-145">**[Vytváření testovacího uživatele vyhledávání LinkedIn](#creating-an-linkedin-lookup-test-user)**  -toohave protějšek Britta Simon LinkedIn vyhledávání, která je propojená toohello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4fca0-145">**[Creating an LinkedIn Lookup test user](#creating-an-linkedin-lookup-test-user)** - toohave a counterpart of Britta Simon in LinkedIn Lookup that is linked toohello Azure AD representation.</span></span>
4. <span data-ttu-id="4fca0-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4fca0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4fca0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="4fca0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4fca0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4fca0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4fca0-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LinkedIn vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fca0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LinkedIn Lookup application.</span></span>

<span data-ttu-id="4fca0-150">**tooconfigure Azure AD jednotné přihlašování s LinkedIn vyhledávání, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4fca0-150">**tooconfigure Azure AD single sign-on with LinkedIn Lookup, perform hello following steps:**</span></span>

1. <span data-ttu-id="4fca0-151">V portálu Azure, na hello hello **LinkedIn vyhledávání** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-151">In hello Azure portal, on hello **LinkedIn Lookup** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="4fca0-153">Na hello **jednotného přihlašování** dialogové okno, v **režimu** vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4fca0-153">On hello **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. <span data-ttu-id="4fca0-155">V okně prohlížeče jiných webových přihlašování tooyour **LinkedIn vyhledávání** webu jako správce.</span><span class="sxs-lookup"><span data-stu-id="4fca0-155">In a different web browser window, sign-on tooyour **LinkedIn Lookup** website as an administrator.</span></span>

4. <span data-ttu-id="4fca0-156">V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="4fca0-157">Kromě toho **vyhledávání** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="4fca0-157">Also, select **Lookup** from hello dropdown list.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. <span data-ttu-id="4fca0-159">Klikněte na tlačítko **nebo klepněte sem tooload a zkopírujte jednotlivých polí z formuláře hello** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**</span><span class="sxs-lookup"><span data-stu-id="4fca0-159">Click **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. <span data-ttu-id="4fca0-161">Na portálu Azure v části **LinkedIn vyhledávání domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="4fca0-161">On Azure portal, under **LinkedIn Lookup Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    <span data-ttu-id="4fca0-163">a.</span><span class="sxs-lookup"><span data-stu-id="4fca0-163">a.</span></span> <span data-ttu-id="4fca0-164">V hello **identifikátor** textovému poli, zadejte hello **Entity ID** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="4fca0-164">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="4fca0-165">b.</span><span class="sxs-lookup"><span data-stu-id="4fca0-165">b.</span></span> <span data-ttu-id="4fca0-166">V hello **adresa URL odpovědi** textovému poli, zadejte hello **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="4fca0-166">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="4fca0-167">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="4fca0-167">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    <span data-ttu-id="4fca0-169">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span><span class="sxs-lookup"><span data-stu-id="4fca0-169">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="4fca0-170">Toto není skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4fca0-170">This is not real value.</span></span> <span data-ttu-id="4fca0-171">má uživatel Hello tooupdate tyto hodnoty s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4fca0-171">hello user has tooupdate these values with hello actual Sign-On URL.</span></span> <span data-ttu-id="4fca0-172">Obraťte se na [tým podpory klienta sítě LinkedIn vyhledávání](https://business.LinkedIn.com/lookup) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4fca0-172">Contact [LinkedIn Lookup Client support team](https://business.LinkedIn.com/lookup) tooget this value.</span></span>

8. <span data-ttu-id="4fca0-173">Vaše **LinkedIn vyhledávání** aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="4fca0-173">Your **LinkedIn Lookup** application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="4fca0-174">Hello uživatel má tooadd vlastních atributů mapování toohello SAML tokenu atributy konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4fca0-174">hello user has tooadd custom attribute mappings toohello SAML token attributes configuration.</span></span> <span data-ttu-id="4fca0-175">Hello následující snímek obrazovky ukazuje příklad.</span><span class="sxs-lookup"><span data-stu-id="4fca0-175">hello following screenshot shows an example.</span></span> <span data-ttu-id="4fca0-176">Výchozí hodnota Hello **uživatelský identifikátor** je **user.userprincipalname** ale LinkedIn vyhledávání očekává tento toobe namapována na hello uživatele e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="4fca0-176">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Lookup expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="4fca0-177">Můžete použít **user.mail** atribut ze seznamu hello nebo použijte hodnotu hello odpovídajícího atributu na základě konfigurace vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="4fca0-177">You can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. <span data-ttu-id="4fca0-179">V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavte atributy hello.</span><span class="sxs-lookup"><span data-stu-id="4fca0-179">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="4fca0-180">Hello uživatel potřebuje tooadd čtyři deklarace identity s názvem **e-mailu**, **oddělení**, **firstname**, a **lastname** a hodnota hello je namapována na toobe **user.mail**, **user.department**, **user.givenname**, a **user.surname** v uvedeném pořadí</span><span class="sxs-lookup"><span data-stu-id="4fca0-180">hello user needs tooadd four claims named **email**,  **department**, **firstname**, and **lastname** and hello value is toobe mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="4fca0-181">Název atributu</span><span class="sxs-lookup"><span data-stu-id="4fca0-181">Attribute Name</span></span> | <span data-ttu-id="4fca0-182">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="4fca0-182">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="4fca0-183">E-mailu</span><span class="sxs-lookup"><span data-stu-id="4fca0-183">email</span></span>| <span data-ttu-id="4fca0-184">User.Mail</span><span class="sxs-lookup"><span data-stu-id="4fca0-184">user.mail</span></span> |    
    | <span data-ttu-id="4fca0-185">Oddělení</span><span class="sxs-lookup"><span data-stu-id="4fca0-185">department</span></span>| <span data-ttu-id="4fca0-186">User.Department</span><span class="sxs-lookup"><span data-stu-id="4fca0-186">user.department</span></span> |
    | <span data-ttu-id="4fca0-187">FirstName</span><span class="sxs-lookup"><span data-stu-id="4fca0-187">firstname</span></span>| <span data-ttu-id="4fca0-188">User.givenName</span><span class="sxs-lookup"><span data-stu-id="4fca0-188">user.givenname</span></span> |
    | <span data-ttu-id="4fca0-189">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4fca0-189">lastname</span></span>| <span data-ttu-id="4fca0-190">User.Surname</span><span class="sxs-lookup"><span data-stu-id="4fca0-190">user.surname</span></span> |

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    <span data-ttu-id="4fca0-192">a.</span><span class="sxs-lookup"><span data-stu-id="4fca0-192">a.</span></span> <span data-ttu-id="4fca0-193">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fca0-193">Click **Add Attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    <span data-ttu-id="4fca0-196">b.</span><span class="sxs-lookup"><span data-stu-id="4fca0-196">b.</span></span> <span data-ttu-id="4fca0-197">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="4fca0-197">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="4fca0-198">c.</span><span class="sxs-lookup"><span data-stu-id="4fca0-198">c.</span></span> <span data-ttu-id="4fca0-199">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="4fca0-199">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="4fca0-200">d.</span><span class="sxs-lookup"><span data-stu-id="4fca0-200">d.</span></span> <span data-ttu-id="4fca0-201">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="4fca0-201">Click **Ok**</span></span>

10. <span data-ttu-id="4fca0-202">Proveďte následující kroky na hello hello **název** atribut -</span><span class="sxs-lookup"><span data-stu-id="4fca0-202">Perform hello following steps on hello **name** attribute-</span></span>

    <span data-ttu-id="4fca0-203">a.</span><span class="sxs-lookup"><span data-stu-id="4fca0-203">a.</span></span> <span data-ttu-id="4fca0-204">Klikněte na hello atribut tooopen hello **Upravit atribut** okno.</span><span class="sxs-lookup"><span data-stu-id="4fca0-204">Click on hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    <span data-ttu-id="4fca0-206">b.</span><span class="sxs-lookup"><span data-stu-id="4fca0-206">b.</span></span> <span data-ttu-id="4fca0-207">Odstranit hodnotu adresy URL hello z hello **obor názvů**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-207">Delete hello URL value from hello **namespace**.</span></span>
    
    <span data-ttu-id="4fca0-208">c.</span><span class="sxs-lookup"><span data-stu-id="4fca0-208">c.</span></span> <span data-ttu-id="4fca0-209">Klikněte na tlačítko **Ok** toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="4fca0-209">Click **Ok** toosave hello setting.</span></span>

10. <span data-ttu-id="4fca0-210">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="4fca0-210">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. <span data-ttu-id="4fca0-212">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4fca0-212">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="4fca0-214">Přejděte příliš**nastavení správce LinkedIn** části.</span><span class="sxs-lookup"><span data-stu-id="4fca0-214">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="4fca0-215">Soubor XML hello nahrávání jste si stáhli z portálu Azure hello kliknutím hello **soubor nahrát XML** možnost.</span><span class="sxs-lookup"><span data-stu-id="4fca0-215">Upload hello XML file you downloaded from hello Azure portal by clicking hello **Upload XML file** option.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. <span data-ttu-id="4fca0-217">Klikněte na tlačítko **na** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4fca0-217">Click **On** tooenable SSO.</span></span> <span data-ttu-id="4fca0-218">Jednotné přihlašování stav se změní z **Nepřipojeno** příliš**připojeno**</span><span class="sxs-lookup"><span data-stu-id="4fca0-218">SSO status changes from **Not Connected** too**Connected**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> <span data-ttu-id="4fca0-220">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="4fca0-220">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4fca0-221">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="4fca0-221">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4fca0-222">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4fca0-222">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4fca0-223">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4fca0-223">Creating an Azure AD test user</span></span>
<span data-ttu-id="4fca0-224">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="4fca0-224">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="4fca0-226">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4fca0-226">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4fca0-227">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4fca0-227">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4fca0-229">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-229">Go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4fca0-231">Klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fca0-231">Click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4fca0-233">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4fca0-233">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4fca0-235">a.</span><span class="sxs-lookup"><span data-stu-id="4fca0-235">a.</span></span> <span data-ttu-id="4fca0-236">V hello **název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-236">In hello **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="4fca0-237">b.</span><span class="sxs-lookup"><span data-stu-id="4fca0-237">b.</span></span> <span data-ttu-id="4fca0-238">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4fca0-238">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="4fca0-239">c.</span><span class="sxs-lookup"><span data-stu-id="4fca0-239">c.</span></span> <span data-ttu-id="4fca0-240">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-240">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4fca0-241">d.</span><span class="sxs-lookup"><span data-stu-id="4fca0-241">d.</span></span> <span data-ttu-id="4fca0-242">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-242">Click **Create**.</span></span>
 
### <a name="creating-an-linkedin-lookup-test-user"></a><span data-ttu-id="4fca0-243">Vytváření testovacího uživatele vyhledávání LinkedIn</span><span class="sxs-lookup"><span data-stu-id="4fca0-243">Creating an LinkedIn Lookup test user</span></span>

<span data-ttu-id="4fca0-244">Propojené vyhledávání aplikace podporuje pouze v zřizování uživatelů čas (JIT) a po ověření uživatele v aplikaci hello automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="4fca0-244">Linked Lookup Application supports Just in Time (JIT) user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="4fca0-245">Aktivovat **automaticky přiřadit licence** tooassign uživatel toohello licence.</span><span class="sxs-lookup"><span data-stu-id="4fca0-245">Activate **Automatically assign licenses** tooassign a license toohello user.</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4fca0-247">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="4fca0-247">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4fca0-248">V této části povolíte tak, že udělíte přístup tooLinkedIn vyhledávání toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4fca0-248">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLinkedIn Lookup.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="4fca0-250">**tooassign tooLinkedIn Britta Simon vyhledávání, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4fca0-250">**tooassign Britta Simon tooLinkedIn Lookup, perform hello following steps:**</span></span>

1. <span data-ttu-id="4fca0-251">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-251">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="4fca0-253">V seznamu aplikace hello vyberte **LinkedIn vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-253">In hello applications list, select **LinkedIn Lookup**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. <span data-ttu-id="4fca0-255">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="4fca0-255">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="4fca0-257">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4fca0-257">Click **Add** button.</span></span> <span data-ttu-id="4fca0-258">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fca0-258">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="4fca0-260">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="4fca0-260">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4fca0-261">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fca0-261">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4fca0-262">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fca0-262">Click **Assign** button on **Add Assignment** dialog.</span></span>

    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4fca0-263">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4fca0-263">Testing single sign-on</span></span>

<span data-ttu-id="4fca0-264">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="4fca0-264">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4fca0-265">Po kliknutí na tlačítko hello LinkedIn vyhledávání dlaždici v hello přístupový Panel, musí být přesměrované tooOrganizational stránku, kde jsou tooprovide osobní podrobnosti o účtu LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="4fca0-265">When you click hello LinkedIn Lookup tile in hello Access Panel, you should be redirected tooOrganizational page where you have tooprovide your personal LinkedIn account details.</span></span> <span data-ttu-id="4fca0-266">Ho propojí s vaším účtem obchodní LinkedIn svůj osobní účet.</span><span class="sxs-lookup"><span data-stu-id="4fca0-266">It links your personal account with your LinkedIn business account.</span></span> 

<span data-ttu-id="4fca0-267">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4fca0-267">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4fca0-268">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4fca0-268">Additional resources</span></span>

* [<span data-ttu-id="4fca0-269">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4fca0-269">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4fca0-270">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4fca0-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_203.png

