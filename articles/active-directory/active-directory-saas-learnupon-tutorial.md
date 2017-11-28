---
title: 'Kurz: Azure Active Directory integrace s LearnUpon | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LearnUpon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fdb9c62172327a539f0459c98aa20e63fa441e4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a><span data-ttu-id="7eaf9-103">Kurz: Azure Active Directory integrace s LearnUpon</span><span class="sxs-lookup"><span data-stu-id="7eaf9-103">Tutorial: Azure Active Directory integration with LearnUpon</span></span>

<span data-ttu-id="7eaf9-104">V tomto kurzu zjistíte, jak toointegrate LearnUpon s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7eaf9-104">In this tutorial, you learn how toointegrate LearnUpon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7eaf9-105">Integrace LearnUpon s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7eaf9-105">Integrating LearnUpon with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7eaf9-106">Můžete řídit ve službě Azure AD, který má přístup tooLearnUpon</span><span class="sxs-lookup"><span data-stu-id="7eaf9-106">You can control in Azure AD who has access tooLearnUpon</span></span>
- <span data-ttu-id="7eaf9-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLearnUpon (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7eaf9-107">You can enable your users tooautomatically get signed-on tooLearnUpon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7eaf9-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7eaf9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7eaf9-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7eaf9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7eaf9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7eaf9-110">Prerequisites</span></span>

<span data-ttu-id="7eaf9-111">Integrace služby Azure AD s LearnUpon tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7eaf9-111">tooconfigure Azure AD integration with LearnUpon, you need hello following items:</span></span>

- <span data-ttu-id="7eaf9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7eaf9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7eaf9-113">LearnUpon jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7eaf9-113">A LearnUpon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7eaf9-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7eaf9-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7eaf9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7eaf9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7eaf9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7eaf9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7eaf9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7eaf9-118">Scenario description</span></span>
<span data-ttu-id="7eaf9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7eaf9-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7eaf9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7eaf9-121">Přidání LearnUpon z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7eaf9-121">Adding LearnUpon from hello gallery</span></span>
2. <span data-ttu-id="7eaf9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7eaf9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learnupon-from-hello-gallery"></a><span data-ttu-id="7eaf9-123">Přidání LearnUpon z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7eaf9-123">Adding LearnUpon from hello gallery</span></span>
<span data-ttu-id="7eaf9-124">tooconfigure hello integrace LearnUpon do Azure AD, je nutné tooadd LearnUpon hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-124">tooconfigure hello integration of LearnUpon into Azure AD, you need tooadd LearnUpon from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7eaf9-125">**tooadd LearnUpon z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7eaf9-125">**tooadd LearnUpon from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7eaf9-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7eaf9-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7eaf9-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7eaf9-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7eaf9-133">Hello vyhledávacího pole zadejte **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-133">In hello search box, type **LearnUpon**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. <span data-ttu-id="7eaf9-135">Na panelu výsledků hello vyberte **LearnUpon**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-135">In hello results panel, select **LearnUpon**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7eaf9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7eaf9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7eaf9-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LearnUpon podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7eaf9-138">In this section, you configure and test Azure AD single sign-on with LearnUpon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7eaf9-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v LearnUpon je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LearnUpon is tooa user in Azure AD.</span></span> <span data-ttu-id="7eaf9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LearnUpon musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-140">In other words, a link relationship between an Azure AD user and hello related user in LearnUpon needs toobe established.</span></span>

<span data-ttu-id="7eaf9-141">V LearnUpon, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-141">In LearnUpon, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7eaf9-142">tooconfigure a testu Azure AD jednotné přihlašování s LearnUpon, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="7eaf9-142">tooconfigure and test Azure AD single sign-on with LearnUpon, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7eaf9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7eaf9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7eaf9-145">**[Vytvoření zkušebního uživatele LearnUpon](#creating-a-learnupon-test-user)**  -toohave protějšek Britta Simon v LearnUpon, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-145">**[Creating a LearnUpon test user](#creating-a-learnupon-test-user)** - toohave a counterpart of Britta Simon in LearnUpon that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7eaf9-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7eaf9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7eaf9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7eaf9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7eaf9-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LearnUpon.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LearnUpon application.</span></span>

<span data-ttu-id="7eaf9-150">**tooconfigure Azure AD jednotné přihlašování s LearnUpon, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7eaf9-150">**tooconfigure Azure AD single sign-on with LearnUpon, perform hello following steps:**</span></span>

1. <span data-ttu-id="7eaf9-151">V portálu Azure, na hello hello **LearnUpon** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-151">In hello Azure portal, on hello **LearnUpon** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7eaf9-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. <span data-ttu-id="7eaf9-155">Na hello **LearnUpon domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7eaf9-155">On hello **LearnUpon Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    <span data-ttu-id="7eaf9-157">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.learnupon.com/saml/consumer`</span><span class="sxs-lookup"><span data-stu-id="7eaf9-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.learnupon.com/saml/consumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7eaf9-158">Upozorňujeme, že se nejedná hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-158">Please note that this is not hello real value.</span></span> <span data-ttu-id="7eaf9-159">Máte tooupdate tuto hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-159">you have tooupdate this value with hello actual Reply URL.</span></span> <span data-ttu-id="7eaf9-160">Obraťte se na tooget tuto hodnotu [tým podpory LearnUpon](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="7eaf9-160">tooget this value Contact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span>



4. <span data-ttu-id="7eaf9-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-161">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. <span data-ttu-id="7eaf9-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7eaf9-165">Na hello **LearnUpon konfigurace** klikněte na tlačítko **konfigurace LearnUpon** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-165">On hello **LearnUpon Configuration** section, click **Configure LearnUpon** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7eaf9-166">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7eaf9-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. <span data-ttu-id="7eaf9-168">Otevřete jiné instance prohlížeče a přihlášení do LearnUpon s účtem správce.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-168">Open another browser instance and login into LearnUpon with an administrator account.</span></span> 

8. <span data-ttu-id="7eaf9-169">Klikněte na tlačítko hello **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-169">Click hello **settings** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. <span data-ttu-id="7eaf9-171">Klikněte na tlačítko **jednotné přihlašování – SAML**a potom klikněte na **obecné nastavení** tooconfigure SAML nastavení.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-171">Click **Single Sign On - SAML**, and then click **General Settings** tooconfigure SAML settings.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. <span data-ttu-id="7eaf9-173">V hello **obecné nastavení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7eaf9-173">In hello **General Settings** section, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    <span data-ttu-id="7eaf9-175">a.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-175">a.</span></span> <span data-ttu-id="7eaf9-176">Vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-176">Select **Enabled**.</span></span>

    <span data-ttu-id="7eaf9-177">b.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-177">b.</span></span> <span data-ttu-id="7eaf9-178">Vyberte **verze** jako **2.0**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-178">Select **Version** as **2.0**.</span></span>

    <span data-ttu-id="7eaf9-179">c.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-179">c.</span></span> <span data-ttu-id="7eaf9-180">Vyberte **přeskočit podmínky** jako **ne**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-180">Select **Skip conditions** as **No**.</span></span>

    <span data-ttu-id="7eaf9-181">d.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-181">d.</span></span> <span data-ttu-id="7eaf9-182">V hello **vystavení tokenu SAML název parametru** textovému poli, název typu hello požadavek post parametr toohello výše uvedené adresy URL příjemce SAML obsahující hello kontrolního výrazu SAML toobe ověřit a ověřovat – například  **SAMLResponse**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-182">In hello **SAML Token Post param name** textbox, type hello name of request post parameter toohello SAML consumer URL indicated above that contains hello SAML Assertion toobe verified and authenticated - for example **SAMLResponse**.</span></span>

    <span data-ttu-id="7eaf9-183">e.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-183">e.</span></span> <span data-ttu-id="7eaf9-184">V hello **formát názvu identifikátor** textovému poli, typ hello hodnotu, která určuje, kde ve vaši uživatelé hello kontrolního výrazu SAML identifikátor (e-mailovou adresu) nachází – například **urn: oasis: názvy: tc: SAML:1.1:nameid-formát: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-184">In hello **Name Identifier Format** textbox, type hello value that indicates where in your SAML Assertion hello users identifier (Email address) resides - for example **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>
  
    <span data-ttu-id="7eaf9-185">f.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-185">f.</span></span> <span data-ttu-id="7eaf9-186">V hello **identifikovat umístění poskytovatele** textové pole, typ hello hodnotu, která určuje, kde hello uživatelům jsou odeslány tooif kliknou na ikonu vašeho nahrané z obrazovky přihlášení k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-186">In hello **Identify Provider Location** textbox, type hello value that indicates where hello users are sent tooif they click on your uploaded icon from your Azure portal login screen.</span></span>
  
    <span data-ttu-id="7eaf9-187">g.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-187">g.</span></span> <span data-ttu-id="7eaf9-188">V hello **Odhlásit se adresa URL** textovému poli, vložte hello **Sign-Out URL** který jste zkopírovali z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-188">In hello **Sign out URL** textbox, paste hello **Sign-Out URL** which you have copied from hello Azure portal.</span></span>
    
    <span data-ttu-id="7eaf9-189">h.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-189">h.</span></span> <span data-ttu-id="7eaf9-190">Klikněte na tlačítko **spravovat prstem výtisků**a pak nahrajte hello prstu stažené certifikátu.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-190">Click **Manage finger prints**, and then upload hello finger print of your downloaded certificate.</span></span>

11. <span data-ttu-id="7eaf9-191">Klikněte na tlačítko **uživatelská nastavení**a poté proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7eaf9-191">Click **User Settings**, and then perform hello following steps:</span></span>
   
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    <span data-ttu-id="7eaf9-193">a.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-193">a.</span></span> <span data-ttu-id="7eaf9-194">V hello **křestní jméno identifikátor formátu** textovému poli, typ hello hodnotu, která sděluje nám v vaší kontrolního výrazu SAML hello uživatelé firstname – například nachází: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-194">In hello **First Name Identifier Format** textbox, type hello value that tells us where in your SAML Assertion hello users firstname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>
  
    <span data-ttu-id="7eaf9-195">b.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-195">b.</span></span> <span data-ttu-id="7eaf9-196">V hello **poslední formát názvu identifikátor** textovému poli, typ hello hodnotu, která sděluje nám v vaší kontrolního výrazu SAML hello uživatelé lastname – například nachází: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-196">In hello **Last Name Identifier Format** textbox, type hello value that tells us where in your SAML Assertion hello users lastname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

> [!TIP]
> <span data-ttu-id="7eaf9-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="7eaf9-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7eaf9-198">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7eaf9-199">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7eaf9-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7eaf9-200">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7eaf9-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="7eaf9-201">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7eaf9-203">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7eaf9-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7eaf9-204">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7eaf9-206">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7eaf9-208">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7eaf9-210">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7eaf9-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7eaf9-212">a.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-212">a.</span></span> <span data-ttu-id="7eaf9-213">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7eaf9-214">b.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-214">b.</span></span> <span data-ttu-id="7eaf9-215">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7eaf9-216">c.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-216">c.</span></span> <span data-ttu-id="7eaf9-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7eaf9-218">d.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-218">d.</span></span> <span data-ttu-id="7eaf9-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-219">Click **Create**.</span></span>
 
### <a name="creating-a-learnupon-test-user"></a><span data-ttu-id="7eaf9-220">Vytvoření zkušebního uživatele LearnUpon</span><span class="sxs-lookup"><span data-stu-id="7eaf9-220">Creating a LearnUpon test user</span></span>

<span data-ttu-id="7eaf9-221">Hello cílem této části je toocreate volal Britta Simon v LearnUpon uživatele.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-221">hello objective of this section is toocreate a user called Britta Simon in LearnUpon.</span></span> <span data-ttu-id="7eaf9-222">LearnUpon podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-222">LearnUpon supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="7eaf9-223">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-223">There is no action item for you in this section.</span></span> <span data-ttu-id="7eaf9-224">Pokud ještě neexistuje, se během pokusu o tooaccess LearnUpon vytvoří nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-224">A new user will be created during an attempt tooaccess LearnUpon if it doesn't exist yet.</span></span> <span data-ttu-id="7eaf9-225">[Konfigurace Azure AD jednotné přihlášení](#configuring-azure-ad-single-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="7eaf9-225">[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="7eaf9-226">Pokud potřebujete toocreate uživatelé ručně, je nutné toocontact [tým podpory LearnUpon](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="7eaf9-226">If you need toocreate an user manually, you need toocontact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7eaf9-227">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7eaf9-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7eaf9-228">V této části povolíte tak, že udělíte přístup tooLearnUpon toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLearnUpon.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7eaf9-230">**tooassign Britta Simon tooLearnUpon, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7eaf9-230">**tooassign Britta Simon tooLearnUpon, perform hello following steps:**</span></span>

1. <span data-ttu-id="7eaf9-231">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7eaf9-233">V seznamu aplikace hello vyberte **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-233">In hello applications list, select **LearnUpon**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. <span data-ttu-id="7eaf9-235">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7eaf9-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-237">Click **Add** button.</span></span> <span data-ttu-id="7eaf9-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7eaf9-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7eaf9-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7eaf9-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7eaf9-243">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7eaf9-243">Testing single sign-on</span></span>

<span data-ttu-id="7eaf9-244">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7eaf9-245">Když kliknete na dlaždici LearnUpon hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour LearnUpon aplikace.</span><span class="sxs-lookup"><span data-stu-id="7eaf9-245">When you click hello LearnUpon tile in hello Access Panel, you should get automatically signed-on tooyour LearnUpon application.</span></span>
<span data-ttu-id="7eaf9-246">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7eaf9-246">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7eaf9-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7eaf9-247">Additional resources</span></span>

* [<span data-ttu-id="7eaf9-248">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7eaf9-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7eaf9-249">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7eaf9-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

