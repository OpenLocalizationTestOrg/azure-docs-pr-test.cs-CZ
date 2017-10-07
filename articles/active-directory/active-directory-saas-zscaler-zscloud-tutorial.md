---
title: 'Kurz: Azure Active Directory integrace s Zscaler ZSCloud | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Zscaler ZSCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: af6d5c1994e715cccf959cc9fd3ba998e5b9effa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a><span data-ttu-id="25b74-103">Kurz: Azure Active Directory integrace s Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="25b74-103">Tutorial: Azure Active Directory integration with Zscaler ZSCloud</span></span>

<span data-ttu-id="25b74-104">V tomto kurzu zjistíte, jak toointegrate Zscaler ZSCloud s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="25b74-104">In this tutorial, you learn how toointegrate Zscaler ZSCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="25b74-105">Integrace Zscaler ZSCloud s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="25b74-105">Integrating Zscaler ZSCloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="25b74-106">Můžete řídit ve službě Azure AD, který má přístup tooZscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="25b74-106">You can control in Azure AD who has access tooZscaler ZSCloud</span></span>
- <span data-ttu-id="25b74-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooZscaler ZSCloud (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="25b74-107">You can enable your users tooautomatically get signed-on tooZscaler ZSCloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="25b74-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="25b74-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="25b74-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="25b74-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25b74-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="25b74-110">Prerequisites</span></span>

<span data-ttu-id="25b74-111">Integrace služby Azure AD s Zscaler ZSCloud tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="25b74-111">tooconfigure Azure AD integration with Zscaler ZSCloud, you need hello following items:</span></span>

- <span data-ttu-id="25b74-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="25b74-112">An Azure AD subscription</span></span>
- <span data-ttu-id="25b74-113">Zscaler ZSCloud jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="25b74-113">A Zscaler ZSCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="25b74-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="25b74-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="25b74-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="25b74-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="25b74-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="25b74-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="25b74-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="25b74-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="25b74-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="25b74-118">Scenario description</span></span>
<span data-ttu-id="25b74-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="25b74-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="25b74-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="25b74-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="25b74-121">Přidání Zscaler ZSCloud z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="25b74-121">Adding Zscaler ZSCloud from hello gallery</span></span>
2. <span data-ttu-id="25b74-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="25b74-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-zscloud-from-hello-gallery"></a><span data-ttu-id="25b74-123">Přidání Zscaler ZSCloud z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="25b74-123">Adding Zscaler ZSCloud from hello gallery</span></span>
<span data-ttu-id="25b74-124">tooconfigure hello integrace Zscaler ZSCloud do Azure AD, je nutné tooadd Zscaler ZSCloud hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="25b74-124">tooconfigure hello integration of Zscaler ZSCloud into Azure AD, you need tooadd Zscaler ZSCloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="25b74-125">**tooadd Zscaler ZSCloud z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="25b74-125">**tooadd Zscaler ZSCloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="25b74-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="25b74-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="25b74-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="25b74-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="25b74-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="25b74-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="25b74-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="25b74-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="25b74-133">Hello vyhledávacího pole zadejte **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="25b74-133">In hello search box, type **Zscaler ZSCloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. <span data-ttu-id="25b74-135">Na panelu výsledků hello vyberte **Zscaler ZSCloud**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="25b74-135">In hello results panel, select **Zscaler ZSCloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="25b74-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="25b74-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="25b74-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s Zscaler ZSCloud podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="25b74-138">In this section, you configure and test Azure AD single sign-on with Zscaler ZSCloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="25b74-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Zscaler ZSCloud je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="25b74-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler ZSCloud is tooa user in Azure AD.</span></span> <span data-ttu-id="25b74-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Zscaler ZSCloud musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="25b74-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler ZSCloud needs toobe established.</span></span>

<span data-ttu-id="25b74-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="25b74-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Zscaler ZSCloud.</span></span>

<span data-ttu-id="25b74-142">tooconfigure a testu Azure AD jednotné přihlašování s Zscaler ZSCloud, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="25b74-142">tooconfigure and test Azure AD single sign-on with Zscaler ZSCloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="25b74-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="25b74-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="25b74-144">**[Konfigurace nastavení proxy serveru](#configuring-proxy-settings)**  -tooconfigure hello nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="25b74-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
2. <span data-ttu-id="25b74-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="25b74-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)**  - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="25b74-146">**[Vytvoření zkušebního uživatele Zscaler ZSCloud](#creating-a-zscaler-zscloud-test-user)**  -toohave protějšek Britta Simon v Zscaler ZSCloud, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="25b74-146">**[Creating a Zscaler ZSCloud test user](#creating-a-zscaler-zscloud-test-user)** - toohave a counterpart of Britta Simon in Zscaler ZSCloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="25b74-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="25b74-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="25b74-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="25b74-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="25b74-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="25b74-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="25b74-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="25b74-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler ZSCloud application.</span></span>

<span data-ttu-id="25b74-151">**tooconfigure Azure AD jednotné přihlašování s Zscaler ZSCloud, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="25b74-151">**tooconfigure Azure AD single sign-on with Zscaler ZSCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="25b74-152">V portálu Azure, na hello hello **Zscaler ZSCloud** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="25b74-152">In hello Azure portal, on hello **Zscaler ZSCloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="25b74-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="25b74-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. <span data-ttu-id="25b74-156">Na hello **Zscaler ZSCloud domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="25b74-156">On hello **Zscaler ZSCloud Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     <span data-ttu-id="25b74-158">V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše tooyour toosign na uživatele ZScaler ZSCloud aplikace.</span><span class="sxs-lookup"><span data-stu-id="25b74-158">In hello **Sign-on URL** textbox, type hello URL used by your users toosign-on tooyour ZScaler ZSCloud application.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="25b74-159">Máte tooupdate tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="25b74-159">You have tooupdate this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="25b74-160">Obraťte se na [tým podpory klienta ZSCloud Zscaler](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="25b74-160">Contact [Zscaler ZSCloud Client support team](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) tooget this value.</span></span> 
 
4. <span data-ttu-id="25b74-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="25b74-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. <span data-ttu-id="25b74-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="25b74-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="25b74-165">Na hello **Zscaler ZSCloud konfigurace** klikněte na tlačítko **konfigurace Zscaler ZSCloud** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="25b74-165">On hello **Zscaler ZSCloud Configuration** section, click **Configure Zscaler ZSCloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="25b74-166">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="25b74-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. <span data-ttu-id="25b74-168">V okně prohlížeče jiný web Přihlaste se tooyour ZScaler ZSCloud společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="25b74-168">In a different web browser window, log in tooyour ZScaler ZSCloud company site as an administrator.</span></span>

8. <span data-ttu-id="25b74-169">V nabídce hello hello nahoře, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="25b74-169">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="25b74-170">![Správa](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "správy")</span><span class="sxs-lookup"><span data-stu-id="25b74-170">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="25b74-171">V části **spravovat správci & role**, klikněte na tlačítko **Správa uživatelů a ověřování**.</span><span class="sxs-lookup"><span data-stu-id="25b74-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="25b74-172">![Správa uživatelů a ověřování](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "správu uživatelů a ověřování")</span><span class="sxs-lookup"><span data-stu-id="25b74-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="25b74-173">V hello **zvolte možnosti ověřování pro vaši organizaci** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="25b74-173">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="25b74-174">![Ověřování](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="25b74-174">![Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="25b74-175">a.</span><span class="sxs-lookup"><span data-stu-id="25b74-175">a.</span></span> <span data-ttu-id="25b74-176">Vyberte **ověření pomocí SAML Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="25b74-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="25b74-177">b.</span><span class="sxs-lookup"><span data-stu-id="25b74-177">b.</span></span> <span data-ttu-id="25b74-178">Klikněte na tlačítko **konfigurace SAML jeden přihlašování parametrů**.</span><span class="sxs-lookup"><span data-stu-id="25b74-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="25b74-179">Na hello **konfigurace SAML jeden přihlašování parametry** dialogové okno stránky, proveďte následující kroky hello a pak klikněte na **provést**</span><span class="sxs-lookup"><span data-stu-id="25b74-179">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="25b74-180">![Jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="25b74-180">![Single Sign-On](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="25b74-181">a.</span><span class="sxs-lookup"><span data-stu-id="25b74-181">a.</span></span> <span data-ttu-id="25b74-182">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu do hello **URL hello SAML portál toowhich uživatelů jsou odesílány pro ověřování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="25b74-182">Paste hello **SAML Single Sign-On Service URL** value into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="25b74-183">b.</span><span class="sxs-lookup"><span data-stu-id="25b74-183">b.</span></span> <span data-ttu-id="25b74-184">V hello **atribut obsahující přihlašovací jméno** textovému poli, typ **NameID**.</span><span class="sxs-lookup"><span data-stu-id="25b74-184">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="25b74-185">c.</span><span class="sxs-lookup"><span data-stu-id="25b74-185">c.</span></span> <span data-ttu-id="25b74-186">tooupload stažený certifikát, klikněte na tlačítko **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="25b74-186">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="25b74-187">d.</span><span class="sxs-lookup"><span data-stu-id="25b74-187">d.</span></span> <span data-ttu-id="25b74-188">Vyberte **zapnout automatické zřizování SAML**.</span><span class="sxs-lookup"><span data-stu-id="25b74-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="25b74-189">Na hello **konfigurace ověřování uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="25b74-189">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="25b74-190">![Správa](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "správy")</span><span class="sxs-lookup"><span data-stu-id="25b74-190">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="25b74-191">a.</span><span class="sxs-lookup"><span data-stu-id="25b74-191">a.</span></span> <span data-ttu-id="25b74-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="25b74-192">Click **Save**.</span></span>

    <span data-ttu-id="25b74-193">b.</span><span class="sxs-lookup"><span data-stu-id="25b74-193">b.</span></span> <span data-ttu-id="25b74-194">Klikněte na tlačítko **aktivovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="25b74-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="25b74-195">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="25b74-195">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="25b74-196">tooconfigure hello nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="25b74-196">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="25b74-197">Spustit **aplikace Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="25b74-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="25b74-198">Vyberte **Možnosti Internetu** z hello **nástroje** nabídku pro otevřete hello **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="25b74-198">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="25b74-199">![Možnosti Internetu](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Možnosti Internetu")</span><span class="sxs-lookup"><span data-stu-id="25b74-199">![Internet Options](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="25b74-200">Klikněte na tlačítko hello **připojení** kartě.</span><span class="sxs-lookup"><span data-stu-id="25b74-200">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="25b74-201">![Připojení](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "připojení")</span><span class="sxs-lookup"><span data-stu-id="25b74-201">![Connections](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="25b74-202">Klikněte na tlačítko **nastavení místní sítě** tooopen hello **nastavení místní sítě** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="25b74-202">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="25b74-203">V části Proxy server hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="25b74-203">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="25b74-204">![Proxy server](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "Proxy serveru")</span><span class="sxs-lookup"><span data-stu-id="25b74-204">![Proxy server](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="25b74-205">a.</span><span class="sxs-lookup"><span data-stu-id="25b74-205">a.</span></span> <span data-ttu-id="25b74-206">Vyberte **použít proxy server pro síť LAN**.</span><span class="sxs-lookup"><span data-stu-id="25b74-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="25b74-207">b.</span><span class="sxs-lookup"><span data-stu-id="25b74-207">b.</span></span> <span data-ttu-id="25b74-208">V textovém poli hello adresu, zadejte **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="25b74-208">In hello Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="25b74-209">c.</span><span class="sxs-lookup"><span data-stu-id="25b74-209">c.</span></span> <span data-ttu-id="25b74-210">V textovém poli hello Port, zadejte **80**.</span><span class="sxs-lookup"><span data-stu-id="25b74-210">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="25b74-211">d.</span><span class="sxs-lookup"><span data-stu-id="25b74-211">d.</span></span> <span data-ttu-id="25b74-212">Vyberte **Nepoužívat proxy server pro místní adresy**.</span><span class="sxs-lookup"><span data-stu-id="25b74-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="25b74-213">e.</span><span class="sxs-lookup"><span data-stu-id="25b74-213">e.</span></span> <span data-ttu-id="25b74-214">Klikněte na tlačítko **OK** tooclose hello **nastavení místní sítě (LAN)** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="25b74-214">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="25b74-215">Klikněte na tlačítko **OK** tooclose hello **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="25b74-215">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="25b74-216">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="25b74-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="25b74-217">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="25b74-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="25b74-219">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="25b74-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="25b74-220">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="25b74-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="25b74-222">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="25b74-222">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="25b74-224">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="25b74-224">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="25b74-226">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="25b74-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="25b74-228">a.</span><span class="sxs-lookup"><span data-stu-id="25b74-228">a.</span></span> <span data-ttu-id="25b74-229">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="25b74-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="25b74-230">b.</span><span class="sxs-lookup"><span data-stu-id="25b74-230">b.</span></span> <span data-ttu-id="25b74-231">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="25b74-231">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="25b74-232">c.</span><span class="sxs-lookup"><span data-stu-id="25b74-232">c.</span></span> <span data-ttu-id="25b74-233">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="25b74-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="25b74-234">d.</span><span class="sxs-lookup"><span data-stu-id="25b74-234">d.</span></span> <span data-ttu-id="25b74-235">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="25b74-235">Click **Create**.</span></span>

### <a name="creating-a-zscaler-zscloud-test-user"></a><span data-ttu-id="25b74-236">Vytvoření zkušebního uživatele Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="25b74-236">Creating a Zscaler ZSCloud test user</span></span>

<span data-ttu-id="25b74-237">Uživatelé toolog tooenable Azure AD v tooZScaler ZSCloud, musí být zřízená tooZScaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="25b74-237">tooenable Azure AD users toolog in tooZScaler ZSCloud, they must be provisioned tooZScaler ZSCloud.</span></span>  
<span data-ttu-id="25b74-238">V případě hello ZScaler ZSCloud zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="25b74-238">In hello case of ZScaler ZSCloud, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="25b74-239">tooconfigure zřizování uživatelů, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="25b74-239">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="25b74-240">Přihlaste se tooyour **Zscaler** klienta.</span><span class="sxs-lookup"><span data-stu-id="25b74-240">Log in tooyour **Zscaler** tenant.</span></span>

2. <span data-ttu-id="25b74-241">Klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="25b74-241">Click **Administration**.</span></span>   
   
    <span data-ttu-id="25b74-242">![Správa](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "správy")</span><span class="sxs-lookup"><span data-stu-id="25b74-242">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="25b74-243">Klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="25b74-243">Click **User Management**.</span></span>   
        
     <span data-ttu-id="25b74-244">![Přidat](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="25b74-244">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

4. <span data-ttu-id="25b74-245">V hello **uživatelé** , klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="25b74-245">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="25b74-246">![Přidat](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="25b74-246">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="25b74-247">V části přidat uživatele hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="25b74-247">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="25b74-248">![Přidat uživatele](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="25b74-248">![Add User](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="25b74-249">a.</span><span class="sxs-lookup"><span data-stu-id="25b74-249">a.</span></span> <span data-ttu-id="25b74-250">Typ hello **UserID**, **zobrazované uživatelské jméno**, **heslo**, **Potvrdit heslo**a potom vyberte **skupiny**a hello **oddělení** platného účtu AAD chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="25b74-250">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid AAD account you want tooprovision.</span></span>

    <span data-ttu-id="25b74-251">b.</span><span class="sxs-lookup"><span data-stu-id="25b74-251">b.</span></span> <span data-ttu-id="25b74-252">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="25b74-252">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="25b74-253">Můžete použít všechny ostatní ZScaler ZSCloud uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ZScaler ZSCloud tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="25b74-253">You can use any other ZScaler ZSCloud user account creation tools or APIs provided by ZScaler ZSCloud tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="25b74-254">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="25b74-254">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="25b74-255">V této části povolíte tak, že udělíte přístup tooZscaler ZSCloud toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="25b74-255">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler ZSCloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="25b74-257">**tooassign Britta Simon tooZscaler ZSCloud, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="25b74-257">**tooassign Britta Simon tooZscaler ZSCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="25b74-258">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="25b74-258">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="25b74-260">V seznamu aplikace hello vyberte **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="25b74-260">In hello applications list, select **Zscaler ZSCloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. <span data-ttu-id="25b74-262">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="25b74-262">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="25b74-264">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="25b74-264">Click **Add** button.</span></span> <span data-ttu-id="25b74-265">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="25b74-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="25b74-267">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="25b74-267">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="25b74-268">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="25b74-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="25b74-269">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="25b74-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="25b74-270">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="25b74-270">Testing single sign-on</span></span>

<span data-ttu-id="25b74-271">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="25b74-271">If you want tootest your single sign-on settings, open hello Access Panel.</span></span>

<span data-ttu-id="25b74-272">Po kliknutí na tlačítko hello Zscaler ZSCloud dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Zscaler ZSCloud aplikace.</span><span class="sxs-lookup"><span data-stu-id="25b74-272">When you click hello Zscaler ZSCloud tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler ZSCloud application.</span></span>

<span data-ttu-id="25b74-273">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="25b74-273">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="25b74-274">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="25b74-274">Additional resources</span></span>

* [<span data-ttu-id="25b74-275">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="25b74-275">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="25b74-276">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="25b74-276">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_203.png

