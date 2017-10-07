---
title: "Kurz: Azure Active Directory integrace s hostované grafitová | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a hostované grafitová."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: d8914f6417ba8fbdef1a48e1b36635200ba130d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a><span data-ttu-id="c09e1-103">Kurz: Azure Active Directory integrace s hostované grafitová</span><span class="sxs-lookup"><span data-stu-id="c09e1-103">Tutorial: Azure Active Directory integration with Hosted Graphite</span></span>

<span data-ttu-id="c09e1-104">V tomto kurzu zjistíte, jak toointegrate grafitová hostovaná v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c09e1-104">In this tutorial, you learn how toointegrate Hosted Graphite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c09e1-105">Integrace hostované grafitová s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c09e1-105">Integrating Hosted Graphite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c09e1-106">Můžete řídit ve službě Azure AD, který má přístup tooHosted grafitová</span><span class="sxs-lookup"><span data-stu-id="c09e1-106">You can control in Azure AD who has access tooHosted Graphite</span></span>
- <span data-ttu-id="c09e1-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHosted grafitová (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c09e1-107">You can enable your users tooautomatically get signed-on tooHosted Graphite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c09e1-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c09e1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c09e1-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c09e1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c09e1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c09e1-110">Prerequisites</span></span>

<span data-ttu-id="c09e1-111">tooconfigure integrace Azure AD s hostované grafitová, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c09e1-111">tooconfigure Azure AD integration with Hosted Graphite, you need hello following items:</span></span>

- <span data-ttu-id="c09e1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c09e1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c09e1-113">Hostované grafitová jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c09e1-113">A Hosted Graphite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c09e1-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c09e1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c09e1-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c09e1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c09e1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c09e1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c09e1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c09e1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c09e1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c09e1-118">Scenario description</span></span>
<span data-ttu-id="c09e1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c09e1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c09e1-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c09e1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c09e1-121">Přidání hostované grafitová z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c09e1-121">Adding Hosted Graphite from hello gallery</span></span>
2. <span data-ttu-id="c09e1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c09e1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hosted-graphite-from-hello-gallery"></a><span data-ttu-id="c09e1-123">Přidání hostované grafitová z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c09e1-123">Adding Hosted Graphite from hello gallery</span></span>
<span data-ttu-id="c09e1-124">tooconfigure hello integrace hostované grafitu do Azure AD, je nutné tooadd Hosted grafitová hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c09e1-124">tooconfigure hello integration of Hosted Graphite into Azure AD, you need tooadd Hosted Graphite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c09e1-125">**tooadd Hosted grafitová z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c09e1-125">**tooadd Hosted Graphite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c09e1-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c09e1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c09e1-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c09e1-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c09e1-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c09e1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c09e1-133">Hello vyhledávacího pole zadejte **hostované grafitová**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-133">In hello search box, type **Hosted Graphite**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

5. <span data-ttu-id="c09e1-135">Na panelu výsledků hello vyberte **hostované grafitová**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c09e1-135">In hello results panel, select **Hosted Graphite**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c09e1-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c09e1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c09e1-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s hostované grafitová podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c09e1-138">In this section, you configure and test Azure AD single sign-on with Hosted Graphite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c09e1-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v hostované grafitová je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c09e1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Hosted Graphite is tooa user in Azure AD.</span></span> <span data-ttu-id="c09e1-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v hostované grafitová musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c09e1-140">In other words, a link relationship between an Azure AD user and hello related user in Hosted Graphite needs toobe established.</span></span>

<span data-ttu-id="c09e1-141">V hostované grafitová přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c09e1-141">In Hosted Graphite, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c09e1-142">tooconfigure a testu Azure AD jednotné přihlašování s hostované grafitová, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c09e1-142">tooconfigure and test Azure AD single sign-on with Hosted Graphite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c09e1-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c09e1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c09e1-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c09e1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c09e1-145">**[Vytvoření zkušebního uživatele hostované grafitová](#creating-a-hosted-graphite-test-user)**  -toohave protějšek Britta Simon v hostované grafitová, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c09e1-145">**[Creating a Hosted Graphite test user](#creating-a-hosted-graphite-test-user)** - toohave a counterpart of Britta Simon in Hosted Graphite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c09e1-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c09e1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c09e1-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c09e1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c09e1-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c09e1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c09e1-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci hostované grafitová.</span><span class="sxs-lookup"><span data-stu-id="c09e1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Hosted Graphite application.</span></span>

<span data-ttu-id="c09e1-150">**tooconfigure Azure AD jednotné přihlašování s hostované grafitová proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c09e1-150">**tooconfigure Azure AD single sign-on with Hosted Graphite, perform hello following steps:**</span></span>

1. <span data-ttu-id="c09e1-151">V portálu Azure, na hello hello **hostované grafitová** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-151">In hello Azure portal, on hello **Hosted Graphite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c09e1-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c09e1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

3. <span data-ttu-id="c09e1-155">Na hello **hostované grafitová domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c09e1-155">On hello **Hosted Graphite Domain and URLs** section, if you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    <span data-ttu-id="c09e1-157">a.</span><span class="sxs-lookup"><span data-stu-id="c09e1-157">a.</span></span> <span data-ttu-id="c09e1-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.hostedgraphite.com/metadata/<user id>`</span><span class="sxs-lookup"><span data-stu-id="c09e1-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.hostedgraphite.com/metadata/<user id>`</span></span>

    <span data-ttu-id="c09e1-159">b.</span><span class="sxs-lookup"><span data-stu-id="c09e1-159">b.</span></span> <span data-ttu-id="c09e1-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.hostedgraphite.com/complete/saml/<user id>`</span><span class="sxs-lookup"><span data-stu-id="c09e1-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.hostedgraphite.com/complete/saml/<user id>`</span></span>

4. <span data-ttu-id="c09e1-161">Na hello **hostované grafitová domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c09e1-161">On hello **Hosted Graphite Domain and URLs** section, if you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    <span data-ttu-id="c09e1-163">a.</span><span class="sxs-lookup"><span data-stu-id="c09e1-163">a.</span></span> <span data-ttu-id="c09e1-164">Klikněte na hello **zobrazit upřesňující nastavení adresy URL** možnost</span><span class="sxs-lookup"><span data-stu-id="c09e1-164">Click on hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="c09e1-165">b.</span><span class="sxs-lookup"><span data-stu-id="c09e1-165">b.</span></span> <span data-ttu-id="c09e1-166">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.hostedgraphite.com/login/saml/<user id>/`</span><span class="sxs-lookup"><span data-stu-id="c09e1-166">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://www.hostedgraphite.com/login/saml/<user id>/`</span></span>   

    > [!NOTE] 
    > <span data-ttu-id="c09e1-167">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c09e1-167">Please note that these are not hello real values.</span></span> <span data-ttu-id="c09e1-168">Máte tooupdate tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="c09e1-168">You have tooupdate these values with hello actual Identifier, Reply URL and Sign On URL.</span></span> <span data-ttu-id="c09e1-169">tooget tyto hodnoty, můžete přejít tooAccess -> Nastavení SAML na straně aplikace nebo kontaktujte [tým podpory hostované grafitová](mailto:help@hostedgraphite.com).</span><span class="sxs-lookup"><span data-stu-id="c09e1-169">tooget these values, you can go tooAccess->SAML setup on your Application side or Contact [Hosted Graphite support team](mailto:help@hostedgraphite.com).</span></span>
    >
 
5. <span data-ttu-id="c09e1-170">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c09e1-170">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

6. <span data-ttu-id="c09e1-172">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c09e1-172">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="c09e1-174">Na hello **hostované konfigurace grafitová** klikněte na tlačítko **konfigurace hostované grafitová** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c09e1-174">On hello **Hosted Graphite Configuration** section, click **Configure Hosted Graphite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c09e1-175">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c09e1-175">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

8. <span data-ttu-id="c09e1-177">Klient hostované grafitová tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="c09e1-177">Sign-on tooyour Hosted Graphite tenant as an administrator.</span></span>

9. <span data-ttu-id="c09e1-178">Přejděte toohello **stránce instalace SAML** bočním panelu hello (**přístup -> Nastavení SAML**).</span><span class="sxs-lookup"><span data-stu-id="c09e1-178">Go toohello **SAML Setup page** in hello sidebar (**Access -> SAML Setup**).</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

10. <span data-ttu-id="c09e1-180">Potvrďte tyto adresy URL neodpovídá konfiguraci provést v hello **hostované grafitová domény a adresy URL** části hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c09e1-180">Confirm these URls match your configuration done on hello **Hosted Graphite Domain and URLs** section of hello Azure portal.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

11. <span data-ttu-id="c09e1-182">V **Entity nebo ID vystavitele** a **adresu URL pro přihlášení SSO** textových polí, vložte hodnotu hello **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** které jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c09e1-182">In  **Entity or Issuer ID** and **SSO Login URL** textboxes, paste hello value of **SAML Entity ID** and **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

12. <span data-ttu-id="c09e1-184">Vyberte "**jen pro čtení**" jako **výchozí Role uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-184">Select "**Read-only**" as **Default User Role**.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

13. <span data-ttu-id="c09e1-186">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c09e1-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

14. <span data-ttu-id="c09e1-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c09e1-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="c09e1-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c09e1-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c09e1-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c09e1-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c09e1-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c09e1-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c09e1-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c09e1-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="c09e1-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c09e1-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c09e1-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c09e1-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c09e1-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c09e1-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c09e1-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c09e1-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c09e1-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c09e1-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c09e1-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c09e1-204">a.</span><span class="sxs-lookup"><span data-stu-id="c09e1-204">a.</span></span> <span data-ttu-id="c09e1-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c09e1-206">b.</span><span class="sxs-lookup"><span data-stu-id="c09e1-206">b.</span></span> <span data-ttu-id="c09e1-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c09e1-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c09e1-208">c.</span><span class="sxs-lookup"><span data-stu-id="c09e1-208">c.</span></span> <span data-ttu-id="c09e1-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c09e1-210">d.</span><span class="sxs-lookup"><span data-stu-id="c09e1-210">d.</span></span> <span data-ttu-id="c09e1-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-211">Click **Create**.</span></span>
 
### <a name="creating-a-hosted-graphite-test-user"></a><span data-ttu-id="c09e1-212">Vytvoření zkušebního uživatele hostované grafitová</span><span class="sxs-lookup"><span data-stu-id="c09e1-212">Creating a Hosted Graphite test user</span></span>

<span data-ttu-id="c09e1-213">Hello cílem této části je toocreate volal Britta Simon v hostované grafitová uživatele.</span><span class="sxs-lookup"><span data-stu-id="c09e1-213">hello objective of this section is toocreate a user called Britta Simon in Hosted Graphite.</span></span> <span data-ttu-id="c09e1-214">Hostované grafitová podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="c09e1-214">Hosted Graphite supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="c09e1-215">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="c09e1-215">There is no action item for you in this section.</span></span> <span data-ttu-id="c09e1-216">Nového uživatele bude vytvořen během pokusu o tooaccess Hosted grafitová, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c09e1-216">A new user will be created during an attempt tooaccess Hosted Graphite if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="c09e1-217">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello hostované grafitová tým podpory prostřednictvím < mailto:help@hostedgraphite.com >.</span><span class="sxs-lookup"><span data-stu-id="c09e1-217">If you need toocreate a user manually, you need toocontact hello Hosted Graphite support team via <mailto:help@hostedgraphite.com>.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c09e1-218">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c09e1-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c09e1-219">V této části povolíte tak, že udělíte přístup tooHosted grafitová Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c09e1-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHosted Graphite.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c09e1-221">**tooassign tooHosted Britta Simon grafitová, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c09e1-221">**tooassign Britta Simon tooHosted Graphite, perform hello following steps:**</span></span>

1. <span data-ttu-id="c09e1-222">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c09e1-224">V seznamu aplikace hello vyberte **hostované grafitová**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-224">In hello applications list, select **Hosted Graphite**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

3. <span data-ttu-id="c09e1-226">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c09e1-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c09e1-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c09e1-228">Click **Add** button.</span></span> <span data-ttu-id="c09e1-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c09e1-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c09e1-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c09e1-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c09e1-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c09e1-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c09e1-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c09e1-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c09e1-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c09e1-234">Testing single sign-on</span></span>

<span data-ttu-id="c09e1-235">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="c09e1-235">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="c09e1-236">Když kliknete na dlaždici hostované grafitová hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour grafitová hostované aplikace.</span><span class="sxs-lookup"><span data-stu-id="c09e1-236">When you click hello Hosted Graphite tile in hello Access Panel, you should get automatically signed-on tooyour Hosted Graphite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c09e1-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c09e1-237">Additional resources</span></span>

* [<span data-ttu-id="c09e1-238">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c09e1-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c09e1-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c09e1-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png

