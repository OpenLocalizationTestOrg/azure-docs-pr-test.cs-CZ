---
title: 'Kurz: Azure Active Directory integrace s HappyFox | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a HappyFox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 1190652d7d1144c7eddea339cb3f9175912407fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a><span data-ttu-id="2d837-103">Kurz: Azure Active Directory integrace s HappyFox</span><span class="sxs-lookup"><span data-stu-id="2d837-103">Tutorial: Azure Active Directory integration with HappyFox</span></span>

<span data-ttu-id="2d837-104">V tomto kurzu zjistíte, jak toointegrate HappyFox s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2d837-104">In this tutorial, you learn how toointegrate HappyFox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2d837-105">Integrace HappyFox s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2d837-105">Integrating HappyFox with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2d837-106">Můžete řídit ve službě Azure AD, který má přístup tooHappyFox</span><span class="sxs-lookup"><span data-stu-id="2d837-106">You can control in Azure AD who has access tooHappyFox</span></span>
- <span data-ttu-id="2d837-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHappyFox (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d837-107">You can enable your users tooautomatically get signed-on tooHappyFox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2d837-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2d837-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2d837-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2d837-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d837-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2d837-110">Prerequisites</span></span>

<span data-ttu-id="2d837-111">Integrace služby Azure AD s HappyFox tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2d837-111">tooconfigure Azure AD integration with HappyFox, you need hello following items:</span></span>

- <span data-ttu-id="2d837-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d837-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2d837-113">HappyFox jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2d837-113">A HappyFox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2d837-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2d837-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2d837-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2d837-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2d837-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2d837-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2d837-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2d837-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2d837-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2d837-118">Scenario description</span></span>
<span data-ttu-id="2d837-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2d837-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2d837-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2d837-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2d837-121">Přidání HappyFox z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2d837-121">Adding HappyFox from hello gallery</span></span>
2. <span data-ttu-id="2d837-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d837-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-happyfox-from-hello-gallery"></a><span data-ttu-id="2d837-123">Přidání HappyFox z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2d837-123">Adding HappyFox from hello gallery</span></span>
<span data-ttu-id="2d837-124">tooconfigure hello integrace HappyFox do Azure AD, je nutné tooadd HappyFox hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2d837-124">tooconfigure hello integration of HappyFox into Azure AD, you need tooadd HappyFox from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2d837-125">**tooadd HappyFox z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2d837-125">**tooadd HappyFox from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d837-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2d837-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2d837-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2d837-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2d837-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2d837-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2d837-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2d837-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2d837-133">Hello vyhledávacího pole zadejte **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="2d837-133">In hello search box, type **HappyFox**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_search.png)

5. <span data-ttu-id="2d837-135">Na panelu výsledků hello vyberte **HappyFox**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d837-135">In hello results panel, select **HappyFox**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2d837-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d837-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2d837-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s HappyFox podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="2d837-138">In this section, you configure and test Azure AD single sign-on with HappyFox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2d837-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v HappyFox je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d837-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in HappyFox is tooa user in Azure AD.</span></span> <span data-ttu-id="2d837-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v HappyFox musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2d837-140">In other words, a link relationship between an Azure AD user and hello related user in HappyFox needs toobe established.</span></span>

<span data-ttu-id="2d837-141">V HappyFox, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="2d837-141">In HappyFox, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2d837-142">tooconfigure a testu Azure AD jednotné přihlašování s HappyFox, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2d837-142">tooconfigure and test Azure AD single sign-on with HappyFox, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2d837-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2d837-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2d837-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d837-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2d837-145">**[Vytvoření zkušebního uživatele HappyFox](#creating-a-happyfox-test-user)**  -toohave protějšek Britta Simon v HappyFox, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2d837-145">**[Creating a HappyFox test user](#creating-a-happyfox-test-user)** - toohave a counterpart of Britta Simon in HappyFox that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2d837-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2d837-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2d837-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2d837-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2d837-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d837-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2d837-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci HappyFox.</span><span class="sxs-lookup"><span data-stu-id="2d837-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HappyFox application.</span></span>

<span data-ttu-id="2d837-150">**tooconfigure Azure AD jednotné přihlašování s HappyFox, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2d837-150">**tooconfigure Azure AD single sign-on with HappyFox, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d837-151">V portálu Azure, na hello hello **HappyFox** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2d837-151">In hello Azure portal, on hello **HappyFox** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2d837-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2d837-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_samlbase.png)

3. <span data-ttu-id="2d837-155">Na hello **HappyFox domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2d837-155">On hello **HappyFox Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_url.png)

    <span data-ttu-id="2d837-157">a.</span><span class="sxs-lookup"><span data-stu-id="2d837-157">a.</span></span> <span data-ttu-id="2d837-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.happyfox.com/`</span><span class="sxs-lookup"><span data-stu-id="2d837-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.happyfox.com/`</span></span>

    <span data-ttu-id="2d837-159">b.</span><span class="sxs-lookup"><span data-stu-id="2d837-159">b.</span></span> <span data-ttu-id="2d837-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.happyfox.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="2d837-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.happyfox.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2d837-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="2d837-161">These values are not real.</span></span> <span data-ttu-id="2d837-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2d837-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2d837-163">Obraťte se na [tým podpory HappyFox klienta](https://support.happyfox.com/home) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2d837-163">Contact [HappyFox Client support team](https://support.happyfox.com/home) tooget these values.</span></span> 
 
4. <span data-ttu-id="2d837-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2d837-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_certificate.png) 

5. <span data-ttu-id="2d837-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2d837-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2d837-168">Na hello **HappyFox konfigurace** klikněte na tlačítko **konfigurace HappyFox** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="2d837-168">On hello **HappyFox Configuration** section, click **Configure HappyFox** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2d837-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části**.</span><span class="sxs-lookup"><span data-stu-id="2d837-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_configure.png) 

7. <span data-ttu-id="2d837-171">Přihlášení na portál zaměstnanci HappyFox tooyour a přejděte příliš**spravovat**, klikněte na **integrace** kartě.</span><span class="sxs-lookup"><span data-stu-id="2d837-171">Sign on tooyour HappyFox staff portal and navigate too**Manage**, Click on **Integrations** tab.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/header.png) 

8. <span data-ttu-id="2d837-173">Na kartě Integrace hello, klikněte na tlačítko **konfigurace** pod **SAML integrace** tooopen hello jeden znak na nastavení.</span><span class="sxs-lookup"><span data-stu-id="2d837-173">In hello Integrations tab, Click **Configure** under **SAML Integration** tooopen hello Single Sign On Settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/configure.png) 

9. <span data-ttu-id="2d837-175">V části Konfigurace SAML, vložte hello **SAML jeden přihlašování adresa URL služby** kterou jste zkopírovali z portálu Azure do **jednotné přihlašování cílová adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2d837-175">Inside SAML configuration section, paste hello **SAML Single Sign-On Service URL** that you have copied from Azure portal into **SSO Target URL** textbox.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/targeturl.png)

10. <span data-ttu-id="2d837-177">Otevřete certifikát hello si stáhli z portálu Azure v programu Poznámkový blok a vložte jeho obsah v **IdP podpis** části.</span><span class="sxs-lookup"><span data-stu-id="2d837-177">Open hello certificate downloaded from Azure portal in notepad and paste its content in **IdP Signature** section.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/cert.png)

11. <span data-ttu-id="2d837-179">Klikněte na tlačítko **uložit nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2d837-179">Click **Save Settings** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/savesettings.png)

> [!TIP]
> <span data-ttu-id="2d837-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="2d837-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2d837-182">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="2d837-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2d837-183">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2d837-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2d837-184">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d837-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="2d837-185">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2d837-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2d837-187">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2d837-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d837-188">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2d837-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2d837-190">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2d837-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2d837-192">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="2d837-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2d837-194">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2d837-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2d837-196">a.</span><span class="sxs-lookup"><span data-stu-id="2d837-196">a.</span></span> <span data-ttu-id="2d837-197">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2d837-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2d837-198">b.</span><span class="sxs-lookup"><span data-stu-id="2d837-198">b.</span></span> <span data-ttu-id="2d837-199">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2d837-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2d837-200">c.</span><span class="sxs-lookup"><span data-stu-id="2d837-200">c.</span></span> <span data-ttu-id="2d837-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2d837-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2d837-202">d.</span><span class="sxs-lookup"><span data-stu-id="2d837-202">d.</span></span> <span data-ttu-id="2d837-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2d837-203">Click **Create**.</span></span>
 
### <a name="creating-a-happyfox-test-user"></a><span data-ttu-id="2d837-204">Vytvoření zkušebního uživatele HappyFox</span><span class="sxs-lookup"><span data-stu-id="2d837-204">Creating a HappyFox test user</span></span>

<span data-ttu-id="2d837-205">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele v aplikaci hello automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="2d837-205">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span>
        
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2d837-206">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2d837-206">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2d837-207">V této části povolíte tak, že udělíte přístup tooHappyFox toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2d837-207">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHappyFox.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2d837-209">**tooassign Britta Simon tooHappyFox, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2d837-209">**tooassign Britta Simon tooHappyFox, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d837-210">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2d837-210">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2d837-212">V seznamu aplikace hello vyberte **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="2d837-212">In hello applications list, select **HappyFox**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_app.png) 

3. <span data-ttu-id="2d837-214">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2d837-214">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2d837-216">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2d837-216">Click **Add** button.</span></span> <span data-ttu-id="2d837-217">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2d837-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2d837-219">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2d837-219">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2d837-220">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2d837-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2d837-221">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2d837-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2d837-222">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d837-222">Testing single sign-on</span></span>

<span data-ttu-id="2d837-223">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2d837-223">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

1. <span data-ttu-id="2d837-224">Když kliknete na dlaždici HappyFox hello v hello přístupového panelu, měli byste obdržet přihlašovací stránku HappyFox aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d837-224">When you click hello HappyFox tile in hello Access Panel, you should get login page of HappyFox application.</span></span> <span data-ttu-id="2d837-225">Měli byste vidět hello **'SAML'** tlačítko na přihlašovací stránku hello.</span><span class="sxs-lookup"><span data-stu-id="2d837-225">You should see hello **‘SAML’** button on hello sign-in page.</span></span>

    ![Modul plug-in](./media/active-directory-saas-happyfox-tutorial/saml.png) 

2. <span data-ttu-id="2d837-227">Klikněte na tlačítko hello **'SAML'** tlačítko toolog v tooHappyFox pomocí účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d837-227">Click hello **‘SAML’** button toolog in tooHappyFox using your Azure AD account.</span></span>

<span data-ttu-id="2d837-228">Další informace o hello přístupového panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2d837-228">For more information about hello Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2d837-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2d837-229">Additional resources</span></span>

* [<span data-ttu-id="2d837-230">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2d837-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2d837-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2d837-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_203.png

