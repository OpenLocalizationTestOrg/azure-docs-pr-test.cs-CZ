---
title: "Kurz: Azure Active Directory integrace s TOPdesk - veřejné | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TOPdesk - veřejné."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ef0dd06157ecc3b33814590039f5cbae64e8c916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a><span data-ttu-id="75326-103">Kurz: Azure Active Directory integrace s TOPdesk - veřejné</span><span class="sxs-lookup"><span data-stu-id="75326-103">Tutorial: Azure Active Directory integration with TOPdesk - Public</span></span>

<span data-ttu-id="75326-104">V tomto kurzu zjistíte, jak toointegrate TOPdesk - veřejné službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="75326-104">In this tutorial, you learn how toointegrate TOPdesk - Public with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="75326-105">Integrace TOPdesk - veřejné s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="75326-105">Integrating TOPdesk - Public with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="75326-106">Můžete ovládat ve službě Azure AD, který má přístup tooTOPdesk - veřejné.</span><span class="sxs-lookup"><span data-stu-id="75326-106">You can control in Azure AD who has access tooTOPdesk - Public.</span></span>
- <span data-ttu-id="75326-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTOPdesk - veřejné (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="75326-107">You can enable your users tooautomatically get signed-on tooTOPdesk - Public (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="75326-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="75326-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="75326-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="75326-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75326-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="75326-110">Prerequisites</span></span>

<span data-ttu-id="75326-111">tooconfigure integrace Azure AD s TOPdesk - veřejné, budete potřebovat hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="75326-111">tooconfigure Azure AD integration with TOPdesk - Public, you need hello following items:</span></span>

- <span data-ttu-id="75326-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="75326-112">An Azure AD subscription</span></span>
- <span data-ttu-id="75326-113">TOPdesk - veřejné jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="75326-113">A TOPdesk - Public single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="75326-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="75326-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="75326-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="75326-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="75326-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="75326-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="75326-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75326-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="75326-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="75326-118">Scenario description</span></span>
<span data-ttu-id="75326-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="75326-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="75326-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="75326-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="75326-121">Přidání TOPdesk - veřejné z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="75326-121">Adding TOPdesk - Public from hello gallery</span></span>
2. <span data-ttu-id="75326-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="75326-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-topdesk---public-from-hello-gallery"></a><span data-ttu-id="75326-123">Přidání TOPdesk - veřejné z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="75326-123">Adding TOPdesk - Public from hello gallery</span></span>
<span data-ttu-id="75326-124">integrace hello tooconfigure TOPdesk - veřejné do služby Azure AD, je nutné tooadd TOPdesk - veřejné hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="75326-124">tooconfigure hello integration of TOPdesk - Public into Azure AD, you need tooadd TOPdesk - Public from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="75326-125">**tooadd TOPdesk - veřejné z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="75326-125">**tooadd TOPdesk - Public from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="75326-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="75326-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="75326-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="75326-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="75326-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="75326-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="75326-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="75326-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="75326-133">Hello vyhledávacího pole zadejte **TOPdesk - veřejné**, vyberte **TOPdesk - veřejné** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="75326-133">In hello search box, type **TOPdesk - Public**, select **TOPdesk - Public** from result panel then click **Add** button tooadd hello application.</span></span>

    ![TOPdesk - veřejné v seznamu výsledků hello](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="75326-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="75326-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="75326-136">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s TOPdesk – veřejné podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="75326-136">In this section, you configure and test Azure AD single sign-on with TOPdesk - Public based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="75326-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v TOPdesk – veřejné je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="75326-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TOPdesk - Public is tooa user in Azure AD.</span></span> <span data-ttu-id="75326-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v TOPdesk - veřejné musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="75326-138">In other words, a link relationship between an Azure AD user and hello related user in TOPdesk - Public needs toobe established.</span></span>

<span data-ttu-id="75326-139">V TOPdesk - Public, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="75326-139">In TOPdesk - Public, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="75326-140">tooconfigure a test Azure AD jednotné přihlašování s TOPdesk - veřejné, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="75326-140">tooconfigure and test Azure AD single sign-on with TOPdesk - Public, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="75326-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="75326-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="75326-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75326-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="75326-143">**[Vytvoření TOPdesk - veřejné testovacího uživatele](#create-a-topdesk---public-test-user)**  - toohave protějšek Britta Simon v TOPdesk - Public, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="75326-143">**[Create a TOPdesk - Public test user](#create-a-topdesk---public-test-user)** - toohave a counterpart of Britta Simon in TOPdesk - Public that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="75326-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="75326-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="75326-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="75326-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="75326-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="75326-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="75326-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší TOPdesk - veřejné aplikace.</span><span class="sxs-lookup"><span data-stu-id="75326-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TOPdesk - Public application.</span></span>

<span data-ttu-id="75326-148">**tooconfigure Azure AD jednotné přihlašování s TOPdesk - veřejné, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="75326-148">**tooconfigure Azure AD single sign-on with TOPdesk - Public, perform hello following steps:**</span></span>

1. <span data-ttu-id="75326-149">V portálu Azure, na hello hello **TOPdesk - veřejné** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="75326-149">In hello Azure portal, on hello **TOPdesk - Public** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="75326-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="75326-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. <span data-ttu-id="75326-153">Na hello **TOPdesk - veřejnou doménu a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="75326-153">On hello **TOPdesk - Public Domain and URLs** section, perform hello following steps:</span></span>

    ![Jednotné přihlašování informace TOPdesk - veřejnou doménu a adresy URL](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    <span data-ttu-id="75326-155">a.</span><span class="sxs-lookup"><span data-stu-id="75326-155">a.</span></span> <span data-ttu-id="75326-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.topdesk.net`</span><span class="sxs-lookup"><span data-stu-id="75326-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.topdesk.net`</span></span>
    
    <span data-ttu-id="75326-157">b.</span><span class="sxs-lookup"><span data-stu-id="75326-157">b.</span></span> <span data-ttu-id="75326-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.topdesk.net/tas/public/login/verify`</span><span class="sxs-lookup"><span data-stu-id="75326-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.topdesk.net/tas/public/login/verify`</span></span>

    <span data-ttu-id="75326-159">c.</span><span class="sxs-lookup"><span data-stu-id="75326-159">c.</span></span> <span data-ttu-id="75326-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.topdesk.net/tas/public/login/saml`</span><span class="sxs-lookup"><span data-stu-id="75326-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.topdesk.net/tas/public/login/saml`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="75326-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="75326-161">These values are not real.</span></span> <span data-ttu-id="75326-162">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="75326-162">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="75326-163">Adresa URL odpovědi je explaned později v kurzu.</span><span class="sxs-lookup"><span data-stu-id="75326-163">Reply URL is explaned later in tutorial.</span></span> <span data-ttu-id="75326-164">Obraťte se na [TOPdesk - tým podpory veřejné klienta](https://help.topdesk.com/saas/enterprise/user/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="75326-164">Contact [TOPdesk - Public Client support team](https://help.topdesk.com/saas/enterprise/user/) tooget these values.</span></span>  

4. <span data-ttu-id="75326-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="75326-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. <span data-ttu-id="75326-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="75326-167">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="75326-169">Na hello **TOPdesk - veřejné konfigurace** klikněte na tlačítko **konfigurace TOPdesk - veřejné** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="75326-169">On hello **TOPdesk - Public Configuration** section, click **Configure TOPdesk - Public** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="75326-170">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="75326-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![TOPdesk - veřejné konfigurace](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. <span data-ttu-id="75326-172">Přihlaste se tooyour **TOPdesk - veřejné** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="75326-172">Sign on tooyour **TOPdesk - Public** company site as an administrator.</span></span>

8. <span data-ttu-id="75326-173">V hello **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="75326-173">In hello **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="75326-174">![Nastavení](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="75326-174">![Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "Settings")</span></span>

9. <span data-ttu-id="75326-175">Klikněte na tlačítko **nastavení přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="75326-175">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="75326-176">![Nastavení přihlášení](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "nastavení přihlášení")</span><span class="sxs-lookup"><span data-stu-id="75326-176">![Login Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "Login Settings")</span></span>

10. <span data-ttu-id="75326-177">Rozbalte hello **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="75326-177">Expand hello **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="75326-178">![Obecné](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "obecné")</span><span class="sxs-lookup"><span data-stu-id="75326-178">![General](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "General")</span></span>

11. <span data-ttu-id="75326-179">V hello **veřejné** části hello **SAML přihlášení** konfigurace části, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="75326-179">In hello **Public** section of hello **SAML login** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="75326-180">![Technické nastavení](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "technické nastavení")</span><span class="sxs-lookup"><span data-stu-id="75326-180">![Technical Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "Technical Settings")</span></span>
   
    <span data-ttu-id="75326-181">a.</span><span class="sxs-lookup"><span data-stu-id="75326-181">a.</span></span> <span data-ttu-id="75326-182">Klikněte na tlačítko **Stáhnout** toodownload hello veřejné metadata souboru a poté je uložit místně v počítači.</span><span class="sxs-lookup"><span data-stu-id="75326-182">Click **Download** toodownload hello public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="75326-183">b.</span><span class="sxs-lookup"><span data-stu-id="75326-183">b.</span></span> <span data-ttu-id="75326-184">Otevřete soubor metadat hello stáhli a vyhledejte hello **AssertionConsumerService** uzlu.</span><span class="sxs-lookup"><span data-stu-id="75326-184">Open hello downloaded metadata file, and then locate hello **AssertionConsumerService** node.</span></span>

    <span data-ttu-id="75326-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span><span class="sxs-lookup"><span data-stu-id="75326-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span></span>
   
    <span data-ttu-id="75326-186">c.</span><span class="sxs-lookup"><span data-stu-id="75326-186">c.</span></span> <span data-ttu-id="75326-187">Kopírování hello **AssertionConsumerService** hodnotu, vložte tuto hodnotu hello **adresa URL odpovědi** textového pole v **TOPdesk - veřejnou doménu a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="75326-187">Copy hello **AssertionConsumerService** value, paste this value in hello **Reply URL** textbox in **TOPdesk - Public Domain and URLs** section.</span></span>      
   
12. <span data-ttu-id="75326-188">toocreate soubor certifikátu, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="75326-188">toocreate a certificate file, perform hello following steps:</span></span>
    
    <span data-ttu-id="75326-189">![Certifikát](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "certifikátu")</span><span class="sxs-lookup"><span data-stu-id="75326-189">![Certificate](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "Certificate")</span></span>
    
    <span data-ttu-id="75326-190">a.</span><span class="sxs-lookup"><span data-stu-id="75326-190">a.</span></span> <span data-ttu-id="75326-191">Otevřete hello stáhnout soubor metadat z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="75326-191">Open hello downloaded metadata file from Azure portal.</span></span>
    
    <span data-ttu-id="75326-192">b.</span><span class="sxs-lookup"><span data-stu-id="75326-192">b.</span></span> <span data-ttu-id="75326-193">Rozbalte hello **RoleDescriptor** uzlu, který má **xsi: type** z **dodáni: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="75326-193">Expand hello **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    
    <span data-ttu-id="75326-194">c.</span><span class="sxs-lookup"><span data-stu-id="75326-194">c.</span></span> <span data-ttu-id="75326-195">Zkopírujte hodnotu hello hello **certifikátu x 509** uzlu.</span><span class="sxs-lookup"><span data-stu-id="75326-195">Copy hello value of hello **X509Certificate** node.</span></span>
    
    <span data-ttu-id="75326-196">d.</span><span class="sxs-lookup"><span data-stu-id="75326-196">d.</span></span> <span data-ttu-id="75326-197">Uložit hello zkopírovat **certifikátu x 509** hodnota místně na vašem počítači v souboru.</span><span class="sxs-lookup"><span data-stu-id="75326-197">Save hello copied **X509Certificate** value locally on your computer in a file.</span></span>

13. <span data-ttu-id="75326-198">V hello **veřejné** klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="75326-198">In hello **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="75326-199">![Přihlašování SAML](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML přihlášení")</span><span class="sxs-lookup"><span data-stu-id="75326-199">![SAML Login](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML Login")</span></span>

14. <span data-ttu-id="75326-200">Na hello **pomocníka konfigurace SAML** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="75326-200">On hello **SAML configuration assistant** dialog page, perform hello following steps:</span></span>
    
    <span data-ttu-id="75326-201">![Pomocník pro konfigurace SAML](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "pomocníka konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="75326-201">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="75326-202">a.</span><span class="sxs-lookup"><span data-stu-id="75326-202">a.</span></span> <span data-ttu-id="75326-203">tooupload stažené metadata souboru z portálu Azure, v části **federačních metadat**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="75326-203">tooupload your downloaded metadata file from Azure portal, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="75326-204">b.</span><span class="sxs-lookup"><span data-stu-id="75326-204">b.</span></span> <span data-ttu-id="75326-205">tooupload svůj certifikát v části souboru **certifikát (RSA)**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="75326-205">tooupload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="75326-206">c.</span><span class="sxs-lookup"><span data-stu-id="75326-206">c.</span></span> <span data-ttu-id="75326-207">Soubor loga hello tooupload jste získali od týmu podpory TOPdesk hello, v části **Logo ikonu**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="75326-207">tooupload hello logo file you got from hello TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="75326-208">d.</span><span class="sxs-lookup"><span data-stu-id="75326-208">d.</span></span> <span data-ttu-id="75326-209">V hello **atribut uživatelského jména** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="75326-209">In hello **User name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="75326-210">e.</span><span class="sxs-lookup"><span data-stu-id="75326-210">e.</span></span> <span data-ttu-id="75326-211">V hello **zobrazovaný název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="75326-211">In hello **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="75326-212">f.</span><span class="sxs-lookup"><span data-stu-id="75326-212">f.</span></span> <span data-ttu-id="75326-213">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="75326-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="75326-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="75326-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="75326-215">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="75326-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="75326-216">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="75326-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="75326-217">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="75326-217">Create an Azure AD test user</span></span>

<span data-ttu-id="75326-218">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="75326-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="75326-220">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="75326-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="75326-221">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="75326-221">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="75326-223">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="75326-223">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="75326-225">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75326-225">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="75326-227">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="75326-227">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    <span data-ttu-id="75326-229">a.</span><span class="sxs-lookup"><span data-stu-id="75326-229">a.</span></span> <span data-ttu-id="75326-230">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="75326-230">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="75326-231">b.</span><span class="sxs-lookup"><span data-stu-id="75326-231">b.</span></span> <span data-ttu-id="75326-232">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75326-232">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="75326-233">c.</span><span class="sxs-lookup"><span data-stu-id="75326-233">c.</span></span> <span data-ttu-id="75326-234">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="75326-234">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="75326-235">d.</span><span class="sxs-lookup"><span data-stu-id="75326-235">d.</span></span> <span data-ttu-id="75326-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="75326-236">Click **Create**.</span></span>
 
### <a name="create-a-topdesk---public-test-user"></a><span data-ttu-id="75326-237">Vytvoření TOPdesk - veřejné testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="75326-237">Create a TOPdesk - Public test user</span></span>

<span data-ttu-id="75326-238">V pořadí tooenable Azure AD Uživatelé toolog do TOPdesk - veřejné, musí být zřízená do TOPdesk - veřejné.</span><span class="sxs-lookup"><span data-stu-id="75326-238">In order tooenable Azure AD users toolog into TOPdesk - Public, they must be provisioned into TOPdesk - Public.</span></span>  
<span data-ttu-id="75326-239">V případě hello TOPdesk - veřejné, zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="75326-239">In hello case of TOPdesk - Public, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="75326-240">tooconfigure zřizování uživatelů, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="75326-240">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="75326-241">Přihlaste se tooyour **TOPdesk - veřejné** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="75326-241">Sign on tooyour **TOPdesk - Public** company site as administrator.</span></span>

2. <span data-ttu-id="75326-242">V nabídce hello hello nahoře, klikněte na tlačítko **TOPdesk \> nový \> soubory podpory \> osoba**.</span><span class="sxs-lookup"><span data-stu-id="75326-242">In hello menu on hello top, click **TOPdesk \> New \> Support Files \> Person**.</span></span>
   
    <span data-ttu-id="75326-243">![Osoba](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "osoba")</span><span class="sxs-lookup"><span data-stu-id="75326-243">![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")</span></span>

3. <span data-ttu-id="75326-244">V dialogovém okně hello nové osobě proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="75326-244">On hello New Person dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="75326-245">![Nové osobě](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "nové osobě")</span><span class="sxs-lookup"><span data-stu-id="75326-245">![New Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "New Person")</span></span>
   
    <span data-ttu-id="75326-246">a.</span><span class="sxs-lookup"><span data-stu-id="75326-246">a.</span></span> <span data-ttu-id="75326-247">Klikněte na kartu Obecné hello.</span><span class="sxs-lookup"><span data-stu-id="75326-247">Click hello General tab.</span></span>

    <span data-ttu-id="75326-248">b.</span><span class="sxs-lookup"><span data-stu-id="75326-248">b.</span></span> <span data-ttu-id="75326-249">V hello **Přezdívka** textovému poli, zadejte příjmení uživatele hello jako Simon</span><span class="sxs-lookup"><span data-stu-id="75326-249">In hello **Surname** textbox, type Surname of hello user like Simon</span></span>
 
    <span data-ttu-id="75326-250">c.</span><span class="sxs-lookup"><span data-stu-id="75326-250">c.</span></span> <span data-ttu-id="75326-251">Vyberte **lokality** pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="75326-251">Select a **Site** for hello account.</span></span>
 
    <span data-ttu-id="75326-252">d.</span><span class="sxs-lookup"><span data-stu-id="75326-252">d.</span></span> <span data-ttu-id="75326-253">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="75326-253">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="75326-254">Můžete použít jiné TOPdesk – nástroje pro tvorbu veřejné uživatelského účtu nebo rozhraní API poskytovaných TOPdesk - veřejné tooprovision Azure AD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="75326-254">You can use any other TOPdesk - Public user account creation tools or APIs provided by TOPdesk - Public tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="75326-255">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="75326-255">Assign hello Azure AD test user</span></span>

<span data-ttu-id="75326-256">V této části povolíte tak, že udělíte přístup tooTOPdesk - veřejné Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="75326-256">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTOPdesk - Public.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="75326-258">**tooassign Britta Simon tooTOPdesk - veřejné, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="75326-258">**tooassign Britta Simon tooTOPdesk - Public, perform hello following steps:**</span></span>

1. <span data-ttu-id="75326-259">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="75326-259">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="75326-261">V seznamu aplikace hello vyberte **TOPdesk - veřejné**.</span><span class="sxs-lookup"><span data-stu-id="75326-261">In hello applications list, select **TOPdesk - Public**.</span></span>

    ![Hello TOPdesk - veřejné odkaz v seznamu aplikace hello](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. <span data-ttu-id="75326-263">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="75326-263">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="75326-265">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="75326-265">Click **Add** button.</span></span> <span data-ttu-id="75326-266">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75326-266">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="75326-268">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="75326-268">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="75326-269">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75326-269">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="75326-270">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75326-270">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="75326-271">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="75326-271">Test single sign-on</span></span>

<span data-ttu-id="75326-272">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="75326-272">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="75326-273">Po kliknutí na tlačítko hello TOPdesk - veřejné dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour TOPdesk - veřejné aplikace.</span><span class="sxs-lookup"><span data-stu-id="75326-273">When you click hello TOPdesk - Public tile in hello Access Panel, you should get automatically signed-on tooyour TOPdesk - Public application.</span></span>
<span data-ttu-id="75326-274">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="75326-274">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="75326-275">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="75326-275">Additional resources</span></span>

* [<span data-ttu-id="75326-276">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="75326-276">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="75326-277">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="75326-277">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

