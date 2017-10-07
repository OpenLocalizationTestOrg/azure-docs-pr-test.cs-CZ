---
title: 'Kurz: Azure Active Directory integrace s CloudPassage | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a CloudPassage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 32fb007b90f071626c9b40fb5afc341dd3c1ae99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a><span data-ttu-id="16a4d-103">Kurz: Azure Active Directory integrace s CloudPassage</span><span class="sxs-lookup"><span data-stu-id="16a4d-103">Tutorial: Azure Active Directory integration with CloudPassage</span></span>

<span data-ttu-id="16a4d-104">V tomto kurzu zjistíte, jak toointegrate CloudPassage s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="16a4d-104">In this tutorial, you learn how toointegrate CloudPassage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="16a4d-105">Integrace CloudPassage s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="16a4d-105">Integrating CloudPassage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="16a4d-106">Můžete řídit ve službě Azure AD, který má přístup tooCloudPassage</span><span class="sxs-lookup"><span data-stu-id="16a4d-106">You can control in Azure AD who has access tooCloudPassage</span></span>
- <span data-ttu-id="16a4d-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCloudPassage (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="16a4d-107">You can enable your users tooautomatically get signed-on tooCloudPassage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="16a4d-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="16a4d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="16a4d-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="16a4d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16a4d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="16a4d-110">Prerequisites</span></span>

<span data-ttu-id="16a4d-111">Integrace služby Azure AD s CloudPassage tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="16a4d-111">tooconfigure Azure AD integration with CloudPassage, you need hello following items:</span></span>

- <span data-ttu-id="16a4d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="16a4d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="16a4d-113">CloudPassage jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="16a4d-113">A CloudPassage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="16a4d-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="16a4d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="16a4d-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="16a4d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="16a4d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="16a4d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="16a4d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="16a4d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="16a4d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="16a4d-118">Scenario description</span></span>
<span data-ttu-id="16a4d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="16a4d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="16a4d-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="16a4d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="16a4d-121">Přidání CloudPassage z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="16a4d-121">Adding CloudPassage from hello gallery</span></span>
2. <span data-ttu-id="16a4d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="16a4d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloudpassage-from-hello-gallery"></a><span data-ttu-id="16a4d-123">Přidání CloudPassage z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="16a4d-123">Adding CloudPassage from hello gallery</span></span>
<span data-ttu-id="16a4d-124">tooconfigure hello integrace CloudPassage do Azure AD, je nutné tooadd CloudPassage hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="16a4d-124">tooconfigure hello integration of CloudPassage into Azure AD, you need tooadd CloudPassage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="16a4d-125">**tooadd CloudPassage z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="16a4d-125">**tooadd CloudPassage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="16a4d-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="16a4d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="16a4d-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="16a4d-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="16a4d-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="16a4d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="16a4d-133">Hello vyhledávacího pole zadejte **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-133">In hello search box, type **CloudPassage**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_search.png)

5. <span data-ttu-id="16a4d-135">Na panelu výsledků hello vyberte **CloudPassage**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="16a4d-135">In hello results panel, select **CloudPassage**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="16a4d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="16a4d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="16a4d-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s CloudPassage podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="16a4d-138">In this section, you configure and test Azure AD single sign-on with CloudPassage based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="16a4d-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v CloudPassage je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="16a4d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CloudPassage is tooa user in Azure AD.</span></span> <span data-ttu-id="16a4d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v CloudPassage musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="16a4d-140">In other words, a link relationship between an Azure AD user and hello related user in CloudPassage needs toobe established.</span></span>

<span data-ttu-id="16a4d-141">V CloudPassage, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="16a4d-141">In CloudPassage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="16a4d-142">tooconfigure a testu Azure AD jednotné přihlašování s CloudPassage, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="16a4d-142">tooconfigure and test Azure AD single sign-on with CloudPassage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="16a4d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="16a4d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="16a4d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="16a4d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="16a4d-145">**[Vytvoření zkušebního uživatele CloudPassage](#creating-a-cloudpassage-test-user)**  -toohave protějšek Britta Simon v CloudPassage, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="16a4d-145">**[Creating a CloudPassage test user](#creating-a-cloudpassage-test-user)** - toohave a counterpart of Britta Simon in CloudPassage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="16a4d-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="16a4d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="16a4d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="16a4d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="16a4d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="16a4d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="16a4d-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci CloudPassage.</span><span class="sxs-lookup"><span data-stu-id="16a4d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CloudPassage application.</span></span>

<span data-ttu-id="16a4d-150">**tooconfigure Azure AD jednotné přihlašování s CloudPassage, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="16a4d-150">**tooconfigure Azure AD single sign-on with CloudPassage, perform hello following steps:**</span></span>

1. <span data-ttu-id="16a4d-151">V portálu Azure, na hello hello **CloudPassage** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-151">In hello Azure portal, on hello **CloudPassage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="16a4d-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="16a4d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

3. <span data-ttu-id="16a4d-155">Na hello **CloudPassage domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="16a4d-155">On hello **CloudPassage Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    <span data-ttu-id="16a4d-157">a.</span><span class="sxs-lookup"><span data-stu-id="16a4d-157">a.</span></span> <span data-ttu-id="16a4d-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://portal.cloudpassage.com/saml/init/accountid`</span><span class="sxs-lookup"><span data-stu-id="16a4d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://portal.cloudpassage.com/saml/init/accountid`</span></span>

    <span data-ttu-id="16a4d-159">b.</span><span class="sxs-lookup"><span data-stu-id="16a4d-159">b.</span></span> <span data-ttu-id="16a4d-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzor: `https://portal.cloudpassage.com/saml/consume/accountid`.</span><span class="sxs-lookup"><span data-stu-id="16a4d-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://portal.cloudpassage.com/saml/consume/accountid`.</span></span> <span data-ttu-id="16a4d-161">Hodnota tohoto atributu můžete získat kliknutím **nastavení jednotného přihlašování k dokumentaci** v hello **nastavení jednotného přihlašování** části portálu CloudPassage.</span><span class="sxs-lookup"><span data-stu-id="16a4d-161">You can get your value for this attribute by clicking **SSO Setup documentation** in hello **Single Sign-on Settings** section of your CloudPassage portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > <span data-ttu-id="16a4d-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="16a4d-163">These values are not real.</span></span> <span data-ttu-id="16a4d-164">Tyto hodnoty aktualizujte hello skutečná adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="16a4d-164">Update these values with hello actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="16a4d-165">Obraťte se na [tým podpory CloudPassage klienta](https://www.cloudpassage.com/company/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="16a4d-165">Contact [CloudPassage Client support team](https://www.cloudpassage.com/company/contact/) tooget these values.</span></span> 

4. <span data-ttu-id="16a4d-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="16a4d-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

5. <span data-ttu-id="16a4d-168">Aplikace CloudPassage očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="16a4d-168">Your CloudPassage application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span>   
<span data-ttu-id="16a4d-169">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="16a4d-169">hello following screenshot shows an example for this.</span></span>
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

6. <span data-ttu-id="16a4d-171">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="16a4d-171">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>

    | <span data-ttu-id="16a4d-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="16a4d-172">Attribute Name</span></span> | <span data-ttu-id="16a4d-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="16a4d-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="16a4d-174">FirstName</span><span class="sxs-lookup"><span data-stu-id="16a4d-174">firstname</span></span> |<span data-ttu-id="16a4d-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="16a4d-175">user.givenname</span></span> |
    | <span data-ttu-id="16a4d-176">Příjmení</span><span class="sxs-lookup"><span data-stu-id="16a4d-176">lastname</span></span> |<span data-ttu-id="16a4d-177">User.Surname</span><span class="sxs-lookup"><span data-stu-id="16a4d-177">user.surname</span></span> |
    | <span data-ttu-id="16a4d-178">E-mailu</span><span class="sxs-lookup"><span data-stu-id="16a4d-178">email</span></span> |<span data-ttu-id="16a4d-179">User.Mail</span><span class="sxs-lookup"><span data-stu-id="16a4d-179">user.mail</span></span> |
    
    <span data-ttu-id="16a4d-180">a.</span><span class="sxs-lookup"><span data-stu-id="16a4d-180">a.</span></span> <span data-ttu-id="16a4d-181">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16a4d-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="16a4d-184">b.</span><span class="sxs-lookup"><span data-stu-id="16a4d-184">b.</span></span> <span data-ttu-id="16a4d-185">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="16a4d-185">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="16a4d-186">c.</span><span class="sxs-lookup"><span data-stu-id="16a4d-186">c.</span></span> <span data-ttu-id="16a4d-187">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="16a4d-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="16a4d-188">d.</span><span class="sxs-lookup"><span data-stu-id="16a4d-188">d.</span></span> <span data-ttu-id="16a4d-189">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-189">Click **Ok**.</span></span>

7. <span data-ttu-id="16a4d-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="16a4d-190">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="16a4d-192">Na hello **CloudPassage konfigurace** klikněte na tlačítko **konfigurace CloudPassage** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="16a4d-192">On hello **CloudPassage Configuration** section, click **Configure CloudPassage** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="16a4d-193">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="16a4d-193">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

9. <span data-ttu-id="16a4d-195">V okně jiný prohlížeč, lokality společnosti CloudPassage tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="16a4d-195">In a different browser window, sign-on tooyour CloudPassage company site as administrator.</span></span>

10. <span data-ttu-id="16a4d-196">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení**a potom klikněte na **Správa webu**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-196">In hello menu on hello top, click **Settings**, and then click **Site Administration**.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][12]

11. <span data-ttu-id="16a4d-198">Klikněte na tlačítko hello **nastavení ověřování** kartě.</span><span class="sxs-lookup"><span data-stu-id="16a4d-198">Click hello **Authentication Settings** tab.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][13]

12. <span data-ttu-id="16a4d-200">V hello **nastavení jednotného přihlašování** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="16a4d-200">In hello **Single Sign-on Settings** section, perform hello following steps:</span></span> 
   
    ![Konfigurovat jednotné přihlašování][14]

    <span data-ttu-id="16a4d-202">a.</span><span class="sxs-lookup"><span data-stu-id="16a4d-202">a.</span></span> <span data-ttu-id="16a4d-203">Vyberte **povolit jeden sign-on(SSO) (dokumentace instalace jednotné přihlašování)** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="16a4d-203">Select **Enable Single sign-on(SSO)(SSO Setup Documentation)** checkbox.</span></span>
    
    <span data-ttu-id="16a4d-204">b.</span><span class="sxs-lookup"><span data-stu-id="16a4d-204">b.</span></span> <span data-ttu-id="16a4d-205">Vložení **SAML Entity ID** do hello **URL vystavitele SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="16a4d-205">Paste **SAML Entity ID** into hello **SAML issuer URL** textbox.</span></span>
  
    <span data-ttu-id="16a4d-206">c.</span><span class="sxs-lookup"><span data-stu-id="16a4d-206">c.</span></span> <span data-ttu-id="16a4d-207">Vložení **SAML jeden přihlašování adresa URL služby** do hello **adresu URL koncového bodu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="16a4d-207">Paste **SAML Single Sign-On Service URL** into hello **SAML endpoint URL** textbox.</span></span>
  
    <span data-ttu-id="16a4d-208">d.</span><span class="sxs-lookup"><span data-stu-id="16a4d-208">d.</span></span> <span data-ttu-id="16a4d-209">Vložení **Sign-Out URL** do hello **odhlášení cílová stránka** textové pole.</span><span class="sxs-lookup"><span data-stu-id="16a4d-209">Paste **Sign-Out URL** into hello **Logout landing page** textbox.</span></span>
  
    <span data-ttu-id="16a4d-210">e.</span><span class="sxs-lookup"><span data-stu-id="16a4d-210">e.</span></span> <span data-ttu-id="16a4d-211">Otevřete stažený certifikát v poznámkovém bloku hello kopírovat obsah stažený certifikát do schránky a pak ji vložit do hello **x 509 certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="16a4d-211">Open your downloaded certificate in notepad, copy hello content of downloaded certificate into your clipboard, and then paste it into hello **x 509 certificate** textbox.</span></span>
  
    <span data-ttu-id="16a4d-212">f.</span><span class="sxs-lookup"><span data-stu-id="16a4d-212">f.</span></span> <span data-ttu-id="16a4d-213">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="16a4d-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="16a4d-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="16a4d-215">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="16a4d-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="16a4d-216">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="16a4d-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="16a4d-217">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="16a4d-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="16a4d-218">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="16a4d-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="16a4d-220">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="16a4d-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="16a4d-221">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="16a4d-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="16a4d-223">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="16a4d-225">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="16a4d-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="16a4d-227">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="16a4d-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="16a4d-229">a.</span><span class="sxs-lookup"><span data-stu-id="16a4d-229">a.</span></span> <span data-ttu-id="16a4d-230">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="16a4d-231">b.</span><span class="sxs-lookup"><span data-stu-id="16a4d-231">b.</span></span> <span data-ttu-id="16a4d-232">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="16a4d-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="16a4d-233">c.</span><span class="sxs-lookup"><span data-stu-id="16a4d-233">c.</span></span> <span data-ttu-id="16a4d-234">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="16a4d-235">d.</span><span class="sxs-lookup"><span data-stu-id="16a4d-235">d.</span></span> <span data-ttu-id="16a4d-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-236">Click **Create**.</span></span>
 
### <a name="creating-a-cloudpassage-test-user"></a><span data-ttu-id="16a4d-237">Vytvoření zkušebního uživatele CloudPassage</span><span class="sxs-lookup"><span data-stu-id="16a4d-237">Creating a CloudPassage test user</span></span>

<span data-ttu-id="16a4d-238">Hello cílem této části je toocreate volal Britta Simon v CloudPassage uživatele.</span><span class="sxs-lookup"><span data-stu-id="16a4d-238">hello objective of this section is toocreate a user called Britta Simon in CloudPassage.</span></span>

<span data-ttu-id="16a4d-239">**toocreate uživatel volal Britta Simon v CloudPassage, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="16a4d-239">**toocreate a user called Britta Simon in CloudPassage, perform hello following steps:**</span></span>

1. <span data-ttu-id="16a4d-240">Přihlášení tooyour **CloudPassage** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="16a4d-240">Sign-on tooyour **CloudPassage** company site as an administrator.</span></span> 

2. <span data-ttu-id="16a4d-241">V panelu nástrojů hello hello nahoře, klikněte na **nastavení**a potom klikněte na **Správa webu**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-241">In hello toolbar on hello top, click **Settings**, and then click **Site Administration**.</span></span> 
   
   ![Vytvoření zkušebního uživatele CloudPassage][22] 

3. <span data-ttu-id="16a4d-243">Klikněte na tlačítko hello **uživatelé** a pak klikněte **přidat nové uživatele**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-243">Click hello **Users** tab, and then click **Add New User**.</span></span> 
   
   ![Vytvoření zkušebního uživatele CloudPassage][23]

4. <span data-ttu-id="16a4d-245">V hello **přidat nové uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="16a4d-245">In hello **Add New User** section, perform hello following steps:</span></span> 
   
   ![Vytvoření zkušebního uživatele CloudPassage][24]
    
    <span data-ttu-id="16a4d-247">a.</span><span class="sxs-lookup"><span data-stu-id="16a4d-247">a.</span></span> <span data-ttu-id="16a4d-248">V hello **křestní jméno** textovému poli, zadejte Britta.</span><span class="sxs-lookup"><span data-stu-id="16a4d-248">In hello **First Name** textbox, type Britta.</span></span> 
  
    <span data-ttu-id="16a4d-249">b.</span><span class="sxs-lookup"><span data-stu-id="16a4d-249">b.</span></span> <span data-ttu-id="16a4d-250">V hello **příjmení** textovému poli, zadejte Simon.</span><span class="sxs-lookup"><span data-stu-id="16a4d-250">In hello **Last Name** textbox, type Simon.</span></span>
  
    <span data-ttu-id="16a4d-251">c.</span><span class="sxs-lookup"><span data-stu-id="16a4d-251">c.</span></span> <span data-ttu-id="16a4d-252">V hello **uživatelské jméno** textovému poli, hello **e-mailu** textové pole a hello **znovu zadejte e-mailu** textovému poli, zadejte uživatelské jméno je Britta ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="16a4d-252">In hello **Username** textbox, hello **Email** textbox and hello **Retype Email** textbox, type Britta's user name in Azure AD.</span></span>
  
    <span data-ttu-id="16a4d-253">d.</span><span class="sxs-lookup"><span data-stu-id="16a4d-253">d.</span></span> <span data-ttu-id="16a4d-254">Jako **typ přístupu**, vyberte **povolit přístup z portálu bylo**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-254">As **Access Type**, select **Enable Halo Portal Access**.</span></span>
  
    <span data-ttu-id="16a4d-255">e.</span><span class="sxs-lookup"><span data-stu-id="16a4d-255">e.</span></span> <span data-ttu-id="16a4d-256">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-256">Click **Add**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="16a4d-257">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="16a4d-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="16a4d-258">V této části povolíte tak, že udělíte přístup tooCloudPassage toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="16a4d-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCloudPassage.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="16a4d-260">**tooassign Britta Simon tooCloudPassage, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="16a4d-260">**tooassign Britta Simon tooCloudPassage, perform hello following steps:**</span></span>

1. <span data-ttu-id="16a4d-261">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="16a4d-263">V seznamu aplikace hello vyberte **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-263">In hello applications list, select **CloudPassage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

3. <span data-ttu-id="16a4d-265">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="16a4d-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="16a4d-267">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="16a4d-267">Click **Add** button.</span></span> <span data-ttu-id="16a4d-268">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16a4d-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="16a4d-270">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="16a4d-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="16a4d-271">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16a4d-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="16a4d-272">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16a4d-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="16a4d-273">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="16a4d-273">Testing single sign-on</span></span>

<span data-ttu-id="16a4d-274">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="16a4d-274">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="16a4d-275">Když kliknete na dlaždici CloudPassage hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour CloudPassage aplikace.</span><span class="sxs-lookup"><span data-stu-id="16a4d-275">When you click hello CloudPassage tile in hello Access Panel, you should get automatically signed-on tooyour CloudPassage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16a4d-276">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="16a4d-276">Additional resources</span></span>

* [<span data-ttu-id="16a4d-277">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16a4d-277">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="16a4d-278">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="16a4d-278">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_203.png

