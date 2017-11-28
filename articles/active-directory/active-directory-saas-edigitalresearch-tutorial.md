---
title: 'Kurz: Azure Active Directory integrace s eDigitalResearch | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a eDigitalResearch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c6b66ea0-16ba-45b4-b550-e81c56262b1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 6dd3cafb25ef8ede3a4c16902ed8da69cb7b715f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-edigitalresearch"></a><span data-ttu-id="bc673-103">Kurz: Azure Active Directory integrace s eDigitalResearch</span><span class="sxs-lookup"><span data-stu-id="bc673-103">Tutorial: Azure Active Directory integration with eDigitalResearch</span></span>

<span data-ttu-id="bc673-104">V tomto kurzu zjistíte, jak toointegrate eDigitalResearch s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bc673-104">In this tutorial, you learn how toointegrate eDigitalResearch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bc673-105">Integrace eDigitalResearch s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="bc673-105">Integrating eDigitalResearch with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bc673-106">Můžete ovládat ve službě Azure AD, který má přístup tooeDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="bc673-106">You can control in Azure AD who has access tooeDigitalResearch.</span></span>
- <span data-ttu-id="bc673-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooeDigitalResearch (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc673-107">You can enable your users tooautomatically get signed-on tooeDigitalResearch (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="bc673-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bc673-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="bc673-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bc673-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc673-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bc673-110">Prerequisites</span></span>

<span data-ttu-id="bc673-111">Integrace služby Azure AD s eDigitalResearch tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="bc673-111">tooconfigure Azure AD integration with eDigitalResearch, you need hello following items:</span></span>

- <span data-ttu-id="bc673-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc673-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bc673-113">EDigitalResearch jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="bc673-113">A eDigitalResearch single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bc673-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc673-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bc673-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="bc673-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bc673-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="bc673-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bc673-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bc673-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bc673-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="bc673-118">Scenario description</span></span>
<span data-ttu-id="bc673-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc673-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bc673-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="bc673-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bc673-121">Přidání eDigitalResearch z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="bc673-121">Adding eDigitalResearch from hello gallery</span></span>
2. <span data-ttu-id="bc673-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bc673-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-edigitalresearch-from-hello-gallery"></a><span data-ttu-id="bc673-123">Přidání eDigitalResearch z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="bc673-123">Adding eDigitalResearch from hello gallery</span></span>
<span data-ttu-id="bc673-124">tooconfigure hello integrace eDigitalResearch do Azure AD, je nutné tooadd eDigitalResearch hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="bc673-124">tooconfigure hello integration of eDigitalResearch into Azure AD, you need tooadd eDigitalResearch from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bc673-125">**tooadd eDigitalResearch z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bc673-125">**tooadd eDigitalResearch from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc673-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bc673-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="bc673-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="bc673-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bc673-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bc673-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="bc673-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bc673-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="bc673-133">Hello vyhledávacího pole zadejte **eDigitalResearch**, vyberte **eDigitalResearch** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc673-133">In hello search box, type **eDigitalResearch**, select **eDigitalResearch** from result panel then click **Add** button tooadd hello application.</span></span>

    ![eDigitalResearch v seznamu výsledků hello](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="bc673-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bc673-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="bc673-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s eDigitalResearch podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="bc673-136">In this section, you configure and test Azure AD single sign-on with eDigitalResearch based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bc673-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v eDigitalResearch je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc673-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in eDigitalResearch is tooa user in Azure AD.</span></span> <span data-ttu-id="bc673-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v eDigitalResearch musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="bc673-138">In other words, a link relationship between an Azure AD user and hello related user in eDigitalResearch needs toobe established.</span></span>

<span data-ttu-id="bc673-139">V eDigitalResearch, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="bc673-139">In eDigitalResearch, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bc673-140">tooconfigure a testu Azure AD jednotné přihlašování s eDigitalResearch, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="bc673-140">tooconfigure and test Azure AD single sign-on with eDigitalResearch, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bc673-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="bc673-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bc673-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bc673-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bc673-143">**[Vytvoření zkušebního uživatele eDigitalResearch](#create-a-edigitalresearch-test-user)**  -toohave protějšek Britta Simon v eDigitalResearch, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="bc673-143">**[Create a eDigitalResearch test user](#create-a-edigitalresearch-test-user)** - toohave a counterpart of Britta Simon in eDigitalResearch that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bc673-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bc673-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bc673-145">**[Test jednotného přihlašování](#test-single-sign-on)**  tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="bc673-145">**[Test single sign-on](#test-single-sign-on)**  tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="bc673-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bc673-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="bc673-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="bc673-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your eDigitalResearch application.</span></span>

<span data-ttu-id="bc673-148">**tooconfigure Azure AD jednotné přihlašování s eDigitalResearch, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bc673-148">**tooconfigure Azure AD single sign-on with eDigitalResearch, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc673-149">V portálu Azure, na hello hello **eDigitalResearch** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bc673-149">In hello Azure portal, on hello **eDigitalResearch** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="bc673-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bc673-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_samlbase.png)

3. <span data-ttu-id="bc673-153">Na hello **eDigitalResearch domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="bc673-153">On hello **eDigitalResearch Domain and URLs** section, perform hello following steps:</span></span>

    ![eDigitalResearch domény a adresy URL jeden přihlašování informace](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_url.png)

    <span data-ttu-id="bc673-155">a.</span><span class="sxs-lookup"><span data-stu-id="bc673-155">a.</span></span> <span data-ttu-id="bc673-156">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company-name>.edigitalresearch.com`</span><span class="sxs-lookup"><span data-stu-id="bc673-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company-name>.edigitalresearch.com`</span></span>

    <span data-ttu-id="bc673-157">b.</span><span class="sxs-lookup"><span data-stu-id="bc673-157">b.</span></span> <span data-ttu-id="bc673-158">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company-name>.edigitalresearch.com/login/consume`</span><span class="sxs-lookup"><span data-stu-id="bc673-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company-name>.edigitalresearch.com/login/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bc673-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="bc673-159">These values are not real.</span></span> <span data-ttu-id="bc673-160">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="bc673-160">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="bc673-161">Obraťte se na [tým podpory eDigitalResearch](http://www.maruedr.com/contact) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc673-161">Contact [eDigitalResearch support team](http://www.maruedr.com/contact) tooget these values.</span></span>
 


4. <span data-ttu-id="bc673-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikát Base(64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="bc673-162">On hello **SAML Signing Certificate** section, click **Certificate Base(64)** and then save hello certificate file on your computer.</span></span>

    <span data-ttu-id="bc673-163">!</span><span class="sxs-lookup"><span data-stu-id="bc673-163">!</span></span>![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_certificate.png) 

5. <span data-ttu-id="bc673-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bc673-165">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bc673-167">Na hello **eDigitalResearch konfigurace** klikněte na tlačítko **konfigurace eDigitalResearch** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="bc673-167">On hello **eDigitalResearch Configuration** section, click **Configure eDigitalResearch** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="bc673-168">Kopírování hello **Sign-Out adresu URL, SAML Entity ID** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="bc673-168">Copy hello **Sign-Out URL, SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![eDigitalResearch konfigurace](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_configure.png) 

7. <span data-ttu-id="bc673-170">tooconfigure jednotného přihlašování na **eDigitalResearch** straně, je nutné stáhnout hello toosend **soubor certifikátu (Base64)**, **SAML Entity ID**, a  **Adresa URL odhlašování** příliš[tým podpory eDigitalResearch](http://www.maruedr.com/contact).</span><span class="sxs-lookup"><span data-stu-id="bc673-170">tooconfigure single sign-on on **eDigitalResearch** side, you need toosend hello downloaded **Certificate (Base64) File**, **SAML Entity ID**, and **Sign-Out URL** too[eDigitalResearch support team](http://www.maruedr.com/contact).</span></span> <span data-ttu-id="bc673-171">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="bc673-171">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="bc673-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="bc673-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bc673-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="bc673-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bc673-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bc673-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="bc673-175">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc673-175">Create an Azure AD test user</span></span>

<span data-ttu-id="bc673-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="bc673-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="bc673-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bc673-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc673-179">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bc673-179">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="bc673-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="bc673-181">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="bc673-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bc673-183">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="bc673-185">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="bc673-185">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_04.png)

    <span data-ttu-id="bc673-187">a.</span><span class="sxs-lookup"><span data-stu-id="bc673-187">a.</span></span> <span data-ttu-id="bc673-188">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bc673-188">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bc673-189">b.</span><span class="sxs-lookup"><span data-stu-id="bc673-189">b.</span></span> <span data-ttu-id="bc673-190">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bc673-190">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="bc673-191">c.</span><span class="sxs-lookup"><span data-stu-id="bc673-191">c.</span></span> <span data-ttu-id="bc673-192">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="bc673-192">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="bc673-193">d.</span><span class="sxs-lookup"><span data-stu-id="bc673-193">d.</span></span> <span data-ttu-id="bc673-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bc673-194">Click **Create**.</span></span>
  
### <a name="create-a-edigitalresearch-test-user"></a><span data-ttu-id="bc673-195">Vytvoření zkušebního uživatele eDigitalResearch</span><span class="sxs-lookup"><span data-stu-id="bc673-195">Create a eDigitalResearch test user</span></span>

<span data-ttu-id="bc673-196">Hello cílem této části je toocreate volal Britta Simon v eDigitalResearch uživatele.</span><span class="sxs-lookup"><span data-stu-id="bc673-196">hello objective of this section is toocreate a user called Britta Simon in eDigitalResearch.</span></span> 

<span data-ttu-id="bc673-197">Práce s hello [tým podpory eDigitalResearch](http://www.maruedr.com/contact) tooget uživatelé vytvoření.</span><span class="sxs-lookup"><span data-stu-id="bc673-197">Work with hello [eDigitalResearch support team](http://www.maruedr.com/contact) tooget users created.</span></span>       
    
 > [!NOTE]
 > <span data-ttu-id="bc673-198">Držitel účtu Azure Active Directory Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="bc673-198">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="bc673-199">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="bc673-199">Assign hello Azure AD test user</span></span>

<span data-ttu-id="bc673-200">V této části povolíte tak, že udělíte přístup tooeDigitalResearch toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bc673-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooeDigitalResearch.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="bc673-202">**tooassign Britta Simon tooeDigitalResearch, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bc673-202">**tooassign Britta Simon tooeDigitalResearch, perform hello following steps:**</span></span>

1. <span data-ttu-id="bc673-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bc673-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="bc673-205">V seznamu aplikace hello vyberte **eDigitalResearch**.</span><span class="sxs-lookup"><span data-stu-id="bc673-205">In hello applications list, select **eDigitalResearch**.</span></span>

    ![v seznamu aplikace hello Hello eDigitalResearch odkaz](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_app.png)  

3. <span data-ttu-id="bc673-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="bc673-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="bc673-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bc673-209">Click **Add** button.</span></span> <span data-ttu-id="bc673-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bc673-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="bc673-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="bc673-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bc673-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bc673-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bc673-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bc673-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="bc673-215">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bc673-215">Test single sign-on</span></span>

<span data-ttu-id="bc673-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bc673-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="bc673-217">Když kliknete na dlaždici eDigitalResearch hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour eDigitalResearch aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc673-217">When you click hello eDigitalResearch tile in hello Access Panel, you should get automatically signed-on tooyour eDigitalResearch application.</span></span>
<span data-ttu-id="bc673-218">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bc673-218">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bc673-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bc673-219">Additional resources</span></span>

* [<span data-ttu-id="bc673-220">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bc673-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bc673-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bc673-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_203.png

