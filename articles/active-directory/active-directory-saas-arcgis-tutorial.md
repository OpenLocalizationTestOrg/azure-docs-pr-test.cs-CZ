---
title: 'Kurz: Azure Active Directory integrace s ArcGIS Online | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ArcGIS Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: f3dd55d798cf3256fb2758e011f33946baa405ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a><span data-ttu-id="3c9dd-103">Kurz: Azure Active Directory integrace s ArcGIS Online</span><span class="sxs-lookup"><span data-stu-id="3c9dd-103">Tutorial: Azure Active Directory integration with ArcGIS Online</span></span>

<span data-ttu-id="3c9dd-104">V tomto kurzu zjistíte, jak toointegrate ArcGIS Online se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3c9dd-104">In this tutorial, you learn how toointegrate ArcGIS Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3c9dd-105">Integrace ArcGIS Online s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3c9dd-105">Integrating ArcGIS Online with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3c9dd-106">Můžete řídit ve službě Azure AD, který má přístup tooArcGIS Online</span><span class="sxs-lookup"><span data-stu-id="3c9dd-106">You can control in Azure AD who has access tooArcGIS Online</span></span>
- <span data-ttu-id="3c9dd-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooArcGIS Online (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3c9dd-107">You can enable your users tooautomatically get signed-on tooArcGIS Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3c9dd-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3c9dd-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3c9dd-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3c9dd-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with ArcGIS Online, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="3c9dd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3c9dd-110">Prerequisites</span></span>

<span data-ttu-id="3c9dd-111">tooconfigure integrace Azure AD s ArcGIS Online, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3c9dd-111">tooconfigure Azure AD integration with ArcGIS Online, you need hello following items:</span></span>

- <span data-ttu-id="3c9dd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3c9dd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3c9dd-113">ArcGIS Online jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3c9dd-113">A ArcGIS Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3c9dd-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3c9dd-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3c9dd-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3c9dd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3c9dd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3c9dd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3c9dd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3c9dd-118">Scenario description</span></span>
<span data-ttu-id="3c9dd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3c9dd-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3c9dd-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3c9dd-121">Přidání ArcGIS Online z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3c9dd-121">Adding ArcGIS Online from hello gallery</span></span>
2. <span data-ttu-id="3c9dd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3c9dd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-arcgis-online-from-hello-gallery"></a><span data-ttu-id="3c9dd-123">Přidání ArcGIS Online z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3c9dd-123">Adding ArcGIS Online from hello gallery</span></span>
<span data-ttu-id="3c9dd-124">tooconfigure hello integrace ArcGIS Online do služby Azure AD, je nutné tooadd ArcGIS Online hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-124">tooconfigure hello integration of ArcGIS Online into Azure AD, you need tooadd ArcGIS Online from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3c9dd-125">**tooadd ArcGIS Online z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3c9dd-125">**tooadd ArcGIS Online from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3c9dd-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3c9dd-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3c9dd-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3c9dd-131">Klikněte na tlačítko **novou aplikaci** tlačítka v horní části hello hello dialogové okno tooadd nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-131">Click **New application** button on hello top of hello dialog tooadd new application.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3c9dd-133">Hello vyhledávacího pole zadejte **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-133">In hello search box, type **ArcGIS Online**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. <span data-ttu-id="3c9dd-135">Na panelu výsledků hello vyberte **ArcGIS Online**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-135">In hello results panel, select **ArcGIS Online**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3c9dd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3c9dd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3c9dd-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ArcGIS Online na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3c9dd-138">In this section, you configure and test Azure AD single sign-on with ArcGIS Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3c9dd-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ArcGIS Online je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ArcGIS Online is tooa user in Azure AD.</span></span> <span data-ttu-id="3c9dd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ArcGIS Online musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-140">In other words, a link relationship between an Azure AD user and hello related user in ArcGIS Online needs toobe established.</span></span>

<span data-ttu-id="3c9dd-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ArcGIS Online.</span></span>

<span data-ttu-id="3c9dd-142">tooconfigure a testu Azure AD jednotné přihlašování s ArcGIS Online, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="3c9dd-142">tooconfigure and test Azure AD single sign-on with ArcGIS Online, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3c9dd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3c9dd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3c9dd-145">**[Vytvoření ArcGIS Online testovacího uživatele](#creating-an-arcgis-online-test-user)**  -toohave protějšek Britta Simon v ArcGIS Online, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-145">**[Creating an ArcGIS Online test user](#creating-an-arcgis-online-test-user)** - toohave a counterpart of Britta Simon in ArcGIS Online that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3c9dd-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3c9dd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3c9dd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3c9dd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3c9dd-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ArcGIS Online application.</span></span>

<span data-ttu-id="3c9dd-150">**tooconfigure Azure AD jednotné přihlašování s ArcGIS Online, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3c9dd-150">**tooconfigure Azure AD single sign-on with ArcGIS Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="3c9dd-151">V portálu Azure, na hello hello **ArcGIS Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-151">In hello Azure portal, on hello **ArcGIS Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3c9dd-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. <span data-ttu-id="3c9dd-155">Na hello **ArcGIS Online domény a adresy URL** část, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="3c9dd-155">On hello **ArcGIS Online Domain and URLs** section, perform hello following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    <span data-ttu-id="3c9dd-157">V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<company>.maps.arcgis.com`</span><span class="sxs-lookup"><span data-stu-id="3c9dd-157">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<company>.maps.arcgis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3c9dd-158">Tato hodnota není skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-158">This value is not hello real.</span></span> <span data-ttu-id="3c9dd-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="3c9dd-160">Obraťte se na [tým podpory Online klienta ArcGIS](http://support.esri.com/) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-160">Contact [ArcGIS Online Client support team](http://support.esri.com/) tooget this value.</span></span> 

4. <span data-ttu-id="3c9dd-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. <span data-ttu-id="3c9dd-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3c9dd-165">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti ArcGIS.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-165">In a different web browser window, log into your ArcGIS company site as an administrator.</span></span>

7. <span data-ttu-id="3c9dd-166">Klikněte na tlačítko **upravovat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-166">Click **EDIT SETTINGS**.</span></span>

    <span data-ttu-id="3c9dd-167">![Upravit nastavení](./media/active-directory-saas-arcgis-tutorial/ic784742.png "upravit nastavení")</span><span class="sxs-lookup"><span data-stu-id="3c9dd-167">![Edit Settings](./media/active-directory-saas-arcgis-tutorial/ic784742.png "Edit Settings")</span></span>

8. <span data-ttu-id="3c9dd-168">Klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-168">Click **Security**.</span></span>

    <span data-ttu-id="3c9dd-169">![Zabezpečení](./media/active-directory-saas-arcgis-tutorial/ic784743.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="3c9dd-169">![Security](./media/active-directory-saas-arcgis-tutorial/ic784743.png "Security")</span></span>

9. <span data-ttu-id="3c9dd-170">V části **přihlášení Enterprise**, klikněte na tlačítko **nastavit zprostředkovatele IDENTITY**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-170">Under **Enterprise Logins**, click **SET IDENTITY PROVIDER**.</span></span>

    <span data-ttu-id="3c9dd-171">![Přihlášení Enterprise](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise přihlášení")</span><span class="sxs-lookup"><span data-stu-id="3c9dd-171">![Enterprise Logins](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise Logins")</span></span>

10. <span data-ttu-id="3c9dd-172">Na hello **nastavit zprostředkovatele Identity** konfigurace proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3c9dd-172">On hello **Set Identity Provider** configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="3c9dd-173">![Nastavení zprostředkovatele Identity](./media/active-directory-saas-arcgis-tutorial/ic784745.png "nastavit zprostředkovatele Identity")</span><span class="sxs-lookup"><span data-stu-id="3c9dd-173">![Set Identity Provider](./media/active-directory-saas-arcgis-tutorial/ic784745.png "Set Identity Provider")</span></span>
   
    <span data-ttu-id="3c9dd-174">a.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-174">a.</span></span> <span data-ttu-id="3c9dd-175">V hello **název** textovému poli, zadejte název vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-175">In hello **Name** textbox, type your organization’s name.</span></span>

    <span data-ttu-id="3c9dd-176">b.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-176">b.</span></span> <span data-ttu-id="3c9dd-177">Pro **Metadata pro hello zprostředkovatele Identity Enterprise budou předávána prostřednictvím**, vyberte **A soubor**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-177">For **Metadata for hello Enterprise Identity Provider will be supplied using**, select **A File**.</span></span>

    <span data-ttu-id="3c9dd-178">c.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-178">c.</span></span> <span data-ttu-id="3c9dd-179">tooupload váš soubor stažený metadata, klikněte na tlačítko **zvolte soubor**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-179">tooupload your downloaded metadata file, click **Choose file**.</span></span>

    <span data-ttu-id="3c9dd-180">d.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-180">d.</span></span> <span data-ttu-id="3c9dd-181">Klikněte na tlačítko **zprostředkovatele IDENTITY sadu**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-181">Click **SET IDENTITY PROVIDER**.</span></span>

> [!TIP]
> <span data-ttu-id="3c9dd-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="3c9dd-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3c9dd-183">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3c9dd-184">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3c9dd-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3c9dd-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3c9dd-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="3c9dd-186">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3c9dd-188">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3c9dd-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3c9dd-189">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3c9dd-191">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-191">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3c9dd-193">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-193">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3c9dd-195">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3c9dd-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3c9dd-197">a.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-197">a.</span></span> <span data-ttu-id="3c9dd-198">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3c9dd-199">b.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-199">b.</span></span> <span data-ttu-id="3c9dd-200">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-200">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="3c9dd-201">c.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-201">c.</span></span> <span data-ttu-id="3c9dd-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3c9dd-203">d.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-203">d.</span></span> <span data-ttu-id="3c9dd-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-204">Click **Create**.</span></span>
 
### <a name="creating-an-arcgis-online-test-user"></a><span data-ttu-id="3c9dd-205">Vytvoření ArcGIS Online testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="3c9dd-205">Creating an ArcGIS Online test user</span></span>

<span data-ttu-id="3c9dd-206">V pořadí tooenable Azure AD Uživatelé toolog do ArcGIS Online musí být zřízená do ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-206">In order tooenable Azure AD users toolog into ArcGIS Online, they must be provisioned into ArcGIS Online.</span></span>  
<span data-ttu-id="3c9dd-207">V případě hello ArcGIS online zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-207">In hello case of ArcGIS Online, provisioning is a manual task.</span></span>

<span data-ttu-id="3c9dd-208">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3c9dd-208">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="3c9dd-209">Přihlaste se tooyour **ArcGIS** klienta.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-209">Log in tooyour **ArcGIS** tenant.</span></span>

2. <span data-ttu-id="3c9dd-210">Klikněte na tlačítko **POZVAT členy**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-210">Click **INVITE MEMBERS**.</span></span>
   
    <span data-ttu-id="3c9dd-211">![Pozvěte členy](./media/active-directory-saas-arcgis-tutorial/ic784747.png "pozvat členy")</span><span class="sxs-lookup"><span data-stu-id="3c9dd-211">![Invite Members](./media/active-directory-saas-arcgis-tutorial/ic784747.png "Invite Members")</span></span>

3. <span data-ttu-id="3c9dd-212">Vyberte **přidat členy automaticky bez odeslání e-mailu**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-212">Select **Add members automatically without sending an email**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="3c9dd-213">![Automaticky přidat členy](./media/active-directory-saas-arcgis-tutorial/ic784748.png "automaticky přidat členy")</span><span class="sxs-lookup"><span data-stu-id="3c9dd-213">![Add Members Automatically](./media/active-directory-saas-arcgis-tutorial/ic784748.png "Add Members Automatically")</span></span>

4. <span data-ttu-id="3c9dd-214">Na hello **členy** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3c9dd-214">On hello **Members** dialog page, perform hello following steps:</span></span>
   
     <span data-ttu-id="3c9dd-215">![Přidat a zkontrolovat](./media/active-directory-saas-arcgis-tutorial/ic784749.png "přidat a zkontrolujte")</span><span class="sxs-lookup"><span data-stu-id="3c9dd-215">![Add and review](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Add and review")</span></span>
    
     <span data-ttu-id="3c9dd-216">a.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-216">a.</span></span> <span data-ttu-id="3c9dd-217">Zadejte hello **e-mailu**, **křestní jméno**, a **příjmení** platného účtu AAD chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-217">Enter hello **Email**, **First Name**, and **Last Name** of a valid AAD account you want tooprovision.</span></span>
  
     <span data-ttu-id="3c9dd-218">b.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-218">b.</span></span> <span data-ttu-id="3c9dd-219">Klikněte na tlačítko **přidat a ZKONTROLUJTE**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-219">Click **ADD AND REVIEW**.</span></span>
5. <span data-ttu-id="3c9dd-220">Zkontrolujte data hello jste zadali a pak klikněte na tlačítko **přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-220">Review hello data you have entered, and then click **ADD MEMBERS**.</span></span>
   
    <span data-ttu-id="3c9dd-221">![Přidat člena](./media/active-directory-saas-arcgis-tutorial/ic784750.png "přidat člena")</span><span class="sxs-lookup"><span data-stu-id="3c9dd-221">![Add member](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Add member")</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="3c9dd-222">Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-222">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3c9dd-223">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="3c9dd-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3c9dd-224">V této části povolíte tak, že udělíte přístup tooArcGIS Online toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooArcGIS Online.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3c9dd-226">**tooassign Britta Simon tooArcGIS Online, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3c9dd-226">**tooassign Britta Simon tooArcGIS Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="3c9dd-227">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3c9dd-229">V seznamu aplikace hello vyberte **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-229">In hello applications list, select **ArcGIS Online**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. <span data-ttu-id="3c9dd-231">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3c9dd-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-233">Click **Add** button.</span></span> <span data-ttu-id="3c9dd-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3c9dd-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3c9dd-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3c9dd-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3c9dd-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3c9dd-239">Testing single sign-on</span></span>

<span data-ttu-id="3c9dd-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3c9dd-241">Po kliknutí na tlačítko hello ArcGIS Online dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ArcGIS Online aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9dd-241">When you click hello ArcGIS Online tile in hello Access Panel, you should get automatically signed-on tooyour ArcGIS Online application.</span></span>
<span data-ttu-id="3c9dd-242">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3c9dd-242">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c9dd-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3c9dd-243">Additional resources</span></span>

* [<span data-ttu-id="3c9dd-244">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3c9dd-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3c9dd-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3c9dd-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

