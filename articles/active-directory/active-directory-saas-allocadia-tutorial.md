---
title: 'Kurz: Azure Active Directory integrace s Allocadia | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Allocadia."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c415fc55-6dc1-49f2-a8a2-2fc6e3790d65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: jeedes
ms.openlocfilehash: 9a01c232f9dc50e690dd348430899db9c13f1564
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a><span data-ttu-id="2482a-103">Kurz: Azure Active Directory integrace s Allocadia</span><span class="sxs-lookup"><span data-stu-id="2482a-103">Tutorial: Azure Active Directory integration with Allocadia</span></span>

<span data-ttu-id="2482a-104">V tomto kurzu zjistíte, jak toointegrate Allocadia s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2482a-104">In this tutorial, you learn how toointegrate Allocadia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2482a-105">Integrace Allocadia s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2482a-105">Integrating Allocadia with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2482a-106">Můžete řídit ve službě Azure AD, který má přístup tooAllocadia</span><span class="sxs-lookup"><span data-stu-id="2482a-106">You can control in Azure AD who has access tooAllocadia</span></span>
- <span data-ttu-id="2482a-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAllocadia (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2482a-107">You can enable your users tooautomatically get signed-on tooAllocadia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2482a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2482a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2482a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2482a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2482a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2482a-110">Prerequisites</span></span>

<span data-ttu-id="2482a-111">Integrace služby Azure AD s Allocadia tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2482a-111">tooconfigure Azure AD integration with Allocadia, you need hello following items:</span></span>

- <span data-ttu-id="2482a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2482a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2482a-113">Allocadia jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2482a-113">An Allocadia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2482a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2482a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2482a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2482a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2482a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2482a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2482a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2482a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2482a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2482a-118">Scenario description</span></span>
<span data-ttu-id="2482a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2482a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2482a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2482a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2482a-121">Přidání Allocadia z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2482a-121">Adding Allocadia from hello gallery</span></span>
2. <span data-ttu-id="2482a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2482a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-allocadia-from-hello-gallery"></a><span data-ttu-id="2482a-123">Přidání Allocadia z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2482a-123">Adding Allocadia from hello gallery</span></span>
<span data-ttu-id="2482a-124">tooconfigure hello integrace Allocadia do Azure AD, je nutné tooadd Allocadia hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2482a-124">tooconfigure hello integration of Allocadia into Azure AD, you need tooadd Allocadia from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2482a-125">**tooadd Allocadia z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2482a-125">**tooadd Allocadia from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2482a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2482a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2482a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2482a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2482a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2482a-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2482a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2482a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2482a-133">Hello vyhledávacího pole zadejte **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="2482a-133">In hello search box, type **Allocadia**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_search.png)

5. <span data-ttu-id="2482a-135">Na panelu výsledků hello vyberte **Allocadia**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2482a-135">In hello results panel, select **Allocadia**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2482a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2482a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2482a-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Allocadia podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="2482a-138">In this section, you configure and test Azure AD single sign-on with Allocadia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2482a-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Allocadia je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2482a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Allocadia is tooa user in Azure AD.</span></span> <span data-ttu-id="2482a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Allocadia musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2482a-140">In other words, a link relationship between an Azure AD user and hello related user in Allocadia needs toobe established.</span></span>

<span data-ttu-id="2482a-141">V Allocadia, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="2482a-141">In Allocadia, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2482a-142">tooconfigure a testu Azure AD jednotné přihlašování s Allocadia, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2482a-142">tooconfigure and test Azure AD single sign-on with Allocadia, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2482a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2482a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2482a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2482a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2482a-145">**[Vytváření testovacího uživatele Allocadia](#creating-an-allocadia-test-user)**  -toohave protějšek Britta Simon v Allocadia, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2482a-145">**[Creating an Allocadia test user](#creating-an-allocadia-test-user)** - toohave a counterpart of Britta Simon in Allocadia that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2482a-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2482a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2482a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2482a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2482a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2482a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2482a-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Allocadia.</span><span class="sxs-lookup"><span data-stu-id="2482a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Allocadia application.</span></span>

<span data-ttu-id="2482a-150">**tooconfigure Azure AD jednotné přihlašování s Allocadia, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2482a-150">**tooconfigure Azure AD single sign-on with Allocadia, perform hello following steps:**</span></span>

1. <span data-ttu-id="2482a-151">V portálu Azure, na hello hello **Allocadia** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2482a-151">In hello Azure portal, on hello **Allocadia** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2482a-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2482a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_samlbase.png)

3. <span data-ttu-id="2482a-155">Na hello **Allocadia domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2482a-155">On hello **Allocadia Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_url.png)

    <span data-ttu-id="2482a-157">a.</span><span class="sxs-lookup"><span data-stu-id="2482a-157">a.</span></span> <span data-ttu-id="2482a-158">V hello **identifikátor** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="2482a-158">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
       
     <span data-ttu-id="2482a-159">Pro testovací prostředí-`https://na2standby.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="2482a-159">For test environment -  `https://na2standby.allocadia.com`</span></span>
    
     <span data-ttu-id="2482a-160">Pro produkční prostředí-`https://na2.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="2482a-160">For production environment - `https://na2.allocadia.com`</span></span>

    <span data-ttu-id="2482a-161">b.</span><span class="sxs-lookup"><span data-stu-id="2482a-161">b.</span></span> <span data-ttu-id="2482a-162">V hello **adresa URL odpovědi** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="2482a-162">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 
    
     <span data-ttu-id="2482a-163">Pro testovací prostředí-`https://na2standby.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="2482a-163">For test environment - `https://na2standby.allocadia.com/allocadia/saml/SSO`</span></span>
    
     <span data-ttu-id="2482a-164">Pro produkční prostředí-`https://na2.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="2482a-164">For production environment - `https://na2.allocadia.com/allocadia/saml/SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2482a-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="2482a-165">These values are not real.</span></span> <span data-ttu-id="2482a-166">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2482a-166">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="2482a-167">Obraťte se na [tým podpory Allocadia](mailTo:support@allocadia.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2482a-167">Contact [Allocadia support team](mailTo:support@allocadia.com) tooget these values.</span></span>

4. <span data-ttu-id="2482a-168">Aplikace Allocadia očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="2482a-168">Allocadia application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2482a-169">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2482a-169">Configure hello following claims for this application.</span></span> <span data-ttu-id="2482a-170">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="2482a-170">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="2482a-171">Hello následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2482a-171">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_attributes.png)
    
5. <span data-ttu-id="2482a-173">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2482a-173">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="2482a-174">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2482a-174">Attribute Name</span></span> | <span data-ttu-id="2482a-175">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="2482a-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="2482a-176">FirstName</span><span class="sxs-lookup"><span data-stu-id="2482a-176">firstname</span></span> | <span data-ttu-id="2482a-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="2482a-177">user.givenname</span></span> |
    | <span data-ttu-id="2482a-178">Příjmení</span><span class="sxs-lookup"><span data-stu-id="2482a-178">lastname</span></span> | <span data-ttu-id="2482a-179">User.Surname</span><span class="sxs-lookup"><span data-stu-id="2482a-179">user.surname</span></span> |
    | <span data-ttu-id="2482a-180">E-mailu</span><span class="sxs-lookup"><span data-stu-id="2482a-180">email</span></span> | <span data-ttu-id="2482a-181">User.Mail</span><span class="sxs-lookup"><span data-stu-id="2482a-181">user.mail</span></span> |
    
    <span data-ttu-id="2482a-182">a.</span><span class="sxs-lookup"><span data-stu-id="2482a-182">a.</span></span> <span data-ttu-id="2482a-183">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2482a-183">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="2482a-185">b.</span><span class="sxs-lookup"><span data-stu-id="2482a-185">b.</span></span> <span data-ttu-id="2482a-186">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2482a-186">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="2482a-188">c.</span><span class="sxs-lookup"><span data-stu-id="2482a-188">c.</span></span> <span data-ttu-id="2482a-189">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2482a-189">From hello **Value** list, type hello attribute value shown for that row.</span></span>
 
    <span data-ttu-id="2482a-190">d.</span><span class="sxs-lookup"><span data-stu-id="2482a-190">d.</span></span> <span data-ttu-id="2482a-191">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2482a-191">Click **Ok**.</span></span>



6. <span data-ttu-id="2482a-192">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2482a-192">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_certificate.png) 


7. <span data-ttu-id="2482a-194">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2482a-194">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2482a-196">tooconfigure jednotného přihlašování na **Allocadia** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Allocadia](mailTo:support@allocadia.com).</span><span class="sxs-lookup"><span data-stu-id="2482a-196">tooconfigure single sign-on on **Allocadia** side, you need toosend hello downloaded **Metadata XML** too[Allocadia support team](mailTo:support@allocadia.com).</span></span> <span data-ttu-id="2482a-197">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="2482a-197">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="2482a-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="2482a-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2482a-199">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="2482a-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2482a-200">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2482a-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2482a-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2482a-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="2482a-202">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2482a-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2482a-204">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2482a-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2482a-205">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2482a-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2482a-207">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2482a-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2482a-209">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="2482a-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2482a-211">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2482a-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2482a-213">a.</span><span class="sxs-lookup"><span data-stu-id="2482a-213">a.</span></span> <span data-ttu-id="2482a-214">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2482a-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2482a-215">b.</span><span class="sxs-lookup"><span data-stu-id="2482a-215">b.</span></span> <span data-ttu-id="2482a-216">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2482a-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2482a-217">c.</span><span class="sxs-lookup"><span data-stu-id="2482a-217">c.</span></span> <span data-ttu-id="2482a-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2482a-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2482a-219">d.</span><span class="sxs-lookup"><span data-stu-id="2482a-219">d.</span></span> <span data-ttu-id="2482a-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2482a-220">Click **Create**.</span></span>
 
### <a name="creating-an-allocadia-test-user"></a><span data-ttu-id="2482a-221">Vytváření testovacího uživatele Allocadia</span><span class="sxs-lookup"><span data-stu-id="2482a-221">Creating an Allocadia test user</span></span>

<span data-ttu-id="2482a-222">Aplikace podporuje pouze v době zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2482a-222">Application supports Just in time user provisioning.</span></span> <span data-ttu-id="2482a-223">Po ověření uživatelé jsou automaticky vytvořené v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="2482a-223">After authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2482a-224">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2482a-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2482a-225">V této části povolíte tak, že udělíte přístup tooAllocadia toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2482a-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAllocadia.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2482a-227">**tooassign Britta Simon tooAllocadia, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2482a-227">**tooassign Britta Simon tooAllocadia, perform hello following steps:**</span></span>

1. <span data-ttu-id="2482a-228">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2482a-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2482a-230">V seznamu aplikace hello vyberte **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="2482a-230">In hello applications list, select **Allocadia**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_app.png) 

3. <span data-ttu-id="2482a-232">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2482a-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2482a-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2482a-234">Click **Add** button.</span></span> <span data-ttu-id="2482a-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2482a-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2482a-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2482a-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2482a-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2482a-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2482a-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2482a-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2482a-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2482a-240">Testing single sign-on</span></span>

<span data-ttu-id="2482a-241">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2482a-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2482a-242">Když kliknete na dlaždici Allocadia hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Allocadia aplikace.</span><span class="sxs-lookup"><span data-stu-id="2482a-242">When you click hello Allocadia tile in hello Access Panel, you should get automatically signed-on tooyour Allocadia application.</span></span>
<span data-ttu-id="2482a-243">Další informace o na přístupovém panelu najdete v tématu [Úvod toohello přístupového panelu](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="2482a-243">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2482a-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2482a-244">Additional resources</span></span>

* [<span data-ttu-id="2482a-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2482a-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2482a-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2482a-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png

