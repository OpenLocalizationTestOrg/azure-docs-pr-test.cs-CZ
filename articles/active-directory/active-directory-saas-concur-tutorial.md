---
title: 'Kurz: Azure Active Directory integrace s Concur | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Concur."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1eee0a5d-24fa-4986-9aef-3c543cfe3296
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 1012bf8c6f036306d0ca90689415d5e22e449989
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-concur"></a><span data-ttu-id="2c449-103">Kurz: Azure Active Directory integrace s Concur</span><span class="sxs-lookup"><span data-stu-id="2c449-103">Tutorial: Azure Active Directory integration with Concur</span></span>

<span data-ttu-id="2c449-104">V tomto kurzu zjistíte, jak toointegrate souhlasily se Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2c449-104">In this tutorial, you learn how toointegrate Concur with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2c449-105">Integrace Concur s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2c449-105">Integrating Concur with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2c449-106">Můžete řídit ve službě Azure AD, který má přístup tooConcur</span><span class="sxs-lookup"><span data-stu-id="2c449-106">You can control in Azure AD who has access tooConcur</span></span>
- <span data-ttu-id="2c449-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooConcur (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c449-107">You can enable your users tooautomatically get signed-on tooConcur (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2c449-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2c449-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2c449-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2c449-109">If you want tooknow more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c449-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2c449-110">Prerequisites</span></span>

<span data-ttu-id="2c449-111">Integrace služby Azure AD s Concur tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2c449-111">tooconfigure Azure AD integration with Concur, you need hello following items:</span></span>

- <span data-ttu-id="2c449-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c449-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2c449-113">Concur jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2c449-113">A Concur single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2c449-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c449-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2c449-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2c449-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2c449-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2c449-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2c449-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2c449-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2c449-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2c449-118">Scenario description</span></span>
<span data-ttu-id="2c449-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c449-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2c449-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2c449-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2c449-121">Přidání Concur z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2c449-121">Adding Concur from hello gallery</span></span>
2. <span data-ttu-id="2c449-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c449-122">Configuring and testing Azure AD single sign-on</span></span>

>[!NOTE]
><span data-ttu-id="2c449-123">Konfigurace Hello Concur předplatného pro federované jednotné přihlašování pomocí SAML je samostatná úloha, která bude nutné se obrátit [tým podpory vyústit klienta](https://www.concur.co.in/contact) tooperform.</span><span class="sxs-lookup"><span data-stu-id="2c449-123">hello configuration of your Concur subscription for federated SSO via SAML is a separate task, which you must contact [Concur Client support team](https://www.concur.co.in/contact) tooperform.</span></span> 

## <a name="adding-concur-from-hello-gallery"></a><span data-ttu-id="2c449-124">Přidání Concur z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2c449-124">Adding Concur from hello gallery</span></span>
<span data-ttu-id="2c449-125">tooconfigure hello integrace Concur do Azure AD, je nutné tooadd Concur hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2c449-125">tooconfigure hello integration of Concur into Azure AD, you need tooadd Concur from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2c449-126">**tooadd Concur z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c449-126">**tooadd Concur from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c449-127">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2c449-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2c449-129">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2c449-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2c449-130">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2c449-130">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2c449-132">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c449-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2c449-134">Hello vyhledávacího pole zadejte **Concur**.</span><span class="sxs-lookup"><span data-stu-id="2c449-134">In hello search box, type **Concur**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/tutorial_concur_search.png)

5. <span data-ttu-id="2c449-136">Na panelu výsledků hello vyberte **Concur**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c449-136">In hello results panel, select **Concur**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/tutorial_concur_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2c449-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c449-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2c449-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Concur podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="2c449-139">In this section, you configure and test Azure AD single sign-on with Concur based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2c449-140">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Concur je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c449-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Concur is tooa user in Azure AD.</span></span> <span data-ttu-id="2c449-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Concur musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2c449-141">In other words, a link relationship between an Azure AD user and hello related user in Concur needs toobe established.</span></span>

<span data-ttu-id="2c449-142">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Concur.</span><span class="sxs-lookup"><span data-stu-id="2c449-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Concur.</span></span>

<span data-ttu-id="2c449-143">tooconfigure a testu Azure AD jednotné přihlašování s Concur, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2c449-143">tooconfigure and test Azure AD single sign-on with Concur, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2c449-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2c449-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2c449-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2c449-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2c449-146">**[Vytvoření zkušebního uživatele Concur](#creating-a-concur-test-user)**  -toohave protějšek Britta Simon v Concur, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c449-146">**[Creating a Concur test user](#creating-a-concur-test-user)** - toohave a counterpart of Britta Simon in Concur that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2c449-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c449-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2c449-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2c449-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2c449-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c449-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2c449-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Concur.</span><span class="sxs-lookup"><span data-stu-id="2c449-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Concur application.</span></span>

<span data-ttu-id="2c449-151">**tooconfigure Azure AD jednotné přihlašování s Concur, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c449-151">**tooconfigure Azure AD single sign-on with Concur, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c449-152">V portálu Azure, na hello hello **Concur** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2c449-152">In hello Azure portal, on hello **Concur** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2c449-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c449-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_concur_samlbase.png)

3. <span data-ttu-id="2c449-156">Na hello **vyústit domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2c449-156">On hello **Concur Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_concur_url.png)

    <span data-ttu-id="2c449-158">a.</span><span class="sxs-lookup"><span data-stu-id="2c449-158">a.</span></span> <span data-ttu-id="2c449-159">V hello **přihlásit na adrese URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://www.concursolutions.com/UI/SSO/<OrganizationId>`</span><span class="sxs-lookup"><span data-stu-id="2c449-159">In hello **Sign on URL** textbox, type hello value using hello following pattern: `https://www.concursolutions.com/UI/SSO/<OrganizationId>`</span></span>

    <span data-ttu-id="2c449-160">b.</span><span class="sxs-lookup"><span data-stu-id="2c449-160">b.</span></span> <span data-ttu-id="2c449-161">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<customer-domain>.concursolutions.com`</span><span class="sxs-lookup"><span data-stu-id="2c449-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<customer-domain>.concursolutions.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2c449-162">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="2c449-162">These values are not hello real.</span></span> <span data-ttu-id="2c449-163">Tyto hodnoty s hello skutečné přihlášení adresy URL a identifikátor aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2c449-163">Update these values with hello actual Sign on URL and Identifier.</span></span> <span data-ttu-id="2c449-164">Obraťte se na [tým podpory vyústit klienta](https://www.concur.co.in/contact) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2c449-164">Contact [Concur Client support team](https://www.concur.co.in/contact) tooget these values.</span></span> 

4. <span data-ttu-id="2c449-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2c449-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_concur_certificate.png) 

5. <span data-ttu-id="2c449-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c449-167">Click **Save** button.</span></span>

    <span data-ttu-id="2c449-168">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="2c449-168">![Configure Single Sign-On](./media/active-directory-saas-concur-tutorial/tutorial_general_400.png)
<CS></span></span>

6. <span data-ttu-id="2c449-169">tooconfigure jednotného přihlašování na **Concur** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** tooConcur podpory.</span><span class="sxs-lookup"><span data-stu-id="2c449-169">tooconfigure single sign-on on **Concur** side, you need toosend hello downloaded **Metadata XML** tooConcur support.</span></span> <span data-ttu-id="2c449-170">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="2c449-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

  >[!NOTE]
  ><span data-ttu-id="2c449-171">Konfigurace Hello Concur předplatného pro federované jednotné přihlašování pomocí SAML je samostatná úloha, která bude nutné se obrátit [tým podpory vyústit klienta](https://www.concur.co.in/contact) tooperform.</span><span class="sxs-lookup"><span data-stu-id="2c449-171">hello configuration of your Concur subscription for federated SSO via SAML is a separate task, which you must contact [Concur Client support team](https://www.concur.co.in/contact) tooperform.</span></span> 
  
<CE>

> [!TIP]
> <span data-ttu-id="2c449-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="2c449-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2c449-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="2c449-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2c449-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2c449-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2c449-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c449-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="2c449-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2c449-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2c449-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c449-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c449-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2c449-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2c449-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2c449-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2c449-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="2c449-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2c449-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2c449-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2c449-187">a.</span><span class="sxs-lookup"><span data-stu-id="2c449-187">a.</span></span> <span data-ttu-id="2c449-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2c449-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2c449-189">b.</span><span class="sxs-lookup"><span data-stu-id="2c449-189">b.</span></span> <span data-ttu-id="2c449-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2c449-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2c449-191">c.</span><span class="sxs-lookup"><span data-stu-id="2c449-191">c.</span></span> <span data-ttu-id="2c449-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2c449-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2c449-193">d.</span><span class="sxs-lookup"><span data-stu-id="2c449-193">d.</span></span> <span data-ttu-id="2c449-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2c449-194">Click **Create**.</span></span>
 
### <a name="creating-a-concur-test-user"></a><span data-ttu-id="2c449-195">Vytvoření zkušebního uživatele Concur</span><span class="sxs-lookup"><span data-stu-id="2c449-195">Creating a Concur test user</span></span>

<span data-ttu-id="2c449-196">Aplikace podporuje hello pouze v době zřizování uživatelů a po ověření uživatele v aplikaci hello automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="2c449-196">Application supports hello Just in time user provisioning and after authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2c449-197">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2c449-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2c449-198">V této části povolíte tak, že udělíte přístup tooConcur toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c449-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooConcur.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2c449-200">**tooassign Britta Simon tooConcur, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c449-200">**tooassign Britta Simon tooConcur, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c449-201">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2c449-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2c449-203">V seznamu aplikace hello vyberte **Concur**.</span><span class="sxs-lookup"><span data-stu-id="2c449-203">In hello applications list, select **Concur**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-concur-tutorial/tutorial_concur_app.png) 

3. <span data-ttu-id="2c449-205">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2c449-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2c449-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c449-207">Click **Add** button.</span></span> <span data-ttu-id="2c449-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c449-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2c449-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2c449-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2c449-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c449-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2c449-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c449-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2c449-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c449-213">Testing single sign-on</span></span>

<span data-ttu-id="2c449-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2c449-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2c449-215">Když kliknete na dlaždici Concur hello v hello přístupového panelu, měli byste obdržet přihlašovací stránku Concur aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c449-215">When you click hello Concur tile in hello Access Panel, you should get login page of Concur application.</span></span>
<span data-ttu-id="2c449-216">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2c449-216">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2c449-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2c449-217">Additional resources</span></span>

* [<span data-ttu-id="2c449-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2c449-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2c449-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2c449-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="2c449-220">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="2c449-220">Configure User Provisioning</span></span>](active-directory-saas-concur-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-concur-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-concur-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-concur-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-concur-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-concur-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-concur-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-concur-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-concur-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-concur-tutorial/tutorial_general_203.png

