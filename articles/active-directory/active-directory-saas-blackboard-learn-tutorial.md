---
title: "Kurz: Azure Active Directory integrace s další Tabule | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a další Tabule."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b8ca505-61ea-487c-9a3e-fa50c936df0c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jeedes
ms.openlocfilehash: e94cdd6eaf876d4f66bdd783c442dc468f104e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a><span data-ttu-id="f9b59-103">Kurz: Azure Active Directory integrace s další Tabule</span><span class="sxs-lookup"><span data-stu-id="f9b59-103">Tutorial: Azure Active Directory integration with Blackboard Learn</span></span>

<span data-ttu-id="f9b59-104">V tomto kurzu zjistíte, jak toointegrate Tabule další službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f9b59-104">In this tutorial, you learn how toointegrate Blackboard Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f9b59-105">Integrace Další tabule s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f9b59-105">Integrating Blackboard Learn with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f9b59-106">Můžete řídit ve službě Azure AD, který má přístup tooBlackboard informace</span><span class="sxs-lookup"><span data-stu-id="f9b59-106">You can control in Azure AD who has access tooBlackboard Learn</span></span>
- <span data-ttu-id="f9b59-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBlackboard informace (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f9b59-107">You can enable your users tooautomatically get signed-on tooBlackboard Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f9b59-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f9b59-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f9b59-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f9b59-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9b59-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f9b59-110">Prerequisites</span></span>

<span data-ttu-id="f9b59-111">tooconfigure integrace Azure AD s další Tabule, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f9b59-111">tooconfigure Azure AD integration with Blackboard Learn, you need hello following items:</span></span>

- <span data-ttu-id="f9b59-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f9b59-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f9b59-113">Další tabule jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f9b59-113">A Blackboard Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f9b59-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f9b59-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f9b59-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f9b59-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f9b59-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f9b59-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f9b59-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f9b59-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f9b59-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f9b59-118">Scenario description</span></span>
<span data-ttu-id="f9b59-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f9b59-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f9b59-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f9b59-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f9b59-121">Přidání další Tabule z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f9b59-121">Adding Blackboard Learn from hello gallery</span></span>
2. <span data-ttu-id="f9b59-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f9b59-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-blackboard-learn-from-hello-gallery"></a><span data-ttu-id="f9b59-123">Přidání další Tabule z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f9b59-123">Adding Blackboard Learn from hello gallery</span></span>
<span data-ttu-id="f9b59-124">tooconfigure hello integrace další Tabule do Azure AD, je nutné tooadd další Tabule hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f9b59-124">tooconfigure hello integration of Blackboard Learn into Azure AD, you need tooadd Blackboard Learn from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f9b59-125">**tooadd Tabule informace z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f9b59-125">**tooadd Blackboard Learn from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f9b59-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f9b59-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f9b59-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f9b59-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f9b59-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f9b59-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f9b59-133">Hello vyhledávacího pole zadejte **další Tabule**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-133">In hello search box, type **Blackboard Learn**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_search.png)

5. <span data-ttu-id="f9b59-135">Na panelu výsledků hello vyberte **další Tabule**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9b59-135">In hello results panel, select **Blackboard Learn**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f9b59-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f9b59-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f9b59-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tabule další podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f9b59-138">In this section, you configure and test Azure AD single sign-on with Blackboard Learn based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f9b59-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v další Tabule je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f9b59-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Blackboard Learn is tooa user in Azure AD.</span></span> <span data-ttu-id="f9b59-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v další Tabule musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f9b59-140">In other words, a link relationship between an Azure AD user and hello related user in Blackboard Learn needs toobe established.</span></span>

<span data-ttu-id="f9b59-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Tabule Další.</span><span class="sxs-lookup"><span data-stu-id="f9b59-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Blackboard Learn.</span></span>

<span data-ttu-id="f9b59-142">tooconfigure a testu Azure AD jednotné přihlašování s další Tabule, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f9b59-142">tooconfigure and test Azure AD single sign-on with Blackboard Learn, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f9b59-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f9b59-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f9b59-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f9b59-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f9b59-145">**[Vytvoření zkušebního uživatele další Tabule](#creating-a-blackboard-learn-test-user)**  -toohave protějšek Britta Simon v Tabule informace, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="f9b59-145">**[Creating a Blackboard Learn test user](#creating-a-blackboard-learn-test-user)** - toohave a counterpart of Britta Simon in Blackboard Learn that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f9b59-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f9b59-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f9b59-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f9b59-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f9b59-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f9b59-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f9b59-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Tabule Další.</span><span class="sxs-lookup"><span data-stu-id="f9b59-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Blackboard Learn application.</span></span>

<span data-ttu-id="f9b59-150">**tooconfigure Azure AD jednotné přihlašování s další Tabule, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f9b59-150">**tooconfigure Azure AD single sign-on with Blackboard Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="f9b59-151">V portálu Azure, na hello hello **další Tabule** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-151">In hello Azure portal, on hello **Blackboard Learn** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f9b59-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f9b59-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_samlbase.png)

3. <span data-ttu-id="f9b59-155">Na hello **Tabule další domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f9b59-155">On hello **Blackboard Learn Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_url.png)

    <span data-ttu-id="f9b59-157">a.</span><span class="sxs-lookup"><span data-stu-id="f9b59-157">a.</span></span> <span data-ttu-id="f9b59-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.blackboard.com/`</span><span class="sxs-lookup"><span data-stu-id="f9b59-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.blackboard.com/`</span></span>

    <span data-ttu-id="f9b59-159">b.</span><span class="sxs-lookup"><span data-stu-id="f9b59-159">b.</span></span> <span data-ttu-id="f9b59-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span><span class="sxs-lookup"><span data-stu-id="f9b59-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="f9b59-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f9b59-161">These values are not real.</span></span> <span data-ttu-id="f9b59-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="f9b59-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f9b59-163">Obraťte se na [tým podpory pro další klienta Tabule](https://www.blackboard.com/support/index.aspx) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f9b59-163">Contact [Blackboard Learn Client support team](https://www.blackboard.com/support/index.aspx) tooget these values.</span></span> 

4. <span data-ttu-id="f9b59-164">Další aplikace Tabule očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="f9b59-164">Blackboard Learn application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="f9b59-165">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f9b59-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="f9b59-166">Můžete spravovat hello hodnoty těchto atributů z hello **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9b59-166">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span>
 <span data-ttu-id="f9b59-167">Hello následující snímek obrazovky ukazuje příklad o něm.</span><span class="sxs-lookup"><span data-stu-id="f9b59-167">hello following screenshot shows an example about it.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="f9b59-169">V hello **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, konfigurovat atributy tokenu SAML, jak je znázorněno v bitové kopii hello a proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="f9b59-169">In hello **User Attributes** section on **Single sign-on** dialog, configure SAML token attributes as shown in hello image and perform hello following steps.</span></span> <span data-ttu-id="f9b59-170">Jako atribut jedinečný uživatele hello Zde jsme mají mapovat hello Userprincipalname ale namapovat je toohello odpovídající hodnotu, která jedinečně hello uživatele v organizaci hello a která se mapuje pole pro uživatelské jméno tooBlackboard informace.</span><span class="sxs-lookup"><span data-stu-id="f9b59-170">We have mapped hello Userprincipalname as hello unique user attribute here but you can map it toohello appropriate value, which uniquely distinguishes hello user in hello organization and that maps tooBlackboard Learn username field.</span></span>
           
    | <span data-ttu-id="f9b59-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="f9b59-171">Attribute Name</span></span> | <span data-ttu-id="f9b59-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="f9b59-172">Attribute Value</span></span> |   
    | ---------------| ----------------|
    | <span data-ttu-id="f9b59-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span><span class="sxs-lookup"><span data-stu-id="f9b59-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span></span> |<span data-ttu-id="f9b59-174">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="f9b59-174">user.userprincipalname</span></span> |

    <span data-ttu-id="f9b59-175">a.</span><span class="sxs-lookup"><span data-stu-id="f9b59-175">a.</span></span> <span data-ttu-id="f9b59-176">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f9b59-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="f9b59-179">b.</span><span class="sxs-lookup"><span data-stu-id="f9b59-179">b.</span></span> <span data-ttu-id="f9b59-180">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="f9b59-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="f9b59-181">c.</span><span class="sxs-lookup"><span data-stu-id="f9b59-181">c.</span></span> <span data-ttu-id="f9b59-182">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="f9b59-182">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="f9b59-183">d.</span><span class="sxs-lookup"><span data-stu-id="f9b59-183">d.</span></span> <span data-ttu-id="f9b59-184">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-184">Click **Ok**.</span></span>

4. <span data-ttu-id="f9b59-185">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f9b59-185">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_certificate.png)

7. <span data-ttu-id="f9b59-187">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f9b59-187">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f9b59-189">Na hello **Tabule další konfigurace** klikněte na tlačítko **nakonfigurovat další Tabule** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f9b59-189">On hello **Blackboard Learn Configuration** section, click **Configure Blackboard Learn** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f9b59-190">Kopírování hello **SAML Entity ID** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f9b59-190">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_configure.png) 

9. <span data-ttu-id="f9b59-192">tooconfigure jednotného přihlašování na **další Tabule** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** a **SAML Entity ID** příliš[další Tabule Podpora](https://www.blackboard.com/support/index.aspx).</span><span class="sxs-lookup"><span data-stu-id="f9b59-192">tooconfigure single sign-on on **Blackboard Learn** side, you need toosend hello downloaded **Metadata XML** and **SAML Entity ID** too[Blackboard Learn support](https://www.blackboard.com/support/index.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="f9b59-193">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="f9b59-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f9b59-194">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="f9b59-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f9b59-195">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f9b59-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f9b59-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f9b59-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="f9b59-197">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="f9b59-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f9b59-199">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f9b59-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f9b59-200">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f9b59-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f9b59-202">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f9b59-204">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="f9b59-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f9b59-206">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f9b59-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f9b59-208">a.</span><span class="sxs-lookup"><span data-stu-id="f9b59-208">a.</span></span> <span data-ttu-id="f9b59-209">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f9b59-210">b.</span><span class="sxs-lookup"><span data-stu-id="f9b59-210">b.</span></span> <span data-ttu-id="f9b59-211">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f9b59-211">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="f9b59-212">c.</span><span class="sxs-lookup"><span data-stu-id="f9b59-212">c.</span></span> <span data-ttu-id="f9b59-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f9b59-214">d.</span><span class="sxs-lookup"><span data-stu-id="f9b59-214">d.</span></span> <span data-ttu-id="f9b59-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-215">Click **Create**.</span></span>
 
### <a name="creating-a-blackboard-learn-test-user"></a><span data-ttu-id="f9b59-216">Vytvoření zkušebního uživatele další Tabule</span><span class="sxs-lookup"><span data-stu-id="f9b59-216">Creating a Blackboard Learn test user</span></span>
<span data-ttu-id="f9b59-217">V této části vytvoříte volal Britta Simon v Tabule další uživatele.</span><span class="sxs-lookup"><span data-stu-id="f9b59-217">In this section, you create a user called Britta Simon in Blackboard Learn.</span></span> 

<span data-ttu-id="f9b59-218">Tabule další aplikace podporují jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="f9b59-218">Blackboard Learn application support just in time user provisioning.</span></span> <span data-ttu-id="f9b59-219">Ujistěte se, že jste nakonfigurovali hello deklarace identity, jak je popsáno v části hello  **[konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**</span><span class="sxs-lookup"><span data-stu-id="f9b59-219">Make sure that you have configured hello claims as described in hello section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**</span></span>
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f9b59-220">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f9b59-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f9b59-221">V této části povolíte tak, že udělíte přístup tooBlackboard další toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f9b59-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBlackboard Learn.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f9b59-223">**tooassign Britta Simon tooBlackboard informace, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f9b59-223">**tooassign Britta Simon tooBlackboard Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="f9b59-224">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f9b59-226">V seznamu aplikace hello vyberte **další Tabule**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-226">In hello applications list, select **Blackboard Learn**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_app.png) 

3. <span data-ttu-id="f9b59-228">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f9b59-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f9b59-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f9b59-230">Click **Add** button.</span></span> <span data-ttu-id="f9b59-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f9b59-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f9b59-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f9b59-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f9b59-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f9b59-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f9b59-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f9b59-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f9b59-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f9b59-236">Testing single sign-on</span></span>

<span data-ttu-id="f9b59-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f9b59-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f9b59-238">Po kliknutí na tlačítko dlaždice Tabule další hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Tabule další aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9b59-238">When you click hello Blackboard Learn tile in hello Access Panel, you should get automatically signed-on tooyour Blackboard Learn application.</span></span> <span data-ttu-id="f9b59-239">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f9b59-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f9b59-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f9b59-240">Additional resources</span></span>

* [<span data-ttu-id="f9b59-241">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f9b59-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f9b59-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f9b59-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png

