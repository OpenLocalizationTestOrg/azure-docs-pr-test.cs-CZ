---
title: 'Kurz: Azure Active Directory integrace s Heroku | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Heroku."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d7d72ec6-4a60-4524-8634-26d8fbbcc833
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ee11db647fd385140f1dbcab2586dfafffe5d912
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-heroku"></a><span data-ttu-id="021a0-103">Kurz: Azure Active Directory integrace s Heroku</span><span class="sxs-lookup"><span data-stu-id="021a0-103">Tutorial: Azure Active Directory integration with Heroku</span></span>

<span data-ttu-id="021a0-104">V tomto kurzu zjistíte, jak toointegrate Heroku s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="021a0-104">In this tutorial, you learn how toointegrate Heroku with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="021a0-105">Integrace Heroku s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="021a0-105">Integrating Heroku with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="021a0-106">Můžete řídit ve službě Azure AD, který má přístup tooHeroku</span><span class="sxs-lookup"><span data-stu-id="021a0-106">You can control in Azure AD who has access tooHeroku</span></span>
- <span data-ttu-id="021a0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHeroku (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="021a0-107">You can enable your users tooautomatically get signed-on tooHeroku (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="021a0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="021a0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="021a0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="021a0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="021a0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="021a0-110">Prerequisites</span></span>

<span data-ttu-id="021a0-111">Integrace služby Azure AD s Heroku tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="021a0-111">tooconfigure Azure AD integration with Heroku, you need hello following items:</span></span>

- <span data-ttu-id="021a0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="021a0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="021a0-113">Heroku jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="021a0-113">A Heroku single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="021a0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="021a0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="021a0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="021a0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="021a0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="021a0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="021a0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="021a0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="021a0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="021a0-118">Scenario description</span></span>
<span data-ttu-id="021a0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="021a0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="021a0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="021a0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="021a0-121">Přidání Heroku z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="021a0-121">Adding Heroku from hello gallery</span></span>
2. <span data-ttu-id="021a0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="021a0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-heroku-from-hello-gallery"></a><span data-ttu-id="021a0-123">Přidání Heroku z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="021a0-123">Adding Heroku from hello gallery</span></span>
<span data-ttu-id="021a0-124">tooconfigure hello integrace Heroku do Azure AD, je nutné tooadd Heroku hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="021a0-124">tooconfigure hello integration of Heroku into Azure AD, you need tooadd Heroku from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="021a0-125">**tooadd Heroku z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="021a0-125">**tooadd Heroku from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="021a0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="021a0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="021a0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="021a0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="021a0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="021a0-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="021a0-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="021a0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="021a0-133">Hello vyhledávacího pole zadejte **Heroku**.</span><span class="sxs-lookup"><span data-stu-id="021a0-133">In hello search box, type **Heroku**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_search.png)

5. <span data-ttu-id="021a0-135">Na panelu výsledků hello vyberte **Heroku**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="021a0-135">In hello results panel, select **Heroku**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="021a0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="021a0-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="021a0-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Heroku podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="021a0-138">In this section, you configure and test Azure AD single sign-on with Heroku based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="021a0-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Heroku je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="021a0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Heroku is tooa user in Azure AD.</span></span> <span data-ttu-id="021a0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Heroku musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="021a0-140">In other words, a link relationship between an Azure AD user and hello related user in Heroku needs toobe established.</span></span>

<span data-ttu-id="021a0-141">V Heroku, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="021a0-141">In Heroku, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="021a0-142">tooconfigure a testu Azure AD jednotné přihlašování s Heroku, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="021a0-142">tooconfigure and test Azure AD single sign-on with Heroku, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="021a0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="021a0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="021a0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="021a0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="021a0-145">**[Vytvoření zkušebního uživatele Heroku](#creating-a-heroku-test-user)**  -toohave protějšek Britta Simon v Heroku, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="021a0-145">**[Creating a Heroku test user](#creating-a-heroku-test-user)** - toohave a counterpart of Britta Simon in Heroku that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="021a0-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="021a0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="021a0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="021a0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="021a0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="021a0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="021a0-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Heroku.</span><span class="sxs-lookup"><span data-stu-id="021a0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Heroku application.</span></span>

<span data-ttu-id="021a0-150">**tooconfigure Azure AD jednotné přihlašování s Heroku, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="021a0-150">**tooconfigure Azure AD single sign-on with Heroku, perform hello following steps:**</span></span>

1. <span data-ttu-id="021a0-151">V portálu Azure, na hello hello **Heroku** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="021a0-151">In hello Azure portal, on hello **Heroku** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="021a0-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="021a0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_samlbase.png)

3. <span data-ttu-id="021a0-155">Na hello **Heroku domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="021a0-155">On hello **Heroku Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_url.png)

    <span data-ttu-id="021a0-157">a.</span><span class="sxs-lookup"><span data-stu-id="021a0-157">a.</span></span> <span data-ttu-id="021a0-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="021a0-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>    
    `https://sso.heroku.com/saml/<company-name>/init`

    <span data-ttu-id="021a0-159">b.</span><span class="sxs-lookup"><span data-stu-id="021a0-159">b.</span></span> <span data-ttu-id="021a0-160">V hello **identifikátoru adresy URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="021a0-160">In hello **Identifier URL** textbox, type a URL using hello following pattern:</span></span>            
    `https://sso.heroku.com/saml/<company-name>`

    > [!NOTE]
    ><span data-ttu-id="021a0-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="021a0-161">These values are not real.</span></span> <span data-ttu-id="021a0-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="021a0-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="021a0-163">Tyto hodnoty můžete získat od Heroku týmu, který je popsán v pozdějších částech tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="021a0-163">You get these values from Heroku team, which is described in later sections of this article.</span></span> 
        
4. <span data-ttu-id="021a0-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="021a0-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_certificate.png) 

5. <span data-ttu-id="021a0-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="021a0-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="021a0-168">tooenable jednotné přihlašování v Heroku, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="021a0-168">tooenable SSO in Heroku, perform hello following steps:</span></span>
   
    <span data-ttu-id="021a0-169">a.</span><span class="sxs-lookup"><span data-stu-id="021a0-169">a.</span></span> <span data-ttu-id="021a0-170">Přihlaste se toohello Heroku účet jako správce.</span><span class="sxs-lookup"><span data-stu-id="021a0-170">Log in toohello Heroku account as an administrator.</span></span>

    <span data-ttu-id="021a0-171">b.</span><span class="sxs-lookup"><span data-stu-id="021a0-171">b.</span></span> <span data-ttu-id="021a0-172">Klikněte na tlačítko hello **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="021a0-172">Click hello **Settings** tab.</span></span>

    <span data-ttu-id="021a0-173">c.</span><span class="sxs-lookup"><span data-stu-id="021a0-173">c.</span></span> <span data-ttu-id="021a0-174">Na hello **jediné přihlášení na stránce**, klikněte na tlačítko **nahrát Metadata**.</span><span class="sxs-lookup"><span data-stu-id="021a0-174">On hello **Single Sign On Page**, click **Upload Metadata**.</span></span>

    <span data-ttu-id="021a0-175">d.</span><span class="sxs-lookup"><span data-stu-id="021a0-175">d.</span></span> <span data-ttu-id="021a0-176">Nahrajte hello metadata souboru, který jste si stáhli z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="021a0-176">Upload hello metadata file, which you have downloaded from hello Azure portal.</span></span>

    <span data-ttu-id="021a0-177">e.</span><span class="sxs-lookup"><span data-stu-id="021a0-177">e.</span></span> <span data-ttu-id="021a0-178">Po úspěšné instalaci hello správci najdete v potvrzovacím dialogovém okně a zobrazí se adresa URL hello hello jednotného přihlášení pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="021a0-178">When hello setup is successful, administrators see a confirmation dialog and hello URL of hello SSO Login for end users is displayed.</span></span> 

    <span data-ttu-id="021a0-179">f.</span><span class="sxs-lookup"><span data-stu-id="021a0-179">f.</span></span> <span data-ttu-id="021a0-180">Kopírování hello **Heroku přihlašovací adresa URL** a **Heroku Entity ID** hodnoty a přejděte zpátky příliš**Heroku domény a adresy URL** části na portálu Azure a vložte tyto hodnoty do hello **Přihlašovací adresa Url** a **identifikátor** textových polí v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="021a0-180">Copy hello **Heroku Login URL** and **Heroku Entity ID** values and go back too**Heroku Domain and URLs** section in Azure portal and paste these values into hello **Sign-On Url** and **Identifier** textboxes respectively.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_52.png) 
    
8. <span data-ttu-id="021a0-182">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="021a0-182">Click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="021a0-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="021a0-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="021a0-184">Po přidání této aplikace z hello **Active Directory podnikové aplikace** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="021a0-184">After adding this app from hello **Active Directory Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="021a0-185">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="021a0-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="021a0-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="021a0-186">Creating an Azure AD test user</span></span>

<span data-ttu-id="021a0-187">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="021a0-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="021a0-189">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="021a0-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="021a0-190">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="021a0-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="021a0-192">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="021a0-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="021a0-194">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="021a0-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="021a0-196">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="021a0-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="021a0-198">a.</span><span class="sxs-lookup"><span data-stu-id="021a0-198">a.</span></span> <span data-ttu-id="021a0-199">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="021a0-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="021a0-200">b.</span><span class="sxs-lookup"><span data-stu-id="021a0-200">b.</span></span> <span data-ttu-id="021a0-201">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="021a0-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="021a0-202">c.</span><span class="sxs-lookup"><span data-stu-id="021a0-202">c.</span></span> <span data-ttu-id="021a0-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="021a0-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="021a0-204">d.</span><span class="sxs-lookup"><span data-stu-id="021a0-204">d.</span></span> <span data-ttu-id="021a0-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="021a0-205">Click **Create**.</span></span>
 
### <a name="creating-a-heroku-test-user"></a><span data-ttu-id="021a0-206">Vytvoření zkušebního uživatele Heroku</span><span class="sxs-lookup"><span data-stu-id="021a0-206">Creating a Heroku test user</span></span>

<span data-ttu-id="021a0-207">V této části vytvoříte volal Britta Simon v Heroku uživatele.</span><span class="sxs-lookup"><span data-stu-id="021a0-207">In this section, you create a user called Britta Simon in Heroku.</span></span> <span data-ttu-id="021a0-208">Heroku podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="021a0-208">Heroku supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="021a0-209">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="021a0-209">There is no action item for you in this section.</span></span> <span data-ttu-id="021a0-210">Nový uživatel se vytvoří při přístupu k Heroku, pokud ještě neexistuje hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="021a0-210">A new user is created when accessing Heroku if hello user doesn't exist yet.</span></span> <span data-ttu-id="021a0-211">Po zřízení účtu hello hello koncový uživatel obdrží e-mail o ověření a musí propojit tooclick hello potvrzení.</span><span class="sxs-lookup"><span data-stu-id="021a0-211">After hello account is provisioned, hello end user receives a verification email and needs tooclick hello acknowledgement link.</span></span>

>[!NOTE]
><span data-ttu-id="021a0-212">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory Heroku klienta](https://www.heroku.com/support).</span><span class="sxs-lookup"><span data-stu-id="021a0-212">If you need toocreate a user manually, you need toocontact hello [Heroku Client support team](https://www.heroku.com/support).</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="021a0-213">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="021a0-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="021a0-214">V této části povolíte tak, že udělíte přístup tooHeroku toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="021a0-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHeroku.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="021a0-216">**tooassign Britta Simon tooHeroku, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="021a0-216">**tooassign Britta Simon tooHeroku, perform hello following steps:**</span></span>

1. <span data-ttu-id="021a0-217">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="021a0-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="021a0-219">V seznamu aplikace hello vyberte **Heroku**.</span><span class="sxs-lookup"><span data-stu-id="021a0-219">In hello applications list, select **Heroku**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_app.png) 

3. <span data-ttu-id="021a0-221">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="021a0-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="021a0-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="021a0-223">Click **Add** button.</span></span> <span data-ttu-id="021a0-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="021a0-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="021a0-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="021a0-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="021a0-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="021a0-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="021a0-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="021a0-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="021a0-229">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="021a0-229">Testing single sign-on</span></span>

<span data-ttu-id="021a0-230">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="021a0-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="021a0-231">Když kliknete na dlaždici Heroku hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Heroku aplikace.</span><span class="sxs-lookup"><span data-stu-id="021a0-231">When you click hello Heroku tile in hello Access Panel, you should get automatically signed-on tooyour Heroku application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="021a0-232">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="021a0-232">Additional resources</span></span>

* [<span data-ttu-id="021a0-233">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="021a0-233">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="021a0-234">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="021a0-234">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_203.png
