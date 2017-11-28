---
title: 'Kurz: Azure Active Directory integrace s Moxtra | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Moxtra."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 82e2fcc390ba508e86a3992ec1c81d0a0ffed96b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a><span data-ttu-id="6d1bb-103">Kurz: Azure Active Directory integrace s Moxtra</span><span class="sxs-lookup"><span data-stu-id="6d1bb-103">Tutorial: Azure Active Directory integration with Moxtra</span></span>

<span data-ttu-id="6d1bb-104">V tomto kurzu zjistíte, jak toointegrate Moxtra s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6d1bb-104">In this tutorial, you learn how toointegrate Moxtra with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6d1bb-105">Integrace Moxtra s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-105">Integrating Moxtra with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6d1bb-106">Můžete řídit ve službě Azure AD, který má přístup tooMoxtra</span><span class="sxs-lookup"><span data-stu-id="6d1bb-106">You can control in Azure AD who has access tooMoxtra</span></span>
- <span data-ttu-id="6d1bb-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMoxtra (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d1bb-107">You can enable your users tooautomatically get signed-on tooMoxtra (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6d1bb-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6d1bb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6d1bb-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6d1bb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d1bb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6d1bb-110">Prerequisites</span></span>

<span data-ttu-id="6d1bb-111">Integrace služby Azure AD s Moxtra tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-111">tooconfigure Azure AD integration with Moxtra, you need hello following items:</span></span>

- <span data-ttu-id="6d1bb-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d1bb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6d1bb-113">Moxtra jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6d1bb-113">A Moxtra single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6d1bb-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6d1bb-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6d1bb-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6d1bb-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6d1bb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6d1bb-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6d1bb-118">Scenario description</span></span>
<span data-ttu-id="6d1bb-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6d1bb-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6d1bb-121">Přidání Moxtra z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6d1bb-121">Adding Moxtra from hello gallery</span></span>
2. <span data-ttu-id="6d1bb-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d1bb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moxtra-from-hello-gallery"></a><span data-ttu-id="6d1bb-123">Přidání Moxtra z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6d1bb-123">Adding Moxtra from hello gallery</span></span>
<span data-ttu-id="6d1bb-124">tooconfigure hello integrace Moxtra do Azure AD, je nutné tooadd Moxtra hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-124">tooconfigure hello integration of Moxtra into Azure AD, you need tooadd Moxtra from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6d1bb-125">**tooadd Moxtra z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6d1bb-125">**tooadd Moxtra from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6d1bb-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6d1bb-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6d1bb-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6d1bb-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6d1bb-133">Hello vyhledávacího pole zadejte **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-133">In hello search box, type **Moxtra**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. <span data-ttu-id="6d1bb-135">Na panelu výsledků hello vyberte **Moxtra**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-135">In hello results panel, select **Moxtra**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6d1bb-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d1bb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6d1bb-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Moxtra podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6d1bb-138">In this section, you configure and test Azure AD single sign-on with Moxtra based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6d1bb-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Moxtra je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Moxtra is tooa user in Azure AD.</span></span> <span data-ttu-id="6d1bb-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Moxtra musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-140">In other words, a link relationship between an Azure AD user and hello related user in Moxtra needs toobe established.</span></span>

<span data-ttu-id="6d1bb-141">V Moxtra, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-141">In Moxtra, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6d1bb-142">tooconfigure a testu Azure AD jednotné přihlašování s Moxtra, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-142">tooconfigure and test Azure AD single sign-on with Moxtra, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6d1bb-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6d1bb-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6d1bb-145">**[Vytvoření zkušebního uživatele Moxtra](#creating-a-moxtra-test-user)**  -toohave protějšek Britta Simon v Moxtra, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-145">**[Creating a Moxtra test user](#creating-a-moxtra-test-user)** - toohave a counterpart of Britta Simon in Moxtra that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6d1bb-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6d1bb-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6d1bb-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d1bb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6d1bb-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Moxtra.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Moxtra application.</span></span>

<span data-ttu-id="6d1bb-150">**tooconfigure Azure AD jednotné přihlašování s Moxtra, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6d1bb-150">**tooconfigure Azure AD single sign-on with Moxtra, perform hello following steps:**</span></span>

1. <span data-ttu-id="6d1bb-151">V portálu Azure, na hello hello **Moxtra** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-151">In hello Azure portal, on hello **Moxtra** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6d1bb-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. <span data-ttu-id="6d1bb-155">Na hello **Moxtra domény a adresy URL** část, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-155">On hello **Moxtra Domain and URLs** section, perform hello following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    <span data-ttu-id="6d1bb-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://www.moxtra.com/service/#login`</span><span class="sxs-lookup"><span data-stu-id="6d1bb-157">In hello **Sign-on URL** textbox, type a URL as: `https://www.moxtra.com/service/#login`</span></span>

4. <span data-ttu-id="6d1bb-158">Aplikace Moxtra očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-158">Moxtra application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="6d1bb-159">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-159">Configure hello following claims for this application.</span></span> <span data-ttu-id="6d1bb-160">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-160">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="6d1bb-161">Hello následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-161">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. <span data-ttu-id="6d1bb-163">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-163">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="6d1bb-164">Název atributu</span><span class="sxs-lookup"><span data-stu-id="6d1bb-164">Attribute Name</span></span> | <span data-ttu-id="6d1bb-165">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="6d1bb-165">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="6d1bb-166">FirstName</span><span class="sxs-lookup"><span data-stu-id="6d1bb-166">firstname</span></span> | <span data-ttu-id="6d1bb-167">User.givenName</span><span class="sxs-lookup"><span data-stu-id="6d1bb-167">user.givenname</span></span> |
    | <span data-ttu-id="6d1bb-168">Příjmení</span><span class="sxs-lookup"><span data-stu-id="6d1bb-168">lastname</span></span> | <span data-ttu-id="6d1bb-169">User.Surname</span><span class="sxs-lookup"><span data-stu-id="6d1bb-169">user.surname</span></span> |
    | <span data-ttu-id="6d1bb-170">idpid</span><span class="sxs-lookup"><span data-stu-id="6d1bb-170">idpid</span></span>    | <span data-ttu-id="6d1bb-171">< SAML Entity ID ></span><span class="sxs-lookup"><span data-stu-id="6d1bb-171">< SAML Entity ID ></span></span> 

    > [!Note]
    > <span data-ttu-id="6d1bb-172">Hello hodnotu **idpid** atribut není skutečné.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-172">hello value of **idpid** attribute is not real.</span></span> <span data-ttu-id="6d1bb-173">Můžete získat skutečnou hodnotu hello z **Stručná referenční příručka** oddílu pod **Moxtra konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-173">You can get hello actual value from **Quick reference** section under **Moxtra Configuration**.</span></span>
    
    <span data-ttu-id="6d1bb-174">a.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-174">a.</span></span> <span data-ttu-id="6d1bb-175">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-175">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="6d1bb-177">b.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-177">b.</span></span> <span data-ttu-id="6d1bb-178">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-178">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="6d1bb-180">c.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-180">c.</span></span> <span data-ttu-id="6d1bb-181">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-181">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="6d1bb-182">d.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-182">d.</span></span> <span data-ttu-id="6d1bb-183">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-183">Click **Ok**.</span></span>
    
5. <span data-ttu-id="6d1bb-184">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-184">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. <span data-ttu-id="6d1bb-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-186">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="6d1bb-188">Na hello **Moxtra konfigurace** klikněte na tlačítko **konfigurace Moxtra** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-188">On hello **Moxtra Configuration** section, click **Configure Moxtra** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6d1bb-189">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6d1bb-189">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. <span data-ttu-id="6d1bb-191">V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti Moxtra tooyour.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-191">In another browser window, sign on tooyour Moxtra company site as an administrator.</span></span>

9. <span data-ttu-id="6d1bb-192">V panelu nástrojů hello na levé straně hello, klikněte na **konzoly pro správu > SAML jednotné přihlašování**a potom klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-192">In hello toolbar on hello left, click **Admin Console > SAML Single Sign-on**, and then click **New**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. <span data-ttu-id="6d1bb-194">Na hello **SAML** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-194">On hello **SAML** page, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    <span data-ttu-id="6d1bb-196">a.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-196">a.</span></span> <span data-ttu-id="6d1bb-197">V hello **název** textovému poli, zadejte název pro svou konfiguraci (například: *SAML*).</span><span class="sxs-lookup"><span data-stu-id="6d1bb-197">In hello **Name** textbox, type a name for your configuration (e.g.: *SAML*).</span></span> 
  
    <span data-ttu-id="6d1bb-198">b.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-198">b.</span></span> <span data-ttu-id="6d1bb-199">V hello **IdP Entity ID** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-199">In hello **IdP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="6d1bb-200">c.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-200">c.</span></span> <span data-ttu-id="6d1bb-201">V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-201">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="6d1bb-202">d.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-202">d.</span></span> <span data-ttu-id="6d1bb-203">V hello **AuthnContextClassRef** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-203">In hello **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span> 
 
    <span data-ttu-id="6d1bb-204">e.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-204">e.</span></span> <span data-ttu-id="6d1bb-205">V hello **NameID formátu** textovému poli, typ **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-205">In hello **NameID Format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span> 
 
    <span data-ttu-id="6d1bb-206">f.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-206">f.</span></span> <span data-ttu-id="6d1bb-207">Otevřete certifikát, který jste si stáhli z portálu Azure v poznámkovém bloku hello obsah zkopírujte a vložte jej do hello **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-207">Open certificate which you have downloaded from Azure portal in notepad, copy hello content, and then paste it into hello **Certificate** textbox.</span></span>    
 
    <span data-ttu-id="6d1bb-208">g.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-208">g.</span></span> <span data-ttu-id="6d1bb-209">Do pole pro e-mailovou doménu hello SAML zadejte vaše e-mailovou doménu SAML.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-209">In hello SAML email domain textbox, type your SAML email domain.</span></span>    
  
    >[!NOTE]
    ><span data-ttu-id="6d1bb-210">toosee hello kroky tooverify hello domény, klikněte na tlačítko hello "**i**" níže.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-210">toosee hello steps tooverify hello domain, click hello "**i**" below.</span></span>

    <span data-ttu-id="6d1bb-211">h.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-211">h.</span></span> <span data-ttu-id="6d1bb-212">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-212">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="6d1bb-213">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6d1bb-213">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6d1bb-214">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-214">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6d1bb-215">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6d1bb-215">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6d1bb-216">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d1bb-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="6d1bb-217">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6d1bb-219">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6d1bb-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6d1bb-220">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6d1bb-222">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-222">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6d1bb-224">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-224">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6d1bb-226">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6d1bb-228">a.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-228">a.</span></span> <span data-ttu-id="6d1bb-229">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6d1bb-230">b.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-230">b.</span></span> <span data-ttu-id="6d1bb-231">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-231">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6d1bb-232">c.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-232">c.</span></span> <span data-ttu-id="6d1bb-233">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6d1bb-234">d.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-234">d.</span></span> <span data-ttu-id="6d1bb-235">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-235">Click **Create**.</span></span>
 
### <a name="creating-a-moxtra-test-user"></a><span data-ttu-id="6d1bb-236">Vytvoření zkušebního uživatele Moxtra</span><span class="sxs-lookup"><span data-stu-id="6d1bb-236">Creating a Moxtra test user</span></span>

<span data-ttu-id="6d1bb-237">Hello cílem této části je toocreate volal Britta Simon v Moxtra uživatele.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-237">hello objective of this section is toocreate a user called Britta Simon in Moxtra.</span></span>

<span data-ttu-id="6d1bb-238">**toocreate uživatel volal Britta Simon v Moxtra, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6d1bb-238">**toocreate a user called Britta Simon in Moxtra, perform hello following steps:**</span></span>

1. <span data-ttu-id="6d1bb-239">Přihlaste se na tooyour Moxtra společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-239">Sign on tooyour Moxtra company site as an administrator.</span></span>

2. <span data-ttu-id="6d1bb-240">V panelu nástrojů hello na levé straně hello, klikněte na **konzoly pro správu > Správa uživatelů**a potom **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-240">In hello toolbar on hello left, click **Admin Console > User Management**, and then **Add User**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. <span data-ttu-id="6d1bb-242">Na hello **přidat uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6d1bb-242">On hello **Add User** dialog, perform hello following steps:</span></span>
  
    <span data-ttu-id="6d1bb-243">a.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-243">a.</span></span> <span data-ttu-id="6d1bb-244">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-244">In hello **First Name** textbox, type **Britta**.</span></span>
  
    <span data-ttu-id="6d1bb-245">b.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-245">b.</span></span> <span data-ttu-id="6d1bb-246">V hello **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-246">In hello **Last Name** textbox, type **Simon**.</span></span>
  
    <span data-ttu-id="6d1bb-247">c.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-247">c.</span></span> <span data-ttu-id="6d1bb-248">V hello **e-mailu** textovému poli, typ Britta na e-mailová adresa, aby se pro stejné jako na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-248">In hello **Email** textbox, type Britta's email address same as on Azure portal.</span></span>
  
    <span data-ttu-id="6d1bb-249">d.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-249">d.</span></span> <span data-ttu-id="6d1bb-250">V hello **dělení** textovému poli, typ **Dev**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-250">In hello **Division** textbox, type **Dev**.</span></span>
  
    <span data-ttu-id="6d1bb-251">e.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-251">e.</span></span> <span data-ttu-id="6d1bb-252">V hello **oddělení** textovému poli, typ **IT**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-252">In hello **Department** textbox, type **IT**.</span></span>
  
    <span data-ttu-id="6d1bb-253">f.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-253">f.</span></span> <span data-ttu-id="6d1bb-254">Vyberte **správce**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-254">Select **Administrator**.</span></span>
  
    <span data-ttu-id="6d1bb-255">g.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-255">g.</span></span> <span data-ttu-id="6d1bb-256">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-256">Click **Add**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6d1bb-257">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6d1bb-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6d1bb-258">V této části povolíte tak, že udělíte přístup tooMoxtra toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMoxtra.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6d1bb-260">**tooassign Britta Simon tooMoxtra, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6d1bb-260">**tooassign Britta Simon tooMoxtra, perform hello following steps:**</span></span>

1. <span data-ttu-id="6d1bb-261">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6d1bb-263">V seznamu aplikace hello vyberte **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-263">In hello applications list, select **Moxtra**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. <span data-ttu-id="6d1bb-265">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6d1bb-267">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-267">Click **Add** button.</span></span> <span data-ttu-id="6d1bb-268">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6d1bb-270">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6d1bb-271">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6d1bb-272">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6d1bb-273">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d1bb-273">Testing single sign-on</span></span>

<span data-ttu-id="6d1bb-274">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-274">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6d1bb-275">Když kliknete na dlaždici Moxtra hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Moxtra aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d1bb-275">When you click hello Moxtra tile in hello Access Panel, you should get automatically signed-on tooyour Moxtra application.</span></span>
<span data-ttu-id="6d1bb-276">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6d1bb-276">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6d1bb-277">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6d1bb-277">Additional resources</span></span>

* [<span data-ttu-id="6d1bb-278">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d1bb-278">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6d1bb-279">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6d1bb-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

