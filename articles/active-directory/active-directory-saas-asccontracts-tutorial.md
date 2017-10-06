---
title: 'Kurz: Azure Active Directory integrace se smlouvami ASC | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotného přihlašování služby Azure Active Directory a jiné smlouvy ASC."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7f54202-1581-4e55-a97e-02633ff9382d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: jeedes
ms.openlocfilehash: 8320af8acfda3e3d37e589c9887cd697d5ab651c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asc-contracts"></a><span data-ttu-id="b27f2-103">Kurz: Azure Active Directory integrace se smlouvami ASC</span><span class="sxs-lookup"><span data-stu-id="b27f2-103">Tutorial: Azure Active Directory integration with ASC Contracts</span></span>

<span data-ttu-id="b27f2-104">V tomto kurzu zjistíte, jak toointegrate ASC měnící se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b27f2-104">In this tutorial, you learn how toointegrate ASC Contracts with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b27f2-105">Integrace ASC kontrakty s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b27f2-105">Integrating ASC Contracts with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b27f2-106">Můžete řídit ve službě Azure AD, který má přístup tooASC kontrakty</span><span class="sxs-lookup"><span data-stu-id="b27f2-106">You can control in Azure AD who has access tooASC Contracts</span></span>
- <span data-ttu-id="b27f2-107">Můžete povolit tooautomatically vaši uživatelé získat přihlášeného tooASC kontrakty (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b27f2-107">You can enable your users tooautomatically get signed-on tooASC Contracts (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b27f2-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b27f2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b27f2-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b27f2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b27f2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b27f2-110">Prerequisites</span></span>

<span data-ttu-id="b27f2-111">tooconfigure integrace Azure AD s ASC kontrakty, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b27f2-111">tooconfigure Azure AD integration with ASC Contracts, you need hello following items:</span></span>

- <span data-ttu-id="b27f2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b27f2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b27f2-113">ASC kontrakty jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b27f2-113">An ASC Contracts single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b27f2-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b27f2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b27f2-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b27f2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b27f2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b27f2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b27f2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b27f2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b27f2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b27f2-118">Scenario description</span></span>
<span data-ttu-id="b27f2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b27f2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b27f2-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b27f2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b27f2-121">Přidání smluv ASC z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b27f2-121">Adding ASC Contracts from hello gallery</span></span>
2. <span data-ttu-id="b27f2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b27f2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asc-contracts-from-hello-gallery"></a><span data-ttu-id="b27f2-123">Přidání smluv ASC z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b27f2-123">Adding ASC Contracts from hello gallery</span></span>
<span data-ttu-id="b27f2-124">tooconfigure hello integrace ASC smluv do Azure AD, je nutné tooadd ASC kontrakty hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b27f2-124">tooconfigure hello integration of ASC Contracts into Azure AD, you need tooadd ASC Contracts from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b27f2-125">**tooadd ASC kontrakty z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b27f2-125">**tooadd ASC Contracts from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b27f2-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b27f2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b27f2-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b27f2-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b27f2-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b27f2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b27f2-133">Hello vyhledávacího pole zadejte **ASC kontrakty**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-133">In hello search box, type **ASC Contracts**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_search.png)

5. <span data-ttu-id="b27f2-135">Na panelu výsledků hello vyberte **ASC kontrakty**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b27f2-135">In hello results panel, select **ASC Contracts**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b27f2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b27f2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b27f2-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s kontrakty ASC podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b27f2-138">In this section, you configure and test Azure AD single sign-on with ASC Contracts based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b27f2-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v kontraktech ASC je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b27f2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ASC Contracts is tooa user in Azure AD.</span></span> <span data-ttu-id="b27f2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v kontraktech ASC musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b27f2-140">In other words, a link relationship between an Azure AD user and hello related user in ASC Contracts needs toobe established.</span></span>

<span data-ttu-id="b27f2-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v kontraktech ASC.</span><span class="sxs-lookup"><span data-stu-id="b27f2-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ASC Contracts.</span></span>

<span data-ttu-id="b27f2-142">tooconfigure a testu Azure AD jednotné přihlašování s ASC kontrakty, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b27f2-142">tooconfigure and test Azure AD single sign-on with ASC Contracts, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b27f2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b27f2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b27f2-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b27f2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b27f2-145">**[Vytváření testovacího uživatele ASC kontrakty](#creating-an-asc-contracts-test-user)**  -toohave protějšek Britta Simon v kontraktech ASC, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b27f2-145">**[Creating an ASC Contracts test user](#creating-an-asc-contracts-test-user)** - toohave a counterpart of Britta Simon in ASC Contracts that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b27f2-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b27f2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b27f2-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b27f2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b27f2-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b27f2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b27f2-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci kontrakty ASC.</span><span class="sxs-lookup"><span data-stu-id="b27f2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ASC Contracts application.</span></span>

<span data-ttu-id="b27f2-150">**tooconfigure Azure AD jednotné přihlašování s ASC kontrakty, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b27f2-150">**tooconfigure Azure AD single sign-on with ASC Contracts, perform hello following steps:**</span></span>

1. <span data-ttu-id="b27f2-151">V portálu Azure, na hello hello **ASC kontrakty** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-151">In hello Azure portal, on hello **ASC Contracts** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b27f2-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b27f2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_samlbase.png)

3. <span data-ttu-id="b27f2-155">Na hello **ASC kontrakty domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b27f2-155">On hello **ASC Contracts Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_url.png)

    <span data-ttu-id="b27f2-157">a.</span><span class="sxs-lookup"><span data-stu-id="b27f2-157">a.</span></span> <span data-ttu-id="b27f2-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.asccontracts.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="b27f2-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.asccontracts.com/shibboleth`</span></span>

    <span data-ttu-id="b27f2-159">b.</span><span class="sxs-lookup"><span data-stu-id="b27f2-159">b.</span></span> <span data-ttu-id="b27f2-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span><span class="sxs-lookup"><span data-stu-id="b27f2-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b27f2-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b27f2-161">These values are not real.</span></span> <span data-ttu-id="b27f2-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="b27f2-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="b27f2-163">Obraťte se na tým ASC sítě Inc. (ASC) na **613.599.6178** tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b27f2-163">Contact ASC Networks Inc. (ASC) team at **613.599.6178** tooget these values.</span></span>

4. <span data-ttu-id="b27f2-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b27f2-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_certificate.png) 

5. <span data-ttu-id="b27f2-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b27f2-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b27f2-168">tooconfigure jednotného přihlašování na **ASC kontrakty** straně, obraťte se na podporu ASC sítě Inc. (ASC) na **613.599.6178** a poskytnout hello Stáhnout **soubor XML s metadaty**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-168">tooconfigure single sign-on on **ASC Contracts** side, call ASC Networks Inc. (ASC) support at **613.599.6178** and provide them with hello downloaded **Metadata XML**.</span></span> <span data-ttu-id="b27f2-169">Nastavují tuto aplikaci toohave hello jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="b27f2-169">They set this application up toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="b27f2-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b27f2-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b27f2-171">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b27f2-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b27f2-172">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b27f2-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b27f2-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b27f2-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="b27f2-174">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b27f2-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b27f2-176">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b27f2-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b27f2-177">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b27f2-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b27f2-179">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b27f2-181">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b27f2-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b27f2-183">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b27f2-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b27f2-185">a.</span><span class="sxs-lookup"><span data-stu-id="b27f2-185">a.</span></span> <span data-ttu-id="b27f2-186">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b27f2-187">b.</span><span class="sxs-lookup"><span data-stu-id="b27f2-187">b.</span></span> <span data-ttu-id="b27f2-188">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b27f2-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b27f2-189">c.</span><span class="sxs-lookup"><span data-stu-id="b27f2-189">c.</span></span> <span data-ttu-id="b27f2-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b27f2-191">d.</span><span class="sxs-lookup"><span data-stu-id="b27f2-191">d.</span></span> <span data-ttu-id="b27f2-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-192">Click **Create**.</span></span>
 
### <a name="creating-an-asc-contracts-test-user"></a><span data-ttu-id="b27f2-193">Vytváření testovacího uživatele kontrakty ASC</span><span class="sxs-lookup"><span data-stu-id="b27f2-193">Creating an ASC Contracts test user</span></span>

<span data-ttu-id="b27f2-194">Práce s tým podpory ASC sítě Inc. (ASC) na **613.599.6178** tooget hello uživatelů přidaných v platformě ASC kontrakty hello.</span><span class="sxs-lookup"><span data-stu-id="b27f2-194">Work with ASC Networks Inc. (ASC) support team at **613.599.6178** tooget hello users added in hello ASC Contracts platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b27f2-195">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b27f2-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b27f2-196">V této části povolíte tak, že udělíte přístup kontrakty tooASC toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b27f2-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooASC Contracts.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b27f2-198">**tooassign Britta Simon tooASC kontrakty, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b27f2-198">**tooassign Britta Simon tooASC Contracts, perform hello following steps:**</span></span>

1. <span data-ttu-id="b27f2-199">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b27f2-201">V seznamu aplikace hello vyberte **ASC kontrakty**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-201">In hello applications list, select **ASC Contracts**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_app.png) 

3. <span data-ttu-id="b27f2-203">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b27f2-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b27f2-205">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b27f2-205">Click **Add** button.</span></span> <span data-ttu-id="b27f2-206">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b27f2-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b27f2-208">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b27f2-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b27f2-209">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b27f2-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b27f2-210">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b27f2-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b27f2-211">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b27f2-211">Testing single sign-on</span></span>

<span data-ttu-id="b27f2-212">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b27f2-212">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b27f2-213">Po kliknutí na tlačítko hello ASC kontrakty dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ASC kontrakty aplikace.</span><span class="sxs-lookup"><span data-stu-id="b27f2-213">When you click hello ASC Contracts tile in hello Access Panel, you should get automatically signed-on tooyour ASC Contracts application.</span></span> <span data-ttu-id="b27f2-214">Další informace o na přístupovém panelu najdete v tématu.</span><span class="sxs-lookup"><span data-stu-id="b27f2-214">For more information about the Access Panel, see.</span></span> <span data-ttu-id="b27f2-215">[Úvod do přístupového panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="b27f2-215">[Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b27f2-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b27f2-216">Additional resources</span></span>

* [<span data-ttu-id="b27f2-217">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b27f2-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b27f2-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b27f2-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_203.png

