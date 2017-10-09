---
title: 'Kurz: Azure Active Directory integrace s T & E Express | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a T & E Express."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.openlocfilehash: 9a568ace8dbc75fadbf37554996b1b597a813d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a><span data-ttu-id="b1214-103">Kurz: Azure Active Directory integrace s T & E Express</span><span class="sxs-lookup"><span data-stu-id="b1214-103">Tutorial: Azure Active Directory integration with T&E Express</span></span>

<span data-ttu-id="b1214-104">V tomto kurzu zjistíte, jak toointegrate T & E Express s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b1214-104">In this tutorial, you learn how toointegrate T&E Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b1214-105">N & E Express integraci s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b1214-105">Integrating T&E Express with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b1214-106">Můžete řídit v Azure AD, který má přístup Tool & E Express</span><span class="sxs-lookup"><span data-stu-id="b1214-106">You can control in Azure AD who has access tooT&E Express</span></span>
- <span data-ttu-id="b1214-107">Můžete povolit uživatelům tooautomatically get přihlášeného Tool & E Express (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1214-107">You can enable your users tooautomatically get signed-on tooT&E Express (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b1214-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="b1214-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="b1214-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b1214-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1214-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b1214-110">Prerequisites</span></span>

<span data-ttu-id="b1214-111">tooconfigure integrace Azure AD s T & E Express, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b1214-111">tooconfigure Azure AD integration with T&E Express, you need hello following items:</span></span>

- <span data-ttu-id="b1214-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1214-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b1214-113">T & E Express jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b1214-113">A T&E Express single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b1214-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b1214-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b1214-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b1214-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b1214-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="b1214-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="b1214-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1214-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b1214-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b1214-118">Scenario description</span></span>
<span data-ttu-id="b1214-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b1214-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b1214-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b1214-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b1214-121">Přidání T & E Express z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b1214-121">Adding T&E Express from hello gallery</span></span>
2. <span data-ttu-id="b1214-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1214-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-te-express-from-hello-gallery"></a><span data-ttu-id="b1214-123">Přidání T & E Express z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b1214-123">Adding T&E Express from hello gallery</span></span>
<span data-ttu-id="b1214-124">tooconfigure hello integrace T & E Express do Azure AD, je nutné tooadd T & E Express hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b1214-124">tooconfigure hello integration of T&E Express into Azure AD, you need tooadd T&E Express from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b1214-125">**tooadd T & E Express z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1214-125">**tooadd T&E Express from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1214-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b1214-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b1214-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b1214-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b1214-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b1214-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b1214-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1214-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b1214-133">Hello vyhledávacího pole zadejte **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="b1214-133">In hello search box, type **T&E Express**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_search.png)

5. <span data-ttu-id="b1214-135">Na panelu výsledků hello vyberte **T & E Express**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1214-135">In hello results panel, select **T&E Express**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b1214-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1214-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b1214-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s T & E Express podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b1214-138">In this section, you configure and test Azure AD single sign-on with T&E Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b1214-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v T & E Express je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1214-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in T&E Express is tooa user in Azure AD.</span></span> <span data-ttu-id="b1214-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v T & E Express toobe musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b1214-140">In other words, a link relationship between an Azure AD user and hello related user in T&E Express needs toobe established.</span></span>

<span data-ttu-id="b1214-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v T & E Express.</span><span class="sxs-lookup"><span data-stu-id="b1214-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in T&E Express.</span></span>

<span data-ttu-id="b1214-142">tooconfigure a testu Azure AD jednotné přihlašování s T & E Express, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="b1214-142">tooconfigure and test Azure AD single sign-on with T&E Express, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b1214-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b1214-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b1214-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1214-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b1214-145">**[Vytvoření zkušebního uživatele T & E Express](#creating-a-te-express-test-user)**  -toohave protějšek Britta Simon v T & E Express, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1214-145">**[Creating a T&E Express test user](#creating-a-te-express-test-user)** - toohave a counterpart of Britta Simon in T&E Express that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="b1214-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b1214-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b1214-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b1214-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b1214-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1214-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b1214-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci T & E Express.</span><span class="sxs-lookup"><span data-stu-id="b1214-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your T&E Express application.</span></span>

<span data-ttu-id="b1214-150">**tooconfigure Azure AD jednotné přihlašování s T & E Express, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1214-150">**tooconfigure Azure AD single sign-on with T&E Express, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1214-151">V hello Azure Management portal na hello **T & E Express** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b1214-151">In hello Azure Management portal, on hello **T&E Express** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b1214-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="b1214-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

3. <span data-ttu-id="b1214-155">Na hello **T & E Express domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b1214-155">On hello **T&E Express Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    <span data-ttu-id="b1214-157">a.</span><span class="sxs-lookup"><span data-stu-id="b1214-157">a.</span></span> <span data-ttu-id="b1214-158">V hello **identifikátor** textovému poli, hodnota typu hello jako:`https://<domain>.tyeexpress.com`</span><span class="sxs-lookup"><span data-stu-id="b1214-158">In hello **Identifier** textbox, type hello value as: `https://<domain>.tyeexpress.com`</span></span>

    <span data-ttu-id="b1214-159">b.</span><span class="sxs-lookup"><span data-stu-id="b1214-159">b.</span></span> <span data-ttu-id="b1214-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span><span class="sxs-lookup"><span data-stu-id="b1214-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b1214-161">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b1214-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="b1214-162">Máte tooupdate tyto hodnoty pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="b1214-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="b1214-163">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b1214-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="b1214-164">Obraťte se na [T & E Express tým podpory](http://www.tyeexpress.com/contacto.aspx) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b1214-164">Contact [T&E Express support team](http://www.tyeexpress.com/contacto.aspx) tooget these values.</span></span>

5. <span data-ttu-id="b1214-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b1214-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

6. <span data-ttu-id="b1214-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1214-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b1214-169">tooconfigure jednotného přihlašování na **T & E Express** straně, přihlášení toohello T & E expresní aplikaci bez SAML jednotného přihlašování pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="b1214-169">tooconfigure single sign-on on **T&E Express** side, login toohello T&E express application without SAML single sign on using admin credentials.</span></span>

9. <span data-ttu-id="b1214-170">V části hello **správce** klikněte na **SAML domény** tooOpen hello SAML nastavení stránky.</span><span class="sxs-lookup"><span data-stu-id="b1214-170">Under hello **Admin** Tab, Click on **SAML domain** tooOpen hello SAML settings page.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tye-SAML.png)

10. <span data-ttu-id="b1214-172">Vyberte hello **Activar(Activate)** možnost z **ne** příliš**SI(Yes)**.</span><span class="sxs-lookup"><span data-stu-id="b1214-172">Select hello **Activar(Activate)** option from **No** too**SI(Yes)**.</span></span> <span data-ttu-id="b1214-173">V hello **metadat zprostředkovatelů Identity** textovému poli, vložte hello metadata XML, které jste donwloaded z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b1214-173">In hello **Identity Provider Metadata** textbox, paste hello metadata XML which you have donwloaded from Azure portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tyeAdmin.png)

11. <span data-ttu-id="b1214-175">Klikněte na hello **Guardar(Save)** tlačítko toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="b1214-175">Click on hello **Guardar(Save)** button toosave hello settings.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b1214-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1214-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="b1214-177">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1214-177">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b1214-179">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1214-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1214-180">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b1214-180">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b1214-182">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b1214-182">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b1214-184">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1214-184">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b1214-186">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b1214-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b1214-188">a.</span><span class="sxs-lookup"><span data-stu-id="b1214-188">a.</span></span> <span data-ttu-id="b1214-189">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b1214-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b1214-190">b.</span><span class="sxs-lookup"><span data-stu-id="b1214-190">b.</span></span> <span data-ttu-id="b1214-191">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b1214-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b1214-192">c.</span><span class="sxs-lookup"><span data-stu-id="b1214-192">c.</span></span> <span data-ttu-id="b1214-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b1214-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b1214-194">d.</span><span class="sxs-lookup"><span data-stu-id="b1214-194">d.</span></span> <span data-ttu-id="b1214-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b1214-195">Click **Create**.</span></span>
 
### <a name="creating-a-te-express-test-user"></a><span data-ttu-id="b1214-196">Vytvoření zkušebního uživatele T & E Express</span><span class="sxs-lookup"><span data-stu-id="b1214-196">Creating a T&E Express test user</span></span>

<span data-ttu-id="b1214-197">V pořadí tooenable Azure AD Uživatelé toolog do n & E Express musí být zřízená do n & E Express.</span><span class="sxs-lookup"><span data-stu-id="b1214-197">In order tooenable Azure AD users toolog into T&E Express, they must be provisioned into T&E Express.</span></span>  
<span data-ttu-id="b1214-198">V případě T & E Express zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b1214-198">In case of T&E Express, provisioning is a manual task.</span></span>

<span data-ttu-id="b1214-199">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b1214-199">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1214-200">Přihlaste se tooyour T & E Express lokalita společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="b1214-200">Log in tooyour T&E Express company site as an administrator.</span></span>

2. <span data-ttu-id="b1214-201">V části značky správce klikněte na uživatele tooopen hello uživatelé hlavní stránky.</span><span class="sxs-lookup"><span data-stu-id="b1214-201">Under Admin tag, click on Users tooopen hello Users master page.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-tyeexpress-tutorial/tye-adminusers.png)

3. <span data-ttu-id="b1214-203">Na domovské stránce hello, klikněte na  **+**  tooadd hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="b1214-203">On hello home page, click on **+** tooadd hello users.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-tyeexpress-tutorial/tye-usershome.png)

4. <span data-ttu-id="b1214-205">Zadejte všechny povinné podrobnosti hello, zobrazí výzva, ve formuláři hello a klikněte na tlačítko hello uložit tlačítko toosave hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="b1214-205">Enter all hello mandatory details as asked in hello form and click hello save button toosave hello details.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-tyeexpress-tutorial/tye-usersadd.png)

    ![Můžete přidat zaměstnance](./media/active-directory-saas-tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b1214-208">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b1214-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b1214-209">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte svůj přístup Tool & E Express.</span><span class="sxs-lookup"><span data-stu-id="b1214-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooT&E Express.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b1214-211">**tooassign Britta Simon Tool & E Express, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1214-211">**tooassign Britta Simon tooT&E Express, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1214-212">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b1214-212">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b1214-214">V seznamu aplikace hello vyberte **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="b1214-214">In hello applications list, select **T&E Express**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

3. <span data-ttu-id="b1214-216">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b1214-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b1214-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1214-218">Click **Add** button.</span></span> <span data-ttu-id="b1214-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1214-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b1214-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b1214-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b1214-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1214-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b1214-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1214-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b1214-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1214-224">Testing single sign-on</span></span>

<span data-ttu-id="b1214-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b1214-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b1214-226">Po kliknutí na tlačítko hello T & E Express dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour T & E Express aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1214-226">When you click hello T&E Express tile in hello Access Panel, you should get automatically signed-on tooyour T&E Express application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1214-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b1214-227">Additional resources</span></span>

* [<span data-ttu-id="b1214-228">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b1214-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b1214-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b1214-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_203.png

