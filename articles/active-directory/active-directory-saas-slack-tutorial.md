---
title: 'Kurz: Azure Active Directory integrace s Slack | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Slack."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 7f0151401af4dc63d2f714d4b4f66380c4b51e0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a><span data-ttu-id="1fc77-103">Kurz: Azure Active Directory integrace s Slack</span><span class="sxs-lookup"><span data-stu-id="1fc77-103">Tutorial: Azure Active Directory integration with Slack</span></span>

<span data-ttu-id="1fc77-104">V tomto kurzu zjistíte, jak toointegrate Slack s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1fc77-104">In this tutorial, you learn how toointegrate Slack with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1fc77-105">Integrace systému Slack s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1fc77-105">Integrating Slack with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1fc77-106">Můžete řídit ve službě Azure AD, který má přístup tooSlack</span><span class="sxs-lookup"><span data-stu-id="1fc77-106">You can control in Azure AD who has access tooSlack</span></span>
- <span data-ttu-id="1fc77-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSlack (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="1fc77-107">You can enable your users tooautomatically get signed-on tooSlack (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1fc77-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1fc77-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1fc77-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1fc77-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1fc77-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1fc77-110">Prerequisites</span></span>

<span data-ttu-id="1fc77-111">Integrace služby Azure AD s Slack tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="1fc77-111">tooconfigure Azure AD integration with Slack, you need hello following items:</span></span>

- <span data-ttu-id="1fc77-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1fc77-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1fc77-113">Slack jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1fc77-113">A Slack single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1fc77-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1fc77-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1fc77-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1fc77-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1fc77-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="1fc77-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1fc77-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1fc77-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1fc77-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1fc77-118">Scenario description</span></span>
<span data-ttu-id="1fc77-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1fc77-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1fc77-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1fc77-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1fc77-121">Přidání Slack z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1fc77-121">Adding Slack from hello gallery</span></span>
2. <span data-ttu-id="1fc77-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1fc77-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-slack-from-hello-gallery"></a><span data-ttu-id="1fc77-123">Přidání Slack z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1fc77-123">Adding Slack from hello gallery</span></span>
<span data-ttu-id="1fc77-124">integrace hello tooconfigure Slack do služby Azure AD, je nutné tooadd Slack hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1fc77-124">tooconfigure hello integration of Slack into Azure AD, you need tooadd Slack from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1fc77-125">**tooadd Slack z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1fc77-125">**tooadd Slack from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1fc77-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1fc77-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1fc77-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1fc77-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="1fc77-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1fc77-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="1fc77-133">Hello vyhledávacího pole zadejte **Slack**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-133">In hello search box, type **Slack**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_search.png)

5. <span data-ttu-id="1fc77-135">Na panelu výsledků hello vyberte **Slack**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1fc77-135">In hello results panel, select **Slack**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1fc77-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1fc77-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1fc77-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Slack podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="1fc77-138">In this section, you configure and test Azure AD single sign-on with Slack based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1fc77-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v systému Slack je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1fc77-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Slack is tooa user in Azure AD.</span></span> <span data-ttu-id="1fc77-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Slack musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="1fc77-140">In other words, a link relationship between an Azure AD user and hello related user in Slack needs toobe established.</span></span>

<span data-ttu-id="1fc77-141">V systému Slack, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="1fc77-141">In Slack, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1fc77-142">tooconfigure a testu Azure AD jednotné přihlašování s Slack, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="1fc77-142">tooconfigure and test Azure AD single sign-on with Slack, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1fc77-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1fc77-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1fc77-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1fc77-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1fc77-145">**[Vytváření Slack testovacího uživatele](#creating-a-slack-test-user)**  -toohave protějšek Britta Simon v Slack, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="1fc77-145">**[Creating a Slack test user](#creating-a-slack-test-user)** - toohave a counterpart of Britta Simon in Slack that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1fc77-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1fc77-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1fc77-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="1fc77-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1fc77-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1fc77-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1fc77-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Slack.</span><span class="sxs-lookup"><span data-stu-id="1fc77-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Slack application.</span></span>

<span data-ttu-id="1fc77-150">**tooconfigure Azure AD jednotné přihlašování s Slack, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1fc77-150">**tooconfigure Azure AD single sign-on with Slack, perform hello following steps:**</span></span>

1. <span data-ttu-id="1fc77-151">V portálu Azure, na hello hello **Slack** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-151">In hello Azure portal, on hello **Slack** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="1fc77-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1fc77-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_samlbase.png)

3. <span data-ttu-id="1fc77-155">Na hello **Slack domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1fc77-155">On hello **Slack Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_url.png)

    <span data-ttu-id="1fc77-157">a.</span><span class="sxs-lookup"><span data-stu-id="1fc77-157">a.</span></span> <span data-ttu-id="1fc77-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.slack.com`</span><span class="sxs-lookup"><span data-stu-id="1fc77-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.slack.com`</span></span>

    <span data-ttu-id="1fc77-159">b.</span><span class="sxs-lookup"><span data-stu-id="1fc77-159">b.</span></span> <span data-ttu-id="1fc77-160">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://slack.com`</span><span class="sxs-lookup"><span data-stu-id="1fc77-160">In hello **Identifier** textbox, type hello URL: `https://slack.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1fc77-161">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="1fc77-161">hello value is not real.</span></span> <span data-ttu-id="1fc77-162">Máte tooupdate hello hodnotu s hello skutečné přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="1fc77-162">You have tooupdate hello value with hello actual Sign On URL.</span></span> <span data-ttu-id="1fc77-163">Obraťte se na [tým podpory Slack](https://slack.com/help/contact) tooget hello hodnota</span><span class="sxs-lookup"><span data-stu-id="1fc77-163">Contact [Slack support team](https://slack.com/help/contact) tooget hello value</span></span>
     
4. <span data-ttu-id="1fc77-164">Slack aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="1fc77-164">Slack application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="1fc77-165">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1fc77-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="1fc77-166">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="1fc77-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="1fc77-167">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="1fc77-167">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute.png)

5. <span data-ttu-id="1fc77-169">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogovém okně, vyberte **user.mail** jako **uživatelský identifikátor** a pro každý řádek ukazuje v tabulce hello níže proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1fc77-169">In hello **User Attributes** section on hello **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in hello table below, perform hello following steps:</span></span>
    
    | <span data-ttu-id="1fc77-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="1fc77-170">Attribute Name</span></span> | <span data-ttu-id="1fc77-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="1fc77-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="1fc77-172">křestní_jméno</span><span class="sxs-lookup"><span data-stu-id="1fc77-172">first_name</span></span> | <span data-ttu-id="1fc77-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="1fc77-173">user.givenname</span></span> |
    | <span data-ttu-id="1fc77-174">Příjmení</span><span class="sxs-lookup"><span data-stu-id="1fc77-174">last_name</span></span> | <span data-ttu-id="1fc77-175">User.Surname</span><span class="sxs-lookup"><span data-stu-id="1fc77-175">user.surname</span></span> |
    | <span data-ttu-id="1fc77-176">User.Email</span><span class="sxs-lookup"><span data-stu-id="1fc77-176">User.Email</span></span> | <span data-ttu-id="1fc77-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="1fc77-177">user.mail</span></span> |  
    | <span data-ttu-id="1fc77-178">User.Username</span><span class="sxs-lookup"><span data-stu-id="1fc77-178">User.Username</span></span> | <span data-ttu-id="1fc77-179">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="1fc77-179">user.userprincipalname</span></span> |

    <span data-ttu-id="1fc77-180">a.</span><span class="sxs-lookup"><span data-stu-id="1fc77-180">a.</span></span> <span data-ttu-id="1fc77-181">Klikněte na **atribut** tooopen **Upravit atribut** dialogové okno pole a provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1fc77-181">Click on **Attribute** tooopen **Edit Attribute** dialog box and perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute1.png)

    <span data-ttu-id="1fc77-183">a.</span><span class="sxs-lookup"><span data-stu-id="1fc77-183">a.</span></span> <span data-ttu-id="1fc77-184">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="1fc77-184">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="1fc77-185">b.</span><span class="sxs-lookup"><span data-stu-id="1fc77-185">b.</span></span> <span data-ttu-id="1fc77-186">Z hello **hodnotu** seznamu, vyberte hello hodnota atributu zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="1fc77-186">From hello **Value** list, select hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="1fc77-187">c.</span><span class="sxs-lookup"><span data-stu-id="1fc77-187">c.</span></span> <span data-ttu-id="1fc77-188">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-188">Click **OK**</span></span>

6. <span data-ttu-id="1fc77-189">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1fc77-189">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_certificate.png)

7. <span data-ttu-id="1fc77-191">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1fc77-191">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="1fc77-193">Na hello **Slack konfigurace** klikněte na tlačítko **konfigurace Slack** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="1fc77-193">On hello **Slack Configuration** section, click **Configure Slack** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1fc77-194">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="1fc77-194">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_configure.png) 

9.  <span data-ttu-id="1fc77-196">V okně prohlížeče jiný web Přihlaste se jako správce v lokalitě tooyour Slack společnosti.</span><span class="sxs-lookup"><span data-stu-id="1fc77-196">In a different web browser window, log in tooyour Slack company site as an administrator.</span></span>

10.  <span data-ttu-id="1fc77-197">Přejděte příliš**Microsoft Azure AD** potom přejděte příliš**nastavení Team**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-197">Navigate too**Microsoft Azure AD** then go too**Team Settings**.</span></span>

     ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

11.  <span data-ttu-id="1fc77-199">V hello **nastavení Team** klikněte na tlačítko hello **ověřování** a pak klikněte **změnit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-199">In hello **Team Settings** section, click hello **Authentication** tab, and then click **Change Settings**.</span></span>

     ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

12. <span data-ttu-id="1fc77-201">Na hello **nastavení ověřování SAML** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1fc77-201">On hello **SAML Authentication Settings** dialog, perform hello following steps:</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    <span data-ttu-id="1fc77-203">a.</span><span class="sxs-lookup"><span data-stu-id="1fc77-203">a.</span></span>  <span data-ttu-id="1fc77-204">V hello **SAML 2.0 koncový bod (HTTP)** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1fc77-204">In hello **SAML 2.0 Endpoint (HTTP)** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="1fc77-205">b.</span><span class="sxs-lookup"><span data-stu-id="1fc77-205">b.</span></span>  <span data-ttu-id="1fc77-206">V hello **vystavitele zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1fc77-206">In hello **Identity Provider Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="1fc77-207">c.</span><span class="sxs-lookup"><span data-stu-id="1fc77-207">c.</span></span>  <span data-ttu-id="1fc77-208">Otevřete soubor stažený certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **veřejný certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1fc77-208">Open your downloaded certificate file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Public Certificate** textbox.</span></span>

    <span data-ttu-id="1fc77-209">d.</span><span class="sxs-lookup"><span data-stu-id="1fc77-209">d.</span></span> <span data-ttu-id="1fc77-210">Proveďte konfiguraci hello výše uvedených tří nastavení vhodnou pro váš tým Slack.</span><span class="sxs-lookup"><span data-stu-id="1fc77-210">Configure hello above three settings as appropriate for your Slack team.</span></span> <span data-ttu-id="1fc77-211">Další informace o nastavení hello najde hello **příručce Konfigurace jednotného přihlašování k systému Slack na** sem.</span><span class="sxs-lookup"><span data-stu-id="1fc77-211">For more information about hello settings, please find hello **Slack's SSO configuration guide** here.</span></span> `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    <span data-ttu-id="1fc77-212">e.</span><span class="sxs-lookup"><span data-stu-id="1fc77-212">e.</span></span>  <span data-ttu-id="1fc77-213">Klikněte na tlačítko **uložte konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-213">Click **Save Configuration**.</span></span>
     
    <!-- Deselect **Allow users toochange their email address**.

    e.  Select **Allow users toochoose their own username**.

    f.  As **Authentication for your team must be used by**, select **It’s optional**. -->

> [!TIP]
> <span data-ttu-id="1fc77-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="1fc77-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1fc77-215">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="1fc77-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1fc77-216">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1fc77-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1fc77-217">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1fc77-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="1fc77-218">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="1fc77-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="1fc77-220">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1fc77-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1fc77-221">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1fc77-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1fc77-223">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1fc77-225">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="1fc77-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1fc77-227">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1fc77-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1fc77-229">a.</span><span class="sxs-lookup"><span data-stu-id="1fc77-229">a.</span></span> <span data-ttu-id="1fc77-230">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1fc77-231">b.</span><span class="sxs-lookup"><span data-stu-id="1fc77-231">b.</span></span> <span data-ttu-id="1fc77-232">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1fc77-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1fc77-233">c.</span><span class="sxs-lookup"><span data-stu-id="1fc77-233">c.</span></span> <span data-ttu-id="1fc77-234">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1fc77-235">d.</span><span class="sxs-lookup"><span data-stu-id="1fc77-235">d.</span></span> <span data-ttu-id="1fc77-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-236">Click **Create**.</span></span>
 
### <a name="creating-a-slack-test-user"></a><span data-ttu-id="1fc77-237">Vytváření Slack zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="1fc77-237">Creating a Slack test user</span></span>

<span data-ttu-id="1fc77-238">Hello cílem této části je toocreate uživatel volal Britta Simon v Slack.</span><span class="sxs-lookup"><span data-stu-id="1fc77-238">hello objective of this section is toocreate a user called Britta Simon in Slack.</span></span> <span data-ttu-id="1fc77-239">Slack podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="1fc77-239">Slack supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="1fc77-240">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="1fc77-240">There is no action item for you in this section.</span></span> <span data-ttu-id="1fc77-241">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess Slack.</span><span class="sxs-lookup"><span data-stu-id="1fc77-241">A new user is created during an attempt tooaccess Slack if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="1fc77-242">Pokud potřebujete toocreate uživatel ručně, je nutné tooContact [tým podpory Slack](https://slack.com/help/contact).</span><span class="sxs-lookup"><span data-stu-id="1fc77-242">If you need toocreate a user manually, you need tooContact [Slack support team](https://slack.com/help/contact).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1fc77-243">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="1fc77-243">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1fc77-244">V této části povolíte tak, že udělíte přístup tooSlack toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1fc77-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSlack.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="1fc77-246">**tooassign Britta Simon tooSlack, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1fc77-246">**tooassign Britta Simon tooSlack, perform hello following steps:**</span></span>

1. <span data-ttu-id="1fc77-247">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1fc77-249">V seznamu aplikace hello vyberte **Slack**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-249">In hello applications list, select **Slack**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-slack-tutorial/tutorial_slack_app.png) 

3. <span data-ttu-id="1fc77-251">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1fc77-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="1fc77-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1fc77-253">Click **Add** button.</span></span> <span data-ttu-id="1fc77-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1fc77-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="1fc77-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="1fc77-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1fc77-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1fc77-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1fc77-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1fc77-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1fc77-259">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1fc77-259">Testing single sign-on</span></span>

<span data-ttu-id="1fc77-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1fc77-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1fc77-261">Po kliknutí na tlačítko hello Slack dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Slack aplikace.</span><span class="sxs-lookup"><span data-stu-id="1fc77-261">When you click hello Slack tile in hello Access Panel, you should get automatically signed-on tooyour Slack application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1fc77-262">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1fc77-262">Additional resources</span></span>

* [<span data-ttu-id="1fc77-263">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1fc77-263">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1fc77-264">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1fc77-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png

