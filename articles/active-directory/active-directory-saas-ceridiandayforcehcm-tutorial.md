---
title: 'Kurz: Azure Active Directory integrace s Ceridian Dayforce HCM | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Ceridian Dayforce HCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 4d72f29b4e5e30ef8881806d789f6676fc541e2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a><span data-ttu-id="ba85c-103">Kurz: Azure Active Directory integrace s Ceridian Dayforce HCM</span><span class="sxs-lookup"><span data-stu-id="ba85c-103">Tutorial: Azure Active Directory integration with Ceridian Dayforce HCM</span></span>

<span data-ttu-id="ba85c-104">V tomto kurzu zjistíte, jak toointegrate Ceridian Dayforce HCM službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ba85c-104">In this tutorial, you learn how toointegrate Ceridian Dayforce HCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ba85c-105">Integrace Ceridian Dayforce HCM s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ba85c-105">Integrating Ceridian Dayforce HCM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ba85c-106">Můžete ovládat ve službě Azure AD, který má přístup tooCeridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="ba85c-106">You can control in Azure AD who has access tooCeridian Dayforce HCM.</span></span>
- <span data-ttu-id="ba85c-107">Vaši uživatelé tooautomatically get přihlášeného tooCeridian Dayforce HCM (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ba85c-107">You can enable your users tooautomatically get signed-on tooCeridian Dayforce HCM (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="ba85c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba85c-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="ba85c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ba85c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba85c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ba85c-110">Prerequisites</span></span>

<span data-ttu-id="ba85c-111">Integrace služby Azure AD s Ceridian Dayforce HCM tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ba85c-111">tooconfigure Azure AD integration with Ceridian Dayforce HCM, you need hello following items:</span></span>

- <span data-ttu-id="ba85c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ba85c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ba85c-113">Ceridian Dayforce HCM jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ba85c-113">A Ceridian Dayforce HCM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ba85c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba85c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ba85c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ba85c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ba85c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ba85c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ba85c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ba85c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ba85c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ba85c-118">Scenario description</span></span>
<span data-ttu-id="ba85c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba85c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ba85c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ba85c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ba85c-121">Přidání Ceridian Dayforce HCM z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ba85c-121">Adding Ceridian Dayforce HCM from hello gallery</span></span>
2. <span data-ttu-id="ba85c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ba85c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ceridian-dayforce-hcm-from-hello-gallery"></a><span data-ttu-id="ba85c-123">Přidání Ceridian Dayforce HCM z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ba85c-123">Adding Ceridian Dayforce HCM from hello gallery</span></span>
<span data-ttu-id="ba85c-124">tooconfigure hello integrace Ceridian Dayforce HCM do Azure AD, je nutné tooadd Ceridian Dayforce HCM hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ba85c-124">tooconfigure hello integration of Ceridian Dayforce HCM into Azure AD, you need tooadd Ceridian Dayforce HCM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ba85c-125">**tooadd Ceridian Dayforce HCM z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ba85c-125">**tooadd Ceridian Dayforce HCM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ba85c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ba85c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="ba85c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ba85c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="ba85c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ba85c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="ba85c-133">Hello vyhledávacího pole zadejte **Ceridian Dayforce HCM**, vyberte **Ceridian Dayforce HCM** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba85c-133">In hello search box, type **Ceridian Dayforce HCM**, select **Ceridian Dayforce HCM** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Ceridian Dayforce HCM v seznamu výsledků hello](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="ba85c-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ba85c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="ba85c-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Ceridian Dayforce HCM podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ba85c-136">In this section, you configure and test Azure AD single sign-on with Ceridian Dayforce HCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ba85c-137">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Ceridian Dayforce HCM je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ba85c-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Ceridian Dayforce HCM is tooa user in Azure AD.</span></span> <span data-ttu-id="ba85c-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Ceridian Dayforce HCM musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ba85c-138">In other words, a link relationship between an Azure AD user and hello related user in Ceridian Dayforce HCM needs toobe established.</span></span>

<span data-ttu-id="ba85c-139">V Ceridian Dayforce HCM, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="ba85c-139">In Ceridian Dayforce HCM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ba85c-140">tooconfigure a testu Azure AD jednotné přihlašování s Ceridian Dayforce HCM, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="ba85c-140">tooconfigure and test Azure AD single sign-on with Ceridian Dayforce HCM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ba85c-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ba85c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ba85c-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ba85c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ba85c-143">**[Vytvoření zkušebního uživatele Ceridian Dayforce HCM](#create-a-ceridian-dayforce-hcm-test-user)**  -toohave protějšek Britta Simon v HCM Dayforce Ceridian, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="ba85c-143">**[Create a Ceridian Dayforce HCM test user](#create-a-ceridian-dayforce-hcm-test-user)** - toohave a counterpart of Britta Simon in Ceridian Dayforce HCM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ba85c-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ba85c-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ba85c-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ba85c-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="ba85c-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ba85c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="ba85c-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="ba85c-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Ceridian Dayforce HCM application.</span></span>

<span data-ttu-id="ba85c-148">**tooconfigure Azure AD jednotné přihlašování s Ceridian Dayforce HCM, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ba85c-148">**tooconfigure Azure AD single sign-on with Ceridian Dayforce HCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="ba85c-149">V portálu Azure, na hello hello **Ceridian Dayforce HCM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-149">In hello Azure portal, on hello **Ceridian Dayforce HCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="ba85c-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ba85c-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

3. <span data-ttu-id="ba85c-153">Na hello **Ceridian Dayforce HCM domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ba85c-153">On hello **Ceridian Dayforce HCM Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    <span data-ttu-id="ba85c-155">a.</span><span class="sxs-lookup"><span data-stu-id="ba85c-155">a.</span></span> <span data-ttu-id="ba85c-156">V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše tooyour toosign na uživatele Ceridian Dayforce HCM aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba85c-156">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour Ceridian Dayforce HCM application.</span></span>
    
    | <span data-ttu-id="ba85c-157">Prostředí</span><span class="sxs-lookup"><span data-stu-id="ba85c-157">Environment</span></span> | <span data-ttu-id="ba85c-158">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="ba85c-158">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="ba85c-159">Pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="ba85c-159">For production</span></span> | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | <span data-ttu-id="ba85c-160">Pro test</span><span class="sxs-lookup"><span data-stu-id="ba85c-160">For test</span></span> | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    <span data-ttu-id="ba85c-161">b.</span><span class="sxs-lookup"><span data-stu-id="ba85c-161">b.</span></span> <span data-ttu-id="ba85c-162">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="ba85c-162">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    
    | <span data-ttu-id="ba85c-163">Prostředí</span><span class="sxs-lookup"><span data-stu-id="ba85c-163">Environment</span></span> | <span data-ttu-id="ba85c-164">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="ba85c-164">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="ba85c-165">Pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="ba85c-165">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp` |
    | <span data-ttu-id="ba85c-166">Pro test</span><span class="sxs-lookup"><span data-stu-id="ba85c-166">For test</span></span> | `https://fs-test.dayforcehcm.com/sp` |
    
    <span data-ttu-id="ba85c-167">c.</span><span class="sxs-lookup"><span data-stu-id="ba85c-167">c.</span></span> <span data-ttu-id="ba85c-168">V hello **adresa URL odpovědi** textovému poli, adresa URL typu hello používá Azure AD toopost hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba85c-168">In hello **Reply URL** textbox, type hello URL used by Azure AD toopost hello response.</span></span>
    
    | <span data-ttu-id="ba85c-169">Prostředí</span><span class="sxs-lookup"><span data-stu-id="ba85c-169">Environment</span></span> | <span data-ttu-id="ba85c-170">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="ba85c-170">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="ba85c-171">Pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="ba85c-171">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | <span data-ttu-id="ba85c-172">Pro test</span><span class="sxs-lookup"><span data-stu-id="ba85c-172">For test</span></span> | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > <span data-ttu-id="ba85c-173">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ba85c-173">These values are not real.</span></span> <span data-ttu-id="ba85c-174">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="ba85c-174">Update these values with hello actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="ba85c-175">Obraťte se na [tým podpory Ceridian Dayforce HCM klienta](https://www.ceridian.com/contact-us/index.html) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ba85c-175">Contact [Ceridian Dayforce HCM Client support team](https://www.ceridian.com/contact-us/index.html) tooget these values.</span></span>

4. <span data-ttu-id="ba85c-176">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ba85c-176">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

5. <span data-ttu-id="ba85c-178">Aplikace Ceridian Dayforce HCM očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="ba85c-178">Your Ceridian Dayforce HCM application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="ba85c-179">Práce s [tým podpory Ceridian Dayforce HCM](https://www.ceridian.com/contact-us/index.html) první tooidentify hello správné uživatelské identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ba85c-179">Work with [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) first tooidentify hello correct user identifier.</span></span> <span data-ttu-id="ba85c-180">Společnost Microsoft doporučuje používat hello **"název"** atribut jako identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="ba85c-180">Microsoft recommends using hello **"name"** attribute as user identifier.</span></span> <span data-ttu-id="ba85c-181">Můžete spravovat hello hodnoty těchto atributů z hello **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba85c-181">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="ba85c-182">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="ba85c-182">hello following screenshot shows an example for this.</span></span>  

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

6. <span data-ttu-id="ba85c-184">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ba85c-184">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="ba85c-185">Název atributu</span><span class="sxs-lookup"><span data-stu-id="ba85c-185">Attribute Name</span></span>  | <span data-ttu-id="ba85c-186">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="ba85c-186">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="ba85c-187">jméno</span><span class="sxs-lookup"><span data-stu-id="ba85c-187">name</span></span>  | <span data-ttu-id="ba85c-188">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="ba85c-188">user.extensionattribute2</span></span> |    

    <span data-ttu-id="ba85c-189">a.</span><span class="sxs-lookup"><span data-stu-id="ba85c-189">a.</span></span> <span data-ttu-id="ba85c-190">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ba85c-190">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="ba85c-193">b.</span><span class="sxs-lookup"><span data-stu-id="ba85c-193">b.</span></span> <span data-ttu-id="ba85c-194">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="ba85c-194">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="ba85c-195">c.</span><span class="sxs-lookup"><span data-stu-id="ba85c-195">c.</span></span> <span data-ttu-id="ba85c-196">V hello **hodnotu** seznamu, vyberte hello atribut uživatele chcete toouse týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="ba85c-196">In hello **Value** list, select hello user attribute you want toouse for your implementation.</span></span>
    <span data-ttu-id="ba85c-197">Pokud chcete, aby toouse hello EmployeeID jako jedinečný identifikátor uživatele a hodnota atributu hello jsou uloženy v hello ExtensionAttribute2, pak vyberte například **user.extensionattribute2**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-197">For example, if you want toouse hello EmployeeID as unique user identifier and you have stored hello attribute value in hello ExtensionAttribute2, then select **user.extensionattribute2**.</span></span>
    
    <span data-ttu-id="ba85c-198">d.</span><span class="sxs-lookup"><span data-stu-id="ba85c-198">d.</span></span> <span data-ttu-id="ba85c-199">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-199">Click **Ok**.</span></span>

7. <span data-ttu-id="ba85c-200">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ba85c-200">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="ba85c-202">Na hello **Ceridian Dayforce HCM konfigurace** klikněte na tlačítko **konfigurace HCM Dayforce Ceridian** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ba85c-202">On hello **Ceridian Dayforce HCM Configuration** section, click **Configure Ceridian Dayforce HCM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ba85c-203">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ba85c-203">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace HCM Ceridian Dayforce](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

9. <span data-ttu-id="ba85c-205">tooconfigure jednotného přihlašování na **Ceridian Dayforce HCM** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** a **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[tým podpory Ceridian Dayforce HCM](https://www.ceridian.com/contact-us/index.html).</span><span class="sxs-lookup"><span data-stu-id="ba85c-205">tooconfigure single sign-on on **Ceridian Dayforce HCM** side, you need toosend hello downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html).</span></span>

> [!TIP]
> <span data-ttu-id="ba85c-206">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ba85c-206">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ba85c-207">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="ba85c-207">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ba85c-208">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ba85c-208">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ba85c-209">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ba85c-209">Create an Azure AD test user</span></span>

<span data-ttu-id="ba85c-210">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="ba85c-210">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="ba85c-212">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ba85c-212">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ba85c-213">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ba85c-213">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="ba85c-215">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-215">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="ba85c-217">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ba85c-217">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="ba85c-219">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ba85c-219">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    <span data-ttu-id="ba85c-221">a.</span><span class="sxs-lookup"><span data-stu-id="ba85c-221">a.</span></span> <span data-ttu-id="ba85c-222">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-222">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ba85c-223">b.</span><span class="sxs-lookup"><span data-stu-id="ba85c-223">b.</span></span> <span data-ttu-id="ba85c-224">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ba85c-224">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="ba85c-225">c.</span><span class="sxs-lookup"><span data-stu-id="ba85c-225">c.</span></span> <span data-ttu-id="ba85c-226">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="ba85c-226">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="ba85c-227">d.</span><span class="sxs-lookup"><span data-stu-id="ba85c-227">d.</span></span> <span data-ttu-id="ba85c-228">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-228">Click **Create**.</span></span>
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a><span data-ttu-id="ba85c-229">Vytvoření zkušebního uživatele Ceridian Dayforce HCM</span><span class="sxs-lookup"><span data-stu-id="ba85c-229">Create a Ceridian Dayforce HCM test user</span></span>

<span data-ttu-id="ba85c-230">Hello cílem této části je toocreate volal Britta Simon v Ceridian Dayforce HCM uživatele.</span><span class="sxs-lookup"><span data-stu-id="ba85c-230">hello objective of this section is toocreate a user called Britta Simon in Ceridian Dayforce HCM.</span></span> <span data-ttu-id="ba85c-231">Práce s hello [tým podpory Ceridian Dayforce HCM](https://www.ceridian.com/contact-us/index.html) tooget uživatelů přidaných v hello Ceridian Dayforce HCM aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba85c-231">Work with hello [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) tooget users added in hello Ceridian Dayforce HCM application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ba85c-232">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ba85c-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ba85c-233">V této části povolíte tak, že udělíte přístup tooCeridian Dayforce HCM Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ba85c-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCeridian Dayforce HCM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ba85c-235">**tooassign Britta Simon tooCeridian Dayforce HCM proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ba85c-235">**tooassign Britta Simon tooCeridian Dayforce HCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="ba85c-236">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ba85c-238">V seznamu aplikace hello vyberte **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-238">In hello applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

3. <span data-ttu-id="ba85c-240">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ba85c-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ba85c-242">Click **Add** button.</span></span> <span data-ttu-id="ba85c-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ba85c-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ba85c-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ba85c-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ba85c-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ba85c-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ba85c-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ba85c-247">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="ba85c-248">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ba85c-248">Assign hello Azure AD test user</span></span>

<span data-ttu-id="ba85c-249">V této části povolíte tak, že udělíte přístup tooCeridian Dayforce HCM Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ba85c-249">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCeridian Dayforce HCM.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="ba85c-251">**tooassign Britta Simon tooCeridian Dayforce HCM proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ba85c-251">**tooassign Britta Simon tooCeridian Dayforce HCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="ba85c-252">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-252">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ba85c-254">V seznamu aplikace hello vyberte **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-254">In hello applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Hello Ceridian Dayforce HCM odkaz v seznamu aplikace hello](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

3. <span data-ttu-id="ba85c-256">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ba85c-256">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="ba85c-258">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ba85c-258">Click **Add** button.</span></span> <span data-ttu-id="ba85c-259">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ba85c-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="ba85c-261">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ba85c-261">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ba85c-262">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ba85c-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ba85c-263">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ba85c-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="ba85c-264">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ba85c-264">Test single sign-on</span></span>

<span data-ttu-id="ba85c-265">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ba85c-265">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="ba85c-266">Když kliknete na dlaždici Ceridian Dayforce HCM hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Ceridian Dayforce HCM aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba85c-266">When you click hello Ceridian Dayforce HCM tile in hello Access Panel, you should get automatically signed-on tooyour Ceridian Dayforce HCM application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ba85c-267">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ba85c-267">Additional resources</span></span>

* [<span data-ttu-id="ba85c-268">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ba85c-268">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ba85c-269">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ba85c-269">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png

