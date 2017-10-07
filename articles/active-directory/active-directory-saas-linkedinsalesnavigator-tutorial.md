---
title: 'Kurz: Azure Active Directory integrace s LinkedInSalesNavigator | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LinkedInSalesNavigator."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 443d302d40d7af16aba5114e00963f23ea8d12d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a><span data-ttu-id="78a40-103">Kurz: Azure Active Directory integrace s LinkedIn prodej Navigátor</span><span class="sxs-lookup"><span data-stu-id="78a40-103">Tutorial: Azure Active Directory integration with LinkedIn Sales Navigator</span></span>

<span data-ttu-id="78a40-104">V tomto kurzu zjistíte, jak toointegrate LinkedIn prodej Navigátor službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="78a40-104">In this tutorial, you learn how toointegrate LinkedIn Sales Navigator with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="78a40-105">Integrace LinkedIn prodej Navigátor s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="78a40-105">Integrating LinkedIn Sales Navigator with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="78a40-106">Můžete řídit ve službě Azure AD, který má přístup tooLinkedIn Navigátor prodeje</span><span class="sxs-lookup"><span data-stu-id="78a40-106">You can control in Azure AD who has access tooLinkedIn Sales Navigator</span></span>
- <span data-ttu-id="78a40-107">Vaši uživatelé tooautomatically get přihlášeného tooLinkedIn prodej Navigátor (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="78a40-107">You can enable your users tooautomatically get signed-on tooLinkedIn Sales Navigator (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="78a40-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="78a40-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="78a40-109">Pokud chcete další informace o integraci aplikací SaaS v Azure AD tooknow, Procházet [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="78a40-109">If you want tooknow more details about SaaS app integration with Azure AD, browse [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78a40-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="78a40-110">Prerequisites</span></span>

<span data-ttu-id="78a40-111">tooconfigure integrace Azure AD s Navigátor LinkedIn prodej, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="78a40-111">tooconfigure Azure AD integration with LinkedIn Sales Navigator, you need hello following items:</span></span>

- <span data-ttu-id="78a40-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="78a40-112">An Azure AD subscription</span></span>
- <span data-ttu-id="78a40-113">Organizační jednotky prodej Navigátor LinkedIn jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="78a40-113">A LinkedIn Sales Navigator single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="78a40-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="78a40-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="78a40-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="78a40-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="78a40-116">Pokud to není nezbytné, vyhněte se použití produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="78a40-116">Avoid using your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="78a40-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="78a40-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="78a40-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="78a40-118">Scenario description</span></span>
<span data-ttu-id="78a40-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="78a40-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="78a40-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="78a40-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="78a40-121">Přidání LinkedIn prodej Navigátor z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="78a40-121">Adding LinkedIn Sales Navigator from hello gallery</span></span>
2. <span data-ttu-id="78a40-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="78a40-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-sales-navigator-from-hello-gallery"></a><span data-ttu-id="78a40-123">Přidání LinkedIn prodej Navigátor z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="78a40-123">Adding LinkedIn Sales Navigator from hello gallery</span></span>
<span data-ttu-id="78a40-124">tooconfigure hello integrace LinkedIn prodej Navigátor do Azure AD, je nutné tooadd LinkedIn prodej Navigátor hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="78a40-124">tooconfigure hello integration of LinkedIn Sales Navigator into Azure AD, you need tooadd LinkedIn Sales Navigator from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="78a40-125">**tooadd LinkedIn prodej Navigátor z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="78a40-125">**tooadd LinkedIn Sales Navigator from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="78a40-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="78a40-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="78a40-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="78a40-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="78a40-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="78a40-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="78a40-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="78a40-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="78a40-133">Hello vyhledávacího pole zadejte **LinkedIn prodej Navigátor**.</span><span class="sxs-lookup"><span data-stu-id="78a40-133">In hello search box, type **LinkedIn Sales Navigator**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. <span data-ttu-id="78a40-135">Na panelu výsledků hello vyberte **LinkedIn prodej Navigátor**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="78a40-135">In hello results panel, select **LinkedIn Sales Navigator**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="78a40-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="78a40-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="78a40-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Navigátor LinkedIn prodeje podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="78a40-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Sales Navigator based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="78a40-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v LinkedIn prodej navigátor je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="78a40-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Sales Navigator is tooa user in Azure AD.</span></span> <span data-ttu-id="78a40-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LinkedIn prodej Navigátor musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="78a40-140">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Sales Navigator needs toobe established.</span></span>

<span data-ttu-id="78a40-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Navigátor prodej LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="78a40-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Sales Navigator.</span></span>

<span data-ttu-id="78a40-142">tooconfigure a testu Azure AD jednotné přihlašování s Navigátor prodej LinkedIn, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="78a40-142">tooconfigure and test Azure AD single sign-on with LinkedIn Sales Navigator, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="78a40-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="78a40-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="78a40-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="78a40-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="78a40-145">**[Vytvoření zkušebního uživatele LinkedIn prodej Navigátor](#creating-a-linkedin-sales-navigator-test-user)**  -toohave protějšek Britta Simon v Navigátor LinkedIn prodej, která je propojená toohello Azure AD reprezentace hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="78a40-145">**[Creating a LinkedIn Sales Navigator test user](#creating-a-linkedin-sales-navigator-test-user)** - toohave a counterpart of Britta Simon in LinkedIn Sales Navigator that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="78a40-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="78a40-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="78a40-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="78a40-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="78a40-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="78a40-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="78a40-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Navigátor prodej LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="78a40-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LinkedIn Sales Navigator application.</span></span>

<span data-ttu-id="78a40-150">**tooconfigure Azure AD jednotné přihlašování s LinkedIn Navigátor prodej, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="78a40-150">**tooconfigure Azure AD single sign-on with LinkedIn Sales Navigator, perform hello following steps:**</span></span>

1. <span data-ttu-id="78a40-151">V portálu Azure, na hello hello **LinkedIn prodej Navigátor** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="78a40-151">In hello Azure portal, on hello **LinkedIn Sales Navigator** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="78a40-153">Na hello **jednotného přihlašování** dialogové okno, v **režimu** vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="78a40-153">On hello **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. <span data-ttu-id="78a40-155">V okně prohlížeče jiných webových přihlašování tooyour **LinkedIn prodej Navigátor** webu jako správce.</span><span class="sxs-lookup"><span data-stu-id="78a40-155">In a different web browser window, sign-on tooyour **LinkedIn Sales Navigator** website as an administrator.</span></span>

4. <span data-ttu-id="78a40-156">V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="78a40-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="78a40-157">Kromě toho **prodej Navigátor** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="78a40-157">Also, select **Sales Navigator** from hello dropdown list.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="78a40-159">Klikněte na tlačítko **nebo klepněte sem tooload a zkopírujte jednotlivých polí z formuláře hello** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**.</span><span class="sxs-lookup"><span data-stu-id="78a40-159">Click **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. <span data-ttu-id="78a40-161">Na portálu Azure v části **LinkedIn prodej Navigátor domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu.</span><span class="sxs-lookup"><span data-stu-id="78a40-161">On Azure portal, under **LinkedIn Sales Navigator Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    <span data-ttu-id="78a40-163">a.</span><span class="sxs-lookup"><span data-stu-id="78a40-163">a.</span></span> <span data-ttu-id="78a40-164">V hello **identifikátor** textovému poli, zadejte hello **Entity ID** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="78a40-164">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="78a40-165">b.</span><span class="sxs-lookup"><span data-stu-id="78a40-165">b.</span></span> <span data-ttu-id="78a40-166">V hello **adresa URL odpovědi** textovému poli, zadejte hello **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="78a40-166">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="78a40-167">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu.</span><span class="sxs-lookup"><span data-stu-id="78a40-167">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    <span data-ttu-id="78a40-169">V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span><span class="sxs-lookup"><span data-stu-id="78a40-169">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span></span>

8. <span data-ttu-id="78a40-170">Vaše **LinkedIn prodej Navigátor** aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour SAML tokenu atributy konfigurace.</span><span class="sxs-lookup"><span data-stu-id="78a40-170">Your **LinkedIn Sales Navigator** application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="78a40-171">Hello následující snímek obrazovky ukazuje příklad.</span><span class="sxs-lookup"><span data-stu-id="78a40-171">hello following screenshot shows an example.</span></span> <span data-ttu-id="78a40-172">Hello výchozí hodnotu **uživatelský identifikátor** je **user.userprincipalname** ale LinkedIn prodej Navigátor očekává, že toobe namapována na hello uživatele e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="78a40-172">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Sales Navigator expects it toobe mapped with hello user's email address.</span></span> <span data-ttu-id="78a40-173">Můžete použít **user.mail** atribut ze seznamu hello nebo použijte hodnotu hello odpovídajícího atributu na základě konfigurace vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="78a40-173">You can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. <span data-ttu-id="78a40-175">V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavte atributy hello.</span><span class="sxs-lookup"><span data-stu-id="78a40-175">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="78a40-176">Hello uživatel potřebuje tooadd čtyři deklarace identity s názvem **e-mailu**, **oddělení**, **firstname**, a **lastname** a hodnota hello je namapována na toobe **user.mail**, **user.department**, **user.givenname**, a **user.surname** v uvedeném pořadí</span><span class="sxs-lookup"><span data-stu-id="78a40-176">hello user needs tooadd four claims named **email**, **department**, **firstname**, and **lastname** and hello value is toobe mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="78a40-177">Název atributu</span><span class="sxs-lookup"><span data-stu-id="78a40-177">Attribute Name</span></span> | <span data-ttu-id="78a40-178">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="78a40-178">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="78a40-179">E-mailu</span><span class="sxs-lookup"><span data-stu-id="78a40-179">email</span></span>| <span data-ttu-id="78a40-180">User.Mail</span><span class="sxs-lookup"><span data-stu-id="78a40-180">user.mail</span></span> |
    | <span data-ttu-id="78a40-181">Oddělení</span><span class="sxs-lookup"><span data-stu-id="78a40-181">department</span></span>| <span data-ttu-id="78a40-182">User.Department</span><span class="sxs-lookup"><span data-stu-id="78a40-182">user.department</span></span> |
    | <span data-ttu-id="78a40-183">FirstName</span><span class="sxs-lookup"><span data-stu-id="78a40-183">firstname</span></span>| <span data-ttu-id="78a40-184">User.givenName</span><span class="sxs-lookup"><span data-stu-id="78a40-184">user.givenname</span></span> |
    | <span data-ttu-id="78a40-185">Příjmení</span><span class="sxs-lookup"><span data-stu-id="78a40-185">lastname</span></span>| <span data-ttu-id="78a40-186">User.Surname</span><span class="sxs-lookup"><span data-stu-id="78a40-186">user.surname</span></span> |
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    <span data-ttu-id="78a40-188">a.</span><span class="sxs-lookup"><span data-stu-id="78a40-188">a.</span></span> <span data-ttu-id="78a40-189">Klikněte na **přidat atribut** tooopen hello atribut dialogu.</span><span class="sxs-lookup"><span data-stu-id="78a40-189">Click on **Add Attribute** tooopen hello attribute dialog.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    <span data-ttu-id="78a40-192">b.</span><span class="sxs-lookup"><span data-stu-id="78a40-192">b.</span></span> <span data-ttu-id="78a40-193">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="78a40-193">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="78a40-194">c.</span><span class="sxs-lookup"><span data-stu-id="78a40-194">c.</span></span> <span data-ttu-id="78a40-195">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="78a40-195">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="78a40-196">d.</span><span class="sxs-lookup"><span data-stu-id="78a40-196">d.</span></span> <span data-ttu-id="78a40-197">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="78a40-197">Click **Ok**</span></span>

10. <span data-ttu-id="78a40-198">Proveďte následující kroky na hello hello **název** atribut -</span><span class="sxs-lookup"><span data-stu-id="78a40-198">Perform hello following steps on hello **name** attribute-</span></span>

    <span data-ttu-id="78a40-199">a.</span><span class="sxs-lookup"><span data-stu-id="78a40-199">a.</span></span> <span data-ttu-id="78a40-200">Klikněte na hello atribut tooopen hello **Upravit atribut** okno.</span><span class="sxs-lookup"><span data-stu-id="78a40-200">Click on hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    <span data-ttu-id="78a40-202">b.</span><span class="sxs-lookup"><span data-stu-id="78a40-202">b.</span></span> <span data-ttu-id="78a40-203">Odstranit hodnotu adresy URL hello z hello **obor názvů**.</span><span class="sxs-lookup"><span data-stu-id="78a40-203">Delete hello URL value from hello **namespace**.</span></span>
    
    <span data-ttu-id="78a40-204">c.</span><span class="sxs-lookup"><span data-stu-id="78a40-204">c.</span></span> <span data-ttu-id="78a40-205">Klikněte na tlačítko **Ok** toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="78a40-205">Click **Ok** toosave hello setting.</span></span>

11. <span data-ttu-id="78a40-206">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="78a40-206">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. <span data-ttu-id="78a40-208">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="78a40-208">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="78a40-210">Přejděte příliš**nastavení správce LinkedIn** části.</span><span class="sxs-lookup"><span data-stu-id="78a40-210">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="78a40-211">Klikněte na tlačítko **soubor nahrát XML** tooupload hello souboru soubor XML s metadaty, kterou jste si stáhli z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="78a40-211">Click **Upload XML file** tooupload hello Metadata XML file that you have downloaded from hello Azure portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="78a40-213">Klikněte na tlačítko **na** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="78a40-213">Click **On** tooenable SSO.</span></span> <span data-ttu-id="78a40-214">Jednotné přihlašování stav se změní z **Nepřipojeno** příliš**připojeno**</span><span class="sxs-lookup"><span data-stu-id="78a40-214">SSO status changes from **Not Connected** too**Connected**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> <span data-ttu-id="78a40-216">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="78a40-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="78a40-217">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="78a40-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="78a40-218">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="78a40-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="78a40-219">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="78a40-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="78a40-220">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="78a40-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="78a40-222">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="78a40-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="78a40-223">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="78a40-223">In hello **Azure  portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="78a40-225">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="78a40-225">Go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="78a40-227">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="78a40-227">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="78a40-229">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="78a40-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="78a40-231">a.</span><span class="sxs-lookup"><span data-stu-id="78a40-231">a.</span></span> <span data-ttu-id="78a40-232">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="78a40-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="78a40-233">b.</span><span class="sxs-lookup"><span data-stu-id="78a40-233">b.</span></span> <span data-ttu-id="78a40-234">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="78a40-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="78a40-235">c.</span><span class="sxs-lookup"><span data-stu-id="78a40-235">c.</span></span> <span data-ttu-id="78a40-236">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="78a40-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="78a40-237">d.</span><span class="sxs-lookup"><span data-stu-id="78a40-237">d.</span></span> <span data-ttu-id="78a40-238">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="78a40-238">Click **Create**.</span></span>
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a><span data-ttu-id="78a40-239">Vytvoření zkušebního uživatele LinkedIn prodej Navigátor</span><span class="sxs-lookup"><span data-stu-id="78a40-239">Creating a LinkedIn Sales Navigator test user</span></span>

<span data-ttu-id="78a40-240">Propojené aplikace Navigátor prodej podporuje pouze v zřizování uživatelů čas (JIT) a po ověření uživatele v aplikaci hello automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="78a40-240">Linked Sales Navigator Application supports Just in Time (JIT) user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="78a40-241">Aktivovat **automaticky přiřadit licence** tooassign uživatel toohello licence.</span><span class="sxs-lookup"><span data-stu-id="78a40-241">Activate **Automatically assign licenses** tooassign a license toohello user.</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="78a40-243">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="78a40-243">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="78a40-244">V této části povolíte tak, že udělíte přístup tooLinkedIn prodej Navigátor Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="78a40-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLinkedIn Sales Navigator.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="78a40-246">**tooassign tooLinkedIn Britta Simon Navigátor prodej, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="78a40-246">**tooassign Britta Simon tooLinkedIn Sales Navigator, perform hello following steps:**</span></span>

1. <span data-ttu-id="78a40-247">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="78a40-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="78a40-249">V seznamu aplikace hello vyberte **LinkedIn prodej Navigátor**.</span><span class="sxs-lookup"><span data-stu-id="78a40-249">In hello applications list, select **LinkedIn Sales Navigator**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. <span data-ttu-id="78a40-251">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="78a40-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="78a40-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="78a40-253">Click **Add** button.</span></span> <span data-ttu-id="78a40-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="78a40-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="78a40-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="78a40-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="78a40-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="78a40-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="78a40-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="78a40-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="78a40-259">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="78a40-259">Testing single sign-on</span></span>

<span data-ttu-id="78a40-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="78a40-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="78a40-261">Když kliknete na dlaždici LinkedIn prodej Navigátor hello v hello přístupový Panel, musí být přesměrované tooOrganizational stránku, kde jsou tooprovide osobní podrobnosti o účtu LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="78a40-261">When you click hello LinkedIn Sales Navigator tile in hello Access Panel, you should be redirected tooOrganizational page where you have tooprovide your personal LinkedIn account details.</span></span> <span data-ttu-id="78a40-262">Ho propojí s vaším účtem obchodní LinkedIn svůj osobní účet.</span><span class="sxs-lookup"><span data-stu-id="78a40-262">It links your personal account with your LinkedIn business account.</span></span> <span data-ttu-id="78a40-263">Další informace o hello přístupového panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="78a40-263">For more information about hello Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="78a40-264">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="78a40-264">Additional resources</span></span>

* [<span data-ttu-id="78a40-265">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="78a40-265">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="78a40-266">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="78a40-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_203.png

