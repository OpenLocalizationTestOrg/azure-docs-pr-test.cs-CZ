---
title: 'Kurz: Azure Active Directory integrace s Workpath | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Workpath."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 320b0daf-14be-4813-b59b-25a6a5070690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 69f443f314edb7c8c489a6c193e09b6f8fe6795a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workpath"></a><span data-ttu-id="47c92-103">Kurz: Azure Active Directory integrace s Workpath</span><span class="sxs-lookup"><span data-stu-id="47c92-103">Tutorial: Azure Active Directory integration with Workpath</span></span>

<span data-ttu-id="47c92-104">V tomto kurzu zjistíte, jak toointegrate Workpath s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="47c92-104">In this tutorial, you learn how toointegrate Workpath with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="47c92-105">Integrace Workpath s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="47c92-105">Integrating Workpath with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="47c92-106">Můžete řídit ve službě Azure AD, který má přístup tooWorkpath</span><span class="sxs-lookup"><span data-stu-id="47c92-106">You can control in Azure AD who has access tooWorkpath</span></span>
- <span data-ttu-id="47c92-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWorkpath (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="47c92-107">You can enable your users tooautomatically get signed-on tooWorkpath (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="47c92-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="47c92-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="47c92-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="47c92-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47c92-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="47c92-110">Prerequisites</span></span>

<span data-ttu-id="47c92-111">Integrace služby Azure AD s Workpath tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="47c92-111">tooconfigure Azure AD integration with Workpath, you need hello following items:</span></span>

- <span data-ttu-id="47c92-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="47c92-112">An Azure AD subscription</span></span>
- <span data-ttu-id="47c92-113">Workpath jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="47c92-113">A Workpath single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="47c92-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="47c92-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="47c92-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="47c92-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="47c92-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="47c92-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="47c92-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="47c92-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="47c92-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="47c92-118">Scenario description</span></span>
<span data-ttu-id="47c92-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="47c92-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="47c92-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="47c92-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="47c92-121">Přidání Workpath z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="47c92-121">Adding Workpath from hello gallery</span></span>
2. <span data-ttu-id="47c92-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="47c92-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workpath-from-hello-gallery"></a><span data-ttu-id="47c92-123">Přidání Workpath z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="47c92-123">Adding Workpath from hello gallery</span></span>
<span data-ttu-id="47c92-124">tooconfigure hello integrace Workpath do Azure AD, je nutné tooadd Workpath hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="47c92-124">tooconfigure hello integration of Workpath into Azure AD, you need tooadd Workpath from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="47c92-125">**tooadd Workpath z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="47c92-125">**tooadd Workpath from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="47c92-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="47c92-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="47c92-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="47c92-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="47c92-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="47c92-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="47c92-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="47c92-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="47c92-133">Hello vyhledávacího pole zadejte **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="47c92-133">In hello search box, type **Workpath**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_search.png)

5. <span data-ttu-id="47c92-135">Na panelu výsledků hello vyberte **Workpath**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="47c92-135">In hello results panel, select **Workpath**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="47c92-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="47c92-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="47c92-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workpath podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="47c92-138">In this section, you configure and test Azure AD single sign-on with Workpath based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="47c92-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Workpath je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="47c92-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workpath is tooa user in Azure AD.</span></span> <span data-ttu-id="47c92-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Workpath musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="47c92-140">In other words, a link relationship between an Azure AD user and hello related user in Workpath needs toobe established.</span></span>

<span data-ttu-id="47c92-141">V Workpath, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="47c92-141">In Workpath, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="47c92-142">tooconfigure a testu Azure AD jednotné přihlašování s Workpath, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="47c92-142">tooconfigure and test Azure AD single sign-on with Workpath, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="47c92-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="47c92-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="47c92-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="47c92-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="47c92-145">**[Vytvoření zkušebního uživatele Workpath](#creating-a-workpath-test-user)**  -toohave protějšek Britta Simon v Workpath, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="47c92-145">**[Creating a Workpath test user](#creating-a-workpath-test-user)** - toohave a counterpart of Britta Simon in Workpath that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="47c92-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="47c92-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="47c92-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="47c92-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="47c92-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="47c92-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="47c92-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Workpath.</span><span class="sxs-lookup"><span data-stu-id="47c92-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workpath application.</span></span>

<span data-ttu-id="47c92-150">**tooconfigure Azure AD jednotné přihlašování s Workpath, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="47c92-150">**tooconfigure Azure AD single sign-on with Workpath, perform hello following steps:**</span></span>

1. <span data-ttu-id="47c92-151">V portálu Azure, na hello hello **Workpath** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="47c92-151">In hello Azure portal, on hello **Workpath** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="47c92-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="47c92-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_samlbase.png)

3. <span data-ttu-id="47c92-155">Na hello **Workpath domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** initiated režim provedení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47c92-155">On hello **Workpath Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url.png)

    <span data-ttu-id="47c92-157">a.</span><span class="sxs-lookup"><span data-stu-id="47c92-157">a.</span></span> <span data-ttu-id="47c92-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://api.workpath.com/v1/saml/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="47c92-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://api.workpath.com/v1/saml/metadata/<instancename>`</span></span>

    <span data-ttu-id="47c92-159">b.</span><span class="sxs-lookup"><span data-stu-id="47c92-159">b.</span></span> <span data-ttu-id="47c92-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://api.workpath.com/v1/saml/assert/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="47c92-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://api.workpath.com/v1/saml/assert/<instancename>`</span></span>

4. <span data-ttu-id="47c92-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="47c92-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="47c92-162">Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="47c92-162">If you wish tooconfigure hello application in **SP** initiated mode, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url1.png)

    <span data-ttu-id="47c92-164">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.workpath.com/ `</span><span class="sxs-lookup"><span data-stu-id="47c92-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.workpath.com/ `</span></span>

    > [!NOTE] 
    > <span data-ttu-id="47c92-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="47c92-165">These values are not real.</span></span> <span data-ttu-id="47c92-166">Tyto hodnoty aktualizujte s hello skutečná adresa URL přihlašování, identifikátor a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="47c92-166">Update these values with hello actual Sign-on URL, Identifier and Reply URL.</span></span> <span data-ttu-id="47c92-167">Obraťte se na [tým podpory Workpath](https://help.workpath.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="47c92-167">Contact [Workpath support team](https://help.workpath.com) tooget these values.</span></span>

5. <span data-ttu-id="47c92-168">Aplikace Workpath očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="47c92-168">Workpath application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="47c92-169">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="47c92-169">Configure hello following claims for this application.</span></span> <span data-ttu-id="47c92-170">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="47c92-170">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="47c92-171">Hello následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="47c92-171">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_attributes.png)
    
6. <span data-ttu-id="47c92-173">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47c92-173">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="47c92-174">Název atributu</span><span class="sxs-lookup"><span data-stu-id="47c92-174">Attribute Name</span></span> | <span data-ttu-id="47c92-175">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="47c92-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="47c92-176">křestní_jméno</span><span class="sxs-lookup"><span data-stu-id="47c92-176">first_name</span></span> | <span data-ttu-id="47c92-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="47c92-177">user.givenname</span></span> |
    | <span data-ttu-id="47c92-178">Příjmení</span><span class="sxs-lookup"><span data-stu-id="47c92-178">last_name</span></span> | <span data-ttu-id="47c92-179">User.Surname</span><span class="sxs-lookup"><span data-stu-id="47c92-179">user.surname</span></span> |
    
    <span data-ttu-id="47c92-180">a.</span><span class="sxs-lookup"><span data-stu-id="47c92-180">a.</span></span> <span data-ttu-id="47c92-181">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="47c92-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="47c92-183">b.</span><span class="sxs-lookup"><span data-stu-id="47c92-183">b.</span></span> <span data-ttu-id="47c92-184">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="47c92-184">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="47c92-186">c.</span><span class="sxs-lookup"><span data-stu-id="47c92-186">c.</span></span> <span data-ttu-id="47c92-187">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="47c92-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="47c92-188">d.</span><span class="sxs-lookup"><span data-stu-id="47c92-188">d.</span></span> <span data-ttu-id="47c92-189">Nechte hello **Namespace** textové pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="47c92-189">Leave hello **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="47c92-190">e.</span><span class="sxs-lookup"><span data-stu-id="47c92-190">e.</span></span> <span data-ttu-id="47c92-191">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="47c92-191">Click **Ok**.</span></span>
    

7. <span data-ttu-id="47c92-192">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="47c92-192">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_certificate.png) 

8. <span data-ttu-id="47c92-194">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="47c92-194">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="47c92-196">Na hello **Workpath konfigurace** klikněte na tlačítko **konfigurace Workpath** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="47c92-196">On hello **Workpath Configuration** section, click **Configure Workpath** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="47c92-197">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="47c92-197">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_configure.png) 

10. <span data-ttu-id="47c92-199">tooconfigure jednotného přihlašování na **Workpath** straně, je nutné stáhnout hello toosend **soubor XML s metadaty**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[tým podpory Workpath](https://help.workpath.com).</span><span class="sxs-lookup"><span data-stu-id="47c92-199">tooconfigure single sign-on on **Workpath** side, you need toosend hello downloaded **Metadata XML**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Workpath support team](https://help.workpath.com).</span></span> 

> [!TIP]
> <span data-ttu-id="47c92-200">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="47c92-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="47c92-201">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="47c92-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="47c92-202">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="47c92-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="47c92-203">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="47c92-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="47c92-204">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="47c92-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="47c92-206">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="47c92-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="47c92-207">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="47c92-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="47c92-209">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="47c92-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="47c92-211">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="47c92-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="47c92-213">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47c92-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="47c92-215">a.</span><span class="sxs-lookup"><span data-stu-id="47c92-215">a.</span></span> <span data-ttu-id="47c92-216">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="47c92-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="47c92-217">b.</span><span class="sxs-lookup"><span data-stu-id="47c92-217">b.</span></span> <span data-ttu-id="47c92-218">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="47c92-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="47c92-219">c.</span><span class="sxs-lookup"><span data-stu-id="47c92-219">c.</span></span> <span data-ttu-id="47c92-220">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="47c92-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="47c92-221">d.</span><span class="sxs-lookup"><span data-stu-id="47c92-221">d.</span></span> <span data-ttu-id="47c92-222">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="47c92-222">Click **Create**.</span></span>
 
### <a name="creating-a-workpath-test-user"></a><span data-ttu-id="47c92-223">Vytvoření zkušebního uživatele Workpath</span><span class="sxs-lookup"><span data-stu-id="47c92-223">Creating a Workpath test user</span></span>

<span data-ttu-id="47c92-224">Workpath podporuje pouze v době zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="47c92-224">Workpath supports Just in time user provisioning.</span></span> <span data-ttu-id="47c92-225">Po ověření uživatelé jsou automaticky vytvořené v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="47c92-225">After authentication users are created in hello application automatically.</span></span> 


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="47c92-226">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="47c92-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="47c92-227">V této části povolíte tak, že udělíte přístup tooWorkpath toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="47c92-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkpath.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="47c92-229">**tooassign Britta Simon tooWorkpath, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="47c92-229">**tooassign Britta Simon tooWorkpath, perform hello following steps:**</span></span>

1. <span data-ttu-id="47c92-230">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="47c92-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="47c92-232">V seznamu aplikace hello vyberte **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="47c92-232">In hello applications list, select **Workpath**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_app.png) 

3. <span data-ttu-id="47c92-234">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="47c92-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="47c92-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="47c92-236">Click **Add** button.</span></span> <span data-ttu-id="47c92-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="47c92-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="47c92-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="47c92-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="47c92-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="47c92-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="47c92-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="47c92-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="47c92-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="47c92-242">Testing single sign-on</span></span>

<span data-ttu-id="47c92-243">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="47c92-243">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="47c92-244">Když kliknete na dlaždici Workpath hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Workpath aplikace.</span><span class="sxs-lookup"><span data-stu-id="47c92-244">When you click hello Workpath tile in hello Access Panel, you should get automatically signed-on tooyour Workpath application.</span></span>
<span data-ttu-id="47c92-245">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="47c92-245">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="47c92-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="47c92-246">Additional resources</span></span>

* [<span data-ttu-id="47c92-247">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="47c92-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="47c92-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="47c92-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_203.png

