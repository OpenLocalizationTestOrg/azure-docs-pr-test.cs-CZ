---
title: "Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML JIRA microsoftem | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování SAML JIRA společností Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0dc847b9-eec4-4c31-845e-0144ddedc4a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 178c4c040d9939bca271ac185ca5c2feb14f1247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a><span data-ttu-id="b9875-103">Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML JIRA společností Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9875-103">Tutorial: Azure Active Directory integration with JIRA SAML SSO by Microsoft</span></span>

<span data-ttu-id="b9875-104">V tomto kurzu zjistíte, jak toointegrate jednotné přihlašování SAML JIRA společností Microsoft s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b9875-104">In this tutorial, you learn how toointegrate JIRA SAML SSO by Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b9875-105">Integrace jednotné přihlašování SAML JIRA společností Microsoft s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b9875-105">Integrating JIRA SAML SSO by Microsoft with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b9875-106">Můžete řídit ve službě Azure AD, který má přístup tooJIRA jednotné přihlašování SAML společností Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9875-106">You can control in Azure AD who has access tooJIRA SAML SSO by Microsoft</span></span>
- <span data-ttu-id="b9875-107">Vaši uživatelé tooautomatically get přihlášeného tooJIRA jednotné přihlašování SAML společností Microsoft (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9875-107">You can enable your users tooautomatically get signed-on tooJIRA SAML SSO by Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b9875-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b9875-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b9875-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b9875-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9875-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b9875-110">Prerequisites</span></span>

<span data-ttu-id="b9875-111">tooconfigure integrace Azure AD pomocí jednotného přihlašování SAML JIRA společností Microsoft, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b9875-111">tooconfigure Azure AD integration with JIRA SAML SSO by Microsoft, you need hello following items:</span></span>

- <span data-ttu-id="b9875-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9875-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b9875-113">JIRA aplikaci na server nainstalován na 64bitovou verzi Windows serveru (místní nebo v cloudu hello infrastruktury IaaS)</span><span class="sxs-lookup"><span data-stu-id="b9875-113">JIRA server application installed on a Windows 64-bit server (on premise or on hello cloud IaaS infrastructure)</span></span>
- <span data-ttu-id="b9875-114">JIRA server je povolen protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="b9875-114">JIRA server is HTTPS enabled</span></span>
- <span data-ttu-id="b9875-115">Poznámka hello podporované verze pro modul plug-in JIRA jsou uvedené v následující části.</span><span class="sxs-lookup"><span data-stu-id="b9875-115">Note hello supported versions for JIRA Plugin are mentioned in below section.</span></span>
- <span data-ttu-id="b9875-116">JIRA server je dostupný na Internetu zvlášť tooAzure AD přihlašovací stránka pro ověřování a musí mít tooreceive hello tokenu z Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9875-116">JIRA server is reachable on internet particularly tooAzure AD Login page for authentication and should able tooreceive hello token from Azure AD</span></span>
- <span data-ttu-id="b9875-117">Přihlašovací údaje správce se nastavují v JIRA</span><span class="sxs-lookup"><span data-stu-id="b9875-117">Admin credentials are set up in JIRA</span></span>
- <span data-ttu-id="b9875-118">V JIRA je zakázáno WebSudo</span><span class="sxs-lookup"><span data-stu-id="b9875-118">WebSudo is disabled in JIRA</span></span>
- <span data-ttu-id="b9875-119">Testovací uživatel vytvořené v hello JIRA serverové aplikace</span><span class="sxs-lookup"><span data-stu-id="b9875-119">Test user created in hello JIRA server application</span></span>

> [!NOTE]
> <span data-ttu-id="b9875-120">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí JIRA.</span><span class="sxs-lookup"><span data-stu-id="b9875-120">tootest hello steps in this tutorial, we do not recommend using a production environment of JIRA.</span></span> <span data-ttu-id="b9875-121">Integrace hello se nejdřív otestujte v vývoj nebo pracovní prostředí hello aplikace a pak použijte hello produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9875-121">Test hello integration first in development or staging environment of hello application and then use hello production environment.</span></span>

<span data-ttu-id="b9875-122">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b9875-122">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b9875-123">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b9875-123">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b9875-124">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b9875-124">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="supported-versions-of-jira"></a><span data-ttu-id="b9875-125">Podporované verze systému JIRA</span><span class="sxs-lookup"><span data-stu-id="b9875-125">Supported versions of JIRA</span></span> 

<span data-ttu-id="b9875-126">Od nyní jsou podporovány následující verze JIRA:</span><span class="sxs-lookup"><span data-stu-id="b9875-126">As of now, following versions of JIRA are supported:</span></span>

- <span data-ttu-id="b9875-127">Základní JIRA a Software: 6.0 too7.2.0</span><span class="sxs-lookup"><span data-stu-id="b9875-127">JIRA Core and Software: 6.0 too7.2.0</span></span>
- <span data-ttu-id="b9875-128">JIRA technickou podporu: 3.0 too3.2</span><span class="sxs-lookup"><span data-stu-id="b9875-128">JIRA Service Desk: 3.0 too3.2</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b9875-129">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b9875-129">Scenario description</span></span>
<span data-ttu-id="b9875-130">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9875-130">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b9875-131">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b9875-131">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b9875-132">Přidání jednotné přihlašování SAML JIRA společností Microsoft z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b9875-132">Adding JIRA SAML SSO by Microsoft from hello gallery</span></span>
2. <span data-ttu-id="b9875-133">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b9875-133">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jira-saml-sso-by-microsoft-from-hello-gallery"></a><span data-ttu-id="b9875-134">Přidání jednotné přihlašování SAML JIRA společností Microsoft z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b9875-134">Adding JIRA SAML SSO by Microsoft from hello gallery</span></span>
<span data-ttu-id="b9875-135">tooconfigure hello integrace přihlašování SAML JIRA společností Microsoft do služby Azure AD, je nutné tooadd jednotné přihlašování SAML JIRA Microsoft hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b9875-135">tooconfigure hello integration of JIRA SAML SSO by Microsoft into Azure AD, you need tooadd JIRA SAML SSO by Microsoft from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b9875-136">**tooadd jednotné přihlašování SAML JIRA společností Microsoft z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b9875-136">**tooadd JIRA SAML SSO by Microsoft from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9875-137">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b9875-137">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b9875-139">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b9875-139">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b9875-140">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b9875-140">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b9875-142">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b9875-142">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b9875-144">Hello vyhledávacího pole zadejte **jednotné přihlašování SAML JIRA Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b9875-144">In hello search box, type **JIRA SAML SSO by Microsoft**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_search.png)

5. <span data-ttu-id="b9875-146">Na panelu výsledků hello vyberte **jednotné přihlašování SAML JIRA Microsoft**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9875-146">In hello results panel, select **JIRA SAML SSO by Microsoft**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b9875-148">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b9875-148">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b9875-149">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML JIRA společnosti Microsoft podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b9875-149">In this section, you configure and test Azure AD single sign-on with JIRA SAML SSO by Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b9875-150">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v jednotné přihlašování SAML JIRA společností Microsoft je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b9875-150">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in JIRA SAML SSO by Microsoft is tooa user in Azure AD.</span></span> <span data-ttu-id="b9875-151">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v jednotné přihlašování SAML JIRA společností Microsoft musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b9875-151">In other words, a link relationship between an Azure AD user and hello related user in JIRA SAML SSO by Microsoft needs toobe established.</span></span>

<span data-ttu-id="b9875-152">V JIRA jednotné přihlašování SAML společností Microsoft, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b9875-152">In JIRA SAML SSO by Microsoft, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b9875-153">tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML JIRA společností Microsoft, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="b9875-153">tooconfigure and test Azure AD single sign-on with JIRA SAML SSO by Microsoft, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b9875-154">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b9875-154">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b9875-155">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b9875-155">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b9875-156">**[Vytváření jednotné přihlašování SAML JIRA uživatelem testovací Microsoft](#creating-a-jira-saml-sso-by-microsoft-test-user)**  -toohave protějšek Britta Simon v jednotné přihlašování SAML JIRA společnosti Microsoft, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b9875-156">**[Creating a JIRA SAML SSO by Microsoft test user](#creating-a-jira-saml-sso-by-microsoft-test-user)** - toohave a counterpart of Britta Simon in JIRA SAML SSO by Microsoft that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b9875-157">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b9875-157">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b9875-158">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b9875-158">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b9875-159">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b9875-159">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b9875-160">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší jednotné přihlašování SAML JIRA aplikací společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9875-160">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your JIRA SAML SSO by Microsoft application.</span></span>

<span data-ttu-id="b9875-161">**tooconfigure Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML JIRA společností Microsoft, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b9875-161">**tooconfigure Azure AD single sign-on with JIRA SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9875-162">V portálu Azure, na hello hello **jednotné přihlašování SAML JIRA Microsoft** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b9875-162">In hello Azure portal, on hello **JIRA SAML SSO by Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b9875-164">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b9875-164">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_samlbase.png)

3. <span data-ttu-id="b9875-166">Na hello **jednotné přihlašování SAML JIRA Microsoft Domain a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b9875-166">On hello **JIRA SAML SSO by Microsoft Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_url.png)

    <span data-ttu-id="b9875-168">a.</span><span class="sxs-lookup"><span data-stu-id="b9875-168">a.</span></span> <span data-ttu-id="b9875-169">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="b9875-169">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    <span data-ttu-id="b9875-170">b.</span><span class="sxs-lookup"><span data-stu-id="b9875-170">b.</span></span> <span data-ttu-id="b9875-171">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<domain:port>/`</span><span class="sxs-lookup"><span data-stu-id="b9875-171">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<domain:port>/`</span></span>

    <span data-ttu-id="b9875-172">c.</span><span class="sxs-lookup"><span data-stu-id="b9875-172">c.</span></span> <span data-ttu-id="b9875-173">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="b9875-173">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b9875-174">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b9875-174">These values are not real.</span></span> <span data-ttu-id="b9875-175">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="b9875-175">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="b9875-176">Port je volitelný, v případě, že je adresa URL s názvem.</span><span class="sxs-lookup"><span data-stu-id="b9875-176">Port is optional in case it’s a named URL.</span></span> <span data-ttu-id="b9875-177">Tyto hodnoty jsou přijímány během konfigurace hello Jira modul plug-in, který je vysvětlen později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="b9875-177">These values are received during hello configuration of Jira plugin, which is explained later in hello tutorial.</span></span>
 
4. <span data-ttu-id="b9875-178">toogenerate hello **Metadata** adresu url, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b9875-178">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="b9875-179">a.</span><span class="sxs-lookup"><span data-stu-id="b9875-179">a.</span></span> <span data-ttu-id="b9875-180">Klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b9875-180">Click **App registrations**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/appregistrations.png)
   
    <span data-ttu-id="b9875-182">b.</span><span class="sxs-lookup"><span data-stu-id="b9875-182">b.</span></span> <span data-ttu-id="b9875-183">Klikněte na tlačítko **koncové body** tooopen **koncové body** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b9875-183">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/endpointicon.png)

    <span data-ttu-id="b9875-185">c.</span><span class="sxs-lookup"><span data-stu-id="b9875-185">c.</span></span> <span data-ttu-id="b9875-186">Klikněte na tlačítko hello kopie tlačítko toocopy **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="b9875-186">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/endpoint.png)
     
    <span data-ttu-id="b9875-188">d.</span><span class="sxs-lookup"><span data-stu-id="b9875-188">d.</span></span> <span data-ttu-id="b9875-189">Nyní přejděte na stránce vlastností toohello **jednotné přihlašování SAML JIRA Microsoft** a kopírování hello **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="b9875-189">Now go toohello property page of **JIRA SAML SSO by Microsoft** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/appid.png)

    <span data-ttu-id="b9875-191">e.</span><span class="sxs-lookup"><span data-stu-id="b9875-191">e.</span></span> <span data-ttu-id="b9875-192">Generovat hello **adresu URL metadat** pomocí hello následující vzor: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` a zkopírujte tuto hodnotu v poznámkovém bloku, jak se později používá pro konfiguraci hello hello modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="b9875-192">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` and copy this value in notepad as it is used later for hello configuration of hello plugin.</span></span>

5. <span data-ttu-id="b9875-193">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b9875-193">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b9875-195">Obraťte se na [Microsoft](mailto:waadpartners@microsoft.com) s hello následující informace pro modul plug-in JIRA hello.</span><span class="sxs-lookup"><span data-stu-id="b9875-195">Contact [Microsoft](mailto:waadpartners@microsoft.com) with hello following information for hello JIRA plugin.</span></span>
    
    *   <span data-ttu-id="b9875-196">Jméno zákazníka:</span><span class="sxs-lookup"><span data-stu-id="b9875-196">Customer Name:</span></span>
    *   <span data-ttu-id="b9875-197">Název domény:</span><span class="sxs-lookup"><span data-stu-id="b9875-197">Primary domain name:</span></span>
    *   <span data-ttu-id="b9875-198">Azure AD Premium: Ano/Ne (modul plug-in bude k dispozici tooall hello zákazníka volné, Basic a skladová položka Premium)</span><span class="sxs-lookup"><span data-stu-id="b9875-198">Azure AD Premium: Yes/No (Plugin will be available tooall     hello customer Free, Basic, and Premium SKU)</span></span>
    *   <span data-ttu-id="b9875-199">Počet uživatelů, kteří budou používat Tato integrace:</span><span class="sxs-lookup"><span data-stu-id="b9875-199">Number of users who will be using this integration:</span></span>
    *   <span data-ttu-id="b9875-200">JIRA verze:</span><span class="sxs-lookup"><span data-stu-id="b9875-200">JIRA Version:</span></span>
    *   <span data-ttu-id="b9875-201">Poznámka:</span><span class="sxs-lookup"><span data-stu-id="b9875-201">Comments:</span></span>

7. <span data-ttu-id="b9875-202">V okně prohlížeče jiný web Přihlaste se jako správce v instanci JIRA tooyour.</span><span class="sxs-lookup"><span data-stu-id="b9875-202">In a different web browser window, log in tooyour JIRA instance as an administrator.</span></span>

8. <span data-ttu-id="b9875-203">Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **doplňky**.</span><span class="sxs-lookup"><span data-stu-id="b9875-203">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/addon1.png)

9. <span data-ttu-id="b9875-205">Karta části doplňky, klikněte na tlačítko **spravovat doplňky**.</span><span class="sxs-lookup"><span data-stu-id="b9875-205">Under Add-ons tab section, click **Manage add-ons**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/addon7.png)

10. <span data-ttu-id="b9875-207">Ručně odešlete modulu plug-in hello od společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9875-207">Manually upload hello plugin provided by Microsoft.</span></span> <span data-ttu-id="b9875-208">Po instalaci modulu plug-in hello se zobrazí v **uživatel nainstaloval** doplňky části **spravovat rozšíření** části.</span><span class="sxs-lookup"><span data-stu-id="b9875-208">Once hello plugin is installed, it appears in **User Installed** add-ons section of **Manage Add-on** section.</span></span>

11. <span data-ttu-id="b9875-209">Klikněte na tlačítko **konfigurace** tooconfigure hello nového modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="b9875-209">Click **Configure** tooconfigure hello new plugin.</span></span>

12. <span data-ttu-id="b9875-210">Proveďte následující kroky na stránce konfigurace:</span><span class="sxs-lookup"><span data-stu-id="b9875-210">Perform following steps on configuration page:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/addon5.png)
 
    <span data-ttu-id="b9875-212">a.</span><span class="sxs-lookup"><span data-stu-id="b9875-212">a.</span></span> <span data-ttu-id="b9875-213">V **adresu URL metadat** vložit hello **adresu URL metadat** generované z Azure AD a klikněte na tlačítko hello **vyřešit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b9875-213">In **Metadata URL** paste hello **Metadata URL** generated from Azure AD and click hello **Resolve** button.</span></span> <span data-ttu-id="b9875-214">Přečte adresu URL metadat hello IdP a naplní všechny informace pole hello.</span><span class="sxs-lookup"><span data-stu-id="b9875-214">It reads hello IdP metadata URL and populates all hello fields information.</span></span>

    > [!Note]
    > <span data-ttu-id="b9875-215">Výchozí umístění SAML ID uživatele je identifikátor jména.</span><span class="sxs-lookup"><span data-stu-id="b9875-215">Default SAML User ID location is Name Identifier.</span></span> <span data-ttu-id="b9875-216">Můžete změnit tato tooan atribut možnost a zadejte název odpovídajícího atributu hello.</span><span class="sxs-lookup"><span data-stu-id="b9875-216">You can change this tooan attribute option and enter hello appropriate attribute name.</span></span>

    > [!TIP]
    > <span data-ttu-id="b9875-217">Zajistěte, aby pouze jeden certifikát namapované proti hello aplikace tak, aby se nezobrazí žádná chyba při řešení hello metadat.</span><span class="sxs-lookup"><span data-stu-id="b9875-217">Ensure that there is only one certificate mapped against hello app so that there is no error in resolving hello metadata.</span></span> <span data-ttu-id="b9875-218">Pokud máte víc certifikátů, při řešení hello metadata, získá správce k chybě.</span><span class="sxs-lookup"><span data-stu-id="b9875-218">If there are multiple certificates, upon resolving hello metadata, admin gets an error.</span></span>
    
    <span data-ttu-id="b9875-219">b.</span><span class="sxs-lookup"><span data-stu-id="b9875-219">b.</span></span> <span data-ttu-id="b9875-220">Kopírování hello **identifikátor, adresa URL odpovědi a adresa URL přihlašování** hodnoty a vložte je do **identifikátor, adresa URL odpovědi a adresa URL přihlašování** textových polí v uvedeném pořadí v **jednotné přihlašování SAML JIRA Microsoft Domain a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b9875-220">Copy hello **Identifier, Reply URL and Sign on URL** values and paste them in **Identifier, Reply URL and Sign on URL** textboxes respectively in **JIRA SAML SSO by Microsoft Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="b9875-221">c.</span><span class="sxs-lookup"><span data-stu-id="b9875-221">c.</span></span> <span data-ttu-id="b9875-222">V **přihlašovací jméno tlačítko** hello název typu tlačítka vaše organizace hodlá hello toosee uživatele na přihlašovací obrazovce.</span><span class="sxs-lookup"><span data-stu-id="b9875-222">In **Login Button Name** type hello name of button your organization wants hello users toosee on login screen.</span></span>

    <span data-ttu-id="b9875-223">d.</span><span class="sxs-lookup"><span data-stu-id="b9875-223">d.</span></span> <span data-ttu-id="b9875-224">V **SAML uživatele ID umístění** vyberte buď **ID uživatele je v elementu NameIdentifier hello hello subjektu příkaz** nebo **ID uživatele je v elementu atribut**.</span><span class="sxs-lookup"><span data-stu-id="b9875-224">In **SAML User ID Locations** select either **User ID is in hello NameIdentifier element of hello Subject statement** or **User ID is in an Attribute element**.</span></span>  <span data-ttu-id="b9875-225">Toto ID má id uživatele JIRA toobe hello. Pokud id uživatele hello neodpovídá, systém nedovolí toolog uživatelé v.</span><span class="sxs-lookup"><span data-stu-id="b9875-225">This ID has toobe hello JIRA user id. If hello user id is not matched, then system will not allow users toolog in.</span></span> 
    
    <span data-ttu-id="b9875-226">e.</span><span class="sxs-lookup"><span data-stu-id="b9875-226">e.</span></span> <span data-ttu-id="b9875-227">Pokud vyberete **ID uživatele je v elementu atribut** možnost, pak v **název atributu** textového pole Název hello typu atributu hello, kde je očekávána Id uživatele.</span><span class="sxs-lookup"><span data-stu-id="b9875-227">If you select **User ID is in an Attribute element** option, then in **Attribute name** textbox type hello name of hello attribute where User Id is expected.</span></span> 

    <span data-ttu-id="b9875-228">f.</span><span class="sxs-lookup"><span data-stu-id="b9875-228">f.</span></span> <span data-ttu-id="b9875-229">Pokud používáte hello federované domény (například služby AD FS atd.) s Azure AD, potom klikněte na hello **povolit zjišťování domovské sféry** řádku a nakonfigurovat hello **název domény**.</span><span class="sxs-lookup"><span data-stu-id="b9875-229">If you are using hello federated domain (like ADFS etc.) with Azure AD, then click on hello **Enable Home Realm Discovery** option and configure hello **Domain Name**.</span></span>
    
    <span data-ttu-id="b9875-230">g.</span><span class="sxs-lookup"><span data-stu-id="b9875-230">g.</span></span> <span data-ttu-id="b9875-231">V **název domény** hello domény sem napište název v případě hello přihlášení na základě služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="b9875-231">In **Domain Name** type hello domain name here in case of hello ADFS-based login.</span></span>

    <span data-ttu-id="b9875-232">h.</span><span class="sxs-lookup"><span data-stu-id="b9875-232">h.</span></span> <span data-ttu-id="b9875-233">Zkontrolujte **povolit jednotné přihlašování se** z Azure AD, když se uživatel neodhlásí z JIRA toolog si chcete.</span><span class="sxs-lookup"><span data-stu-id="b9875-233">Check **Enable Single Sign out** if you wish toolog out from Azure AD when a user logs out from JIRA.</span></span> 

    <span data-ttu-id="b9875-234">i.</span><span class="sxs-lookup"><span data-stu-id="b9875-234">i.</span></span> <span data-ttu-id="b9875-235">Klikněte na tlačítko **Uložit** tlačítko toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="b9875-235">Click **Save** button toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="b9875-236">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b9875-236">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b9875-237">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b9875-237">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b9875-238">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b9875-238">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b9875-239">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9875-239">Creating an Azure AD test user</span></span>
<span data-ttu-id="b9875-240">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b9875-240">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b9875-242">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b9875-242">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9875-243">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b9875-243">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b9875-245">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b9875-245">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b9875-247">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b9875-247">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b9875-249">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b9875-249">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b9875-251">a.</span><span class="sxs-lookup"><span data-stu-id="b9875-251">a.</span></span> <span data-ttu-id="b9875-252">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b9875-252">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b9875-253">b.</span><span class="sxs-lookup"><span data-stu-id="b9875-253">b.</span></span> <span data-ttu-id="b9875-254">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b9875-254">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b9875-255">c.</span><span class="sxs-lookup"><span data-stu-id="b9875-255">c.</span></span> <span data-ttu-id="b9875-256">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b9875-256">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b9875-257">d.</span><span class="sxs-lookup"><span data-stu-id="b9875-257">d.</span></span> <span data-ttu-id="b9875-258">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b9875-258">Click **Create**.</span></span>
 
### <a name="creating-a-jira-saml-sso-by-microsoft-test-user"></a><span data-ttu-id="b9875-259">Vytváření jednotné přihlašování SAML JIRA Microsoft testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="b9875-259">Creating a JIRA SAML SSO by Microsoft test user</span></span>

<span data-ttu-id="b9875-260">Uživatelé toolog tooenable Azure AD v tooJIRA na místním serveru, se musí být zřízená do jednotné přihlašování SAML JIRA společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9875-260">tooenable Azure AD users toolog in tooJIRA on-premise server, they must be provisioned into JIRA SAML SSO by Microsoft.</span></span> <span data-ttu-id="b9875-261">Pro jednotné přihlašování SAML JIRA společností Microsoft zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b9875-261">For JIRA SAML SSO by Microsoft, provisioning is a manual task.</span></span>

<span data-ttu-id="b9875-262">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b9875-262">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9875-263">Přihlaste se tooyour JIRA místní server jako správce.</span><span class="sxs-lookup"><span data-stu-id="b9875-263">Log in tooyour JIRA on-premise server as an administrator.</span></span>

2. <span data-ttu-id="b9875-264">Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="b9875-264">Hover on cog and click hello **User management**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user1.png) 

3. <span data-ttu-id="b9875-266">Jsou přesměrovaného tooAdministrator přístup stránky tooenter **heslo** a klikněte na tlačítko **potvrdit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b9875-266">You are redirected tooAdministrator Access page tooenter **Password** and click **Confirm** button.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user2.png) 

4. <span data-ttu-id="b9875-268">V části **Správa uživatelů** oddíl, klikněte na **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b9875-268">Under **User management** tab section, click **create user**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user3.png) 

5. <span data-ttu-id="b9875-270">Na hello **"Vytvořit nový uživatel"** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b9875-270">On hello **“Create new user”** dialog page, perform hello following steps:</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user4.png) 

    <span data-ttu-id="b9875-272">a.</span><span class="sxs-lookup"><span data-stu-id="b9875-272">a.</span></span> <span data-ttu-id="b9875-273">V hello **e-mailová adresa** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="b9875-273">In hello **Email address** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="b9875-274">b.</span><span class="sxs-lookup"><span data-stu-id="b9875-274">b.</span></span> <span data-ttu-id="b9875-275">V hello **úplný název** textovému poli, úplný název typu hello uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b9875-275">In hello **Full Name** textbox, type full name of hello user like Britta Simon.</span></span>

    <span data-ttu-id="b9875-276">c.</span><span class="sxs-lookup"><span data-stu-id="b9875-276">c.</span></span> <span data-ttu-id="b9875-277">V hello **uživatelské jméno** jako typ hello e-mailu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="b9875-277">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="b9875-278">d.</span><span class="sxs-lookup"><span data-stu-id="b9875-278">d.</span></span> <span data-ttu-id="b9875-279">V hello **heslo** textovému poli, zadejte hello heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="b9875-279">In hello **Password** textbox, type hello password of user.</span></span>

    <span data-ttu-id="b9875-280">e.</span><span class="sxs-lookup"><span data-stu-id="b9875-280">e.</span></span> <span data-ttu-id="b9875-281">Klikněte na tlačítko **vytvořit uživateli**.</span><span class="sxs-lookup"><span data-stu-id="b9875-281">Click **Create user**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b9875-282">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b9875-282">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b9875-283">V této části je povolit Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup tooJIRA jednotné přihlašování SAML společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9875-283">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJIRA SAML SSO by Microsoft.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b9875-285">**tooassign tooJIRA Britta Simon jednotné přihlašování SAML společností Microsoft, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b9875-285">**tooassign Britta Simon tooJIRA SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9875-286">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b9875-286">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b9875-288">V seznamu aplikace hello vyberte **jednotné přihlašování SAML JIRA Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b9875-288">In hello applications list, select **JIRA SAML SSO by Microsoft**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_app.png) 

3. <span data-ttu-id="b9875-290">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b9875-290">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b9875-292">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b9875-292">Click **Add** button.</span></span> <span data-ttu-id="b9875-293">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b9875-293">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b9875-295">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b9875-295">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b9875-296">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b9875-296">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b9875-297">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b9875-297">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b9875-298">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b9875-298">Testing single sign-on</span></span>

<span data-ttu-id="b9875-299">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b9875-299">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b9875-300">Po kliknutí na tlačítko hello jednotné přihlašování SAML JIRA podle Microsoft dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour jednotné přihlašování SAML JIRA aplikací společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9875-300">When you click hello JIRA SAML SSO by Microsoft tile in hello Access Panel, you should get automatically signed-on tooyour JIRA SAML SSO by Microsoft application.</span></span>
<span data-ttu-id="b9875-301">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b9875-301">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b9875-302">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b9875-302">Additional resources</span></span>

* [<span data-ttu-id="b9875-303">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b9875-303">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b9875-304">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b9875-304">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_203.png

