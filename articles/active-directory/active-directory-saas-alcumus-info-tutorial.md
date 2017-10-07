---
title: "Kurz: Azure Active Directory integrace s Alcumus údaje Exchange | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Alcumus údaje Exchange."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d26034b8-f0d5-4f65-aa56-0fc168ceec8c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 4ef9f4d654b6c451db44f929bdad1016304168b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a><span data-ttu-id="a8792-103">Kurz: Azure Active Directory integrace s Alcumus údaje Exchange</span><span class="sxs-lookup"><span data-stu-id="a8792-103">Tutorial: Azure Active Directory integration with Alcumus Info Exchange</span></span>

<span data-ttu-id="a8792-104">V tomto kurzu zjistíte, jak toointegrate Alcumus informace o výměně s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a8792-104">In this tutorial, you learn how toointegrate Alcumus Info Exchange with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a8792-105">Integrace Alcumus údaje Exchange s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a8792-105">Integrating Alcumus Info Exchange with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a8792-106">Můžete řídit ve službě Azure AD, který má přístup tooAlcumus údaje Exchange</span><span class="sxs-lookup"><span data-stu-id="a8792-106">You can control in Azure AD who has access tooAlcumus Info Exchange</span></span>
- <span data-ttu-id="a8792-107">Vaši uživatelé tooautomatically get přihlášeného tooAlcumus údaje Exchange (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8792-107">You can enable your users tooautomatically get signed-on tooAlcumus Info Exchange (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a8792-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a8792-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a8792-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a8792-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8792-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a8792-110">Prerequisites</span></span>

<span data-ttu-id="a8792-111">tooconfigure integrace Azure AD s Alcumus údaje Exchange, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a8792-111">tooconfigure Azure AD integration with Alcumus Info Exchange, you need hello following items:</span></span>

- <span data-ttu-id="a8792-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8792-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a8792-113">Alcumus údaje Exchange jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a8792-113">An Alcumus Info Exchange single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a8792-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a8792-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a8792-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a8792-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a8792-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a8792-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a8792-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8792-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a8792-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a8792-118">Scenario description</span></span>
<span data-ttu-id="a8792-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a8792-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a8792-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a8792-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a8792-121">Přidání Alcumus údaje Exchange z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a8792-121">Adding Alcumus Info Exchange from hello gallery</span></span>
2. <span data-ttu-id="a8792-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a8792-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-alcumus-info-exchange-from-hello-gallery"></a><span data-ttu-id="a8792-123">Přidání Alcumus údaje Exchange z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a8792-123">Adding Alcumus Info Exchange from hello gallery</span></span>
<span data-ttu-id="a8792-124">tooconfigure hello integrace systému Exchange Alcumus informace do služby Azure AD, je nutné tooadd Alcumus údaje Exchange hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a8792-124">tooconfigure hello integration of Alcumus Info Exchange into Azure AD, you need tooadd Alcumus Info Exchange from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a8792-125">**tooadd Alcumus Exchange informace z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a8792-125">**tooadd Alcumus Info Exchange from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8792-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a8792-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a8792-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a8792-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a8792-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a8792-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a8792-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a8792-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a8792-133">Hello vyhledávacího pole zadejte **Alcumus údaje Exchange**.</span><span class="sxs-lookup"><span data-stu-id="a8792-133">In hello search box, type **Alcumus Info Exchange**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_search.png)

5. <span data-ttu-id="a8792-135">Na panelu výsledků hello vyberte **Alcumus údaje Exchange**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a8792-135">In hello results panel, select **Alcumus Info Exchange**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a8792-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a8792-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a8792-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování se serverem Exchange Alcumus informace podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="a8792-138">In this section, you configure and test Azure AD single sign-on with Alcumus Info Exchange based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a8792-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Alcumus údaje Exchange je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8792-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Alcumus Info Exchange is tooa user in Azure AD.</span></span> <span data-ttu-id="a8792-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Alcumus údaje Exchange potřebuje toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="a8792-140">In other words, a link relationship between an Azure AD user and hello related user in Alcumus Info Exchange needs toobe established.</span></span>

<span data-ttu-id="a8792-141">V systému Exchange Alcumus informace, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="a8792-141">In Alcumus Info Exchange, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a8792-142">tooconfigure a testu Azure AD jednotné přihlašování s Alcumus údaje Exchange, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="a8792-142">tooconfigure and test Azure AD single sign-on with Alcumus Info Exchange, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a8792-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a8792-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a8792-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8792-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a8792-145">**[Vytváření testovacího uživatele Exchange informace Alcumus](#creating-an-alcumus-info-exchange-test-user)**  -toohave protějšek Britta Simon v systému Exchange Alcumus informace, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="a8792-145">**[Creating an Alcumus Info Exchange test user](#creating-an-alcumus-info-exchange-test-user)** - toohave a counterpart of Britta Simon in Alcumus Info Exchange that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a8792-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a8792-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a8792-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="a8792-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a8792-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a8792-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a8792-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Alcumus údaje Exchange.</span><span class="sxs-lookup"><span data-stu-id="a8792-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Alcumus Info Exchange application.</span></span>

<span data-ttu-id="a8792-150">**tooconfigure Azure AD jednotné přihlašování s Alcumus informace o systému Exchange, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a8792-150">**tooconfigure Azure AD single sign-on with Alcumus Info Exchange, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8792-151">V portálu Azure, na hello hello **Alcumus údaje Exchange** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a8792-151">In hello Azure portal, on hello **Alcumus Info Exchange** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a8792-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a8792-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_samlbase.png)

3. <span data-ttu-id="a8792-155">Na hello **Alcumus údaje Exchange domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a8792-155">On hello **Alcumus Info Exchange Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_url.png)

    <span data-ttu-id="a8792-157">a.</span><span class="sxs-lookup"><span data-stu-id="a8792-157">a.</span></span> <span data-ttu-id="a8792-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.info-exchange.com`</span><span class="sxs-lookup"><span data-stu-id="a8792-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.info-exchange.com`</span></span>

    <span data-ttu-id="a8792-159">b.</span><span class="sxs-lookup"><span data-stu-id="a8792-159">b.</span></span> <span data-ttu-id="a8792-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.info-exchange.com/Auth/`</span><span class="sxs-lookup"><span data-stu-id="a8792-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.info-exchange.com/Auth/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a8792-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a8792-161">These values are not real.</span></span> <span data-ttu-id="a8792-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="a8792-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="a8792-163">Obraťte se na [tým podpory Alcumus údaje Exchange](mailto:helpdesk@alcumusgroup.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a8792-163">Contact [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com) tooget these values.</span></span>
 
4. <span data-ttu-id="a8792-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a8792-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_certificate.png) 

5. <span data-ttu-id="a8792-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a8792-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a8792-168">tooconfigure jednotného přihlašování na **Alcumus údaje Exchange** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Alcumus údaje Exchange](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="a8792-168">tooconfigure single sign-on on **Alcumus Info Exchange** side, you need toosend hello downloaded **Metadata XML** too[Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

> [!TIP]
> <span data-ttu-id="a8792-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="a8792-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a8792-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="a8792-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a8792-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a8792-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a8792-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8792-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="a8792-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="a8792-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a8792-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a8792-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8792-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a8792-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a8792-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a8792-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a8792-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="a8792-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a8792-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a8792-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a8792-184">a.</span><span class="sxs-lookup"><span data-stu-id="a8792-184">a.</span></span> <span data-ttu-id="a8792-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a8792-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a8792-186">b.</span><span class="sxs-lookup"><span data-stu-id="a8792-186">b.</span></span> <span data-ttu-id="a8792-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a8792-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a8792-188">c.</span><span class="sxs-lookup"><span data-stu-id="a8792-188">c.</span></span> <span data-ttu-id="a8792-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a8792-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a8792-190">d.</span><span class="sxs-lookup"><span data-stu-id="a8792-190">d.</span></span> <span data-ttu-id="a8792-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a8792-191">Click **Create**.</span></span>
 
### <a name="creating-an-alcumus-info-exchange-test-user"></a><span data-ttu-id="a8792-192">Vytvoření Exchange Alcumus údaje testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a8792-192">Creating an Alcumus Info Exchange test user</span></span>

<span data-ttu-id="a8792-193">Hello cílem této části je toocreate volal Britta Simon v Alcumus údaje Exchange uživatele.</span><span class="sxs-lookup"><span data-stu-id="a8792-193">hello objective of this section is toocreate a user called Britta Simon in Alcumus Info Exchange.</span></span>

<span data-ttu-id="a8792-194">toocreate uživatel volal Britta Simon v Alcumus údaje Exchange, obraťte se na hello [tým podpory Alcumus údaje Exchange](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="a8792-194">toocreate a user called Britta Simon in Alcumus Info Exchange, Contact hello [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a8792-195">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a8792-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a8792-196">V této části povolíte tak, že udělíte přístup tooAlcumus údaje Exchange toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a8792-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAlcumus Info Exchange.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a8792-198">**tooassign Britta Simon tooAlcumus informace o systému Exchange, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a8792-198">**tooassign Britta Simon tooAlcumus Info Exchange, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8792-199">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a8792-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a8792-201">V seznamu aplikace hello vyberte **Alcumus údaje Exchange**.</span><span class="sxs-lookup"><span data-stu-id="a8792-201">In hello applications list, select **Alcumus Info Exchange**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_app.png) 

3. <span data-ttu-id="a8792-203">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a8792-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a8792-205">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a8792-205">Click **Add** button.</span></span> <span data-ttu-id="a8792-206">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8792-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a8792-208">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="a8792-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a8792-209">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8792-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a8792-210">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8792-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a8792-211">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a8792-211">Testing single sign-on</span></span>

<span data-ttu-id="a8792-212">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a8792-212">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="a8792-213">Když kliknete na dlaždici Alcumus údaje Exchange hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Exchange Alcumus informace o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a8792-213">When you click hello Alcumus Info Exchange tile in hello Access Panel, you should get automatically signed-on tooyour Alcumus Info Exchange application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8792-214">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a8792-214">Additional resources</span></span>

* [<span data-ttu-id="a8792-215">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8792-215">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a8792-216">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a8792-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png

