---
title: 'Kurz: Azure Active Directory integrace s Syncplicity | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Syncplicity."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 6148112a959232ed24d76d1c7b8773f06568fee7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a><span data-ttu-id="62345-103">Kurz: Azure Active Directory integrace s Syncplicity</span><span class="sxs-lookup"><span data-stu-id="62345-103">Tutorial: Azure Active Directory integration with Syncplicity</span></span>

<span data-ttu-id="62345-104">V tomto kurzu zjistíte, jak toointegrate Syncplicity s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="62345-104">In this tutorial, you learn how toointegrate Syncplicity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="62345-105">Integrace Syncplicity s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="62345-105">Integrating Syncplicity with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="62345-106">Můžete řídit ve službě Azure AD, který má přístup tooSyncplicity</span><span class="sxs-lookup"><span data-stu-id="62345-106">You can control in Azure AD who has access tooSyncplicity</span></span>
- <span data-ttu-id="62345-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSyncplicity (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="62345-107">You can enable your users tooautomatically get signed-on tooSyncplicity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="62345-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="62345-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="62345-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="62345-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62345-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="62345-110">Prerequisites</span></span>

<span data-ttu-id="62345-111">Integrace služby Azure AD s Syncplicity tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="62345-111">tooconfigure Azure AD integration with Syncplicity, you need hello following items:</span></span>

- <span data-ttu-id="62345-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="62345-112">An Azure AD subscription</span></span>
- <span data-ttu-id="62345-113">Syncplicity jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="62345-113">A Syncplicity single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="62345-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="62345-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="62345-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="62345-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="62345-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="62345-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="62345-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="62345-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="62345-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="62345-118">Scenario description</span></span>
<span data-ttu-id="62345-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="62345-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="62345-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="62345-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="62345-121">Přidání Syncplicity z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="62345-121">Adding Syncplicity from hello gallery</span></span>
2. <span data-ttu-id="62345-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="62345-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-syncplicity-from-hello-gallery"></a><span data-ttu-id="62345-123">Přidání Syncplicity z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="62345-123">Adding Syncplicity from hello gallery</span></span>
<span data-ttu-id="62345-124">tooconfigure hello integrace Syncplicity do Azure AD, je nutné tooadd Syncplicity hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="62345-124">tooconfigure hello integration of Syncplicity into Azure AD, you need tooadd Syncplicity from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="62345-125">**tooadd Syncplicity z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="62345-125">**tooadd Syncplicity from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="62345-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="62345-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="62345-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="62345-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="62345-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="62345-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="62345-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="62345-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="62345-133">Hello vyhledávacího pole zadejte **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="62345-133">In hello search box, type **Syncplicity**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. <span data-ttu-id="62345-135">Na panelu výsledků hello vyberte **Syncplicity**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="62345-135">In hello results panel, select **Syncplicity**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="62345-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="62345-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="62345-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Syncplicity podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="62345-138">In this section, you configure and test Azure AD single sign-on with Syncplicity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="62345-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Syncplicity je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62345-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Syncplicity is tooa user in Azure AD.</span></span> <span data-ttu-id="62345-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Syncplicity musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="62345-140">In other words, a link relationship between an Azure AD user and hello related user in Syncplicity needs toobe established.</span></span>

<span data-ttu-id="62345-141">V Syncplicity, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="62345-141">In Syncplicity, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="62345-142">tooconfigure a testu Azure AD jednotné přihlašování s Syncplicity, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="62345-142">tooconfigure and test Azure AD single sign-on with Syncplicity, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="62345-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="62345-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="62345-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="62345-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="62345-145">**[Vytvoření zkušebního uživatele Syncplicity](#creating-a-syncplicity-test-user)**  -toohave protějšek Britta Simon v Syncplicity, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="62345-145">**[Creating a Syncplicity test user](#creating-a-syncplicity-test-user)** - toohave a counterpart of Britta Simon in Syncplicity that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="62345-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="62345-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="62345-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="62345-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="62345-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="62345-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="62345-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Syncplicity.</span><span class="sxs-lookup"><span data-stu-id="62345-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Syncplicity application.</span></span>

<span data-ttu-id="62345-150">**tooconfigure Azure AD jednotné přihlašování s Syncplicity, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="62345-150">**tooconfigure Azure AD single sign-on with Syncplicity, perform hello following steps:**</span></span>

1. <span data-ttu-id="62345-151">V portálu Azure, na hello hello **Syncplicity** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="62345-151">In hello Azure portal, on hello **Syncplicity** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="62345-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="62345-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. <span data-ttu-id="62345-155">Na hello **Syncplicity domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="62345-155">On hello **Syncplicity Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    <span data-ttu-id="62345-157">a.</span><span class="sxs-lookup"><span data-stu-id="62345-157">a.</span></span> <span data-ttu-id="62345-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.syncplicity.com`</span><span class="sxs-lookup"><span data-stu-id="62345-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.syncplicity.com`</span></span>

    <span data-ttu-id="62345-159">b.</span><span class="sxs-lookup"><span data-stu-id="62345-159">b.</span></span> <span data-ttu-id="62345-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.syncplicity.com/sp`</span><span class="sxs-lookup"><span data-stu-id="62345-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.syncplicity.com/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="62345-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="62345-161">These values are not real.</span></span> <span data-ttu-id="62345-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="62345-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="62345-163">Obraťte se na [tým podpory Syncplicity klienta](https://www.syncplicity.com/contact-us) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="62345-163">Contact [Syncplicity Client support team](https://www.syncplicity.com/contact-us) tooget these values.</span></span> 
 

4. <span data-ttu-id="62345-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="62345-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. <span data-ttu-id="62345-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="62345-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="62345-168">Na hello **Syncplicity konfigurace** klikněte na tlačítko **konfigurace Syncplicity** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="62345-168">On hello **Syncplicity Configuration** section, click **Configure Syncplicity** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="62345-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="62345-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. <span data-ttu-id="62345-171">Přihlaste se tooyour **Syncplicity** klienta.</span><span class="sxs-lookup"><span data-stu-id="62345-171">Sign in tooyour **Syncplicity** tenant.</span></span>

8. <span data-ttu-id="62345-172">V nabídce hello hello nahoře, klikněte na tlačítko **správce**, vyberte **nastavení**a potom klikněte na **vlastní domény a jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="62345-172">In hello menu on hello top, click **admin**, select **settings**, and then click **Custom domain and single sign-on**.</span></span>
   
    <span data-ttu-id="62345-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span><span class="sxs-lookup"><span data-stu-id="62345-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span></span>

9. <span data-ttu-id="62345-174">Na hello **jednotné přihlašování (SSO)** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="62345-174">On hello **Single Sign-On (SSO)** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="62345-175">![Jednotné přihlašování \(jednotného přihlašování\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span><span class="sxs-lookup"><span data-stu-id="62345-175">![Single Sign-On \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span></span>   

    <span data-ttu-id="62345-176">a.</span><span class="sxs-lookup"><span data-stu-id="62345-176">a.</span></span> <span data-ttu-id="62345-177">V hello **vlastní domény** textovému poli, typ hello název vaší domény.</span><span class="sxs-lookup"><span data-stu-id="62345-177">In hello **Custom Domain** textbox, type hello name of your domain.</span></span>
  
    <span data-ttu-id="62345-178">b.</span><span class="sxs-lookup"><span data-stu-id="62345-178">b.</span></span> <span data-ttu-id="62345-179">Vyberte **povoleno** jako **jednotné přihlašování stav**.</span><span class="sxs-lookup"><span data-stu-id="62345-179">Select **Enabled** as **Single Sign-On Status**.</span></span>

    <span data-ttu-id="62345-180">c.</span><span class="sxs-lookup"><span data-stu-id="62345-180">c.</span></span> <span data-ttu-id="62345-181">V hello **Entity Id** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="62345-181">In hello **Entity Id** textbox, Paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="62345-182">d.</span><span class="sxs-lookup"><span data-stu-id="62345-182">d.</span></span> <span data-ttu-id="62345-183">V hello **přihlašovací adresa URL stránky** textovému poli, vložte hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="62345-183">In hello **Sign-in page URL** textbox, Paste hello **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="62345-184">e.</span><span class="sxs-lookup"><span data-stu-id="62345-184">e.</span></span> <span data-ttu-id="62345-185">V hello **adresa URL odhlašovací stránky** textovému poli, vložte hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="62345-185">In hello **Logout page URL** textbox, Paste hello **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="62345-186">f.</span><span class="sxs-lookup"><span data-stu-id="62345-186">f.</span></span> <span data-ttu-id="62345-187">V **certifikát zprostředkovatele Identity**, klikněte na tlačítko **zvolte soubor**a pak nahrajte hello certifikát, který jste si stáhli z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="62345-187">In **Identity Provider Certificate**, click **Choose file**, and then upload hello certificate which you have downloaded from hello Azure portal.</span></span> 

    <span data-ttu-id="62345-188">g.</span><span class="sxs-lookup"><span data-stu-id="62345-188">g.</span></span> <span data-ttu-id="62345-189">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="62345-189">Click **SAVE CHANGES**.</span></span>

> [!TIP]
> <span data-ttu-id="62345-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="62345-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="62345-191">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="62345-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="62345-192">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="62345-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="62345-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="62345-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="62345-194">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="62345-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="62345-196">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="62345-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="62345-197">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="62345-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="62345-199">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="62345-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="62345-201">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="62345-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="62345-203">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="62345-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="62345-205">a.</span><span class="sxs-lookup"><span data-stu-id="62345-205">a.</span></span> <span data-ttu-id="62345-206">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="62345-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="62345-207">b.</span><span class="sxs-lookup"><span data-stu-id="62345-207">b.</span></span> <span data-ttu-id="62345-208">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="62345-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="62345-209">c.</span><span class="sxs-lookup"><span data-stu-id="62345-209">c.</span></span> <span data-ttu-id="62345-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="62345-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="62345-211">d.</span><span class="sxs-lookup"><span data-stu-id="62345-211">d.</span></span> <span data-ttu-id="62345-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="62345-212">Click **Create**.</span></span>
 
### <a name="creating-a-syncplicity-test-user"></a><span data-ttu-id="62345-213">Vytvoření zkušebního uživatele Syncplicity</span><span class="sxs-lookup"><span data-stu-id="62345-213">Creating a Syncplicity test user</span></span>
<span data-ttu-id="62345-214">Pro AAD uživatelé toobe možné toosign v musí být zřízená tooSyncplicity aplikace.</span><span class="sxs-lookup"><span data-stu-id="62345-214">For AAD users toobe able toosign in, they must be provisioned tooSyncplicity application.</span></span> <span data-ttu-id="62345-215">Tato část popisuje, jak toocreate AAD uživatelských účtů v Syncplicity.</span><span class="sxs-lookup"><span data-stu-id="62345-215">This section describes how toocreate AAD user accounts in Syncplicity.</span></span>

<span data-ttu-id="62345-216">**tooprovision tooSyncplicity účet uživatele, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="62345-216">**tooprovision a user account tooSyncplicity, perform hello following steps:**</span></span>

1. <span data-ttu-id="62345-217">Přihlaste se tooyour **Syncplicity** klienta (například: `https://company.Syncplicity.com`).</span><span class="sxs-lookup"><span data-stu-id="62345-217">Log in tooyour **Syncplicity** tenant (for example: `https://company.Syncplicity.com`).</span></span>

2. <span data-ttu-id="62345-218">Klikněte na tlačítko **správce** a vyberte **uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="62345-218">Click **admin** and select **user accounts**.</span></span>

3. <span data-ttu-id="62345-219">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="62345-219">Click **ADD A USER**.</span></span>
   
    <span data-ttu-id="62345-220">![Správa uživatelů](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="62345-220">![Manage Users](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Manage Users")</span></span>

4. <span data-ttu-id="62345-221">Typ hello **e-mailových adres** účtu AAD, tooprovision, vyberte **uživatele** jako **Role**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="62345-221">Type hello **Email addressess** of an AAD account you want tooprovision, select **User** as **Role**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="62345-222">![Informace o účtu](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "účet informace")</span><span class="sxs-lookup"><span data-stu-id="62345-222">![Account Information](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "Account Information")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="62345-223">Držitel účtu AAD Hello získá včetně tooconfirm odkaz e-mailu a aktivaci hello účtu.</span><span class="sxs-lookup"><span data-stu-id="62345-223">hello AAD account holder  gets an email including a link tooconfirm and activate hello account.</span></span> 
    > 

5. <span data-ttu-id="62345-224">Vyberte skupinu ve vaší společnosti, nový uživatel by měl stane členem, a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="62345-224">Select a group in your company that your new user should become a member of, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="62345-225">![Členství ve skupinách](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "členství ve skupinách")</span><span class="sxs-lookup"><span data-stu-id="62345-225">![Group Membership](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "Group Membership")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="62345-226">Pokud nebyly nalezeny žádné skupiny uvedeny, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="62345-226">If there are no groups listed, click **NEXT**.</span></span> 
    > 

6. <span data-ttu-id="62345-227">Vyberte složky hello chcete vytvořit tooplace pod kontrolou společnosti Syncplicity počítače hello uživatele a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="62345-227">Select hello folders you would like tooplace under Syncplicity’s control on hello user’s computer, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="62345-228">![Složky Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity složek")</span><span class="sxs-lookup"><span data-stu-id="62345-228">![Syncplicity Folders](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity Folders")</span></span>

>[!NOTE]
><span data-ttu-id="62345-229">Můžete použít všechny ostatní Syncplicity uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Syncplicity tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="62345-229">You can use any other Syncplicity user account creation tools or APIs provided by Syncplicity tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="62345-230">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="62345-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="62345-231">V této části povolíte tak, že udělíte přístup tooSyncplicity toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="62345-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSyncplicity.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="62345-233">**tooassign Britta Simon tooSyncplicity, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="62345-233">**tooassign Britta Simon tooSyncplicity, perform hello following steps:**</span></span>

1. <span data-ttu-id="62345-234">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="62345-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="62345-236">V seznamu aplikace hello vyberte **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="62345-236">In hello applications list, select **Syncplicity**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. <span data-ttu-id="62345-238">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="62345-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="62345-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="62345-240">Click **Add** button.</span></span> <span data-ttu-id="62345-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="62345-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="62345-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="62345-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="62345-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="62345-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="62345-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="62345-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="62345-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="62345-246">Testing single sign-on</span></span>

<span data-ttu-id="62345-247">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="62345-247">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="62345-248">Když kliknete na dlaždici Syncplicity hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Syncplicity aplikace.</span><span class="sxs-lookup"><span data-stu-id="62345-248">When you click hello Syncplicity tile in hello Access Panel, you should get automatically signed-on tooyour Syncplicity application.</span></span>
## <a name="additional-resources"></a><span data-ttu-id="62345-249">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="62345-249">Additional resources</span></span>

* [<span data-ttu-id="62345-250">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="62345-250">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="62345-251">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="62345-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

