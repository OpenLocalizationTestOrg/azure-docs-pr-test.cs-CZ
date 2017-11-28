---
title: "Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML pro soutoku podle řešení GmbH | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování SAML pro soutoku podle řešení GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: fe50636709857ec49023e24bdc8c6cd8c58e3c7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a><span data-ttu-id="3b990-103">Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML pro soutoku podle řešení GmbH</span><span class="sxs-lookup"><span data-stu-id="3b990-103">Tutorial: Azure Active Directory integration with SAML SSO for Confluence by resolution GmbH</span></span>

<span data-ttu-id="3b990-104">V tomto kurzu zjistíte, jak toointegrate jednotné přihlašování SAML pro soutoku podle řešení GmbH službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3b990-104">In this tutorial, you learn how toointegrate SAML SSO for Confluence by resolution GmbH with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3b990-105">Integrace jednotné přihlašování SAML pro soutoku podle řešení GmbH s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3b990-105">Integrating SAML SSO for Confluence by resolution GmbH with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3b990-106">Můžete řídit ve službě Azure AD, který má přístup tooSAML jednotné přihlašování pro soutoku podle řešení GmbH</span><span class="sxs-lookup"><span data-stu-id="3b990-106">You can control in Azure AD who has access tooSAML SSO for Confluence by resolution GmbH</span></span>
- <span data-ttu-id="3b990-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSAML jednotné přihlašování pro soutoku podle řešení GmbH (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b990-107">You can enable your users tooautomatically get signed-on tooSAML SSO for Confluence by resolution GmbH (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3b990-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3b990-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3b990-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3b990-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b990-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3b990-110">Prerequisites</span></span>

<span data-ttu-id="3b990-111">Integrace služby Azure AD pomocí jednotného přihlašování SAML pro soutoku podle řešení GmbH tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3b990-111">tooconfigure Azure AD integration with SAML SSO for Confluence by resolution GmbH, you need hello following items:</span></span>

- <span data-ttu-id="3b990-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b990-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3b990-113">Jednotné přihlašování SAML pro soutoku podle řešení GmbH jednotné přihlašování v předplatném povolené</span><span class="sxs-lookup"><span data-stu-id="3b990-113">A SAML SSO for Confluence by resolution GmbH single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3b990-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b990-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3b990-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3b990-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3b990-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3b990-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3b990-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3b990-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3b990-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3b990-118">Scenario description</span></span>
<span data-ttu-id="3b990-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b990-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3b990-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3b990-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3b990-121">Přidání jednotné přihlašování SAML pro soutoku podle řešení GmbH z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3b990-121">Adding SAML SSO for Confluence by resolution GmbH from hello gallery</span></span>
2. <span data-ttu-id="3b990-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b990-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-hello-gallery"></a><span data-ttu-id="3b990-123">Přidání jednotné přihlašování SAML pro soutoku podle řešení GmbH z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3b990-123">Adding SAML SSO for Confluence by resolution GmbH from hello gallery</span></span>

<span data-ttu-id="3b990-124">tooconfigure hello integrace přihlašování SAML pro soutoku podle řešení GmbH do Azure AD, musíte pro soutoku podle řešení GmbH hello Galerie tooyour seznamu spravovaných aplikací SaaS tooadd jednotné přihlašování SAML.</span><span class="sxs-lookup"><span data-stu-id="3b990-124">tooconfigure hello integration of SAML SSO for Confluence by resolution GmbH into Azure AD, you need tooadd SAML SSO for Confluence by resolution GmbH from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3b990-125">**tooadd jednotné přihlašování SAML pro soutoku podle řešení GmbH z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3b990-125">**tooadd SAML SSO for Confluence by resolution GmbH from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b990-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3b990-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3b990-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3b990-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3b990-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3b990-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3b990-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b990-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3b990-133">Hello vyhledávacího pole zadejte **jednotné přihlašování SAML pro soutoku podle řešení GmbH**.</span><span class="sxs-lookup"><span data-stu-id="3b990-133">In hello search box, type **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

5. <span data-ttu-id="3b990-135">Na panelu výsledků hello vyberte **jednotné přihlašování SAML pro soutoku podle řešení GmbH**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b990-135">In hello results panel, select **SAML SSO for Confluence by resolution GmbH**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3b990-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b990-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="3b990-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML pro soutoku pomocí řešení, které GmbH podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3b990-138">In this section, you configure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3b990-139">Azure AD pro toowork jeden přihlašování, musí tooknow uživatele co hello protějškem v jednotné přihlašování SAML pro soutoku podle řešení GmbH je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3b990-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAML SSO for Confluence by resolution GmbH is tooa user in Azure AD.</span></span> <span data-ttu-id="3b990-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a hello související uživatele v jednotné přihlašování SAML pro soutoku podle řešení GmbH musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="3b990-140">In other words, a link relationship between an Azure AD user and hello related user in SAML SSO for Confluence by resolution GmbH needs toobe established.</span></span>

<span data-ttu-id="3b990-141">V jednotné přihlašování SAML pro soutoku podle řešení GmbH, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="3b990-141">In SAML SSO for Confluence by resolution GmbH, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3b990-142">tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML soutoku podle řešení GmbH, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="3b990-142">tooconfigure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3b990-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="3b990-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3b990-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b990-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3b990-145">**[Vytváření jednotné přihlašování SAML pro soutoku uživatelem testovací GmbH řešení](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  -toohave protějšek Britta Simon v jednotné přihlašování SAML pro soutoku podle řešení GmbH, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="3b990-145">**[Creating a SAML SSO for Confluence by resolution GmbH test user](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)** - toohave a counterpart of Britta Simon in SAML SSO for Confluence by resolution GmbH that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3b990-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3b990-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3b990-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="3b990-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3b990-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b990-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3b990-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší jednotné přihlašování SAML pro soutoku podle řešení GmbH aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b990-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAML SSO for Confluence by resolution GmbH application.</span></span>

<span data-ttu-id="3b990-150">**tooconfigure Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML pro soutoku podle řešení GmbH, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3b990-150">**tooconfigure Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b990-151">V portálu Azure, na hello hello **jednotné přihlašování SAML pro soutoku podle řešení GmbH** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3b990-151">In hello Azure portal, on hello **SAML SSO for Confluence by resolution GmbH** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3b990-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3b990-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

3. <span data-ttu-id="3b990-155">Na hello **jednotné přihlašování SAML soutoku podle řešení GmbH domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="3b990-155">On hello **SAML SSO for Confluence by resolution GmbH Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    <span data-ttu-id="3b990-157">a.</span><span class="sxs-lookup"><span data-stu-id="3b990-157">a.</span></span> <span data-ttu-id="3b990-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="3b990-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

    <span data-ttu-id="3b990-159">b.</span><span class="sxs-lookup"><span data-stu-id="3b990-159">b.</span></span> <span data-ttu-id="3b990-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="3b990-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

4. <span data-ttu-id="3b990-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="3b990-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="3b990-162">Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="3b990-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    <span data-ttu-id="3b990-164">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="3b990-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="3b990-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3b990-165">These values are not real.</span></span> <span data-ttu-id="3b990-166">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="3b990-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="3b990-167">Obraťte se na [tým podpory jednotné přihlašování SAML pro soutoku podle řešení GmbH klienta](https://www.resolution.de/go/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3b990-167">Contact [SAML SSO for Confluence by resolution GmbH Client support team](https://www.resolution.de/go/support) tooget these values.</span></span> 

5. <span data-ttu-id="3b990-168">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3b990-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

6. <span data-ttu-id="3b990-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b990-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_400.png)  
    
7. <span data-ttu-id="3b990-172">V okně prohlížeče jiných webových přihlásit tooyour **jednotné přihlašování SAML pro soutoku pomocí portálu pro správu GmbH řešení** jako správce.</span><span class="sxs-lookup"><span data-stu-id="3b990-172">In a different web browser window, log in tooyour **SAML SSO for Confluence by resolution GmbH admin portal** as an administrator.</span></span>

8. <span data-ttu-id="3b990-173">Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **doplňky**.</span><span class="sxs-lookup"><span data-stu-id="3b990-173">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon1.png)

9. <span data-ttu-id="3b990-175">Jste přesměrovaného tooAdministrator stránky.</span><span class="sxs-lookup"><span data-stu-id="3b990-175">You are redirected tooAdministrator Access page.</span></span> <span data-ttu-id="3b990-176">Zadejte hello heslo a klikněte na tlačítko **potvrdit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b990-176">Enter hello password and click **Confirm** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon2.png)

10. <span data-ttu-id="3b990-178">V části **ATLASSIAN MARKETPLACE** , klikněte na **najít nové rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="3b990-178">Under **ATLASSIAN MARKETPLACE** tab, click **Find new add-ons**.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon.png)

11. <span data-ttu-id="3b990-180">Hledání **SAML jednotné přihlašování na (SSO) pro soutoku** a klikněte na tlačítko **nainstalovat** tooinstall tlačítko hello nové zásuvný modul SAML.</span><span class="sxs-lookup"><span data-stu-id="3b990-180">Search **SAML Single Sign On (SSO) for Confluence** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon7.png)

12. <span data-ttu-id="3b990-182">Instalace modulu plug-in Hello se spustí.</span><span class="sxs-lookup"><span data-stu-id="3b990-182">hello plugin installation will start.</span></span> <span data-ttu-id="3b990-183">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="3b990-183">Click **Close**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon8.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon9.png)

13. <span data-ttu-id="3b990-186">Klikněte na **Manage** (Spravovat).</span><span class="sxs-lookup"><span data-stu-id="3b990-186">Click **Manage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon10.png)
    
14. <span data-ttu-id="3b990-188">Klikněte na tlačítko **konfigurace** tooconfigure hello nového modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="3b990-188">Click **Configure** tooconfigure hello new plugin.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon11.png)

15. <span data-ttu-id="3b990-190">Tento nový modul plug-in můžete také najít v části **uživatelů a zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="3b990-190">This new plugin can also be found under **USERS & SECURITY** tab.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon3.png)
    
16. <span data-ttu-id="3b990-192">Na **konfigurace modulu plug-in SingleSignOn SAML** klikněte na tlačítko **přidat další zprostředkovatele Identity** tlačítko tooconfigure hello nastavení zprostředkovatele Identity.</span><span class="sxs-lookup"><span data-stu-id="3b990-192">On **SAML SingleSignOn Plugin Configuration** page, click **Add additional Identity Provider** button tooconfigure hello settings of Identity Provider.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon4.png)

17. <span data-ttu-id="3b990-194">Proveďte následující kroky na této stránce:</span><span class="sxs-lookup"><span data-stu-id="3b990-194">Perform following steps on this page:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon5.png)
 
    <span data-ttu-id="3b990-196">a.</span><span class="sxs-lookup"><span data-stu-id="3b990-196">a.</span></span> <span data-ttu-id="3b990-197">Přidat **název** z hello zprostředkovatele Identity (např. Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3b990-197">Add **Name** of hello Identity Provider (e.g Azure AD).</span></span>
    
    <span data-ttu-id="3b990-198">b.</span><span class="sxs-lookup"><span data-stu-id="3b990-198">b.</span></span> <span data-ttu-id="3b990-199">Přidat **popis** z hello zprostředkovatele Identity (např. Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3b990-199">Add **Description** of hello Identity Provider (e.g Azure AD).</span></span>

    <span data-ttu-id="3b990-200">c.</span><span class="sxs-lookup"><span data-stu-id="3b990-200">c.</span></span> <span data-ttu-id="3b990-201">Klikněte na tlačítko **XML** a vyberte hello **Metadata** soubor, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b990-201">Click **XML** and select hello **Metadata** file that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="3b990-202">d.</span><span class="sxs-lookup"><span data-stu-id="3b990-202">d.</span></span> <span data-ttu-id="3b990-203">Klikněte na tlačítko **zatížení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b990-203">Click **Load** button.</span></span>

    <span data-ttu-id="3b990-204">e.</span><span class="sxs-lookup"><span data-stu-id="3b990-204">e.</span></span> <span data-ttu-id="3b990-205">Přečte hello IdP metadata a naplní pole hello jako zvýrazněná hello snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="3b990-205">It reads hello IdP metadata and populates hello fields as highlighted in hello screenshot.</span></span>   
18. <span data-ttu-id="3b990-206">Klikněte na tlačítko **uložit nastavení** tlačítko toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="3b990-206">Click **Save settings** button toosave hello settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/addon6.png)

> [!TIP]
> <span data-ttu-id="3b990-208">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="3b990-208">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3b990-209">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="3b990-209">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3b990-210">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3b990-210">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3b990-211">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b990-211">Creating an Azure AD test user</span></span>
<span data-ttu-id="3b990-212">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="3b990-212">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3b990-214">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3b990-214">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b990-215">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3b990-215">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3b990-217">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3b990-217">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3b990-219">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="3b990-219">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3b990-221">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3b990-221">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3b990-223">a.</span><span class="sxs-lookup"><span data-stu-id="3b990-223">a.</span></span> <span data-ttu-id="3b990-224">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3b990-224">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3b990-225">b.</span><span class="sxs-lookup"><span data-stu-id="3b990-225">b.</span></span> <span data-ttu-id="3b990-226">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3b990-226">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3b990-227">c.</span><span class="sxs-lookup"><span data-stu-id="3b990-227">c.</span></span> <span data-ttu-id="3b990-228">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3b990-228">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3b990-229">d.</span><span class="sxs-lookup"><span data-stu-id="3b990-229">d.</span></span> <span data-ttu-id="3b990-230">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3b990-230">Click **Create**.</span></span>
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a><span data-ttu-id="3b990-231">Vytváření jednotné přihlašování SAML pro soutoku řešení GmbH testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="3b990-231">Creating a SAML SSO for Confluence by resolution GmbH test user</span></span>

<span data-ttu-id="3b990-232">Uživatelé toolog tooenable Azure AD v tooSAML jednotné přihlašování pro soutoku podle řešení GmbH, se musí být zřízená do jednotné přihlašování SAML pro soutoku podle řešení GmbH.</span><span class="sxs-lookup"><span data-stu-id="3b990-232">tooenable Azure AD users toolog in tooSAML SSO for Confluence by resolution GmbH, they must be provisioned into SAML SSO for Confluence by resolution GmbH.</span></span>  
<span data-ttu-id="3b990-233">V jednotné přihlašování SAML pro soutoku podle řešení GmbH zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="3b990-233">In SAML SSO for Confluence by resolution GmbH, provisioning is a manual task.</span></span>

<span data-ttu-id="3b990-234">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3b990-234">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b990-235">Přihlaste se tooyour jednotné přihlašování SAML pro soutoku řešení GmbH společnosti lokalita jako správce.</span><span class="sxs-lookup"><span data-stu-id="3b990-235">Log in tooyour SAML SSO for Confluence by resolution GmbH company site as an administrator.</span></span>

2. <span data-ttu-id="3b990-236">Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="3b990-236">Hover on cog and click hello **User management**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-samlssoconfluence-tutorial/user1.png) 

3. <span data-ttu-id="3b990-238">V části uživatelé klikněte na tlačítko **přidat uživatele** kartě. Na hello **"Přidat uživatele"** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3b990-238">Under Users section, click **Add users** tab. On hello **“Add a User”** dialog page, perform hello following steps:</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-samlssoconfluence-tutorial/user2.png) 

    <span data-ttu-id="3b990-240">a.</span><span class="sxs-lookup"><span data-stu-id="3b990-240">a.</span></span> <span data-ttu-id="3b990-241">V hello **uživatelské jméno** textovému poli, typ hello e-mailu uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b990-241">In hello **Username** textbox, type hello email of user like Britta Simon.</span></span>

    <span data-ttu-id="3b990-242">b.</span><span class="sxs-lookup"><span data-stu-id="3b990-242">b.</span></span> <span data-ttu-id="3b990-243">V hello **úplný název** textovému poli, úplný název typu hello uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b990-243">In hello **Full Name** textbox, type hello full name of user like Britta Simon.</span></span>

    <span data-ttu-id="3b990-244">c.</span><span class="sxs-lookup"><span data-stu-id="3b990-244">c.</span></span> <span data-ttu-id="3b990-245">V hello **e-mailu** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="3b990-245">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="3b990-246">d.</span><span class="sxs-lookup"><span data-stu-id="3b990-246">d.</span></span> <span data-ttu-id="3b990-247">V hello **heslo** textovému poli, zadejte hello heslo pro Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b990-247">In hello **Password** textbox, type hello password for Britta Simon.</span></span>

    <span data-ttu-id="3b990-248">e.</span><span class="sxs-lookup"><span data-stu-id="3b990-248">e.</span></span> <span data-ttu-id="3b990-249">Klikněte na tlačítko **Potvrdit heslo** zadejte znovu heslo hello.</span><span class="sxs-lookup"><span data-stu-id="3b990-249">Click **Confirm Password** reenter hello password.</span></span>
    
    <span data-ttu-id="3b990-250">f.</span><span class="sxs-lookup"><span data-stu-id="3b990-250">f.</span></span> <span data-ttu-id="3b990-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b990-251">Click **Add** button.</span></span>    

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3b990-252">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="3b990-252">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3b990-253">V této části povolíte tak, že udělíte přístup tooSAML jednotné přihlašování pro soutoku podle řešení GmbH Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3b990-253">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAML SSO for Confluence by resolution GmbH.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3b990-255">**tooassign Britta Simon tooSAML jednotné přihlašování pro soutoku podle řešení GmbH, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3b990-255">**tooassign Britta Simon tooSAML SSO for Confluence by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b990-256">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3b990-256">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3b990-258">V seznamu aplikace hello vyberte **jednotné přihlašování SAML pro soutoku podle řešení GmbH**.</span><span class="sxs-lookup"><span data-stu-id="3b990-258">In hello applications list, select **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

3. <span data-ttu-id="3b990-260">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3b990-260">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3b990-262">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b990-262">Click **Add** button.</span></span> <span data-ttu-id="3b990-263">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b990-263">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3b990-265">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="3b990-265">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3b990-266">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b990-266">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3b990-267">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b990-267">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3b990-268">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b990-268">Testing single sign-on</span></span>

<span data-ttu-id="3b990-269">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3b990-269">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3b990-270">Po kliknutí na tlačítko hello jednotné přihlašování SAML pro soutoku pomocí řešení GmbH dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour jednotné přihlašování SAML pro soutoku podle řešení GmbH aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b990-270">When you click hello SAML SSO for Confluence by resolution GmbH tile in hello Access Panel, you should get automatically signed-on tooyour SAML SSO for Confluence by resolution GmbH application.</span></span>
<span data-ttu-id="3b990-271">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3b990-271">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3b990-272">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3b990-272">Additional resources</span></span>

* [<span data-ttu-id="3b990-273">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b990-273">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3b990-274">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3b990-274">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_203.png

