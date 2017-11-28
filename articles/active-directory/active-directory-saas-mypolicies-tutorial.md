---
title: 'Kurz: Azure Active Directory integrace s myPolicies | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a myPolicies."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bf79e858-1dfb-4ab3-a6df-74b2d5a878d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: jeedes
ms.openlocfilehash: d8890457ebdb1b80e0d3126d4210e6265ae7f1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mypolicies"></a><span data-ttu-id="5887b-103">Kurz: Azure Active Directory integrace s myPolicies</span><span class="sxs-lookup"><span data-stu-id="5887b-103">Tutorial: Azure Active Directory integration with myPolicies</span></span>

<span data-ttu-id="5887b-104">V tomto kurzu zjistíte, jak toointegrate myPolicies s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5887b-104">In this tutorial, you learn how toointegrate myPolicies with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5887b-105">Integrace myPolicies s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5887b-105">Integrating myPolicies with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5887b-106">Můžete řídit ve službě Azure AD, který má přístup toomyPolicies</span><span class="sxs-lookup"><span data-stu-id="5887b-106">You can control in Azure AD who has access toomyPolicies</span></span>
- <span data-ttu-id="5887b-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného toomyPolicies (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5887b-107">You can enable your users tooautomatically get signed-on toomyPolicies (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5887b-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5887b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5887b-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5887b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5887b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5887b-110">Prerequisites</span></span>

<span data-ttu-id="5887b-111">Integrace služby Azure AD s myPolicies tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5887b-111">tooconfigure Azure AD integration with myPolicies, you need hello following items:</span></span>

- <span data-ttu-id="5887b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5887b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5887b-113">MyPolicies jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5887b-113">A myPolicies single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5887b-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5887b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5887b-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5887b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5887b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5887b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5887b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5887b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5887b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5887b-118">Scenario description</span></span>
<span data-ttu-id="5887b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5887b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5887b-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5887b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5887b-121">Přidání myPolicies z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5887b-121">Adding myPolicies from hello gallery</span></span>
2. <span data-ttu-id="5887b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5887b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mypolicies-from-hello-gallery"></a><span data-ttu-id="5887b-123">Přidání myPolicies z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5887b-123">Adding myPolicies from hello gallery</span></span>
<span data-ttu-id="5887b-124">tooconfigure hello integrace myPolicies do Azure AD, je nutné tooadd myPolicies hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5887b-124">tooconfigure hello integration of myPolicies into Azure AD, you need tooadd myPolicies from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5887b-125">**tooadd myPolicies z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5887b-125">**tooadd myPolicies from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5887b-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5887b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5887b-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5887b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5887b-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5887b-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5887b-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5887b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5887b-133">Hello vyhledávacího pole zadejte **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="5887b-133">In hello search box, type **myPolicies**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_search.png)

5. <span data-ttu-id="5887b-135">Na panelu výsledků hello vyberte **myPolicies**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5887b-135">In hello results panel, select **myPolicies**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5887b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5887b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5887b-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s myPolicies podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5887b-138">In this section, you configure and test Azure AD single sign-on with myPolicies based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5887b-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v myPolicies je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5887b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in myPolicies is tooa user in Azure AD.</span></span> <span data-ttu-id="5887b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v myPolicies musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5887b-140">In other words, a link relationship between an Azure AD user and hello related user in myPolicies needs toobe established.</span></span>

<span data-ttu-id="5887b-141">V myPolicies, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5887b-141">In myPolicies, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5887b-142">tooconfigure a testu Azure AD jednotné přihlašování s myPolicies, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5887b-142">tooconfigure and test Azure AD single sign-on with myPolicies, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5887b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5887b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5887b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5887b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5887b-145">**[Vytvoření zkušebního uživatele myPolicies](#creating-a-mypolicies-test-user)**  -toohave protějšek Britta Simon v myPolicies, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5887b-145">**[Creating a myPolicies test user](#creating-a-mypolicies-test-user)** - toohave a counterpart of Britta Simon in myPolicies that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5887b-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5887b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5887b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5887b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5887b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5887b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5887b-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci myPolicies.</span><span class="sxs-lookup"><span data-stu-id="5887b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your myPolicies application.</span></span>

<span data-ttu-id="5887b-150">**tooconfigure Azure AD jednotné přihlašování s myPolicies, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5887b-150">**tooconfigure Azure AD single sign-on with myPolicies, perform hello following steps:**</span></span>

1. <span data-ttu-id="5887b-151">V portálu Azure, na hello hello **myPolicies** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5887b-151">In hello Azure portal, on hello **myPolicies** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5887b-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5887b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_samlbase.png)

3. <span data-ttu-id="5887b-155">Na hello **myPolicies domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5887b-155">On hello **myPolicies Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_url.png)

    <span data-ttu-id="5887b-157">a.</span><span class="sxs-lookup"><span data-stu-id="5887b-157">a.</span></span> <span data-ttu-id="5887b-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.mypolicies.com/`</span><span class="sxs-lookup"><span data-stu-id="5887b-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.mypolicies.com/`</span></span>

    <span data-ttu-id="5887b-159">b.</span><span class="sxs-lookup"><span data-stu-id="5887b-159">b.</span></span> <span data-ttu-id="5887b-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="5887b-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<tenantname>.mypolicies.com/users/auth/saml/callback`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5887b-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5887b-161">These values are not real.</span></span> <span data-ttu-id="5887b-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="5887b-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="5887b-163">Obraťte se na [tým podpory myPolicies](mailto:support@mypolicies.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5887b-163">Contact [myPolicies support team](mailto:support@mypolicies.com) tooget these values.</span></span>

4. <span data-ttu-id="5887b-164">aplikace Hello myPolicies očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="5887b-164">hello myPolicies application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="5887b-165">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5887b-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="5887b-166">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="5887b-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="5887b-167">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="5887b-167">hello following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_attribute.png)

5. <span data-ttu-id="5887b-169">Klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** zaškrtnout políčko hello **uživatelské atributy** části tooexpand hello atributy.</span><span class="sxs-lookup"><span data-stu-id="5887b-169">Click **View and edit all other user attributes** checkbox in hello **User Attributes** section tooexpand hello attributes.</span></span> <span data-ttu-id="5887b-170">Proveďte následující kroky na každém z hello zobrazí atributy - hello</span><span class="sxs-lookup"><span data-stu-id="5887b-170">Perform hello following steps on each of hello displayed attributes-</span></span>

    | <span data-ttu-id="5887b-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="5887b-171">Attribute Name</span></span> | <span data-ttu-id="5887b-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="5887b-172">Attribute Value</span></span> |
    | ------------------- | ---------- |
    | <span data-ttu-id="5887b-173">givenName</span><span class="sxs-lookup"><span data-stu-id="5887b-173">givenname</span></span> | <span data-ttu-id="5887b-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="5887b-174">user.givenname</span></span> |
    | <span data-ttu-id="5887b-175">Příjmení</span><span class="sxs-lookup"><span data-stu-id="5887b-175">surname</span></span> | <span data-ttu-id="5887b-176">User.Surname</span><span class="sxs-lookup"><span data-stu-id="5887b-176">user.surname</span></span> |
    | <span data-ttu-id="5887b-177">EmailAddress</span><span class="sxs-lookup"><span data-stu-id="5887b-177">emailaddress</span></span> | <span data-ttu-id="5887b-178">User.Mail</span><span class="sxs-lookup"><span data-stu-id="5887b-178">user.mail</span></span> |
    | <span data-ttu-id="5887b-179">jméno</span><span class="sxs-lookup"><span data-stu-id="5887b-179">name</span></span> | <span data-ttu-id="5887b-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="5887b-180">user.userprincipalname</span></span> |
    
    <span data-ttu-id="5887b-181">a.</span><span class="sxs-lookup"><span data-stu-id="5887b-181">a.</span></span> <span data-ttu-id="5887b-182">Klikněte na hello atribut tooopen hello **Upravit atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5887b-182">Click on hello attribute tooopen hello **Edit Attribute** dialog.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="5887b-184">b.</span><span class="sxs-lookup"><span data-stu-id="5887b-184">b.</span></span> <span data-ttu-id="5887b-185">Odstranit hodnotu adresy URL hello z hello **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="5887b-185">Delete hello URL value from hello **Namespace**.</span></span>
    
    <span data-ttu-id="5887b-186">c.</span><span class="sxs-lookup"><span data-stu-id="5887b-186">c.</span></span> <span data-ttu-id="5887b-187">Klikněte na tlačítko **Ok** toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="5887b-187">Click **Ok** toosave hello setting.</span></span>
    
6. <span data-ttu-id="5887b-188">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5887b-188">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_certificate.png) 

7. <span data-ttu-id="5887b-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5887b-190">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="5887b-192">Na hello **myPolicies konfigurace** klikněte na tlačítko **konfigurace myPolicies** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5887b-192">On hello **myPolicies Configuration** section, click **Configure myPolicies** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5887b-193">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5887b-193">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_configure.png) 

9. <span data-ttu-id="5887b-195">tooconfigure jednotného přihlašování na **myPolicies** straně, je nutné stáhnout hello toosend **Certificate(Base64)** a **SAML jeden přihlašování adresa URL služby** příliš[tým podpory myPolicies](mailto:support@mypolicies.com).</span><span class="sxs-lookup"><span data-stu-id="5887b-195">tooconfigure single sign-on on **myPolicies** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** too[myPolicies support team](mailto:support@mypolicies.com).</span></span> 

> [!TIP]
> <span data-ttu-id="5887b-196">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5887b-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5887b-197">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5887b-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5887b-198">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5887b-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5887b-199">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5887b-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="5887b-200">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5887b-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5887b-202">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5887b-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5887b-203">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5887b-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5887b-205">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5887b-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5887b-207">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="5887b-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5887b-209">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5887b-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mypolicies-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5887b-211">a.</span><span class="sxs-lookup"><span data-stu-id="5887b-211">a.</span></span> <span data-ttu-id="5887b-212">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5887b-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5887b-213">b.</span><span class="sxs-lookup"><span data-stu-id="5887b-213">b.</span></span> <span data-ttu-id="5887b-214">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5887b-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5887b-215">c.</span><span class="sxs-lookup"><span data-stu-id="5887b-215">c.</span></span> <span data-ttu-id="5887b-216">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5887b-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5887b-217">d.</span><span class="sxs-lookup"><span data-stu-id="5887b-217">d.</span></span> <span data-ttu-id="5887b-218">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5887b-218">Click **Create**.</span></span>
 
### <a name="creating-a-mypolicies-test-user"></a><span data-ttu-id="5887b-219">Vytvoření zkušebního uživatele myPolicies</span><span class="sxs-lookup"><span data-stu-id="5887b-219">Creating a myPolicies test user</span></span>

<span data-ttu-id="5887b-220">V této části vytvoříte volal Britta Simon v myPolicies uživatele.</span><span class="sxs-lookup"><span data-stu-id="5887b-220">In this section, you create a user called Britta Simon in myPolicies.</span></span> <span data-ttu-id="5887b-221">Práce s [tým podpory myPolicies](mailto:support@mypolicies.com) pro přidání uživatelů hello hello myPolicies platformy.</span><span class="sxs-lookup"><span data-stu-id="5887b-221">Work with [myPolicies support team](mailto:support@mypolicies.com) to add hello users in hello myPolicies platform.</span></span> <span data-ttu-id="5887b-222">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5887b-222">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5887b-223">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5887b-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5887b-224">V této části povolíte tak, že udělíte přístup toomyPolicies toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5887b-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access toomyPolicies.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5887b-226">**tooassign Britta Simon toomyPolicies, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5887b-226">**tooassign Britta Simon toomyPolicies, perform hello following steps:**</span></span>

1. <span data-ttu-id="5887b-227">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5887b-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5887b-229">V seznamu aplikace hello vyberte **myPolicies**.</span><span class="sxs-lookup"><span data-stu-id="5887b-229">In hello applications list, select **myPolicies**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mypolicies-tutorial/tutorial_mypolicies_app.png) 

3. <span data-ttu-id="5887b-231">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5887b-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5887b-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5887b-233">Click **Add** button.</span></span> <span data-ttu-id="5887b-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5887b-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5887b-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5887b-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5887b-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5887b-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5887b-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5887b-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5887b-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5887b-239">Testing single sign-on</span></span>

<span data-ttu-id="5887b-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5887b-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5887b-241">Po kliknutí na tlačítko hello myPolicies dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour myPolicies aplikace.</span><span class="sxs-lookup"><span data-stu-id="5887b-241">When you click hello myPolicies tile in hello Access Panel, you should get automatically signed-on tooyour myPolicies application.</span></span>
<span data-ttu-id="5887b-242">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5887b-242">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5887b-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5887b-243">Additional resources</span></span>

* [<span data-ttu-id="5887b-244">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5887b-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5887b-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5887b-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mypolicies-tutorial/tutorial_general_203.png

