---
title: "Kurz: Azure Active Directory integrace s plánováním sortimentu Predictix | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Predictix sortimentu plánování."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 37e686ff-f8e5-40b1-9d7e-f64b076917b7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1294b712caf12fdafaf65d70a02ee9fbdc3e84a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a><span data-ttu-id="45dc4-103">Kurz: Azure Active Directory integrace s plánováním sortimentu Predictix</span><span class="sxs-lookup"><span data-stu-id="45dc4-103">Tutorial: Azure Active Directory integration with Predictix Assortment Planning</span></span>

<span data-ttu-id="45dc4-104">V tomto kurzu zjistíte, jak toointegrate Predictix sortimentu plánování s použitím Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="45dc4-104">In this tutorial, you learn how toointegrate Predictix Assortment Planning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="45dc4-105">Integrace plánování sortimentu Predictix s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="45dc4-105">Integrating Predictix Assortment Planning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="45dc4-106">Můžete ovládat ve službě Azure AD, který má přístup tooPredictix sortimentu plánování.</span><span class="sxs-lookup"><span data-stu-id="45dc4-106">You can control in Azure AD who has access tooPredictix Assortment Planning.</span></span>
- <span data-ttu-id="45dc4-107">Vaši uživatelé tooautomatically get přihlášeného tooPredictix sortimentu plánování (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="45dc4-107">You can enable your users tooautomatically get signed-on tooPredictix Assortment Planning (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="45dc4-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="45dc4-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="45dc4-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="45dc4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45dc4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="45dc4-110">Prerequisites</span></span>

<span data-ttu-id="45dc4-111">Integrace služby Azure AD s plánováním sortimentu Predictix tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="45dc4-111">tooconfigure Azure AD integration with Predictix Assortment Planning, you need hello following items:</span></span>

- <span data-ttu-id="45dc4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="45dc4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="45dc4-113">Plánování sortimentu Predictix jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="45dc4-113">A Predictix Assortment Planning single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="45dc4-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="45dc4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="45dc4-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="45dc4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="45dc4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="45dc4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="45dc4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="45dc4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="45dc4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="45dc4-118">Scenario description</span></span>
<span data-ttu-id="45dc4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="45dc4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="45dc4-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="45dc4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="45dc4-121">Přidání Predictix sortimentu plánování z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="45dc4-121">Adding Predictix Assortment Planning from hello gallery</span></span>
2. <span data-ttu-id="45dc4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="45dc4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-predictix-assortment-planning-from-hello-gallery"></a><span data-ttu-id="45dc4-123">Přidání Predictix sortimentu plánování z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="45dc4-123">Adding Predictix Assortment Planning from hello gallery</span></span>
<span data-ttu-id="45dc4-124">tooconfigure hello integrace plánování sortimentu Predictix do Azure AD, je nutné tooadd Predictix sortimentu plánování hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="45dc4-124">tooconfigure hello integration of Predictix Assortment Planning into Azure AD, you need tooadd Predictix Assortment Planning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="45dc4-125">**tooadd Predictix sortimentu plánování z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="45dc4-125">**tooadd Predictix Assortment Planning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="45dc4-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="45dc4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="45dc4-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="45dc4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="45dc4-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="45dc4-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="45dc4-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="45dc4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="45dc4-133">Hello vyhledávacího pole zadejte **plánování sortimentu Predictix**, vyberte **plánování sortimentu Predictix** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="45dc4-133">In hello search box, type **Predictix Assortment Planning**, select **Predictix Assortment Planning** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Predictix sortimentu plánování v seznamu výsledků hello](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="45dc4-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="45dc4-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="45dc4-136">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Predictix sortimentu plánování podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="45dc4-136">In this section, you configure and test Azure AD single sign-on with Predictix Assortment Planning based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="45dc4-137">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello Predictix sortimentu plánování je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="45dc4-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Predictix Assortment Planning is tooa user in Azure AD.</span></span> <span data-ttu-id="45dc4-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Predictix sortimentu plánování musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="45dc4-138">In other words, a link relationship between an Azure AD user and hello related user in Predictix Assortment Planning needs toobe established.</span></span>

<span data-ttu-id="45dc4-139">Při plánování sortimentu Predictix, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="45dc4-139">In Predictix Assortment Planning, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="45dc4-140">tooconfigure a testování Azure AD jednotné přihlašování s plánováním sortimentu Predictix, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="45dc4-140">tooconfigure and test Azure AD single sign-on with Predictix Assortment Planning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="45dc4-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="45dc4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="45dc4-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="45dc4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="45dc4-143">**[Vytvoření zkušebního uživatele plánování sortimentu Predictix](#create-a-predictix-assortment-planning-test-user)**  -toohave protějšek Britta Simon Predictix sortimentu plánování, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="45dc4-143">**[Create a Predictix Assortment Planning test user](#create-a-predictix-assortment-planning-test-user)** - toohave a counterpart of Britta Simon in Predictix Assortment Planning that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="45dc4-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="45dc4-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="45dc4-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="45dc4-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="45dc4-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="45dc4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="45dc4-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Predictix sortimentu plánování.</span><span class="sxs-lookup"><span data-stu-id="45dc4-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Predictix Assortment Planning application.</span></span>

<span data-ttu-id="45dc4-148">**tooconfigure Azure AD jednotné přihlašování s Predictix sortimentu plánování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="45dc4-148">**tooconfigure Azure AD single sign-on with Predictix Assortment Planning, perform hello following steps:**</span></span>

1. <span data-ttu-id="45dc4-149">V portálu Azure, na hello hello **plánování sortimentu Predictix** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="45dc4-149">In hello Azure portal, on hello **Predictix Assortment Planning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="45dc4-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="45dc4-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_samlbase.png)

3. <span data-ttu-id="45dc4-153">Na hello **Predictix sortimentu plánování domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="45dc4-153">On hello **Predictix Assortment Planning Domain and URLs** section, perform hello following steps:</span></span>

    ![Predictix sortimentu plánování domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_url.png)

    <span data-ttu-id="45dc4-155">a.</span><span class="sxs-lookup"><span data-stu-id="45dc4-155">a.</span></span> <span data-ttu-id="45dc4-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="45dc4-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com/sso/request`|
    | `https://<sub-domain>.dev.ap.predictix.com/`|

    <span data-ttu-id="45dc4-157">b.</span><span class="sxs-lookup"><span data-stu-id="45dc4-157">b.</span></span> <span data-ttu-id="45dc4-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="45dc4-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com`|
    | `https://<sub-domain>.dev.ap.predictix.com`|
    
    > [!NOTE] 
    > <span data-ttu-id="45dc4-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="45dc4-159">These values are not real.</span></span> <span data-ttu-id="45dc4-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="45dc4-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="45dc4-161">Obraťte se na [tým podpory Predictix sortimentu plánování klienta](http://www.infor.com/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="45dc4-161">Contact [Predictix Assortment Planning Client support team](http://www.infor.com/support) tooget these values.</span></span> 
 


4. <span data-ttu-id="45dc4-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="45dc4-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_certificate.png) 

5. <span data-ttu-id="45dc4-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="45dc4-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="45dc4-166">Na hello **Predictix sortimentu plánování konfigurace** klikněte na tlačítko **Konfigurace plánování sortimentu Predictix** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="45dc4-166">On hello **Predictix Assortment Planning Configuration** section, click **Configure Predictix Assortment Planning** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="45dc4-167">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="45dc4-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Plánování konfigurace sortimentu Predictix](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_configure.png) 

7. <span data-ttu-id="45dc4-169">tooconfigure jednotného přihlašování na **plánování sortimentu Predictix** straně, je nutné stáhnout hello toosend **Certificate(Base64)**, **SAML Entity ID**,  **SAML jeden přihlašování adresa URL služby**, a **Sign-Out URL** příliš[Predictix sortimentu plánování tým podpory](http://www.infor.com/support).</span><span class="sxs-lookup"><span data-stu-id="45dc4-169">tooconfigure single sign-on on **Predictix Assortment Planning** side, you need toosend hello downloaded **Certificate(Base64)**, **SAML Entity ID**, **SAML Single Sign-On Service URL**, and **Sign-Out URL**  too[Predictix Assortment Planning support team](http://www.infor.com/support).</span></span> <span data-ttu-id="45dc4-170">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="45dc4-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="45dc4-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="45dc4-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="45dc4-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="45dc4-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="45dc4-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="45dc4-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="45dc4-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="45dc4-174">Create an Azure AD test user</span></span>

<span data-ttu-id="45dc4-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="45dc4-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="45dc4-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="45dc4-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="45dc4-178">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="45dc4-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="45dc4-180">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="45dc4-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="45dc4-182">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="45dc4-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="45dc4-184">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="45dc4-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_04.png)

    <span data-ttu-id="45dc4-186">a.</span><span class="sxs-lookup"><span data-stu-id="45dc4-186">a.</span></span> <span data-ttu-id="45dc4-187">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="45dc4-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="45dc4-188">b.</span><span class="sxs-lookup"><span data-stu-id="45dc4-188">b.</span></span> <span data-ttu-id="45dc4-189">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="45dc4-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="45dc4-190">c.</span><span class="sxs-lookup"><span data-stu-id="45dc4-190">c.</span></span> <span data-ttu-id="45dc4-191">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="45dc4-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="45dc4-192">d.</span><span class="sxs-lookup"><span data-stu-id="45dc4-192">d.</span></span> <span data-ttu-id="45dc4-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="45dc4-193">Click **Create**.</span></span>
 
### <a name="create-a-predictix-assortment-planning-test-user"></a><span data-ttu-id="45dc4-194">Vytvoření zkušebního uživatele Predictix sortimentu plánování</span><span class="sxs-lookup"><span data-stu-id="45dc4-194">Create a Predictix Assortment Planning test user</span></span>

<span data-ttu-id="45dc4-195">V této části vytvoříte uživatele volat Britta Simon Predictix sortimentu plánování.</span><span class="sxs-lookup"><span data-stu-id="45dc4-195">In this section, you create a user called Britta Simon in Predictix Assortment Planning.</span></span> <span data-ttu-id="45dc4-196">Spojte se s [Predictix sortimentu plánování tým podpory](http://www.infor.com/contact/) tooadd hello uživatelé v platformě Predictix sortimentu plánování hello.</span><span class="sxs-lookup"><span data-stu-id="45dc4-196">Please work with [Predictix Assortment Planning support team](http://www.infor.com/contact/) tooadd hello users in hello Predictix Assortment Planning platform.</span></span>
 > [!NOTE]
 > <span data-ttu-id="45dc4-197">Držitel účtu Azure Active Directory Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="45dc4-197">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="45dc4-198">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="45dc4-198">Assign hello Azure AD test user</span></span>

<span data-ttu-id="45dc4-199">V této části povolíte tak, že udělíte přístup tooPredictix sortimentu plánování Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="45dc4-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPredictix Assortment Planning.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="45dc4-201">**tooassign tooPredictix Britta Simon sortimentu plánování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="45dc4-201">**tooassign Britta Simon tooPredictix Assortment Planning, perform hello following steps:**</span></span>

1. <span data-ttu-id="45dc4-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="45dc4-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="45dc4-204">V seznamu aplikace hello vyberte **Predictix sortimentu plánování**.</span><span class="sxs-lookup"><span data-stu-id="45dc4-204">In hello applications list, select **Predictix Assortment Planning**.</span></span>

    ![v seznamu aplikace hello odkaz Predictix sortimentu plánování Hello](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_app.png)  

3. <span data-ttu-id="45dc4-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="45dc4-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="45dc4-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="45dc4-208">Click **Add** button.</span></span> <span data-ttu-id="45dc4-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="45dc4-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="45dc4-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="45dc4-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="45dc4-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="45dc4-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="45dc4-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="45dc4-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="45dc4-214">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="45dc4-214">Test single sign-on</span></span>

<span data-ttu-id="45dc4-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="45dc4-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="45dc4-216">Po kliknutí na tlačítko hello Predictix sortimentu plánování dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Predictix sortimentu plánování aplikace.</span><span class="sxs-lookup"><span data-stu-id="45dc4-216">When you click hello Predictix Assortment Planning tile in hello Access Panel, you should get automatically signed-on tooyour Predictix Assortment Planning application.</span></span>
<span data-ttu-id="45dc4-217">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="45dc4-217">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="45dc4-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="45dc4-218">Additional resources</span></span>

* [<span data-ttu-id="45dc4-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="45dc4-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="45dc4-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="45dc4-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_203.png

