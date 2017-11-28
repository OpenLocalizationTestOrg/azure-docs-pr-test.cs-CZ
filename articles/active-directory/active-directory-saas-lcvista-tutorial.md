---
title: 'Kurz: Azure Active Directory integrace s LCVista | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LCVista."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8db80d6e-3275-419f-aa39-6115a7bc9800
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: 5a4c7eb7d54b8b68c8241a97b9e516a3f6e55c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lcvista"></a><span data-ttu-id="1fd94-103">Kurz: Azure Active Directory integrace s LCVista</span><span class="sxs-lookup"><span data-stu-id="1fd94-103">Tutorial: Azure Active Directory integration with LCVista</span></span>

<span data-ttu-id="1fd94-104">V tomto kurzu zjistíte, jak toointegrate LCVista s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1fd94-104">In this tutorial, you learn how toointegrate LCVista with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1fd94-105">Integrace LCVista s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1fd94-105">Integrating LCVista with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1fd94-106">Můžete řídit ve službě Azure AD, který má přístup tooLCVista</span><span class="sxs-lookup"><span data-stu-id="1fd94-106">You can control in Azure AD who has access tooLCVista</span></span>
- <span data-ttu-id="1fd94-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLCVista (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="1fd94-107">You can enable your users tooautomatically get signed-on tooLCVista (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1fd94-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1fd94-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1fd94-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1fd94-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1fd94-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1fd94-110">Prerequisites</span></span>

<span data-ttu-id="1fd94-111">Integrace služby Azure AD s LCVista tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="1fd94-111">tooconfigure Azure AD integration with LCVista, you need hello following items:</span></span>

- <span data-ttu-id="1fd94-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1fd94-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1fd94-113">LCVista jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1fd94-113">A LCVista single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1fd94-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1fd94-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1fd94-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1fd94-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1fd94-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="1fd94-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1fd94-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1fd94-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1fd94-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1fd94-118">Scenario description</span></span>
<span data-ttu-id="1fd94-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1fd94-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1fd94-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1fd94-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1fd94-121">Přidání LCVista z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1fd94-121">Adding LCVista from hello gallery</span></span>
2. <span data-ttu-id="1fd94-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1fd94-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lcvista-from-hello-gallery"></a><span data-ttu-id="1fd94-123">Přidání LCVista z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1fd94-123">Adding LCVista from hello gallery</span></span>
<span data-ttu-id="1fd94-124">tooconfigure hello integrace LCVista do Azure AD, je nutné tooadd LCVista hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1fd94-124">tooconfigure hello integration of LCVista into Azure AD, you need tooadd LCVista from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1fd94-125">**tooadd LCVista z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1fd94-125">**tooadd LCVista from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1fd94-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1fd94-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1fd94-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1fd94-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="1fd94-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1fd94-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="1fd94-133">Hello vyhledávacího pole zadejte **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-133">In hello search box, type **LCVista**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_search.png)

5. <span data-ttu-id="1fd94-135">Na panelu výsledků hello vyberte **LCVista**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1fd94-135">In hello results panel, select **LCVista**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1fd94-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1fd94-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1fd94-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s LCVista podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="1fd94-138">In this section, you configure and test Azure AD single sign-on with LCVista based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1fd94-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v LCVista je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1fd94-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LCVista is tooa user in Azure AD.</span></span> <span data-ttu-id="1fd94-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LCVista musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="1fd94-140">In other words, a link relationship between an Azure AD user and hello related user in LCVista needs toobe established.</span></span>

<span data-ttu-id="1fd94-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v LCVista.</span><span class="sxs-lookup"><span data-stu-id="1fd94-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LCVista.</span></span>

<span data-ttu-id="1fd94-142">tooconfigure a testu Azure AD jednotné přihlašování s LCVista, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="1fd94-142">tooconfigure and test Azure AD single sign-on with LCVista, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1fd94-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1fd94-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1fd94-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1fd94-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1fd94-145">**[Vytvoření zkušebního uživatele LCVista](#creating-a-lcvista-test-user)**  -toohave protějšek Britta Simon v LCVista, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="1fd94-145">**[Creating a LCVista test user](#creating-a-lcvista-test-user)** - toohave a counterpart of Britta Simon in LCVista that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1fd94-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1fd94-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1fd94-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="1fd94-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1fd94-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1fd94-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1fd94-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LCVista.</span><span class="sxs-lookup"><span data-stu-id="1fd94-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LCVista application.</span></span>

<span data-ttu-id="1fd94-150">**tooconfigure Azure AD jednotné přihlašování s LCVista, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1fd94-150">**tooconfigure Azure AD single sign-on with LCVista, perform hello following steps:**</span></span>

1. <span data-ttu-id="1fd94-151">V portálu Azure, na hello hello **LCVista** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-151">In hello Azure portal, on hello **LCVista** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="1fd94-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1fd94-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_samlbase.png)

3. <span data-ttu-id="1fd94-155">Na hello **LCVista domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1fd94-155">On hello **LCVista Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_url.png)

    <span data-ttu-id="1fd94-157">a.</span><span class="sxs-lookup"><span data-stu-id="1fd94-157">a.</span></span> <span data-ttu-id="1fd94-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.lcvista.com/rainier/login`</span><span class="sxs-lookup"><span data-stu-id="1fd94-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.lcvista.com/rainier/login`</span></span>

    <span data-ttu-id="1fd94-159">b.</span><span class="sxs-lookup"><span data-stu-id="1fd94-159">b.</span></span> <span data-ttu-id="1fd94-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.lcvista.com`</span><span class="sxs-lookup"><span data-stu-id="1fd94-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.lcvista.com`</span></span> 
     
    > [!NOTE] 
    > <span data-ttu-id="1fd94-161">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="1fd94-161">These values are not hello real.</span></span> <span data-ttu-id="1fd94-162">Aktualizovat tyto hodnoty s hello skutečné identifikátor a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="1fd94-162">Update these values with hello actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="1fd94-163">Obraťte se na [tým podpory LCVista klienta](https://lcvista.com/contact) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1fd94-163">Contact [LCVista Client support team](https://lcvista.com/contact) tooget these values.</span></span> 

4. <span data-ttu-id="1fd94-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1fd94-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_certificate.png) 

5. <span data-ttu-id="1fd94-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1fd94-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="1fd94-168">Na hello **LCVista konfigurace** klikněte na tlačítko **konfigurace LCVista** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="1fd94-168">On hello **LCVista Configuration** section, click **Configure LCVista** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1fd94-169">Kopírování hello **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="1fd94-169">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_configure.png) 

7.  <span data-ttu-id="1fd94-171">Přihlášení tooyour LCVista aplikace jako správce.</span><span class="sxs-lookup"><span data-stu-id="1fd94-171">Sign on tooyour LCVista application as an administrator.</span></span>

8. <span data-ttu-id="1fd94-172">V hello **konfigurace SAML** část, zkontrolujte hello **povolit SAML přihlášení** a zadejte podrobnosti hello, jak je uvedeno v následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="1fd94-172">In hello **SAML Config** section, check hello **Enable SAML login** and enter hello details as mentioned in below image.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_config.png)

    <span data-ttu-id="1fd94-174">a.</span><span class="sxs-lookup"><span data-stu-id="1fd94-174">a.</span></span> <span data-ttu-id="1fd94-175">Vložení hello **URL vystavitele** který jste zkopírovali z Azure AD v hello **Entity ID** části.</span><span class="sxs-lookup"><span data-stu-id="1fd94-175">Paste hello **Issuer URL** which you have copied from Azure AD in hello **Entity ID** section.</span></span> 

    <span data-ttu-id="1fd94-176">b.</span><span class="sxs-lookup"><span data-stu-id="1fd94-176">b.</span></span> <span data-ttu-id="1fd94-177">Vložení hello **jeden přihlašování adresa URL služby** který jste zkopírovali z Azure AD v hello **URL** části.</span><span class="sxs-lookup"><span data-stu-id="1fd94-177">Paste hello **Single Sign-On Service URL** which you have copied from Azure AD in hello **URL** section.</span></span>

    <span data-ttu-id="1fd94-178">c.</span><span class="sxs-lookup"><span data-stu-id="1fd94-178">c.</span></span> <span data-ttu-id="1fd94-179">Z metadat (XML), který jste si stáhli z portálu Azure, zkopírujte hodnotu hello **certifikátu x 509** a vložte ji do hello **x509 certifikátu** části.</span><span class="sxs-lookup"><span data-stu-id="1fd94-179">From Metadata (XML) which you have downloaded from Azure portal, copy hello value **X509Certificate** and paste it in hello **x509 Certificate** section.</span></span>

    <span data-ttu-id="1fd94-180">d.</span><span class="sxs-lookup"><span data-stu-id="1fd94-180">d.</span></span> <span data-ttu-id="1fd94-181">V hello **křestní jméno atribut** textovému poli, vložte hodnotu hello `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="1fd94-181">In hello **First name attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>

    <span data-ttu-id="1fd94-182">e.</span><span class="sxs-lookup"><span data-stu-id="1fd94-182">e.</span></span> <span data-ttu-id="1fd94-183">V hello **poslední atribut name** textovému poli, vložte hodnotu hello `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="1fd94-183">In hello **Last name attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>

    <span data-ttu-id="1fd94-184">f.</span><span class="sxs-lookup"><span data-stu-id="1fd94-184">f.</span></span> <span data-ttu-id="1fd94-185">V hello **atribut e-mailu** textovému poli, vložte hodnotu hello `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="1fd94-185">In hello **Email attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="1fd94-186">g.</span><span class="sxs-lookup"><span data-stu-id="1fd94-186">g.</span></span> <span data-ttu-id="1fd94-187">V hello **uživatelské jméno atribut** textovému poli, vložte hodnotu hello `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="1fd94-187">In hello **Username attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>

    <span data-ttu-id="1fd94-188">e.</span><span class="sxs-lookup"><span data-stu-id="1fd94-188">e.</span></span> <span data-ttu-id="1fd94-189">Klikněte na tlačítko **Uložit** toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="1fd94-189">Click **Save** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="1fd94-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="1fd94-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1fd94-191">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="1fd94-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1fd94-192">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1fd94-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1fd94-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1fd94-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="1fd94-194">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="1fd94-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="1fd94-196">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1fd94-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1fd94-197">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1fd94-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1fd94-199">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1fd94-201">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="1fd94-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1fd94-203">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1fd94-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1fd94-205">a.</span><span class="sxs-lookup"><span data-stu-id="1fd94-205">a.</span></span> <span data-ttu-id="1fd94-206">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1fd94-207">b.</span><span class="sxs-lookup"><span data-stu-id="1fd94-207">b.</span></span> <span data-ttu-id="1fd94-208">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1fd94-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1fd94-209">c.</span><span class="sxs-lookup"><span data-stu-id="1fd94-209">c.</span></span> <span data-ttu-id="1fd94-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1fd94-211">d.</span><span class="sxs-lookup"><span data-stu-id="1fd94-211">d.</span></span> <span data-ttu-id="1fd94-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-212">Click **Create**.</span></span>
 
### <a name="creating-a-lcvista-test-user"></a><span data-ttu-id="1fd94-213">Vytvoření zkušebního uživatele LCVista</span><span class="sxs-lookup"><span data-stu-id="1fd94-213">Creating a LCVista test user</span></span>

<span data-ttu-id="1fd94-214">V této části vytvoříte volal Britta Simon v LCVista uživatele.</span><span class="sxs-lookup"><span data-stu-id="1fd94-214">In this section, you create a user called Britta Simon in LCVista.</span></span> <span data-ttu-id="1fd94-215">Je třeba toocontact [tým podpory LCVista klienta](https://lcvista.com/contact) tooadd hello uživatele v hello LCVista aplikace.</span><span class="sxs-lookup"><span data-stu-id="1fd94-215">You need toocontact [LCVista Client support team](https://lcvista.com/contact) tooadd hello users in hello LCVista application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1fd94-216">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="1fd94-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1fd94-217">V této části povolíte tak, že udělíte přístup tooLCVista toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1fd94-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLCVista.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="1fd94-219">**tooassign Britta Simon tooLCVista, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1fd94-219">**tooassign Britta Simon tooLCVista, perform hello following steps:**</span></span>

1. <span data-ttu-id="1fd94-220">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1fd94-222">V seznamu aplikace hello vyberte **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-222">In hello applications list, select **LCVista**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_app.png) 

3. <span data-ttu-id="1fd94-224">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1fd94-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="1fd94-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1fd94-226">Click **Add** button.</span></span> <span data-ttu-id="1fd94-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1fd94-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="1fd94-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="1fd94-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1fd94-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1fd94-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1fd94-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1fd94-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1fd94-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1fd94-232">Testing single sign-on</span></span>

<span data-ttu-id="1fd94-233">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1fd94-233">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> <span data-ttu-id="1fd94-234">Klikněte na dlaždici LCVista hello v hello přístupového panelu, bude přesměrován tooOrganization přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="1fd94-234">Click hello LCVista tile in hello Access Panel, you will be redirected tooOrganization sign on page.</span></span> <span data-ttu-id="1fd94-235">Po úspěšném přihlášení bude přihlášeného tooyour LCVista aplikace.</span><span class="sxs-lookup"><span data-stu-id="1fd94-235">After successful login, you will be signed-on tooyour LCVista application.</span></span> <span data-ttu-id="1fd94-236">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="1fd94-236">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1fd94-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1fd94-237">Additional resources</span></span>

* [<span data-ttu-id="1fd94-238">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1fd94-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1fd94-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1fd94-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_203.png

