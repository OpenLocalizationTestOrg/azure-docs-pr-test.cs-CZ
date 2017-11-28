---
title: 'Kurz: Azure Active Directory integrace s QuickHelp | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a QuickHelp."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: bbde5eb9bdad89680923ccd36c321b6923f91789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a><span data-ttu-id="713f1-103">Kurz: Azure Active Directory integrace s QuickHelp</span><span class="sxs-lookup"><span data-stu-id="713f1-103">Tutorial: Azure Active Directory integration with QuickHelp</span></span>

<span data-ttu-id="713f1-104">V tomto kurzu zjistíte, jak toointegrate QuickHelp s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="713f1-104">In this tutorial, you learn how toointegrate QuickHelp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="713f1-105">Integrace QuickHelp s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="713f1-105">Integrating QuickHelp with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="713f1-106">Můžete řídit ve službě Azure AD, který má přístup tooQuickHelp</span><span class="sxs-lookup"><span data-stu-id="713f1-106">You can control in Azure AD who has access tooQuickHelp</span></span>
- <span data-ttu-id="713f1-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooQuickHelp (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="713f1-107">You can enable your users tooautomatically get signed-on tooQuickHelp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="713f1-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="713f1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="713f1-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="713f1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="713f1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="713f1-110">Prerequisites</span></span>

<span data-ttu-id="713f1-111">Integrace služby Azure AD s QuickHelp tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="713f1-111">tooconfigure Azure AD integration with QuickHelp, you need hello following items:</span></span>

- <span data-ttu-id="713f1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="713f1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="713f1-113">QuickHelp jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="713f1-113">A QuickHelp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="713f1-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="713f1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="713f1-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="713f1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="713f1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="713f1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="713f1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="713f1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="713f1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="713f1-118">Scenario description</span></span>
<span data-ttu-id="713f1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="713f1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="713f1-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="713f1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="713f1-121">Přidání QuickHelp z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="713f1-121">Adding QuickHelp from hello gallery</span></span>
2. <span data-ttu-id="713f1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="713f1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-quickhelp-from-hello-gallery"></a><span data-ttu-id="713f1-123">Přidání QuickHelp z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="713f1-123">Adding QuickHelp from hello gallery</span></span>
<span data-ttu-id="713f1-124">tooconfigure hello integrace QuickHelp do Azure AD, je nutné tooadd QuickHelp hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="713f1-124">tooconfigure hello integration of QuickHelp into Azure AD, you need tooadd QuickHelp from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="713f1-125">**tooadd QuickHelp z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="713f1-125">**tooadd QuickHelp from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="713f1-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="713f1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="713f1-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="713f1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="713f1-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="713f1-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="713f1-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="713f1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="713f1-133">Hello vyhledávacího pole zadejte **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="713f1-133">In hello search box, type **QuickHelp**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_search.png)

5. <span data-ttu-id="713f1-135">Na panelu výsledků hello vyberte **QuickHelp**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="713f1-135">In hello results panel, select **QuickHelp**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="713f1-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="713f1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="713f1-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s QuickHelp podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="713f1-138">In this section, you configure and test Azure AD single sign-on with QuickHelp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="713f1-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v QuickHelp je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="713f1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in QuickHelp is tooa user in Azure AD.</span></span> <span data-ttu-id="713f1-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v QuickHelp musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="713f1-140">In other words, a link relationship between an Azure AD user and hello related user in QuickHelp needs toobe established.</span></span>

<span data-ttu-id="713f1-141">V QuickHelp, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="713f1-141">In QuickHelp, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="713f1-142">tooconfigure a testu Azure AD jednotné přihlašování s QuickHelp, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="713f1-142">tooconfigure and test Azure AD single sign-on with QuickHelp, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="713f1-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="713f1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="713f1-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="713f1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="713f1-145">**[Vytvoření zkušebního uživatele QuickHelp](#creating-a-quickhelp-test-user)**  -toohave protějšek Britta Simon v QuickHelp, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="713f1-145">**[Creating a QuickHelp test user](#creating-a-quickhelp-test-user)** - toohave a counterpart of Britta Simon in QuickHelp that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="713f1-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="713f1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="713f1-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="713f1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="713f1-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="713f1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="713f1-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci QuickHelp.</span><span class="sxs-lookup"><span data-stu-id="713f1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your QuickHelp application.</span></span>

<span data-ttu-id="713f1-150">**tooconfigure Azure AD jednotné přihlašování s QuickHelp, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="713f1-150">**tooconfigure Azure AD single sign-on with QuickHelp, perform hello following steps:**</span></span>

1. <span data-ttu-id="713f1-151">V portálu Azure, na hello hello **QuickHelp** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="713f1-151">In hello Azure portal, on hello **QuickHelp** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="713f1-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="713f1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

3. <span data-ttu-id="713f1-155">Na hello **QuickHelp domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="713f1-155">On hello **QuickHelp Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_url.png)

    <span data-ttu-id="713f1-157">a.</span><span class="sxs-lookup"><span data-stu-id="713f1-157">a.</span></span> <span data-ttu-id="713f1-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://quickhelp.com/<instancename>/#/Login`</span><span class="sxs-lookup"><span data-stu-id="713f1-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://quickhelp.com/<instancename>/#/Login`</span></span>

    <span data-ttu-id="713f1-159">b.</span><span class="sxs-lookup"><span data-stu-id="713f1-159">b.</span></span> <span data-ttu-id="713f1-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.quickhelp.com`</span><span class="sxs-lookup"><span data-stu-id="713f1-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.quickhelp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="713f1-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="713f1-161">These values are not real.</span></span> <span data-ttu-id="713f1-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="713f1-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="713f1-163">Obraťte se na [tým podpory QuickHelp klienta](https://support.quickhelp.com/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="713f1-163">Contact [QuickHelp Client support team](https://support.quickhelp.com/) tooget these values.</span></span> 
 
4. <span data-ttu-id="713f1-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="713f1-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

5. <span data-ttu-id="713f1-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="713f1-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="713f1-168">Web společnosti QuickHelp tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="713f1-168">Sign-on tooyour QuickHelp company site as administrator.</span></span>

7. <span data-ttu-id="713f1-169">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="713f1-169">In hello menu on hello top, click **Admin**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][21]

8. <span data-ttu-id="713f1-171">V hello **QuickHelp správce** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="713f1-171">In hello **QuickHelp Admin** menu, click **Settings**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][22]

9. <span data-ttu-id="713f1-173">Klikněte na tlačítko **nastavení ověřování**.</span><span class="sxs-lookup"><span data-stu-id="713f1-173">Click **Authentication Settings**.</span></span>

10. <span data-ttu-id="713f1-174">Na hello **nastavení ověřování** proveďte následující kroky hello</span><span class="sxs-lookup"><span data-stu-id="713f1-174">On hello **Authentication Settings** page, perform hello following steps</span></span>
   
    ![Konfigurovat jednotné přihlašování][23]
   
    <span data-ttu-id="713f1-176">a.</span><span class="sxs-lookup"><span data-stu-id="713f1-176">a.</span></span> <span data-ttu-id="713f1-177">Jako **typ jednotného přihlašování k**, vyberte **WSFederation**.</span><span class="sxs-lookup"><span data-stu-id="713f1-177">As **SSO Type**, select **WSFederation**.</span></span>
   
    <span data-ttu-id="713f1-178">b.</span><span class="sxs-lookup"><span data-stu-id="713f1-178">b.</span></span> <span data-ttu-id="713f1-179">tooupload váš soubor stažený Azure metadata, klikněte na tlačítko **Procházet**přejděte toohello soubor, pak klikněte na tlačítko Ukončit **nahrát Metadata**.</span><span class="sxs-lookup"><span data-stu-id="713f1-179">tooupload your downloaded Azure metadata file, click **Browse**, navigate toohello file, end then click **Upload Metadata**.</span></span>
   
    <span data-ttu-id="713f1-180">c.</span><span class="sxs-lookup"><span data-stu-id="713f1-180">c.</span></span> <span data-ttu-id="713f1-181">V hello **e-mailu** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="713f1-181">In hello **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
   
    <span data-ttu-id="713f1-182">d.</span><span class="sxs-lookup"><span data-stu-id="713f1-182">d.</span></span> <span data-ttu-id="713f1-183">V hello **křestní jméno** textovému poli, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="713f1-183">In hello **First Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
   
    <span data-ttu-id="713f1-184">e.</span><span class="sxs-lookup"><span data-stu-id="713f1-184">e.</span></span> <span data-ttu-id="713f1-185">V hello **příjmení** textovému poli, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="713f1-185">In hello **Last Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
   
    <span data-ttu-id="713f1-186">f.</span><span class="sxs-lookup"><span data-stu-id="713f1-186">f.</span></span> <span data-ttu-id="713f1-187">V hello **panelu akcí**, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="713f1-187">In hello **Action Bar**, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="713f1-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="713f1-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="713f1-189">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="713f1-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="713f1-190">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="713f1-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="713f1-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="713f1-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="713f1-192">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="713f1-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="713f1-194">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="713f1-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="713f1-195">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="713f1-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="713f1-197">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="713f1-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="713f1-199">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="713f1-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="713f1-201">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="713f1-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="713f1-203">a.</span><span class="sxs-lookup"><span data-stu-id="713f1-203">a.</span></span> <span data-ttu-id="713f1-204">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="713f1-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="713f1-205">b.</span><span class="sxs-lookup"><span data-stu-id="713f1-205">b.</span></span> <span data-ttu-id="713f1-206">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="713f1-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="713f1-207">c.</span><span class="sxs-lookup"><span data-stu-id="713f1-207">c.</span></span> <span data-ttu-id="713f1-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="713f1-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="713f1-209">d.</span><span class="sxs-lookup"><span data-stu-id="713f1-209">d.</span></span> <span data-ttu-id="713f1-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="713f1-210">Click **Create**.</span></span>
 
### <a name="creating-a-quickhelp-test-user"></a><span data-ttu-id="713f1-211">Vytvoření zkušebního uživatele QuickHelp</span><span class="sxs-lookup"><span data-stu-id="713f1-211">Creating a QuickHelp test user</span></span>

<span data-ttu-id="713f1-212">Hello cílem této části je toocreate volal Britta Simon v QuickHelp uživatele.</span><span class="sxs-lookup"><span data-stu-id="713f1-212">hello objective of this section is toocreate a user called Britta Simon in QuickHelp.</span></span>
<span data-ttu-id="713f1-213">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel hello protějškem v QuickHelp tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="713f1-213">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in QuickHelp tooa user in Azure AD is.</span></span> <span data-ttu-id="713f1-214">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v QuickHelp musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="713f1-214">In other words, a link relationship between an Azure AD user and hello related user in QuickHelp needs toobe established.</span></span>

<span data-ttu-id="713f1-215">QuickHelp podporuje zřizování za běhu.</span><span class="sxs-lookup"><span data-stu-id="713f1-215">QuickHelp supports just-in-time provisioning.</span></span> <span data-ttu-id="713f1-216">To znamená, v případě potřeby v QuickHelp je automaticky vytvořený uživatelský účet a účet hello je propojené toohello účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="713f1-216">This means, if necessary, a user account is automatically created in QuickHelp and hello account is linked toohello Azure AD account.</span></span>

<span data-ttu-id="713f1-217">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="713f1-217">There is no action item for you in this section.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="713f1-218">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="713f1-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="713f1-219">V této části povolíte tak, že udělíte přístup tooQuickHelp toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="713f1-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooQuickHelp.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="713f1-221">**tooassign Britta Simon tooQuickHelp, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="713f1-221">**tooassign Britta Simon tooQuickHelp, perform hello following steps:**</span></span>

1. <span data-ttu-id="713f1-222">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="713f1-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="713f1-224">V seznamu aplikace hello vyberte **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="713f1-224">In hello applications list, select **QuickHelp**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_app.png) 

3. <span data-ttu-id="713f1-226">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="713f1-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="713f1-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="713f1-228">Click **Add** button.</span></span> <span data-ttu-id="713f1-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="713f1-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="713f1-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="713f1-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="713f1-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="713f1-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="713f1-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="713f1-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="713f1-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="713f1-234">Testing single sign-on</span></span>

<span data-ttu-id="713f1-235">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="713f1-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="713f1-236">Když kliknete na dlaždici QuickHelp hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour QuickHelp aplikace.</span><span class="sxs-lookup"><span data-stu-id="713f1-236">When you click hello QuickHelp tile in hello Access Panel, you should get automatically signed-on tooyour QuickHelp application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="713f1-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="713f1-237">Additional resources</span></span>

* [<span data-ttu-id="713f1-238">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="713f1-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="713f1-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="713f1-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
