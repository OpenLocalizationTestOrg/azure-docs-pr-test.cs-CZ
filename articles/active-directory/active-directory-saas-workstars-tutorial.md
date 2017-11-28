---
title: 'Kurz: Azure Active Directory integrace s Workstars | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Workstars."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 51a4a4e4-ff60-4971-b3f8-a0367b70d220
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 86250d7538f058d2a821ded7dda0b2fc185d80df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workstars"></a><span data-ttu-id="1d005-103">Kurz: Azure Active Directory integrace s Workstars</span><span class="sxs-lookup"><span data-stu-id="1d005-103">Tutorial: Azure Active Directory integration with Workstars</span></span>

<span data-ttu-id="1d005-104">V tomto kurzu zjistíte, jak toointegrate Workstars s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1d005-104">In this tutorial, you learn how toointegrate Workstars with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1d005-105">Integrace Workstars s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1d005-105">Integrating Workstars with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1d005-106">Můžete ovládat ve službě Azure AD, který má přístup tooWorkstars.</span><span class="sxs-lookup"><span data-stu-id="1d005-106">You can control in Azure AD who has access tooWorkstars.</span></span>
- <span data-ttu-id="1d005-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWorkstars (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d005-107">You can enable your users tooautomatically get signed-on tooWorkstars (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="1d005-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d005-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="1d005-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1d005-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d005-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1d005-110">Prerequisites</span></span>

<span data-ttu-id="1d005-111">Integrace služby Azure AD s Workstars tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="1d005-111">tooconfigure Azure AD integration with Workstars, you need hello following items:</span></span>

- <span data-ttu-id="1d005-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1d005-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1d005-113">Workstars jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1d005-113">A Workstars single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1d005-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d005-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1d005-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1d005-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1d005-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="1d005-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1d005-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1d005-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1d005-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1d005-118">Scenario description</span></span>
<span data-ttu-id="1d005-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1d005-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1d005-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1d005-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1d005-121">Přidání Workstars z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1d005-121">Adding Workstars from hello gallery</span></span>
2. <span data-ttu-id="1d005-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1d005-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workstars-from-hello-gallery"></a><span data-ttu-id="1d005-123">Přidání Workstars z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1d005-123">Adding Workstars from hello gallery</span></span>
<span data-ttu-id="1d005-124">tooconfigure hello integrace Workstars do Azure AD, je nutné tooadd Workstars hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1d005-124">tooconfigure hello integration of Workstars into Azure AD, you need tooadd Workstars from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1d005-125">**tooadd Workstars z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1d005-125">**tooadd Workstars from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1d005-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1d005-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="1d005-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1d005-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1d005-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1d005-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="1d005-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1d005-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="1d005-133">Hello vyhledávacího pole zadejte **Workstars**, vyberte **Workstars** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d005-133">In hello search box, type **Workstars**, select **Workstars** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Workstars v seznamu výsledků hello](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="1d005-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1d005-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="1d005-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workstars podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="1d005-136">In this section, you configure and test Azure AD single sign-on with Workstars based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1d005-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Workstars je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d005-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workstars is tooa user in Azure AD.</span></span> <span data-ttu-id="1d005-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Workstars musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="1d005-138">In other words, a link relationship between an Azure AD user and hello related user in Workstars needs toobe established.</span></span>

<span data-ttu-id="1d005-139">V Workstars, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="1d005-139">In Workstars, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1d005-140">tooconfigure a testu Azure AD jednotné přihlašování s Workstars, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="1d005-140">tooconfigure and test Azure AD single sign-on with Workstars, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1d005-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1d005-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1d005-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1d005-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1d005-143">**[Vytvoření zkušebního uživatele Workstars](#create-a-workstars-test-user)**  -toohave protějšek Britta Simon v Workstars, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="1d005-143">**[Create a Workstars test user](#create-a-workstars-test-user)** - toohave a counterpart of Britta Simon in Workstars that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1d005-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1d005-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1d005-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="1d005-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="1d005-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1d005-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="1d005-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Workstars.</span><span class="sxs-lookup"><span data-stu-id="1d005-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workstars application.</span></span>

<span data-ttu-id="1d005-148">**tooconfigure Azure AD jednotné přihlašování s Workstars, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1d005-148">**tooconfigure Azure AD single sign-on with Workstars, perform hello following steps:**</span></span>

1. <span data-ttu-id="1d005-149">V portálu Azure, na hello hello **Workstars** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1d005-149">In hello Azure portal, on hello **Workstars** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="1d005-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1d005-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_samlbase.png)

3. <span data-ttu-id="1d005-153">Na hello **Workstars domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1d005-153">On hello **Workstars Domain and URLs** section, perform hello following steps:</span></span>

    ![Workstars domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_url.png)

    <span data-ttu-id="1d005-155">a.</span><span class="sxs-lookup"><span data-stu-id="1d005-155">a.</span></span> <span data-ttu-id="1d005-156">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://workstars.com`</span><span class="sxs-lookup"><span data-stu-id="1d005-156">In hello **Identifier** textbox, type hello URL: `https://workstars.com`</span></span>

    <span data-ttu-id="1d005-157">b.</span><span class="sxs-lookup"><span data-stu-id="1d005-157">b.</span></span> <span data-ttu-id="1d005-158">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.workstars.com/saml/login_check`</span><span class="sxs-lookup"><span data-stu-id="1d005-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.workstars.com/saml/login_check`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1d005-159">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="1d005-159">hello value is not real.</span></span> <span data-ttu-id="1d005-160">Aktualizace hello hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1d005-160">Update hello value with hello actual Reply URL.</span></span> <span data-ttu-id="1d005-161">Obraťte se na [tým podpory Workstars](https://support.workstars.com) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1d005-161">Contact [Workstars support team](https://support.workstars.com) tooget hello value.</span></span>
 
4. <span data-ttu-id="1d005-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1d005-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_certificate.png) 

5. <span data-ttu-id="1d005-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1d005-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-workstars-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1d005-166">Na hello **Workstars konfigurace** klikněte na tlačítko **konfigurace Workstars** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="1d005-166">On hello **Workstars Configuration** section, click **Configure Workstars** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1d005-167">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="1d005-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_configure.png) 

7. <span data-ttu-id="1d005-169">V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti Workstars tooyour.</span><span class="sxs-lookup"><span data-stu-id="1d005-169">In another browser window, sign on tooyour Workstars company site as an administrator.</span></span>

8. <span data-ttu-id="1d005-170">Na hlavním panelu nástrojů hello klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="1d005-170">In hello main toolbar, click **Settings**.</span></span>

    ![Nastavení Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_sett.png)

9. <span data-ttu-id="1d005-172">Přejděte příliš**přihlašování** > **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="1d005-172">Go too**Sign On** > **Settings**.</span></span>

    ![Sign-on Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_signon.png)

    ![Nastavení Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_settings.png)

10. <span data-ttu-id="1d005-175">Na hello **jeden znak na (SAML) – nastavení** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1d005-175">On hello **Single Sign On (SAML) - Settings** page, perform hello following steps:</span></span>
    
    ![Workstars saml](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_saml.png)

    <span data-ttu-id="1d005-177">a.</span><span class="sxs-lookup"><span data-stu-id="1d005-177">a.</span></span> <span data-ttu-id="1d005-178">V **název zprostředkovatele Identity** textovému poli, typ **Office 365**.</span><span class="sxs-lookup"><span data-stu-id="1d005-178">In **Identity Provider Name** textbox, type **Office 365**.</span></span>

    <span data-ttu-id="1d005-179">b.</span><span class="sxs-lookup"><span data-stu-id="1d005-179">b.</span></span> <span data-ttu-id="1d005-180">V hello **ID Entity zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d005-180">In hello **Identity Provider Entity ID** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="1d005-181">c.</span><span class="sxs-lookup"><span data-stu-id="1d005-181">c.</span></span> <span data-ttu-id="1d005-182">Kopírovat obsah hello hello stažený certifikát souboru v programu Poznámkový blok a pak ji vložit do hello **x509 certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1d005-182">Copy hello content of hello downloaded certificate file in notepad, and then paste it into hello **x509 Certificate** textbox.</span></span> 

    <span data-ttu-id="1d005-183">d.</span><span class="sxs-lookup"><span data-stu-id="1d005-183">d.</span></span> <span data-ttu-id="1d005-184">V hello **URL jednotné přihlašování SAML** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d005-184">In hello **SAML SSO URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="1d005-185">e.</span><span class="sxs-lookup"><span data-stu-id="1d005-185">e.</span></span> <span data-ttu-id="1d005-186">V hello **vzdálené adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d005-186">In hello **Remote Logout URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="1d005-187">f.</span><span class="sxs-lookup"><span data-stu-id="1d005-187">f.</span></span> <span data-ttu-id="1d005-188">Vyberte **ID názvu** jako **e-mailu (výchozí)**.</span><span class="sxs-lookup"><span data-stu-id="1d005-188">select **Name ID** as **Email (Default)**.</span></span>

    <span data-ttu-id="1d005-189">g.</span><span class="sxs-lookup"><span data-stu-id="1d005-189">g.</span></span> <span data-ttu-id="1d005-190">Klikněte na tlačítko **potvrďte**.</span><span class="sxs-lookup"><span data-stu-id="1d005-190">Click **Confirm**.</span></span>
    
> [!TIP]
> <span data-ttu-id="1d005-191">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="1d005-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1d005-192">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="1d005-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1d005-193">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1d005-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="1d005-194">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1d005-194">Create an Azure AD test user</span></span>

<span data-ttu-id="1d005-195">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="1d005-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="1d005-197">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1d005-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1d005-198">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1d005-198">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-workstars-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="1d005-200">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1d005-200">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-workstars-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="1d005-202">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d005-202">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-workstars-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="1d005-204">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1d005-204">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-workstars-tutorial/create_aaduser_04.png)

    <span data-ttu-id="1d005-206">a.</span><span class="sxs-lookup"><span data-stu-id="1d005-206">a.</span></span> <span data-ttu-id="1d005-207">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1d005-207">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1d005-208">b.</span><span class="sxs-lookup"><span data-stu-id="1d005-208">b.</span></span> <span data-ttu-id="1d005-209">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1d005-209">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="1d005-210">c.</span><span class="sxs-lookup"><span data-stu-id="1d005-210">c.</span></span> <span data-ttu-id="1d005-211">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="1d005-211">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="1d005-212">d.</span><span class="sxs-lookup"><span data-stu-id="1d005-212">d.</span></span> <span data-ttu-id="1d005-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1d005-213">Click **Create**.</span></span>
  
### <a name="create-a-workstars-test-user"></a><span data-ttu-id="1d005-214">Vytvoření zkušebního uživatele Workstars</span><span class="sxs-lookup"><span data-stu-id="1d005-214">Create a Workstars test user</span></span>

<span data-ttu-id="1d005-215">V této části vytvoříte volal Britta Simon v Workstars uživatele.</span><span class="sxs-lookup"><span data-stu-id="1d005-215">In this section, you create a user called Britta Simon in Workstars.</span></span> <span data-ttu-id="1d005-216">Práce s [tým podpory Workstars](https://support.workstars.com) tooadd hello uživatelé v platformě Workstars hello.</span><span class="sxs-lookup"><span data-stu-id="1d005-216">Work with [Workstars support team](https://support.workstars.com) tooadd hello users in hello Workstars platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="1d005-217">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="1d005-217">Assign hello Azure AD test user</span></span>

<span data-ttu-id="1d005-218">V této části povolíte tak, že udělíte přístup tooWorkstars toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1d005-218">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkstars.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="1d005-220">**tooassign Britta Simon tooWorkstars, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1d005-220">**tooassign Britta Simon tooWorkstars, perform hello following steps:**</span></span>

1. <span data-ttu-id="1d005-221">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1d005-221">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1d005-223">V seznamu aplikace hello vyberte **Workstars**.</span><span class="sxs-lookup"><span data-stu-id="1d005-223">In hello applications list, select **Workstars**.</span></span>

    ![v seznamu aplikace hello Hello Workstars odkaz](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_app.png)  

3. <span data-ttu-id="1d005-225">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1d005-225">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="1d005-227">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1d005-227">Click **Add** button.</span></span> <span data-ttu-id="1d005-228">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d005-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="1d005-230">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="1d005-230">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1d005-231">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d005-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1d005-232">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1d005-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="1d005-233">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1d005-233">Test single sign-on</span></span>

<span data-ttu-id="1d005-234">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1d005-234">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1d005-235">Po kliknutí na tlačítko hello Workstars dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Workstars aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d005-235">When you click hello Workstars tile in hello Access Panel, you should get automatically signed-on tooyour Workstars application.</span></span>
<span data-ttu-id="1d005-236">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1d005-236">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1d005-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1d005-237">Additional resources</span></span>

* [<span data-ttu-id="1d005-238">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1d005-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1d005-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1d005-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_203.png

