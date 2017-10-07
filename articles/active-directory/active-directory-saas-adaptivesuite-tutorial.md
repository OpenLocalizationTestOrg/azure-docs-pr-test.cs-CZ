---
title: "Kurz: Azure Active Directory integrace s adaptivní Suite | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a adaptivní Suite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: af309c27ab74098c1e229c80adb11c96dc2774fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a><span data-ttu-id="42da1-103">Kurz: Azure Active Directory integrace s adaptivní Suite</span><span class="sxs-lookup"><span data-stu-id="42da1-103">Tutorial: Azure Active Directory integration with Adaptive Suite</span></span>

<span data-ttu-id="42da1-104">V tomto kurzu zjistíte, jak toointegrate adaptivní Suite a Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="42da1-104">In this tutorial, you learn how toointegrate Adaptive Suite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="42da1-105">Integrace adaptivní Suite s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="42da1-105">Integrating Adaptive Suite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="42da1-106">Můžete řídit ve službě Azure AD, který má přístup tooAdaptive Suite</span><span class="sxs-lookup"><span data-stu-id="42da1-106">You can control in Azure AD who has access tooAdaptive Suite</span></span>
- <span data-ttu-id="42da1-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAdaptive Suite (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="42da1-107">You can enable your users tooautomatically get signed-on tooAdaptive Suite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="42da1-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="42da1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="42da1-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="42da1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42da1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="42da1-110">Prerequisites</span></span>

<span data-ttu-id="42da1-111">tooconfigure integrace Azure AD s adaptivní Suite, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="42da1-111">tooconfigure Azure AD integration with Adaptive Suite, you need hello following items:</span></span>

- <span data-ttu-id="42da1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="42da1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="42da1-113">Adaptivní Suite jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="42da1-113">An Adaptive Suite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="42da1-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="42da1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="42da1-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="42da1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="42da1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="42da1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="42da1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="42da1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="42da1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="42da1-118">Scenario description</span></span>
<span data-ttu-id="42da1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="42da1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="42da1-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="42da1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="42da1-121">Přidání adaptivní Suite z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="42da1-121">Adding Adaptive Suite from hello gallery</span></span>
2. <span data-ttu-id="42da1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="42da1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adaptive-suite-from-hello-gallery"></a><span data-ttu-id="42da1-123">Přidání adaptivní Suite z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="42da1-123">Adding Adaptive Suite from hello gallery</span></span>
<span data-ttu-id="42da1-124">tooconfigure hello integraci sady adaptivní do služby Azure AD, je nutné tooadd adaptivní Suite hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="42da1-124">tooconfigure hello integration of Adaptive Suite into Azure AD, you need tooadd Adaptive Suite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="42da1-125">**tooadd adaptivní Suite z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="42da1-125">**tooadd Adaptive Suite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="42da1-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="42da1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="42da1-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="42da1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="42da1-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="42da1-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="42da1-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="42da1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="42da1-133">Hello vyhledávacího pole zadejte **adaptivní Suite**.</span><span class="sxs-lookup"><span data-stu-id="42da1-133">In hello search box, type **Adaptive Suite**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. <span data-ttu-id="42da1-135">Na panelu výsledků hello vyberte **adaptivní Suite**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="42da1-135">In hello results panel, select **Adaptive Suite**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="42da1-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="42da1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="42da1-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s adaptivní Suite podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="42da1-138">In this section, you configure and test Azure AD single sign-on with Adaptive Suite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="42da1-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v adaptivní Suite je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="42da1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Adaptive Suite is tooa user in Azure AD.</span></span> <span data-ttu-id="42da1-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v sadě adaptivní musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="42da1-140">In other words, a link relationship between an Azure AD user and hello related user in Adaptive Suite needs toobe established.</span></span>

<span data-ttu-id="42da1-141">V adaptivní Suite přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="42da1-141">In Adaptive Suite, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="42da1-142">tooconfigure a testu Azure AD jednotné přihlašování s adaptivní Suite, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="42da1-142">tooconfigure and test Azure AD single sign-on with Adaptive Suite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="42da1-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="42da1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="42da1-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="42da1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="42da1-145">**[Vytváření testovacího uživatele adaptivní Suite](#creating-an-adaptive-suite-test-user)**  -toohave protějšek Britta Simon adaptivní sady, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="42da1-145">**[Creating an Adaptive Suite test user](#creating-an-adaptive-suite-test-user)** - toohave a counterpart of Britta Simon in Adaptive Suite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="42da1-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="42da1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="42da1-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="42da1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="42da1-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="42da1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="42da1-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci adaptivní Suite.</span><span class="sxs-lookup"><span data-stu-id="42da1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Adaptive Suite application.</span></span>

<span data-ttu-id="42da1-150">**tooconfigure Azure AD jednotné přihlašování s adaptivní Suite, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="42da1-150">**tooconfigure Azure AD single sign-on with Adaptive Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="42da1-151">V portálu Azure, na hello hello **adaptivní Suite** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="42da1-151">In hello Azure portal, on hello **Adaptive Suite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="42da1-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="42da1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. <span data-ttu-id="42da1-155">Na hello **adaptivní Suite domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="42da1-155">On hello **Adaptive Suite Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    <span data-ttu-id="42da1-157">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span><span class="sxs-lookup"><span data-stu-id="42da1-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span></span>

    >[!NOTE]
    > <span data-ttu-id="42da1-158">Tuto hodnotu můžete získat z hello adaptivní Suite je **nastavení jednotného přihlašování SAML** stránky.</span><span class="sxs-lookup"><span data-stu-id="42da1-158">You can get this value from hello Adaptive Suite’s **SAML SSO Settings** page.</span></span>
    >  

4. <span data-ttu-id="42da1-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="42da1-159">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. <span data-ttu-id="42da1-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="42da1-161">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="42da1-163">Na hello **adaptivní Suite konfigurace** klikněte na tlačítko **konfigurace adaptivní Suite** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="42da1-163">On hello **Adaptive Suite Configuration** section, click **Configure Adaptive Suite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="42da1-164">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="42da1-164">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. <span data-ttu-id="42da1-166">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour adaptivní Suite.</span><span class="sxs-lookup"><span data-stu-id="42da1-166">In a different web browser window, log in tooyour Adaptive Suite company site as an administrator.</span></span>

8. <span data-ttu-id="42da1-167">Přejděte příliš**správce**.</span><span class="sxs-lookup"><span data-stu-id="42da1-167">Go too**Admin**.</span></span>
   
    <span data-ttu-id="42da1-168">![Správce](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "správce")</span><span class="sxs-lookup"><span data-stu-id="42da1-168">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>

9. <span data-ttu-id="42da1-169">V hello **uživatelů a rolí** klikněte na tlačítko **spravovat nastavení jednotného přihlašování SAML**.</span><span class="sxs-lookup"><span data-stu-id="42da1-169">In hello **Users and Roles** section, click **Manage SAML SSO Settings**.</span></span>
   
    <span data-ttu-id="42da1-170">![Spravovat nastavení jednotného přihlašování SAML](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "spravovat nastavení jednotného přihlašování SAML")</span><span class="sxs-lookup"><span data-stu-id="42da1-170">![Manage SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "Manage SAML SSO Settings")</span></span>

10. <span data-ttu-id="42da1-171">Na hello **nastavení jednotného přihlašování SAML** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="42da1-171">On hello **SAML SSO Settings** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="42da1-172">![Nastavení jednotného přihlašování SAML](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "nastavení jednotného přihlašování SAML")</span><span class="sxs-lookup"><span data-stu-id="42da1-172">![SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO Settings")</span></span>

    <span data-ttu-id="42da1-173">a.</span><span class="sxs-lookup"><span data-stu-id="42da1-173">a.</span></span> <span data-ttu-id="42da1-174">V hello **název zprostředkovatele Identity** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="42da1-174">In hello **Identity provider name** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="42da1-175">b.</span><span class="sxs-lookup"><span data-stu-id="42da1-175">b.</span></span> <span data-ttu-id="42da1-176">Vložení hello **SAML Entity ID** hodnota zkopírována z portálu Azure do hello **zprostředkovatele Identity Entity ID** textové pole.</span><span class="sxs-lookup"><span data-stu-id="42da1-176">Paste hello **SAML Entity ID** value copied from Azure portal into hello **Identity provider Entity ID** textbox.</span></span>
  
    <span data-ttu-id="42da1-177">c.</span><span class="sxs-lookup"><span data-stu-id="42da1-177">c.</span></span> <span data-ttu-id="42da1-178">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnota zkopírována z portálu Azure do hello **zprostředkovatele Identity jednotného přihlašování k adrese URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="42da1-178">Paste hello **SAML Single Sign-On Service URL** value copied from Azure portal into hello **Identity provider SSO URL** textbox.</span></span>
  
    <span data-ttu-id="42da1-179">d.</span><span class="sxs-lookup"><span data-stu-id="42da1-179">d.</span></span> <span data-ttu-id="42da1-180">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnota zkopírována z portálu Azure do hello **vlastní adresa URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="42da1-180">Paste hello **SAML Single Sign-On Service URL** value copied from Azure portal into hello **Custom logout URL** textbox.</span></span>
  
    <span data-ttu-id="42da1-181">e.</span><span class="sxs-lookup"><span data-stu-id="42da1-181">e.</span></span> <span data-ttu-id="42da1-182">tooupload stažený certifikát, klikněte na tlačítko **zvolte soubor**.</span><span class="sxs-lookup"><span data-stu-id="42da1-182">tooupload your downloaded certificate, click **Choose file**.</span></span>
  
    <span data-ttu-id="42da1-183">f.</span><span class="sxs-lookup"><span data-stu-id="42da1-183">f.</span></span> <span data-ttu-id="42da1-184">Vyberte pro hello následující:</span><span class="sxs-lookup"><span data-stu-id="42da1-184">Select hello following, for:</span></span>
    * <span data-ttu-id="42da1-185">**Id uživatele SAML**, vyberte **adaptivní Statistika uživatelského jména**.</span><span class="sxs-lookup"><span data-stu-id="42da1-185">**SAML user id**, select **User’s Adaptive Insights user name**.</span></span>
    * <span data-ttu-id="42da1-186">**Umístění id uživatele SAML**, vyberte **id uživatele v NameID předmětu**.</span><span class="sxs-lookup"><span data-stu-id="42da1-186">**SAML user id location**, select **User id in NameID of Subject**.</span></span>
    * <span data-ttu-id="42da1-187">**Formát SAML NameID**, vyberte **e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="42da1-187">**SAML NameID format**, select **Email address**.</span></span>
    * <span data-ttu-id="42da1-188">**Povolit SAML**, vyberte **povolit jednotné přihlašování SAML a přímé přihlášení adaptivní Insights**.</span><span class="sxs-lookup"><span data-stu-id="42da1-188">**Enable SAML**, select **Allow SAML SSO and direct Adaptive Insights login**.</span></span>
    
    <span data-ttu-id="42da1-189">g.</span><span class="sxs-lookup"><span data-stu-id="42da1-189">g.</span></span> <span data-ttu-id="42da1-190">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="42da1-190">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="42da1-191">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="42da1-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="42da1-192">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="42da1-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="42da1-193">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="42da1-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="42da1-194">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="42da1-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="42da1-195">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="42da1-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="42da1-197">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="42da1-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="42da1-198">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="42da1-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="42da1-200">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="42da1-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="42da1-202">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="42da1-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="42da1-204">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="42da1-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="42da1-206">a.</span><span class="sxs-lookup"><span data-stu-id="42da1-206">a.</span></span> <span data-ttu-id="42da1-207">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="42da1-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="42da1-208">b.</span><span class="sxs-lookup"><span data-stu-id="42da1-208">b.</span></span> <span data-ttu-id="42da1-209">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="42da1-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="42da1-210">c.</span><span class="sxs-lookup"><span data-stu-id="42da1-210">c.</span></span> <span data-ttu-id="42da1-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="42da1-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="42da1-212">d.</span><span class="sxs-lookup"><span data-stu-id="42da1-212">d.</span></span> <span data-ttu-id="42da1-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="42da1-213">Click **Create**.</span></span>
 
### <a name="creating-an-adaptive-suite-test-user"></a><span data-ttu-id="42da1-214">Vytváření testovacího uživatele adaptivní Suite</span><span class="sxs-lookup"><span data-stu-id="42da1-214">Creating an Adaptive Suite test user</span></span>

<span data-ttu-id="42da1-215">Uživatelé toolog tooenable Azure AD v tooAdaptive Suite, se musí být zřízená do adaptivní Suite.</span><span class="sxs-lookup"><span data-stu-id="42da1-215">tooenable Azure AD users toolog in tooAdaptive Suite, they must be provisioned into Adaptive Suite.</span></span>  

* <span data-ttu-id="42da1-216">V případě hello adaptivní sady zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="42da1-216">In hello case of Adaptive Suite, provisioning is a manual task.</span></span>

<span data-ttu-id="42da1-217">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="42da1-217">**tooconfigure user provisioning, perform hello following steps:**</span></span> 

1. <span data-ttu-id="42da1-218">Přihlaste se tooyour **adaptivní Suite** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="42da1-218">Log in tooyour **Adaptive Suite** company site as an administrator.</span></span>
2. <span data-ttu-id="42da1-219">Přejděte příliš**správce**.</span><span class="sxs-lookup"><span data-stu-id="42da1-219">Go too**Admin**.</span></span>
   
   <span data-ttu-id="42da1-220">![Správce](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "správce")</span><span class="sxs-lookup"><span data-stu-id="42da1-220">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>
3. <span data-ttu-id="42da1-221">V hello **uživatelů a rolí** klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="42da1-221">In hello **Users and Roles** section, click **Add User**.</span></span>
   
   <span data-ttu-id="42da1-222">![Přidat uživatele](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="42da1-222">![Add User](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "Add User")</span></span>
4. <span data-ttu-id="42da1-223">V hello **nového uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="42da1-223">In hello **New User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="42da1-224">![Odeslání](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "odeslání")</span><span class="sxs-lookup"><span data-stu-id="42da1-224">![Submit](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "Submit")</span></span>   

   <span data-ttu-id="42da1-225">a.</span><span class="sxs-lookup"><span data-stu-id="42da1-225">a.</span></span> <span data-ttu-id="42da1-226">Typ hello **název**, **přihlášení**, **e-mailu**, **heslo** platného uživatele Azure Active Directory, kterou chcete tooprovision do hello související textová pole.</span><span class="sxs-lookup"><span data-stu-id="42da1-226">Type hello **Name**, **Login**, **Email**, **Password** of a valid Azure Active Directory user you want tooprovision into hello related textboxes.</span></span>
  
   <span data-ttu-id="42da1-227">b.</span><span class="sxs-lookup"><span data-stu-id="42da1-227">b.</span></span> <span data-ttu-id="42da1-228">Vyberte **Role**.</span><span class="sxs-lookup"><span data-stu-id="42da1-228">Select a **Role**.</span></span>
  
   <span data-ttu-id="42da1-229">c.</span><span class="sxs-lookup"><span data-stu-id="42da1-229">c.</span></span> <span data-ttu-id="42da1-230">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="42da1-230">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="42da1-231">Můžete použít všechny ostatní adaptivní Suite uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované adaptivní Suite tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="42da1-231">You can use any other Adaptive Suite user account creation tools or APIs provided by Adaptive Suite tooprovision AAD user accounts.</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="42da1-232">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="42da1-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="42da1-233">V této části povolíte tak, že udělíte přístup tooAdaptive Suite Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="42da1-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAdaptive Suite.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="42da1-235">**tooassign Britta Simon tooAdaptive Suite, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="42da1-235">**tooassign Britta Simon tooAdaptive Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="42da1-236">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="42da1-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="42da1-238">V seznamu aplikace hello vyberte **adaptivní Suite**.</span><span class="sxs-lookup"><span data-stu-id="42da1-238">In hello applications list, select **Adaptive Suite**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. <span data-ttu-id="42da1-240">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="42da1-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="42da1-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="42da1-242">Click **Add** button.</span></span> <span data-ttu-id="42da1-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="42da1-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="42da1-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="42da1-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="42da1-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="42da1-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="42da1-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="42da1-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="42da1-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="42da1-248">Testing single sign-on</span></span>

<span data-ttu-id="42da1-249">Hello cílem této části je tootest vaší Microsoft Azure AD Single Sign-On konfigurace pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="42da1-249">hello objective of this section is tootest your Microsoft Azure AD Single Sign-On configuration using hello Access Panel.</span></span>

<span data-ttu-id="42da1-250">Když kliknete na dlaždici adaptivní Suite hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour adaptivní sada aplikací.</span><span class="sxs-lookup"><span data-stu-id="42da1-250">When you click hello Adaptive Suite tile in hello Access Panel, you should get automatically signed-on tooyour Adaptive Suite application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="42da1-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="42da1-251">Additional resources</span></span>

* [<span data-ttu-id="42da1-252">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="42da1-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="42da1-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="42da1-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

