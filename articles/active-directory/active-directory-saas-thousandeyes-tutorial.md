---
title: 'Kurz: Azure Active Directory integrace s ThousandEyes | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ThousandEyes."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.openlocfilehash: fbfbfb71809355b1b138762757a851907737730b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a><span data-ttu-id="d08e6-103">Kurz: Azure Active Directory integrace s ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="d08e6-103">Tutorial: Azure Active Directory integration with ThousandEyes</span></span>

<span data-ttu-id="d08e6-104">V tomto kurzu zjistíte, jak toointegrate ThousandEyes s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d08e6-104">In this tutorial, you learn how toointegrate ThousandEyes with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d08e6-105">Integrace ThousandEyes s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d08e6-105">Integrating ThousandEyes with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d08e6-106">Můžete řídit ve službě Azure AD, který má přístup tooThousandEyes</span><span class="sxs-lookup"><span data-stu-id="d08e6-106">You can control in Azure AD who has access tooThousandEyes</span></span>
- <span data-ttu-id="d08e6-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooThousandEyes (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d08e6-107">You can enable your users tooautomatically get signed-on tooThousandEyes (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d08e6-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d08e6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d08e6-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d08e6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d08e6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d08e6-110">Prerequisites</span></span>

<span data-ttu-id="d08e6-111">Integrace služby Azure AD s ThousandEyes tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="d08e6-111">tooconfigure Azure AD integration with ThousandEyes, you need hello following items:</span></span>

- <span data-ttu-id="d08e6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d08e6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d08e6-113">ThousandEyes jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d08e6-113">A ThousandEyes single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d08e6-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d08e6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d08e6-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d08e6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d08e6-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d08e6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d08e6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d08e6-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d08e6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d08e6-118">Scenario description</span></span>
<span data-ttu-id="d08e6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d08e6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d08e6-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d08e6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d08e6-121">Přidání ThousandEyes z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d08e6-121">Adding ThousandEyes from hello gallery</span></span>
2. <span data-ttu-id="d08e6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d08e6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thousandeyes-from-hello-gallery"></a><span data-ttu-id="d08e6-123">Přidání ThousandEyes z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d08e6-123">Adding ThousandEyes from hello gallery</span></span>
<span data-ttu-id="d08e6-124">tooconfigure hello integrace ThousandEyes do Azure AD, je nutné tooadd ThousandEyes hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d08e6-124">tooconfigure hello integration of ThousandEyes into Azure AD, you need tooadd ThousandEyes from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d08e6-125">**tooadd ThousandEyes z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d08e6-125">**tooadd ThousandEyes from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d08e6-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d08e6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d08e6-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d08e6-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d08e6-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d08e6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d08e6-133">Hello vyhledávacího pole zadejte **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-133">In hello search box, type **ThousandEyes**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_search.png)

5. <span data-ttu-id="d08e6-135">Na panelu výsledků hello vyberte **ThousandEyes**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d08e6-135">In hello results panel, select **ThousandEyes**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d08e6-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d08e6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d08e6-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ThousandEyes podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d08e6-138">In this section, you configure and test Azure AD single sign-on with ThousandEyes based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d08e6-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ThousandEyes je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d08e6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ThousandEyes is tooa user in Azure AD.</span></span> <span data-ttu-id="d08e6-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ThousandEyes musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="d08e6-140">In other words, a link relationship between an Azure AD user and hello related user in ThousandEyes needs toobe established.</span></span>

<span data-ttu-id="d08e6-141">V ThousandEyes, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="d08e6-141">In ThousandEyes, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d08e6-142">tooconfigure a testu Azure AD jednotné přihlašování s ThousandEyes, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="d08e6-142">tooconfigure and test Azure AD single sign-on with ThousandEyes, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d08e6-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="d08e6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d08e6-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d08e6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d08e6-145">**[Vytvoření zkušebního uživatele ThousandEyes](#creating-a-thousandeyes-test-user)**  -toohave protějšek Britta Simon v ThousandEyes, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="d08e6-145">**[Creating a ThousandEyes test user](#creating-a-thousandeyes-test-user)** - toohave a counterpart of Britta Simon in ThousandEyes that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d08e6-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d08e6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d08e6-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="d08e6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d08e6-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d08e6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d08e6-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="d08e6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ThousandEyes application.</span></span>

<span data-ttu-id="d08e6-150">**tooconfigure Azure AD jednotné přihlašování s ThousandEyes, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d08e6-150">**tooconfigure Azure AD single sign-on with ThousandEyes, perform hello following steps:**</span></span>

1. <span data-ttu-id="d08e6-151">V portálu Azure, na hello hello **ThousandEyes** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-151">In hello Azure portal, on hello **ThousandEyes** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d08e6-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d08e6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

3. <span data-ttu-id="d08e6-155">Na hello **ThousandEyes domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d08e6-155">On hello **ThousandEyes Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    <span data-ttu-id="d08e6-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://app.thousandeyes.com/login/sso`</span><span class="sxs-lookup"><span data-stu-id="d08e6-157">In hello **Sign-on URL** textbox, type a URL as: `https://app.thousandeyes.com/login/sso`</span></span>

4. <span data-ttu-id="d08e6-158">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d08e6-158">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_certificate.png) 

5. <span data-ttu-id="d08e6-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d08e6-160">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d08e6-162">Na hello **ThousandEyes konfigurace** klikněte na tlačítko **konfigurace ThousandEyes** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d08e6-162">On hello **ThousandEyes Configuration** section, click **Configure ThousandEyes** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d08e6-163">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d08e6-163">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

7. <span data-ttu-id="d08e6-165">V okně prohlížeče jiných webových přihlásit tooyour **ThousandEyes** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="d08e6-165">In a different web browser window, sign on tooyour **ThousandEyes** company site as an administrator.</span></span>

8. <span data-ttu-id="d08e6-166">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-166">In hello menu on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="d08e6-167">![Nastavení](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="d08e6-167">![Settings](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "Settings")</span></span>

9. <span data-ttu-id="d08e6-168">Klikněte na tlačítko **účtu**</span><span class="sxs-lookup"><span data-stu-id="d08e6-168">Click **Account**</span></span>
   
    <span data-ttu-id="d08e6-169">![Účet](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "účtu")</span><span class="sxs-lookup"><span data-stu-id="d08e6-169">![Account](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "Account")</span></span>

10. <span data-ttu-id="d08e6-170">Klikněte na tlačítko hello **ověřování a zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="d08e6-170">Click hello **Security & Authentication** tab.</span></span>
   
    <span data-ttu-id="d08e6-171">![Zabezpečení a ověřování](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "ověřování a zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="d08e6-171">![Security & Authentication](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "Security & Authentication")</span></span>

11. <span data-ttu-id="d08e6-172">V hello **instalace Single Sign-On** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d08e6-172">In hello **Setup Single Sign-On** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d08e6-173">![Nastavení jednotného přihlašování](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "nastavit jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="d08e6-173">![Setup Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "Setup Single Sign-On")</span></span>
  
    <span data-ttu-id="d08e6-174">a.</span><span class="sxs-lookup"><span data-stu-id="d08e6-174">a.</span></span> <span data-ttu-id="d08e6-175">Vyberte **povolit jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-175">Select **Enable Single Sign-On**.</span></span>
  
    <span data-ttu-id="d08e6-176">b.</span><span class="sxs-lookup"><span data-stu-id="d08e6-176">b.</span></span> <span data-ttu-id="d08e6-177">V **URL přihlašovací stránky** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d08e6-177">In **Login Page URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="d08e6-178">c.</span><span class="sxs-lookup"><span data-stu-id="d08e6-178">c.</span></span> <span data-ttu-id="d08e6-179">V **adresa URL odhlašovací stránky** textovému poli, vložte **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d08e6-179">In **Logout Page URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="d08e6-180">d.</span><span class="sxs-lookup"><span data-stu-id="d08e6-180">d.</span></span> <span data-ttu-id="d08e6-181">**Vystavitel zprostředkovatele identity** textovému poli, vložte **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d08e6-181">**Identity Provider Issuer** textbox, paste **SAML Entity ID** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="d08e6-182">e.</span><span class="sxs-lookup"><span data-stu-id="d08e6-182">e.</span></span> <span data-ttu-id="d08e6-183">V **ověřovací certifikát**, klikněte na tlačítko **zvolte soubor**a pak nahrajte certifikát hello jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d08e6-183">In **Verification Certificate**, click **Choose file**, and then upload hello certificate you have downloaded from Azure portal.</span></span>
  
    <span data-ttu-id="d08e6-184">f.</span><span class="sxs-lookup"><span data-stu-id="d08e6-184">f.</span></span> <span data-ttu-id="d08e6-185">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-185">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="d08e6-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="d08e6-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d08e6-187">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="d08e6-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d08e6-188">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d08e6-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d08e6-189">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d08e6-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="d08e6-190">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="d08e6-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d08e6-192">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d08e6-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d08e6-193">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d08e6-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d08e6-195">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d08e6-197">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="d08e6-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d08e6-199">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d08e6-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d08e6-201">a.</span><span class="sxs-lookup"><span data-stu-id="d08e6-201">a.</span></span> <span data-ttu-id="d08e6-202">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d08e6-203">b.</span><span class="sxs-lookup"><span data-stu-id="d08e6-203">b.</span></span> <span data-ttu-id="d08e6-204">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d08e6-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d08e6-205">c.</span><span class="sxs-lookup"><span data-stu-id="d08e6-205">c.</span></span> <span data-ttu-id="d08e6-206">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d08e6-207">d.</span><span class="sxs-lookup"><span data-stu-id="d08e6-207">d.</span></span> <span data-ttu-id="d08e6-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-208">Click **Create**.</span></span>
 
### <a name="creating-a-thousandeyes-test-user"></a><span data-ttu-id="d08e6-209">Vytvoření zkušebního uživatele ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="d08e6-209">Creating a ThousandEyes test user</span></span>

<span data-ttu-id="d08e6-210">V pořadí tooenable Azure AD Uživatelé toolog do ThousandEyes musí být zřízená do ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="d08e6-210">In order tooenable Azure AD users toolog into ThousandEyes, they must be provisioned into ThousandEyes.</span></span>  
<span data-ttu-id="d08e6-211">V případě hello ThousandEyes zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d08e6-211">In hello case of ThousandEyes, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="d08e6-212">Můžete použít všechny ostatní ThousandEyes uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ThousandEyes tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="d08e6-212">You can use any other ThousandEyes user account creation tools or APIs provided by ThousandEyes tooprovision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="d08e6-213">**tooprovision tooThousandEyes účet uživatele, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d08e6-213">**tooprovision a user account tooThousandEyes, perform hello following steps:**</span></span>

1. <span data-ttu-id="d08e6-214">Přihlaste se k serveru vaší společnosti ThousandEyes jako správce.</span><span class="sxs-lookup"><span data-stu-id="d08e6-214">Log into your ThousandEyes company site as an administrator.</span></span>

2. <span data-ttu-id="d08e6-215">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-215">Click **Settings**.</span></span>
   
    <span data-ttu-id="d08e6-216">![Nastavení](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="d08e6-216">![Settings](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")</span></span>

3. <span data-ttu-id="d08e6-217">Klikněte na tlačítko **účet**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-217">Click **Account**.</span></span>
   
    <span data-ttu-id="d08e6-218">![Účet](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "účtu")</span><span class="sxs-lookup"><span data-stu-id="d08e6-218">![Account](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")</span></span>

4. <span data-ttu-id="d08e6-219">Klikněte na tlačítko hello **účty a uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="d08e6-219">Click hello **Accounts & Users** tab.</span></span>
   
    <span data-ttu-id="d08e6-220">![Účty a uživatelé](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "účty a uživatelé")</span><span class="sxs-lookup"><span data-stu-id="d08e6-220">![Accounts & Users](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts & Users")</span></span>

5. <span data-ttu-id="d08e6-221">V hello **přidat uživatele & účty** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d08e6-221">In hello **Add Users & Accounts** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d08e6-222">![Přidání uživatelských účtů](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "přidejte uživatelské účty")</span><span class="sxs-lookup"><span data-stu-id="d08e6-222">![Add User Accounts](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Add User Accounts")</span></span>   
  
    <span data-ttu-id="d08e6-223">a.</span><span class="sxs-lookup"><span data-stu-id="d08e6-223">a.</span></span> <span data-ttu-id="d08e6-224">V **název** textovému poli, název typu hello uživatele jako **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-224">In **Name** textbox, type hello name of user like **Britta Simon**.</span></span>

    <span data-ttu-id="d08e6-225">b.</span><span class="sxs-lookup"><span data-stu-id="d08e6-225">b.</span></span> <span data-ttu-id="d08e6-226">V **e-mailu** jako typ hello e-mailu uživatele k textovému poli,  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d08e6-226">In **Email** textbox, type hello email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="d08e6-227">b.</span><span class="sxs-lookup"><span data-stu-id="d08e6-227">b.</span></span> <span data-ttu-id="d08e6-228">Klikněte na tlačítko **přidat nové uživatele tooAccount**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-228">Click **Add New User tooAccount**.</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="d08e6-229">Držitel účtu Azure Active Directory Hello se získat e-mailu, včetně tooconfirm odkaz a aktivovat účet hello.</span><span class="sxs-lookup"><span data-stu-id="d08e6-229">hello Azure Active Directory account holder will get an email including a link tooconfirm and activate hello account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d08e6-230">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="d08e6-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d08e6-231">V této části povolíte tak, že udělíte přístup tooThousandEyes toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d08e6-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooThousandEyes.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d08e6-233">**tooassign Britta Simon tooThousandEyes, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d08e6-233">**tooassign Britta Simon tooThousandEyes, perform hello following steps:**</span></span>

1. <span data-ttu-id="d08e6-234">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d08e6-236">V seznamu aplikace hello vyberte **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-236">In hello applications list, select **ThousandEyes**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

3. <span data-ttu-id="d08e6-238">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d08e6-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d08e6-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d08e6-240">Click **Add** button.</span></span> <span data-ttu-id="d08e6-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d08e6-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d08e6-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="d08e6-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d08e6-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d08e6-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d08e6-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d08e6-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d08e6-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d08e6-246">Testing single sign-on</span></span>

<span data-ttu-id="d08e6-247">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d08e6-247">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d08e6-248">Po kliknutí na tlačítko hello ThousandEyes dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ThousandEyes aplikace.</span><span class="sxs-lookup"><span data-stu-id="d08e6-248">When you click hello ThousandEyes tile in hello Access Panel, you should get automatically signed-on tooyour ThousandEyes application.</span></span>

<span data-ttu-id="d08e6-249">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d08e6-249">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d08e6-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d08e6-250">Additional resources</span></span>

* [<span data-ttu-id="d08e6-251">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d08e6-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d08e6-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d08e6-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_203.png

