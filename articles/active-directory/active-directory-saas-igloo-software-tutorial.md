---
title: 'Kurz: Azure Active Directory integrace s Igloo softwarem | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Igloo softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 406405d4faa6e56f1005a61e69a29ef2ef2eb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a><span data-ttu-id="227bd-103">Kurz: Azure Active Directory integrace s Igloo softwaru</span><span class="sxs-lookup"><span data-stu-id="227bd-103">Tutorial: Azure Active Directory integration with Igloo Software</span></span>

<span data-ttu-id="227bd-104">V tomto kurzu zjistíte, jak toointegrate Igloo Software s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="227bd-104">In this tutorial, you learn how toointegrate Igloo Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="227bd-105">Integrace Igloo softwaru s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="227bd-105">Integrating Igloo Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="227bd-106">Můžete řídit ve službě Azure AD, který má přístup tooIgloo softwaru</span><span class="sxs-lookup"><span data-stu-id="227bd-106">You can control in Azure AD who has access tooIgloo Software</span></span>
- <span data-ttu-id="227bd-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooIgloo softwaru (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="227bd-107">You can enable your users tooautomatically get signed-on tooIgloo Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="227bd-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="227bd-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="227bd-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="227bd-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="227bd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="227bd-110">Prerequisites</span></span>

<span data-ttu-id="227bd-111">tooconfigure integrace Azure AD s Igloo softwaru, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="227bd-111">tooconfigure Azure AD integration with Igloo Software, you need hello following items:</span></span>

- <span data-ttu-id="227bd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="227bd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="227bd-113">Softwaru Igloo jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="227bd-113">An Igloo Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="227bd-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="227bd-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="227bd-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="227bd-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="227bd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="227bd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="227bd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="227bd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="227bd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="227bd-118">Scenario description</span></span>
<span data-ttu-id="227bd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="227bd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="227bd-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="227bd-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="227bd-121">Přidání softwaru Igloo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="227bd-121">Adding Igloo Software from hello gallery</span></span>
2. <span data-ttu-id="227bd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="227bd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-igloo-software-from-hello-gallery"></a><span data-ttu-id="227bd-123">Přidání softwaru Igloo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="227bd-123">Adding Igloo Software from hello gallery</span></span>
<span data-ttu-id="227bd-124">tooconfigure hello integrace Igloo softwaru do služby Azure AD, je nutné tooadd Igloo softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="227bd-124">tooconfigure hello integration of Igloo Software into Azure AD, you need tooadd Igloo Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="227bd-125">**tooadd Igloo softwaru z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="227bd-125">**tooadd Igloo Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="227bd-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="227bd-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="227bd-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="227bd-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="227bd-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="227bd-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="227bd-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="227bd-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="227bd-133">Hello vyhledávacího pole zadejte **Igloo softwaru**.</span><span class="sxs-lookup"><span data-stu-id="227bd-133">In hello search box, type **Igloo Software**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. <span data-ttu-id="227bd-135">Na panelu výsledků hello vyberte **Igloo softwaru**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="227bd-135">In hello results panel, select **Igloo Software**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="227bd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="227bd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="227bd-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Igloo Software založený na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="227bd-138">In this section, you configure and test Azure AD single sign-on with Igloo Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="227bd-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Igloo softwaru je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="227bd-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Igloo Software is tooa user in Azure AD.</span></span> <span data-ttu-id="227bd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v softwaru Igloo musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="227bd-140">In other words, a link relationship between an Azure AD user and hello related user in Igloo Software needs toobe established.</span></span>

<span data-ttu-id="227bd-141">V softwaru Igloo přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="227bd-141">In Igloo Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="227bd-142">tooconfigure a testu Azure AD jednotné přihlašování s Igloo softwaru, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="227bd-142">tooconfigure and test Azure AD single sign-on with Igloo Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="227bd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="227bd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="227bd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="227bd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="227bd-145">**[Vytváření testovacího uživatele softwaru Igloo](#creating-an-igloo-software-test-user)**  -toohave protějšek Britta Simon v Igloo Software, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="227bd-145">**[Creating an Igloo Software test user](#creating-an-igloo-software-test-user)** - toohave a counterpart of Britta Simon in Igloo Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="227bd-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="227bd-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="227bd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="227bd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="227bd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="227bd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="227bd-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Igloo softwaru.</span><span class="sxs-lookup"><span data-stu-id="227bd-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Igloo Software application.</span></span>

<span data-ttu-id="227bd-150">**tooconfigure Azure AD jednotné přihlašování s Igloo softwarem, provést hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="227bd-150">**tooconfigure Azure AD single sign-on with Igloo Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="227bd-151">V portálu Azure, na hello hello **Igloo softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="227bd-151">In hello Azure portal, on hello **Igloo Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="227bd-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="227bd-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. <span data-ttu-id="227bd-155">Na hello **Igloo softwaru domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="227bd-155">On hello **Igloo Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    <span data-ttu-id="227bd-157">a.</span><span class="sxs-lookup"><span data-stu-id="227bd-157">a.</span></span> <span data-ttu-id="227bd-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.igloocommmunities.com`</span><span class="sxs-lookup"><span data-stu-id="227bd-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.igloocommmunities.com`</span></span>

    <span data-ttu-id="227bd-159">b.</span><span class="sxs-lookup"><span data-stu-id="227bd-159">b.</span></span> <span data-ttu-id="227bd-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="227bd-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    <span data-ttu-id="227bd-161">c.</span><span class="sxs-lookup"><span data-stu-id="227bd-161">c.</span></span> <span data-ttu-id="227bd-162">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="227bd-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="227bd-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="227bd-163">These values are not real.</span></span> <span data-ttu-id="227bd-164">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="227bd-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="227bd-165">Obraťte se na [tým podpory klientský Software Igloo](https://www.igloosoftware.com/services/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="227bd-165">Contact [Igloo Software Client support team](https://www.igloosoftware.com/services/support) tooget these values.</span></span> 

4. <span data-ttu-id="227bd-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="227bd-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. <span data-ttu-id="227bd-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="227bd-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="227bd-170">Na hello **Igloo softwarové konfigurace** klikněte na tlačítko **konfigurace softwaru Igloo** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="227bd-170">On hello **Igloo Software Configuration** section, click **Configure Igloo Software** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="227bd-171">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="227bd-171">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. <span data-ttu-id="227bd-173">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour Igloo softwaru.</span><span class="sxs-lookup"><span data-stu-id="227bd-173">In a different web browser window, log in tooyour Igloo Software company site as an administrator.</span></span>

8. <span data-ttu-id="227bd-174">Přejděte toohello **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="227bd-174">Go toohello **Control Panel**.</span></span>
   
     <span data-ttu-id="227bd-175">![Ovládací panely](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "ovládací panely")</span><span class="sxs-lookup"><span data-stu-id="227bd-175">![Control Panel](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Control Panel")</span></span>

9. <span data-ttu-id="227bd-176">V hello **členství** , klikněte na **přihlášení v nastavení**.</span><span class="sxs-lookup"><span data-stu-id="227bd-176">In hello **Membership** tab, click **Sign In Settings**.</span></span>
   
    <span data-ttu-id="227bd-177">![Přihlaste se nastavení](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "přihlášení nastavení")</span><span class="sxs-lookup"><span data-stu-id="227bd-177">![Sign in Settings](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "Sign in Settings")</span></span>

10. <span data-ttu-id="227bd-178">V hello SAML konfigurační oddíl, klikněte na **konfigurace ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="227bd-178">In hello SAML Configuration section, click **Configure SAML Authentication**.</span></span>
   
    <span data-ttu-id="227bd-179">![Konfigurace SAML](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="227bd-179">![SAML Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML Configuration")</span></span>
   
11. <span data-ttu-id="227bd-180">V hello **obecné konfigurace** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="227bd-180">In hello **General Configuration** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="227bd-181">![Obecná konfigurace](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "obecné konfigurace")</span><span class="sxs-lookup"><span data-stu-id="227bd-181">![General Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "General Configuration")</span></span>

    <span data-ttu-id="227bd-182">a.</span><span class="sxs-lookup"><span data-stu-id="227bd-182">a.</span></span> <span data-ttu-id="227bd-183">V hello **název připojení** textovému poli, napište vlastní název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="227bd-183">In hello **Connection Name** textbox, type a custom name for your configuration.</span></span>
   
    <span data-ttu-id="227bd-184">b.</span><span class="sxs-lookup"><span data-stu-id="227bd-184">b.</span></span> <span data-ttu-id="227bd-185">V hello **IdP přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="227bd-185">In hello **IdP Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="227bd-186">c.</span><span class="sxs-lookup"><span data-stu-id="227bd-186">c.</span></span> <span data-ttu-id="227bd-187">V hello **adresy URL odhlašovací IdP** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="227bd-187">In hello **IdP Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="227bd-188">d.</span><span class="sxs-lookup"><span data-stu-id="227bd-188">d.</span></span> <span data-ttu-id="227bd-189">Vyberte **odhlášení odpovědi a typ požadavku HTTP** jako **POST**.</span><span class="sxs-lookup"><span data-stu-id="227bd-189">Select **Logout Response and Request HTTP Type** as **POST**.</span></span>
   
    <span data-ttu-id="227bd-190">e.</span><span class="sxs-lookup"><span data-stu-id="227bd-190">e.</span></span> <span data-ttu-id="227bd-191">Otevřete váš **kódování base-64** kódování certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **veřejný certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="227bd-191">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **Public Certificate** textbox.</span></span>
    
12. <span data-ttu-id="227bd-192">V hello **odpovědi a konfiguraci ověřování**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="227bd-192">In hello **Response and Authentication Configuration**, perform hello following steps:</span></span>
    
    <span data-ttu-id="227bd-193">![Odpověď a konfiguraci ověřování](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "odpovědi a konfiguraci ověřování")</span><span class="sxs-lookup"><span data-stu-id="227bd-193">![Response and Authentication Configuration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Response and Authentication Configuration")</span></span>
  
      <span data-ttu-id="227bd-194">a.</span><span class="sxs-lookup"><span data-stu-id="227bd-194">a.</span></span> <span data-ttu-id="227bd-195">Jako **zprostředkovatele Identity**, vyberte **Microsoft ADFS**.</span><span class="sxs-lookup"><span data-stu-id="227bd-195">As **Identity Provider**, select **Microsoft ADFS**.</span></span>
      
      <span data-ttu-id="227bd-196">b.</span><span class="sxs-lookup"><span data-stu-id="227bd-196">b.</span></span> <span data-ttu-id="227bd-197">Jako **typ identifikátoru**, vyberte **e-mailovou adresu**.</span><span class="sxs-lookup"><span data-stu-id="227bd-197">As **Identifier Type**, select **Email Address**.</span></span> 

      <span data-ttu-id="227bd-198">c.</span><span class="sxs-lookup"><span data-stu-id="227bd-198">c.</span></span> <span data-ttu-id="227bd-199">V hello **atribut e-mailu** textovému poli, typ **emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="227bd-199">In hello **Email Attribute** textbox, type **emailaddress**.</span></span>

      <span data-ttu-id="227bd-200">d.</span><span class="sxs-lookup"><span data-stu-id="227bd-200">d.</span></span> <span data-ttu-id="227bd-201">V hello **křestní jméno atribut** textovému poli, typ **givenname**.</span><span class="sxs-lookup"><span data-stu-id="227bd-201">In hello **First Name Attribute** textbox, type **givenname**.</span></span>

      <span data-ttu-id="227bd-202">e.</span><span class="sxs-lookup"><span data-stu-id="227bd-202">e.</span></span> <span data-ttu-id="227bd-203">V hello **poslední atribut Name** textovému poli, typ **Přezdívka**.</span><span class="sxs-lookup"><span data-stu-id="227bd-203">In hello **Last Name Attribute** textbox, type **surname**.</span></span>

13. <span data-ttu-id="227bd-204">Proveďte následující kroky toocomplete hello konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="227bd-204">Perform hello following steps toocomplete hello configuration:</span></span>
    
    <span data-ttu-id="227bd-205">![Vytvoření uživatele na přihlašovací](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "vytvoření uživatele na přihlášení")</span><span class="sxs-lookup"><span data-stu-id="227bd-205">![User creation on Sign in](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "User creation on Sign in")</span></span> 

     <span data-ttu-id="227bd-206">a.</span><span class="sxs-lookup"><span data-stu-id="227bd-206">a.</span></span> <span data-ttu-id="227bd-207">Jako **vytvoření uživatele na přihlašovací**, vyberte **vytvořte nového uživatele ve vaší lokalitě při přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="227bd-207">As **User creation on Sign in**, select **Create a new user in your site when they sign in**.</span></span>

     <span data-ttu-id="227bd-208">b.</span><span class="sxs-lookup"><span data-stu-id="227bd-208">b.</span></span> <span data-ttu-id="227bd-209">Jako **přihlášení nastavení**, vyberte **SAML použijte tlačítko na obrazovce "Přihlásit"**.</span><span class="sxs-lookup"><span data-stu-id="227bd-209">As **Sign in Settings**, select **Use SAML button on “Sign in” screen**.</span></span>

     <span data-ttu-id="227bd-210">c.</span><span class="sxs-lookup"><span data-stu-id="227bd-210">c.</span></span> <span data-ttu-id="227bd-211">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="227bd-211">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="227bd-212">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="227bd-212">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="227bd-213">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="227bd-213">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="227bd-214">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="227bd-214">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="227bd-215">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="227bd-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="227bd-216">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="227bd-216">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="227bd-218">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="227bd-218">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="227bd-219">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="227bd-219">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="227bd-221">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="227bd-221">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="227bd-223">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="227bd-223">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="227bd-225">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="227bd-225">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="227bd-227">a.</span><span class="sxs-lookup"><span data-stu-id="227bd-227">a.</span></span> <span data-ttu-id="227bd-228">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="227bd-228">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="227bd-229">b.</span><span class="sxs-lookup"><span data-stu-id="227bd-229">b.</span></span> <span data-ttu-id="227bd-230">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="227bd-230">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="227bd-231">c.</span><span class="sxs-lookup"><span data-stu-id="227bd-231">c.</span></span> <span data-ttu-id="227bd-232">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="227bd-232">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="227bd-233">d.</span><span class="sxs-lookup"><span data-stu-id="227bd-233">d.</span></span> <span data-ttu-id="227bd-234">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="227bd-234">Click **Create**.</span></span>
 
### <a name="creating-an-igloo-software-test-user"></a><span data-ttu-id="227bd-235">Vytváření testovacího uživatele Igloo softwaru</span><span class="sxs-lookup"><span data-stu-id="227bd-235">Creating an Igloo Software test user</span></span>

<span data-ttu-id="227bd-236">Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooIgloo softwaru.</span><span class="sxs-lookup"><span data-stu-id="227bd-236">There is no action item for you tooconfigure user provisioning tooIgloo Software.</span></span>  

<span data-ttu-id="227bd-237">Když se uživatel s přiřazenou pokusí toolog v tooIgloo Software pomocí hello přístupový panel Igloo softwaru kontroly, zda text hello uživatele existuje.</span><span class="sxs-lookup"><span data-stu-id="227bd-237">When an assigned user tries toolog in tooIgloo Software using hello access panel, Igloo Software checks whether hello user exists.</span></span>  <span data-ttu-id="227bd-238">Pokud neexistuje žádný účet k dispozici dosud, se automaticky vytvoří Igloo softwarem.</span><span class="sxs-lookup"><span data-stu-id="227bd-238">If there is no user account available yet, it is automatically created by Igloo Software.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="227bd-239">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="227bd-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="227bd-240">V této části povolíte tak, že udělíte přístup tooIgloo softwaru Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="227bd-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIgloo Software.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="227bd-242">**tooassign Britta Simon tooIgloo softwaru, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="227bd-242">**tooassign Britta Simon tooIgloo Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="227bd-243">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="227bd-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="227bd-245">V seznamu aplikace hello vyberte **Igloo softwaru**.</span><span class="sxs-lookup"><span data-stu-id="227bd-245">In hello applications list, select **Igloo Software**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. <span data-ttu-id="227bd-247">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="227bd-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="227bd-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="227bd-249">Click **Add** button.</span></span> <span data-ttu-id="227bd-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="227bd-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="227bd-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="227bd-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="227bd-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="227bd-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="227bd-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="227bd-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="227bd-255">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="227bd-255">Testing single sign-on</span></span>

<span data-ttu-id="227bd-256">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="227bd-256">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="227bd-257">Když kliknete na dlaždici Igloo softwaru hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Igloo softwarová aplikace.</span><span class="sxs-lookup"><span data-stu-id="227bd-257">When you click hello Igloo Software tile in hello Access Panel, you should get automatically signed-on tooyour Igloo Software application.</span></span>
<span data-ttu-id="227bd-258">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="227bd-258">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="227bd-259">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="227bd-259">Additional resources</span></span>

* [<span data-ttu-id="227bd-260">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="227bd-260">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="227bd-261">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="227bd-261">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

