---
title: "Kurz: Azure Active Directory integrace s interaktivní příručky Yonyx | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a interaktivní příručky Yonyx."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: 24e30d243143651b8d32535c76dc300931ae5746
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a><span data-ttu-id="90546-103">Kurz: Azure Active Directory integrace s Yonyx interaktivní příručky</span><span class="sxs-lookup"><span data-stu-id="90546-103">Tutorial: Azure Active Directory integration with Yonyx Interactive Guides</span></span>

<span data-ttu-id="90546-104">V tomto kurzu zjistíte, jak toointegrate Yonyx interaktivní Průvodce službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="90546-104">In this tutorial, you learn how toointegrate Yonyx Interactive Guides with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="90546-105">Integrace Yonyx interaktivní příručky s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="90546-105">Integrating Yonyx Interactive Guides with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="90546-106">Můžete řídit ve službě Azure AD, který má přístup tooYonyx interaktivní příručky</span><span class="sxs-lookup"><span data-stu-id="90546-106">You can control in Azure AD who has access tooYonyx Interactive Guides</span></span>
- <span data-ttu-id="90546-107">Vaši uživatelé tooautomatically get přihlášeného tooYonyx interaktivní příručky (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="90546-107">You can enable your users tooautomatically get signed-on tooYonyx Interactive Guides (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="90546-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="90546-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="90546-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="90546-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90546-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="90546-110">Prerequisites</span></span>

<span data-ttu-id="90546-111">tooconfigure integrace Azure AD pomocí Yonyx interaktivní příruček, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="90546-111">tooconfigure Azure AD integration with Yonyx Interactive Guides, you need hello following items:</span></span>

- <span data-ttu-id="90546-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="90546-112">An Azure AD subscription</span></span>
- <span data-ttu-id="90546-113">Interaktivní příručky Yonyx jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="90546-113">A Yonyx Interactive Guides single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="90546-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="90546-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="90546-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="90546-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="90546-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="90546-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="90546-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="90546-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="90546-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="90546-118">Scenario description</span></span>
<span data-ttu-id="90546-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="90546-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="90546-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="90546-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="90546-121">Přidání příručky interaktivní Yonyx z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="90546-121">Adding Yonyx Interactive Guides from hello gallery</span></span>
2. <span data-ttu-id="90546-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="90546-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-yonyx-interactive-guides-from-hello-gallery"></a><span data-ttu-id="90546-123">Přidání příručky interaktivní Yonyx z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="90546-123">Adding Yonyx Interactive Guides from hello gallery</span></span>
<span data-ttu-id="90546-124">tooconfigure hello integrace interaktivní příručky Yonyx do Azure AD, je nutné tooadd Yonyx interaktivní příručky hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="90546-124">tooconfigure hello integration of Yonyx Interactive Guides into Azure AD, you need tooadd Yonyx Interactive Guides from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="90546-125">**tooadd Yonyx interaktivní příručky z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="90546-125">**tooadd Yonyx Interactive Guides from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="90546-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="90546-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="90546-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="90546-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="90546-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="90546-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="90546-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="90546-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="90546-133">Hello vyhledávacího pole zadejte **interaktivní příručky Yonyx**, vyberte **interaktivní příručky Yonyx** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="90546-133">In hello search box, type **Yonyx Interactive Guides**, select  **Yonyx Interactive Guides**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Interaktivní příručky Yonyx v seznamu výsledků hello](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="90546-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="90546-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="90546-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Yonyx interaktivní příručky podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="90546-136">In this section, you configure and test Azure AD single sign-on with Yonyx Interactive Guides based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="90546-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v příručkách interaktivní Yonyx je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90546-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Yonyx Interactive Guides is tooa user in Azure AD.</span></span> <span data-ttu-id="90546-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v příručkách interaktivní Yonyx musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="90546-138">In other words, a link relationship between an Azure AD user and hello related user in Yonyx Interactive Guides needs toobe established.</span></span>

<span data-ttu-id="90546-139">V Yonyx interaktivní příručky, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="90546-139">In Yonyx Interactive Guides, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="90546-140">tooconfigure a testování Azure AD jednotné přihlašování pomocí Yonyx interaktivní příruček, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="90546-140">tooconfigure and test Azure AD single sign-on with Yonyx Interactive Guides, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="90546-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="90546-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="90546-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="90546-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="90546-143">**[Vytvoření zkušebního uživatele interaktivní příručky Yonyx](#create-a-yonyx-interactive-guides-test-user)**  -toohave protějšek Britta Simon v příručkách interaktivní Yonyx, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="90546-143">**[Create a Yonyx Interactive Guides test user](#create-a-yonyx-interactive-guides-test-user)** - toohave a counterpart of Britta Simon in Yonyx Interactive Guides that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="90546-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="90546-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="90546-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="90546-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="90546-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="90546-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="90546-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Yonyx interaktivní příručky.</span><span class="sxs-lookup"><span data-stu-id="90546-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Yonyx Interactive Guides application.</span></span>

<span data-ttu-id="90546-148">**tooconfigure Azure AD jednotné přihlašování pomocí Yonyx interaktivní příruček, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="90546-148">**tooconfigure Azure AD single sign-on with Yonyx Interactive Guides, perform hello following steps:**</span></span>

1. <span data-ttu-id="90546-149">V portálu Azure, na hello hello **interaktivní příručky Yonyx** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="90546-149">In hello Azure portal, on hello **Yonyx Interactive Guides** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="90546-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="90546-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_samlbase.png)

3. <span data-ttu-id="90546-153">Na hello **Yonyx interaktivní příručky domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="90546-153">On hello **Yonyx Interactive Guides Domain and URLs** section, perform hello following steps:</span></span>

    ![Yonyx interaktivní příručky domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_url.png)

    <span data-ttu-id="90546-155">a.</span><span class="sxs-lookup"><span data-stu-id="90546-155">a.</span></span> <span data-ttu-id="90546-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.yonyx.com/y/conversation/?id=<guid number>`</span><span class="sxs-lookup"><span data-stu-id="90546-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`</span></span>

    <span data-ttu-id="90546-157">b.</span><span class="sxs-lookup"><span data-stu-id="90546-157">b.</span></span> <span data-ttu-id="90546-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.yonyx.com`</span><span class="sxs-lookup"><span data-stu-id="90546-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.yonyx.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="90546-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="90546-159">These values are not real.</span></span> <span data-ttu-id="90546-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="90546-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="90546-161">Obraťte se na [tým podpory Yonyx interaktivní příručky klienta](mailto:support@yonyx.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="90546-161">Contact [Yonyx Interactive Guides Client support team](mailto:support@yonyx.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="90546-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="90546-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_certificate.png) 

5. <span data-ttu-id="90546-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="90546-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-yonyx-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="90546-166">Na hello **Yonyx interaktivní příručky konfigurace** klikněte na tlačítko **konfigurace interaktivní příručky Yonyx** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="90546-166">On hello **Yonyx Interactive Guides Configuration** section, click **Configure Yonyx Interactive Guides** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="90546-167">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="90546-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Yonyx interaktivní příručky](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_configure.png) 

7. <span data-ttu-id="90546-169">tooconfigure jednotného přihlašování na **interaktivní příručky Yonyx** straně, je nutné stáhnout hello toosend **Certificate(Base64)**, **Sign-Out URL**, **SAML Jednotné přihlašování adresa URL služby** **SAML Entity ID** příliš[tým podpory interaktivní příručky Yonyx](mailto:support@yonyx.com).</span><span class="sxs-lookup"><span data-stu-id="90546-169">tooconfigure single sign-on on **Yonyx Interactive Guides** side, you need toosend hello downloaded **Certificate(Base64)**, **Sign-Out URL**, **SAML Single Sign-On Service URL** **SAML Entity ID** too[Yonyx Interactive Guides support team](mailto:support@yonyx.com).</span></span> <span data-ttu-id="90546-170">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="90546-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="90546-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="90546-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="90546-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="90546-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="90546-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="90546-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="90546-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="90546-174">Create an Azure AD test user</span></span>

<span data-ttu-id="90546-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="90546-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

  ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="90546-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="90546-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="90546-178">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="90546-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-yonyx-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="90546-180">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="90546-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-yonyx-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="90546-182">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="90546-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="90546-184">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="90546-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="90546-186">a.</span><span class="sxs-lookup"><span data-stu-id="90546-186">a.</span></span> <span data-ttu-id="90546-187">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="90546-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="90546-188">b.</span><span class="sxs-lookup"><span data-stu-id="90546-188">b.</span></span> <span data-ttu-id="90546-189">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="90546-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="90546-190">c.</span><span class="sxs-lookup"><span data-stu-id="90546-190">c.</span></span> <span data-ttu-id="90546-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="90546-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="90546-192">d.</span><span class="sxs-lookup"><span data-stu-id="90546-192">d.</span></span> <span data-ttu-id="90546-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="90546-193">Click **Create**.</span></span>
 
### <a name="create-a-yonyx-interactive-guides-test-user"></a><span data-ttu-id="90546-194">Vytvoření zkušebního uživatele Yonyx interaktivní příručky</span><span class="sxs-lookup"><span data-stu-id="90546-194">Create a Yonyx Interactive Guides test user</span></span>

<span data-ttu-id="90546-195">Hello cílem této části je toocreate názvem Britta Simon v příručkách Yonyx interaktivní uživatel.</span><span class="sxs-lookup"><span data-stu-id="90546-195">hello objective of this section is toocreate a user called Britta Simon in Yonyx Interactive Guides.</span></span> <span data-ttu-id="90546-196">Interaktivní příručky Yonyx podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="90546-196">Yonyx Interactive Guides supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="90546-197">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="90546-197">There is no action item for you in this section.</span></span> <span data-ttu-id="90546-198">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess Yonyx interaktivní příručky.</span><span class="sxs-lookup"><span data-stu-id="90546-198">A new user is created during an attempt tooaccess Yonyx Interactive Guides if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="90546-199">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello tým prostřednictvím podpory interaktivní příručky Yonyx < mailto:support@yonyx.com >.</span><span class="sxs-lookup"><span data-stu-id="90546-199">If you need toocreate a user manually, you need toocontact hello Yonyx Interactive Guides support team via <mailto:support@yonyx.com>.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="90546-200">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="90546-200">Assign hello Azure AD test user</span></span>

<span data-ttu-id="90546-201">V této části povolíte tak, že udělíte přístup tooYonyx interaktivní příručky Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="90546-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooYonyx Interactive Guides.</span></span>

![Přiřadit role uživatele hello][200]

<span data-ttu-id="90546-203">**tooassign Britta Simon tooYonyx interaktivní příručky, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="90546-203">**tooassign Britta Simon tooYonyx Interactive Guides, perform hello following steps:**</span></span>

1. <span data-ttu-id="90546-204">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="90546-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="90546-206">V seznamu aplikace hello vyberte **Yonyx interaktivní příručky**.</span><span class="sxs-lookup"><span data-stu-id="90546-206">In hello applications list, select **Yonyx Interactive Guides**.</span></span>

    ![Hello interaktivní příručky Yonyx odkaz v seznamu aplikace hello](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_app.png) 

3. <span data-ttu-id="90546-208">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="90546-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="90546-210">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="90546-210">Click **Add** button.</span></span> <span data-ttu-id="90546-211">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="90546-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="90546-213">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="90546-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="90546-214">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="90546-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="90546-215">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="90546-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="90546-216">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="90546-216">Test single sign-on</span></span>

<span data-ttu-id="90546-217">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="90546-217">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="90546-218">Po kliknutí na tlačítko hello interaktivní příručky Yonyx dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Yonyx interaktivní příručky aplikace.</span><span class="sxs-lookup"><span data-stu-id="90546-218">When you click hello Yonyx Interactive Guides tile in hello Access Panel, you should get automatically signed-on tooyour Yonyx Interactive Guides application.</span></span>

<span data-ttu-id="90546-219">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="90546-219">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90546-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="90546-220">Additional resources</span></span>

* [<span data-ttu-id="90546-221">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="90546-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="90546-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="90546-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png

