---
title: "Kurz: Azure Active Directory integrace s zvýšení oprávnění LinkedIn | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a zvýšení oprávnění LinkedIn."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 189bd72c230be7dc0c0b934f94ea01e84af9ad23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a><span data-ttu-id="1f82f-103">Kurz: Azure Active Directory integrace s LinkedIn zvýšení oprávnění</span><span class="sxs-lookup"><span data-stu-id="1f82f-103">Tutorial: Azure Active Directory integration with LinkedIn Elevate</span></span>

<span data-ttu-id="1f82f-104">V tomto kurzu zjistíte, jak toointegrate LinkedIn zvýšení oprávnění v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1f82f-104">In this tutorial, you learn how toointegrate LinkedIn Elevate with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1f82f-105">Integrace zvýšení oprávnění LinkedIn s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1f82f-105">Integrating LinkedIn Elevate with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1f82f-106">Můžete řídit ve službě Azure AD, který má přístup tooLinkedIn zvýšení</span><span class="sxs-lookup"><span data-stu-id="1f82f-106">You can control in Azure AD who has access tooLinkedIn Elevate</span></span>
- <span data-ttu-id="1f82f-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLinkedIn zvýšení (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="1f82f-107">You can enable your users tooautomatically get signed-on tooLinkedIn Elevate (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1f82f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="1f82f-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="1f82f-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1f82f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f82f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1f82f-110">Prerequisites</span></span>

<span data-ttu-id="1f82f-111">tooconfigure integrace Azure AD s LinkedIn zvýšení oprávnění, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="1f82f-111">tooconfigure Azure AD integration with LinkedIn Elevate, you need hello following items:</span></span>

- <span data-ttu-id="1f82f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1f82f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1f82f-113">Zvýšení oprávnění LinkedIn jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1f82f-113">A LinkedIn Elevate single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1f82f-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1f82f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1f82f-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1f82f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1f82f-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="1f82f-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="1f82f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1f82f-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1f82f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1f82f-118">Scenario description</span></span>
<span data-ttu-id="1f82f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1f82f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1f82f-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1f82f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1f82f-121">Přidání LinkedIn zvýšení oprávnění z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1f82f-121">Adding LinkedIn Elevate from hello gallery</span></span>
2. <span data-ttu-id="1f82f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1f82f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-elevate-from-hello-gallery"></a><span data-ttu-id="1f82f-123">Přidání LinkedIn zvýšení oprávnění z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1f82f-123">Adding LinkedIn Elevate from hello gallery</span></span>
<span data-ttu-id="1f82f-124">tooconfigure hello integrace zvýšení oprávnění LinkedIn do Azure AD, je nutné tooadd zvýšení oprávnění LinkedIn hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1f82f-124">tooconfigure hello integration of LinkedIn Elevate into Azure AD, you need tooadd LinkedIn Elevate from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1f82f-125">**tooadd LinkedIn zvýšení oprávnění z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1f82f-125">**tooadd LinkedIn Elevate from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1f82f-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1f82f-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1f82f-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1f82f-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="1f82f-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1f82f-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="1f82f-133">Hello vyhledávacího pole zadejte **zvýšení oprávnění LinkedIn**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-133">In hello search box, type **LinkedIn Elevate**.</span></span> <span data-ttu-id="1f82f-134">Na panelu výsledků klikněte na **zvýšení oprávnění LinkedIn** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f82f-134">From results panel, click **LinkedIn Elevate** tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1f82f-136">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1f82f-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1f82f-137">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LinkedIn zvýšení oprávnění na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="1f82f-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Elevate based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1f82f-138">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v LinkedIn zvýšení oprávnění je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1f82f-138">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Elevate is tooa user in Azure AD.</span></span> <span data-ttu-id="1f82f-139">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v zvýšení oprávnění LinkedIn musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="1f82f-139">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Elevate needs toobe established.</span></span>

<span data-ttu-id="1f82f-140">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1f82f-140">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Elevate.</span></span>

<span data-ttu-id="1f82f-141">tooconfigure a testu Azure AD jednotné přihlašování s LinkedIn zvýšení oprávnění, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="1f82f-141">tooconfigure and test Azure AD single sign-on with LinkedIn Elevate, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1f82f-142">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1f82f-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1f82f-143">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1f82f-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1f82f-144">**[Vytvoření zkušebního uživatele zvýšení oprávnění LinkedIn](#creating-a-linkedin-elevate-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1f82f-144">**[Creating a LinkedIn Elevate test user](#creating-a-linkedin-elevate-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="1f82f-145">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1f82f-145">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1f82f-146">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="1f82f-146">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1f82f-147">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1f82f-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1f82f-148">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1f82f-148">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your LinkedIn Elevate application.</span></span>

<span data-ttu-id="1f82f-149">**tooconfigure Azure AD jednotné přihlašování s LinkedIn zvýšení oprávnění, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1f82f-149">**tooconfigure Azure AD single sign-on with LinkedIn Elevate, perform hello following steps:**</span></span>

1. <span data-ttu-id="1f82f-150">V hello Azure Management portal na hello **zvýšení oprávnění LinkedIn** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-150">In hello Azure Management portal, on hello **LinkedIn Elevate** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="1f82f-152">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="1f82f-152">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="1f82f-154">V okně prohlížeče jiný web, klienta LinkedIn zvýšení tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="1f82f-154">In a different web browser window, sign-on tooyour LinkedIn Elevate tenant as an administrator.</span></span>

4. <span data-ttu-id="1f82f-155">V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="1f82f-156">Kromě toho **zvýšení - zvýšení oprávnění Test AAD** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="1f82f-156">Also, select **Elevate - Elevate AAD Test** from hello dropdown list.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="1f82f-158">Klikněte na **nebo klepněte sem tooload a zkopírujte jednotlivých polí z formuláře hello** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**</span><span class="sxs-lookup"><span data-stu-id="1f82f-158">Click on **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="1f82f-160">Na portálu Azure v části **LinkedIn zvýšení oprávnění domény a adresy URL**, proveďte následující kroky, pokud chcete, aby tooconfigure jednotné přihlašování hello v **IdP iniciované** režimu</span><span class="sxs-lookup"><span data-stu-id="1f82f-160">On Azure Portal, under **LinkedIn Elevate Domain and URLs**, perform hello following steps if you want tooconfigure SSO in **IdP Initiated** mode</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="1f82f-162">a.</span><span class="sxs-lookup"><span data-stu-id="1f82f-162">a.</span></span> <span data-ttu-id="1f82f-163">V hello **identifikátor** textovému poli, zadejte hello **Entity ID** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="1f82f-163">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="1f82f-164">b.</span><span class="sxs-lookup"><span data-stu-id="1f82f-164">b.</span></span> <span data-ttu-id="1f82f-165">V hello **adresa URL odpovědi** textovému poli, zadejte hello **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="1f82f-165">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="1f82f-166">Pokud chcete, aby tooconfigure jednotné přihlašování v **iniciované SP**, klikněte na možnost zobrazit Advanced adresy URL v části Konfigurace hello a konfigurace přihlášení hello na adresu URL s hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="1f82f-166">If you want tooconfigure SSO in **SP Initiated**, then click Show Advanced URL setting option in hello configuration section and configure hello sign on URL with hello following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 
    
8. <span data-ttu-id="1f82f-168">Zvýšení oprávnění LinkedIn aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="1f82f-168">Your LinkedIn Elevate application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="1f82f-169">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="1f82f-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="1f82f-170">Výchozí hodnota Hello **uživatelský identifikátor** je **user.userprincipalname** ale zvýšení oprávnění LinkedIn očekává tento toobe namapována na hello uživatele e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="1f82f-170">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Elevate expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="1f82f-171">K tomu můžete použít **user.mail** atribut ze seznamu hello nebo použijte hodnotu hello odpovídajícího atributu na základě konfigurace vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="1f82f-171">For that you can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. <span data-ttu-id="1f82f-173">V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavte atributy hello.</span><span class="sxs-lookup"><span data-stu-id="1f82f-173">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="1f82f-174">Budete potřebovat další deklarace identity s názvem tooadd **oddělení** a hello hodnota se musí mapovat příliš toobe**user.department**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-174">You need tooadd another claim named **department** and hello value needs toobe mapped too**user.department**.</span></span>

    | <span data-ttu-id="1f82f-175">Název atributu</span><span class="sxs-lookup"><span data-stu-id="1f82f-175">Attribute Name</span></span> | <span data-ttu-id="1f82f-176">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="1f82f-176">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="1f82f-177">Oddělení</span><span class="sxs-lookup"><span data-stu-id="1f82f-177">department</span></span>| <span data-ttu-id="1f82f-178">User.Department</span><span class="sxs-lookup"><span data-stu-id="1f82f-178">user.department</span></span> |

      ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      <span data-ttu-id="1f82f-180">a.</span><span class="sxs-lookup"><span data-stu-id="1f82f-180">a.</span></span> <span data-ttu-id="1f82f-181">Kliknutím na Přidat atribut tooopen hello atribut stránku Podrobnosti o přidání atributu hello oddělení, jak je znázorněno níže-</span><span class="sxs-lookup"><span data-stu-id="1f82f-181">Click on Add attribute tooopen hello attribute details page add hello department attribute as shown below-</span></span>

      ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      <span data-ttu-id="1f82f-183">b.</span><span class="sxs-lookup"><span data-stu-id="1f82f-183">b.</span></span> <span data-ttu-id="1f82f-184">Klikněte na **Ok** toosave hello atribut.</span><span class="sxs-lookup"><span data-stu-id="1f82f-184">Click on **Ok** toosave hello attribute.</span></span>

      <span data-ttu-id="1f82f-185">c.</span><span class="sxs-lookup"><span data-stu-id="1f82f-185">c.</span></span> <span data-ttu-id="1f82f-186">Název hello změnu atributu hello **emailaddress** příliš**e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-186">Change hello name of hello attribute **emailaddress** too**email**.</span></span>


10. <span data-ttu-id="1f82f-187">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1f82f-187">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. <span data-ttu-id="1f82f-189">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-189">Click **Save**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="1f82f-191">Přejděte příliš**nastavení správce LinkedIn** části.</span><span class="sxs-lookup"><span data-stu-id="1f82f-191">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="1f82f-192">Nahrajte soubor hello XML, který jste právě stáhli z portálu Azure hello kliknutím na možnost soubor nahrát XML hello.</span><span class="sxs-lookup"><span data-stu-id="1f82f-192">Upload hello XML file you just downloaded from hello Azure portal by clicking on hello Upload XML file option.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. <span data-ttu-id="1f82f-194">Klikněte na tlačítko **na** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1f82f-194">Click **On** tooenable SSO.</span></span> <span data-ttu-id="1f82f-195">Jednotné přihlašování stav se změní z **Nepřipojeno** příliš**připojeno**</span><span class="sxs-lookup"><span data-stu-id="1f82f-195">SSO status will change from **Not Connected** too**Connected**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1f82f-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1f82f-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="1f82f-198">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1f82f-198">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="1f82f-200">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1f82f-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1f82f-201">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1f82f-201">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1f82f-203">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1f82f-203">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1f82f-205">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1f82f-205">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1f82f-207">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1f82f-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1f82f-209">a.</span><span class="sxs-lookup"><span data-stu-id="1f82f-209">a.</span></span> <span data-ttu-id="1f82f-210">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1f82f-211">b.</span><span class="sxs-lookup"><span data-stu-id="1f82f-211">b.</span></span> <span data-ttu-id="1f82f-212">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1f82f-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1f82f-213">c.</span><span class="sxs-lookup"><span data-stu-id="1f82f-213">c.</span></span> <span data-ttu-id="1f82f-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1f82f-215">d.</span><span class="sxs-lookup"><span data-stu-id="1f82f-215">d.</span></span> <span data-ttu-id="1f82f-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-216">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-elevate-test-user"></a><span data-ttu-id="1f82f-217">Vytvoření zkušebního uživatele LinkedIn zvýšení oprávnění</span><span class="sxs-lookup"><span data-stu-id="1f82f-217">Creating a LinkedIn Elevate test user</span></span>

<span data-ttu-id="1f82f-218">Propojené zvýšení oprávnění aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele budou vytvořeny v hello aplikace automaticky.</span><span class="sxs-lookup"><span data-stu-id="1f82f-218">Linked Elevate Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> <span data-ttu-id="1f82f-219">Na stránce Nastavení správce hello na přepínači portálu flip hello zvýšení oprávnění LinkedIn hello **automaticky přiřadit licence** tooactive tooenable pouze v době zřizování a to také přiřadit licence toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="1f82f-219">On hello admin settings page on hello LinkedIn Elevate portal flip hello switch **Automatically Assign licenses** tooactive tooenable Just in time provisioning and this will also assign a license toohello user.</span></span>

   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1f82f-221">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="1f82f-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1f82f-222">V této části povolíte tak, že udělíte svůj přístup tooLinkedIn zvýšení Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1f82f-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooLinkedIn Elevate.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="1f82f-224">**tooassign tooLinkedIn Britta Simon zvýšení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1f82f-224">**tooassign Britta Simon tooLinkedIn Elevate, perform hello following steps:**</span></span>

1. <span data-ttu-id="1f82f-225">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-225">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1f82f-227">V seznamu aplikace hello vyberte **zvýšení oprávnění LinkedIn**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-227">In hello applications list, select **LinkedIn Elevate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. <span data-ttu-id="1f82f-229">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1f82f-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="1f82f-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1f82f-231">Click **Add** button.</span></span> <span data-ttu-id="1f82f-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1f82f-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="1f82f-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="1f82f-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1f82f-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1f82f-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1f82f-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1f82f-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1f82f-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1f82f-237">Testing single sign-on</span></span>

<span data-ttu-id="1f82f-238">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1f82f-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1f82f-239">Když kliknete na dlaždici hello LinkedIn zvýšení oprávnění v hello přístupového panelu, měli byste obdržet hello Azure přihlašovací stránku a na po úspěšném přihlášení, měli byste obdržet do své aplikace LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1f82f-239">When you click hello LinkedIn Elevate tile in hello Access Panel, you should get hello Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Elevate application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1f82f-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1f82f-240">Additional resources</span></span>

* [<span data-ttu-id="1f82f-241">Kurz: Konfigurace LinkedIn zvýšení oprávnění pro automatické zřizování s Azure Active Directory uživatelů</span><span class="sxs-lookup"><span data-stu-id="1f82f-241">Tutorial: Configuring LinkedIn Elevate for automatic user provisioning with Azure Active Directory</span></span>](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [<span data-ttu-id="1f82f-242">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1f82f-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1f82f-243">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1f82f-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png
