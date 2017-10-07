---
title: "Kurz: Azure Active Directory integrace s přenosy MOVEit – integrace Azure AD | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a přenos MOVEit – integrace Azure AD."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 5bbe4f2d952bd45c4d58d55ffc3467b4eb871fd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a><span data-ttu-id="6e19e-103">Kurz: Azure Active Directory integrace s přenosy MOVEit – integrace Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e19e-103">Tutorial: Azure Active Directory integration with MOVEit Transfer - Azure AD integration</span></span>

<span data-ttu-id="6e19e-104">V tomto kurzu zjistíte, jak toointegrate MOVEit přenos – integrace Azure AD s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6e19e-104">In this tutorial, you learn how toointegrate MOVEit Transfer - Azure AD integration with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6e19e-105">Integrace MOVEit přenos – Azure AD integraci s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6e19e-105">Integrating MOVEit Transfer - Azure AD integration with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6e19e-106">Můžete ovládat ve službě Azure AD, který má přístup tooMOVEit přenos – integrace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e19e-106">You can control in Azure AD who has access tooMOVEit Transfer - Azure AD integration.</span></span>
- <span data-ttu-id="6e19e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMOVEit přenos – integrace Azure AD (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e19e-107">You can enable your users tooautomatically get signed-on tooMOVEit Transfer - Azure AD integration (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="6e19e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6e19e-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="6e19e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6e19e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e19e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6e19e-110">Prerequisites</span></span>

<span data-ttu-id="6e19e-111">tooconfigure integrace Azure AD s přenosy MOVEit – integrace služby Azure AD, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6e19e-111">tooconfigure Azure AD integration with MOVEit Transfer - Azure AD integration, you need hello following items:</span></span>

- <span data-ttu-id="6e19e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e19e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6e19e-113">Přenos MOVEit – Azure AD integrace jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6e19e-113">A MOVEit Transfer - Azure AD integration single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6e19e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e19e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6e19e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6e19e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6e19e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6e19e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6e19e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6e19e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6e19e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6e19e-118">Scenario description</span></span>
<span data-ttu-id="6e19e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e19e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6e19e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6e19e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6e19e-121">Přidání MOVEit přenos – integrace Azure AD z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6e19e-121">Adding MOVEit Transfer - Azure AD integration from hello gallery</span></span>
2. <span data-ttu-id="6e19e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e19e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moveit-transfer---azure-ad-integration-from-hello-gallery"></a><span data-ttu-id="6e19e-123">Přidání MOVEit přenos – integrace Azure AD z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6e19e-123">Adding MOVEit Transfer - Azure AD integration from hello gallery</span></span>
<span data-ttu-id="6e19e-124">integrace hello tooconfigure MOVEit přenos – integrace Azure AD do služby Azure AD, je nutné tooadd MOVEit přenos – integrace Azure AD hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6e19e-124">tooconfigure hello integration of MOVEit Transfer - Azure AD integration into Azure AD, you need tooadd MOVEit Transfer - Azure AD integration from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6e19e-125">**tooadd MOVEit přenos – integrace Azure AD z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e19e-125">**tooadd MOVEit Transfer - Azure AD integration from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e19e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6e19e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="6e19e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6e19e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="6e19e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e19e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="6e19e-133">Hello vyhledávacího pole zadejte **MOVEit přenos – integrace Azure AD**, vyberte **MOVEit přenos – integrace Azure AD** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e19e-133">In hello search box, type **MOVEit Transfer - Azure AD integration**, select **MOVEit Transfer - Azure AD integration** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Přenos MOVEit – integrace Azure AD v seznamu výsledků hello](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6e19e-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e19e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="6e19e-136">V této části konfiguraci a testování Azure AD jednotné přihlašování s přenosy MOVEit – integrace Azure AD na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6e19e-136">In this section, you configure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6e19e-137">Pro toowork jeden přihlašování Azure AD musí tooknow uživatele hello protějškem v MOVEit přenos – integrace Azure AD je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e19e-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in MOVEit Transfer - Azure AD integration is tooa user in Azure AD.</span></span> <span data-ttu-id="6e19e-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v MOVEit přenos – integrace Azure AD musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6e19e-138">In other words, a link relationship between an Azure AD user and hello related user in MOVEit Transfer - Azure AD integration needs toobe established.</span></span>

<span data-ttu-id="6e19e-139">V MOVEit přenos – integrace služby Azure AD, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6e19e-139">In MOVEit Transfer - Azure AD integration, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6e19e-140">tooconfigure a testování Azure AD jednotné přihlašování s přenosy MOVEit – integrace služby Azure AD, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="6e19e-140">tooconfigure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6e19e-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6e19e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6e19e-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e19e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6e19e-143">**[Vytvoření MOVEit přenos – Azure AD integrace testovacího uživatele](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - toohave protějšek Britta Simon při přenosu MOVEit - integraci Azure AD, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6e19e-143">**[Create a MOVEit Transfer - Azure AD integration test user](#create-a-moveit-transfer---azure-ad-integration-test-user)** - toohave a counterpart of Britta Simon in MOVEit Transfer - Azure AD integration that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6e19e-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e19e-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6e19e-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6e19e-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6e19e-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e19e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6e19e-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v MOVEit přenos – integrace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e19e-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your MOVEit Transfer - Azure AD integration application.</span></span>

<span data-ttu-id="6e19e-148">**tooconfigure Azure AD jednotné přihlašování s přenosy MOVEit – integrace služby Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e19e-148">**tooconfigure Azure AD single sign-on with MOVEit Transfer - Azure AD integration, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e19e-149">V portálu Azure, na hello hello **MOVEit přenos – integrace Azure AD** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-149">In hello Azure portal, on hello **MOVEit Transfer - Azure AD integration** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="6e19e-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e19e-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. <span data-ttu-id="6e19e-153">Na hello **MOVEit přenos – integrace Azure AD domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6e19e-153">On hello **MOVEit Transfer - Azure AD integration Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    <span data-ttu-id="6e19e-155">a.</span><span class="sxs-lookup"><span data-stu-id="6e19e-155">a.</span></span> <span data-ttu-id="6e19e-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://contoso.com`</span><span class="sxs-lookup"><span data-stu-id="6e19e-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://contoso.com`</span></span>

    <span data-ttu-id="6e19e-157">b.</span><span class="sxs-lookup"><span data-stu-id="6e19e-157">b.</span></span> <span data-ttu-id="6e19e-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://contoso.com/<tenatid>`</span><span class="sxs-lookup"><span data-stu-id="6e19e-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://contoso.com/<tenatid>`</span></span>

    <span data-ttu-id="6e19e-159">c.</span><span class="sxs-lookup"><span data-stu-id="6e19e-159">c.</span></span> <span data-ttu-id="6e19e-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span><span class="sxs-lookup"><span data-stu-id="6e19e-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="6e19e-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6e19e-161">These values are not real.</span></span> <span data-ttu-id="6e19e-162">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="6e19e-162">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="6e19e-163">Najdete tyto hodnoty později v **adresa URL služby zprostředkovatele metadat** části nebo kontaktujte [MOVEit přenos – tým podpory pro Azure AD integrace klienta](https://community.ipswitch.com/s/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6e19e-163">You can refer these values later in **Service Provider Metadata URL** section or contact [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support) tooget these values.</span></span>

4. <span data-ttu-id="6e19e-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6e19e-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. <span data-ttu-id="6e19e-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e19e-166">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="6e19e-168">Přihlášení tooyour MOVEit přenos klienta jako správce.</span><span class="sxs-lookup"><span data-stu-id="6e19e-168">Sign on tooyour MOVEit Transfer tenant as an administrator.</span></span>

7. <span data-ttu-id="6e19e-169">V levém navigačním podokně hello, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-169">On hello left navigation pane, click **Settings**.</span></span>

    ![Nastavení oddílu na aplikaci straně](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. <span data-ttu-id="6e19e-171">Klikněte na tlačítko **jeden Sign-on** odkaz, což je pod **zásady zabezpečení -> Auth uživatele**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-171">Click **Single Signon** link, which is under **Security Policies -> User Auth**.</span></span>

    ![Zabezpečení straně zásady na aplikace](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. <span data-ttu-id="6e19e-173">Klikněte na tlačítko hello adresu URL metadat odkaz toodownload hello dokument metadat.</span><span class="sxs-lookup"><span data-stu-id="6e19e-173">Click hello Metadata URL link toodownload hello metadata document.</span></span>

    ![Adresa URL služby zprostředkovatele metadat](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * <span data-ttu-id="6e19e-175">Ověřte **entityID** odpovídá **identifikátor** v hello **MOVEit přenos – integrace Azure AD domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="6e19e-175">Verify **entityID** matches **Identifier** in hello **MOVEit Transfer - Azure AD integration Domain and URLs** section .</span></span>
    * <span data-ttu-id="6e19e-176">Ověřte **AssertionConsumerService** umístění URL odpovídá **adresa URL odpovědi** v hello **MOVEit přenos – integrace Azure AD domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="6e19e-176">Verify **AssertionConsumerService** Location URL matches **REPLY URL** in hello **MOVEit Transfer - Azure AD integration Domain and URLs** section.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. <span data-ttu-id="6e19e-178">Klikněte na tlačítko **přidat zprostředkovatele Identity** tlačítko tooadd nového poskytovatele federovaných identit.</span><span class="sxs-lookup"><span data-stu-id="6e19e-178">Click **Add Identity Provider** button tooadd a new Federated Identity Provider.</span></span>

    ![Přidejte zprostředkovatele Identity](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. <span data-ttu-id="6e19e-180">Klikněte na tlačítko **Procházet...**  tooselect hello metadata souboru, který jste si stáhli z portálu Azure a pak klikněte na **přidat zprostředkovatele Identity** tooupload hello stáhli soubor.</span><span class="sxs-lookup"><span data-stu-id="6e19e-180">Click **Browse...** tooselect hello metadata file which you downloaded from Azure portal, then click **Add Identity Provider** tooupload hello downloaded file.</span></span>

    ![Poskytovatele Identity SAML](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. <span data-ttu-id="6e19e-182">Vyberte "**Ano**" jako **povoleno** v hello **upravit nastavení federovaný zprostředkovatele Identity...**  a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-182">Select "**Yes**" as **Enabled** in hello **Edit Federated Identity Provider Settings...** page and click **Save**.</span></span>

    ![Nastavení zprostředkovatele federovaných identit](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. <span data-ttu-id="6e19e-184">V hello **Upravit federované Identity uživatele nastavení zprostředkovatele** proveďte hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="6e19e-184">In hello **Edit Federated Identity Provider User Settings** page, perform hello following actions:</span></span>
    
    ![Upravit nastavení zprostředkovatele federovaných identit](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    <span data-ttu-id="6e19e-186">a.</span><span class="sxs-lookup"><span data-stu-id="6e19e-186">a.</span></span> <span data-ttu-id="6e19e-187">Vyberte **SAML NameID** jako **přihlašovací jméno**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-187">Select **SAML NameID** as **Login name**.</span></span>
    
    <span data-ttu-id="6e19e-188">b.</span><span class="sxs-lookup"><span data-stu-id="6e19e-188">b.</span></span> <span data-ttu-id="6e19e-189">Vyberte **jiných** jako **úplný název** a v hello **název atributu** textbox vložte hodnotu hello: `http://schemas.microsoft.com/identity/claims/displayname`.</span><span class="sxs-lookup"><span data-stu-id="6e19e-189">Select **Other** as **Full name** and in hello **Attribute name** textbox put hello value: `http://schemas.microsoft.com/identity/claims/displayname`.</span></span>
    
    <span data-ttu-id="6e19e-190">c.</span><span class="sxs-lookup"><span data-stu-id="6e19e-190">c.</span></span> <span data-ttu-id="6e19e-191">Vyberte **jiných** jako **e-mailu** a v hello **název atributu** textbox vložte hodnotu hello: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="6e19e-191">Select **Other** as **Email** and in hello **Attribute name** textbox put hello value: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="6e19e-192">d.</span><span class="sxs-lookup"><span data-stu-id="6e19e-192">d.</span></span> <span data-ttu-id="6e19e-193">Vyberte **Ano** jako **automatické vytvoření účtu na Sign-on**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-193">Select **Yes** as **Auto-create account on signon**.</span></span>
    
    <span data-ttu-id="6e19e-194">e.</span><span class="sxs-lookup"><span data-stu-id="6e19e-194">e.</span></span> <span data-ttu-id="6e19e-195">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e19e-195">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="6e19e-196">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6e19e-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6e19e-197">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6e19e-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6e19e-198">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6e19e-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6e19e-199">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e19e-199">Create an Azure AD test user</span></span>

<span data-ttu-id="6e19e-200">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6e19e-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="6e19e-202">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e19e-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e19e-203">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e19e-203">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="6e19e-205">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-205">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="6e19e-207">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e19e-207">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="6e19e-209">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6e19e-209">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    <span data-ttu-id="6e19e-211">a.</span><span class="sxs-lookup"><span data-stu-id="6e19e-211">a.</span></span> <span data-ttu-id="6e19e-212">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-212">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6e19e-213">b.</span><span class="sxs-lookup"><span data-stu-id="6e19e-213">b.</span></span> <span data-ttu-id="6e19e-214">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e19e-214">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="6e19e-215">c.</span><span class="sxs-lookup"><span data-stu-id="6e19e-215">c.</span></span> <span data-ttu-id="6e19e-216">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="6e19e-216">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="6e19e-217">d.</span><span class="sxs-lookup"><span data-stu-id="6e19e-217">d.</span></span> <span data-ttu-id="6e19e-218">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-218">Click **Create**.</span></span>
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a><span data-ttu-id="6e19e-219">Vytvoření MOVEit přenos – Azure AD integrace testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6e19e-219">Create a MOVEit Transfer - Azure AD integration test user</span></span>

<span data-ttu-id="6e19e-220">Hello cílem této části je toocreate uživatel volal Britta Simon v MOVEit přenos – integrace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e19e-220">hello objective of this section is toocreate a user called Britta Simon in MOVEit Transfer - Azure AD integration.</span></span> <span data-ttu-id="6e19e-221">Přenos MOVEit – integrace Azure AD podporuje zřizování za běhu, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="6e19e-221">MOVEit Transfer - Azure AD integration supports just-in-time provisioning, which you have enabled.</span></span> <span data-ttu-id="6e19e-222">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="6e19e-222">There is no action item for you in this section.</span></span> <span data-ttu-id="6e19e-223">Nový uživatel se vytvoří během tooaccess pokusu o přenos MOVEit – integrace Azure AD, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="6e19e-223">A new user is created during an attempt tooaccess MOVEit Transfer - Azure AD integration if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="6e19e-224">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [MOVEit přenos – tým podpory pro Azure AD integrace klienta](https://community.ipswitch.com/s/support).</span><span class="sxs-lookup"><span data-stu-id="6e19e-224">If you need toocreate a user manually, you need toocontact hello [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="6e19e-225">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6e19e-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="6e19e-226">V této části povolíte tak, že udělíte přístup tooMOVEit přenos – integrace Azure AD toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e19e-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMOVEit Transfer - Azure AD integration.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="6e19e-228">**tooassign tooMOVEit Britta Simon přenos – integrace služby Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e19e-228">**tooassign Britta Simon tooMOVEit Transfer - Azure AD integration, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e19e-229">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6e19e-231">V seznamu aplikace hello vyberte **MOVEit přenos – integrace Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-231">In hello applications list, select **MOVEit Transfer - Azure AD integration**.</span></span>

    ![Hello MOVEit přenos – integrace Azure AD na odkaz v seznamu aplikace hello](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. <span data-ttu-id="6e19e-233">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6e19e-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="6e19e-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e19e-235">Click **Add** button.</span></span> <span data-ttu-id="6e19e-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e19e-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="6e19e-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6e19e-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6e19e-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e19e-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6e19e-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e19e-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6e19e-241">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e19e-241">Test single sign-on</span></span>

<span data-ttu-id="6e19e-242">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="6e19e-242">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="6e19e-243">Když kliknete na tlačítko hello MOVEit přenos – dlaždici integrace Azure AD v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour MOVEit přenos – integrace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e19e-243">When you click hello MOVEit Transfer - Azure AD integration tile in hello Access Panel, you should get automatically signed-on tooyour MOVEit Transfer - Azure AD integration application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6e19e-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6e19e-244">Additional resources</span></span>

* [<span data-ttu-id="6e19e-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6e19e-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6e19e-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6e19e-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

