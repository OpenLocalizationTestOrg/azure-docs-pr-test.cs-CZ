---
title: 'Kurz: Azure Active Directory integrace s Pingboard | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Pingboard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 0a916b1f9ef32d8124aa11284d2115bb4fc0bbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a><span data-ttu-id="b1958-103">Kurz: Azure Active Directory integrace s Pingboard</span><span class="sxs-lookup"><span data-stu-id="b1958-103">Tutorial: Azure Active Directory integration with Pingboard</span></span>

<span data-ttu-id="b1958-104">V tomto kurzu zjistíte, jak toointegrate Pingboard s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b1958-104">In this tutorial, you learn how toointegrate Pingboard with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b1958-105">Integrace Pingboard s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b1958-105">Integrating Pingboard with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b1958-106">Můžete řídit ve službě Azure AD, který má přístup tooPingboard</span><span class="sxs-lookup"><span data-stu-id="b1958-106">You can control in Azure AD who has access tooPingboard</span></span>
- <span data-ttu-id="b1958-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPingboard (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1958-107">You can enable your users tooautomatically get signed-on tooPingboard (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b1958-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="b1958-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="b1958-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b1958-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1958-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b1958-110">Prerequisites</span></span>

<span data-ttu-id="b1958-111">Integrace služby Azure AD s Pingboard tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b1958-111">tooconfigure Azure AD integration with Pingboard, you need hello following items:</span></span>

- <span data-ttu-id="b1958-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1958-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b1958-113">Pingboard jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b1958-113">A Pingboard single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b1958-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b1958-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b1958-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b1958-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b1958-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="b1958-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="b1958-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1958-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b1958-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b1958-118">Scenario description</span></span>
<span data-ttu-id="b1958-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b1958-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b1958-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b1958-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b1958-121">Přidání Pingboard z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b1958-121">Adding Pingboard from hello gallery</span></span>
2. <span data-ttu-id="b1958-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1958-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pingboard-from-hello-gallery"></a><span data-ttu-id="b1958-123">Přidání Pingboard z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b1958-123">Adding Pingboard from hello gallery</span></span>
<span data-ttu-id="b1958-124">tooconfigure hello integrace Pingboard do Azure AD, je nutné tooadd Pingboard hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b1958-124">tooconfigure hello integration of Pingboard into Azure AD, you need tooadd Pingboard from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b1958-125">**tooadd Pingboard z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1958-125">**tooadd Pingboard from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1958-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b1958-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b1958-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b1958-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b1958-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b1958-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b1958-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1958-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b1958-133">Hello vyhledávacího pole zadejte **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="b1958-133">In hello search box, type **Pingboard**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. <span data-ttu-id="b1958-135">Na panelu výsledků hello vyberte **Pingboard**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1958-135">In hello results panel, select **Pingboard**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b1958-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1958-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b1958-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pingboard podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b1958-138">In this section, you configure and test Azure AD single sign-on with Pingboard based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b1958-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Pingboard je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1958-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pingboard is tooa user in Azure AD.</span></span> <span data-ttu-id="b1958-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Pingboard musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b1958-140">In other words, a link relationship between an Azure AD user and hello related user in Pingboard needs toobe established.</span></span>

<span data-ttu-id="b1958-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Pingboard.</span><span class="sxs-lookup"><span data-stu-id="b1958-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Pingboard.</span></span>

<span data-ttu-id="b1958-142">tooconfigure a testu Azure AD jednotné přihlašování s Pingboard, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b1958-142">tooconfigure and test Azure AD single sign-on with Pingboard, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b1958-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b1958-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b1958-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1958-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b1958-145">**[Vytvoření zkušebního uživatele Pingboard](#creating-a-pingboard-test-user)**  -toohave protějšek Britta Simon v Pingboard, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1958-145">**[Creating a Pingboard test user](#creating-a-pingboard-test-user)** - toohave a counterpart of Britta Simon in Pingboard that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="b1958-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b1958-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b1958-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b1958-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b1958-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1958-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b1958-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Pingboard.</span><span class="sxs-lookup"><span data-stu-id="b1958-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Pingboard application.</span></span>

<span data-ttu-id="b1958-150">**tooconfigure Azure AD jednotné přihlašování s Pingboard, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1958-150">**tooconfigure Azure AD single sign-on with Pingboard, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1958-151">V hello Azure Management portal na hello **Pingboard** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b1958-151">In hello Azure Management portal, on hello **Pingboard** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b1958-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="b1958-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. <span data-ttu-id="b1958-155">Na hello **Pingboard domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="b1958-155">On hello **Pingboard Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    <span data-ttu-id="b1958-157">a.</span><span class="sxs-lookup"><span data-stu-id="b1958-157">a.</span></span> <span data-ttu-id="b1958-158">V hello **identifikátor** textovému poli, hodnota typu hello jako:`http://<entity-id>.pingboard.com/sp`</span><span class="sxs-lookup"><span data-stu-id="b1958-158">In hello **Identifier** textbox, type hello value as: `http://<entity-id>.pingboard.com/sp`</span></span>

    <span data-ttu-id="b1958-159">b.</span><span class="sxs-lookup"><span data-stu-id="b1958-159">b.</span></span> <span data-ttu-id="b1958-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<entity-id>.pingboard.com/auth/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="b1958-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<entity-id>.pingboard.com/auth/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b1958-161">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b1958-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="b1958-162">Máte tooupdate tyto hodnoty pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="b1958-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="b1958-163">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b1958-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="b1958-164">Obraťte se na [tým podpory Pingboard klienta](https://support.pingboard.com/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b1958-164">Contact [Pingboard Client support team](https://support.pingboard.com/) tooget these values.</span></span> 

4. <span data-ttu-id="b1958-165">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="b1958-165">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    <span data-ttu-id="b1958-167">a.</span><span class="sxs-lookup"><span data-stu-id="b1958-167">a.</span></span> <span data-ttu-id="b1958-168">V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`http://<sub-domain>.pingboard.com/sign_in`</span><span class="sxs-lookup"><span data-stu-id="b1958-168">In hello **Sign-on URL** textbox, type hello value as: `http://<sub-domain>.pingboard.com/sign_in`</span></span>
     
5. <span data-ttu-id="b1958-169">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b1958-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. <span data-ttu-id="b1958-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1958-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="b1958-173">tooconfigure jednotného přihlašování na straně Pingboard, otevřete nové okno prohlížeče a tooyour Pingboard účet pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b1958-173">tooconfigure SSO on Pingboard side, open a new browser window and log in tooyour Pingboard Account.</span></span> <span data-ttu-id="b1958-174">Správce tooset Pingboard si jednotné přihlašování musí být na.</span><span class="sxs-lookup"><span data-stu-id="b1958-174">You must be a Pingboard admin tooset up single sign on.</span></span>

8. <span data-ttu-id="b1958-175">Hello horní nabídce vyberte **aplikace > integrace**</span><span class="sxs-lookup"><span data-stu-id="b1958-175">From hello top menu select **Apps > Integrations**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  <span data-ttu-id="b1958-177">Na hello **integrace** stránky, vyhledejte hello **"Azure Active Directory"** dlaždici a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="b1958-177">On hello **Integrations** page, find hello **"Azure Active Directory"** tile, and click it.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. <span data-ttu-id="b1958-179">V hello modální, který následuje dále klikněte na tlačítko **"Konfigurace"**</span><span class="sxs-lookup"><span data-stu-id="b1958-179">In hello modal that follows click **"Configure"**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. <span data-ttu-id="b1958-181">Na následující stránku hello si všimnete, že "Integrace se službou Azure jednotného přihlašování k dispozici.".</span><span class="sxs-lookup"><span data-stu-id="b1958-181">On hello following page, you will notice that "Azure SSO Integration is enabled.".</span></span> <span data-ttu-id="b1958-182">Otevřete hello stáhli soubor soubor XML s metadaty v programu Poznámkový blok a vložit hello obsahu v **IDP Metadata**.</span><span class="sxs-lookup"><span data-stu-id="b1958-182">Open hello downloaded Metadata XML file in a notepad and paste hello content in **IDP Metadata**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. <span data-ttu-id="b1958-184">ověří Hello souboru, a pokud se vše, co je správná, bude nyní povoleno jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1958-184">hello file will be validated, and if everything is correct, single sign on will now be enabled</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b1958-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1958-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="b1958-186">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1958-186">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b1958-188">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1958-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1958-189">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b1958-189">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b1958-191">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b1958-191">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b1958-193">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1958-193">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b1958-195">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b1958-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b1958-197">a.</span><span class="sxs-lookup"><span data-stu-id="b1958-197">a.</span></span> <span data-ttu-id="b1958-198">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b1958-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b1958-199">b.</span><span class="sxs-lookup"><span data-stu-id="b1958-199">b.</span></span> <span data-ttu-id="b1958-200">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b1958-200">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b1958-201">c.</span><span class="sxs-lookup"><span data-stu-id="b1958-201">c.</span></span> <span data-ttu-id="b1958-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b1958-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b1958-203">d.</span><span class="sxs-lookup"><span data-stu-id="b1958-203">d.</span></span> <span data-ttu-id="b1958-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b1958-204">Click **Create**.</span></span>
 
### <a name="creating-a-pingboard-test-user"></a><span data-ttu-id="b1958-205">Vytvoření zkušebního uživatele Pingboard</span><span class="sxs-lookup"><span data-stu-id="b1958-205">Creating a Pingboard test user</span></span>

<span data-ttu-id="b1958-206">V pořadí tooenable Azure AD Uživatelé toolog do Pingboard musí být zřízená do Pingboard.</span><span class="sxs-lookup"><span data-stu-id="b1958-206">In order tooenable Azure AD users toolog into Pingboard, they must be provisioned into Pingboard.</span></span>  
<span data-ttu-id="b1958-207">V případě hello Pingboard zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b1958-207">In hello case of Pingboard, provisioning is a manual task.</span></span>

<span data-ttu-id="b1958-208">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b1958-208">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1958-209">Přihlaste se tooyour Pingboard společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b1958-209">Log in tooyour Pingboard company site as an administrator.</span></span>

2. <span data-ttu-id="b1958-210">Klikněte na tlačítko **"Přidat zaměstnancem"** tlačítko **Directory** stránky.</span><span class="sxs-lookup"><span data-stu-id="b1958-210">Click **“Add Employee”** button on **Directory** page.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. <span data-ttu-id="b1958-212">Na hello **"Přidat zaměstnancem"** dialogové okno proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="b1958-212">On hello **“Add Employee”** dialog page, perform hello following steps.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    <span data-ttu-id="b1958-214">a.</span><span class="sxs-lookup"><span data-stu-id="b1958-214">a.</span></span> <span data-ttu-id="b1958-215">V hello **úplný název** textovému poli, hello úplný název typu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1958-215">In hello **Full Name** textbox, type hello full name of Britta Simon.</span></span>

    <span data-ttu-id="b1958-216">b.</span><span class="sxs-lookup"><span data-stu-id="b1958-216">b.</span></span> <span data-ttu-id="b1958-217">V hello **e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1958-217">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="b1958-218">c.</span><span class="sxs-lookup"><span data-stu-id="b1958-218">c.</span></span> <span data-ttu-id="b1958-219">V hello **funkce** textovému poli, typ hello pozici Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1958-219">In hello **Job Title** textbox, type hello job title of Britta Simon.</span></span>

    <span data-ttu-id="b1958-220">d.</span><span class="sxs-lookup"><span data-stu-id="b1958-220">d.</span></span> <span data-ttu-id="b1958-221">V hello **umístění** rozevíracího seznamu, vyberte hello umístění Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1958-221">In hello **Location** dropdown, select hello location  of Britta Simon.</span></span>
    
    <span data-ttu-id="b1958-222">e.</span><span class="sxs-lookup"><span data-stu-id="b1958-222">e.</span></span> <span data-ttu-id="b1958-223">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="b1958-223">Click **Add**.</span></span>   

4. <span data-ttu-id="b1958-224">Obrazovka s potvrzením bude spuštěna tooconfirm hello přidání uživatele.</span><span class="sxs-lookup"><span data-stu-id="b1958-224">A confirmation screen will come up tooconfirm hello addition of user.</span></span>
    
    ![Potvrďte](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > <span data-ttu-id="b1958-226">Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="b1958-226">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b1958-227">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b1958-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b1958-228">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooPingboard svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="b1958-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooPingboard.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b1958-230">**tooassign Britta Simon tooPingboard, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1958-230">**tooassign Britta Simon tooPingboard, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1958-231">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b1958-231">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b1958-233">V seznamu aplikace hello vyberte **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="b1958-233">In hello applications list, select **Pingboard**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. <span data-ttu-id="b1958-235">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b1958-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b1958-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1958-237">Click **Add** button.</span></span> <span data-ttu-id="b1958-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1958-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b1958-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b1958-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b1958-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1958-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b1958-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1958-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b1958-243">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1958-243">Testing single sign-on</span></span>

<span data-ttu-id="b1958-244">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b1958-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b1958-245">Když kliknete na dlaždici Pingboard hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Pingboard aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1958-245">When you click hello Pingboard tile in hello Access Panel, you should get automatically signed-on tooyour Pingboard application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1958-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b1958-246">Additional resources</span></span>

* [<span data-ttu-id="b1958-247">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b1958-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b1958-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b1958-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
