---
title: 'Kurz: Azure Active Directory integrace s ScreenSteps | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ScreenSteps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: fd041e5fe4552727eeda2dabc1773d8043d410f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a><span data-ttu-id="4f12f-103">Kurz: Azure Active Directory integrace s ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="4f12f-103">Tutorial: Azure Active Directory integration with ScreenSteps</span></span>

<span data-ttu-id="4f12f-104">V tomto kurzu zjistíte, jak toointegrate ScreenSteps s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4f12f-104">In this tutorial, you learn how toointegrate ScreenSteps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4f12f-105">Integrace ScreenSteps s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="4f12f-105">Integrating ScreenSteps with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4f12f-106">Můžete ovládat ve službě Azure AD, který má přístup tooScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="4f12f-106">You can control in Azure AD who has access tooScreenSteps.</span></span>
- <span data-ttu-id="4f12f-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooScreenSteps (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4f12f-107">You can enable your users tooautomatically get signed-on tooScreenSteps (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="4f12f-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4f12f-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="4f12f-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4f12f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f12f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4f12f-110">Prerequisites</span></span>

<span data-ttu-id="4f12f-111">Integrace služby Azure AD s ScreenSteps tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="4f12f-111">tooconfigure Azure AD integration with ScreenSteps, you need hello following items:</span></span>

- <span data-ttu-id="4f12f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="4f12f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4f12f-113">ScreenSteps jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="4f12f-113">A ScreenSteps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4f12f-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f12f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4f12f-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="4f12f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4f12f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="4f12f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4f12f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4f12f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4f12f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="4f12f-118">Scenario description</span></span>
<span data-ttu-id="4f12f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="4f12f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4f12f-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="4f12f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4f12f-121">Přidání ScreenSteps z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4f12f-121">Adding ScreenSteps from hello gallery</span></span>
2. <span data-ttu-id="4f12f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4f12f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-screensteps-from-hello-gallery"></a><span data-ttu-id="4f12f-123">Přidání ScreenSteps z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4f12f-123">Adding ScreenSteps from hello gallery</span></span>
<span data-ttu-id="4f12f-124">tooconfigure hello integrace ScreenSteps do Azure AD, je nutné tooadd ScreenSteps hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="4f12f-124">tooconfigure hello integration of ScreenSteps into Azure AD, you need tooadd ScreenSteps from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4f12f-125">**tooadd ScreenSteps z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4f12f-125">**tooadd ScreenSteps from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4f12f-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4f12f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="4f12f-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4f12f-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="4f12f-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4f12f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="4f12f-133">Hello vyhledávacího pole zadejte **ScreenSteps**, vyberte **ScreenSteps** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f12f-133">In hello search box, type **ScreenSteps**, select **ScreenSteps** from result panel then click **Add** button tooadd hello application.</span></span>

    ![ScreenSteps v seznamu výsledků hello](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="4f12f-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4f12f-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="4f12f-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ScreenSteps podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="4f12f-136">In this section, you configure and test Azure AD single sign-on with ScreenSteps based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4f12f-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ScreenSteps je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4f12f-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ScreenSteps is tooa user in Azure AD.</span></span> <span data-ttu-id="4f12f-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ScreenSteps musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="4f12f-138">In other words, a link relationship between an Azure AD user and hello related user in ScreenSteps needs toobe established.</span></span>

<span data-ttu-id="4f12f-139">V ScreenSteps, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="4f12f-139">In ScreenSteps, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4f12f-140">tooconfigure a testu Azure AD jednotné přihlašování s ScreenSteps, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="4f12f-140">tooconfigure and test Azure AD single sign-on with ScreenSteps, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4f12f-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="4f12f-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4f12f-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4f12f-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4f12f-143">**[Vytvoření zkušebního uživatele ScreenSteps](#create-a-screensteps-test-user)**  -toohave protějšek Britta Simon v ScreenSteps, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f12f-143">**[Create a ScreenSteps test user](#create-a-screensteps-test-user)** - toohave a counterpart of Britta Simon in ScreenSteps that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4f12f-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4f12f-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4f12f-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="4f12f-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="4f12f-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4f12f-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="4f12f-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="4f12f-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ScreenSteps application.</span></span>

<span data-ttu-id="4f12f-148">**tooconfigure Azure AD jednotné přihlašování s ScreenSteps, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4f12f-148">**tooconfigure Azure AD single sign-on with ScreenSteps, perform hello following steps:**</span></span>

1. <span data-ttu-id="4f12f-149">V portálu Azure, na hello hello **ScreenSteps** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-149">In hello Azure portal, on hello **ScreenSteps** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="4f12f-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4f12f-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. <span data-ttu-id="4f12f-153">Na hello **ScreenSteps domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="4f12f-153">On hello **ScreenSteps Domain and URLs** section, perform hello following steps:</span></span>

    ![ScreenSteps domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    <span data-ttu-id="4f12f-155">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.ScreenSteps.com`</span><span class="sxs-lookup"><span data-stu-id="4f12f-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.ScreenSteps.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4f12f-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="4f12f-156">This value is not real.</span></span> <span data-ttu-id="4f12f-157">Aktualizujte tuto hodnotu s hello skutečné přihlašování adresu URL, která se vysvětluje dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4f12f-157">Update this value with hello actual Sign-On URL, which is explained later in this tutorial.</span></span> 

4. <span data-ttu-id="4f12f-158">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="4f12f-158">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. <span data-ttu-id="4f12f-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4f12f-160">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4f12f-162">Na hello **ScreenSteps konfigurace** klikněte na tlačítko **konfigurace ScreenSteps** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="4f12f-162">On hello **ScreenSteps Configuration** section, click **Configure ScreenSteps** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4f12f-163">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="4f12f-163">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace ScreenSteps](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. <span data-ttu-id="4f12f-165">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="4f12f-165">In a different web browser window, log into your ScreenSteps company site as an administrator.</span></span>

8. <span data-ttu-id="4f12f-166">Klikněte na tlačítko **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-166">Click **Account Settings**.</span></span>

    <span data-ttu-id="4f12f-167">![Správa účtů](./media/active-directory-saas-screensteps-tutorial/ic778523.png "Správa účtů")</span><span class="sxs-lookup"><span data-stu-id="4f12f-167">![Account management](./media/active-directory-saas-screensteps-tutorial/ic778523.png "Account management")</span></span>

9. <span data-ttu-id="4f12f-168">Klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-168">Click **Single Sign-on**.</span></span>

    <span data-ttu-id="4f12f-169">![Vzdálené ověření](./media/active-directory-saas-screensteps-tutorial/ic778524.png "vzdáleného ověřování")</span><span class="sxs-lookup"><span data-stu-id="4f12f-169">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778524.png "Remote authentication")</span></span>

10. <span data-ttu-id="4f12f-170">Klikněte na tlačítko **vytvořit Endpoint přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-170">Click **Create Single Sign-on Endpoint**.</span></span>

    <span data-ttu-id="4f12f-171">![Vzdálené ověření](./media/active-directory-saas-screensteps-tutorial/ic778525.png "vzdáleného ověřování")</span><span class="sxs-lookup"><span data-stu-id="4f12f-171">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778525.png "Remote authentication")</span></span>

11. <span data-ttu-id="4f12f-172">V hello **vytvořit jeden přihlašování koncový bod** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="4f12f-172">In hello **Create Single Sign-on Endpoint** section, perform hello following steps:</span></span>

    <span data-ttu-id="4f12f-173">![Vytvořit koncový bod ověřování](./media/active-directory-saas-screensteps-tutorial/ic778526.png "vytvořit koncový bod ověřování")</span><span class="sxs-lookup"><span data-stu-id="4f12f-173">![Create an authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778526.png "Create an authentication endpoint")</span></span>
    
    <span data-ttu-id="4f12f-174">a.</span><span class="sxs-lookup"><span data-stu-id="4f12f-174">a.</span></span> <span data-ttu-id="4f12f-175">V hello **název** textovému poli, zadejte název.</span><span class="sxs-lookup"><span data-stu-id="4f12f-175">In hello **Title** textbox, type a title.</span></span>
    
    <span data-ttu-id="4f12f-176">b.</span><span class="sxs-lookup"><span data-stu-id="4f12f-176">b.</span></span> <span data-ttu-id="4f12f-177">Z hello **režimu** seznamu, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-177">From hello **Mode** list, select **SAML**.</span></span>
    
    <span data-ttu-id="4f12f-178">c.</span><span class="sxs-lookup"><span data-stu-id="4f12f-178">c.</span></span> <span data-ttu-id="4f12f-179">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-179">Click **Create**.</span></span>

12. <span data-ttu-id="4f12f-180">**Upravit** hello nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="4f12f-180">**Edit** hello new endpoint.</span></span>

    <span data-ttu-id="4f12f-181">![Upravit koncový bod](./media/active-directory-saas-screensteps-tutorial/ic778528.png "upravit koncový bod")</span><span class="sxs-lookup"><span data-stu-id="4f12f-181">![Edit endpoint](./media/active-directory-saas-screensteps-tutorial/ic778528.png "Edit endpoint")</span></span>

13. <span data-ttu-id="4f12f-182">V hello **upravit jeden přihlašování koncový bod** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="4f12f-182">In hello **Edit Single Sign-on Endpoint** section, perform hello following steps:</span></span>

    <span data-ttu-id="4f12f-183">![Koncový bod ověřování. vzdálený](./media/active-directory-saas-screensteps-tutorial/ic778527.png "koncový bod vzdálené ověřování.")</span><span class="sxs-lookup"><span data-stu-id="4f12f-183">![Remote authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778527.png "Remote authentication endpoint")</span></span>

    <span data-ttu-id="4f12f-184">a.</span><span class="sxs-lookup"><span data-stu-id="4f12f-184">a.</span></span> <span data-ttu-id="4f12f-185">Klikněte na tlačítko **nový soubor certifikátu SAML nahrávání**a pak nahrajte hello certifikát, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4f12f-185">Click **Upload new SAML Certificate file**, and then upload hello certificate, which you have downloaded from Azure portal.</span></span>
    
    <span data-ttu-id="4f12f-186">b.</span><span class="sxs-lookup"><span data-stu-id="4f12f-186">b.</span></span> <span data-ttu-id="4f12f-187">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **vzdálené adresy URL pro přihlášení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="4f12f-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **Remote Login URL** textbox.</span></span>
    
    <span data-ttu-id="4f12f-188">c.</span><span class="sxs-lookup"><span data-stu-id="4f12f-188">c.</span></span> <span data-ttu-id="4f12f-189">Vložení **Sign-Out URL** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **URL odhlašovací stránky** textové pole.</span><span class="sxs-lookup"><span data-stu-id="4f12f-189">Paste **Sign-Out URL** value, which you have copied from hello Azure portal into hello **Log out URL** textbox.</span></span>
    
    <span data-ttu-id="4f12f-190">d.</span><span class="sxs-lookup"><span data-stu-id="4f12f-190">d.</span></span> <span data-ttu-id="4f12f-191">Vyberte **skupiny** toowhen tooassign uživatelů, které byly povoleny.</span><span class="sxs-lookup"><span data-stu-id="4f12f-191">Select a **Group** tooassign users toowhen they are provisioned.</span></span>
    
    <span data-ttu-id="4f12f-192">e.</span><span class="sxs-lookup"><span data-stu-id="4f12f-192">e.</span></span> <span data-ttu-id="4f12f-193">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-193">Click **Update**.</span></span>

    <span data-ttu-id="4f12f-194">f.</span><span class="sxs-lookup"><span data-stu-id="4f12f-194">f.</span></span> <span data-ttu-id="4f12f-195">Kopírování hello **SAML příjemce URL** toohello schránky a vložte toohello **přihlašovací adresa URL** textového pole v **ScreenSteps domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="4f12f-195">Copy hello **SAML Consumer URL** toohello clipboard and paste in toohello **Sign-on URL** textbox in **ScreenSteps Domain and URLs** section.</span></span>
    
    <span data-ttu-id="4f12f-196">g.</span><span class="sxs-lookup"><span data-stu-id="4f12f-196">g.</span></span> <span data-ttu-id="4f12f-197">Vrátí toohello **upravit jeden přihlašování koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-197">Return toohello **Edit Single Sign-on Endpoint**.</span></span>
    
    <span data-ttu-id="4f12f-198">h.</span><span class="sxs-lookup"><span data-stu-id="4f12f-198">h.</span></span> <span data-ttu-id="4f12f-199">Klikněte na tlačítko hello **nastavit jako výchozí pro účet** tlačítko toouse tento koncový bod pro všechny uživatele, kteří se připojují do ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="4f12f-199">Click hello **Make default for account** button toouse this endpoint for all users who log into ScreenSteps.</span></span> <span data-ttu-id="4f12f-200">Případně můžete kliknutím na tlačítko hello **přidat tooSite** tlačítko toouse tento koncový bod pro určité weby v **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-200">Alternatively you can click hello **Add tooSite** button toouse this endpoint for specific sites in **ScreenSteps**.</span></span>

> [!TIP]
> <span data-ttu-id="4f12f-201">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="4f12f-201">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4f12f-202">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="4f12f-202">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4f12f-203">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4f12f-203">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="4f12f-204">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4f12f-204">Create an Azure AD test user</span></span>

<span data-ttu-id="4f12f-205">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="4f12f-205">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="4f12f-207">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4f12f-207">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4f12f-208">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4f12f-208">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="4f12f-210">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-210">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="4f12f-212">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4f12f-212">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="4f12f-214">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="4f12f-214">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    <span data-ttu-id="4f12f-216">a.</span><span class="sxs-lookup"><span data-stu-id="4f12f-216">a.</span></span> <span data-ttu-id="4f12f-217">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-217">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4f12f-218">b.</span><span class="sxs-lookup"><span data-stu-id="4f12f-218">b.</span></span> <span data-ttu-id="4f12f-219">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4f12f-219">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="4f12f-220">c.</span><span class="sxs-lookup"><span data-stu-id="4f12f-220">c.</span></span> <span data-ttu-id="4f12f-221">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="4f12f-221">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="4f12f-222">d.</span><span class="sxs-lookup"><span data-stu-id="4f12f-222">d.</span></span> <span data-ttu-id="4f12f-223">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-223">Click **Create**.</span></span>
 
### <a name="create-a-screensteps-test-user"></a><span data-ttu-id="4f12f-224">Vytvoření zkušebního uživatele ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="4f12f-224">Create a ScreenSteps test user</span></span>

<span data-ttu-id="4f12f-225">V této části vytvoříte volal Britta Simon v ScreenSteps uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f12f-225">In this section, you create a user called Britta Simon in ScreenSteps.</span></span> <span data-ttu-id="4f12f-226">Práce s [tým podpory ScreenSteps klienta](https://www.screensteps.com/contact) pro přidání uživatelů hello hello ScreenSteps platformy.</span><span class="sxs-lookup"><span data-stu-id="4f12f-226">Work with [ScreenSteps Client support team](https://www.screensteps.com/contact) to add hello users in hello ScreenSteps platform.</span></span> <span data-ttu-id="4f12f-227">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4f12f-227">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="4f12f-228">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="4f12f-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="4f12f-229">V této části povolíte tak, že udělíte přístup tooScreenSteps toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4f12f-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooScreenSteps.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="4f12f-231">**tooassign Britta Simon tooScreenSteps, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4f12f-231">**tooassign Britta Simon tooScreenSteps, perform hello following steps:**</span></span>

1. <span data-ttu-id="4f12f-232">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="4f12f-234">V seznamu aplikace hello vyberte **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-234">In hello applications list, select **ScreenSteps**.</span></span>

    ![v seznamu aplikace hello Hello ScreenSteps odkaz](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. <span data-ttu-id="4f12f-236">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="4f12f-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="4f12f-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4f12f-238">Click **Add** button.</span></span> <span data-ttu-id="4f12f-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4f12f-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="4f12f-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="4f12f-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4f12f-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4f12f-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4f12f-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4f12f-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="4f12f-244">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4f12f-244">Test single sign-on</span></span>

<span data-ttu-id="4f12f-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="4f12f-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4f12f-246">Po kliknutí na tlačítko hello ScreenSteps dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ScreenSteps aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f12f-246">When you click hello ScreenSteps tile in hello Access Panel, you should get automatically signed-on tooyour ScreenSteps application.</span></span>
<span data-ttu-id="4f12f-247">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4f12f-247">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4f12f-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4f12f-248">Additional resources</span></span>

* [<span data-ttu-id="4f12f-249">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4f12f-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4f12f-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4f12f-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

