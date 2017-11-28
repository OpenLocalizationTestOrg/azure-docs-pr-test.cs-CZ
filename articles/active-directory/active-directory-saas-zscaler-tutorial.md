---
title: 'Kurz: Azure Active Directory integrace s Zscaler | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Zscaler."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68c453f7-aff1-4614-92d3-9b86f3ad99dc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e2894534f5d6711fd6af618cd699fa5837b5bf26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler"></a><span data-ttu-id="b38f4-103">Kurz: Azure Active Directory integrace s Zscaler</span><span class="sxs-lookup"><span data-stu-id="b38f4-103">Tutorial: Azure Active Directory integration with Zscaler</span></span>

<span data-ttu-id="b38f4-104">V tomto kurzu zjistíte, jak toointegrate Zscaler s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b38f4-104">In this tutorial, you learn how toointegrate Zscaler with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b38f4-105">Integrace Zscaler s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b38f4-105">Integrating Zscaler with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b38f4-106">Můžete řídit ve službě Azure AD, který má přístup tooZscaler</span><span class="sxs-lookup"><span data-stu-id="b38f4-106">You can control in Azure AD who has access tooZscaler</span></span>
- <span data-ttu-id="b38f4-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooZscaler (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b38f4-107">You can enable your users tooautomatically get signed-on tooZscaler (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b38f4-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b38f4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b38f4-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b38f4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b38f4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b38f4-110">Prerequisites</span></span>

<span data-ttu-id="b38f4-111">Integrace služby Azure AD s Zscaler tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b38f4-111">tooconfigure Azure AD integration with Zscaler, you need hello following items:</span></span>

- <span data-ttu-id="b38f4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b38f4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b38f4-113">Zscaler jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b38f4-113">A Zscaler single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b38f4-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b38f4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b38f4-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b38f4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b38f4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b38f4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b38f4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b38f4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b38f4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b38f4-118">Scenario description</span></span>
<span data-ttu-id="b38f4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b38f4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b38f4-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b38f4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b38f4-121">Přidání Zscaler z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b38f4-121">Adding Zscaler from hello gallery</span></span>
2. <span data-ttu-id="b38f4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b38f4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-from-hello-gallery"></a><span data-ttu-id="b38f4-123">Přidání Zscaler z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b38f4-123">Adding Zscaler from hello gallery</span></span>
<span data-ttu-id="b38f4-124">tooconfigure hello integrace Zscaler do Azure AD, je nutné tooadd Zscaler hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b38f4-124">tooconfigure hello integration of Zscaler into Azure AD, you need tooadd Zscaler from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b38f4-125">**tooadd Zscaler z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b38f4-125">**tooadd Zscaler from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b38f4-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b38f4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b38f4-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b38f4-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b38f4-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b38f4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b38f4-133">Hello vyhledávacího pole zadejte **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-133">In hello search box, type **Zscaler**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_search.png)

5. <span data-ttu-id="b38f4-135">Na panelu výsledků hello vyberte **Zscaler**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b38f4-135">In hello results panel, select **Zscaler**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b38f4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b38f4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b38f4-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b38f4-138">In this section, you configure and test Azure AD single sign-on with Zscaler based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b38f4-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Zscaler je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b38f4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler is tooa user in Azure AD.</span></span> <span data-ttu-id="b38f4-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Zscaler musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b38f4-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler needs toobe established.</span></span>

<span data-ttu-id="b38f4-141">V Zscaler, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b38f4-141">In Zscaler, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b38f4-142">tooconfigure a testu Azure AD jednotné přihlašování s Zscaler, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b38f4-142">tooconfigure and test Azure AD single sign-on with Zscaler, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b38f4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b38f4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b38f4-144">**[Konfigurace nastavení proxy serveru](#configuring-proxy-settings)**  -tooconfigure hello nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="b38f4-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="b38f4-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b38f4-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="b38f4-146">**[Vytvoření zkušebního uživatele Zscaler](#creating-a-zscaler-test-user)**  -toohave protějšek Britta Simon v Zscaler, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b38f4-146">**[Creating a Zscaler test user](#creating-a-zscaler-test-user)** - toohave a counterpart of Britta Simon in Zscaler that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="b38f4-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b38f4-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="b38f4-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b38f4-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b38f4-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b38f4-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b38f4-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Zscaler.</span><span class="sxs-lookup"><span data-stu-id="b38f4-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler application.</span></span>

<span data-ttu-id="b38f4-151">**tooconfigure Azure AD jednotné přihlašování s Zscaler, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b38f4-151">**tooconfigure Azure AD single sign-on with Zscaler, perform hello following steps:**</span></span>

1. <span data-ttu-id="b38f4-152">V portálu Azure, na hello hello **Zscaler** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-152">In hello Azure portal, on hello **Zscaler** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b38f4-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b38f4-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_samlbase.png)

3. <span data-ttu-id="b38f4-156">Na hello **Zscaler domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b38f4-156">On hello **Zscaler Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_url.png)

    <span data-ttu-id="b38f4-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.zsclaer.net`</span><span class="sxs-lookup"><span data-stu-id="b38f4-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.zsclaer.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b38f4-159">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="b38f4-159">This value is not real.</span></span> <span data-ttu-id="b38f4-160">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b38f4-160">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b38f4-161">Obraťte se na [tým podpory Zscaler klienta](https://www.zscaler.com/company/contact) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b38f4-161">Contact [Zscaler Client support team](https://www.zscaler.com/company/contact) tooget this value.</span></span> 

4. <span data-ttu-id="b38f4-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b38f4-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_certificate.png) 

5. <span data-ttu-id="b38f4-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b38f4-164">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b38f4-166">Na hello **Zscaler konfigurace** klikněte na tlačítko **konfigurace Zscaler** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b38f4-166">On hello **Zscaler Configuration** section, click **Configure Zscaler** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b38f4-167">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b38f4-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_configure.png) 

7. <span data-ttu-id="b38f4-169">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti ZScaler tooyour.</span><span class="sxs-lookup"><span data-stu-id="b38f4-169">In a different web browser window, log in tooyour ZScaler company site as an administrator.</span></span>

8. <span data-ttu-id="b38f4-170">V nabídce hello hello nahoře, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-170">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="b38f4-171">![Správa](./media/active-directory-saas-zscaler-tutorial/ic800206.png "správy")</span><span class="sxs-lookup"><span data-stu-id="b38f4-171">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="b38f4-172">V části **spravovat správci & role**, klikněte na tlačítko **Správa uživatelů a ověřování**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-172">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="b38f4-173">![Správa uživatelů a ověřování](./media/active-directory-saas-zscaler-tutorial/ic800207.png "správu uživatelů a ověřování")</span><span class="sxs-lookup"><span data-stu-id="b38f4-173">![Manage Users & Authentication](./media/active-directory-saas-zscaler-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="b38f4-174">V hello **zvolte možnosti ověřování pro vaši organizaci** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b38f4-174">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="b38f4-175">![Ověřování](./media/active-directory-saas-zscaler-tutorial/ic800208.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="b38f4-175">![Authentication](./media/active-directory-saas-zscaler-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="b38f4-176">a.</span><span class="sxs-lookup"><span data-stu-id="b38f4-176">a.</span></span> <span data-ttu-id="b38f4-177">Vyberte **ověření pomocí SAML Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-177">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="b38f4-178">b.</span><span class="sxs-lookup"><span data-stu-id="b38f4-178">b.</span></span> <span data-ttu-id="b38f4-179">Klikněte na tlačítko **konfigurace SAML jeden přihlašování parametrů**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-179">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="b38f4-180">Na hello **konfigurace SAML jeden přihlašování parametry** dialogové okno stránky, proveďte následující kroky hello a pak klikněte na **provést**</span><span class="sxs-lookup"><span data-stu-id="b38f4-180">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="b38f4-181">![Jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/ic800209.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="b38f4-181">![Single Sign-On](./media/active-directory-saas-zscaler-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="b38f4-182">a.</span><span class="sxs-lookup"><span data-stu-id="b38f4-182">a.</span></span> <span data-ttu-id="b38f4-183">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **URL hello SAML portál toowhich uživatelů jsou odesílány pro ověřování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b38f4-183">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="b38f4-184">b.</span><span class="sxs-lookup"><span data-stu-id="b38f4-184">b.</span></span> <span data-ttu-id="b38f4-185">V hello **atribut obsahující přihlašovací jméno** textovému poli, typ **NameID**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-185">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="b38f4-186">c.</span><span class="sxs-lookup"><span data-stu-id="b38f4-186">c.</span></span> <span data-ttu-id="b38f4-187">tooupload stažený certifikát, klikněte na tlačítko **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-187">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="b38f4-188">d.</span><span class="sxs-lookup"><span data-stu-id="b38f4-188">d.</span></span> <span data-ttu-id="b38f4-189">Vyberte **zapnout automatické zřizování SAML**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-189">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="b38f4-190">Na hello **konfigurace ověřování uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b38f4-190">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="b38f4-191">![Správa](./media/active-directory-saas-zscaler-tutorial/ic800210.png "správy")</span><span class="sxs-lookup"><span data-stu-id="b38f4-191">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="b38f4-192">a.</span><span class="sxs-lookup"><span data-stu-id="b38f4-192">a.</span></span> <span data-ttu-id="b38f4-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-193">Click **Save**.</span></span>

    <span data-ttu-id="b38f4-194">b.</span><span class="sxs-lookup"><span data-stu-id="b38f4-194">b.</span></span> <span data-ttu-id="b38f4-195">Klikněte na tlačítko **aktivovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-195">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="b38f4-196">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="b38f4-196">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="b38f4-197">tooconfigure hello nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="b38f4-197">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="b38f4-198">Spustit **aplikace Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-198">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="b38f4-199">Vyberte **Možnosti Internetu** z hello **nástroje** nabídku pro otevřete hello **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b38f4-199">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="b38f4-200">![Možnosti Internetu](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Možnosti Internetu")</span><span class="sxs-lookup"><span data-stu-id="b38f4-200">![Internet Options](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="b38f4-201">Klikněte na tlačítko hello **připojení** kartě.</span><span class="sxs-lookup"><span data-stu-id="b38f4-201">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="b38f4-202">![Připojení](./media/active-directory-saas-zscaler-tutorial/ic769493.png "připojení")</span><span class="sxs-lookup"><span data-stu-id="b38f4-202">![Connections](./media/active-directory-saas-zscaler-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="b38f4-203">Klikněte na tlačítko **nastavení místní sítě** tooopen hello **nastavení místní sítě** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b38f4-203">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="b38f4-204">V části Proxy server hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b38f4-204">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="b38f4-205">![Proxy server](./media/active-directory-saas-zscaler-tutorial/ic769494.png "Proxy serveru")</span><span class="sxs-lookup"><span data-stu-id="b38f4-205">![Proxy server](./media/active-directory-saas-zscaler-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="b38f4-206">a.</span><span class="sxs-lookup"><span data-stu-id="b38f4-206">a.</span></span> <span data-ttu-id="b38f4-207">Vyberte **použít proxy server pro síť LAN**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-207">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="b38f4-208">b.</span><span class="sxs-lookup"><span data-stu-id="b38f4-208">b.</span></span> <span data-ttu-id="b38f4-209">V textovém poli hello adresu, zadejte **gateway.zscaler.net**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-209">In hello Address textbox, type **gateway.zscaler.net**.</span></span>

    <span data-ttu-id="b38f4-210">c.</span><span class="sxs-lookup"><span data-stu-id="b38f4-210">c.</span></span> <span data-ttu-id="b38f4-211">V textovém poli hello Port, zadejte **80**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-211">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="b38f4-212">d.</span><span class="sxs-lookup"><span data-stu-id="b38f4-212">d.</span></span> <span data-ttu-id="b38f4-213">Vyberte **Nepoužívat proxy server pro místní adresy**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-213">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="b38f4-214">e.</span><span class="sxs-lookup"><span data-stu-id="b38f4-214">e.</span></span> <span data-ttu-id="b38f4-215">Klikněte na tlačítko **OK** tooclose hello **nastavení místní sítě (LAN)** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b38f4-215">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="b38f4-216">Klikněte na tlačítko **OK** tooclose hello **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b38f4-216">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="b38f4-217">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b38f4-217">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b38f4-218">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b38f4-218">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b38f4-219">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b38f4-219">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b38f4-220">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b38f4-220">Creating an Azure AD test user</span></span>
<span data-ttu-id="b38f4-221">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b38f4-221">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b38f4-223">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b38f4-223">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b38f4-224">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b38f4-224">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b38f4-226">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-226">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b38f4-228">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b38f4-228">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b38f4-230">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b38f4-230">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b38f4-232">a.</span><span class="sxs-lookup"><span data-stu-id="b38f4-232">a.</span></span> <span data-ttu-id="b38f4-233">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-233">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b38f4-234">b.</span><span class="sxs-lookup"><span data-stu-id="b38f4-234">b.</span></span> <span data-ttu-id="b38f4-235">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b38f4-235">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b38f4-236">c.</span><span class="sxs-lookup"><span data-stu-id="b38f4-236">c.</span></span> <span data-ttu-id="b38f4-237">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-237">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b38f4-238">d.</span><span class="sxs-lookup"><span data-stu-id="b38f4-238">d.</span></span> <span data-ttu-id="b38f4-239">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-239">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-test-user"></a><span data-ttu-id="b38f4-240">Vytvoření zkušebního uživatele Zscaler</span><span class="sxs-lookup"><span data-stu-id="b38f4-240">Creating a Zscaler test user</span></span>

<span data-ttu-id="b38f4-241">Uživatelé toolog tooenable Azure AD v tooZScaler, musí být zřízená tooZScaler.</span><span class="sxs-lookup"><span data-stu-id="b38f4-241">tooenable Azure AD users toolog in tooZScaler, they must be provisioned tooZScaler.</span></span>  
<span data-ttu-id="b38f4-242">V případě hello ZScaler zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b38f4-242">In hello case of ZScaler, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="b38f4-243">tooconfigure zřizování uživatelů, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b38f4-243">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="b38f4-244">Přihlaste se tooyour **Zscaler** klienta.</span><span class="sxs-lookup"><span data-stu-id="b38f4-244">Log in tooyour **Zscaler** tenant.</span></span>

2. <span data-ttu-id="b38f4-245">Klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-245">Click **Administration**.</span></span>   
   
    <span data-ttu-id="b38f4-246">![Správa](./media/active-directory-saas-zscaler-tutorial/ic781035.png "správy")</span><span class="sxs-lookup"><span data-stu-id="b38f4-246">![Administration](./media/active-directory-saas-zscaler-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="b38f4-247">Klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-247">Click **User Management**.</span></span>   
        
     <span data-ttu-id="b38f4-248">![Přidat](./media/active-directory-saas-zscaler-tutorial/ic781036.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="b38f4-248">![Add](./media/active-directory-saas-zscaler-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="b38f4-249">V hello **uživatelé** , klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-249">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="b38f4-250">![Přidat](./media/active-directory-saas-zscaler-tutorial/ic781037.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="b38f4-250">![Add](./media/active-directory-saas-zscaler-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="b38f4-251">V části přidat uživatele hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b38f4-251">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="b38f4-252">![Přidat uživatele](./media/active-directory-saas-zscaler-tutorial/ic781038.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="b38f4-252">![Add User](./media/active-directory-saas-zscaler-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="b38f4-253">a.</span><span class="sxs-lookup"><span data-stu-id="b38f4-253">a.</span></span> <span data-ttu-id="b38f4-254">Typ hello **UserID**, **zobrazované uživatelské jméno**, **heslo**, **Potvrdit heslo**a potom vyberte **skupiny**a hello **oddělení** platného účtu AAD chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="b38f4-254">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid AAD account you want tooprovision.</span></span>

    <span data-ttu-id="b38f4-255">b.</span><span class="sxs-lookup"><span data-stu-id="b38f4-255">b.</span></span> <span data-ttu-id="b38f4-256">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-256">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="b38f4-257">Můžete použít všechny ostatní ZScaler uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ZScaler tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b38f4-257">You can use any other ZScaler user account creation tools or APIs provided by ZScaler tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b38f4-258">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b38f4-258">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b38f4-259">V této části povolíte tak, že udělíte přístup tooZscaler toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b38f4-259">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b38f4-261">**tooassign Britta Simon tooZscaler, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b38f4-261">**tooassign Britta Simon tooZscaler, perform hello following steps:**</span></span>

1. <span data-ttu-id="b38f4-262">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-262">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b38f4-264">V seznamu aplikace hello vyberte **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-264">In hello applications list, select **Zscaler**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_app.png) 

3. <span data-ttu-id="b38f4-266">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b38f4-266">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b38f4-268">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b38f4-268">Click **Add** button.</span></span> <span data-ttu-id="b38f4-269">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b38f4-269">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b38f4-271">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b38f4-271">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b38f4-272">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b38f4-272">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b38f4-273">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b38f4-273">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b38f4-274">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b38f4-274">Testing single sign-on</span></span>

<span data-ttu-id="b38f4-275">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b38f4-275">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b38f4-276">Když kliknete na dlaždici Zscaler hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Zscaler aplikace.</span><span class="sxs-lookup"><span data-stu-id="b38f4-276">When you click hello Zscaler tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler application.</span></span>
<span data-ttu-id="b38f4-277">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b38f4-277">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b38f4-278">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b38f4-278">Additional resources</span></span>

* [<span data-ttu-id="b38f4-279">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b38f4-279">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b38f4-280">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b38f4-280">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_203.png

