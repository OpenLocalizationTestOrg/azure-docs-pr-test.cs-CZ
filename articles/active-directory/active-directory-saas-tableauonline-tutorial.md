---
title: 'Kurz: Azure Active Directory integrace s Tableau Online | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Tableau Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 590b2674270c340b4750c7b6feeaf4f0df4bf853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a><span data-ttu-id="0d612-103">Kurz: Azure Active Directory integrace s Tableau Online</span><span class="sxs-lookup"><span data-stu-id="0d612-103">Tutorial: Azure Active Directory integration with Tableau Online</span></span>

<span data-ttu-id="0d612-104">V tomto kurzu zjistíte, jak toointegrate Tableau Online se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0d612-104">In this tutorial, you learn how toointegrate Tableau Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0d612-105">Integrace Tableau Online s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0d612-105">Integrating Tableau Online with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0d612-106">Můžete řídit ve službě Azure AD, který má přístup tooTableau Online</span><span class="sxs-lookup"><span data-stu-id="0d612-106">You can control in Azure AD who has access tooTableau Online</span></span>
- <span data-ttu-id="0d612-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTableau Online (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d612-107">You can enable your users tooautomatically get signed-on tooTableau Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0d612-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0d612-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0d612-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0d612-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d612-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0d612-110">Prerequisites</span></span>

<span data-ttu-id="0d612-111">tooconfigure integrace Azure AD s Tableau Online, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="0d612-111">tooconfigure Azure AD integration with Tableau Online, you need hello following items:</span></span>

- <span data-ttu-id="0d612-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d612-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0d612-113">Tableau Online jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0d612-113">A Tableau Online single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0d612-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0d612-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0d612-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0d612-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0d612-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0d612-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0d612-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0d612-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0d612-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0d612-118">Scenario description</span></span>
<span data-ttu-id="0d612-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0d612-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0d612-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0d612-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0d612-121">Přidání Tableau Online z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0d612-121">Adding Tableau Online from hello gallery</span></span>
2. <span data-ttu-id="0d612-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d612-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-online-from-hello-gallery"></a><span data-ttu-id="0d612-123">Přidání Tableau Online z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0d612-123">Adding Tableau Online from hello gallery</span></span>
<span data-ttu-id="0d612-124">tooconfigure hello integrace Tableau Online do služby Azure AD, je nutné tooadd Tableau Online hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0d612-124">tooconfigure hello integration of Tableau Online into Azure AD, you need tooadd Tableau Online from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0d612-125">**tooadd Tableau Online z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0d612-125">**tooadd Tableau Online from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d612-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0d612-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0d612-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0d612-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0d612-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0d612-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0d612-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0d612-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0d612-133">Hello vyhledávacího pole zadejte **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="0d612-133">In hello search box, type **Tableau Online**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. <span data-ttu-id="0d612-135">Na panelu výsledků hello vyberte **Tableau Online**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d612-135">In hello results panel, select **Tableau Online**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0d612-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d612-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0d612-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tableau Online na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0d612-138">In this section, you configure and test Azure AD single sign-on with Tableau Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0d612-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Tableau Online je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d612-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tableau Online is tooa user in Azure AD.</span></span> <span data-ttu-id="0d612-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Tableau Online musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="0d612-140">In other words, a link relationship between an Azure AD user and hello related user in Tableau Online needs toobe established.</span></span>

<span data-ttu-id="0d612-141">V Tableau Online přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="0d612-141">In Tableau Online, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0d612-142">tooconfigure a testu Azure AD jednotné přihlašování s Tableau Online, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="0d612-142">tooconfigure and test Azure AD single sign-on with Tableau Online, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0d612-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="0d612-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0d612-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d612-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0d612-145">**[Vytvoření zkušebního uživatele Tableau Online](#creating-a-tableau-online-test-user)**  -toohave protějšek Britta Simon v Tableau Online, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d612-145">**[Creating a Tableau Online test user](#creating-a-tableau-online-test-user)** - toohave a counterpart of Britta Simon in Tableau Online that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0d612-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0d612-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0d612-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="0d612-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0d612-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d612-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0d612-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="0d612-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tableau Online application.</span></span>

<span data-ttu-id="0d612-150">**tooconfigure Azure AD jednotné přihlašování s Tableau Online, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0d612-150">**tooconfigure Azure AD single sign-on with Tableau Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d612-151">V portálu Azure, na hello hello **Tableau Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0d612-151">In hello Azure portal, on hello **Tableau Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0d612-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0d612-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. <span data-ttu-id="0d612-155">Na hello **Tableau Online domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="0d612-155">On hello **Tableau Online Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    <span data-ttu-id="0d612-157">a.</span><span class="sxs-lookup"><span data-stu-id="0d612-157">a.</span></span> <span data-ttu-id="0d612-158">V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://sso.online.tableau.com`</span><span class="sxs-lookup"><span data-stu-id="0d612-158">In hello **Sign-on URL** textbox, type hello URL: `https://sso.online.tableau.com`</span></span>

    <span data-ttu-id="0d612-159">b.</span><span class="sxs-lookup"><span data-stu-id="0d612-159">b.</span></span> <span data-ttu-id="0d612-160">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://sso.online.tableau.com/public/sp/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="0d612-160">In hello **Identifier** textbox, type hello URL: `https://sso.online.tableau.com/public/sp/<instancename>`</span></span>

4. <span data-ttu-id="0d612-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0d612-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. <span data-ttu-id="0d612-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0d612-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0d612-165">V okně jiný prohlížeč, přihlášení tooyour Tableau Online aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d612-165">In a different browser window, sign-on tooyour Tableau Online application.</span></span> <span data-ttu-id="0d612-166">Přejděte příliš**nastavení** a potom **ověřování**.</span><span class="sxs-lookup"><span data-stu-id="0d612-166">Go too**Settings** and then **Authentication**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. <span data-ttu-id="0d612-168">tooenable SAML, v části **typy ověřování** části.</span><span class="sxs-lookup"><span data-stu-id="0d612-168">tooenable SAML, Under **Authentication Types** section.</span></span> <span data-ttu-id="0d612-169">Zkontrolujte hello **jednotné přihlašování s SAML** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="0d612-169">Check hello **Single sign-on with SAML** checkbox.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. <span data-ttu-id="0d612-171">Posuňte se dolů, dokud **soubor Import metadat do Tableau Online** části.</span><span class="sxs-lookup"><span data-stu-id="0d612-171">Scroll down until **Import metadata file into Tableau Online** section.</span></span>  <span data-ttu-id="0d612-172">Klikněte na tlačítko Procházet a importovat soubor hello metadat, které jste si stáhli z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d612-172">Click Browse and import hello metadata file you have downloaded from Azure AD.</span></span> <span data-ttu-id="0d612-173">Potom klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="0d612-173">Then, click **Apply**.</span></span>
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. <span data-ttu-id="0d612-175">V hello **odpovídat kontrolní výrazy** část, vložte hello odpovídající zprostředkovatele Identity kontrolní název pro **e-mailová adresa**, **křestní jméno**, a **příjmení** .</span><span class="sxs-lookup"><span data-stu-id="0d612-175">In hello **Match assertions** section, insert hello corresponding Identity Provider assertion name for **email address**, **first name**, and **last name**.</span></span> <span data-ttu-id="0d612-176">tooget tyto informace z Azure AD:</span><span class="sxs-lookup"><span data-stu-id="0d612-176">tooget this information from Azure AD:</span></span> 
  
    <span data-ttu-id="0d612-177">a.</span><span class="sxs-lookup"><span data-stu-id="0d612-177">a.</span></span> <span data-ttu-id="0d612-178">V hello portálu Azure, přejděte na hello **Tableau Online** stránky integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d612-178">In hello Azure portal, go on hello **Tableau Online** application integration page.</span></span>
    
    <span data-ttu-id="0d612-179">b.</span><span class="sxs-lookup"><span data-stu-id="0d612-179">b.</span></span> <span data-ttu-id="0d612-180">V části atributy hello vyberte hello **"Zobrazit a upravit všechny ostatní atributy uživatele"** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="0d612-180">In hello attributes section, Select hello **"view and edit all other user attributes"** checkbox.</span></span> 
    
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    <span data-ttu-id="0d612-182">c.</span><span class="sxs-lookup"><span data-stu-id="0d612-182">c.</span></span> <span data-ttu-id="0d612-183">Zkopírujte hodnotu hello oboru názvů pro tyto atributy: givenname, e-mailu a příjmení pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d612-183">Copy hello namespace value for these attributes: givenname, email and surname by using hello following steps:</span></span>

   ![Azure AD jednotné přihlášení](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    <span data-ttu-id="0d612-185">d.</span><span class="sxs-lookup"><span data-stu-id="0d612-185">d.</span></span> <span data-ttu-id="0d612-186">Klikněte na tlačítko **user.givenname** hodnota</span><span class="sxs-lookup"><span data-stu-id="0d612-186">Click **user.givenname** value</span></span> 
    
    <span data-ttu-id="0d612-187">e.</span><span class="sxs-lookup"><span data-stu-id="0d612-187">e.</span></span> <span data-ttu-id="0d612-188">Zkopírujte hodnotu hello z hello **obor názvů** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0d612-188">Copy hello value from hello **namespace** textbox.</span></span>

   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    <span data-ttu-id="0d612-190">f.</span><span class="sxs-lookup"><span data-stu-id="0d612-190">f.</span></span> <span data-ttu-id="0d612-191">toocopy hello namesapce hodnoty pro e-mailu hello a příjmení podle předchozích kroků hello.</span><span class="sxs-lookup"><span data-stu-id="0d612-191">toocopy hello namesapce values for hello email and surname follow hello preceding steps.</span></span>

    <span data-ttu-id="0d612-192">g.</span><span class="sxs-lookup"><span data-stu-id="0d612-192">g.</span></span> <span data-ttu-id="0d612-193">Přepínač toohello Tableau Online aplikace a pak nastavte hello **Tableau Online atributy** části následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0d612-193">Switch toohello Tableau Online application, then set hello **Tableau Online Attributes** section as follows:</span></span>
     * <span data-ttu-id="0d612-194">E-mailu: **e-mailu** nebo **userprincipalname**</span><span class="sxs-lookup"><span data-stu-id="0d612-194">Email: **mail** or **userprincipalname**</span></span>
     * <span data-ttu-id="0d612-195">Křestní jméno: **givenname**</span><span class="sxs-lookup"><span data-stu-id="0d612-195">First name: **givenname**</span></span>
     * <span data-ttu-id="0d612-196">Příjmení: **Přezdívka**</span><span class="sxs-lookup"><span data-stu-id="0d612-196">Last name: **surname**</span></span>
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> <span data-ttu-id="0d612-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="0d612-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0d612-199">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="0d612-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0d612-200">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0d612-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0d612-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d612-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="0d612-202">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="0d612-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0d612-204">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0d612-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d612-205">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0d612-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0d612-207">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0d612-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0d612-209">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="0d612-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0d612-211">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d612-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0d612-213">a.</span><span class="sxs-lookup"><span data-stu-id="0d612-213">a.</span></span> <span data-ttu-id="0d612-214">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0d612-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0d612-215">b.</span><span class="sxs-lookup"><span data-stu-id="0d612-215">b.</span></span> <span data-ttu-id="0d612-216">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0d612-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0d612-217">c.</span><span class="sxs-lookup"><span data-stu-id="0d612-217">c.</span></span> <span data-ttu-id="0d612-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0d612-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0d612-219">d.</span><span class="sxs-lookup"><span data-stu-id="0d612-219">d.</span></span> <span data-ttu-id="0d612-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0d612-220">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-online-test-user"></a><span data-ttu-id="0d612-221">Vytvoření Tableau Online zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="0d612-221">Creating a Tableau Online test user</span></span>

<span data-ttu-id="0d612-222">V této části vytvoříte uživatele volal Britta Simon v Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="0d612-222">In this section, you create a user called Britta Simon in Tableau Online.</span></span>

1. <span data-ttu-id="0d612-223">Na **Tableau Online**, klikněte na tlačítko **nastavení** a potom **ověřování** části.</span><span class="sxs-lookup"><span data-stu-id="0d612-223">On **Tableau Online**, click **Settings** and then **Authentication** section.</span></span> <span data-ttu-id="0d612-224">Posuňte se dolů příliš**vybrat uživatele** části.</span><span class="sxs-lookup"><span data-stu-id="0d612-224">Scroll down too**Select Users** section.</span></span> <span data-ttu-id="0d612-225">Klikněte na tlačítko **přidat uživatele** a potom **zadejte e-mailové adresy**.</span><span class="sxs-lookup"><span data-stu-id="0d612-225">Click **Add Users** and then **Enter Email Addresses**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. <span data-ttu-id="0d612-227">Vyberte **přidat uživatele pro jednotné přihlašování (SSO) ověřování**.</span><span class="sxs-lookup"><span data-stu-id="0d612-227">Select **Add users for single sign-on (SSO) authentication**.</span></span> <span data-ttu-id="0d612-228">V hello **zadejte e-mailové adresy** přidat textové polebritta.simon@contoso.com</span><span class="sxs-lookup"><span data-stu-id="0d612-228">In hello **Enter Email Addresses** textbox add britta.simon@contoso.com</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. <span data-ttu-id="0d612-230">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0d612-230">Click **Create**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0d612-231">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="0d612-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0d612-232">V této části povolíte tak, že udělíte přístup tooTableau Online toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0d612-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTableau Online.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0d612-234">**tooassign Britta Simon tooTableau Online, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0d612-234">**tooassign Britta Simon tooTableau Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d612-235">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0d612-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0d612-237">V seznamu aplikace hello vyberte **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="0d612-237">In hello applications list, select **Tableau Online**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. <span data-ttu-id="0d612-239">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0d612-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0d612-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0d612-241">Click **Add** button.</span></span> <span data-ttu-id="0d612-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d612-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0d612-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="0d612-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0d612-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d612-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0d612-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d612-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0d612-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d612-247">Testing single sign-on</span></span>

<span data-ttu-id="0d612-248">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="0d612-248">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="0d612-249">Po kliknutí na tlačítko hello Tableau Online dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Tableau Online aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d612-249">When you click hello Tableau Online tile in hello Access Panel, you should get automatically signed-on tooyour Tableau Online application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d612-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0d612-250">Additional resources</span></span>

* [<span data-ttu-id="0d612-251">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d612-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0d612-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0d612-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

