---
title: 'Kurz: Azure Active Directory integrace s AppDynamics | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a AppDynamics."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 25fd1df0-411c-4f55-8be3-4273b543100f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 9b63afec73d7442e6ac1ce34b511beea6f43ffe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appdynamics"></a><span data-ttu-id="5df1d-103">Kurz: Azure Active Directory integrace s AppDynamics</span><span class="sxs-lookup"><span data-stu-id="5df1d-103">Tutorial: Azure Active Directory integration with AppDynamics</span></span>

<span data-ttu-id="5df1d-104">V tomto kurzu zjistíte, jak toointegrate AppDynamics s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5df1d-104">In this tutorial, you learn how toointegrate AppDynamics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5df1d-105">Integrace AppDynamics s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5df1d-105">Integrating AppDynamics with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5df1d-106">Můžete řídit ve službě Azure AD, který má přístup tooAppDynamics</span><span class="sxs-lookup"><span data-stu-id="5df1d-106">You can control in Azure AD who has access tooAppDynamics</span></span>
- <span data-ttu-id="5df1d-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAppDynamics (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5df1d-107">You can enable your users tooautomatically get signed-on tooAppDynamics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5df1d-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5df1d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5df1d-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5df1d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5df1d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5df1d-110">Prerequisites</span></span>

<span data-ttu-id="5df1d-111">Integrace služby Azure AD s AppDynamics tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5df1d-111">tooconfigure Azure AD integration with AppDynamics, you need hello following items:</span></span>

- <span data-ttu-id="5df1d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5df1d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5df1d-113">AppDynamics jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5df1d-113">An AppDynamics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5df1d-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5df1d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5df1d-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5df1d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5df1d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5df1d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5df1d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5df1d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5df1d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5df1d-118">Scenario description</span></span>
<span data-ttu-id="5df1d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5df1d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5df1d-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5df1d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5df1d-121">Přidání AppDynamics z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5df1d-121">Adding AppDynamics from hello gallery</span></span>
2. <span data-ttu-id="5df1d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5df1d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appdynamics-from-hello-gallery"></a><span data-ttu-id="5df1d-123">Přidání AppDynamics z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5df1d-123">Adding AppDynamics from hello gallery</span></span>
<span data-ttu-id="5df1d-124">tooconfigure hello integrace AppDynamics do Azure AD, je nutné tooadd AppDynamics hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5df1d-124">tooconfigure hello integration of AppDynamics into Azure AD, you need tooadd AppDynamics from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5df1d-125">**tooadd AppDynamics z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5df1d-125">**tooadd AppDynamics from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5df1d-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5df1d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5df1d-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5df1d-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5df1d-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5df1d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5df1d-133">Hello vyhledávacího pole zadejte **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-133">In hello search box, type **AppDynamics**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_search.png)

5. <span data-ttu-id="5df1d-135">Na panelu výsledků hello vyberte **AppDynamics**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5df1d-135">In hello results panel, select **AppDynamics**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5df1d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5df1d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5df1d-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s AppDynamics podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5df1d-138">In this section, you configure and test Azure AD single sign-on with AppDynamics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5df1d-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v AppDynamics je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5df1d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AppDynamics is tooa user in Azure AD.</span></span> <span data-ttu-id="5df1d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v AppDynamics musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5df1d-140">In other words, a link relationship between an Azure AD user and hello related user in AppDynamics needs toobe established.</span></span>

<span data-ttu-id="5df1d-141">V AppDynamics, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5df1d-141">In AppDynamics, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5df1d-142">tooconfigure a testu Azure AD jednotné přihlašování s AppDynamics, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5df1d-142">tooconfigure and test Azure AD single sign-on with AppDynamics, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5df1d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5df1d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5df1d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5df1d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5df1d-145">**[Vytváření testovacího uživatele AppDynamics](#creating-an-appdynamics-test-user)**  -toohave protějšek Britta Simon v AppDynamics, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5df1d-145">**[Creating an AppDynamics test user](#creating-an-appdynamics-test-user)** - toohave a counterpart of Britta Simon in AppDynamics that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5df1d-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5df1d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5df1d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5df1d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5df1d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5df1d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5df1d-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="5df1d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AppDynamics application.</span></span>

<span data-ttu-id="5df1d-150">**tooconfigure Azure AD jednotné přihlašování s AppDynamics, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5df1d-150">**tooconfigure Azure AD single sign-on with AppDynamics, perform hello following steps:**</span></span>

1. <span data-ttu-id="5df1d-151">V portálu Azure, na hello hello **AppDynamics** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-151">In hello Azure portal, on hello **AppDynamics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5df1d-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5df1d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_samlbase.png)

3. <span data-ttu-id="5df1d-155">Na hello **AppDynamics domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5df1d-155">On hello **AppDynamics Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_url.png)

    <span data-ttu-id="5df1d-157">a.</span><span class="sxs-lookup"><span data-stu-id="5df1d-157">a.</span></span> <span data-ttu-id="5df1d-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.saas.appdynamics.com`</span><span class="sxs-lookup"><span data-stu-id="5df1d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.saas.appdynamics.com`</span></span>

    <span data-ttu-id="5df1d-159">b.</span><span class="sxs-lookup"><span data-stu-id="5df1d-159">b.</span></span> <span data-ttu-id="5df1d-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.saas.appdynamics.com/controller`</span><span class="sxs-lookup"><span data-stu-id="5df1d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.saas.appdynamics.com/controller`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5df1d-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5df1d-161">These values are not real.</span></span> <span data-ttu-id="5df1d-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5df1d-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5df1d-163">Obraťte se na [tým podpory AppDynamics klienta](https://www.appdynamics.com/support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5df1d-163">Contact [AppDynamics Client support team](https://www.appdynamics.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="5df1d-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5df1d-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_certificate.png) 

5. <span data-ttu-id="5df1d-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5df1d-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5df1d-168">Na hello **AppDynamics konfigurace** klikněte na tlačítko **konfigurace AppDynamics** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5df1d-168">On hello **AppDynamics Configuration** section, click **Configure AppDynamics** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5df1d-169">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5df1d-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_configure.png) 

7. <span data-ttu-id="5df1d-171">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti AppDynamics tooyour.</span><span class="sxs-lookup"><span data-stu-id="5df1d-171">In a different web browser window, log in tooyour AppDynamics company site as an administrator.</span></span>

8. <span data-ttu-id="5df1d-172">V panelu nástrojů hello hello nahoře, klikněte na **nastavení**a potom klikněte na **správy**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-172">In hello toolbar on hello top, click **Settings**, and then click **Administration**.</span></span>
   
    <span data-ttu-id="5df1d-173">![Správa](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "správy")</span><span class="sxs-lookup"><span data-stu-id="5df1d-173">![Administration](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "Administration")</span></span>

9. <span data-ttu-id="5df1d-174">Klikněte na tlačítko hello **zprostředkovatele ověřování** kartě.</span><span class="sxs-lookup"><span data-stu-id="5df1d-174">Click hello **Authentication Provider** tab.</span></span>
   
    <span data-ttu-id="5df1d-175">![Zprostředkovatel ověřování](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "zprostředkovatele ověřování")</span><span class="sxs-lookup"><span data-stu-id="5df1d-175">![Authentication Provider](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "Authentication Provider")</span></span>

10. <span data-ttu-id="5df1d-176">V hello **zprostředkovatele ověřování** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5df1d-176">In hello **Authentication Provider** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="5df1d-177">![Konfigurace SAML](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="5df1d-177">![SAML Configuration](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML Configuration")</span></span>   

    <span data-ttu-id="5df1d-178">a.</span><span class="sxs-lookup"><span data-stu-id="5df1d-178">a.</span></span> <span data-ttu-id="5df1d-179">Jako **zprostředkovatele ověřování**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-179">As **Authentication Provider**, select **SAML**.</span></span>

    <span data-ttu-id="5df1d-180">b.</span><span class="sxs-lookup"><span data-stu-id="5df1d-180">b.</span></span> <span data-ttu-id="5df1d-181">V hello **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5df1d-181">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="5df1d-182">c.</span><span class="sxs-lookup"><span data-stu-id="5df1d-182">c.</span></span> <span data-ttu-id="5df1d-183">V hello **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5df1d-183">In hello **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="5df1d-184">d.</span><span class="sxs-lookup"><span data-stu-id="5df1d-184">d.</span></span> <span data-ttu-id="5df1d-185">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát** textbox</span><span class="sxs-lookup"><span data-stu-id="5df1d-185">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Certificate** textbox</span></span>

    <span data-ttu-id="5df1d-186">e.</span><span class="sxs-lookup"><span data-stu-id="5df1d-186">e.</span></span> <span data-ttu-id="5df1d-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-187">Click **Save**.</span></span>

     <span data-ttu-id="5df1d-188">![Uložit](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "uložit")</span><span class="sxs-lookup"><span data-stu-id="5df1d-188">![Save](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "Save")</span></span>

> [!TIP]
> <span data-ttu-id="5df1d-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5df1d-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5df1d-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5df1d-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5df1d-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5df1d-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5df1d-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5df1d-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="5df1d-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5df1d-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5df1d-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5df1d-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5df1d-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5df1d-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5df1d-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5df1d-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="5df1d-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5df1d-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5df1d-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5df1d-204">a.</span><span class="sxs-lookup"><span data-stu-id="5df1d-204">a.</span></span> <span data-ttu-id="5df1d-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5df1d-206">b.</span><span class="sxs-lookup"><span data-stu-id="5df1d-206">b.</span></span> <span data-ttu-id="5df1d-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5df1d-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5df1d-208">c.</span><span class="sxs-lookup"><span data-stu-id="5df1d-208">c.</span></span> <span data-ttu-id="5df1d-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5df1d-210">d.</span><span class="sxs-lookup"><span data-stu-id="5df1d-210">d.</span></span> <span data-ttu-id="5df1d-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-211">Click **Create**.</span></span>
 
### <a name="creating-an-appdynamics-test-user"></a><span data-ttu-id="5df1d-212">Vytváření testovacího uživatele AppDynamics</span><span class="sxs-lookup"><span data-stu-id="5df1d-212">Creating an AppDynamics test user</span></span>

<span data-ttu-id="5df1d-213">Uživatelé toolog tooenable Azure AD v tooAppDynamics, se musí být zřízená do AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="5df1d-213">tooenable Azure AD users toolog in tooAppDynamics, they must be provisioned into AppDynamics.</span></span> <span data-ttu-id="5df1d-214">V případě hello AppDynamics zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="5df1d-214">In hello case of AppDynamics, provisioning is a manual task.</span></span>

<span data-ttu-id="5df1d-215">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5df1d-215">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="5df1d-216">Přihlaste se tooyour AppDynamics společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="5df1d-216">Log in tooyour AppDynamics company site as an administrator.</span></span>

2. <span data-ttu-id="5df1d-217">Přejděte příliš**uživatelé**a potom klikněte na  **+**  tooopen hello **vytvořit uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5df1d-217">Go too**Users**, and then click **+** tooopen hello **Create User** dialog.</span></span>
   
    <span data-ttu-id="5df1d-218">![Uživatelé](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="5df1d-218">![Users](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "Users")</span></span>

3. <span data-ttu-id="5df1d-219">V hello **vytvořit uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5df1d-219">In hello **Create User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="5df1d-220">![Vytvoření uživatele](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="5df1d-220">![Create User](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "Create User")</span></span>
   
    <span data-ttu-id="5df1d-221">a.</span><span class="sxs-lookup"><span data-stu-id="5df1d-221">a.</span></span> <span data-ttu-id="5df1d-222">Typ hello **uživatelské jméno**, **název**, **e-mailu**, **nové heslo**, **opakujte nové heslo** platný aad účet chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="5df1d-222">Type hello **Username**, **Name**, **Email**, **New Password**, **Repeat New Password** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="5df1d-223">b.</span><span class="sxs-lookup"><span data-stu-id="5df1d-223">b.</span></span> <span data-ttu-id="5df1d-224">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-224">Click **Save**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="5df1d-225">Můžete použít všechny ostatní AppDynamics uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované AppDynamics tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5df1d-225">You can use any other AppDynamics user account creation tools or APIs provided by AppDynamics tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5df1d-226">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5df1d-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5df1d-227">V této části povolíte tak, že udělíte přístup tooAppDynamics toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5df1d-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAppDynamics.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5df1d-229">**tooassign Britta Simon tooAppDynamics, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5df1d-229">**tooassign Britta Simon tooAppDynamics, perform hello following steps:**</span></span>

1. <span data-ttu-id="5df1d-230">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5df1d-232">V seznamu aplikace hello vyberte **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-232">In hello applications list, select **AppDynamics**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_app.png) 

3. <span data-ttu-id="5df1d-234">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5df1d-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5df1d-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5df1d-236">Click **Add** button.</span></span> <span data-ttu-id="5df1d-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5df1d-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5df1d-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5df1d-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5df1d-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5df1d-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5df1d-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5df1d-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5df1d-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5df1d-242">Testing single sign-on</span></span>

<span data-ttu-id="5df1d-243">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5df1d-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5df1d-244">Po kliknutí na tlačítko hello AppDynamics dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour AppDynamics aplikace.</span><span class="sxs-lookup"><span data-stu-id="5df1d-244">When you click hello AppDynamics tile in hello Access Panel, you should get automatically signed-on tooyour AppDynamics application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5df1d-245">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5df1d-245">Additional resources</span></span>

* [<span data-ttu-id="5df1d-246">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5df1d-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5df1d-247">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5df1d-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_203.png

