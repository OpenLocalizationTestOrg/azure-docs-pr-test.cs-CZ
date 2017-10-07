---
title: "Kurz: Azure Active Directory integrace s Office virtuální 8 x 8 | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a 8 x 8 virtuální Office."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: df5c5de77285cd3912b68cc3b1e3eee274aa951c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a><span data-ttu-id="faa26-103">Kurz: Azure Active Directory integrace s Office virtuální 8 x 8</span><span class="sxs-lookup"><span data-stu-id="faa26-103">Tutorial: Azure Active Directory integration with 8x8 Virtual Office</span></span>

<span data-ttu-id="faa26-104">V tomto kurzu zjistíte, jak toointegrate 8 x 8 virtuální Office s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="faa26-104">In this tutorial, you learn how toointegrate 8x8 Virtual Office with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="faa26-105">Integrace 8 x 8 virtuální Office s Azure AD umožňuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="faa26-105">Integrating 8x8 Virtual Office with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="faa26-106">Můžete řídit ve službě Azure AD, který má přístup too8x8 virtuální Office</span><span class="sxs-lookup"><span data-stu-id="faa26-106">You can control in Azure AD who has access too8x8 Virtual Office</span></span>
- <span data-ttu-id="faa26-107">Můžete povolit uživatelům získat tooautomatically přihlášeného too8x8 virtuální Office (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="faa26-107">You can enable your users tooautomatically get signed-on too8x8 Virtual Office (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="faa26-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="faa26-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="faa26-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="faa26-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="faa26-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="faa26-110">Prerequisites</span></span>

<span data-ttu-id="faa26-111">tooconfigure integrace Azure AD s 8 x 8 virtuální Office, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="faa26-111">tooconfigure Azure AD integration with 8x8 Virtual Office, you need hello following items:</span></span>

- <span data-ttu-id="faa26-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="faa26-112">An Azure AD subscription</span></span>
- <span data-ttu-id="faa26-113">8 x 8 virtuální Office jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="faa26-113">A 8x8 Virtual Office single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="faa26-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="faa26-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="faa26-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="faa26-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="faa26-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="faa26-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="faa26-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="faa26-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="faa26-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="faa26-118">Scenario description</span></span>
<span data-ttu-id="faa26-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="faa26-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="faa26-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="faa26-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="faa26-121">Přidání Office virtuální 8 x 8 z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="faa26-121">Adding 8x8 Virtual Office from hello gallery</span></span>
2. <span data-ttu-id="faa26-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="faa26-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-8x8-virtual-office-from-hello-gallery"></a><span data-ttu-id="faa26-123">Přidání Office virtuální 8 x 8 z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="faa26-123">Adding 8x8 Virtual Office from hello gallery</span></span>
<span data-ttu-id="faa26-124">tooconfigure hello integrace Office virtuální 8 x 8 do služby Azure AD, je nutné tooadd 8 x 8 virtuální Office hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="faa26-124">tooconfigure hello integration of 8x8 Virtual Office into Azure AD, you need tooadd 8x8 Virtual Office from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="faa26-125">**provedení virtuální Office z Galerie hello tooadd 8 x 8 hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="faa26-125">**tooadd 8x8 Virtual Office from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="faa26-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="faa26-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="faa26-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="faa26-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="faa26-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="faa26-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="faa26-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="faa26-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="faa26-133">Hello vyhledávacího pole zadejte **8 x 8 virtuální Office**.</span><span class="sxs-lookup"><span data-stu-id="faa26-133">In hello search box, type **8x8 Virtual Office**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. <span data-ttu-id="faa26-135">Na panelu výsledků hello vyberte **8 x 8 virtuální Office**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="faa26-135">In hello results panel, select **8x8 Virtual Office**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="faa26-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="faa26-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="faa26-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 8 x 8, které virtuální Office podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="faa26-138">In this section, you configure and test Azure AD single sign-on with 8x8 Virtual Office based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="faa26-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Office virtuální 8 x 8 je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="faa26-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 8x8 Virtual Office is tooa user in Azure AD.</span></span> <span data-ttu-id="faa26-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello 8 x 8 virtuální Office musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="faa26-140">In other words, a link relationship between an Azure AD user and hello related user in 8x8 Virtual Office needs toobe established.</span></span>

<span data-ttu-id="faa26-141">V systému Office virtuální 8 x 8 přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="faa26-141">In 8x8 Virtual Office, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="faa26-142">tooconfigure a testování Azure AD jednotné přihlašování s 8 x 8 virtuální Office, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="faa26-142">tooconfigure and test Azure AD single sign-on with 8x8 Virtual Office, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="faa26-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="faa26-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="faa26-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="faa26-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="faa26-145">**[Vytvoření zkušebního uživatele virtuální Office 8 x 8](#creating-a-8x8-virtual-office-test-user)**  -toohave protějšek Britta Simon v Office virtuální 8 x 8, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="faa26-145">**[Creating a 8x8 Virtual Office test user](#creating-a-8x8-virtual-office-test-user)** - toohave a counterpart of Britta Simon in 8x8 Virtual Office that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="faa26-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="faa26-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="faa26-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="faa26-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="faa26-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="faa26-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="faa26-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Office virtuální 8 x 8.</span><span class="sxs-lookup"><span data-stu-id="faa26-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 8x8 Virtual Office application.</span></span>

<span data-ttu-id="faa26-150">**tooconfigure Azure AD jednotné přihlašování s 8 x 8 virtuální Office, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="faa26-150">**tooconfigure Azure AD single sign-on with 8x8 Virtual Office, perform hello following steps:**</span></span>

1. <span data-ttu-id="faa26-151">V portálu Azure, na hello hello **8 x 8 virtuální Office** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="faa26-151">In hello Azure portal, on hello **8x8 Virtual Office** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="faa26-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="faa26-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. <span data-ttu-id="faa26-155">Na hello **8 x 8 virtuální Office domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="faa26-155">On hello **8x8 Virtual Office Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    <span data-ttu-id="faa26-157">a.</span><span class="sxs-lookup"><span data-stu-id="faa26-157">a.</span></span> <span data-ttu-id="faa26-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="faa26-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>

    <span data-ttu-id="faa26-159">| `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |</span><span class="sxs-lookup"><span data-stu-id="faa26-159">| `https://sso.8x8.com/<companyname>` |
 | `https://www.8x8.com/<companyname>` |
 | `https://sso.8x8pilot.com/<companyname>` |</span></span>

    <span data-ttu-id="faa26-160">b.</span><span class="sxs-lookup"><span data-stu-id="faa26-160">b.</span></span> <span data-ttu-id="faa26-161">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="faa26-161">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>

    <span data-ttu-id="faa26-162">| `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|</span><span class="sxs-lookup"><span data-stu-id="faa26-162">| `https://<subdomain>.8x8.com/saml2` |
 | `https://<subdomain>.8x8pilot.com/saml2`|</span></span>

    > [!NOTE] 
    > <span data-ttu-id="faa26-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="faa26-163">These values are not real.</span></span> <span data-ttu-id="faa26-164">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="faa26-164">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="faa26-165">Obraťte se na [tým podpory virtuální Office 8 x 8](https://www.8x8.com/about-us/contact-us) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="faa26-165">Contact [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us) tooget these values.</span></span>
 


4. <span data-ttu-id="faa26-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="faa26-166">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. <span data-ttu-id="faa26-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="faa26-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="faa26-170">Na hello **8 x 8 virtuální Office konfigurace** klikněte na tlačítko **virtuální Office konfigurovat 8 x 8** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="faa26-170">On hello **8x8 Virtual Office Configuration** section, click **Configure 8x8 Virtual Office** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="faa26-171">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="faa26-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. <span data-ttu-id="faa26-173">Klient virtuální Office přihlašování tooyour 8 x 8 jako správce.</span><span class="sxs-lookup"><span data-stu-id="faa26-173">Sign-on tooyour 8x8 Virtual Office tenant as an administrator.</span></span>

8. <span data-ttu-id="faa26-174">Vyberte **virtuální Office účet Mgr** na panelu aplikace.</span><span class="sxs-lookup"><span data-stu-id="faa26-174">Select **Virtual Office Account Mgr** on Application Panel.</span></span>
   
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. <span data-ttu-id="faa26-176">Vyberte **obchodní** účet toomanage a klikněte na tlačítko **přihlásit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="faa26-176">Select **Business** account toomanage and click **Sign In** button.</span></span>
   
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. <span data-ttu-id="faa26-178">Klikněte na tlačítko **účty** kartě hello v seznamu v nabídce.</span><span class="sxs-lookup"><span data-stu-id="faa26-178">Click **Accounts** tab in hello menu list.</span></span>
   
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. <span data-ttu-id="faa26-180">Klikněte na tlačítko **jednotné přihlašování** v hello seznam účtů.</span><span class="sxs-lookup"><span data-stu-id="faa26-180">Click **Single Sign On** in hello list of Accounts.</span></span>
   
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. <span data-ttu-id="faa26-182">Vyberte **jednotné přihlašování** pod metodu ověřování a klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="faa26-182">Select **Single Sign On** under Authentication method and click **SAML**.</span></span>
    
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. <span data-ttu-id="faa26-184">Kopírování **URL jednotné přihlašování SAML**, **jeden Sing se adresa URL služby** a **URL vystavitele** z Azure AD příliš**přihlášení v adrese URL**, **přihlašovací adresy URL odhlašovací stránky**  a **URL vystavitele** v 8 x 8 virtuální Office.</span><span class="sxs-lookup"><span data-stu-id="faa26-184">Copy **SAML SSO URL**, **Single Sing Out Service URL** and **Issuer URL** from Azure AD too**Sign In URL**, **Sign Out URL** and **Issuer URL** in 8x8 Virtual Office.</span></span> 
    
    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. <span data-ttu-id="faa26-186">Klikněte na tlačítko **prohlížeče** tlačítko tooupload hello certifikát, který jste si stáhli z Azure AD a klikněte na tlačítko hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="faa26-186">Click **Browser** button tooupload hello certificate which you downloaded from Azure AD, and click hello **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="faa26-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="faa26-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="faa26-188">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="faa26-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="faa26-189">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="faa26-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="faa26-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="faa26-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="faa26-191">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="faa26-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="faa26-193">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="faa26-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="faa26-194">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="faa26-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="faa26-196">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="faa26-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="faa26-198">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="faa26-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="faa26-200">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="faa26-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="faa26-202">a.</span><span class="sxs-lookup"><span data-stu-id="faa26-202">a.</span></span> <span data-ttu-id="faa26-203">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="faa26-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="faa26-204">b.</span><span class="sxs-lookup"><span data-stu-id="faa26-204">b.</span></span> <span data-ttu-id="faa26-205">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="faa26-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="faa26-206">c.</span><span class="sxs-lookup"><span data-stu-id="faa26-206">c.</span></span> <span data-ttu-id="faa26-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="faa26-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="faa26-208">d.</span><span class="sxs-lookup"><span data-stu-id="faa26-208">d.</span></span> <span data-ttu-id="faa26-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="faa26-209">Click **Create**.</span></span>
 
### <a name="creating-a-8x8-virtual-office-test-user"></a><span data-ttu-id="faa26-210">Vytvoření zkušebního uživatele virtuální Office 8 x 8</span><span class="sxs-lookup"><span data-stu-id="faa26-210">Creating a 8x8 Virtual Office test user</span></span>

<span data-ttu-id="faa26-211">Hello cílem této části je toocreate uživatel volal Britta Simon v Office virtuální 8 x 8.</span><span class="sxs-lookup"><span data-stu-id="faa26-211">hello objective of this section is toocreate a user called Britta Simon in 8x8 Virtual Office.</span></span> <span data-ttu-id="faa26-212">8 x 8 virtuální Office podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="faa26-212">8x8 Virtual Office supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="faa26-213">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="faa26-213">There is no action item for you in this section.</span></span> <span data-ttu-id="faa26-214">Nový uživatel se vytvoří během pokusu o tooaccess 8 x 8 virtuální Office Pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="faa26-214">A new user is created during an attempt tooaccess 8x8 Virtual Office if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="faa26-215">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory virtuální Office 8 x 8](https://www.8x8.com/about-us/contact-us).</span><span class="sxs-lookup"><span data-stu-id="faa26-215">If you need toocreate a user manually, you need toocontact hello [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="faa26-216">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="faa26-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="faa26-217">V této části povolíte Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup too8x8 virtuální Office.</span><span class="sxs-lookup"><span data-stu-id="faa26-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too8x8 Virtual Office.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="faa26-219">**tooassign Britta Simon too8x8 virtuální Office, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="faa26-219">**tooassign Britta Simon too8x8 Virtual Office, perform hello following steps:**</span></span>

1. <span data-ttu-id="faa26-220">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="faa26-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="faa26-222">V seznamu aplikace hello vyberte **8 x 8 virtuální Office**.</span><span class="sxs-lookup"><span data-stu-id="faa26-222">In hello applications list, select **8x8 Virtual Office**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. <span data-ttu-id="faa26-224">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="faa26-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="faa26-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="faa26-226">Click **Add** button.</span></span> <span data-ttu-id="faa26-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="faa26-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="faa26-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="faa26-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="faa26-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="faa26-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="faa26-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="faa26-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="faa26-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="faa26-232">Testing single sign-on</span></span>

<span data-ttu-id="faa26-233">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="faa26-233">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="faa26-234">Když kliknete na dlaždici virtuální Office hello 8 x 8 v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour 8 x 8 Office virtuální aplikace.</span><span class="sxs-lookup"><span data-stu-id="faa26-234">When you click hello 8x8 Virtual Office tile in hello Access Panel, you should get automatically signed-on tooyour 8x8 Virtual Office application.</span></span>
<span data-ttu-id="faa26-235">Další informace o na přístupovém panelu najdete v tématu [Úvod toohello přístupového panelu](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="faa26-235">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="faa26-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="faa26-236">Additional resources</span></span>

* [<span data-ttu-id="faa26-237">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="faa26-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="faa26-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="faa26-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png

