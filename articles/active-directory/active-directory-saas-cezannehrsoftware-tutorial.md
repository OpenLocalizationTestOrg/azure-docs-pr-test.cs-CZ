---
title: 'Kurz: Azure Active Directory integrace s Cezanne HR softwarem | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Cezanne HR softwarem a Azure Active Directory."
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
ms.openlocfilehash: 623c438edfce5f98c2d32d8bb25a97d86aa77909
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a><span data-ttu-id="9f61f-103">Kurz: Integrate Azure Active Directory s Cezanne HR softwaru</span><span class="sxs-lookup"><span data-stu-id="9f61f-103">Tutorial: Integrate Azure Active Directory with Cezanne HR software</span></span>

<span data-ttu-id="9f61f-104">V tomto kurzu zjistěte, jak integrovat Cezanne HR softwaru s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9f61f-104">In this tutorial, you learn how to integrate Cezanne HR software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9f61f-105">Integrace Cezanne HR softwaru s Azure AD poskytuje následující výhody.</span><span class="sxs-lookup"><span data-stu-id="9f61f-105">Integrating Cezanne HR software with Azure AD provides you with the following benefits.</span></span> <span data-ttu-id="9f61f-106">Můžete:</span><span class="sxs-lookup"><span data-stu-id="9f61f-106">You can:</span></span>

- <span data-ttu-id="9f61f-107">Řízení ve službě Azure AD, který má přístup k softwaru Cezanne oddělení lidských zdrojů.</span><span class="sxs-lookup"><span data-stu-id="9f61f-107">Control in Azure AD who has access to Cezanne HR software.</span></span>
- <span data-ttu-id="9f61f-108">Povolte uživatelům automaticky přihlásit k Cezanne HR software s jednotné přihlašování (SSO) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f61f-108">Enable your users to automatically sign in to Cezanne HR software with single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="9f61f-109">Spravovat účty v jednom centrálním místě: portál Azure.</span><span class="sxs-lookup"><span data-stu-id="9f61f-109">Manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="9f61f-110">Další informace o softwaru jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9f61f-110">To learn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and SSO with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f61f-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f61f-111">Prerequisites</span></span>

<span data-ttu-id="9f61f-112">Konfigurace integrace Azure AD s Cezanne HR softwaru, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9f61f-112">To configure Azure AD integration with Cezanne HR software, you need the following items:</span></span>

- <span data-ttu-id="9f61f-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f61f-113">An Azure AD subscription</span></span>
- <span data-ttu-id="9f61f-114">Software Cezanne HR předplatné povolené jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f61f-114">A Cezanne HR software SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9f61f-115">Chcete-li otestovat kroky v tomto kurzu, doporučujeme nepoužívejte provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f61f-115">To test the steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="9f61f-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="9f61f-116">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="9f61f-117">Nepoužívejte produkční prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9f61f-117">Don't use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9f61f-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9f61f-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9f61f-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9f61f-119">Scenario description</span></span>
<span data-ttu-id="9f61f-120">V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f61f-120">In this tutorial, you test Azure AD SSO in a test environment.</span></span> 

<span data-ttu-id="9f61f-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9f61f-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="9f61f-122">Přidání softwaru Cezanne HR z Galerie</span><span class="sxs-lookup"><span data-stu-id="9f61f-122">Adding Cezanne HR software from the gallery</span></span>
* <span data-ttu-id="9f61f-123">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="9f61f-123">Configuring and testing Azure AD SSO</span></span>

## <a name="add-cezanne-hr-software-from-the-gallery"></a><span data-ttu-id="9f61f-124">Přidat software Cezanne HR z Galerie</span><span class="sxs-lookup"><span data-stu-id="9f61f-124">Add Cezanne HR software from the gallery</span></span>
<span data-ttu-id="9f61f-125">Při konfiguraci integrace Cezanne HR softwaru do služby Azure AD přidáte do seznamu spravovaných aplikací SaaS Cezanne HR software z galerie.</span><span class="sxs-lookup"><span data-stu-id="9f61f-125">To configure the integration of Cezanne HR software into Azure AD, add Cezanne HR software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9f61f-126">Pokud chcete přidat Cezanne HR softwaru z galerie, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9f61f-126">To add Cezanne HR software from the gallery, do the following:</span></span>

1. <span data-ttu-id="9f61f-127">V  **[portál Azure](https://portal.azure.com)**, v levém podokně, vyberte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f61f-127">In the **[Azure portal](https://portal.azure.com)**, in the left pane, select the **Azure Active Directory** button.</span></span> 

    ![Tlačítko "Azure Active Directory"][1]

2. <span data-ttu-id="9f61f-129">Vyberte **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![Na odkaz "Všechny aplikace"][2]
    
3. <span data-ttu-id="9f61f-131">Chcete-li přidat novou aplikaci, v horní části **všechny aplikace** dialogové okno, vyberte **novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-131">To add a new application, at the top of the **All applications** dialog box, select **New application**.</span></span>

    !["Nová aplikace" tlačítko][3]

4. <span data-ttu-id="9f61f-133">Do vyhledávacího pole zadejte **Cezanne HR softwaru**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-133">In the search box, type **Cezanne HR Software**.</span></span>

    ![Do vyhledávacího pole](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. <span data-ttu-id="9f61f-135">V seznamu výsledků vyberte **Cezanne HR softwaru** a pak vyberte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9f61f-135">In the results list, select **Cezanne HR Software** and then select the **Add** button to add the application.</span></span>

    ![Seznam výsledků](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9f61f-137">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f61f-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="9f61f-138">V této části můžete nakonfigurovat a otestovat jednotného přihlašování k AD Azure s Cezanne HR software založený na testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9f61f-138">In this section, you configure and test Azure AD SSO with Cezanne HR software based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9f61f-139">Pro jednotné přihlašování pro práci Azure AD musí znát protějškem softwaru Cezanne HR uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f61f-139">For SSO to work, Azure AD needs to know the Cezanne HR software counterpart to the Azure AD user.</span></span> <span data-ttu-id="9f61f-140">Jinými slovy je potřeba vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské v oddělení lidských zdrojů Cezanne softwaru.</span><span class="sxs-lookup"><span data-stu-id="9f61f-140">In other words, you must establish a link relationship between an Azure AD user and the related user in the Cezanne HR software.</span></span>

<span data-ttu-id="9f61f-141">K navázání vztahu odkaz, přiřazení softwaru Cezanne HR **uživatelské jméno** hodnotu jako Azure AD **uživatelské jméno** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9f61f-141">To establish the link relationship, assign the Cezanne HR software **user name** value as the Azure AD **Username** value.</span></span>

<span data-ttu-id="9f61f-142">Nakonfigurovat a otestovat Azure AD SSO pomocí softwaru Cezanne oddělení lidských zdrojů, dokončete následující stavební bloky.</span><span class="sxs-lookup"><span data-stu-id="9f61f-142">To configure and test Azure AD SSO by using Cezanne HR software, complete the following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="9f61f-143">Konfigurovat Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f61f-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="9f61f-144">V této části můžete povolení jednotného přihlašování Azure AD na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Cezanne HR softwaru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9f61f-144">In this section, you can enable Azure AD SSO in the Azure portal and configure SSO in your Cezanne HR software application by doing the following:</span></span>

1. <span data-ttu-id="9f61f-145">Na portálu Azure na **Cezanne HR softwaru** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-145">In the Azure portal, on the **Cezanne HR Software** application integration page, select **Single sign-on**.</span></span>

    ![Příkaz "Jednotného přihlašování"][4]

2. <span data-ttu-id="9f61f-147">Pro povolení jednotného přihlašování, v **jednotného přihlašování** dialogové okno, vyberte **režimu** jako **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-147">To enable SSO, in the **Single sign-on** dialog box, select the **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![Do pole "Režim"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. <span data-ttu-id="9f61f-149">V části **Cezanne HR softwaru domény a adresy URL**, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9f61f-149">Under **Cezanne HR Software Domain and URLs**, do the following:</span></span>

    ![V části "Cezanne HR softwaru domény adresy URL a"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    <span data-ttu-id="9f61f-151">a.</span><span class="sxs-lookup"><span data-stu-id="9f61f-151">a.</span></span> <span data-ttu-id="9f61f-152">V **přihlašovací adresa URL** zadejte adresu URL, která má následující syntaxi:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="9f61f-152">In the **Sign-on URL** box, type a URL that has the following syntax: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span></span>

    <span data-ttu-id="9f61f-153">b.</span><span class="sxs-lookup"><span data-stu-id="9f61f-153">b.</span></span> <span data-ttu-id="9f61f-154">V **adresa URL odpovědi** zadejte adresu URL, která má následující syntaxi:`https://w3.cezanneondemand.com:443/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="9f61f-154">In the **Reply URL** box, type a URL that has the following syntax: `https://w3.cezanneondemand.com:443/<tenantid>`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="9f61f-155">Předchozí hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9f61f-155">The preceding values are not real.</span></span> <span data-ttu-id="9f61f-156">Adresa URL skutečné odpovědi a adresa URL přihlašování je aktualizujte.</span><span class="sxs-lookup"><span data-stu-id="9f61f-156">Update them with the actual reply URL and the sign-on URL.</span></span> <span data-ttu-id="9f61f-157">Chcete-li získat hodnoty, obraťte se [tým podpory klientský software Cezanne HR](mailto:info@cezannehr.com).</span><span class="sxs-lookup"><span data-stu-id="9f61f-157">To obtain the values, contact the [Cezanne HR software client support team](mailto:info@cezannehr.com).</span></span>

4. <span data-ttu-id="9f61f-158">V části **SAML podpisový certifikát**, vyberte **certifikátu (Base64)**a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="9f61f-158">Under **SAML Signing Certificate**, select **Certificate (Base64)**, and then save the certificate file on your computer.</span></span>

    ![V části "SAML podpisový certifikát"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. <span data-ttu-id="9f61f-160">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-160">Select **Save**.</span></span>

    ![Tlačítko "Uložit"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="9f61f-162">V části **Cezanne HR softwarové konfigurace**, vyberte **konfigurace softwaru HR Cezanne** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9f61f-162">Under **Cezanne HR Software Configuration**, select **Configure Cezanne HR Software** to open the **Configure sign-on** window.</span></span> <span data-ttu-id="9f61f-163">Kopírování **SAML Entity ID** a **SAML-služby přihlášení** adresa URL z **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="9f61f-163">Copy the **SAML Entity ID** and **SAML Single Sign-On Service** URL from the **Quick Reference** section.</span></span>

    ![V části "Konfigurace softwaru HR Cezanne"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. <span data-ttu-id="9f61f-165">V okně prohlížeče jiný web Přihlaste se ke klientovi Cezanne HR softwaru jako správce.</span><span class="sxs-lookup"><span data-stu-id="9f61f-165">In a different web browser window, sign on to your Cezanne HR software tenant as an administrator.</span></span>

8. <span data-ttu-id="9f61f-166">V levém podokně vyberte **nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-166">In the left pane, select **System Setup**.</span></span> <span data-ttu-id="9f61f-167">Vyberte **nastavení zabezpečení** > **jednotné přihlašování konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-167">Select **Security Settings** > **Single Sign-On Configuration**.</span></span>

    ![Na odkaz "Jednoho přihlášení konfigurace"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. <span data-ttu-id="9f61f-169">V **umožňují uživatelům přihlásit pomocí následujících služeb jednotné přihlašování (SSO)** podokně, vyberte **SAML 2.0** zaškrtávací políčko a vyberte **Upřesnit konfiguraci** možnost.</span><span class="sxs-lookup"><span data-stu-id="9f61f-169">In the **Allow users to log in using the following Single Sign-On (SSO) services** pane, select the **SAML 2.0** check box and select the **Advanced Configuration** option.</span></span>

    ![Jednotné přihlašování možnosti služeb](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. <span data-ttu-id="9f61f-171">Vyberte **přidat nové**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-171">Select **Add New**.</span></span>

    ![Tlačítko "Přidat nové"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. <span data-ttu-id="9f61f-173">V části **zprostředkovatelů Identity SAML 2.0**, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9f61f-173">Under **SAML 2.0 Identity Providers**, do the following:</span></span>

    ![V části "Zprostředkovatelů Identity SAML 2.0"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    <span data-ttu-id="9f61f-175">a.</span><span class="sxs-lookup"><span data-stu-id="9f61f-175">a.</span></span> <span data-ttu-id="9f61f-176">V **zobrazovaný název** pole, zadejte název zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="9f61f-176">In the **Display Name** box, enter the name of your identity provider.</span></span>

    <span data-ttu-id="9f61f-177">b.</span><span class="sxs-lookup"><span data-stu-id="9f61f-177">b.</span></span> <span data-ttu-id="9f61f-178">V **identifikátor Entity** pole, vložte **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9f61f-178">In the **Entity Identifier** box, paste the **SAML Entity ID** that you copied from the Azure portal.</span></span> 

    <span data-ttu-id="9f61f-179">c.</span><span class="sxs-lookup"><span data-stu-id="9f61f-179">c.</span></span> <span data-ttu-id="9f61f-180">V **SAML vazby** pole se seznamem, vyberte **POST**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-180">In the **SAML Binding** list box, select **POST**.</span></span>

    <span data-ttu-id="9f61f-181">d.</span><span class="sxs-lookup"><span data-stu-id="9f61f-181">d.</span></span> <span data-ttu-id="9f61f-182">V **koncový bod služby tokenu zabezpečení** pole, vložte **SAML-služby přihlášení** adresu URL, kterou jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9f61f-182">In the **Security Token Service Endpoint** box, paste the **SAML Single Sign-On Service** URL that you copied from the Azure portal.</span></span> 
    
    <span data-ttu-id="9f61f-183">e.</span><span class="sxs-lookup"><span data-stu-id="9f61f-183">e.</span></span> <span data-ttu-id="9f61f-184">V **název atributu ID uživatele** zadejte `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="9f61f-184">In the **User ID Attribute Name** box, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="9f61f-185">f.</span><span class="sxs-lookup"><span data-stu-id="9f61f-185">f.</span></span> <span data-ttu-id="9f61f-186">Chcete-li nahrát na server certifikát stažený z Azure AD, vyberte **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f61f-186">To upload the downloaded certificate from Azure AD, select the **Upload** button.</span></span>
    
    <span data-ttu-id="9f61f-187">g.</span><span class="sxs-lookup"><span data-stu-id="9f61f-187">g.</span></span> <span data-ttu-id="9f61f-188">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-188">Select **OK**.</span></span> 

12. <span data-ttu-id="9f61f-189">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-189">Select **Save**.</span></span>

    ![Tlačítko "Uložit"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> <span data-ttu-id="9f61f-191">Jak nastavit aplikaci si můžete přečíst stručným verzi podle předchozích pokynů v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9f61f-191">As you set up the app, you can read a concise version of the preceding instructions in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9f61f-192">Po přidání aplikace z **služby Active Directory** > **podnikové aplikace, které** vyberte **jednotného přihlašování** kartě. Přejděte k embedded dokumentace z **konfigurace** části.</span><span class="sxs-lookup"><span data-stu-id="9f61f-192">After you add the app from the **Active Directory** > **Enterprise applications** section, select the **Single sign-on** tab. Then access the embedded documentation from the **Configuration** section.</span></span> 

<span data-ttu-id="9f61f-193">Další informace o funkci embedded dokumentaci najdete v tématu [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="9f61f-193">To learn more about the embedded documentation feature, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9f61f-194">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f61f-194">Create an Azure AD test user</span></span>
<span data-ttu-id="9f61f-195">V této části vytvoříte testovacího uživatele Britta Simon na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9f61f-195">In this section, you create test user Britta Simon in the Azure portal.</span></span>

![Britta Simon testovacího uživatele][100]

<span data-ttu-id="9f61f-197">Vytvoření zkušebního uživatele ve službě Azure AD, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9f61f-197">To create a test user in Azure AD, do the following:</span></span>

1. <span data-ttu-id="9f61f-198">V **portál Azure**, v levém podokně, vyberte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f61f-198">In the **Azure portal**, in the left pane, select the **Azure Active Directory** button.</span></span>

    ![Tlačítko "Azure Active Directory"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9f61f-200">Chcete-li zobrazit seznam uživatelů, vyberte **uživatelů a skupin** > **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-200">To display the list of users, select **Users and groups** > **All users**.</span></span>
    
    ![Na odkaz "Všichni uživatelé"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    <span data-ttu-id="9f61f-202">**Všichni uživatelé** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f61f-202">The **All users** dialog box opens.</span></span>

3. <span data-ttu-id="9f61f-203">Chcete-li otevřít **uživatele** dialogové okno, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-203">To open the **User** dialog box, select **Add**.</span></span>
 
    ![Tlačítko "Přidat"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9f61f-205">V **uživatele** dialogové okno pole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9f61f-205">In the **User** dialog box, do the following:</span></span>
 
    ![Dialogové okno "User"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9f61f-207">a.</span><span class="sxs-lookup"><span data-stu-id="9f61f-207">a.</span></span> <span data-ttu-id="9f61f-208">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-208">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9f61f-209">b.</span><span class="sxs-lookup"><span data-stu-id="9f61f-209">b.</span></span> <span data-ttu-id="9f61f-210">V **uživatelské jméno** pole, zadejte uživatele Britta Simon **e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-210">In the **User name** box, type user Britta Simon's **email address**.</span></span>

    <span data-ttu-id="9f61f-211">c.</span><span class="sxs-lookup"><span data-stu-id="9f61f-211">c.</span></span> <span data-ttu-id="9f61f-212">Vyberte **zobrazit hesla** zaškrtněte políčko a poznamenejte si hodnotu, která byla vygenerována v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="9f61f-212">Select the **Show Password** check box, and then note the value that was generated in the **Password** box.</span></span>

    <span data-ttu-id="9f61f-213">d.</span><span class="sxs-lookup"><span data-stu-id="9f61f-213">d.</span></span> <span data-ttu-id="9f61f-214">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-214">Select **Create**.</span></span>
 
### <a name="create-a-cezanne-hr-software-test-user"></a><span data-ttu-id="9f61f-215">Vytvoření zkušebního uživatele Cezanne HR softwaru</span><span class="sxs-lookup"><span data-stu-id="9f61f-215">Create a Cezanne HR software test user</span></span>

<span data-ttu-id="9f61f-216">Povolit uživatelům Azure AD přihlášení k Cezanne HR softwaru, musí být zřízená do Cezanne HR softwaru.</span><span class="sxs-lookup"><span data-stu-id="9f61f-216">To enable Azure AD users to sign in to Cezanne HR software, they must be provisioned into Cezanne HR software.</span></span> <span data-ttu-id="9f61f-217">V případě Cezanne HR softwaru zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="9f61f-217">In the case of Cezanne HR software, provisioning is a manual task.</span></span>

<span data-ttu-id="9f61f-218">Poskytnutí uživatelského účtu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9f61f-218">Provision a user account by doing the following:</span></span>

1.  <span data-ttu-id="9f61f-219">Přihlaste se k serveru vaší společnosti softwaru Cezanne HR jako správce.</span><span class="sxs-lookup"><span data-stu-id="9f61f-219">Sign in to your Cezanne HR software company site as an administrator.</span></span>

2.  <span data-ttu-id="9f61f-220">V levém podokně vyberte **nastavení systému** > **spravovat uživatele** > **přidat nové uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-220">In the left pane, select **System Setup** > **Manage Users** > **Add New User**.</span></span>

    <span data-ttu-id="9f61f-221">![Na odkaz "Přidat nový uživatel"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="9f61f-221">![The "Add New User" link](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "New User")</span></span>

3.  <span data-ttu-id="9f61f-222">V části **osoba podrobnosti**, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9f61f-222">Under **Person Details**, do the following:</span></span>

    <span data-ttu-id="9f61f-223">![V části "Osoba podrobnosti"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="9f61f-223">![The "Person Details" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "New User")</span></span>
    
    <span data-ttu-id="9f61f-224">a.</span><span class="sxs-lookup"><span data-stu-id="9f61f-224">a.</span></span> <span data-ttu-id="9f61f-225">Nastavit **interní uživatele** jako **OFF**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-225">Set **Internal User** as **OFF**.</span></span>
    
    <span data-ttu-id="9f61f-226">b.</span><span class="sxs-lookup"><span data-stu-id="9f61f-226">b.</span></span> <span data-ttu-id="9f61f-227">V **křestní jméno** zadejte jméno uživatele, například **Britta**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-227">In the **First Name** box, type the user's first name, for example, **Britta**.</span></span>  
 
    <span data-ttu-id="9f61f-228">c.</span><span class="sxs-lookup"><span data-stu-id="9f61f-228">c.</span></span> <span data-ttu-id="9f61f-229">V **příjmení** zadejte příjmení uživatele, například **Simon**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-229">In the **Last Name** box, type the user's last name, for example, **Simon**.</span></span>
    
    <span data-ttu-id="9f61f-230">d.</span><span class="sxs-lookup"><span data-stu-id="9f61f-230">d.</span></span> <span data-ttu-id="9f61f-231">V **e-mailu** zadejte e-mailovou adresu uživatele, například Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="9f61f-231">In the **E-mail** box, type the user's email address, for example, Brittasimon@contoso.com.</span></span>

4.  <span data-ttu-id="9f61f-232">V části **informace o účtu**, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9f61f-232">Under **Account Information**, do the following:</span></span>

    <span data-ttu-id="9f61f-233">![V části "Informace o účtu"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="9f61f-233">![The "Account Information" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "New User")</span></span>
    
    <span data-ttu-id="9f61f-234">a.</span><span class="sxs-lookup"><span data-stu-id="9f61f-234">a.</span></span> <span data-ttu-id="9f61f-235">V **uživatelské jméno** zadejte e-mailovou adresu uživatele, například Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="9f61f-235">In the **Username** box, type the user's email address, for example, Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="9f61f-236">b.</span><span class="sxs-lookup"><span data-stu-id="9f61f-236">b.</span></span> <span data-ttu-id="9f61f-237">V **heslo** zadejte heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="9f61f-237">In the **Password** box, type the user's password.</span></span>
    
    <span data-ttu-id="9f61f-238">c.</span><span class="sxs-lookup"><span data-stu-id="9f61f-238">c.</span></span> <span data-ttu-id="9f61f-239">V **Role zabezpečení** vyberte **HR Professional**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-239">In the **Security Role** box, select **HR Professional**.</span></span>
    
    <span data-ttu-id="9f61f-240">d.</span><span class="sxs-lookup"><span data-stu-id="9f61f-240">d.</span></span> <span data-ttu-id="9f61f-241">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-241">Select **OK**.</span></span>

5. <span data-ttu-id="9f61f-242">Na **jednotného přihlašování** ve **SAML 2.0 identifikátory** vyberte **přidat nové**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-242">On the **Single sign-on** tab, in the **SAML 2.0 Identifiers** section, select **Add New**.</span></span>

    <span data-ttu-id="9f61f-243">![Tlačítko "Přidat nové"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="9f61f-243">![The "Add New" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "User")</span></span>

6. <span data-ttu-id="9f61f-244">V **zprostředkovatele Identity** vyberte zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="9f61f-244">In the **Identity Provider** list box, select your identity provider.</span></span> <span data-ttu-id="9f61f-245">V **uživatelský identifikátor** zadejte e-mailovou adresu pro testovací uživatele Britta Simon na účet.</span><span class="sxs-lookup"><span data-stu-id="9f61f-245">In the **User Identifier** box, enter the email address for test user Britta Simon's account.</span></span>

    <span data-ttu-id="9f61f-246">![Do polí "Zprostředkovatele Identity" a "Uživatelský identifikátor"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="9f61f-246">![The "Identity Provider" and "User Identifier" boxes](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "User")</span></span>
    
7. <span data-ttu-id="9f61f-247">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-247">Select **Save**.</span></span>

    <span data-ttu-id="9f61f-248">![Tlačítko "Uložit"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="9f61f-248">![The "Save" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "User")</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="9f61f-249">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f61f-249">Assign the Azure AD test user</span></span>

<span data-ttu-id="9f61f-250">V této části povolíte testovacího uživatele Britta Simon používat jednotného přihlašování k Azure tak, že udělíte přístup k softwaru Cezanne oddělení lidských zdrojů.</span><span class="sxs-lookup"><span data-stu-id="9f61f-250">In this section, you enable test user Britta Simon to use Azure SSO by granting access to Cezanne HR software.</span></span>

![Test přístupu uživatele][200] 

1. <span data-ttu-id="9f61f-252">Na portálu Azure otevřete zobrazení aplikace a pak přejděte do zobrazení adresáře.</span><span class="sxs-lookup"><span data-stu-id="9f61f-252">In the Azure portal, open the applications view and then go to the directory view.</span></span> <span data-ttu-id="9f61f-253">Vyberte **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-253">Select **Enterprise applications** > **All applications**.</span></span>

    ![Na odkaz "Všechny aplikace"][201] 

2. <span data-ttu-id="9f61f-255">V seznamu aplikací vyberte **Cezanne HR softwaru**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-255">In the applications list, select **Cezanne HR Software**.</span></span>

    ![Seznam "Aplikací"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. <span data-ttu-id="9f61f-257">V nabídce na levé straně vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-257">In the menu on the left, select **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9f61f-259">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-259">Select **Add**.</span></span> <span data-ttu-id="9f61f-260">Potom v **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-260">Then in the **Add Assignment** dialog box, select **Users and groups**.</span></span>

    !["Uživatelé a skupiny" odkaz][203]

5. <span data-ttu-id="9f61f-262">V **uživatelů a skupin** v dialogovém **uživatelé** seznamu, vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-262">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="9f61f-263">V **uživatelů a skupin** dialogové okno, vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-263">In the **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="9f61f-264">V **přidat přiřazení** dialogové okno, vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="9f61f-264">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-sso"></a><span data-ttu-id="9f61f-265">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f61f-265">Test SSO</span></span>

<span data-ttu-id="9f61f-266">V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9f61f-266">In this section, you test your Azure AD SSO configuration by using the Access Panel.</span></span>

<span data-ttu-id="9f61f-267">Když vyberete dlaždici Cezanne HR softwaru na přístupovém panelu, můžete přihlásit automaticky do vaší aplikace Cezanne HR softwaru.</span><span class="sxs-lookup"><span data-stu-id="9f61f-267">When you select the Cezanne HR software tile in the Access Panel, you sign on automatically to your Cezanne HR software application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f61f-268">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f61f-268">Next steps</span></span>

* [<span data-ttu-id="9f61f-269">Seznam kurzů k integraci aplikací SaaS v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f61f-269">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9f61f-270">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9f61f-270">What is application access and SSO with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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

