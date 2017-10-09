---
title: 'Kurz: Azure Active Directory integrace s Zscaler jeden | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Zscaler jeden."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 5179cb2cc54482334d574951a1ac64e722e5f578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-one"></a><span data-ttu-id="7f152-103">Kurz: Azure Active Directory integrace s Zscaler jeden</span><span class="sxs-lookup"><span data-stu-id="7f152-103">Tutorial: Azure Active Directory integration with Zscaler One</span></span>

<span data-ttu-id="7f152-104">V tomto kurzu zjistíte, jak toointegrate Zscaler jeden službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7f152-104">In this tutorial, you learn how toointegrate Zscaler One with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7f152-105">Integrace s Azure AD Zscaler jeden poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7f152-105">Integrating Zscaler One with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7f152-106">Můžete řídit ve službě Azure AD, který má přístup tooZscaler jeden</span><span class="sxs-lookup"><span data-stu-id="7f152-106">You can control in Azure AD who has access tooZscaler One</span></span>
- <span data-ttu-id="7f152-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooZscaler jeden (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f152-107">You can enable your users tooautomatically get signed-on tooZscaler One (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7f152-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7f152-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7f152-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7f152-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f152-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f152-110">Prerequisites</span></span>

<span data-ttu-id="7f152-111">tooconfigure integrace Azure AD s Zscaler jeden, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7f152-111">tooconfigure Azure AD integration with Zscaler One, you need hello following items:</span></span>

- <span data-ttu-id="7f152-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f152-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7f152-113">Jednu Zscaler jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7f152-113">A Zscaler One single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7f152-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f152-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7f152-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7f152-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7f152-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7f152-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7f152-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7f152-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7f152-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7f152-118">Scenario description</span></span>
<span data-ttu-id="7f152-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f152-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7f152-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7f152-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7f152-121">Přidání Zscaler jeden z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7f152-121">Adding Zscaler One from hello gallery</span></span>
2. <span data-ttu-id="7f152-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f152-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-one-from-hello-gallery"></a><span data-ttu-id="7f152-123">Přidání Zscaler jeden z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7f152-123">Adding Zscaler One from hello gallery</span></span>
<span data-ttu-id="7f152-124">tooconfigure hello integrace Zscaler jeden do Azure AD, je nutné tooadd Zscaler jeden z hello Galerie tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7f152-124">tooconfigure hello integration of Zscaler One into Azure AD, you need tooadd Zscaler One from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7f152-125">**tooadd Zscaler jeden z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f152-125">**tooadd Zscaler One from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f152-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7f152-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7f152-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7f152-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7f152-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f152-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7f152-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f152-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7f152-133">Hello vyhledávacího pole zadejte **Zscaler jeden**.</span><span class="sxs-lookup"><span data-stu-id="7f152-133">In hello search box, type **Zscaler One**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_search.png)

5. <span data-ttu-id="7f152-135">Na panelu výsledků hello vyberte **Zscaler jeden**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f152-135">In hello results panel, select **Zscaler One**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7f152-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f152-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7f152-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler jeden podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7f152-138">In this section, you configure and test Azure AD single sign-on with Zscaler One based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7f152-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Zscaler jeden je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f152-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler One is tooa user in Azure AD.</span></span> <span data-ttu-id="7f152-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Zscaler jeden musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="7f152-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler One needs toobe established.</span></span>

<span data-ttu-id="7f152-141">V jedné Zscaler přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="7f152-141">In Zscaler One, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7f152-142">tooconfigure a testu Azure AD jednotné přihlašování s Zscaler jeden, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="7f152-142">tooconfigure and test Azure AD single sign-on with Zscaler One, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7f152-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7f152-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7f152-144">**[Konfigurace nastavení proxy serveru](#configuring-proxy-settings)**  -tooconfigure hello nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="7f152-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="7f152-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7f152-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="7f152-146">**[Vytváření Zscaler jeden testovací uživatele](#creating-a-zscaler-one-test-user)**  -toohave protějšek Britta Simon v Zscaler jeden, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="7f152-146">**[Creating a Zscaler One test user](#creating-a-zscaler-one-test-user)** - toohave a counterpart of Britta Simon in Zscaler One that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="7f152-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f152-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="7f152-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="7f152-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7f152-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f152-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7f152-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v Zscaler jednu aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7f152-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler One application.</span></span>

<span data-ttu-id="7f152-151">**tooconfigure Azure AD jednotné přihlašování s Zscaler jeden proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7f152-151">**tooconfigure Azure AD single sign-on with Zscaler One, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f152-152">V portálu Azure, na hello hello **Zscaler jeden** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7f152-152">In hello Azure portal, on hello **Zscaler One** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7f152-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f152-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_samlbase.png)

3. <span data-ttu-id="7f152-156">Na hello **Zscaler jednu doménu a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7f152-156">On hello **Zscaler One Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_url.png)

    <span data-ttu-id="7f152-158">V textovém poli hello přihlašovací adresa URL zadejte adresu URL hello používá vaše tooyour toosign na uživatele Zscaler jednu aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7f152-158">In hello Sign-on URL textbox, type hello URL used by your users toosign-on tooyour Zscaler One application.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7f152-159">Máte tooupdate tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f152-159">You have tooupdate this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="7f152-160">Obraťte se na [tým podpory pro jednoho klienta Zscaler](https://www.zscaler.com/company/contact) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7f152-160">Contact [Zscaler One Client support team](https://www.zscaler.com/company/contact) tooget these values.</span></span>

4. <span data-ttu-id="7f152-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7f152-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_certificate.png) 

5. <span data-ttu-id="7f152-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f152-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7f152-165">Na hello **jednu konfiguraci Zscaler** klikněte na tlačítko **nakonfigurovat jeden Zscaler** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7f152-165">On hello **Zscaler One Configuration** section, click **Configure Zscaler One** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7f152-166">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7f152-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_configure.png) 

7. <span data-ttu-id="7f152-168">V okně prohlížeče jiný web Přihlaste se jako správce v tooyour Zscaler jeden web společnosti.</span><span class="sxs-lookup"><span data-stu-id="7f152-168">In a different web browser window, log in tooyour Zscaler One company site as an administrator.</span></span>

8. <span data-ttu-id="7f152-169">V nabídce hello hello nahoře, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="7f152-169">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="7f152-170">![Správa](./media/active-directory-saas-zscaler-one-tutorial/ic800206.png "správy")</span><span class="sxs-lookup"><span data-stu-id="7f152-170">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="7f152-171">V části **spravovat správci & role**, klikněte na tlačítko **Správa uživatelů a ověřování**.</span><span class="sxs-lookup"><span data-stu-id="7f152-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="7f152-172">![Správa uživatelů a ověřování](./media/active-directory-saas-zscaler-one-tutorial/ic800207.png "správu uživatelů a ověřování")</span><span class="sxs-lookup"><span data-stu-id="7f152-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-one-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="7f152-173">V hello **zvolte možnosti ověřování pro vaši organizaci** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7f152-173">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="7f152-174">![Ověřování](./media/active-directory-saas-zscaler-one-tutorial/ic800208.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="7f152-174">![Authentication](./media/active-directory-saas-zscaler-one-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="7f152-175">a.</span><span class="sxs-lookup"><span data-stu-id="7f152-175">a.</span></span> <span data-ttu-id="7f152-176">Vyberte **ověření pomocí SAML Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="7f152-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="7f152-177">b.</span><span class="sxs-lookup"><span data-stu-id="7f152-177">b.</span></span> <span data-ttu-id="7f152-178">Klikněte na tlačítko **konfigurace SAML jeden přihlašování parametrů**.</span><span class="sxs-lookup"><span data-stu-id="7f152-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="7f152-179">Na hello **konfigurace SAML jeden přihlašování parametry** dialogové okno stránky, proveďte následující kroky hello a pak klikněte na **provést**</span><span class="sxs-lookup"><span data-stu-id="7f152-179">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="7f152-180">![Jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/ic800209.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="7f152-180">![Single Sign-On](./media/active-directory-saas-zscaler-one-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="7f152-181">a.</span><span class="sxs-lookup"><span data-stu-id="7f152-181">a.</span></span> <span data-ttu-id="7f152-182">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **URL hello SAML portál toowhich uživatelů jsou odesílány pro ověřování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="7f152-182">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="7f152-183">b.</span><span class="sxs-lookup"><span data-stu-id="7f152-183">b.</span></span> <span data-ttu-id="7f152-184">V hello **atribut obsahující přihlašovací jméno** textovému poli, typ **NameID**.</span><span class="sxs-lookup"><span data-stu-id="7f152-184">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="7f152-185">c.</span><span class="sxs-lookup"><span data-stu-id="7f152-185">c.</span></span> <span data-ttu-id="7f152-186">tooupload stažený certifikát, klikněte na tlačítko **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="7f152-186">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="7f152-187">d.</span><span class="sxs-lookup"><span data-stu-id="7f152-187">d.</span></span> <span data-ttu-id="7f152-188">Vyberte **zapnout automatické zřizování SAML**.</span><span class="sxs-lookup"><span data-stu-id="7f152-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="7f152-189">Na hello **konfigurace ověřování uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f152-189">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="7f152-190">![Správa](./media/active-directory-saas-zscaler-one-tutorial/ic800210.png "správy")</span><span class="sxs-lookup"><span data-stu-id="7f152-190">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="7f152-191">a.</span><span class="sxs-lookup"><span data-stu-id="7f152-191">a.</span></span> <span data-ttu-id="7f152-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7f152-192">Click **Save**.</span></span>

    <span data-ttu-id="7f152-193">b.</span><span class="sxs-lookup"><span data-stu-id="7f152-193">b.</span></span> <span data-ttu-id="7f152-194">Klikněte na tlačítko **aktivovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="7f152-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="7f152-195">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="7f152-195">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="7f152-196">tooconfigure hello nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="7f152-196">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="7f152-197">Spustit **aplikace Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="7f152-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="7f152-198">Vyberte **Možnosti Internetu** z hello **nástroje** nabídku pro otevřete hello **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f152-198">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="7f152-199">![Možnosti Internetu](./media/active-directory-saas-zscaler-one-tutorial/ic769492.png "Možnosti Internetu")</span><span class="sxs-lookup"><span data-stu-id="7f152-199">![Internet Options](./media/active-directory-saas-zscaler-one-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="7f152-200">Klikněte na tlačítko hello **připojení** kartě.</span><span class="sxs-lookup"><span data-stu-id="7f152-200">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="7f152-201">![Připojení](./media/active-directory-saas-zscaler-one-tutorial/ic769493.png "připojení")</span><span class="sxs-lookup"><span data-stu-id="7f152-201">![Connections](./media/active-directory-saas-zscaler-one-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="7f152-202">Klikněte na tlačítko **nastavení místní sítě** tooopen hello **nastavení místní sítě** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f152-202">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="7f152-203">V části Proxy server hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f152-203">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="7f152-204">![Proxy server](./media/active-directory-saas-zscaler-one-tutorial/ic769494.png "Proxy serveru")</span><span class="sxs-lookup"><span data-stu-id="7f152-204">![Proxy server](./media/active-directory-saas-zscaler-one-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="7f152-205">a.</span><span class="sxs-lookup"><span data-stu-id="7f152-205">a.</span></span> <span data-ttu-id="7f152-206">Vyberte **použít proxy server pro síť LAN**.</span><span class="sxs-lookup"><span data-stu-id="7f152-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="7f152-207">b.</span><span class="sxs-lookup"><span data-stu-id="7f152-207">b.</span></span> <span data-ttu-id="7f152-208">V textovém poli hello adresu, zadejte **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="7f152-208">In hello Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="7f152-209">c.</span><span class="sxs-lookup"><span data-stu-id="7f152-209">c.</span></span> <span data-ttu-id="7f152-210">V textovém poli hello Port, zadejte **80**.</span><span class="sxs-lookup"><span data-stu-id="7f152-210">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="7f152-211">d.</span><span class="sxs-lookup"><span data-stu-id="7f152-211">d.</span></span> <span data-ttu-id="7f152-212">Vyberte **Nepoužívat proxy server pro místní adresy**.</span><span class="sxs-lookup"><span data-stu-id="7f152-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="7f152-213">e.</span><span class="sxs-lookup"><span data-stu-id="7f152-213">e.</span></span> <span data-ttu-id="7f152-214">Klikněte na tlačítko **OK** tooclose hello **nastavení místní sítě (LAN)** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f152-214">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="7f152-215">Klikněte na tlačítko **OK** tooclose hello **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f152-215">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="7f152-216">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="7f152-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7f152-217">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="7f152-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7f152-218">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7f152-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7f152-219">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f152-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="7f152-220">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="7f152-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7f152-222">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f152-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f152-223">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7f152-223">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7f152-225">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7f152-225">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7f152-227">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7f152-227">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7f152-229">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f152-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7f152-231">a.</span><span class="sxs-lookup"><span data-stu-id="7f152-231">a.</span></span> <span data-ttu-id="7f152-232">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7f152-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7f152-233">b.</span><span class="sxs-lookup"><span data-stu-id="7f152-233">b.</span></span> <span data-ttu-id="7f152-234">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7f152-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7f152-235">c.</span><span class="sxs-lookup"><span data-stu-id="7f152-235">c.</span></span> <span data-ttu-id="7f152-236">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7f152-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7f152-237">d.</span><span class="sxs-lookup"><span data-stu-id="7f152-237">d.</span></span> <span data-ttu-id="7f152-238">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7f152-238">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-one-test-user"></a><span data-ttu-id="7f152-239">Vytváření Zscaler jeden testovací uživatele</span><span class="sxs-lookup"><span data-stu-id="7f152-239">Creating a Zscaler One test user</span></span>

<span data-ttu-id="7f152-240">Uživatelé toolog tooenable Azure AD v tooZscaler jeden, musí být zřízená tooZscaler jeden.</span><span class="sxs-lookup"><span data-stu-id="7f152-240">tooenable Azure AD users toolog in tooZscaler One, they must be provisioned tooZscaler One.</span></span> <span data-ttu-id="7f152-241">V případě hello Zscaler jedné zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="7f152-241">In hello case of Zscaler One, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="7f152-242">tooconfigure zřizování uživatelů, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7f152-242">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="7f152-243">Přihlaste se tooyour **Zscaler jeden** klienta.</span><span class="sxs-lookup"><span data-stu-id="7f152-243">Log in tooyour **Zscaler One** tenant.</span></span>

2. <span data-ttu-id="7f152-244">Klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="7f152-244">Click **Administration**.</span></span>   
   
    <span data-ttu-id="7f152-245">![Správa](./media/active-directory-saas-zscaler-one-tutorial/ic781035.png "správy")</span><span class="sxs-lookup"><span data-stu-id="7f152-245">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="7f152-246">Klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="7f152-246">Click **User Management**.</span></span>   
        
     <span data-ttu-id="7f152-247">![Přidat](./media/active-directory-saas-zscaler-one-tutorial/ic781036.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="7f152-247">![Add](./media/active-directory-saas-zscaler-one-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="7f152-248">V hello **uživatelé** , klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7f152-248">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="7f152-249">![Přidat](./media/active-directory-saas-zscaler-one-tutorial/ic781037.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="7f152-249">![Add](./media/active-directory-saas-zscaler-one-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="7f152-250">V části přidat uživatele hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f152-250">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="7f152-251">![Přidat uživatele](./media/active-directory-saas-zscaler-one-tutorial/ic781038.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="7f152-251">![Add User](./media/active-directory-saas-zscaler-one-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="7f152-252">a.</span><span class="sxs-lookup"><span data-stu-id="7f152-252">a.</span></span> <span data-ttu-id="7f152-253">Typ hello **UserID**, **zobrazované uživatelské jméno**, **heslo**, **Potvrdit heslo**a potom vyberte **skupiny**a hello **oddělení** platný Azure AD účet chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="7f152-253">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid Azure AD account you want tooprovision.</span></span>

    <span data-ttu-id="7f152-254">b.</span><span class="sxs-lookup"><span data-stu-id="7f152-254">b.</span></span> <span data-ttu-id="7f152-255">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7f152-255">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="7f152-256">Můžete použít jakékoli jiné Zscaler jeden uživatel účet vytvoření nástroje nebo rozhraní API poskytované Zscaler jeden tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f152-256">You can use any other Zscaler One user account creation tools or APIs provided by Zscaler One tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7f152-257">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7f152-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7f152-258">V této části povolíte tak, že udělíte přístup tooZscaler jeden Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f152-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler One.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7f152-260">**tooassign tooZscaler Britta Simon, jednu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f152-260">**tooassign Britta Simon tooZscaler One, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f152-261">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f152-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7f152-263">V seznamu aplikace hello vyberte **Zscaler jeden**.</span><span class="sxs-lookup"><span data-stu-id="7f152-263">In hello applications list, select **Zscaler One**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_app.png) 

3. <span data-ttu-id="7f152-265">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7f152-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7f152-267">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f152-267">Click **Add** button.</span></span> <span data-ttu-id="7f152-268">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f152-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7f152-270">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="7f152-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7f152-271">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f152-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7f152-272">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f152-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7f152-273">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f152-273">Testing single sign-on</span></span>

<span data-ttu-id="7f152-274">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7f152-274">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7f152-275">Po kliknutí na tlačítko hello Zscaler jeden dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Zscaler jednu aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7f152-275">When you click hello Zscaler One tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler One application.</span></span>
<span data-ttu-id="7f152-276">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7f152-276">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f152-277">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7f152-277">Additional resources</span></span>

* [<span data-ttu-id="7f152-278">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7f152-278">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7f152-279">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7f152-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_203.png

