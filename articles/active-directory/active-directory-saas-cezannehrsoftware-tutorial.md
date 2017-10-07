---
title: 'Kurz: Azure Active Directory integrace s Cezanne HR softwarem | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Cezanne HR softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 3675acd8871d62c2277def8074f7aa39ac46e2a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a><span data-ttu-id="07c08-103">Kurz: Integrate Azure Active Directory s Cezanne HR softwaru</span><span class="sxs-lookup"><span data-stu-id="07c08-103">Tutorial: Integrate Azure Active Directory with Cezanne HR software</span></span>

<span data-ttu-id="07c08-104">V tomto kurzu zjistíte, jak toointegrate Cezanne HR software s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="07c08-104">In this tutorial, you learn how toointegrate Cezanne HR software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="07c08-105">Integrace Cezanne HR softwaru s Azure AD poskytuje následující výhody hello.</span><span class="sxs-lookup"><span data-stu-id="07c08-105">Integrating Cezanne HR software with Azure AD provides you with hello following benefits.</span></span> <span data-ttu-id="07c08-106">Můžete:</span><span class="sxs-lookup"><span data-stu-id="07c08-106">You can:</span></span>

- <span data-ttu-id="07c08-107">Řízení ve službě Azure AD, který má přístup tooCezanne HR softwaru.</span><span class="sxs-lookup"><span data-stu-id="07c08-107">Control in Azure AD who has access tooCezanne HR software.</span></span>
- <span data-ttu-id="07c08-108">Povolte přihlášení uživatelů tooautomatically tooCezanne HR software s jednotné přihlašování (SSO) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07c08-108">Enable your users tooautomatically sign in tooCezanne HR software with single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="07c08-109">Spravovat účty v jednom centrálním místě: hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="07c08-109">Manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="07c08-110">toolearn Další informace o softwaru, služba (SaaS) aplikace integraci s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="07c08-110">toolearn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and SSO with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07c08-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07c08-111">Prerequisites</span></span>

<span data-ttu-id="07c08-112">tooconfigure integrace služby Azure AD s Cezanne HR softwaru, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="07c08-112">tooconfigure Azure AD integration with Cezanne HR software, you need hello following items:</span></span>

- <span data-ttu-id="07c08-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="07c08-113">An Azure AD subscription</span></span>
- <span data-ttu-id="07c08-114">Software Cezanne HR předplatné povolené jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="07c08-114">A Cezanne HR software SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="07c08-115">Doporučujeme tootest hello kroky v tomto kurzu, nepoužívejte provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="07c08-115">tootest hello steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="07c08-116">tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="07c08-116">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="07c08-117">Nepoužívejte produkční prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="07c08-117">Don't use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="07c08-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="07c08-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="07c08-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="07c08-119">Scenario description</span></span>
<span data-ttu-id="07c08-120">V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="07c08-120">In this tutorial, you test Azure AD SSO in a test environment.</span></span> 

<span data-ttu-id="07c08-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="07c08-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="07c08-122">Přidání softwaru Cezanne HR z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="07c08-122">Adding Cezanne HR software from hello gallery</span></span>
* <span data-ttu-id="07c08-123">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="07c08-123">Configuring and testing Azure AD SSO</span></span>

## <a name="add-cezanne-hr-software-from-hello-gallery"></a><span data-ttu-id="07c08-124">Přidat software Cezanne HR z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="07c08-124">Add Cezanne HR software from hello gallery</span></span>
<span data-ttu-id="07c08-125">integrace hello tooconfigure Cezanne HR softwaru do služby Azure AD, přidejte Cezanne HR softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="07c08-125">tooconfigure hello integration of Cezanne HR software into Azure AD, add Cezanne HR software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="07c08-126">tooadd Cezanne HR softwaru z Galerie hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="07c08-126">tooadd Cezanne HR software from hello gallery, do hello following:</span></span>

1. <span data-ttu-id="07c08-127">V hello  **[portál Azure](https://portal.azure.com)**, v levém podokně text hello, vyberte hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="07c08-127">In hello **[Azure portal](https://portal.azure.com)**, in hello left pane, select hello **Azure Active Directory** button.</span></span> 

    ![tlačítko "Azure Active Directory" Hello][1]

2. <span data-ttu-id="07c08-129">Vyberte **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="07c08-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![Hello "Všechny aplikace" odkaz][2]
    
3. <span data-ttu-id="07c08-131">tooadd novou aplikaci, hello horní části hello **všechny aplikace** dialogové okno, vyberte **novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="07c08-131">tooadd a new application, at hello top of hello **All applications** dialog box, select **New application**.</span></span>

    ![Hello "Nové aplikace" tlačítko][3]

4. <span data-ttu-id="07c08-133">Hello vyhledávacího pole zadejte **Cezanne HR softwaru**.</span><span class="sxs-lookup"><span data-stu-id="07c08-133">In hello search box, type **Cezanne HR Software**.</span></span>

    ![Hello vyhledávacího pole](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. <span data-ttu-id="07c08-135">V seznamu výsledků hello vyberte **Cezanne HR softwaru** a pak vyberte hello **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="07c08-135">In hello results list, select **Cezanne HR Software** and then select hello **Add** button tooadd hello application.</span></span>

    ![seznam výsledků Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="07c08-137">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="07c08-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="07c08-138">V této části můžete nakonfigurovat a otestovat jednotného přihlašování k AD Azure s Cezanne HR software založený na testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="07c08-138">In this section, you configure and test Azure AD SSO with Cezanne HR software based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="07c08-139">Azure AD pro jednotné přihlašování toowork musí tooknow hello Cezanne HR softwaru protějšku toohello uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07c08-139">For SSO toowork, Azure AD needs tooknow hello Cezanne HR software counterpart toohello Azure AD user.</span></span> <span data-ttu-id="07c08-140">Jinými slovy je potřeba vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské hello v hello Cezanne HR softwaru.</span><span class="sxs-lookup"><span data-stu-id="07c08-140">In other words, you must establish a link relationship between an Azure AD user and hello related user in hello Cezanne HR software.</span></span>

<span data-ttu-id="07c08-141">tooestablish hello odkaz vztahu hello přiřazení softwaru Cezanne HR **uživatelské jméno** hodnotu jako hello Azure AD **uživatelské jméno** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="07c08-141">tooestablish hello link relationship, assign hello Cezanne HR software **user name** value as hello Azure AD **Username** value.</span></span>

<span data-ttu-id="07c08-142">tooconfigure a testování Azure AD jednotného přihlašování pomocí softwaru Cezanne HR dokončení hello následující stavební bloky.</span><span class="sxs-lookup"><span data-stu-id="07c08-142">tooconfigure and test Azure AD SSO by using Cezanne HR software, complete hello following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="07c08-143">Konfigurovat Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="07c08-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="07c08-144">V této části můžete povolení jednotného přihlašování Azure AD v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Cezanne HR softwaru pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="07c08-144">In this section, you can enable Azure AD SSO in hello Azure portal and configure SSO in your Cezanne HR software application by doing hello following:</span></span>

1. <span data-ttu-id="07c08-145">V portálu Azure, na hello hello **Cezanne HR softwaru** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="07c08-145">In hello Azure portal, on hello **Cezanne HR Software** application integration page, select **Single sign-on**.</span></span>

    ![příkaz "Jednotného přihlašování" Hello][4]

2. <span data-ttu-id="07c08-147">tooenable jednotné přihlašování, v hello **jednotného přihlašování** dialogové okno, vyberte hello **režimu** jako **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="07c08-147">tooenable SSO, in hello **Single sign-on** dialog box, select hello **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![pole "Režim" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. <span data-ttu-id="07c08-149">V části **Cezanne HR softwaru domény a adresy URL**, hello následující:</span><span class="sxs-lookup"><span data-stu-id="07c08-149">Under **Cezanne HR Software Domain and URLs**, do hello following:</span></span>

    ![část "Cezanne HR softwaru domény adresy URL a" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    <span data-ttu-id="07c08-151">a.</span><span class="sxs-lookup"><span data-stu-id="07c08-151">a.</span></span> <span data-ttu-id="07c08-152">V hello **přihlašovací adresa URL** pole, zadejte adresu URL, která má hello následující syntaxi:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="07c08-152">In hello **Sign-on URL** box, type a URL that has hello following syntax: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span></span>

    <span data-ttu-id="07c08-153">b.</span><span class="sxs-lookup"><span data-stu-id="07c08-153">b.</span></span> <span data-ttu-id="07c08-154">V hello **adresa URL odpovědi** pole, zadejte adresu URL, která má hello následující syntaxi:`https://w3.cezanneondemand.com:443/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="07c08-154">In hello **Reply URL** box, type a URL that has hello following syntax: `https://w3.cezanneondemand.com:443/<tenantid>`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="07c08-155">Hello předchozí hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="07c08-155">hello preceding values are not real.</span></span> <span data-ttu-id="07c08-156">Adresa URL hello skutečné odpovědi a hello přihlašovací adresa URL je aktualizujte.</span><span class="sxs-lookup"><span data-stu-id="07c08-156">Update them with hello actual reply URL and hello sign-on URL.</span></span> <span data-ttu-id="07c08-157">tooobtain hello hodnoty, kontaktujte hello [tým podpory klientský software Cezanne HR](mailto:info@cezannehr.com).</span><span class="sxs-lookup"><span data-stu-id="07c08-157">tooobtain hello values, contact hello [Cezanne HR software client support team](mailto:info@cezannehr.com).</span></span>

4. <span data-ttu-id="07c08-158">V části **SAML podpisový certifikát**, vyberte **certifikátu (Base64)**a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="07c08-158">Under **SAML Signing Certificate**, select **Certificate (Base64)**, and then save hello certificate file on your computer.</span></span>

    ![Hello část "SAML podpisový certifikát"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. <span data-ttu-id="07c08-160">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="07c08-160">Select **Save**.</span></span>

    ![tlačítko "Uložit" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="07c08-162">V části **Cezanne HR softwarové konfigurace**, vyberte **konfigurace softwaru HR Cezanne** tooopen hello **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="07c08-162">Under **Cezanne HR Software Configuration**, select **Configure Cezanne HR Software** tooopen hello **Configure sign-on** window.</span></span> <span data-ttu-id="07c08-163">Kopírování hello **SAML Entity ID** a **SAML-služby přihlášení** adresa URL z hello **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="07c08-163">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service** URL from hello **Quick Reference** section.</span></span>

    ![Hello "Cezanne HR softwaru konfigurace"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. <span data-ttu-id="07c08-165">V okně prohlížeče jiných webových přihlaste tooyour Cezanne HR softwaru klienta jako správce.</span><span class="sxs-lookup"><span data-stu-id="07c08-165">In a different web browser window, sign on tooyour Cezanne HR software tenant as an administrator.</span></span>

8. <span data-ttu-id="07c08-166">V levém podokně hello vyberte **nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="07c08-166">In hello left pane, select **System Setup**.</span></span> <span data-ttu-id="07c08-167">Vyberte **nastavení zabezpečení** > **jednotné přihlašování konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="07c08-167">Select **Security Settings** > **Single Sign-On Configuration**.</span></span>

    ![odkaz "Jednoho přihlášení konfigurace" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. <span data-ttu-id="07c08-169">V hello **povolit uživatelům toolog pomocí hello následující služby Jednotné přihlašování (SSO)** podokně, vyberte hello **SAML 2.0** zaškrtávací políčko a vyberte hello **Upřesnit konfiguraci** možnost.</span><span class="sxs-lookup"><span data-stu-id="07c08-169">In hello **Allow users toolog in using hello following Single Sign-On (SSO) services** pane, select hello **SAML 2.0** check box and select hello **Advanced Configuration** option.</span></span>

    ![Jednotné přihlašování možnosti služeb](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. <span data-ttu-id="07c08-171">Vyberte **přidat nové**.</span><span class="sxs-lookup"><span data-stu-id="07c08-171">Select **Add New**.</span></span>

    ![tlačítko "Přidat nové" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. <span data-ttu-id="07c08-173">V části **zprostředkovatelů Identity SAML 2.0**, hello následující:</span><span class="sxs-lookup"><span data-stu-id="07c08-173">Under **SAML 2.0 Identity Providers**, do hello following:</span></span>

    ![část "Zprostředkovatelů Identity SAML 2.0" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    <span data-ttu-id="07c08-175">a.</span><span class="sxs-lookup"><span data-stu-id="07c08-175">a.</span></span> <span data-ttu-id="07c08-176">V hello **zobrazovaný název** zadejte hello název zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="07c08-176">In hello **Display Name** box, enter hello name of your identity provider.</span></span>

    <span data-ttu-id="07c08-177">b.</span><span class="sxs-lookup"><span data-stu-id="07c08-177">b.</span></span> <span data-ttu-id="07c08-178">V hello **identifikátor Entity** pole, vložte hello **SAML Entity ID** který jste zkopírovali ze hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="07c08-178">In hello **Entity Identifier** box, paste hello **SAML Entity ID** that you copied from hello Azure portal.</span></span> 

    <span data-ttu-id="07c08-179">c.</span><span class="sxs-lookup"><span data-stu-id="07c08-179">c.</span></span> <span data-ttu-id="07c08-180">V hello **SAML vazby** pole se seznamem, vyberte **POST**.</span><span class="sxs-lookup"><span data-stu-id="07c08-180">In hello **SAML Binding** list box, select **POST**.</span></span>

    <span data-ttu-id="07c08-181">d.</span><span class="sxs-lookup"><span data-stu-id="07c08-181">d.</span></span> <span data-ttu-id="07c08-182">V hello **koncový bod služby tokenu zabezpečení** pole, vložte hello **SAML-služby přihlášení** adresu URL, kterou jste zkopírovali z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="07c08-182">In hello **Security Token Service Endpoint** box, paste hello **SAML Single Sign-On Service** URL that you copied from hello Azure portal.</span></span> 
    
    <span data-ttu-id="07c08-183">e.</span><span class="sxs-lookup"><span data-stu-id="07c08-183">e.</span></span> <span data-ttu-id="07c08-184">V hello **název atributu ID uživatele** zadejte `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="07c08-184">In hello **User ID Attribute Name** box, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="07c08-185">f.</span><span class="sxs-lookup"><span data-stu-id="07c08-185">f.</span></span> <span data-ttu-id="07c08-186">tooupload hello stáhnout certifikát z Azure AD, vyberte hello **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="07c08-186">tooupload hello downloaded certificate from Azure AD, select hello **Upload** button.</span></span>
    
    <span data-ttu-id="07c08-187">g.</span><span class="sxs-lookup"><span data-stu-id="07c08-187">g.</span></span> <span data-ttu-id="07c08-188">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="07c08-188">Select **OK**.</span></span> 

12. <span data-ttu-id="07c08-189">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="07c08-189">Select **Save**.</span></span>

    ![tlačítko "Uložit" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> <span data-ttu-id="07c08-191">Při nastavování aplikace hello, si můžete přečíst stručným verzi hello předchozích pokynů v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="07c08-191">As you set up hello app, you can read a concise version of hello preceding instructions in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="07c08-192">Po přidání aplikace hello z hello **služby Active Directory** > **podnikové aplikace, které** části, vyberte hello **jednotného přihlašování** kartě. Potom přístup hello vložených dokumentace z hello **konfigurace** části.</span><span class="sxs-lookup"><span data-stu-id="07c08-192">After you add hello app from hello **Active Directory** > **Enterprise applications** section, select hello **Single sign-on** tab. Then access hello embedded documentation from hello **Configuration** section.</span></span> 

<span data-ttu-id="07c08-193">toolearn Další informace o funkci embedded dokumentace hello, najdete v části [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="07c08-193">toolearn more about hello embedded documentation feature, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="07c08-194">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="07c08-194">Create an Azure AD test user</span></span>
<span data-ttu-id="07c08-195">V této části vytvoříte testovacího uživatele Britta Simon v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="07c08-195">In this section, you create test user Britta Simon in hello Azure portal.</span></span>

![Hello testovacího uživatele Britta Simon][100]

<span data-ttu-id="07c08-197">toocreate testovacího uživatele ve službě Azure AD, hello následující:</span><span class="sxs-lookup"><span data-stu-id="07c08-197">toocreate a test user in Azure AD, do hello following:</span></span>

1. <span data-ttu-id="07c08-198">V hello **portál Azure**, v levém podokně text hello, vyberte hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="07c08-198">In hello **Azure portal**, in hello left pane, select hello **Azure Active Directory** button.</span></span>

    ![tlačítko "Azure Active Directory" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="07c08-200">toodisplay hello seznam uživatelů, vyberte **uživatelů a skupin** > **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="07c08-200">toodisplay hello list of users, select **Users and groups** > **All users**.</span></span>
    
    ![Hello "Všichni uživatelé" odkaz](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    <span data-ttu-id="07c08-202">Hello **všichni uživatelé** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="07c08-202">hello **All users** dialog box opens.</span></span>

3. <span data-ttu-id="07c08-203">tooopen hello **uživatele** dialogové okno, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="07c08-203">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![tlačítko "Přidat" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="07c08-205">V hello **uživatele** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="07c08-205">In hello **User** dialog box, do hello following:</span></span>
 
    ![Dialogové okno "User" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="07c08-207">a.</span><span class="sxs-lookup"><span data-stu-id="07c08-207">a.</span></span> <span data-ttu-id="07c08-208">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="07c08-208">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="07c08-209">b.</span><span class="sxs-lookup"><span data-stu-id="07c08-209">b.</span></span> <span data-ttu-id="07c08-210">V hello **uživatelské jméno** pole, zadejte uživatele Britta Simon **e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="07c08-210">In hello **User name** box, type user Britta Simon's **email address**.</span></span>

    <span data-ttu-id="07c08-211">c.</span><span class="sxs-lookup"><span data-stu-id="07c08-211">c.</span></span> <span data-ttu-id="07c08-212">Vyberte hello **zobrazit hesla** zaškrtávací políčko a potom Poznámka hello hodnotu, která byla vygenerována v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="07c08-212">Select hello **Show Password** check box, and then note hello value that was generated in hello **Password** box.</span></span>

    <span data-ttu-id="07c08-213">d.</span><span class="sxs-lookup"><span data-stu-id="07c08-213">d.</span></span> <span data-ttu-id="07c08-214">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="07c08-214">Select **Create**.</span></span>
 
### <a name="create-a-cezanne-hr-software-test-user"></a><span data-ttu-id="07c08-215">Vytvoření zkušebního uživatele Cezanne HR softwaru</span><span class="sxs-lookup"><span data-stu-id="07c08-215">Create a Cezanne HR software test user</span></span>

<span data-ttu-id="07c08-216">Uživatelé toosign tooenable Azure AD v softwaru tooCezanne oddělení lidských zdrojů, se musí být zřízená do Cezanne HR softwaru.</span><span class="sxs-lookup"><span data-stu-id="07c08-216">tooenable Azure AD users toosign in tooCezanne HR software, they must be provisioned into Cezanne HR software.</span></span> <span data-ttu-id="07c08-217">V případě hello Cezanne HR softwaru zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="07c08-217">In hello case of Cezanne HR software, provisioning is a manual task.</span></span>

<span data-ttu-id="07c08-218">Poskytnutí uživatelského účtu pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="07c08-218">Provision a user account by doing hello following:</span></span>

1.  <span data-ttu-id="07c08-219">Přihlaste se tooyour Cezanne HR softwaru společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="07c08-219">Sign in tooyour Cezanne HR software company site as an administrator.</span></span>

2.  <span data-ttu-id="07c08-220">V levém podokně hello vyberte **nastavení systému** > **spravovat uživatele** > **přidat nové uživatele**.</span><span class="sxs-lookup"><span data-stu-id="07c08-220">In hello left pane, select **System Setup** > **Manage Users** > **Add New User**.</span></span>

    <span data-ttu-id="07c08-221">![odkaz "Přidat nový uživatel" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="07c08-221">![hello "Add New User" link](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "New User")</span></span>

3.  <span data-ttu-id="07c08-222">V části **osoba podrobnosti**, hello následující:</span><span class="sxs-lookup"><span data-stu-id="07c08-222">Under **Person Details**, do hello following:</span></span>

    <span data-ttu-id="07c08-223">![Hello části osoba detaily.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="07c08-223">![hello "Person Details" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "New User")</span></span>
    
    <span data-ttu-id="07c08-224">a.</span><span class="sxs-lookup"><span data-stu-id="07c08-224">a.</span></span> <span data-ttu-id="07c08-225">Nastavit **interní uživatele** jako **OFF**.</span><span class="sxs-lookup"><span data-stu-id="07c08-225">Set **Internal User** as **OFF**.</span></span>
    
    <span data-ttu-id="07c08-226">b.</span><span class="sxs-lookup"><span data-stu-id="07c08-226">b.</span></span> <span data-ttu-id="07c08-227">V hello **křestní jméno** pole, typ hello křestní jméno uživatele, například **Britta**.</span><span class="sxs-lookup"><span data-stu-id="07c08-227">In hello **First Name** box, type hello user's first name, for example, **Britta**.</span></span>  
 
    <span data-ttu-id="07c08-228">c.</span><span class="sxs-lookup"><span data-stu-id="07c08-228">c.</span></span> <span data-ttu-id="07c08-229">V hello **příjmení** pole, typ hello příjmení uživatele, například **Simon**.</span><span class="sxs-lookup"><span data-stu-id="07c08-229">In hello **Last Name** box, type hello user's last name, for example, **Simon**.</span></span>
    
    <span data-ttu-id="07c08-230">d.</span><span class="sxs-lookup"><span data-stu-id="07c08-230">d.</span></span> <span data-ttu-id="07c08-231">V hello **e-mailu** hello uživatele e-mailovou adresu, zadejte například Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="07c08-231">In hello **E-mail** box, type hello user's email address, for example, Brittasimon@contoso.com.</span></span>

4.  <span data-ttu-id="07c08-232">V části **informace o účtu**, hello následující:</span><span class="sxs-lookup"><span data-stu-id="07c08-232">Under **Account Information**, do hello following:</span></span>

    <span data-ttu-id="07c08-233">![Hello část "Informace o účtu"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="07c08-233">![hello "Account Information" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "New User")</span></span>
    
    <span data-ttu-id="07c08-234">a.</span><span class="sxs-lookup"><span data-stu-id="07c08-234">a.</span></span> <span data-ttu-id="07c08-235">V hello **uživatelské jméno** hello uživatele e-mailovou adresu, zadejte například Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="07c08-235">In hello **Username** box, type hello user's email address, for example, Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="07c08-236">b.</span><span class="sxs-lookup"><span data-stu-id="07c08-236">b.</span></span> <span data-ttu-id="07c08-237">V hello **heslo** zadejte heslo uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="07c08-237">In hello **Password** box, type hello user's password.</span></span>
    
    <span data-ttu-id="07c08-238">c.</span><span class="sxs-lookup"><span data-stu-id="07c08-238">c.</span></span> <span data-ttu-id="07c08-239">V hello **Role zabezpečení** vyberte **HR Professional**.</span><span class="sxs-lookup"><span data-stu-id="07c08-239">In hello **Security Role** box, select **HR Professional**.</span></span>
    
    <span data-ttu-id="07c08-240">d.</span><span class="sxs-lookup"><span data-stu-id="07c08-240">d.</span></span> <span data-ttu-id="07c08-241">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="07c08-241">Select **OK**.</span></span>

5. <span data-ttu-id="07c08-242">Na hello **jednotného přihlašování** na kartě hello **SAML 2.0 identifikátory** vyberte **přidat nové**.</span><span class="sxs-lookup"><span data-stu-id="07c08-242">On hello **Single sign-on** tab, in hello **SAML 2.0 Identifiers** section, select **Add New**.</span></span>

    <span data-ttu-id="07c08-243">![tlačítko "Přidat nové" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="07c08-243">![hello "Add New" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "User")</span></span>

6. <span data-ttu-id="07c08-244">V hello **zprostředkovatele Identity** vyberte zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="07c08-244">In hello **Identity Provider** list box, select your identity provider.</span></span> <span data-ttu-id="07c08-245">V hello **uživatelský identifikátor** zadejte hello e-mailovou adresu pro testovací uživatele Britta Simon na účet.</span><span class="sxs-lookup"><span data-stu-id="07c08-245">In hello **User Identifier** box, enter hello email address for test user Britta Simon's account.</span></span>

    <span data-ttu-id="07c08-246">![Hello polí "Zprostředkovatele Identity" a "Uživatelský identifikátor"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="07c08-246">![hello "Identity Provider" and "User Identifier" boxes](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "User")</span></span>
    
7. <span data-ttu-id="07c08-247">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="07c08-247">Select **Save**.</span></span>

    <span data-ttu-id="07c08-248">![tlačítko "Uložit" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="07c08-248">![hello "Save" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "User")</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="07c08-249">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="07c08-249">Assign hello Azure AD test user</span></span>

<span data-ttu-id="07c08-250">V této části povolíte testovacího uživatele Britta Simon toouse Azure jednotného přihlašování k udělení přístupu tooCezanne HR softwaru.</span><span class="sxs-lookup"><span data-stu-id="07c08-250">In this section, you enable test user Britta Simon toouse Azure SSO by granting access tooCezanne HR software.</span></span>

![Test přístupu uživatele][200] 

1. <span data-ttu-id="07c08-252">V hello portálu Azure otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení.</span><span class="sxs-lookup"><span data-stu-id="07c08-252">In hello Azure portal, open hello applications view and then go toohello directory view.</span></span> <span data-ttu-id="07c08-253">Vyberte **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="07c08-253">Select **Enterprise applications** > **All applications**.</span></span>

    ![Hello "Všechny aplikace" odkaz][201] 

2. <span data-ttu-id="07c08-255">V seznamu aplikace hello vyberte **Cezanne HR softwaru**.</span><span class="sxs-lookup"><span data-stu-id="07c08-255">In hello applications list, select **Cezanne HR Software**.</span></span>

    ![seznam "Aplikací" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. <span data-ttu-id="07c08-257">V nabídce hello na levé straně hello vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="07c08-257">In hello menu on hello left, select **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="07c08-259">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="07c08-259">Select **Add**.</span></span> <span data-ttu-id="07c08-260">Potom v hello **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="07c08-260">Then in hello **Add Assignment** dialog box, select **Users and groups**.</span></span>

    !["Uživatelé a skupiny" odkaz][203]

5. <span data-ttu-id="07c08-262">V hello **uživatelů a skupin** dialogové okno, v hello **uživatelé** seznamu, vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="07c08-262">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="07c08-263">V hello **uživatelů a skupin** dialogové okno, vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="07c08-263">In hello **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="07c08-264">V hello **přidat přiřazení** dialogové okno, vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="07c08-264">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-sso"></a><span data-ttu-id="07c08-265">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="07c08-265">Test SSO</span></span>

<span data-ttu-id="07c08-266">V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="07c08-266">In this section, you test your Azure AD SSO configuration by using hello Access Panel.</span></span>

<span data-ttu-id="07c08-267">Když vyberete hello Cezanne HR softwaru dlaždice v hello přístupového panelu, přihlásit automaticky tooyour Cezanne HR softwarová aplikace.</span><span class="sxs-lookup"><span data-stu-id="07c08-267">When you select hello Cezanne HR software tile in hello Access Panel, you sign on automatically tooyour Cezanne HR software application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07c08-268">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07c08-268">Next steps</span></span>

* [<span data-ttu-id="07c08-269">Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="07c08-269">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="07c08-270">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="07c08-270">What is application access and SSO with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

