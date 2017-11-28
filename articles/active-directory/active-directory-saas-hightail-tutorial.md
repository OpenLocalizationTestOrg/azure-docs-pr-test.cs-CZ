---
title: 'Kurz: Azure Active Directory integrace s Hightail | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Hightail."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2b36fcf8d5773255fdf89de2dccdceb95c032bd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a><span data-ttu-id="3df6d-103">Kurz: Azure Active Directory integrace s Hightail</span><span class="sxs-lookup"><span data-stu-id="3df6d-103">Tutorial: Azure Active Directory integration with Hightail</span></span>

<span data-ttu-id="3df6d-104">V tomto kurzu zjistíte, jak toointegrate Hightail s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3df6d-104">In this tutorial, you learn how toointegrate Hightail with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3df6d-105">Integrace Hightail s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3df6d-105">Integrating Hightail with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3df6d-106">Můžete řídit ve službě Azure AD, který má přístup tooHightail</span><span class="sxs-lookup"><span data-stu-id="3df6d-106">You can control in Azure AD who has access tooHightail</span></span>
- <span data-ttu-id="3df6d-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHightail (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3df6d-107">You can enable your users tooautomatically get signed-on tooHightail (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3df6d-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3df6d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3df6d-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3df6d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3df6d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3df6d-110">Prerequisites</span></span>

<span data-ttu-id="3df6d-111">Integrace služby Azure AD s Hightail tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3df6d-111">tooconfigure Azure AD integration with Hightail, you need hello following items:</span></span>

- <span data-ttu-id="3df6d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3df6d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3df6d-113">Hightail jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3df6d-113">A Hightail single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3df6d-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3df6d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3df6d-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3df6d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3df6d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3df6d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3df6d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3df6d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3df6d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3df6d-118">Scenario description</span></span>
<span data-ttu-id="3df6d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3df6d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3df6d-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3df6d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3df6d-121">Přidání Hightail z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3df6d-121">Adding Hightail from hello gallery</span></span>
2. <span data-ttu-id="3df6d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3df6d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hightail-from-hello-gallery"></a><span data-ttu-id="3df6d-123">Přidání Hightail z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3df6d-123">Adding Hightail from hello gallery</span></span>
<span data-ttu-id="3df6d-124">tooconfigure hello integrace Hightail do Azure AD, je nutné tooadd Hightail hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3df6d-124">tooconfigure hello integration of Hightail into Azure AD, you need tooadd Hightail from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3df6d-125">**tooadd Hightail z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3df6d-125">**tooadd Hightail from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3df6d-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3df6d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3df6d-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3df6d-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3df6d-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3df6d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3df6d-133">Hello vyhledávacího pole zadejte **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-133">In hello search box, type **Hightail**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. <span data-ttu-id="3df6d-135">Na panelu výsledků hello vyberte **Hightail**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3df6d-135">In hello results panel, select **Hightail**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3df6d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3df6d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3df6d-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Hightail podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3df6d-138">In this section, you configure and test Azure AD single sign-on with Hightail based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3df6d-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Hightail je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3df6d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Hightail is tooa user in Azure AD.</span></span> <span data-ttu-id="3df6d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Hightail musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="3df6d-140">In other words, a link relationship between an Azure AD user and hello related user in Hightail needs toobe established.</span></span>

<span data-ttu-id="3df6d-141">V Hightail, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="3df6d-141">In Hightail, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3df6d-142">tooconfigure a testu Azure AD jednotné přihlašování s Hightail, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="3df6d-142">tooconfigure and test Azure AD single sign-on with Hightail, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3df6d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="3df6d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3df6d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3df6d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3df6d-145">**[Vytvoření zkušebního uživatele Hightail](#creating-a-hightail-test-user)**  -toohave protějšek Britta Simon v Hightail, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="3df6d-145">**[Creating a Hightail test user](#creating-a-hightail-test-user)** - toohave a counterpart of Britta Simon in Hightail that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3df6d-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3df6d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3df6d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="3df6d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3df6d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3df6d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3df6d-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Hightail.</span><span class="sxs-lookup"><span data-stu-id="3df6d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Hightail application.</span></span>

<span data-ttu-id="3df6d-150">**tooconfigure Azure AD jednotné přihlašování s Hightail, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3df6d-150">**tooconfigure Azure AD single sign-on with Hightail, perform hello following steps:**</span></span>

1. <span data-ttu-id="3df6d-151">V portálu Azure, na hello hello **Hightail** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-151">In hello Azure portal, on hello **Hightail** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3df6d-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3df6d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. <span data-ttu-id="3df6d-155">Na hello **Hightail domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="3df6d-155">On hello **Hightail Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     <span data-ttu-id="3df6d-157">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL hello jako:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span><span class="sxs-lookup"><span data-stu-id="3df6d-157">In hello **Reply URL** textbox, type hello URL as: `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3df6d-158">Hello předchozí hodnota není skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3df6d-158">hello preceding value is not real value.</span></span> <span data-ttu-id="3df6d-159">Aktualizujte hodnotu hello s hello skutečná adresa URL odpovědi, který je vysvětlen později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="3df6d-159">You will update hello value with hello actual Reply URL, which is explained later in hello tutorial.</span></span>
 
4. <span data-ttu-id="3df6d-160">Na hello **Hightail domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="3df6d-160">On hello **Hightail Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    <span data-ttu-id="3df6d-162">a.</span><span class="sxs-lookup"><span data-stu-id="3df6d-162">a.</span></span> <span data-ttu-id="3df6d-163">Klikněte na tlačítko hello **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-163">Click hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="3df6d-164">b.</span><span class="sxs-lookup"><span data-stu-id="3df6d-164">b.</span></span> <span data-ttu-id="3df6d-165">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL hello jako:`https://www.hightail.com/loginSSO`</span><span class="sxs-lookup"><span data-stu-id="3df6d-165">In hello **Sign On URL** textbox, type hello URL as: `https://www.hightail.com/loginSSO`</span></span>

4. <span data-ttu-id="3df6d-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3df6d-166">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. <span data-ttu-id="3df6d-168">Hightail aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="3df6d-168">Hightail application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="3df6d-169">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3df6d-169">Please configure hello following claims for this application.</span></span> <span data-ttu-id="3df6d-170">Můžete spravovat hello hodnoty těchto atributů z hello **"Atrribute"** karta aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3df6d-170">You can manage hello values of these attributes from hello **"Atrribute"** tab of hello application.</span></span> <span data-ttu-id="3df6d-171">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="3df6d-171">hello following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. <span data-ttu-id="3df6d-173">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3df6d-173">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="3df6d-174">Název atributu</span><span class="sxs-lookup"><span data-stu-id="3df6d-174">Attribute Name</span></span> | <span data-ttu-id="3df6d-175">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="3df6d-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="3df6d-176">FirstName</span><span class="sxs-lookup"><span data-stu-id="3df6d-176">FirstName</span></span> | <span data-ttu-id="3df6d-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="3df6d-177">user.givenname</span></span> |
    | <span data-ttu-id="3df6d-178">Příjmení</span><span class="sxs-lookup"><span data-stu-id="3df6d-178">LastName</span></span> | <span data-ttu-id="3df6d-179">User.Surname</span><span class="sxs-lookup"><span data-stu-id="3df6d-179">user.surname</span></span> |
    | <span data-ttu-id="3df6d-180">E-mail</span><span class="sxs-lookup"><span data-stu-id="3df6d-180">Email</span></span> | <span data-ttu-id="3df6d-181">User.Mail</span><span class="sxs-lookup"><span data-stu-id="3df6d-181">user.mail</span></span> |    
    | <span data-ttu-id="3df6d-182">Identity uživatele</span><span class="sxs-lookup"><span data-stu-id="3df6d-182">UserIdentity</span></span> | <span data-ttu-id="3df6d-183">User.Mail</span><span class="sxs-lookup"><span data-stu-id="3df6d-183">user.mail</span></span> |
    
    <span data-ttu-id="3df6d-184">a.</span><span class="sxs-lookup"><span data-stu-id="3df6d-184">a.</span></span> <span data-ttu-id="3df6d-185">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3df6d-185">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="3df6d-188">b.</span><span class="sxs-lookup"><span data-stu-id="3df6d-188">b.</span></span> <span data-ttu-id="3df6d-189">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="3df6d-189">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="3df6d-190">c.</span><span class="sxs-lookup"><span data-stu-id="3df6d-190">c.</span></span> <span data-ttu-id="3df6d-191">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="3df6d-191">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="3df6d-192">d.</span><span class="sxs-lookup"><span data-stu-id="3df6d-192">d.</span></span> <span data-ttu-id="3df6d-193">Nechte hello **Namespace** prázdné.</span><span class="sxs-lookup"><span data-stu-id="3df6d-193">Leave hello **Namespace** blank.</span></span>
    
    <span data-ttu-id="3df6d-194">e.</span><span class="sxs-lookup"><span data-stu-id="3df6d-194">e.</span></span> <span data-ttu-id="3df6d-195">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-195">Click **Ok**.</span></span>

7. <span data-ttu-id="3df6d-196">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3df6d-196">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="3df6d-198">Na hello **Hightail konfigurace** klikněte na tlačítko **konfigurace Hightail** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3df6d-198">On hello **Hightail Configuration** section, click **Configure Hightail** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3df6d-199">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3df6d-199">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    ><span data-ttu-id="3df6d-201">Před konfigurací hello jednotné přihlašování v aplikaci Hightail, prosím seznamu povolených vaše e-mailovou doménu s Hightail team tak, aby všechny hello uživatelé, kteří používají tuto doménu můžete použít funkci jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3df6d-201">Before configuring hello Single Sign On at Hightail app, please white list your email domain with Hightail team so that all hello users who are using this domain can use Single Sign On functionality.</span></span>


9. <span data-ttu-id="3df6d-202">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte klienta Hightail toosign na tooyour jako správce.</span><span class="sxs-lookup"><span data-stu-id="3df6d-202">tooget SSO configured for your application, you need toosign-on tooyour Hightail tenant as an administrator.</span></span>
   
    <span data-ttu-id="3df6d-203">a.</span><span class="sxs-lookup"><span data-stu-id="3df6d-203">a.</span></span> <span data-ttu-id="3df6d-204">V nabídce hello hello nahoře, klikněte na tlačítko hello **účet** a vyberte **konfigurace SAML**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-204">In hello menu on hello top, click hello **Account** tab and select **Configure SAML**.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    <span data-ttu-id="3df6d-206">b.</span><span class="sxs-lookup"><span data-stu-id="3df6d-206">b.</span></span> <span data-ttu-id="3df6d-207">Zaškrtněte políčko hello z **povolit ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-207">Select hello checkbox of **Enable SAML Authentication**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    <span data-ttu-id="3df6d-209">c.</span><span class="sxs-lookup"><span data-stu-id="3df6d-209">c.</span></span> <span data-ttu-id="3df6d-210">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **SAML podpisový certifikát tokenů** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3df6d-210">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **SAML Token Signing Certificate** textbox.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    <span data-ttu-id="3df6d-212">d.</span><span class="sxs-lookup"><span data-stu-id="3df6d-212">d.</span></span> <span data-ttu-id="3df6d-213">V hello **autority SAML (zprostředkovatele Identity)** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** zkopírovaných z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3df6d-213">In hello **SAML Authority (Identity Provider)** textbox, paste hello value of **SAML Single Sign-On Service URL** copied from Azure portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    <span data-ttu-id="3df6d-215">e.</span><span class="sxs-lookup"><span data-stu-id="3df6d-215">e.</span></span> <span data-ttu-id="3df6d-216">Pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu** vyberte **"Zprostředkovatele Identity (IdP) iniciované přihlášení"**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-216">If you wish tooconfigure hello application in **IDP initiated mode** select **"Identity Provider (IdP) initiated log in"**.</span></span> <span data-ttu-id="3df6d-217">Pokud **SP iniciované režimu** vyberte **"Přihlášení iniciované poskytovatelem služeb (SP)"**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-217">If **SP initiated mode** select **"Service Provider (SP) initiated log in"**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    <span data-ttu-id="3df6d-219">f.</span><span class="sxs-lookup"><span data-stu-id="3df6d-219">f.</span></span> <span data-ttu-id="3df6d-220">Hello SAML příjemce adresu URL instance, zkopírujte a vložte jej do **adresa URL odpovědi** textového pole v **Hightail domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3df6d-220">Copy hello SAML consumer URL for your instance and paste it in **Reply URL** textbox in **Hightail Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="3df6d-221">g.</span><span class="sxs-lookup"><span data-stu-id="3df6d-221">g.</span></span> <span data-ttu-id="3df6d-222">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-222">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3df6d-223">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="3df6d-223">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3df6d-224">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="3df6d-224">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3df6d-225">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3df6d-225">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3df6d-226">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3df6d-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="3df6d-227">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="3df6d-227">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3df6d-229">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3df6d-229">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3df6d-230">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3df6d-230">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3df6d-232">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-232">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3df6d-234">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="3df6d-234">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3df6d-236">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3df6d-236">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3df6d-238">a.</span><span class="sxs-lookup"><span data-stu-id="3df6d-238">a.</span></span> <span data-ttu-id="3df6d-239">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-239">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3df6d-240">b.</span><span class="sxs-lookup"><span data-stu-id="3df6d-240">b.</span></span> <span data-ttu-id="3df6d-241">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3df6d-241">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3df6d-242">c.</span><span class="sxs-lookup"><span data-stu-id="3df6d-242">c.</span></span> <span data-ttu-id="3df6d-243">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-243">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3df6d-244">d.</span><span class="sxs-lookup"><span data-stu-id="3df6d-244">d.</span></span> <span data-ttu-id="3df6d-245">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-245">Click **Create**.</span></span>
 
### <a name="creating-a-hightail-test-user"></a><span data-ttu-id="3df6d-246">Vytvoření zkušebního uživatele Hightail</span><span class="sxs-lookup"><span data-stu-id="3df6d-246">Creating a Hightail test user</span></span>

<span data-ttu-id="3df6d-247">Hello cílem této části je toocreate volal Britta Simon v Hightail uživatele.</span><span class="sxs-lookup"><span data-stu-id="3df6d-247">hello objective of this section is toocreate a user called Britta Simon in Hightail.</span></span> 

<span data-ttu-id="3df6d-248">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="3df6d-248">There is no action item for you in this section.</span></span> <span data-ttu-id="3df6d-249">Podporuje zřizování uživatelů za běhu v závislosti na vlastní deklarace hello hightail.</span><span class="sxs-lookup"><span data-stu-id="3df6d-249">Hightail supports just-in-time user provisioning based on hello custom claims.</span></span> <span data-ttu-id="3df6d-250">Pokud jste nakonfigurovali vlastní deklarace hello hello část  **[konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  vyšší, se automaticky vytvoří uživatele v aplikaci hello ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="3df6d-250">If you have configured hello custom claims as shown in hello section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** above, a user is automatically created in hello application it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="3df6d-251">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory Hightail](mailto:support@hightail.com).</span><span class="sxs-lookup"><span data-stu-id="3df6d-251">If you need toocreate a user manually, you need toocontact hello [Hightail support team](mailto:support@hightail.com).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3df6d-252">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="3df6d-252">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3df6d-253">V této části povolíte tak, že udělíte přístup tooHightail toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3df6d-253">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHightail.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3df6d-255">**tooassign Britta Simon tooHightail, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3df6d-255">**tooassign Britta Simon tooHightail, perform hello following steps:**</span></span>

1. <span data-ttu-id="3df6d-256">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-256">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3df6d-258">V seznamu aplikace hello vyberte **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-258">In hello applications list, select **Hightail**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. <span data-ttu-id="3df6d-260">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3df6d-260">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3df6d-262">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3df6d-262">Click **Add** button.</span></span> <span data-ttu-id="3df6d-263">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3df6d-263">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3df6d-265">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="3df6d-265">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3df6d-266">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3df6d-266">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3df6d-267">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3df6d-267">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3df6d-268">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3df6d-268">Testing single sign-on</span></span>

<span data-ttu-id="3df6d-269">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3df6d-269">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3df6d-270">Když kliknete na dlaždici Hightail hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Hightail aplikace.</span><span class="sxs-lookup"><span data-stu-id="3df6d-270">When you click hello Hightail tile in hello Access Panel, you should get automatically signed-on tooyour Hightail application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="3df6d-271">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3df6d-271">Additional resources</span></span>

* [<span data-ttu-id="3df6d-272">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3df6d-272">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3df6d-273">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3df6d-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

