---
title: 'Kurz: Azure Active Directory integrace s TimeOffManager | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TimeOffManager."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: c871257bfb49883e31b1c4860a9d7faa70e9ab48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a><span data-ttu-id="b0daa-103">Kurz: Azure Active Directory integrace s TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="b0daa-103">Tutorial: Azure Active Directory integration with TimeOffManager</span></span>

<span data-ttu-id="b0daa-104">V tomto kurzu zjistíte, jak toointegrate TimeOffManager s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b0daa-104">In this tutorial, you learn how toointegrate TimeOffManager with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b0daa-105">Integrace TimeOffManager s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b0daa-105">Integrating TimeOffManager with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b0daa-106">Můžete řídit ve službě Azure AD, který má přístup tooTimeOffManager</span><span class="sxs-lookup"><span data-stu-id="b0daa-106">You can control in Azure AD who has access tooTimeOffManager</span></span>
- <span data-ttu-id="b0daa-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTimeOffManager (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0daa-107">You can enable your users tooautomatically get signed-on tooTimeOffManager (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b0daa-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b0daa-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b0daa-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b0daa-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0daa-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b0daa-110">Prerequisites</span></span>

<span data-ttu-id="b0daa-111">Integrace služby Azure AD s TimeOffManager tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b0daa-111">tooconfigure Azure AD integration with TimeOffManager, you need hello following items:</span></span>

- <span data-ttu-id="b0daa-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0daa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b0daa-113">TimeOffManager jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b0daa-113">A TimeOffManager single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b0daa-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0daa-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b0daa-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b0daa-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b0daa-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b0daa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b0daa-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b0daa-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b0daa-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b0daa-118">Scenario description</span></span>
<span data-ttu-id="b0daa-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0daa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b0daa-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b0daa-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b0daa-121">Přidat TimeOffManager z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b0daa-121">Add TimeOffManager from hello gallery</span></span>
2. <span data-ttu-id="b0daa-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0daa-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-timeoffmanager-from-hello-gallery"></a><span data-ttu-id="b0daa-123">Přidat TimeOffManager z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b0daa-123">Add TimeOffManager from hello gallery</span></span>
<span data-ttu-id="b0daa-124">tooconfigure hello integrace TimeOffManager do Azure AD, je nutné tooadd TimeOffManager hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b0daa-124">tooconfigure hello integration of TimeOffManager into Azure AD, you need tooadd TimeOffManager from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b0daa-125">**tooadd TimeOffManager z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b0daa-125">**tooadd TimeOffManager from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0daa-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b0daa-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b0daa-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b0daa-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b0daa-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b0daa-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b0daa-133">Hello vyhledávacího pole zadejte **TimeOffManager**, vyberte **TimeOffManager** z panelu výsledek a pak klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0daa-133">In hello search box, type **TimeOffManager**, select **TimeOffManager** from result panel and then click **Add** button tooadd hello application.</span></span>

    ![Přidat z Galerie](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b0daa-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0daa-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="b0daa-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TimeOffManager podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b0daa-136">In this section, you configure and test Azure AD single sign-on with TimeOffManager based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b0daa-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v TimeOffManager je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b0daa-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TimeOffManager is tooa user in Azure AD.</span></span> <span data-ttu-id="b0daa-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v TimeOffManager musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b0daa-138">In other words, a link relationship between an Azure AD user and hello related user in TimeOffManager needs toobe established.</span></span>

<span data-ttu-id="b0daa-139">V TimeOffManager, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b0daa-139">In TimeOffManager, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b0daa-140">tooconfigure a testu Azure AD jednotné přihlašování s TimeOffManager, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b0daa-140">tooconfigure and test Azure AD single sign-on with TimeOffManager, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b0daa-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b0daa-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b0daa-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0daa-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b0daa-143">**[Vytvoření zkušebního uživatele TimeOffManager](#create-a-timeoffmanager-test-user)**  -toohave protějšek Britta Simon v TimeOffManager, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b0daa-143">**[Create a TimeOffManager test user](#create-a-timeoffmanager-test-user)** - toohave a counterpart of Britta Simon in TimeOffManager that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b0daa-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b0daa-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b0daa-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b0daa-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b0daa-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0daa-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b0daa-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="b0daa-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TimeOffManager application.</span></span>

<span data-ttu-id="b0daa-148">**tooconfigure Azure AD jednotné přihlašování s TimeOffManager, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b0daa-148">**tooconfigure Azure AD single sign-on with TimeOffManager, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0daa-149">V portálu Azure, na hello hello **TimeOffManager** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-149">In hello Azure portal, on hello **TimeOffManager** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b0daa-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b0daa-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Přihlášení na základě SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. <span data-ttu-id="b0daa-153">Na hello **TimeOffManager domény a adresy URL** část, proveďte následující hello:</span><span class="sxs-lookup"><span data-stu-id="b0daa-153">On hello **TimeOffManager Domain and URLs** section, perform hello following:</span></span>

     ![Část TimeOffManager domény a adresy URL](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    <span data-ttu-id="b0daa-155">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span><span class="sxs-lookup"><span data-stu-id="b0daa-155">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b0daa-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="b0daa-156">This value is not real.</span></span> <span data-ttu-id="b0daa-157">Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b0daa-157">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="b0daa-158">Můžete získat tuto hodnotu z **jednotného přihlašování na stránce nastavení** kterého se vysvětluje dále v kurzu hello nebo kontaktujte [tým podpory TimeOffManager](http://www.timeoffmanager.com/contact-us.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0daa-158">You can get this value from **Single Sign on settings page** which is explained later in hello tutorial or Contact [TimeOffManager support team](http://www.timeoffmanager.com/contact-us.aspx).</span></span>
 
4. <span data-ttu-id="b0daa-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b0daa-159">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. <span data-ttu-id="b0daa-161">Hello cílem této části je toooutline jak tooTimeOffManger tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="b0daa-161">hello objective of this section is toooutline how tooenable users tooauthenticate tooTimeOffManger with their account in Azure AD using federation based on hello SAML protocol.</span></span>
    
    <span data-ttu-id="b0daa-162">Aplikace TimeOffManger očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="b0daa-162">Your TimeOffManger application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="b0daa-163">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="b0daa-163">hello following screenshot shows an example for this.</span></span>

    <span data-ttu-id="b0daa-164">![atributy tokenu SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "atributy tokenu saml")</span><span class="sxs-lookup"><span data-stu-id="b0daa-164">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml token attributes")</span></span>
    
    | <span data-ttu-id="b0daa-165">Název atributu</span><span class="sxs-lookup"><span data-stu-id="b0daa-165">Attribute Name</span></span> | <span data-ttu-id="b0daa-166">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="b0daa-166">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="b0daa-167">FirstName</span><span class="sxs-lookup"><span data-stu-id="b0daa-167">Firstname</span></span> |<span data-ttu-id="b0daa-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="b0daa-168">User.givenname</span></span> |
    | <span data-ttu-id="b0daa-169">Příjmení</span><span class="sxs-lookup"><span data-stu-id="b0daa-169">Lastname</span></span> |<span data-ttu-id="b0daa-170">User.Surname</span><span class="sxs-lookup"><span data-stu-id="b0daa-170">User.surname</span></span> |
    | <span data-ttu-id="b0daa-171">E-mail</span><span class="sxs-lookup"><span data-stu-id="b0daa-171">Email</span></span> |<span data-ttu-id="b0daa-172">User.Mail</span><span class="sxs-lookup"><span data-stu-id="b0daa-172">User.mail</span></span> |
    
    <span data-ttu-id="b0daa-173">a.</span><span class="sxs-lookup"><span data-stu-id="b0daa-173">a.</span></span>  <span data-ttu-id="b0daa-174">Pro každý řádek dat v tabulce hello výše, klikněte na tlačítko **přidat atribut uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-174">For each data row in hello table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="b0daa-175">![atributy tokenu SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "atributy tokenu saml")</span><span class="sxs-lookup"><span data-stu-id="b0daa-175">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml token attributes")</span></span>
    
    <span data-ttu-id="b0daa-176">![atributy tokenu SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "atributy tokenu saml")</span><span class="sxs-lookup"><span data-stu-id="b0daa-176">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml token attributes")</span></span>
    
    <span data-ttu-id="b0daa-177">b.</span><span class="sxs-lookup"><span data-stu-id="b0daa-177">b.</span></span>  <span data-ttu-id="b0daa-178">V hello **název atributu** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="b0daa-178">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="b0daa-179">c.</span><span class="sxs-lookup"><span data-stu-id="b0daa-179">c.</span></span>  <span data-ttu-id="b0daa-180">V hello **hodnota atributu** textovému poli, hodnota atributu vyberte hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="b0daa-180">In hello **Attribute Value** textbox, select hello attribute  value shown for that row.</span></span>
    
    <span data-ttu-id="b0daa-181">d.</span><span class="sxs-lookup"><span data-stu-id="b0daa-181">d.</span></span>  <span data-ttu-id="b0daa-182">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-182">Click **Ok**.</span></span>
    
6. <span data-ttu-id="b0daa-183">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b0daa-183">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="b0daa-185">Na hello **TimeOffManager konfigurace** klikněte na tlačítko **konfigurace TimeOffManager** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b0daa-185">On hello **TimeOffManager Configuration** section, click **Configure TimeOffManager** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b0daa-186">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b0daa-186">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![TimeOffManager konfigurační oddíl](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. <span data-ttu-id="b0daa-188">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="b0daa-188">In a different web browser window, log into your TimeOffManager company site as an administrator.</span></span>

9. <span data-ttu-id="b0daa-189">Přejděte příliš**účet \> možnosti účtu \> nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-189">Go too**Account \> Account Options \> Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="b0daa-190">![Jednotné přihlašování v nastavení](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="b0daa-190">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "Single Sign-On Settings")</span></span>
7. <span data-ttu-id="b0daa-191">V hello **nastavení jednotného přihlašování** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b0daa-191">In hello **Single Sign-On Settings** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="b0daa-192">![Jednotné přihlašování v nastavení](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="b0daa-192">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="b0daa-193">a.</span><span class="sxs-lookup"><span data-stu-id="b0daa-193">a.</span></span> <span data-ttu-id="b0daa-194">Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku hello kopírování obsahu ho do schránky a potom vložte hello celý certifikát do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b0daa-194">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **X.509 Certificate** textbox.</span></span>
   
   <span data-ttu-id="b0daa-195">b.</span><span class="sxs-lookup"><span data-stu-id="b0daa-195">b.</span></span> <span data-ttu-id="b0daa-196">V **Idp vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-196">In **Idp Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="b0daa-197">c.</span><span class="sxs-lookup"><span data-stu-id="b0daa-197">c.</span></span> <span data-ttu-id="b0daa-198">V **adresu URL koncového bodu IdP** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-198">In **IdP Endpoint URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="b0daa-199">d.</span><span class="sxs-lookup"><span data-stu-id="b0daa-199">d.</span></span> <span data-ttu-id="b0daa-200">Jako **vynutit SAML**, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-200">As **Enforce SAML**, select **No**.</span></span>
   
   <span data-ttu-id="b0daa-201">e.</span><span class="sxs-lookup"><span data-stu-id="b0daa-201">e.</span></span> <span data-ttu-id="b0daa-202">Jako **automaticky vytvářet uživatelé**, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-202">As **Auto-Create Users**, select **Yes**.</span></span>
   
   <span data-ttu-id="b0daa-203">f.</span><span class="sxs-lookup"><span data-stu-id="b0daa-203">f.</span></span> <span data-ttu-id="b0daa-204">V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-204">In **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="b0daa-205">g.</span><span class="sxs-lookup"><span data-stu-id="b0daa-205">g.</span></span> <span data-ttu-id="b0daa-206">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-206">click **Save Changes**.</span></span>

11. <span data-ttu-id="b0daa-207">V **jednotného přihlašování nastavení** stránky, hodnota hello kopie **adresa URL služby příjemce Assertion** a vložte ji do hello **adresa URL odpovědi** textového pole pod **TimeOffManager Domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b0daa-207">In **Single Sign on settings** page, copy hello value of **Assertion Consumer Service URL** and paste it in hello **Reply URL** text box under **TimeOffManager Domain and URLs** section in Azure portal.</span></span> 

      <span data-ttu-id="b0daa-208">![Jednotné přihlašování v nastavení](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="b0daa-208">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "Single Sign-On Settings")</span></span>

> [!TIP]
> <span data-ttu-id="b0daa-209">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b0daa-209">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b0daa-210">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b0daa-210">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b0daa-211">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b0daa-211">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b0daa-212">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0daa-212">Create an Azure AD test user</span></span>
<span data-ttu-id="b0daa-213">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b0daa-213">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b0daa-215">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b0daa-215">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0daa-216">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b0daa-216">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b0daa-218">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-218">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Uživatelé a skupiny--> všechny uživatele](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b0daa-220">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b0daa-220">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b0daa-222">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b0daa-222">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno stránka uživatel](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b0daa-224">a.</span><span class="sxs-lookup"><span data-stu-id="b0daa-224">a.</span></span> <span data-ttu-id="b0daa-225">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-225">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b0daa-226">b.</span><span class="sxs-lookup"><span data-stu-id="b0daa-226">b.</span></span> <span data-ttu-id="b0daa-227">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b0daa-227">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b0daa-228">c.</span><span class="sxs-lookup"><span data-stu-id="b0daa-228">c.</span></span> <span data-ttu-id="b0daa-229">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-229">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b0daa-230">d.</span><span class="sxs-lookup"><span data-stu-id="b0daa-230">d.</span></span> <span data-ttu-id="b0daa-231">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-231">Click **Create**.</span></span>
 
### <a name="create-a-timeoffmanager-test-user"></a><span data-ttu-id="b0daa-232">Vytvoření zkušebního uživatele TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="b0daa-232">Create a TimeOffManager test user</span></span>

<span data-ttu-id="b0daa-233">V pořadí tooenable Azure AD Uživatelé toolog do TimeOffManager musí být zřízená tooTimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="b0daa-233">In order tooenable Azure AD users toolog into TimeOffManager, they must be provisioned tooTimeOffManager.</span></span>  

<span data-ttu-id="b0daa-234">TimeOffManager podporuje jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="b0daa-234">TimeOffManager supports just in time user provisioning.</span></span> <span data-ttu-id="b0daa-235">Neexistuje žádná položka akce za vás.</span><span class="sxs-lookup"><span data-stu-id="b0daa-235">There is no action item for you.</span></span>  

<span data-ttu-id="b0daa-236">Hello uživatelé se přidávají automaticky během hello první přihlášení pomocí jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="b0daa-236">hello users are added automatically during hello first login using single sign on.</span></span>

>[!NOTE]
><span data-ttu-id="b0daa-237">Můžete použít všechny ostatní TimeOffManager uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované TimeOffManager tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b0daa-237">You can use any other TimeOffManager user account creation tools or APIs provided by TimeOffManager tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="b0daa-238">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b0daa-238">Assign hello Azure AD test user</span></span>

<span data-ttu-id="b0daa-239">V této části povolíte tak, že udělíte přístup tooTimeOffManager toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b0daa-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTimeOffManager.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b0daa-241">**tooassign Britta Simon tooTimeOffManager, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b0daa-241">**tooassign Britta Simon tooTimeOffManager, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0daa-242">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b0daa-244">V seznamu aplikace hello vyberte **TimeOffManager**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-244">In hello applications list, select **TimeOffManager**.</span></span>

    ![TimeOffManager v seznamu aplikací](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. <span data-ttu-id="b0daa-246">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b0daa-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b0daa-248">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b0daa-248">Click **Add** button.</span></span> <span data-ttu-id="b0daa-249">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b0daa-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b0daa-251">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b0daa-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b0daa-252">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b0daa-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b0daa-253">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b0daa-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b0daa-254">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b0daa-254">Test single sign-on</span></span>

<span data-ttu-id="b0daa-255">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b0daa-255">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b0daa-256">Když kliknete na dlaždici TimeOffManager hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour TimeOffManager aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0daa-256">When you click hello TimeOffManager tile in hello Access Panel, you should get automatically signed-on tooyour TimeOffManager application.</span></span> <span data-ttu-id="b0daa-257">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b0daa-257">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0daa-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b0daa-258">Additional resources</span></span>

* [<span data-ttu-id="b0daa-259">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b0daa-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b0daa-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b0daa-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

