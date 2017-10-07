---
title: 'Kurz: Azure Active Directory integrace s LockPath Keylight | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LockPath Keylight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 5485aeb068ba6fbdb4ea9bfc89d401e00c5b1d29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a><span data-ttu-id="8f181-103">Kurz: Azure Active Directory integrace s LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="8f181-103">Tutorial: Azure Active Directory integration with LockPath Keylight</span></span>

<span data-ttu-id="8f181-104">V tomto kurzu zjistíte, jak toointegrate LockPath Keylight s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8f181-104">In this tutorial, you learn how toointegrate LockPath Keylight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8f181-105">Integrace LockPath Keylight s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8f181-105">Integrating LockPath Keylight with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8f181-106">Můžete řídit ve službě Azure AD, který má přístup tooLockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="8f181-106">You can control in Azure AD who has access tooLockPath Keylight</span></span>
- <span data-ttu-id="8f181-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLockPath Keylight (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8f181-107">You can enable your users tooautomatically get signed-on tooLockPath Keylight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8f181-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8f181-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8f181-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8f181-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f181-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8f181-110">Prerequisites</span></span>

<span data-ttu-id="8f181-111">Integrace služby Azure AD s LockPath Keylight tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8f181-111">tooconfigure Azure AD integration with LockPath Keylight, you need hello following items:</span></span>

- <span data-ttu-id="8f181-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8f181-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8f181-113">LockPath Keylight jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8f181-113">A LockPath Keylight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8f181-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8f181-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8f181-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8f181-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8f181-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8f181-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8f181-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8f181-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8f181-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8f181-118">Scenario description</span></span>
<span data-ttu-id="8f181-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8f181-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8f181-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8f181-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8f181-121">Přidání LockPath Keylight z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8f181-121">Adding LockPath Keylight from hello gallery</span></span>
2. <span data-ttu-id="8f181-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8f181-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lockpath-keylight-from-hello-gallery"></a><span data-ttu-id="8f181-123">Přidání LockPath Keylight z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8f181-123">Adding LockPath Keylight from hello gallery</span></span>
<span data-ttu-id="8f181-124">tooconfigure hello integrace LockPath Keylight do Azure AD, je nutné tooadd LockPath Keylight hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8f181-124">tooconfigure hello integration of LockPath Keylight into Azure AD, you need tooadd LockPath Keylight from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8f181-125">**tooadd LockPath Keylight z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8f181-125">**tooadd LockPath Keylight from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f181-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8f181-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8f181-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8f181-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8f181-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8f181-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8f181-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8f181-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8f181-133">Hello vyhledávacího pole zadejte **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="8f181-133">In hello search box, type **LockPath Keylight**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_search.png)

5. <span data-ttu-id="8f181-135">Na panelu výsledků hello vyberte **LockPath Keylight**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f181-135">In hello results panel, select **LockPath Keylight**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8f181-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8f181-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8f181-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s LockPath Keylight podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="8f181-138">In this section, you configure and test Azure AD single sign-on with LockPath Keylight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8f181-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v LockPath Keylight je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f181-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LockPath Keylight is tooa user in Azure AD.</span></span> <span data-ttu-id="8f181-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LockPath Keylight musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8f181-140">In other words, a link relationship between an Azure AD user and hello related user in LockPath Keylight needs toobe established.</span></span>

<span data-ttu-id="8f181-141">V LockPath Keylight přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="8f181-141">In LockPath Keylight, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8f181-142">tooconfigure a testu Azure AD jednotné přihlašování s LockPath Keylight, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="8f181-142">tooconfigure and test Azure AD single sign-on with LockPath Keylight, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8f181-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8f181-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8f181-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8f181-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8f181-145">**[Vytvoření zkušebního uživatele LockPath Keylight](#creating-a-lockpath-keylight-test-user)**  -toohave protějšek Britta Simon v LockPath Keylight, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8f181-145">**[Creating a LockPath Keylight test user](#creating-a-lockpath-keylight-test-user)** - toohave a counterpart of Britta Simon in LockPath Keylight that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8f181-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8f181-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8f181-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8f181-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8f181-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8f181-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8f181-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="8f181-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LockPath Keylight application.</span></span>

<span data-ttu-id="8f181-150">**tooconfigure Azure AD jednotné přihlašování s LockPath Keylight, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8f181-150">**tooconfigure Azure AD single sign-on with LockPath Keylight, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f181-151">V portálu Azure, na hello hello **LockPath Keylight** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8f181-151">In hello Azure portal, on hello **LockPath Keylight** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8f181-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8f181-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_samlbase.png)

3. <span data-ttu-id="8f181-155">Na hello **LockPath Keylight domény a adresy URL** část, proveďte následující kroky hello::</span><span class="sxs-lookup"><span data-stu-id="8f181-155">On hello **LockPath Keylight Domain and URLs** section, perform hello following steps::</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_url.png)

    <span data-ttu-id="8f181-157">a.</span><span class="sxs-lookup"><span data-stu-id="8f181-157">a.</span></span> <span data-ttu-id="8f181-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.keylightgrc.com/`</span><span class="sxs-lookup"><span data-stu-id="8f181-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.keylightgrc.com/`</span></span>

    <span data-ttu-id="8f181-159">b.</span><span class="sxs-lookup"><span data-stu-id="8f181-159">b.</span></span> <span data-ttu-id="8f181-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.keylightgrc.com`</span><span class="sxs-lookup"><span data-stu-id="8f181-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.keylightgrc.com`</span></span>

    <span data-ttu-id="8f181-161">c.</span><span class="sxs-lookup"><span data-stu-id="8f181-161">c.</span></span> <span data-ttu-id="8f181-162">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.keylightgrc.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="8f181-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.keylightgrc.com/Login.aspx`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="8f181-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="8f181-163">These values are not real.</span></span> <span data-ttu-id="8f181-164">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="8f181-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="8f181-165">Obraťte se na [tým podpory klienta Keylight LockPath](https://www.lockpath.com/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8f181-165">Contact [LockPath Keylight Client support team](https://www.lockpath.com/contact/) tooget these values.</span></span> 

4. <span data-ttu-id="8f181-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8f181-166">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_certificate.png) 

5. <span data-ttu-id="8f181-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8f181-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="8f181-170">Na hello **LockPath Keylight konfigurace** klikněte na tlačítko **konfigurace LockPath Keylight** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="8f181-170">On hello **LockPath Keylight Configuration** section, click **Configure LockPath Keylight** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8f181-171">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="8f181-171">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_configure.png) 

7. <span data-ttu-id="8f181-173">tooenable jednotné přihlašování v LockPath Keylight, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8f181-173">tooenable SSO in LockPath Keylight, perform hello following steps:</span></span>
   
    <span data-ttu-id="8f181-174">a.</span><span class="sxs-lookup"><span data-stu-id="8f181-174">a.</span></span> <span data-ttu-id="8f181-175">Přihlášení tooyour LockPath Keylight účet jako správce.</span><span class="sxs-lookup"><span data-stu-id="8f181-175">Sign-on tooyour LockPath Keylight account as administrator.</span></span>
    
    <span data-ttu-id="8f181-176">b.</span><span class="sxs-lookup"><span data-stu-id="8f181-176">b.</span></span> <span data-ttu-id="8f181-177">V nabídce hello hello nahoře, klikněte na tlačítko **osoba**a vyberte **Keylight instalace**.</span><span class="sxs-lookup"><span data-stu-id="8f181-177">In hello menu on hello top, click **Person**, and select **Keylight Setup**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/401.png) 

    <span data-ttu-id="8f181-179">c.</span><span class="sxs-lookup"><span data-stu-id="8f181-179">c.</span></span> <span data-ttu-id="8f181-180">Ve stromovém zobrazení hello na levé straně hello klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="8f181-180">In hello treeview on hello left, click **SAML**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/402.png) 

    <span data-ttu-id="8f181-182">d.</span><span class="sxs-lookup"><span data-stu-id="8f181-182">d.</span></span> <span data-ttu-id="8f181-183">Na hello **SAML nastavení** dialogové okno, klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="8f181-183">On hello **SAML Settings** dialog, click **Edit**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/404.png) 

8. <span data-ttu-id="8f181-185">Na hello **upravit nastavení SAML** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8f181-185">On hello **Edit SAML Settings** dialog page, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    <span data-ttu-id="8f181-187">a.</span><span class="sxs-lookup"><span data-stu-id="8f181-187">a.</span></span> <span data-ttu-id="8f181-188">Nastavit **ověřování SAML** příliš**Active**.</span><span class="sxs-lookup"><span data-stu-id="8f181-188">Set **SAML authentication** too**Active**.</span></span>

    <span data-ttu-id="8f181-189">b.</span><span class="sxs-lookup"><span data-stu-id="8f181-189">b.</span></span> <span data-ttu-id="8f181-190">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu, která jste zkopírovali z hello portálu Azure do hello **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8f181-190">Paste hello **SAML Single Sign-On Service URL** value which you have copied from hello Azure portal into hello **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="8f181-191">c.</span><span class="sxs-lookup"><span data-stu-id="8f181-191">c.</span></span> <span data-ttu-id="8f181-192">Vložení hello **jednu adresu URL služby Sign-Out** hodnotu, která jste zkopírovali z hello portálu Azure do hello **adresa URL odhlašovací zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8f181-192">Paste hello **Single Sign-Out Service URL** value which you have copied from hello Azure portal into hello **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="8f181-193">d.</span><span class="sxs-lookup"><span data-stu-id="8f181-193">d.</span></span> <span data-ttu-id="8f181-194">Klikněte na tlačítko **zvolit soubor** tooselect vaše stažené LockPath Keylight certifikátu a potom klikněte na **otevřete** tooupload hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="8f181-194">Click **Choose File** tooselect your downloaded LockPath Keylight certificate, and then click **Open** tooupload hello certificate.</span></span>

    <span data-ttu-id="8f181-195">e.</span><span class="sxs-lookup"><span data-stu-id="8f181-195">e.</span></span> <span data-ttu-id="8f181-196">Nastavit **Id uživatele SAML umístění** příliš**NameIdentifier element hello subjektu příkazu**.</span><span class="sxs-lookup"><span data-stu-id="8f181-196">Set **SAML User Id location** too**NameIdentifier element of hello subject statement**.</span></span>
    
    <span data-ttu-id="8f181-197">f.</span><span class="sxs-lookup"><span data-stu-id="8f181-197">f.</span></span> <span data-ttu-id="8f181-198">Zadejte hello **poskytovatele služeb Keylight** pomocí hello následující vzor: **https://&lt;#companyname&gt;. keylightgrc.com**.</span><span class="sxs-lookup"><span data-stu-id="8f181-198">Provide hello **Keylight Service Provider** using hello following pattern: **https://&lt;CompanyName&gt;.keylightgrc.com**.</span></span>
    
    <span data-ttu-id="8f181-199">g.</span><span class="sxs-lookup"><span data-stu-id="8f181-199">g.</span></span> <span data-ttu-id="8f181-200">Nastavit **automatického zřizování uživatelů** příliš**Active**.</span><span class="sxs-lookup"><span data-stu-id="8f181-200">Set **Auto-provision users** too**Active**.</span></span>

    <span data-ttu-id="8f181-201">h.</span><span class="sxs-lookup"><span data-stu-id="8f181-201">h.</span></span> <span data-ttu-id="8f181-202">Nastavit **typ účtu automatického zřizování** příliš**úplné uživatelské**.</span><span class="sxs-lookup"><span data-stu-id="8f181-202">Set **Auto-provision account type** too**Full User**.</span></span>

    <span data-ttu-id="8f181-203">i.</span><span class="sxs-lookup"><span data-stu-id="8f181-203">i.</span></span> <span data-ttu-id="8f181-204">Nastavit **role zabezpečení automatického zřizování**, vyberte **standardní uživatel s SAML**.</span><span class="sxs-lookup"><span data-stu-id="8f181-204">Set **Auto-provision security role**, select **Standard User with SAML**.</span></span>
    
    <span data-ttu-id="8f181-205">j.</span><span class="sxs-lookup"><span data-stu-id="8f181-205">j.</span></span> <span data-ttu-id="8f181-206">Nastavit **konfigurace zabezpečení automatického zřizování**, vyberte **standardní konfigurace uživatele**.</span><span class="sxs-lookup"><span data-stu-id="8f181-206">Set **Auto-provision security config**, select **Standard User Configuration**.</span></span>
     
    <span data-ttu-id="8f181-207">kB.</span><span class="sxs-lookup"><span data-stu-id="8f181-207">k.</span></span> <span data-ttu-id="8f181-208">V hello **atribut e-mailu** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="8f181-208">In hello **Email attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="8f181-209">l.</span><span class="sxs-lookup"><span data-stu-id="8f181-209">l.</span></span> <span data-ttu-id="8f181-210">V hello **křestní jméno atribut** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="8f181-210">In hello **First name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="8f181-211">m.</span><span class="sxs-lookup"><span data-stu-id="8f181-211">m.</span></span> <span data-ttu-id="8f181-212">V hello **poslední atribut name** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="8f181-212">In hello **Last name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
    
    <span data-ttu-id="8f181-213">n.</span><span class="sxs-lookup"><span data-stu-id="8f181-213">n.</span></span> <span data-ttu-id="8f181-214">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8f181-214">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="8f181-215">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="8f181-215">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8f181-216">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="8f181-216">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8f181-217">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8f181-217">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8f181-218">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8f181-218">Creating an Azure AD test user</span></span>
<span data-ttu-id="8f181-219">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="8f181-219">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8f181-221">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8f181-221">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f181-222">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8f181-222">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8f181-224">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8f181-224">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8f181-226">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="8f181-226">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8f181-228">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8f181-228">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8f181-230">a.</span><span class="sxs-lookup"><span data-stu-id="8f181-230">a.</span></span> <span data-ttu-id="8f181-231">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8f181-231">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8f181-232">b.</span><span class="sxs-lookup"><span data-stu-id="8f181-232">b.</span></span> <span data-ttu-id="8f181-233">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8f181-233">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8f181-234">c.</span><span class="sxs-lookup"><span data-stu-id="8f181-234">c.</span></span> <span data-ttu-id="8f181-235">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8f181-235">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8f181-236">d.</span><span class="sxs-lookup"><span data-stu-id="8f181-236">d.</span></span> <span data-ttu-id="8f181-237">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8f181-237">Click **Create**.</span></span>
 
### <a name="creating-a-lockpath-keylight-test-user"></a><span data-ttu-id="8f181-238">Vytvoření zkušebního uživatele LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="8f181-238">Creating a LockPath Keylight test user</span></span>

<span data-ttu-id="8f181-239">V této části vytvoříte volal Britta Simon v LockPath Keylight uživatele.</span><span class="sxs-lookup"><span data-stu-id="8f181-239">In this section, you create a user called Britta Simon in LockPath Keylight.</span></span> <span data-ttu-id="8f181-240">LockPath Keylight podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="8f181-240">LockPath Keylight supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="8f181-241">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="8f181-241">There is no action item for you in this section.</span></span> <span data-ttu-id="8f181-242">Nový uživatel se vytvoří při přístupu k LockPath Keylight, pokud ještě neexistuje hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="8f181-242">A new user is created when accessing LockPath Keylight if hello user doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="8f181-243">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory klienta Keylight LockPath](https://www.lockpath.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="8f181-243">If you need toocreate a user manually, you need toocontact hello [LockPath Keylight Client support team](https://www.lockpath.com/contact/).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8f181-244">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8f181-244">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8f181-245">V této části povolíte tak, že udělíte přístup tooLockPath Keylight toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8f181-245">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLockPath Keylight.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8f181-247">**tooassign Britta Simon tooLockPath Keylight, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8f181-247">**tooassign Britta Simon tooLockPath Keylight, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f181-248">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8f181-248">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8f181-250">V seznamu aplikace hello vyberte **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="8f181-250">In hello applications list, select **LockPath Keylight**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_app.png) 

3. <span data-ttu-id="8f181-252">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8f181-252">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8f181-254">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8f181-254">Click **Add** button.</span></span> <span data-ttu-id="8f181-255">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8f181-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8f181-257">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8f181-257">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8f181-258">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8f181-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8f181-259">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8f181-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8f181-260">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8f181-260">Testing single sign-on</span></span>

<span data-ttu-id="8f181-261">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8f181-261">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8f181-262">Po kliknutí na tlačítko hello LockPath Keylight dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour LockPath Keylight aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f181-262">When you click hello LockPath Keylight tile in hello Access Panel, you should get automatically signed-on tooyour LockPath Keylight application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8f181-263">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8f181-263">Additional resources</span></span>

* [<span data-ttu-id="8f181-264">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8f181-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8f181-265">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8f181-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png

